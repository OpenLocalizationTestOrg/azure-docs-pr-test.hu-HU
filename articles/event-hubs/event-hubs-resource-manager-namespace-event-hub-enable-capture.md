---
title: "egy Azure Event Hubs névtér és az engedélyezés rögzítése a sablon használatával aaaCreate |} Microsoft Docs"
description: "Azure Event Hubs-névtér létrehozása egy eseményközponttal és a Rögzítés funkció engedélyezése az Azure Resource Manager sablonjának használatával"
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: a43b4e8d690ae825047e8a9d609bfda89cf2a06f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a>Event Hubs-névtér létrehozása egy eseményközponttal és a Rögzítés funkció engedélyezése az Azure Resource Manager-sablonjának használatával

Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely hoz létre egy Event Hubs névtér egy esemény hub-példány és is lehetővé teszi, hogy hello [rögzítése funkció](event-hubs-capture-overview.md) hello event hub. hello cikkből megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.

Ez a következő cikket is bemutatja, hogyan az, hogy a események kerülnek rögzítésre Azure Storage Blobsba vagy egy Azure Data Lake Store toospecify hello alapján választja cél.

A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.

További információk az Azure-erőforrások elnevezési szabályainak mintáiról és gyakorlati megoldásairól: [Az Azure-erőforrások elnevezési szabályai][Azure Resources naming conventions].

Hello teljes sablonok kattintson a GitHub-hivatkozásokat a következő hello:

- [Event hub és engedélyezése rögzítési tooStorage sablon][Event Hub and enable Capture tooStorage template] 
- [Event hub és engedélyezése rögzítési tooAzure Data Lake Store sablon][Event Hub and enable Capture tooAzure Data Lake Store template]

> [!NOTE]
> toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] -dokumentumtárban és keressen rá az Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Mit fog üzembe helyezni?

Ezzel a sablonnal egy Event Hubs-névteret helyez üzembe eseményközponttal, továbbá engedélyezi az [az Event Hubs Rögzítés funkcióját](event-hubs-capture-overview.md).

[Az Event Hubs](event-hubs-what-is-event-hubs.md) egy Eseményfeldolgozási szolgáltatás használt tooprovide esemény- és telemetriabevitelt érkező tooAzure nagy méretű, alacsony késéssel és nagy megbízhatósággal. Esemény hubok rögzítése lehetővé teszi, hogy Ön tooautomatically hello adatfolyam-adatokat az Event Hubs tooAzure Blob storage szolgáltatásban vagy az Azure Data Lake Store, egy adott időszakra vagy mérete időköze a fájlmegosztásba.

Kattintson a gombra tooenable Event Hubs rögzítheti az Azure Storage a következő hello:

[![TooAzure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)

Kattintson a következő gomb tooenable Event Hubs rögzítheti az Azure Data Lake Store hello:

[![TooAzure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket. Ezeket az értékeket, amelyek módosítják a hello projekt telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.

hello a sablon a következő paraméterek hello határozza meg.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

hello hello neve [Event Hubs névtér](event-hubs-create.md) toocreate.

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of hello EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

hello event hubs hello létrehozott hello nevének [Event Hubs névtér](event-hubs-create.md).

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of hello event hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

nap tooretain köszönőüzenetei hello eseményközpont hello száma. 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long tooretain hello data in event hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

az eseményközpont hello partíciók toocreate hello száma.

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="captureenabled"></a>captureEnabled

Engedélyezze a rögzítési a hello eseményközpontot.

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable hello Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a>captureEncodingFormat

hello kódolási formátum tooserialize hello esemény adatok megadása.

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"hello encoding format in which Capture serializes hello EventData"
    }
}
```

### <a name="capturetime"></a>captureTime

hello alatt az időtartam alatt, amelyben Event Hubs rögzítése kezdődik hello adatok rögzítéséhez.

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"hello time window in seconds for hello capture"
    }
}
```

### <a name="capturesize"></a>captureSize
hello mérete időköz, amit rögzítési kezdi hello adatok rögzítéséhez.

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"hello size window in bytes for capture"
    }
}
```

###<a name="capturenameformat"></a>captureNameFormat

hello névformátum Event Hubs rögzítése toowrite hello az Avro-fájlok által használt. Vegye figyelembe, hogy a Capture névformátumának tartalmaznia kell a következő mezőket: `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` és `{Second}`. Ezek a mezők bármilyen sorrend szerint rendezhetők, elválasztó karakterekkel vagy azok nélkül.
 
```json
"captureNameFormat": {
      "type": "string",
      "defaultValue": "{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}",
      "metadata": {
        "description": "A Capture Name Format must contain {Namespace}, {EventHub}, {PartitionId}, {Year}, {Month}, {Day}, {Hour}, {Minute} and {Second} fields. These can be arranged in any order with or without delimeters. E.g.  Prod_{EventHub}/{Namespace}\\{PartitionId}_{Year}_{Month}/{Day}/{Hour}/{Minute}/{Second}"
      }
    }
  }
```

### <a name="apiversion"></a>apiVersion

hello sablon hello API verzióját.

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by hello template"
    }
 }
```

A következő paraméterek, ha úgy dönt, Azure Storage a célként hello használja.

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Rögzítési van szükség, egy Azure Storage fiók erőforrás azonosítója tooenable tooyour rögzítése Storage-fiók szükséges.

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource ID where you want hello blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

hello mely toocapture a blob-tároló az eseményadatok.

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want hello blobs captured"
    }
}
```

Ha úgy dönt, az Azure Data Lake Store a célként a következő paraméterek hello használja. A Data Lake Store elérési úton, tooCapture hello esemény használni kívánt engedélyeket kell beállítani. tooset engedélyek, lásd: [Ez a cikk](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).

###<a name="subscriptionid"></a>subscriptionId

Hello Event Hubs névtér és az Azure Data Lake Store az előfizetés-azonosító. Mindkét ezeket az erőforrásokat hello alatt kell lennie ugyanazon előfizetés-azonosító.

```json
"subscriptionId": {
    "type": "string",
    "metadata": {
        "description": "Subscription Id of both Azure Data Lake Store and Event Hub namespace"
     }
 }
```

###<a name="datalakeaccountname"></a>dataLakeAccountName

hello Azure Data Lake Store nevét hello eseményeket rögzíteni.

```json
"dataLakeAccountName": {
    "type": "string",
    "metadata": {
        "description": "Azure Data Lake Store name"
    }
}
```

###<a name="datalakefolderpath"></a>dataLakeFolderPath

hello célmappa elérési útját a hello eseményeket rögzíteni.

```json
"dataLakeFolderPath": {
    "type": "string",
    "metadata": {
        "description": "Destination archive folder path"
    }
}
```

## <a name="resources-toodeploy-for-azure-storage-as-destination-toocaptured-events"></a>Cél toocaptured eseményként is rögzíti az Azure Storage erőforrások toodeploy

Létrehoz egy névtér, típus **EventHubs**egy eseményközpontot, valamint lehetővé teszi, hogy a Blob Storage tooAzure rögzítése.

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "CaptureDescription":{
                        "enabled":"[parameters('captureEnabled')]",
                        "encoding":"[parameters('captureEncodingFormat')]",
                        "intervalInSeconds":"[parameters('captureTime')]",
                        "sizeLimitInBytes":"[parameters('captureSize')]",
                        "destination":{
                            "name":"EventHubCapture.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="resources-toodeploy-for-azure-data-lake-store-as-destination"></a>Az Azure Data Lake Store jelölje ki célként erőforrások toodeploy

Létrehoz egy névtér, típus **EventHubs**, egy eseményközpontot, és is lehetővé teszi, hogy rögzítési tooAzure Data Lake Store.

```json
 "resources": [
        {
            "apiVersion": "2015-08-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "2015-08-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('eventHubName')]",
                        "ArchiveDescription": {
                            "enabled": "true",
                            "encoding": "[parameters('archiveEncodingFormat')]",
                            "intervalInSeconds": "[parameters('archiveTime')]",
                            "sizeLimitInBytes": "[parameters('archiveSize')]",
                            "destination": {
                                "name": "EventHubArchive.AzureDataLake",
                                "properties": {
                                    "DataLakeSubscriptionId": "[parameters('subscriptionId')]",
                                    "DataLakeAccountName": "[parameters('dataLakeAccountName')]",
                                    "DataLakeFolderPath": "[parameters('dataLakeFolderPath')]",
                                    "ArchiveNameFormat": "[parameters('archiveNameFormat')]"
                                }
                            }
                        }
                    }
                }
            ]
        }
    ]
```

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

Az Event Hubs rögzítése sablon tooenable üzembe helyezés Azure Storage:
 
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

Az Event Hubs rögzítése sablon tooenable üzembe helyezés az Azure Data Lake Store:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

Az Azure Blob Storage kiválasztása célhelyként:

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```

Az Azure Data Lake Store kiválasztása célhelyként:

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json][]
```

## <a name="next-steps"></a>Következő lépések

Beállíthatja úgy is Event Hubs rögzítése keresztül hello [Azure-portálon](https://portal.azure.com). További információkért lásd: [engedélyezése Event Hubs rögzítése segítségével hello Azure-portálon](event-hubs-capture-enable-through-portal.md).

További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure Resources naming conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture tooStorage template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture
[Event hub and enable Capture tooAzure Data Lake Store template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture-for-adls
