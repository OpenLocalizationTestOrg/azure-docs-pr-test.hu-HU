---
title: "Azure Service Bus-névtér létrehozása és a feldolgozási sor az Azure Resource Manager-sablonnal |} Microsoft Docs"
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
ms.openlocfilehash: 4358130a2c8e897a0fdd1f9560f766d6e22db4d2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="d507a-103">A Service Bus-névtér és az Azure Resource Manager-sablonnal várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="d507a-103">Create a Service Bus namespace and a queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="d507a-104">Ez a cikk bemutatja, hogyan hozza létre a Service Bus-névtér és az adott névtérben várólista Azure Resource Manager sablonnal.</span><span class="sxs-lookup"><span data-stu-id="d507a-104">This article shows how to use an Azure Resource Manager template that creates a Service Bus namespace and a queue within that namespace.</span></span> <span data-ttu-id="d507a-105">Megtudhatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="d507a-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="d507a-106">Ez a sablont használhatja a saját környezeteiben, vagy testre is szabhatja a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d507a-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="d507a-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="d507a-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="d507a-108">A teljes sablon, tekintse meg a [Service Bus-névteret és a várólista sablon] [ Service Bus namespace and queue template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="d507a-108">For the complete template, see the [Service Bus namespace and queue template][Service Bus namespace and queue template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="d507a-109">A következő Azure Resource Manager-sablonok letöltése és központi telepítés érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d507a-109">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="d507a-110">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="d507a-110">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="d507a-111">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="d507a-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="d507a-112">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="d507a-112">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="d507a-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="d507a-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="d507a-114">Ellenőrizze a legutóbbi sablonok, látogasson el a [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="d507a-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="d507a-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="d507a-115">What will you deploy?</span></span>

<span data-ttu-id="d507a-116">Ezen sablon esetén telepíti a Service Bus-névtér üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="d507a-116">With this template, you will deploy a Service Bus namespace with a queue.</span></span>

<span data-ttu-id="d507a-117">[Service Bus-üzenetsorok](service-bus-queues-topics-subscriptions.md#queues) kínálnak First In, First Out (, FIFO) üzenetküldést biztosítanak egy vagy több versengő fogyasztó számára.</span><span class="sxs-lookup"><span data-stu-id="d507a-117">[Service Bus queues](service-bus-queues-topics-subscriptions.md#queues) offer First In, First Out (FIFO) message delivery to one or more competing consumers.</span></span>

<span data-ttu-id="d507a-118">Az automatikus üzembe helyezéshez kattintson az alábbi gombra:</span><span class="sxs-lookup"><span data-stu-id="d507a-118">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="d507a-119">[![Üzembe helyezés az Azure-ban](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="d507a-119">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="d507a-120">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d507a-120">Parameters</span></span>

<span data-ttu-id="d507a-121">Az Azure Resource Managerrel meghatározhatja a sablon üzembe helyezésekor megadandó értékek paramétereit.</span><span class="sxs-lookup"><span data-stu-id="d507a-121">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="d507a-122">A sablon nevű szakaszban tartalmaz `Parameters` , amely tartalmazza az összes a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="d507a-122">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="d507a-123">Meg kell határozni egy paramétert ezeket az értékeket, amelyek a projekt telepít vagy telepít, hogy a környezet alapján változhatnak.</span><span class="sxs-lookup"><span data-stu-id="d507a-123">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="d507a-124">Az értékeket, amelyeket a rendszer mindig ugyanaz maradjon paraméterek nem határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="d507a-124">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="d507a-125">A sablonban minden egyes paraméterérték az üzembe helyezendő erőforrások megadásához lesz felhasználva.</span><span class="sxs-lookup"><span data-stu-id="d507a-125">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="d507a-126">A sablon a következő paramétereket adja meg.</span><span class="sxs-lookup"><span data-stu-id="d507a-126">The template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="d507a-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="d507a-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="d507a-128">A Service Bus-névtér létrehozása neve.</span><span class="sxs-lookup"><span data-stu-id="d507a-128">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="d507a-129">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="d507a-129">serviceBusQueueName</span></span>
<span data-ttu-id="d507a-130">A várólista létrehozása a Service Bus-névtér neve.</span><span class="sxs-lookup"><span data-stu-id="d507a-130">The name of the queue created in the Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="d507a-131">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="d507a-131">serviceBusApiVersion</span></span>
<span data-ttu-id="d507a-132">A sablon Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="d507a-132">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a><span data-ttu-id="d507a-133">Üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="d507a-133">Resources to deploy</span></span>
<span data-ttu-id="d507a-134">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="d507a-134">Creates a standard Service Bus namespace of type **Messaging**, with a queue.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="d507a-135">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="d507a-135">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="d507a-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d507a-136">PowerShell</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="d507a-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d507a-137">Azure CLI</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="d507a-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d507a-138">Next steps</span></span>
<span data-ttu-id="d507a-139">Most, hogy már létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan kezelheti ezeket az erőforrásokat megtekintésével, ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="d507a-139">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="d507a-140">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="d507a-140">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="d507a-141">A Service Bus Explorer Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="d507a-141">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
