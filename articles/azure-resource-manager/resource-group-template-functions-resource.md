---
title: "aaaAzure Resource Manager sablonfüggvényei - erőforrások |} Microsoft Docs"
description: "Erőforrások hello funkciók toouse az Azure Resource Manager sablon tooretrieve értékeket ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: tomfitz
ms.openlocfilehash: c9d524b338b8b7ea6d8c9e0135d48e4fb8f167c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokhoz erőforrás-funkciók

A Resource Manager biztosít a következő funkciók az erőforrás-értékek első hello:

* [listKeys és a {Value} lista](#listkeys)
* [szolgáltatók](#providers)
* [hivatkozás](#reference)
* [Erőforráscsoport](#resourcegroup)
* [resourceId](#resourceid)
* [előfizetést](#subscription)

tooget értékeket a paraméterek, a változók vagy a hello jelenlegi üzemelő példány, lásd: [központi telepítési érték funkciók](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys és a {Value} lista
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Beolvasása hello minden erőforrástípus, amely támogatja a hello list művelet értékeit. hello leggyakoribb használata `listKeys`. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| resourceName vagy resourceIdentifier |Igen |Karakterlánc |Hello erőforrás egyedi azonosítója. |
| apiVersion |Igen |Karakterlánc |API-verzió erőforrás futásidejű állapot. Általában a hello formátumban **éééé-hh-nn**. |

### <a name="return-value"></a>Visszatérési érték

hello visszaadott listKeys objektum rendelkezik hello a következő formátumban:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Más lista függvények visszatérési formátumuk eltérő. toosee hello formátum egy függvény figyelembevétel hello kimenetek szakaszban látható módon hello példa sablon. 

### <a name="remarks"></a>Megjegyzések

Bármely művelet kezdetű **lista** használható legyen a sablon egy függvényt. hello elérhető műveletek nem csak listKeys többek között is műveletek, például `list`, `listAdminKeys`, és `listStatus`. Azonban nem használható **lista** hello értékeket igénylő műveletekhez kérelem törzse. Például hello [lista fiók SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) kérelemtörzs-paraméterrel, például a művelethez *signedExpiry*, így a sablonon belül nem használható.

toodetermine, mely rendelkezik a list művelet, a következő beállítások hello rendelkezik:

* Nézet hello [REST API-műveleteket](/rest/api/) az erőforrás-szolgáltató és listázási műveletei keressen. Például, a storage-fiókok vannak hello [listKeys művelet](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Használjon hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-parancsmagot. hello alábbi példa lekérdezi valamennyi listázási műveletei storage-fiókok:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* A következő Azure CLI parancs toofilter csak hello listázási műveletei hello használata:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Adja meg a hello erőforrás vagy hello használatával [resourceId függvény](#resourceid), vagy hello formátum `{providerNamespace}/{resourceType}/{resourceName}`.


### <a name="example"></a>Példa

hello következő példa bemutatja, hogyan tooreturn hello elsődleges és másodlagos hello tárfiókból kulcsok kimenete szakasz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountId": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "storageKeysOutput": {
            "value": "[listKeys(parameters('storageAccountId'), '2016-01-01')]",
            "type" : "object"
        }
    }
}
``` 

<a id="providers" />

## <a name="providers"></a>szolgáltatók
`providers(providerNamespace, [resourceType])`

Egy erőforrás-szolgáltató és a támogatott erőforrástípusai információt ad vissza. Ha nem ad meg egy erőforrás típusa, a hello függvény hello erőforrás-szolgáltató az összes hello támogatott típus adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| providerNamespace |Igen |Karakterlánc |Namespace hello szolgáltató |
| a resourceType |Nem |Karakterlánc |a megadott névtér hello típusú erőforrás hello belül. |

### <a name="return-value"></a>Visszatérési érték

Minden támogatott típus eredmény abban az esetben a következő formátumban hello: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Tömb sorrendje hello visszaadott értékek nem garantált.

### <a name="example"></a>Példa

hello a következő példa bemutatja, hogyan toouse hello szolgáltató funkció:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers('Microsoft.Web', 'sites')]",
            "type" : "object"
        }
    }
}
```

hello előző példa visszaad egy objektumot a hello a következő formátumban:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

<a id="reference" />

## <a name="reference"></a>Hivatkozás
`reference(resourceName or resourceIdentifier, [apiVersion])`

Az erőforrás futásidejű állapot képviselő objektum beállítása/beolvasása.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| resourceName vagy resourceIdentifier |Igen |Karakterlánc |Név vagy egy erőforrás egyedi azonosítója. |
| apiVersion |Nem |Karakterlánc |A megadott erőforrás hello API-verzióját. Ez a paraméter kihagyása hello erőforrás nincs kiépítve belül ugyanazt a sablont. Általában a hello formátumban **éééé-hh-nn**. |

### <a name="return-value"></a>Visszatérési érték

Minden erőforrástípus hello hivatkozás függvény különböző tulajdonságait adja vissza. hello függvény nem ad vissza egy egyetlen, előre meghatározott formátumban. toosee hello tulajdonságok erőforrástípusra, térjen vissza hello hello objektum kimenete szakasz hello példában látható módon.

### <a name="remarks"></a>Megjegyzések

hello hivatkozás függvény az értékét a futásidejű állapot osztályból származik, és ezért nem használható hello változók szakaszban. A sablon kimenetének részében használható. 

Hello hivatkozás funkcióval, akkor implicit módon deklarálja, hogy egy erőforrás függ-e egy másik erőforrás, ha a hivatkozott hello erőforrás ki van építve belül ugyanazt a sablont. Nem kell tooalso használata hello dependsOn tulajdonság. hello függvény nem kerül kiértékelésre hello hivatkozott erőforrás amíg nem fejeződik be telepítési.

toosee hello nevét és értékeit erőforrástípusra, hozzon létre egy sablont, amely hello kimenetek szakaszban hello objektum beállítása/beolvasása. Ha az adott típusú erőforrással rendelkezik, a sablon bármely új erőforrások telepítése nélkül hello objektum beállítása/beolvasása. 

Általában akkor használják hello **hivatkozás** működéséhez tooreturn egy adott értéket az objektumot, például hello blob végpont URI vagy teljesen minősített tartománynevét.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

### <a name="example"></a>Példa

hello toodeploy és a hivatkozás hello erőforrása ugyanazt a sablont, használja:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      }
    }
}
``` 

hello előző példa visszaad egy objektumot a hello a következő formátumban:

```json
{
   "creationTime": "2017-06-13T21:24:46.618364Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

hello alábbi példa hivatkozik, amelyek a sablonban nincs telepítve. hello tárfiók már létezik hello belül azonos erőforráscsoportot.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>Erőforráscsoport
`resourceGroup()`

Hello jelenlegi erőforráscsoportban képviselő objektumot adja vissza. 

### <a name="return-value"></a>Visszatérési érték

hello visszaadott objektum van hello a következő formátumban:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Megjegyzések

A közös hello resourceGroup függvény használata hello toocreate erőforrások hello erőforráscsoport és ugyanazon a helyen. hello alábbi példa hello erőforráscsoport helye tooassign hello helye webhelyhez.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Példa

hello következő sablon adja vissza hello erőforráscsoport hello tulajdonságainak.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

hello előző példa visszaad egy objektumot a hello a következő formátumban:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Beolvasása hello erőforrás egyedi azonosítója. Ezt a funkciót használja, ha hello erőforrás neve nem egyértelmű, illetve nem kiépített hello belül ugyanazt a sablont. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nem |karakterlánc (a GUID formátumban) |Alapértelmezett érték: hello előfizetésben. Adjon meg ezt az értéket, amikor egy erőforrást egy másik előfizetésben tooretrieve van szüksége. |
| erőforráscsoport-név |Nem |Karakterlánc |Alapértelmezett érték: a jelenlegi erőforráscsoportban. Adja meg ezt az értéket, ha tooretrieve egy erőforrást egy másik erőforráscsoportban van szükség. |
| a resourceType |Igen |Karakterlánc |Beleértve az erőforrás-szolgáltató névtere erőforrás típusát. |
| resourceName1 |Igen |Karakterlánc |Erőforrás neve. |
| resourceName2 |Nem |Karakterlánc |Következő neve erőforrásszegmensre. Ha az erőforrás van beágyazva. |

### <a name="return-value"></a>Visszatérési érték

hello azonosító eredmény abban az esetben a következő formátumban hello:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Megjegyzések

hello megadott paraméterértékek függnek, hogy hello erőforrás hello a jelenlegi telepítésben hello azonos előfizetésbe és erőforráscsoportba csoportban.

ugyanaz a tárfiókon lévő hello azonosító tooget hello erőforrás előfizetés és az erőforráscsoport:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

tooget hello erőforrás-azonosító a tárfiók ugyanahhoz az előfizetéshez, de egy másik erőforráscsoportban található, használja hello:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

tooget hello erőforrás-azonosító egy másik előfizetést, és az erőforráscsoportot, egy tárfiókot használja:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

tooget hello erőforrás-azonosító egy másik erőforráscsoportban található adatbázis használata:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Gyakran kell toouse Ez a függvény egy tárfiókhoz vagy a virtuális hálózat használata egy másik erőforráscsoportban. hello következő példa bemutatja, hogyan könnyen használható egy külső erőforráscsoportból erőforrás:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Példa

hello alábbi példa hello erőforrás-Azonosítót adja vissza egy tárfiók hello erőforráscsoport:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| sameRGOutput | Karakterlánc | /Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Karakterlánc | /Subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Karakterlánc | /Subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Karakterlánc | /Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.SQL/Servers/serverName/Databases/databaseName |

<a id="subscription" />

## <a name="subscription"></a>előfizetést
`subscription()`

Hello előfizetés hello aktuális központi telepítés részleteit adja vissza. 

### <a name="return-value"></a>Visszatérési érték

hello függvény hello a következő formátumban:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Példa

hello következő példa bemutatja hello előfizetés függvény hívása hello kimenetek szakaszban. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

