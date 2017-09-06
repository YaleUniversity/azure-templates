# app-service-linux

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

