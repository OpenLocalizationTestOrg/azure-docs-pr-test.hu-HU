---
title: "Hozzon létre Azure Service Bus névtér témakör előfizetést Azure Resource Manager-sablonnal |} Microsoft Docs"
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
ms.openlocfilehash: 8dd48787e7b788d249085b3110484de1a2c1d265
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a><span data-ttu-id="f74c7-103">Hozzon létre egy Service Bus-névtér témakör és előfizetés Azure Resource Manager-sablonnal</span><span class="sxs-lookup"><span data-stu-id="f74c7-103">Create a Service Bus namespace with topic and subscription using an Azure Resource Manager template</span></span>

<span data-ttu-id="f74c7-104">Ez a cikk bemutatja, hogyan használható az Azure Resource Manager-sablon által létrehozott Service Bus-névtér és a témakör és előfizetés az adott névtérben.</span><span class="sxs-lookup"><span data-stu-id="f74c7-104">This article shows how to use an Azure Resource Manager template that creates a Service Bus namespace and a topic and subscription within that namespace.</span></span> <span data-ttu-id="f74c7-105">Megtudhatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="f74c7-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="f74c7-106">Ezt a sablont saját üzembe helyezési műveleteihez is használhatja, vagy akár igényeinek megfelelően testre is szabhatja</span><span class="sxs-lookup"><span data-stu-id="f74c7-106">You can use this template for your own deployments, or customize it to meet your requirements</span></span>

<span data-ttu-id="f74c7-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="f74c7-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="f74c7-108">A teljes sablon, tekintse meg a [Service Bus-névtér témakör és előfizetés] [ Service Bus namespace with topic and subscription] sablont.</span><span class="sxs-lookup"><span data-stu-id="f74c7-108">For the complete template, see the [Service Bus namespace with topic and subscription][Service Bus namespace with topic and subscription] template.</span></span>

> [!NOTE]
> <span data-ttu-id="f74c7-109">A következő Azure Resource Manager-sablonok letöltése és központi telepítés érhetők el.</span><span class="sxs-lookup"><span data-stu-id="f74c7-109">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="f74c7-110">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="f74c7-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="f74c7-111">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="f74c7-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="f74c7-112">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="f74c7-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="f74c7-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="f74c7-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="f74c7-114">Ellenőrizze a legutóbbi sablonok, látogasson el a [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="f74c7-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="f74c7-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="f74c7-115">What will you deploy?</span></span>

<span data-ttu-id="f74c7-116">Ezen sablon esetén telepíti a Service Bus-névtér témakör és előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f74c7-116">With this template, you will deploy a Service Bus namespace with topic and subscription.</span></span>

<span data-ttu-id="f74c7-117">[Service Bus-üzenettémák és előfizetések](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) egy egy-a-többhöz típusú kommunikációt biztosítanak a egy *közzétételi/előfizetési* mintát.</span><span class="sxs-lookup"><span data-stu-id="f74c7-117">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span>

<span data-ttu-id="f74c7-118">Az automatikus üzembe helyezéshez kattintson az alábbi gombra:</span><span class="sxs-lookup"><span data-stu-id="f74c7-118">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="f74c7-119">[![Üzembe helyezés az Azure-ban](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="f74c7-119">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="f74c7-120">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="f74c7-120">Parameters</span></span>

<span data-ttu-id="f74c7-121">Az Azure Resource Managerrel meghatározhatja a sablon üzembe helyezésekor megadandó értékek paramétereit.</span><span class="sxs-lookup"><span data-stu-id="f74c7-121">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="f74c7-122">A sablon nevű szakaszban tartalmaz `Parameters` , amely tartalmazza az összes a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="f74c7-122">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="f74c7-123">Meg kell határozni egy paramétert ezeket az értékeket, amelyek a projekt telepít vagy telepít, hogy a környezet alapján változhatnak.</span><span class="sxs-lookup"><span data-stu-id="f74c7-123">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="f74c7-124">Az értékeket, amelyeket a rendszer mindig ugyanaz maradjon paraméterek nem határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="f74c7-124">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="f74c7-125">A sablonban minden egyes paraméterérték az üzembe helyezendő erőforrások megadásához lesz felhasználva.</span><span class="sxs-lookup"><span data-stu-id="f74c7-125">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="f74c7-126">A sablon a következő paramétereket adja meg.</span><span class="sxs-lookup"><span data-stu-id="f74c7-126">The template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="f74c7-127">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="f74c7-127">serviceBusNamespaceName</span></span>
<span data-ttu-id="f74c7-128">A Service Bus-névtér létrehozása neve.</span><span class="sxs-lookup"><span data-stu-id="f74c7-128">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="f74c7-129">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="f74c7-129">serviceBusTopicName</span></span>
<span data-ttu-id="f74c7-130">A témakör a Service Bus-névtér létrehozása neve.</span><span class="sxs-lookup"><span data-stu-id="f74c7-130">The name of the topic created in the Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="f74c7-131">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="f74c7-131">serviceBusSubscriptionName</span></span>
<span data-ttu-id="f74c7-132">Az előfizetés létrehozása a Service Bus-névtér neve.</span><span class="sxs-lookup"><span data-stu-id="f74c7-132">The name of the subscription created in the Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="f74c7-133">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="f74c7-133">serviceBusApiVersion</span></span>
<span data-ttu-id="f74c7-134">A sablon Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="f74c7-134">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a><span data-ttu-id="f74c7-135">Üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="f74c7-135">Resources to deploy</span></span>
<span data-ttu-id="f74c7-136">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, témakör és előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f74c7-136">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="f74c7-137">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="f74c7-137">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="f74c7-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f74c7-138">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="f74c7-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f74c7-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="f74c7-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f74c7-140">Next steps</span></span>
<span data-ttu-id="f74c7-141">Most, hogy már létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan kezelheti ezeket az erőforrásokat megtekintésével, ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="f74c7-141">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="f74c7-142">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="f74c7-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="f74c7-143">A Service Bus Explorer Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="f74c7-143">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus namespace with topic and subscription]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
