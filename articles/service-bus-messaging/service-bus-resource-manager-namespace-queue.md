---
title: "aaaCreate Azure Service Bus-névtér és a feldolgozási sor az Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "A Service Bus-névtér és az Azure Resource Manager sablonnal várólista létrehozása"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: f230878b7c557bdd80d74da0de5a85ba4ee99ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>A Service Bus-névtér és az Azure Resource Manager-sablonnal várólista létrehozása

Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy Service Bus-névtér és az adott névtérben várólista. Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].

Hello teljes sablon, lásd: hello [Service Bus-névteret és a várólista sablon] [ Service Bus namespace and queue template] a Githubon.

> [!NOTE]
> Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.
> 
> * [Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály](service-bus-resource-manager-namespace-auth-rule.md)
> * [Hozzon létre egy Service Bus-névtér témakör és előfizetés](service-bus-resource-manager-namespace-topic.md)
> * [Service Bus-névtér létrehozása](service-bus-resource-manager-namespace.md)
> * [A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."
> 
> 

## <a name="what-will-you-deploy"></a>Mit fog üzembe helyezni?

Ezen sablon esetén telepíti a Service Bus-névtér üzenetsorokat.

[Service Bus-üzenetsorok](service-bus-queues-topics-subscriptions.md#queues) kínálnak First In, First Out (, FIFO) üzenet kézbesítési tooone vagy több versengő fogyasztó számára.

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket. Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.

hello a sablon a következő paraméterek hello határozza meg.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
a Service Bus-névtér toocreate hello hello neve.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
a Service Bus-névtér hello létrehozott hello várólista hello neve.

```json
"serviceBusQueueName": {
"type": "string"
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
Létrehoz egy standard Service Bus-névtér típusú **Messaging**, üzenetsorokat.

```json
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:

* [A PowerShell használatával a Service Bus kezelése](service-bus-manage-with-ps.md)
* [A Service Bus Explorer hello Service Bus-erőforrások kezelése](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
