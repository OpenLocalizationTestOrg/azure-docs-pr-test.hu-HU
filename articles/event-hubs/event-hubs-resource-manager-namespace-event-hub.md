---
title: "az Azure Event Hubs-névteret és a fogyasztói csoportot sablon aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy Event Hubs-névtér az eseményközpontok és egy felhasználói csoport Azure Resource Manager-sablonok használatával"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/12/2017
ms.author: sethm;shvija
ms.openlocfilehash: 74b0d6b3fbe848705e2c20e628aa4e5269b53edb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal esemény hub és felhasználói csoport

Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely hoz létre egy névtér, típus az Event Hubs egy eseményközpontba és egy fogyasztói csoporton. hello hogyan cikkre mutat be toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet a követelmények

A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.

Hello teljes sablon, lásd: hello [hub és felhasználói csoport eseménysablon] [ Event Hub and consumer group template] a Githubon.

> [!NOTE]
> toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] -dokumentumtárban és keressen rá az Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Mit fog üzembe helyezni?
Ezzel a sablonnal egy Event Hubs névtér eseményközpontban, és egy felhasználói csoport fog üzembe helyezni.

[Az Event Hubs](event-hubs-what-is-event-hubs.md) egy Eseményfeldolgozási szolgáltatás használt tooprovide esemény- és telemetriabevitelt érkező tooAzure nagy méretű, alacsony késéssel és nagy megbízhatósággal.

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek
Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket. Ezeket az értékeket, amelyek változhatnak, hello projekt telepít vagy telepít hello környezet toowhich alapján egy paramétert meg kell határozni. Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello. Minden paraméter értéke, hello sablonban hello telepített erőforrások esetén határozza meg.

hello sablon meghatározza, hogy a következő paraméterek hello:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
hello Event Hubs névtér toocreate hello neve.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName
hello hello eseményközpont létre hello Event Hubs névtér neve.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName
hello fogyasztói csoportot hello eseményközpont létre hello neve.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion
hello sablon hello API verzióját.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a>Erőforrások toodeploy
Létrehoz egy névtér, típus **EventHubs**, az event hubs és a fogyasztói csoportot.

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
