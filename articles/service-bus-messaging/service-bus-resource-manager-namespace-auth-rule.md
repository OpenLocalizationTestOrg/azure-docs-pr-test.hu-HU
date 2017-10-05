---
title: "Hozzon létre a Service Bus-engedélyezési szabály Azure Resource Manager-sablonnal |} Microsoft Docs"
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
ms.openlocfilehash: fbd2372829a1aefa2c080c0a8a72b9ff4375b16f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="b880c-103">Hozzon létre egy Service Bus-névteret és az Azure Resource Manager-sablonnal várólista engedélyezési szabályt</span><span class="sxs-lookup"><span data-stu-id="b880c-103">Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="b880c-104">Ez a cikk bemutatja, hogyan használható az Azure Resource Manager-sablon által létrehozott egy [engedélyezési szabály](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) egy Service Bus-névtér és a várólista.</span><span class="sxs-lookup"><span data-stu-id="b880c-104">This article shows how to use an Azure Resource Manager template that creates an [authorization rule](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) for a Service Bus namespace and queue.</span></span> <span data-ttu-id="b880c-105">Megtudhatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="b880c-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="b880c-106">Ez a sablont használhatja a saját környezeteiben, vagy testre is szabhatja a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b880c-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="b880c-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="b880c-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="b880c-108">A teljes sablon, tekintse meg a [Service Bus-engedélyezési jogcímszabály-sablont] [ Service Bus auth rule template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="b880c-108">For the complete template, see the [Service Bus authorization rule template][Service Bus auth rule template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="b880c-109">A következő Azure Resource Manager-sablonok letöltése és központi telepítés érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b880c-109">The following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="b880c-110">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="b880c-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="b880c-111">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="b880c-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="b880c-112">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="b880c-112">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="b880c-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="b880c-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="b880c-114">Ellenőrizze a legutóbbi sablonok, látogasson el a [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="b880c-114">To check for the latest templates, visit the [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="b880c-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="b880c-115">What will you deploy?</span></span>
<span data-ttu-id="b880c-116">Ezen sablon esetén telepíti a Service Bus a névtér és üzenetküldési entitás (ebben az esetben a várólista) engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="b880c-116">With this template, you will deploy a Service Bus authorization rule for a namespace and messaging entity (in this case, a queue).</span></span>

<span data-ttu-id="b880c-117">Ez a sablon által [közös hozzáférésű Jogosultságkód (SAS)](service-bus-sas.md) hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="b880c-117">This template uses [Shared Access Signature (SAS)](service-bus-sas.md) for authentication.</span></span> <span data-ttu-id="b880c-118">SAS segítségével az alkalmazások a Service Bus a névtér, vagy az üzenetküldési entitásra (üzenetsor vagy témakör), amellyel adott jogosultságok konfigurálva hozzáférési kulcs használata a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="b880c-118">SAS enables applications to authenticate to Service Bus using an access key configured on the namespace, or on the messaging entity (queue or topic) with which specific rights are associated.</span></span> <span data-ttu-id="b880c-119">Ezt a kulcsot egy SAS-jogkivonatot, amellyel az ügyfelek pedig a Service Bus felé történő hitelesítésre létrehozásához használhatja.</span><span class="sxs-lookup"><span data-stu-id="b880c-119">You can then use this key to generate a SAS token that clients can in turn use to authenticate to Service Bus.</span></span>

<span data-ttu-id="b880c-120">Az automatikus üzembe helyezéshez kattintson az alábbi gombra:</span><span class="sxs-lookup"><span data-stu-id="b880c-120">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="b880c-121">[![Üzembe helyezés az Azure-ban](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="b880c-121">[![Deploy to Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="b880c-122">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="b880c-122">Parameters</span></span>

<span data-ttu-id="b880c-123">Az Azure Resource Managerrel meghatározhatja a sablon üzembe helyezésekor megadandó értékek paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b880c-123">With Azure Resource Manager, you define parameters for values you want to specify when the template is deployed.</span></span> <span data-ttu-id="b880c-124">A sablon nevű szakaszban tartalmaz `Parameters` , amely tartalmazza az összes a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="b880c-124">The template includes a section called `Parameters` that contains all of the parameter values.</span></span> <span data-ttu-id="b880c-125">Meg kell határozni egy paramétert ezeket az értékeket, amelyek a projekt telepít vagy telepít, hogy a környezet alapján változhatnak.</span><span class="sxs-lookup"><span data-stu-id="b880c-125">You should define a parameter for those values that will vary based on the project you are deploying or based on the environment you are deploying to.</span></span> <span data-ttu-id="b880c-126">Az értékeket, amelyeket a rendszer mindig ugyanaz maradjon paraméterek nem határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="b880c-126">Do not define parameters for values that will always stay the same.</span></span> <span data-ttu-id="b880c-127">A sablonban minden egyes paraméterérték az üzembe helyezendő erőforrások megadásához lesz felhasználva.</span><span class="sxs-lookup"><span data-stu-id="b880c-127">Each parameter value is used in the template to define the resources that are deployed.</span></span>

<span data-ttu-id="b880c-128">A sablon a következő paramétereket adja meg.</span><span class="sxs-lookup"><span data-stu-id="b880c-128">The template defines the following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="b880c-129">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="b880c-129">serviceBusNamespaceName</span></span>
<span data-ttu-id="b880c-130">A Service Bus-névtér létrehozása neve.</span><span class="sxs-lookup"><span data-stu-id="b880c-130">The name of the Service Bus namespace to create.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a><span data-ttu-id="b880c-131">namespaceAuthorizationRuleName</span><span class="sxs-lookup"><span data-stu-id="b880c-131">namespaceAuthorizationRuleName</span></span>
<span data-ttu-id="b880c-132">Az engedélyezési szabály meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="b880c-132">The name of the authorization rule for the namespace.</span></span>

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="b880c-133">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="b880c-133">serviceBusQueueName</span></span>
<span data-ttu-id="b880c-134">A várólista a Service Bus-névtér neve.</span><span class="sxs-lookup"><span data-stu-id="b880c-134">The name of the queue in the Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="b880c-135">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="b880c-135">serviceBusApiVersion</span></span>
<span data-ttu-id="b880c-136">A sablon Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="b880c-136">The Service Bus API version of the template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a><span data-ttu-id="b880c-137">Üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="b880c-137">Resources to deploy</span></span>
<span data-ttu-id="b880c-138">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, és egy Service Bus-névteret és entitás engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="b880c-138">Creates a standard Service Bus namespace of type **Messaging**, and a Service Bus authorization rule for namespace and entity.</span></span>

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

## <a name="commands-to-run-deployment"></a><span data-ttu-id="b880c-139">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="b880c-139">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="b880c-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b880c-140">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="b880c-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b880c-141">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="b880c-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b880c-142">Next steps</span></span>
<span data-ttu-id="b880c-143">Most, hogy már létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan kezelheti ezeket az erőforrásokat megtekintésével, ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="b880c-143">Now that you've created and deployed resources using Azure Resource Manager, learn how to manage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="b880c-144">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="b880c-144">Manage Service Bus with PowerShell</span></span>](service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="b880c-145">A Service Bus Explorer Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="b880c-145">Manage Service Bus resources with the Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [<span data-ttu-id="b880c-146">A Service Bus-hitelesítés és engedélyezés</span><span class="sxs-lookup"><span data-stu-id="b880c-146">Service Bus authentication and authorization</span></span>](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
