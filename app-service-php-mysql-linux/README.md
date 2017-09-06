# app-service-php-mysql-linux

## Click to Deploy

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FYaleUniversity%2Fazure-templates%2Fmaster%2Fapp-service-php-mysql-linux%2Fazuredeploy.json" target="_blank">
  <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

<br>

<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2FYaleUniversity%2Fazure-templates%2Fmaster%2Fapp-service-php-mysql-linux%2Fazuredeploy.json" target="_blank">
  <img src="http://armviz.io/visualizebutton.png"/>
</a>

## Deployment Via Azure CLI

```bash
$ az login
To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code [CODE] to authenticate.
  {
    "cloudName": "AzureCloud",
    "id": "[subscription guid]",
    "isDefault": false,
    "name": "[subscription name]",
    "state": "Enabled",
    "tenantId": "[tenant guid]",
    "user": {
      "name": "[az login name]",
      "type": "user"
    }
  },
  .
  .
  .
$
```

From the list, find the guid for the subscription that you wish to place the application resource group into. Switch to that subscription.

```bash
$ az account set --subscription [subscription guid]
$
```

Create a new Azure Resource Group to manage the application, its app service plan, and MySQL database in the *East US 2* location.

```bash
$ az group create --name '[netid]-[resource group name]-rg' --location 'eastus2'
{
  "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg",
  "location": "eastus2",
  "managedBy": null,
  "name": "[resource group name]",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
$
```

Deploy a new PHP App Service application, App Service Plan, and MySQL database from the template.

```bash
$ az group deployment create --name [name of deployment] --resource-group [netid]-[resource group name]-rg --template-file azuredeploy.json
{
  "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Resources/deployments/veb-yiitstapp-deployment",
  "name": "veb-yiitstapp-deployment",
  "properties": {
    "correlationId": "aea34f62-6ced-4fac-8a7a-2e40241aaed9",
    "debugSetting": null,
    "dependencies": [
      {
        "dependsOn": [
          {
            "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Web/serverfarms/[hash from resource group id]hostingplan",
            "resourceGroup": "[netid]-[resource group name]-rg",
            "resourceName": "[hash from resource group id]hostingplan",
            "resourceType": "Microsoft.Web/serverfarms"
          }
        ],
        "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Web/sites/[hash from resource group id]website",
        "resourceGroup": "[netid]-[resource group name]-rg",
        "resourceName": "[hash from resource group id]website",
        "resourceType": "Microsoft.Web/sites"
      },
      {
        "dependsOn": [
          {
            "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Web/sites/[hash from resource group id]website",
            "resourceGroup": "[netid]-[resource group name]-rg",
            "resourceName": "[hash from resource group id]website",
            "resourceType": "Microsoft.Web/sites"
          },
          {
            "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/SuccessBricks.ClearDB/databases/[hash from resource group id]db",
            "resourceGroup": "[netid]-[resource group name]-rg",
            "resourceName": "[hash from resource group id]db",
            "resourceType": "SuccessBricks.ClearDB/databases"
          }
        ],
        "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Web/sites/[hash from resource group id]website/config/connectionstrings",
        "resourceGroup": "[netid]-[resource group name]-rg",
        "resourceName": "[hash from resource group id]website/connectionstrings",
        "resourceType": "Microsoft.Web/sites/config"
      },
      {
        "dependsOn": [
          {
            "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Web/sites/[hash from resource group id]website",
            "resourceGroup": "[netid]-[resource group name]-rg",
            "resourceName": "[hash from resource group id]website",
            "resourceType": "Microsoft.Web/sites"
          }
        ],
        "id": "/subscriptions/[subscription guid]/resourceGroups/[netid]-[resource group name]-rg/providers/Microsoft.Web/sites/[hash from resource group id]website/config/web",
        "resourceGroup": "[netid]-[resource group name]-rg",
        "resourceName": "[hash from resource group id]website/web",
        "resourceType": "Microsoft.Web/sites/config"
      }
    ],
    "mode": "Incremental",
    "outputs": null,
    "parameters": null,
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "SuccessBricks.ClearDB",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "eastus2"
            ],
            "properties": null,
            "resourceType": "databases"
          }
        ]
      },
      {
        "id": null,
        "namespace": "Microsoft.Web",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "eastus2"
            ],
            "properties": null,
            "resourceType": "serverfarms"
          },
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "eastus2"
            ],
            "properties": null,
            "resourceType": "sites"
          },
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              null
            ],
            "properties": null,
            "resourceType": "sites/config"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-09-05T15:19:21.069496+00:00"
  },
  "resourceGroup": "[netid]-[resource group name]-rg"
}
```

## Appendix: Setting Up Local Development Environment

### Configure a Local Apache, MySQL, PHP Environment

#### Mac OSX Sierra
The instructions for creating a local AMP (Apache/MySQL/PHP) development environment are detailed at the following links:

* [macOS 10.12 Sierra Apache Setup: Multiple PHP Versions](https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions)
* [macOS 10.12 Sierra Apache Setup: MySQL, APC & More...](https://getgrav.org/blog/macos-sierra-apache-mysql-vhost-apc)

#### Other Operating Systems
_TODO_

### Deploy Azure App Service for Linux and MySQL Database and Prepare Local Development Environment

Deploy the azure template: `template`

Specificy a new resource group. This resource group will comprise an App Service for Linux web app, an App Service Plan (ASP), and a MySQL database.

Open the Azure Portal, select your newly created App Service application and bring up its properties blade. Click download publish settings file.

This file contains information that can be used by Visual Studio to publish an application to Azure.

### Create a TestDrive PHP Application

#### Install YII

The [YII PHP Framework](http://www.yiiframework.com/) will be used to create a test PHP application.

The most current version is 2.0; For the sake of this exercise, version 1.0 will be used.

Install `composer`:

```bash
$ brew install composer
```

Install the `composer` asset plugin:

```bash
composer global require "fxp/composer-asset-plugin:^1.3.1"
```

Change directory into your `~/Sites` folder and create a new `yii` project:

```bash
composer create-project --prefer-dist yiisoft/yii
```

Verify that all the framework requirements have been met by opening a web browser to [http://localhost/yii/requirements/index.php](http://localhost/yii/requirements/index.php).

Create a new application named `testdrive`:

```bash
$ cd ${HOME}/Sites/yii/framework
$ php yiic webapp ../testdrive
```

This will create a new application skeleton `testdrive` at `${HOME}/yii/testdrive`.

You may browse to the application by opening the browser to [http://localhost/yii/testdrive/index.php](http://localhost/yii/testdrive/index.php)

#### Create a DB for testdrive

```bash
$ mysql -u root -p -e "CREATE DATABASE testdrive;"
Enter password:*
# Add User and Password for Azure MySQL DB user 4afa927fc0904a with a password 4aaf0654
$ mysql -u root -p -e "GRANT ALL PRIVILEGES ON *.* TO '4afa927fc0904a'@'localhost' IDENTIFIED BY '4aaf0654';"
Enter password:*
# Initialize the tables in `testdrive` using SQL provided by framework
$ mysql -u4afa927fc0904a -p4aaf0654 testdrive  < "${HOME}/Sites/yii/testdrive/protected/data/schema.mysql.sql"
````

#### Create User Model and User CRUD with GII Tool

Apache environment variables will be used to identify the environment, to specify a password for GII and to specify the database connection string.

Edt `/usr/local/etc/apache2/2.4/httpd.conf` and append the following lines:

```conf
SetEnv PRD_TST_DEV_LCL                "LCL"
SetEnv TESTDRIVE_GII_PASSWORD         "changeThisInProduction"
SetEnv MYSQLCONNSTR_defaultConnection "<Copied from Azure DB defaultConnection Property>"
```

Copy the *defaultConnection* string from the Azure database properties. Edit the string by replacing the `Database` value with the name of the local MySQL database and by replacing the `Data Source` value with `localhost`. Leave the rest of the string alone.

The connection string for the local development environment will use the `testdrive` name for the database an `localhost` for the data source.

The `httpd.conf` should have the following lines at the end:

```conf
SetEnv PRD_TST_DEV_LCL                "LCL"
SetEnv TESTDRIVE_GII_PASSWORD         "changeThisInProduction"
SetEnv MYSQLCONNSTR_defaultConnection "Database=testdrive;Data Source=localhost;User Id=4afa927fc0904a;Password=4aaf0654"
```

The GII module can now be enabled. (TODO: create a more Environment aware method of enabling development code.)

Open the file `${HOME}/Sites/yii/testdrive/protected/config/main.php` and find the following block of code:

```php
  'modules'=>array(
    // uncomment the following to enable the Gii tool
    /*
    'gii'=>array(
      'class'=>'system.gii.GiiModule',
      'password'=>'Enter Your Password Here',
      // If removed, Gii defaults to localhost only. Edit carefully to taste.
      'ipFilters'=>array('127.0.0.1','::1'),
    ),
    */
  ),
```

Remove the multi-line comment delimiters `/*` and `*/`; Replace the string `'Enter Your Password Here'` with `'getenv('TESTDRIVE_GII_PASSWORD')`. The local PHP engine will read the environment variable's value set in `httpd.conf`. (In Azure, PHP will pull the value from the Azure application service's settings.)

By default the YII framework will use a SQLite database. In order to use the MySQL database created above, open the file `${HOME}/Sites/yii/testdrive/protected/config/database.php`.

Replace the contents of the file with the following code:

```php
<?php

// This is the database connection configuration.

# The value for MYSQLCONNSTR_defaultConnection is read from Azure PaaS environment
# or specified in ./etc/apache2/2.4/httpd.conf

$msAzureConnStrRegex = '/Database=(.+);Data Source=(.+);User Id=(.+);Password=(.+)/';
preg_match($msAzureConnStrRegex, getenv('MYSQLCONNSTR_defaultConnection'), $matches);

return array(
  'connectionString' => 'mysql:host=' . $matches[2] . ';dbname=' . $matches[1],
  'emulatePrepare' => true,
  'username' => $matches[3],
  'password' => $matches[4],
  'charset' => 'utf8',
);
```

The connection string conforms to the format expected by PHP PDO; the parameters for username and password are extracted from the MS Azure formatted connection string.

Open a browser to [http://localhost/yii/testdrive/index.php?r=gii](http://localhost/yii/testdrive/index.php?r=gii). Login using the password `deleteThisInProduction`.

Select the link [Model Generator](http://localhost/yii/testdrive/index.php?r=gii/model/index) on the page.

Enter the following values:

* Database Connection: `db`
* Table Prefix: `tbl_`
* Table Name: `tbl_user`
* Model Class: `User`

Click **Preview**. If there are no errors, click **Generate**.

Now we turn to creating CRUD. Select the [CRUD Generator](http://localhost/yii/testdrive/index.php?r=gii/crud/index) link. Enter `User` for *Model Class* and `user` for *Controller ID*. Click *Preview* and then *Generate*.

We now have the beginnings of a YII application. List the users by visiting [http://localhost/yii/testdrive/index.php?r=user](http://localhost/yii/testdrive/index.php?r=user).

Disable the GII module by returning the multiline comment delimiters to `main.php`

### Migrate testdrive database to Azure

```bash
$ mysqldump mysql -u4afa927fc0904a -p4aaf0654 testdrive > ~/testdrive_backup.sql
# Database exported to testdrive_backup.sql using the credentials for 4afa927fc0904a
$ mysql -h us-cdbr-azure-east2-d.cloudapp.net -u4afa927fc0904a -p4aaf0654 u5ts32sd8az2psda < testdrive_backup.sql
# Database imported to Azure u5ts32sd8az2psda database on us-cdbr-azure-east2-d.cloudapp.net
```

### FTP Local YII App to Azure App Service

From the Azure Resource Managemenent portal, open the website blade. Click on *Overview*. Download click on *Get publish profile*.

This xml file contains settings for publishing an application to a web server. It contains the username, password, and ftp site that will be used to push the web app's files to.

Connect and put the local YII application files to `site/wwwroot`. One can use the `wput` commandline tool to copy the entire application to the site.

Use the Python script [`prepare_dev.py`](https://github.com/YaleUniversity/prepare_dev) to create a properly formatted commandline for `wput`.

Once the files have been copied to the site, the application will work.
