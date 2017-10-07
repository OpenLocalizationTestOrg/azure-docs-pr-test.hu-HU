---
title: "Azure Service Bus üzenettémakör-előfizetésben aaaCreate szabály Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "Hozzon létre egy Service Bus-névtér témakör, előfizetés és Azure Resource Manager-sablon segítségével"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: dbc46da8491aee4d0c73bd4db90c696008920df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Hozzon létre egy Service Bus-névtér témakör, előfizetés és Azure Resource Manager-sablonnal szabály

Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely hoz létre egy Service Bus-névtér egy témakör, előfizetés, és a szabály (szűrő). Megismerheti, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet a követelmények

A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.

Az Azure-erőforrások elnevezési konvenciói eljárások és minták kapcsolatos további információkért lásd: [Azure-erőforrások elnevezési szabályai ajánlott][Recommended naming conventions for Azure resources].

Hello teljes sablon, lásd: hello [témakör, előfizetés és a szabály a Service Bus-névtér] [ Service Bus namespace with topic, subscription, and rule] sablont.

> [!NOTE]
> Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.
> 
> * [Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály](service-bus-resource-manager-namespace-auth-rule.md)
> * [Hozzon létre egy Service Bus-névtér várólista](service-bus-resource-manager-namespace-queue.md)
> * [Service Bus-névtér létrehozása](service-bus-resource-manager-namespace.md)
> * [Hozzon létre egy Service Bus-névtér témakör és előfizetés](service-bus-resource-manager-namespace-topic.md)
> 
> toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, majd keresse meg a Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Mit fog üzembe helyezni?

Ezen sablon esetén telepít egy Service Bus-névtér témakör, előfizetés és a szabály (szűrő).

[Service Bus-üzenettémák és előfizetések](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) egy egy-a-többhöz típusú kommunikációt biztosítanak a egy *közzétételi/előfizetési* mintát. Az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, témakörök és előfizetések használata esetén ehelyett azok exchange keresztül témakör, amely közvetítőként működik. Egy előfizetés tooa témakör hasonlít egy virtuális üzenetsorra, amely toohello témakör küldött üzenetek példányait megkapja. Az előfizetésben szűrő lehetővé teszi toospecify üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelennie.

## <a name="what-are-rules-filters"></a>Mik azok a szabályok (szűrők)?

A sok esetben meghatározott jellemzőkkel rendelkező üzenetek különböző módon kell feldolgozni. tooenable, előfizetések toofind üzeneteket, amelyek tulajdonságokat, és végezze el a módosításokat toothose tulajdonságok is beállíthat. Bár a Service Bus-előfizetések toohello témakör küldött összes üzenet láthatja, csak másolhatja azokat üzenetek toohello virtuális előfizetés üzenetsorába egy részét. Mindez előfizetés-szűrők használata. toolearn szabályok (szűrők), bővebben lásd: [szabályok és a műveletek](service-bus-queues-topics-subscriptions.md#rules-and-actions).

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure Resource Manager érdemes definiálni paraméterek értékeit keresi toospecify hello sablon telepítésekor. hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket. Ezeket az értékeket, amelyek módosítják a hello projekt telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.

hello sablon meghatározza, hogy a következő paraméterek hello:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
a Service Bus-névtér toocreate hello hello neve.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
Service Bus-névtér hello létrehozott hello témakör hello neve.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
a Service Bus-névtér hello létrehozott hello előfizetés hello neve.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName
a Service Bus-névtér hello létrehozott hello rule(filter) hello neve.

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a>serviceBusApiVersion
hello sablon hello Service Bus API verzióját.

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a>Erőforrások toodeploy
Létrehoz egy standard Service Bus-névtér típusú **Messaging**, témakör és előfizetés és szabályok.

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:

* [Az Azure Service Bus kezelése](service-bus-management-libraries.md)
* [A PowerShell használatával a Service Bus kezelése](service-bus-manage-with-ps.md)
* [A Service Bus Explorer hello Service Bus-erőforrások kezelése](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

