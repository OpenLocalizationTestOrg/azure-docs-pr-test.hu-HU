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
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a><span data-ttu-id="80e0b-103">Azure Resource Manager-sablonnal Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="80e0b-103">Create a Service Bus namespace using an Azure Resource Manager template</span></span>

<span data-ttu-id="80e0b-104">Ez a cikk ismerteti, hogyan toouse Azure Resource Manager-sablon, amely létrehoz egy Service Bus-névtér, típus **Messaging** rendelkező egy Standard/alapszintű Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="80e0b-104">This article describes how toouse an Azure Resource Manager template that creates a Service Bus namespace of type **Messaging** with a Standard/Basic SKU.</span></span> <span data-ttu-id="80e0b-105">hello cikk hello paramétereket, amelyeket hello végrehajtásra hello központi telepítés is meghatároz.</span><span class="sxs-lookup"><span data-stu-id="80e0b-105">hello article also defines hello parameters that are specified for hello execution of hello deployment.</span></span> <span data-ttu-id="80e0b-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="80e0b-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="80e0b-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="80e0b-107">For more information about creating templates, please see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="80e0b-108">Hello teljes sablon, lásd: hello [Service Bus-névtér sablon] [ Service Bus namespace template] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="80e0b-108">For hello complete template, see hello [Service Bus namespace template][Service Bus namespace template] on GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="80e0b-109">Azure Resource Manager-sablonok a következő hello érhetők el a letöltés és telepítés.</span><span class="sxs-lookup"><span data-stu-id="80e0b-109">hello following Azure Resource Manager templates are available for download and deployment.</span></span> 
> 
> * [<span data-ttu-id="80e0b-110">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="80e0b-110">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
> * [<span data-ttu-id="80e0b-111">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="80e0b-111">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
> * [<span data-ttu-id="80e0b-112">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="80e0b-112">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
> * [<span data-ttu-id="80e0b-113">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="80e0b-113">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> <span data-ttu-id="80e0b-114">toocheck hello legújabb sablonok, látogasson el a hello [Azure gyors üzembe helyezési sablonokat] [ Azure Quickstart Templates] gyűjteménye, majd keresse meg a Service Bus.</span><span class="sxs-lookup"><span data-stu-id="80e0b-114">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Service Bus.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="80e0b-115">Mit fog üzembe helyezni?</span><span class="sxs-lookup"><span data-stu-id="80e0b-115">What will you deploy?</span></span>
<span data-ttu-id="80e0b-116">Ezen sablon esetén központilag telepíti a Service Bus-névtér egy [Basic, Standard vagy prémium](https://azure.microsoft.com/pricing/details/service-bus/) Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="80e0b-116">With this template, you will deploy a Service Bus namespace with a [Basic, Standard, or Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.</span></span>

<span data-ttu-id="80e0b-117">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="80e0b-117">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="80e0b-118">[![TooAzure telepítése](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="80e0b-118">[![Deploy tooAzure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="80e0b-119">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="80e0b-119">Parameters</span></span>
<span data-ttu-id="80e0b-120">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="80e0b-120">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="80e0b-121">hello sablon tartalmaz nevű szakaszban `Parameters` , amely tartalmazza az összes hello paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="80e0b-121">hello template includes a section called `Parameters` that contains all of hello parameter values.</span></span> <span data-ttu-id="80e0b-122">Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="80e0b-122">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="80e0b-123">Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos.</span><span class="sxs-lookup"><span data-stu-id="80e0b-123">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="80e0b-124">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="80e0b-124">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="80e0b-125">Ez a sablon a következő paraméterek hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="80e0b-125">This template defines hello following parameters.</span></span>

### <a name="servicebusnamespacename"></a><span data-ttu-id="80e0b-126">serviceBusNamespaceName</span><span class="sxs-lookup"><span data-stu-id="80e0b-126">serviceBusNamespaceName</span></span>
<span data-ttu-id="80e0b-127">a Service Bus-névtér toocreate hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="80e0b-127">hello name of hello Service Bus namespace toocreate.</span></span>

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of hello Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a><span data-ttu-id="80e0b-128">serviceBusSKU</span><span class="sxs-lookup"><span data-stu-id="80e0b-128">serviceBusSKU</span></span>
<span data-ttu-id="80e0b-129">a Service Bus hello hello neve [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.</span><span class="sxs-lookup"><span data-stu-id="80e0b-129">hello name of hello Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) toocreate.</span></span>

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

<span data-ttu-id="80e0b-130">hello sablon határozza meg, amelyeknél engedélyezve van ez a paraméter (alapszintű, Standard vagy prémium) hello értékeket, és hozzárendel egy alapértelmezett értéket (általános), ha nincs érték megadva.</span><span class="sxs-lookup"><span data-stu-id="80e0b-130">hello template defines hello values that are permitted for this parameter (Basic, Standard, or Premium) and assigns a default value (Standard) if no value is specified.</span></span>

<span data-ttu-id="80e0b-131">A Service Bus árazással kapcsolatos további információkért lásd: [Service Bus árak és számlázás][Service Bus pricing and billing].</span><span class="sxs-lookup"><span data-stu-id="80e0b-131">For more information about Service Bus pricing, see [Service Bus pricing and billing][Service Bus pricing and billing].</span></span>

### <a name="servicebusapiversion"></a><span data-ttu-id="80e0b-132">serviceBusApiVersion</span><span class="sxs-lookup"><span data-stu-id="80e0b-132">serviceBusApiVersion</span></span>
<span data-ttu-id="80e0b-133">hello sablon hello Service Bus API verzióját.</span><span class="sxs-lookup"><span data-stu-id="80e0b-133">hello Service Bus API version of hello template.</span></span>

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by hello template" 
       } 
```

## <a name="resources-toodeploy"></a><span data-ttu-id="80e0b-134">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="80e0b-134">Resources toodeploy</span></span>
### <a name="service-bus-namespace"></a><span data-ttu-id="80e0b-135">Service Bus-névtér</span><span class="sxs-lookup"><span data-stu-id="80e0b-135">Service Bus namespace</span></span>
<span data-ttu-id="80e0b-136">Létrehoz egy standard Service Bus-névtér típusú **Messaging**.</span><span class="sxs-lookup"><span data-stu-id="80e0b-136">Creates a standard Service Bus namespace of type **Messaging**.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="80e0b-137">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="80e0b-137">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="80e0b-138">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80e0b-138">PowerShell</span></span>
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a><span data-ttu-id="80e0b-139">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80e0b-139">Azure CLI</span></span>
```azurecli
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a><span data-ttu-id="80e0b-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80e0b-140">Next steps</span></span>
<span data-ttu-id="80e0b-141">Most, hogy létrehozott és telepített Azure Resource Manager eszközzel, megtudhatja, hogyan toomanage ezeket az erőforrásokat, ezek a cikkek olvasásával:</span><span class="sxs-lookup"><span data-stu-id="80e0b-141">Now that you've created and deployed resources using Azure Resource Manager, learn how toomanage these resources by reading these articles:</span></span>

* [<span data-ttu-id="80e0b-142">A PowerShell használatával a Service Bus kezelése</span><span class="sxs-lookup"><span data-stu-id="80e0b-142">Manage Service Bus with PowerShell</span></span>](service-bus-manage-with-ps.md)
* [<span data-ttu-id="80e0b-143">A Service Bus Explorer hello Service Bus-erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="80e0b-143">Manage Service Bus resources with hello Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
