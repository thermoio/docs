---
title: Installing ModSec for nginx on Ubuntu
subject: ModSecurity
---

# Installing ModSec for nginx on Ubuntu

## But first...
You need root access and the most current version of Ubuntu.

## 1: Update
See [Updating Ubuntu](https://www.thermo.io/how-to/security/updating-ubuntu).

## 2: Install dependencies
Install the following packages:
```shell
apt-get install -y git build-essential libpcre3 libpcre3-dev libssl-dev libtool autoconf apache2-dev libxml2-dev libcurl4-openssl-dev automake pkgconf
```

## 3: Compile ModSec
ModSec for the Nginx master branch has been reported as currently being unstable; therefore, use the `nginx_refactoring` branch as directed below:
1. Download the `nghinx_refactoring` branch of ModSecurity for Nginx:
   ```shell
   cd /usr/src
   git clone -b nginx_refactoring https://github.com/SpiderLabs/ModSecurity.git
   ```
2. Compile ModSec:
**Attention:** The two `sed` commands below prevent warning messages when using newer automake versions.
   ```shell
   cd ModSecurity
   ./autogen.sh
   ./configure --enable-standalone-module --disable-mlogc
   make
   ```

## 4: Compile Nginx
1. Download and unarchive the latest stable release of Nginx. At the time of this writing, this is Nginx 1.13.4:
   ```shell
   cd /usr/src
   wget https://nginx.org/download/nginx-1.13.4.tar.gz
   tar -zxvf nginx-1.13.4.tar.gz && rm -f nginx-1.13.4.tar.gz
   ```
2. Using the existing user `www-data` and group `www-data`, compile Nginx and enable ModSecurity and SSL modules:
   ```shell
   cd nginx-1.13.4/
   ./configure --user=www-data --group=www-data --add-module=/usr/src/ModSecurity/nginx/modsecurity --with-http_ssl_module
   make
   make install
   ```
3. Modify the default Nginx user:
   ```shell
   sed -i "s/#user  nobody;/user www-data www-data;/" /usr/local/nginx/conf/nginx.conf
   ```
4. After installation, you can find the files at:
   ```shell
   nginx path prefix: "/usr/local/nginx"
   nginx binary file: "/usr/local/nginx/sbin/nginx"
   nginx modules path: "/usr/local/nginx/modules"
   nginx configuration prefix: "/usr/local/nginx/conf"
   nginx configuration file: "/usr/local/nginx/conf/nginx.conf"
   nginx pid file: "/usr/local/nginx/logs/nginx.pid"
   nginx error log file: "/usr/local/nginx/logs/error.log"
   nginx http access log file: "/usr/local/nginx/logs/access.log"
   nginx http client request body temporary files: "client_body_temp"
   nginx http proxy temporary files: "proxy_temp"
   nginx http fastcgi temporary files: "fastcgi_temp"
   nginx http uwsgi temporary files: "uwsgi_temp"
   nginx http scgi temporary files: "scgi_temp"
   ```
5. Test the installation:
   ```shell
   /usr/local/nginx/sbin/nginx -t
   ```
6. If desired, you may also set up a systemd unit file for Nginx:
   ```shell
   cat <<EOF>> /lib/systemd/system/nginx.service
   [Service]
   Type=forking
   ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
   ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
   ExecReload=/usr/local/nginx/sbin/nginx -s reload
   KillStop=/usr/local/nginx/sbin/nginx -s stop
   KillMode=process
   Restart=on-failure
   RestartSec=42s
   PrivateTmp=true
   LimitNOFILE=200000
   [Install]
   WantedBy=multi-user.target
   EOF
   ```
7. Finally, you may stop, start, or restart Nginx with the following commands:
   ```shell
   systemctl stop nginx.service
   systemctl start nginx.service
   systemctl restart nginx.service
   ```

## 5: Configure ModSec and Nginx
1. Configure Nginx:
**Attention:** The sample Nginx configuration below uses Nginx as a web server rather than a reverse proxy. If you are using Nginx as a reverse proxy, remove the # character in last two lines, then make the appropriate modifications.
   a. Issue:
      ```shell
      vi /usr/local/nginx/conf/nginx.conf
      ```
   b. Find the following segment within the `http {}` segment:
      ```shell
              location / {
               root   html;
               index  index.html index.htm;
      ```
   Add the lines below so the final result is:
      ```shell
      location / {
       ModSecurityEnabled on;
       ModSecurityConfig modsec_includes.conf;
       root   html;
       index  index.html index.htm;
       }
      ```
   c. Save and quit:
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
   sed -i "s|SecAuditLog /var/log/modsec_audit.log|SecAuditLog /usr/local/nginx/logs/modsec_audit.log|" /usr/local/nginx/conf/modsecurity.conf
   ```
5. Allow Nginx to create Modsec logs:
   ```shell
   chown www-data.root /usr/local/nginx/logs
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

## 6: Test ModSec
1. Start Nginx:
   ```shell
   systemctl start nginx.service
   ```
2. Open port 80 to permit outside access:
   ```shell
   ufw allow OpenSSH
   ufw allow 80
   ufw default deny
   ufw enable
   ```
3. Point your web browser to `http://<YourServersIP>/?param="><script>alert(1);</script>`
(Be sure to change `<YourServersIP>` to the actual IP of your server)
4. Use `grep` to fetch error messages:
   ```shell
   grep error /usr/local/nginx/logs/error.log
   ```
The output should include error messages resembling the following:
   ```shell
   2017/02/15 14:07:54 [error] 10776#0: [client 104.20.23.240] ModSecurity: Warning. detected XSS using libinjection. [file "/usr/local/nginx/conf/owasp-modsecurity-crs/rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf"] [line "56"] [id "941100"] [rev "2"] [msg "XSS Attack Detected via libinjection"] [data "Matched Data:  found within ARGS:param: \x22><script>alert(1);</script>"] [severity "CRITICAL"] [ver "OWASP_CRS/3.0.0"] [maturity "1"] [accuracy "9"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "attack-xss"] [tag "OWASP_CRS/WEB_ATTACK/XSS"] [tag "WASCTC/WASC-8"] [tag "WASCTC/WASC-22"] [tag "OWASP_TOP_10/A3"] [tag "OWASP_AppSensor/IE1"] [tag "CAPEC-242"] [hostname ""] [uri "/index.html"] [unique_id "ATAcAcAkucAchGAcPLAcAcAY"]
   ```
5. The procedure is complete. To customize your settings, review and edit the following files:
* `/usr/local/nginx/conf/modsecurity.conf`
* `/usr/local/nginx/conf/owasp-modsecurity-crs/crs-setup.conf`

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
