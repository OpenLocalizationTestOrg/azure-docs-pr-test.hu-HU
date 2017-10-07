---
title: "aaaCreate Service Bus-névtér Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "Azure Resource Manager sablon toocreate Service Bus-névtér használata"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: fddf370affe761a734991ae9b60c1e5825e54ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Azure Resource Manager-sablonnal Service Bus-névtér létrehozása

Ez a cikk ismerteti, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy Service Bus-névtér, típus **Messaging** rendelkező egy Standard/alapszintű Termékváltozat. hello cikk hello paramétereket, amelyeket hello végrehajtásra hello központi telepítés is meghatároz. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].

Hello teljes sablon, lásd: hello [Service Bus-névtér sablon] [ Service Bus namespace template] a Githubon.

> [!NOTE]
> Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés. 
> 
> * [Hozzon létre egy Service Bus-névtér várólista](service-bus-resource-manager-namespace-queue.md)
> * [Hozzon létre egy Service Bus-névtér témakör és előfizetés](service-bus-resource-manager-namespace-topic.md)
> * [Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály](service-bus-resource-manager-namespace-auth-rule.md)
> * [A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, majd keresse meg a Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Mit fog üzembe helyezni?
Ezen sablon esetén központilag telepíti a Service Bus-névtér egy [Basic, Standard vagy prémium](https://azure.microsoft.com/pricing/details/service-bus/) Termékváltozat.

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek
Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket. Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.

Ez a sablon a következő paraméterek hello határozza meg.

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

### <a name="servicebussku"></a>serviceBusSKU
a Service Bus hello hello neve [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "hello messaging tier for service Bus namespace" 
    } 

```

hello sablon határozza meg, amelyeknél engedélyezve van ez a paraméter (alapszintű, Standard vagy prémium) hello értékeket, és hozzárendel egy alapértelmezett értéket (általános), ha nincs érték megadva.

A Service Bus árazással kapcsolatos további információkért lásd: [Service Bus árak és számlázás][Service Bus pricing and billing].

### <a name="servicebusapiversion"></a>serviceBusApiVersion
hello sablon hello Service Bus API verzióját.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by hello template" 
       } 
```

## <a name="resources-toodeploy"></a>Erőforrások toodeploy
### <a name="service-bus-namespace"></a>Service Bus-névtér
Létrehoz egy standard Service Bus-névtér típusú **Messaging**.

```json
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek olvasásával:

* [A PowerShell használatával a Service Bus kezelése](service-bus-manage-with-ps.md)
* [A Service Bus Explorer hello Service Bus-erőforrások kezelése](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
