---
title: "aaaCreate Azure Service Bus névtér üzenettémakör-előfizetésben Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "Hozzon létre egy Service Bus-névtér témakör és előfizetés Azure Resource Manager-sablonnal"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: d3d55200-5c60-4b5f-822d-59974cafff0e
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;shvija
ms.openlocfilehash: 9b5f7d8710e598b73c0a7ea3daf8c300f7fa9ecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a><span data-ttu-id="0755a-103">Hozzon létre egy Service Bus-névtér témakör és előfizetés Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="0755a-103">Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template</span></span>

<span data-ttu-id="0755a-104">Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy Service Bus-névtér és a témakör és az előfizetés az adott névtérben.</span><span class="sxs-lookup"><span data-stu-id="0755a-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace and a topic and subscription within that namespace.</span></span> <span data-ttu-id="0755a-105">Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="0755a-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="0755a-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet a követelmények</span><span class="sxs-lookup"><span data-stu-id="0755a-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="0755a-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="0755a-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="0755a-108">Hello teljes sablon, lásd: hello [Service Bus-névtér témakör és előfizetés] [ Service Bus namespace with topic and subscription] sablont.</span><span class="sxs-lookup"><span data-stu-id="0755a-108">For hello complete template, see hello [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] template.</span></span>

> [!NOTE]
> <span data-ttu-id="0755a-109">Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.</span><span class="sxs-lookup"><span data-stu-id="0755a-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="0755a-110">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="0755a-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="0755a-111">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="0755a-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="0755a-112">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="0755a-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="0755a-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="0755a-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="0755a-114">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="0755a-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="0755a-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="0755a-115">What will you deploy?</span></span>

<span data-ttu-id="0755a-116">Ezen sablon esetén telepíti a Service Bus-névtér témakör és előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0755a-116">With this template, you will deploy a Service Bus namespace with topic and subscription.</span></span>

<span data-ttu-id="0755a-117">[Service Bus-üzenettémák és előfizetések](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) egy egy-a-többhöz típusú kommunikációt biztosítanak a egy *közzétételi/előfizetési* mintát.</span><span class="sxs-lookup"><span data-stu-id="0755a-117">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span>

<span data-ttu-id="0755a-118">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="0755a-118">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="0755a-119">[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="0755a-119">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="0755a-120">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="0755a-120">Parameters</span></span>

<span data-ttu-id="0755a-121">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="0755a-121">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="0755a-122">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="0755a-122">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="0755a-123">Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="0755a-123">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="0755a-124">Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="0755a-124">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="0755a-125">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="0755a-125">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="0755a-126">hello a sablon a következő paraméterek hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0755a-126">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="0755a-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="0755a-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="0755a-128">a Service Bus-névtér toocreate hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="0755a-128">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="0755a-129">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="0755a-129">serviceBusTopicName</span></span>
<span data-ttu-id="0755a-130">Service Bus-névtér hello létrehozott hello témakör hello neve.</span><span class="sxs-lookup"><span data-stu-id="0755a-130">hello name of hello topic created in hello Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="0755a-131">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="0755a-131">serviceBusSubscriptionName</span></span>
<span data-ttu-id="0755a-132">a Service Bus-névtér hello létrehozott hello előfizetés hello neve.</span><span class="sxs-lookup"><span data-stu-id="0755a-132">hello name of hello subscription created in hello Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="0755a-133">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="0755a-133">serviceBusApiVersion</span></span>
<span data-ttu-id="0755a-134">hello sablon hello Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="0755a-134">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a><span data-ttu-id="0755a-135">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="0755a-135">Resources toodeploy</span></span>
<span data-ttu-id="0755a-136">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, témakör és előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0755a-136">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription.</span></span>

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
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="0755a-137">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="0755a-137">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="0755a-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0755a-138">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="0755a-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0755a-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="0755a-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0755a-140">Next steps</span></span>
<span data-ttu-id="0755a-141">Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="0755a-141">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="0755a-142">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="0755a-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="0755a-143">A Service Bus Explorer hello Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="0755a-143">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
