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
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a><span data-ttu-id="fbd3c-103">Hozzon létre egy Service Bus-névteret és az Azure Resource Manager-sablonnal várólista engedélyezési szabályt</span><span class="sxs-lookup"><span data-stu-id="fbd3c-103">Create a Service Bus authorization rule for namespace and queue using an Azure Resource Manager template</span></span>

<span data-ttu-id="fbd3c-104">Ez a cikk bemutatja, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy [engedélyezési szabály](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) egy Service Bus-névtér és a várólista.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-104">This article shows how toouse an Azure Resource Manager template that creates an [authorization rule](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) for a Service Bus namespace and queue.</span></span> <span data-ttu-id="fbd3c-105">Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="fbd3c-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="fbd3c-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="fbd3c-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="fbd3c-108">Hello teljes sablon, lásd: hello [Service Bus-engedélyezési jogcímszabály-sablont] [ Service Bus auth rule template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-108">For hello complete template, see hello [Service Bus authorization rule template][Service Bus auth rule template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="fbd3c-109">Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span>
> 
> * [<span data-ttu-id="fbd3c-110">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbd3c-110">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
> * [<span data-ttu-id="fbd3c-111">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="fbd3c-111">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="fbd3c-112">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="fbd3c-112">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="fbd3c-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbd3c-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="fbd3c-114">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="fbd3c-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for "Service Bus."</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="fbd3c-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="fbd3c-115">What will you deploy?</span></span>
<span data-ttu-id="fbd3c-116">Ezen sablon esetén telepíti a Service Bus a névtér és üzenetküldési entitás (ebben az esetben a várólista) engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-116">With this template, you will deploy a Service Bus authorization rule for a namespace and messaging entity (in this case, a queue).</span></span>

<span data-ttu-id="fbd3c-117">Ez a sablon által [közös hozzáférésű Jogosultságkód (SAS)](service-bus-sas.md) hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-117">This template uses [Shared Access Signature (SAS)](service-bus-sas.md) for authentication.</span></span> <span data-ttu-id="fbd3c-118">SAS lehetővé teszi, hogy az alkalmazások tooauthenticate tooService Bus hello névtér, illetve üzenetküldési entitás (üzenetsor vagy témakör) az adott jogosultságok társított hello szolgáltatást konfigurált hozzáférési kulcs használata.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-118">SAS enables applications tooauthenticate tooService Bus using an access key configured on hello namespace, or on hello messaging entity (queue or topic) with which specific rights are associated.</span></span> <span data-ttu-id="fbd3c-119">A kulcs toogenerate egy SAS-jogkivonatot, hogy az ügyfelek ezután használhatja tooauthenticate tooService Bus használhatja.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-119">You can then use this key toogenerate a SAS token that clients can in turn use tooauthenticate tooService Bus.</span></span>

<span data-ttu-id="fbd3c-120">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="fbd3c-120">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="fbd3c-121">[![TooAzure telepítése](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="fbd3c-121">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="fbd3c-122">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="fbd3c-122">Parameters</span></span>

<span data-ttu-id="fbd3c-123">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-123">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="fbd3c-124">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-124">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="fbd3c-125">Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-125">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="fbd3c-126">Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-126">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="fbd3c-127">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-127">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="fbd3c-128">hello a sablon a következő paraméterek hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-128">hello template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="fbd3c-129">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="fbd3c-129">serviceBusNamespaceName</span></span>
<span data-ttu-id="fbd3c-130">a Service Bus-névtér toocreate hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-130">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a><span data-ttu-id="fbd3c-131">namespaceAuthorizationRuleName</span><span class="sxs-lookup"><span data-stu-id="fbd3c-131">namespaceAuthorizationRuleName</span></span>
<span data-ttu-id="fbd3c-132">hello neve hello engedélyezési szabály hello névtér.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-132">hello name of hello authorization rule for hello namespace.</span></span>

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a><span data-ttu-id="fbd3c-133">serviceBusQueueName</span><span class="sxs-lookup"><span data-stu-id="fbd3c-133">serviceBusQueueName</span></span>
<span data-ttu-id="fbd3c-134">a Service Bus-névtér hello hello várólista hello neve.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-134">hello name of hello queue in hello Service Bus namespace.</span></span>

```json
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a><span data-ttu-id="fbd3c-135">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="fbd3c-135">serviceBusApiVersion</span></span>
<span data-ttu-id="fbd3c-136">hello sablon hello Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-136">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-toodeploy"></a><span data-ttu-id="fbd3c-137">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="fbd3c-137">Resources toodeploy</span></span>
<span data-ttu-id="fbd3c-138">Létrehoz egy standard Service Bus-névtér típusú **Messaging**, és egy Service Bus-névteret és entitás engedélyezési szabályt.</span><span class="sxs-lookup"><span data-stu-id="fbd3c-138">Creates a standard Service Bus namespace of type **Messaging**, and a Service Bus authorization rule for namespace and entity.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="fbd3c-139">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="fbd3c-139">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="fbd3c-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fbd3c-140">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a><span data-ttu-id="fbd3c-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fbd3c-141">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a><span data-ttu-id="fbd3c-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbd3c-142">Next steps</span></span>
<span data-ttu-id="fbd3c-143">Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek megtekintésével:</span><span class="sxs-lookup"><span data-stu-id="fbd3c-143">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by viewing these articles:</span></span>

* [<span data-ttu-id="fbd3c-144">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="fbd3c-144">Manage Service Bus with PowerShell</span></span>](service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="fbd3c-145">A Service Bus Explorer hello Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="fbd3c-145">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [<span data-ttu-id="fbd3c-146">A Service Bus-hitelesítés és engedélyezés</span><span class="sxs-lookup"><span data-stu-id="fbd3c-146">Service Bus authentication and authorization</span></span>](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
