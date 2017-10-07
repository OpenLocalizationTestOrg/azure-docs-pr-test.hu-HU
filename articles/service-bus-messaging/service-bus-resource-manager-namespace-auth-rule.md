---
title: "aaaCreate Service Bus engedélyezési szabály Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "A névtér és az Azure Resource Manager sablonnal várólista Service Bus engedélyezési szabály létrehozása"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 48df97849281d3b47e9d722d4e821c874644be59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Hozzon létre egy Service Bus-névteret és az Azure Resource Manager-sablonnal várólista engedélyezési szabályt

Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy [engedélyezési szabály](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) egy Service Bus-névtér és a várólista. Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].

Hello teljes sablon, lásd: hello [Service Bus-engedélyezési jogcímszabály-sablont] [ Service Bus auth rule template] a Githubon.

> [!NOTE]
> Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.
> 
> * [Service Bus-névtér létrehozása](service-bus-resource-manager-namespace.md)
> * [Hozzon létre egy Service Bus-névtér várólista](service-bus-resource-manager-namespace-queue.md)
> * [Hozzon létre egy Service Bus-névtér témakör és előfizetés](service-bus-resource-manager-namespace-topic.md)
> * [A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."
> 
> 

## <a name="what-will-you-deploy"></a>Mit fog üzembe helyezni?
Ezen sablon esetén telepíti a Service Bus a névtér és üzenetküldési entitás (ebben az esetben a várólista) engedélyezési szabályt.

Ez a sablon által [közös hozzáférésű Jogosultságkód (SAS)](service-bus-sas.md) hitelesítéshez. SAS lehetővé teszi, hogy az alkalmazások tooauthenticate tooService Bus hello névtér, illetve üzenetküldési entitás (üzenetsor vagy témakör) az adott jogosultságok társított hello szolgáltatást konfigurált hozzáférési kulcs használata. A kulcs toogenerate egy SAS-jogkivonatot, hogy az ügyfelek ezután használhatja tooauthenticate tooService Bus használhatja.

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek

Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket. Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.

hello a sablon a következő paraméterek hello határozza meg.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
a Service Bus-névtér toocreate hello hello neve.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName
hello neve hello engedélyezési szabály hello névtér.

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
a Service Bus-névtér hello hello várólista hello neve.

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
Létrehoz egy standard Service Bus-névtér típusú **Messaging**, és egy Service Bus-névteret és entitás engedélyezési szabályt.

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:

* [A PowerShell használatával a Service Bus kezelése](service-bus-powershell-how-to-provision.md)
* [A Service Bus Explorer hello Service Bus-erőforrások kezelése](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [A Service Bus-hitelesítés és engedélyezés](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
