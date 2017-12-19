---
title: Installing ModSecurity with Nginx on CentOS7
subject: ModSecurity
---

# Installing ModSecurity with Nginx on CentOS7

## But first...
You need sudo access and the most current version of CentOS 7. See [Creating sudo users on CentOS](https://github.com/thermoio/docs/blob/master/getting-started/creating-sudo-users-on-centos) for more information.

## 1: Update
See [Updating CentOS](https://www.thermo.io/how-to/security/updating-centos).

## 2: Install dependencies
Install the following packages:
```shell
yum groupinstall -y "Development Tools"
yum install -y httpd httpd-devel pcre pcre-devel libxml2 libxml2-devel curl curl-devel openssl openssl-devel
shutdown -r now
```

## 3: Compile ModSec
ModSec for the Nginx master branch has been reported as currently being unstable; therefore, use the `nginx_refactoring` branch as directed below:
1. Download the `nginx_refactoring` branch of ModSecurity for Nginx:
   ```shell
   cd /usr/src
   git clone -b nginx_refactoring https://github.com/SpiderLabs/ModSecurity.git
   ```
2. Compile ModSec:
**Attention:** The two `sed` commands below prevent warning messages when using newer automake versions.
   ```shell
   cd ModSecurity
   sed -i '/AC_PROG_CC/a\AM_PROG_CC_C_O' configure.ac
   sed -i '1 i\AUTOMAKE_OPTIONS = subdir-objects' Makefile.am
   ./autogen.sh
   ./configure --enable-standalone-module --disable-mlogc
   make
   ```

## 4: Compile Nginx
1. Download and unarchive the latest stable release of Nginx. Currently, this is Nginx 1.13.4:
   ```shell
   cd /usr/src
   wget https://nginx.org/download/nginx-1.13.4.tar.gz
   tar -zxvf nginx-1.13.4.tar.gz && rm -f nginx-1.13.4.tar.gz
   ```
2. Create a dedicated nginx user and group for Nginx:
   ```shell
   groupadd -r nginx
   useradd -r -g nginx -s /sbin/nologin -M nginx
   ```
3. Compile Nginx and enable ModSecurity and SSL modules:
   ```shell
   cd nginx-1.10.3/
   ./configure --user=nginx --group=nginx --add-module=/usr/src/ModSecurity/nginx/modsecurity --with-http_ssl_module
   make
   make install
   ```
4. Modify the default Nginx user:
   ```shell
   sed -i "s/#user  nobody;/user nginx nginx;/" /usr/local/nginx/conf/nginx.conf
   ```

## 5: Configure ModSec and Nginx
1. Configure Nginx:

   a. Issue:
      ```shell
      vi /usr/local/nginx/conf/nginx.conf
      ```
   b. Find the following segment within the `http {}` segment:
      ```shell
           location / {
               root   html;
               index  index.html index.htm;
           }
      ```
   Add the lines below so the final result should be:
      ```shell
      location / {
       ModSecurityEnabled on;
       ModSecurityConfig modsec_includes.conf;
       root   html;
       index  index.html index.htm;
       }
      ```
   c. You also need to change the location of the default PID file to match the systemd script you will make in the following steps. Find the line `#pid    logs/nginx.pid` and change it to the following by removing the # and changing the path:
   
      ```shell
      pid  /var/run/nginx.pid
      ```
   
   d. Save and quit:
      ```shell
      :wq!
      ```
2. Create the file `/usr/local/nginx/conf/modsec_includes.conf`:
**Attention:** The config below applies all of the OWASP ModSecurity Core Rules in the `owasp-modsecurity-crs/rules/` directory. If you want to apply selective rules only, you should remove the include `owasp-modsecurity-crs/rules/*.conf` line, and then specify exact rules you need after step 5 of this section.
   ```shell
   cat <<EOF>> /usr/local/nginx/conf/modsec_includes.conf
   include modsecurity.conf
   include owasp-modsecurity-crs/crs-setup.conf
   include owasp-modsecurity-crs/rules/*.conf
   EOF
   ```
3. Import the ModSec config files:
   ```shell
   cp /usr/src/ModSecurity/modsecurity.conf-recommended /usr/local/nginx/conf/modsecurity.conf
   cp /usr/src/ModSecurity/unicode.mapping /usr/local/nginx/conf/
   ```
4. Modify the file ``/usr/local/nginx/conf/modsecurity.conf``:
   ```shell
   sed -i "s/SecRuleEngine DetectionOnly/SecRuleEngine On/" /usr/local/nginx/conf/modsecurity.conf
   sed -i "s/SecAuditLogType Serial/SecAuditLogType Concurrent/" /usr/local/nginx/conf/modsecurity.conf
   sed -i "s|SecAuditLog /var/log/modsec_audit.log|SecAuditLog /usr/local/nginx/logs/modsec_audit.log|"        /usr/local/nginx/conf/modsecurity.conf
   ```
5. Allow Nginx to create Modsec logs in the Nginx log directory:
   ```shell
   chown nginx.root /usr/local/nginx/logs
   ```
6. Add OWASP ModSecurity Core Rule Set (CRS) files:
   ```shell
   cd /usr/local/nginx/conf
   git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
   cd owasp-modsecurity-crs
   mv crs-setup.conf.example crs-setup.conf
   cd rules
   mv REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   mv RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf.example RESPONSE-999-EXCLUSION-RULES-AFTER-CRS.conf
   ```

## 6: Create Systemd Script

1. Create the file /lib/systemd/system/nginx.service and add the script:
   ```shell
   cat <<EOF>> /lib/systemd/system/nginx.service
   [Unit]
   Description=The NGINX HTTP and reverse proxy server
   After=syslog.target network.target remote-fs.target nss-lookup.target
   [Service]
   Type=forking
   PIDFile=/var/run/nginx.pid
   ExecStartPre=/usr/local/nginx/sbin/nginx -t
   ExecStart=/usr/local/nginx/sbin/nginx
   ExecReload=/bin/kill -s HUP $MAINPID
   ExecStop=/bin/kill -s QUIT $MAINPID
   PrivateTmp=true
   [Install]
   WantedBy=multi-user.target
   EOF
   ```
2. Reload systemd services:
   ```shell
   systemctl daemon-reload
   ```

## 7: Test ModSec
1. Start Nginx:
   ```shell
   systemctl start nginx.service
   ```
2. Point your web browser to `http://<YourServersIP>/?param="><script>alert(1);</script>`
   (Be sure to replace <YourServersIP> with the IP address of your server)
3. Use `grep` to fetch error messages:
   ```shell
   grep error /usr/local/nginx/logs/error.log
   ```
The output should include error messages resembling the following:
   ```shell
   2017/02/15 14:07:54 [error] 10776#0: [client 104.20.23.240] ModSecurity: Warning. detected XSS using libinjection. [file "/usr/local/nginx/conf/owasp-modsecurity-crs/rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf"] [line "56"] [id "941100"] [rev "2"] [msg "XSS Attack Detected via libinjection"] [data "Matched Data:  found within ARGS:param: \x22><script>alert(1);</script>"] [severity "CRITICAL"] [ver "OWASP_CRS/3.0.0"] [maturity "1"] [accuracy "9"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "attack-xss"] [tag "OWASP_CRS/WEB_ATTACK/XSS"] [tag "WASCTC/WASC-8"] [tag "WASCTC/WASC-22"] [tag "OWASP_TOP_10/A3"] [tag "OWASP_AppSensor/IE1"] [tag "CAPEC-242"] [hostname ""] [uri "/index.html"] [unique_id "ATAcAcAkucAchGAcPLAcAcAY"]
   ```
4. The procedure is complete. To customize your settings, review and edit the following files:
* `/usr/local/nginx/conf/modsecurity.conf`
* `/usr/local/nginx/conf/owasp-modsecurity-crs/crs-setup.conf`

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
