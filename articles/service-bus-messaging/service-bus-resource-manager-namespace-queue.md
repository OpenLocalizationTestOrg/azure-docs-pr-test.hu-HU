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
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="604d4-103">A Service Bus-névtér és az Azure Resource Manager-sablonnal várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="604d4-103">Create a Service Bus namespace and a queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="604d4-104">Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy Service Bus-névtér és az adott névtérben várólista.</span><span class="sxs-lookup"><span data-stu-id="604d4-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace and a queue within that namespace.</span></span> <span data-ttu-id="604d4-105">Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="604d4-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="604d4-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="604d4-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="604d4-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="604d4-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="604d4-108">Hello teljes sablon, lásd: hello [Service Bus-névteret és a várólista sablon] [ Service Bus namespace and queue template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="604d4-108">For hello complete template, see hello [Service Bus namespace and queue template][Service Bus namespace and queue template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="604d4-109">Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.</span><span class="sxs-lookup"><span data-stu-id="604d4-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="604d4-110">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="604d4-110">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="604d4-111">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="604d4-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="604d4-112">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="604d4-112">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="604d4-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="604d4-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="604d4-114">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="604d4-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="604d4-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="604d4-115">What will you deploy?</span></span>

<span data-ttu-id="604d4-116">Ezen sablon esetén telepíti a Service Bus-névtér üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="604d4-116">With this template, you will deploy a Service Bus namespace with a queue.</span></span>

<span data-ttu-id="604d4-117">[Service Bus-üzenetsorok](service-bus-queues-topics-subscriptions.md#queues) kínálnak First In, First Out (, FIFO) üzenet kézbesítési tooone vagy több versengő fogyasztó számára.</span><span class="sxs-lookup"><span data-stu-id="604d4-117">[Service Bus queues](service-bus-queues-topics-subscriptions.md#queues) offer First In, First Out (FIFO) message delivery tooone or more competing consumers.</span></span>

<span data-ttu-id="604d4-118">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="604d4-118">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="604d4-119">[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="604d4-119">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="604d4-120">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="604d4-120">Parameters</span></span>

<span data-ttu-id="604d4-121">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="604d4-121">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="604d4-122">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="604d4-122">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="604d4-123">Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="604d4-123">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="604d4-124">Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="604d4-124">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="604d4-125">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="604d4-125">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="604d4-126">hello a sablon a következő paraméterek hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="604d4-126">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="604d4-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="604d4-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="604d4-128">a Service Bus-névtér toocreate hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="604d4-128">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="604d4-129">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="604d4-129">serviceBusQueueName</span></span>
<span data-ttu-id="604d4-130">a Service Bus-névtér hello létrehozott hello várólista hello neve.</span><span class="sxs-lookup"><span data-stu-id="604d4-130">hello name of hello queue created in hello Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="604d4-131">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="604d4-131">serviceBusApiVersion</span></span>
<span data-ttu-id="604d4-132">hello sablon hello Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="604d4-132">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="604d4-133">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="604d4-133">Resources toodeploy</span></span>
<span data-ttu-id="604d4-134">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="604d4-134">Creates a standard Service Bus namespace of type **Messaging**, with a queue.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="604d4-135">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="604d4-135">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="604d4-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="604d4-136">PowerShell</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="604d4-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="604d4-137">Azure CLI</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="604d4-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="604d4-138">Next steps</span></span>
<span data-ttu-id="604d4-139">Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="604d4-139">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="604d4-140">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="604d4-140">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="604d4-141">A Service Bus Explorer hello Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="604d4-141">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
