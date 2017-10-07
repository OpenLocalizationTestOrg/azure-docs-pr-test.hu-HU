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
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a><span data-ttu-id="1a1c8-103">Hozzon létre egy Service Bus-névtér témakör, előfizetés és Azure Resource Manager-sablonnal szabály</span><span class="sxs-lookup"><span data-stu-id="1a1c8-103">Create a Service Bus namespace with topic, subscription, and rule using an Azure Resource Manager template</span></span>

<span data-ttu-id="1a1c8-104">Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely hoz létre egy Service Bus-névtér egy témakör, előfizetés, és a szabály (szűrő).</span><span class="sxs-lookup"><span data-stu-id="1a1c8-104">This article shows how toouse an Azure Resource Manager template that creates a Service Bus namespace with a topic, subscription, and rule (filter).</span></span> <span data-ttu-id="1a1c8-105">Megismerheti, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-105">You learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="1a1c8-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet a követelmények</span><span class="sxs-lookup"><span data-stu-id="1a1c8-106">You can use this template for your own deployments, or customize it toomeet your requirements</span></span>

<span data-ttu-id="1a1c8-107">A sablonok létrehozásáról további információkat az [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates] (Azure Resource Manager-sablonok készítése) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-107">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="1a1c8-108">Az Azure-erőforrások elnevezési konvenciói eljárások és minták kapcsolatos további információkért lásd: [Azure-erőforrások elnevezési szabályai ajánlott][Recommended naming conventions for Azure resources].</span><span class="sxs-lookup"><span data-stu-id="1a1c8-108">For more information about practice and patterns on Azure resources naming conventions, see [Recommended naming conventions for Azure resources][Recommended naming conventions for Azure resources].</span></span>

<span data-ttu-id="1a1c8-109">Hello teljes sablon, lásd: hello [témakör, előfizetés és a szabály a Service Bus-névtér] [ Service Bus namespace with topic, subscription, and rule] sablont.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-109">For hello complete template, see hello [Service Bus namespace with topic, subscription, and rule][Service Bus namespace with topic, subscription, and rule] template.</span></span>

> [!NOTE]
> <span data-ttu-id="1a1c8-110">Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-110">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="1a1c8-111">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="1a1c8-111">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="1a1c8-112">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="1a1c8-112">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="1a1c8-113">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a1c8-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="1a1c8-114">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="1a1c8-114">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> 
> <span data-ttu-id="1a1c8-115">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, majd keresse meg a Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-115">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="1a1c8-116">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="1a1c8-116">What will you deploy?</span></span>

<span data-ttu-id="1a1c8-117">Ezen sablon esetén telepít egy Service Bus-névtér témakör, előfizetés és a szabály (szűrő).</span><span class="sxs-lookup"><span data-stu-id="1a1c8-117">With this template, you deploy a Service Bus namespace with topic, subscription, and rule (filter).</span></span>

<span data-ttu-id="1a1c8-118">[Service Bus-üzenettémák és előfizetések](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) egy egy-a-többhöz típusú kommunikációt biztosítanak a egy *közzétételi/előfizetési* mintát.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-118">[Service Bus topics and subscriptions](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) provide a one-to-many form of communication, in a *publish/subscribe* pattern.</span></span> <span data-ttu-id="1a1c8-119">Az elosztott alkalmazások összetevői nem közvetlenül egymással kommunikálnak, témakörök és előfizetések használata esetén ehelyett azok exchange keresztül témakör, amely közvetítőként működik. Egy előfizetés tooa témakör hasonlít egy virtuális üzenetsorra, amely toohello témakör küldött üzenetek példányait megkapja.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-119">When using topics and subscriptions, components of a distributed application do not communicate directly with each other, instead they exchange messages via topic that acts as an intermediary.A subscription tooa topic resembles a virtual queue that receives copies of messages that were sent toohello topic.</span></span> <span data-ttu-id="1a1c8-120">Az előfizetésben szűrő lehetővé teszi toospecify üzenetet küldött tooa témakör belül egy adott üzenettémakör-előfizetésben kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-120">A filter on subscription enables you toospecify which messages sent tooa topic should appear within a specific topic subscription.</span></span>

## <a name="what-are-rules-filters"></a><span data-ttu-id="1a1c8-121">Mik azok a szabályok (szűrők)?</span><span class="sxs-lookup"><span data-stu-id="1a1c8-121">What are rules (filters)?</span></span>

<span data-ttu-id="1a1c8-122">A sok esetben meghatározott jellemzőkkel rendelkező üzenetek különböző módon kell feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-122">In many scenarios, messages that have specific characteristics must be processed in different ways.</span></span> <span data-ttu-id="1a1c8-123">tooenable, előfizetések toofind üzeneteket, amelyek tulajdonságokat, és végezze el a módosításokat toothose tulajdonságok is beállíthat.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-123">tooenable this, you can configure subscriptions toofind messages that have specific properties and then perform modifications toothose properties.</span></span> <span data-ttu-id="1a1c8-124">Bár a Service Bus-előfizetések toohello témakör küldött összes üzenet láthatja, csak másolhatja azokat üzenetek toohello virtuális előfizetés üzenetsorába egy részét.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-124">Although Service Bus subscriptions see all messages sent toohello topic, you can only copy a subset of those messages toohello virtual subscription queue.</span></span> <span data-ttu-id="1a1c8-125">Mindez előfizetés-szűrők használata.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-125">This is accomplished using subscription filters.</span></span> <span data-ttu-id="1a1c8-126">toolearn szabályok (szűrők), bővebben lásd: [szabályok és a műveletek](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span><span class="sxs-lookup"><span data-stu-id="1a1c8-126">toolearn more about rules (filters), see [Rules and actions](service-bus-queues-topics-subscriptions.md#rules-and-actions).</span></span>

<span data-ttu-id="1a1c8-127">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="1a1c8-127">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="1a1c8-128">[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="1a1c8-128">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="1a1c8-129">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1a1c8-129">Parameters</span></span>

<span data-ttu-id="1a1c8-130">Az Azure Resource Manager érdemes definiálni paraméterek értékeit keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-130">With Azure Resource Manager, you should define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="1a1c8-131">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-131">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="1a1c8-132">Ezeket az értékeket, amelyek módosítják a hello projekt telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-132">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="1a1c8-133">Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-133">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="1a1c8-134">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-134">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="1a1c8-135">hello sablon meghatározza, hogy a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="1a1c8-135">hello template defines hello following parameters:</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="1a1c8-136">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="1a1c8-136">serviceBusNamespaceName</span></span>
<span data-ttu-id="1a1c8-137">a Service Bus-névtér toocreate hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-137">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a><span data-ttu-id="1a1c8-138">serviceBusTopicName</span><span class="sxs-lookup"><span data-stu-id="1a1c8-138">serviceBusTopicName</span></span>
<span data-ttu-id="1a1c8-139">Service Bus-névtér hello létrehozott hello témakör hello neve.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-139">hello name of hello topic created in hello Service Bus namespace.</span></span>

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a><span data-ttu-id="1a1c8-140">serviceBusSubscriptionName</span><span class="sxs-lookup"><span data-stu-id="1a1c8-140">serviceBusSubscriptionName</span></span>
<span data-ttu-id="1a1c8-141">a Service Bus-névtér hello létrehozott hello előfizetés hello neve.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-141">hello name of hello subscription created in hello Service Bus namespace.</span></span>

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a><span data-ttu-id="1a1c8-142">serviceBusRuleName</span><span class="sxs-lookup"><span data-stu-id="1a1c8-142">serviceBusRuleName</span></span>
<span data-ttu-id="1a1c8-143">a Service Bus-névtér hello létrehozott hello rule(filter) hello neve.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-143">hello name of hello rule(filter) created in hello Service Bus namespace.</span></span>

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
### <a name="servicebusapiversion"></a><span data-ttu-id="1a1c8-144">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="1a1c8-144">serviceBusApiVersion</span></span>
<span data-ttu-id="1a1c8-145">hello sablon hello Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-145">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-toodeploy"></a><span data-ttu-id="1a1c8-146">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="1a1c8-146">Resources toodeploy</span></span>
<span data-ttu-id="1a1c8-147">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, témakör és előfizetés és szabályok.</span><span class="sxs-lookup"><span data-stu-id="1a1c8-147">Creates a standard Service Bus namespace of type **Messaging**, with topic and subscription and rules.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="1a1c8-148">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="1a1c8-148">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="1a1c8-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a1c8-149">PowerShell</span></span>
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="1a1c8-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1a1c8-150">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="1a1c8-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a1c8-151">Next steps</span></span>
<span data-ttu-id="1a1c8-152">Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="1a1c8-152">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="1a1c8-153">Az Azure Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="1a1c8-153">Manage Azure Service Bus</span></span>](service-bus-management-libraries.md)
* [<span data-ttu-id="1a1c8-154">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="1a1c8-154">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="1a1c8-155">A Service Bus Explorer hello Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="1a1c8-155">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

