# compute-disks
Reference deployment
```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "StorageAccountName": {
      "type": "string",
      "defaultValue": ""
    },
    "ContainerName": {
      "type": "string",
      "defaultValue": ""
    },
    "SasToken": {
      "type": "string",
      "defaultValue": ""
    }
  },
  "variables": {    
    "Provider": "/Microsoft.Compute",
    "Resource": "/disks",
    "templateUri": "[concat('https://',parameters('StorageAccountName'),'.blob.core.windows.net/',parameters('ContainerName'),variables('Provider'),variables('Resource'))]"
  },
  "resources": [
    {
      "name": "DeployEmptyManagedDisk",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2016-09-01",
      "dependsOn": [ ],
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[concat(variables('templateUri'), '/disks.json', parameters('SasToken'))]",
          "contentVersion": "2017.09.01.0"
        },
        "parameters": {
          "name": {
            "value": "Empty.vhd"
          },
          "createOption": {
            "value": "Empty"
          },
          "diskSizeGB": {
            "value": 200
          },
          "sku": {
            "value": "Premium_LRS"
          },
          "tags": {
            "value": "[json('{\"TagName\": \"TagValue\"}')]"
          }
        }
      }
    },
    {
      "name": "DeployFromImageManagedDisk",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2016-09-01",
      "dependsOn": [ ],
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[concat(variables('templateUri'), '/disks.json', parameters('SasToken'))]",
          "contentVersion": "2017.09.01.0"
        },
        "parameters": {
          "name": {
            "value": "FromImage.vhd"
          },
          "createOption": {
            "value": "FromImage"
          },
          "imageReference": {
            "value": "/Subscriptions/{subscriptionId}/Providers/Microsoft.Compute/Locations/uswest/Publishers/Microsoft/ArtifactTypes/VMImage/Offers/{offer}"
          },
          "sku": {
            "value": "Premium_LRS"
          },
          "tags": {
            "value": "[json('{\"TagName\": \"TagValue\"}')]"
          }
        }
      }
    },
    {
      "name": "DeployCopyManagedDisk",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2016-09-01",
      "dependsOn": [ ],
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[concat(variables('templateUri'), '/disks.json', parameters('SasToken'))]",
          "contentVersion": "2017.09.01.0"
        },
        "parameters": {
          "name": {
            "value": "Copy.vhd"
          },
          "createOption": {
            "value": "Copy"
          },
          "sourceUri": {
            "value": "https://mystorageaccount.blob.core.windows.net/osimages/osimage.vhd"
          },
          "sku": {
            "value": "Premium_LRS"
          },
          "tags": {
            "value": "[json('{\"TagName\": \"TagValue\"}')]"
          }
        }
      }
    },
    {
      "name": "DeployImportManagedDisk",
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2016-09-01",
      "dependsOn": [ ],
      "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[concat(variables('templateUri'), '/disks.json', parameters('SasToken'))]",
          "contentVersion": "2017.09.01.0"
        },
        "parameters": {
          "name": {
            "value": "Import.vhd"
          },
          "createOption": {
            "value": "Import"
          },
          "sourceUri": {
            "value": "https://mystorageaccount.blob.core.windows.net/osimages/osimage.vhd"
          },
          "sku": {
            "value": "Premium_LRS"
          },
          "tags": {
            "value": "[json('{\"TagName\": \"TagValue\"}')]"
          }
        }
      }
    }
  ],
  "outputs": {
    "Empty": {
      "type": "object",
      "value": "[reference('DeployEmptyManagedDisk').outputs.disks.value]"
    },
    "FromImage": {
      "type": "object",
      "value": "[reference('DeployFromImageManagedDisk').outputs.disks.value]"
    },
    "Copy": {
      "type": "object",
      "value": "[reference('DeployCopyManagedDisk').outputs.disks.value]"
    },
    "Import": {
      "type": "object",
      "value": "[reference('DeployImportManagedDisk').outputs.disks.value]"
    }
  }
}
```
