---
title: Methods of Reindexing
subject: Magento
---

# Methods of Reindexing Magento

## Overview
Magento uses a process called reindexing to send its back-end updates to its front-end interface. This is important for keeping business information like customer accounts and product catalog up-to-date. For example, on a given day, product prices might change by customer type. Performing reindexing on an indexer, or table, that stores pricing information by product and consumer type clears the cache of the current table, inserts updated values, and makes those changes visible in the storefront application.

“Update on Save” is the default reindexing mode in Magento that updates table caches and values in real time every time a site admin makes a change. Although this can work for small sites, it is impractical for large sites and databases because the reindexer will take hours to finish and degrade site performance.

The solution is to replace the default reindexing mode with a scheduled cron job that reindexes at regular intervals.

## What you need
* Administrator access to your Magento store
* SSH credentials

## Checking current indexer settings
If you are a site administrator, you may run Magento through their preferred shell, though syntax may vary.

To run Magento commands from any directory, add its root directory to the directory path:

**Attention:** Your actual path will vary according to the location of your `magento2/bin` file.
```shell
export PATH=$PATH:/var/www/html/magento2/bin
```
From here, you have several ways to check the existing status of indexers:
* For a list of indexers:
```shell
magento indexer:info <indexer>
```
* For the updating status of indexers:
```shell
magento indexer:status <indexer>
```
* For the updating mode of indexers:
```shell
magento indexer:show-mode <indexer>
```
In the above scripts, replace `<indexer>` with the name of indexers separated by spaces, or omit it to include all indexers. For example:

#### Example 1
```shell
magento indexer:info
```
produces:
```shell
category_product Category Products
product_price Product Prices
```
#### Example 2
```shell
magento indexer:status category_product product_price
```
results in:
```shell
Category Products: Reindex required
Product Prices: Reindex required
```
#### Example 3
```shell
magento indexer:show-mode category_product product_price
```
results in:
```shell
Category Products: Update on Save
Product Prices: Update on Save
```
This last example shows that two indexers, category products and product prices, are set to Update on Save.

## Reindexing manually
You can specify a manual reindex with:
```shell
magento indexer:reindex category_product product_price
```
resulting in:
```shell
Category Products index has been rebuilt successfully in 00:00:02
Product Prices index has been rebuilt successfully in 00:00:02
```
The reindexing process can also occur through a scheduled cron job.

## Changing to "Update by Schedule"
For larger sites, manually reindexing is slow and inefficient. For this reason, it is often more practical to schedule cron jobs to update the indexes at regular intervals.

**Attention:** Because reindexing drastically reduces site performance, it is usually best to perform reindexing during off-hours.

To change an indexer's reindexing mode to manual, set the value to “Update by Schedule:”
```shell
magento indexer:set-mode schedule category_product product_price
```
resulting in:
```shell
Index mode for Indexer Category Products was changed from 'Update on Save' to 'Update by Schedule'
Index mode for Indexer Product Prices was changed from 'Update on Save' to 'Update by Schedule'
```
The following PHP command allows you to create a reindexing cron job, where each asterisk (`*`) represents a value:
```shell
* * * * * php -f /shell/indexer.php reindexall
```
As shown below, each asterisk (`*`) represents a different time value. The first asterisk corresponds to <minute:0-59>, the second to <hour:0-23>, and so on.
```shell
<minute:0-59> <hour:0-23> <day_of_the_month:1-31> <month_of_the_year:1-12> <1-7: 1 for Monday>
```
To set a schedule,replace the asterisk (`*`) with values in cron format. For example, to perform reindexing everyday at 4am, issue:
```shell
0 4 * * * php -f /shell/indexer.php reindexall
```
where `0` represents the minute, and the `4` represents the hour. The day, month, and week day were ignored because the user wants to run the cron job every day.

### Scheduling at certain time intervals
You may also run the job at certain intervals using `*/x`, where `x` is the interval.
#### Example 1
Create a cron job that reindexes every 4 hours, regardless of day or date:
```shell
0 */4 * * * php -f /shell/indexer.php reindexall
```
#### Example 2
Create a cron job that reindexes every other hours, regardless of day or date:
```shell
0 0-23/2 * * * php -f /shell/indexer.php reindexall
```
#### Example 3
Create a cron job that reindexes every 5 minutes, but only between the hours of 12 a.m. - 6 a.m. and 6 p.m. - 11 p.m.:
```shell
*/5 0-6,18-23 * * * php -f /shell/indexer.php reindexall
```
#### Example 4
Specify different intervals for each index, such as reindex category products everyday at 6 a.m., and product prices at 8 a.m.:
```shell
0 6 * * * php -f /shell/indexer.php --reindex category_product
0 8 * * * php -f /shell/indexer.php --reindex product_price
```

## Reverting to default "Update on Save"
If it becomes necessary to revert to the default “Update on Save” setting, issue:
```shell
magento indexer:set-mode realtime category_product product_price
```
resulting in:
```shell
Index mode for Indexer Category Products was changed from 'Update by Schedule' to 'Update on Save'
Index mode for Indexer Product Prices was changed from 'Update by Schedule' to 'Update on Save'
```
## Troubleshooting
When performing large scale re-indexing, you may encounter out-of-memory or maximum-execution-time-exceeded errors like the following:
```shell
PHP Fatal error: Allowed memory size of 262144 bytes exhausted (tried to allocate x bytes) in …/app/code/core/Mage/Index/Model/Indexer.php on line x
```
or
```shell
PHP Fatal error: Maximum execution time of 60 seconds exceeded in /var/www/domain.com/lib/Zend/Db/Statement.php on line x
```
If these errors occur, try increasing the values for `memory_limit` and `max_execution_time` in the `.htaccess` file located in Magento's root installation directory. As a note, Magento recommends allocating 768MB memory for normal operations and 2GB for testing.
```shell
############################################
## adjust memory limit

php_value memory_limit 768M
php_value max_execution_time 18000

############################################
```

**_For 24-hour assistance any day of the year, contact a Thermo Physicist [through the Client Portal](https://core.thermo.io/login/)._**
