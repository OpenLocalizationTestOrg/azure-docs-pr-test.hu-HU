---
title: "aaaCreate Azure Service Bus-erőforrások Azure Resource Manager-sablonok |} Microsoft Docs"
description: "Azure Resource Manager sablonok tooautomate hello létrehozása a Service Bus erőforrásainak használata"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: e539902cae307b63ae7c332580e2064761331ec5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a><span data-ttu-id="a1edf-103">Service Bus erőforrásainak használata Azure Resource Manager-sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1edf-103">Create Service Bus resources using Azure Resource Manager templates</span></span>

<span data-ttu-id="a1edf-104">Ez a cikk ismerteti, hogyan toocreate és központi telepítése a Service Bus erőforrásainak Azure Resource Manager sablonok, a PowerShell és a Service Bus-hello erőforrás-szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="a1edf-104">This article describes how toocreate and deploy Service Bus resources using Azure Resource Manager templates, PowerShell, and hello Service Bus resource provider.</span></span>

<span data-ttu-id="a1edf-105">Az Azure Resource Manager-sablonok segítségével hello erőforrások toodeploy megoldás, és toospecify paramétereket és változókat, amelyek lehetővé teszik a különböző környezetek tooinput értékeinek meghatározása.</span><span class="sxs-lookup"><span data-stu-id="a1edf-105">Azure Resource Manager templates help you define hello resources toodeploy for a solution, and toospecify parameters and variables that enable you tooinput values for different environments.</span></span> <span data-ttu-id="a1edf-106">hello sablon JSON és, hogy tooconstruct értékeket is használhat az üzembe helyezéshez kifejezések áll.</span><span class="sxs-lookup"><span data-stu-id="a1edf-106">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="a1edf-107">Azure Resource Manager-sablonok és hello sablon formátum döntéseken írt kapcsolatos részletes információkért lásd: [struktúra és az Azure Resource Manager-sablonok szintaxisát](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a1edf-107">For detailed information about writing Azure Resource Manager templates, and a discussion of hello template format, see [structure and syntax of Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a1edf-108">Ez a cikk megjelenítése szereplő példák hogyan hello Azure Resource Manager toocreate Service Bus-névtér toouse és üzenetküldési entitás (várólista).</span><span class="sxs-lookup"><span data-stu-id="a1edf-108">hello examples in this article show how toouse Azure Resource Manager toocreate a Service Bus namespace and messaging entity (queue).</span></span> <span data-ttu-id="a1edf-109">Más sablon példákat a Microsoft hello [Azure gyors üzembe helyezés sablontárban] [ Azure Quickstart Templates gallery] , és keressen a "Service Bus."</span><span class="sxs-lookup"><span data-stu-id="a1edf-109">For other template examples, visit hello [Azure Quickstart Templates gallery][Azure Quickstart Templates gallery] and search for "Service Bus."</span></span>
>
>

## <a name="service-bus-resource-manager-templates"></a><span data-ttu-id="a1edf-110">Service Bus Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="a1edf-110">Service Bus Resource Manager templates</span></span>

<span data-ttu-id="a1edf-111">A Service Bus Azure Resource Manager sablonok letöltése és központi telepítés érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a1edf-111">These Service Bus Azure Resource Manager templates are available for download and deployment.</span></span> <span data-ttu-id="a1edf-112">Kattintson a következő hivatkozások minden egy, a Githubon található hivatkozások toohello sablonokkal kapcsolatos részletekért hello:</span><span class="sxs-lookup"><span data-stu-id="a1edf-112">Click hello following links for details about each one, with links toohello templates on GitHub:</span></span>

* [<span data-ttu-id="a1edf-113">Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1edf-113">Create a Service Bus namespace</span></span>](service-bus-resource-manager-namespace.md)
* [<span data-ttu-id="a1edf-114">Hozzon létre egy Service Bus-névtér várólista</span><span class="sxs-lookup"><span data-stu-id="a1edf-114">Create a Service Bus namespace with queue</span></span>](service-bus-resource-manager-namespace-queue.md)
* [<span data-ttu-id="a1edf-115">Hozzon létre egy Service Bus-névtér témakör és előfizetés</span><span class="sxs-lookup"><span data-stu-id="a1edf-115">Create a Service Bus namespace with topic and subscription</span></span>](service-bus-resource-manager-namespace-topic.md)
* [<span data-ttu-id="a1edf-116">Hozzon létre egy Service Bus-névtér várólista és engedélyezési szabály</span><span class="sxs-lookup"><span data-stu-id="a1edf-116">Create a Service Bus namespace with queue and authorization rule</span></span>](service-bus-resource-manager-namespace-auth-rule.md)
* [<span data-ttu-id="a1edf-117">A témakör, előfizetés és a szabály a Service Bus-névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1edf-117">Create a Service Bus namespace with topic, subscription, and rule</span></span>](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a><span data-ttu-id="a1edf-118">Üzembe helyezés a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="a1edf-118">Deploy with PowerShell</span></span>

<span data-ttu-id="a1edf-119">hello alábbi eljárás ismerteti, hogyan toouse PowerShell toodeploy Azure Resource Manager-sablon, amely létrehoz egy **szabványos** réteg a Service Bus-névtér, és egy sor az adott névtérben.</span><span class="sxs-lookup"><span data-stu-id="a1edf-119">hello following procedure describes how toouse PowerShell toodeploy an Azure Resource Manager template that creates a **Standard** tier Service Bus namespace, and a queue within that namespace.</span></span> <span data-ttu-id="a1edf-120">Ez a példa alapján hello [hozzon létre egy Service Bus-névtér várólista](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) sablont.</span><span class="sxs-lookup"><span data-stu-id="a1edf-120">This example is based on hello [Create a Service Bus namespace with queue](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) template.</span></span> <span data-ttu-id="a1edf-121">hello hozzávetőleges munkafolyamata a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="a1edf-121">hello approximate workflow is as follows:</span></span>

1. <span data-ttu-id="a1edf-122">Telepítse a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a1edf-122">Install PowerShell.</span></span>
2. <span data-ttu-id="a1edf-123">Hozzon létre hello sablon és (opcionálisan) a paraméter.</span><span class="sxs-lookup"><span data-stu-id="a1edf-123">Create hello template and (optionally) a parameter file.</span></span>
3. <span data-ttu-id="a1edf-124">PowerShell, a bejelentkezés tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a1edf-124">In PowerShell, log in tooyour Azure account.</span></span>
4. <span data-ttu-id="a1edf-125">Ha még nem létezik, hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a1edf-125">Create a new resource group if one does not exist.</span></span>
5. <span data-ttu-id="a1edf-126">Hello telepítés tesztelésére.</span><span class="sxs-lookup"><span data-stu-id="a1edf-126">Test hello deployment.</span></span>
6. <span data-ttu-id="a1edf-127">Ha szükséges, állítsa be a hello telepítési módban.</span><span class="sxs-lookup"><span data-stu-id="a1edf-127">If desired, set hello deployment mode.</span></span>
7. <span data-ttu-id="a1edf-128">Hello sablon üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="a1edf-128">Deploy hello template.</span></span>

<span data-ttu-id="a1edf-129">Azure Resource Manager-sablonok telepítésével kapcsolatos részletes információkért lásd: [telepítése Azure Resource Manager-sablonok erőforrások][Deploy resources with Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="a1edf-129">For complete information about deploying Azure Resource Manager templates, see [Deploy resources with Azure Resource Manager templates][Deploy resources with Azure Resource Manager templates].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="a1edf-130">A PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="a1edf-130">Install PowerShell</span></span>

<span data-ttu-id="a1edf-131">Telepítse az Azure PowerShell hello utasításait követve [Ismerkedés az Azure PowerShell](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="a1edf-131">Install Azure PowerShell by following hello instructions in [Getting started with Azure PowerShell](/powershell/azure/get-started-azureps).</span></span>

### <a name="create-a-template"></a><span data-ttu-id="a1edf-132">Sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1edf-132">Create a template</span></span>

<span data-ttu-id="a1edf-133">Klónozott vagy másolat hello [201-szolgáltatásbusz--várólista létrehozása](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) sablont a Githubból:</span><span class="sxs-lookup"><span data-stu-id="a1edf-133">Clone or copy hello [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) template from GitHub:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of hello Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by hello template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a><span data-ttu-id="a1edf-134">Hozzon létre egy paraméterfájl (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="a1edf-134">Create a parameters file (optional)</span></span>

<span data-ttu-id="a1edf-135">toouse egy választható paraméterek: fájl másolása hello [201-szolgáltatásbusz--várólista létrehozása](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) fájlt.</span><span class="sxs-lookup"><span data-stu-id="a1edf-135">toouse an optional parameters file, copy hello [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) file.</span></span> <span data-ttu-id="a1edf-136">Cserélje le a hello értékének `serviceBusNamespaceName` hello Service Bus-névtér hello nevű szeretné, hogy toocreate ebben a telepítésben, és cserélje le a hello értékének `serviceBusQueueName` hello várólista hello nevű toocreate szeretné.</span><span class="sxs-lookup"><span data-stu-id="a1edf-136">Replace hello value of `serviceBusNamespaceName` with hello name of hello Service Bus namespace you want toocreate in this deployment, and replace hello value of `serviceBusQueueName` with hello name of hello queue you want toocreate.</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

<span data-ttu-id="a1edf-137">További információkért lásd: hello [paraméterek](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) témakör.</span><span class="sxs-lookup"><span data-stu-id="a1edf-137">For more information, see hello [Parameters](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) topic.</span></span>

### <a name="log-in-tooazure-and-set-hello-azure-subscription"></a><span data-ttu-id="a1edf-138">Jelentkezzen be tooAzure és hello Azure-előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="a1edf-138">Log in tooAzure and set hello Azure subscription</span></span>

<span data-ttu-id="a1edf-139">Egy PowerShell-parancssorba futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="a1edf-139">From a PowerShell prompt, run hello following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a1edf-140">Az Azure-fiók tooyour rákérdezéses toolog áll.</span><span class="sxs-lookup"><span data-stu-id="a1edf-140">You are prompted toolog on tooyour Azure account.</span></span> <span data-ttu-id="a1edf-141">A bejelentkezés után futtassa a következő parancs tooview hello az elérhető előfizetések.</span><span class="sxs-lookup"><span data-stu-id="a1edf-141">After logging on, run hello following command tooview your available subscriptions.</span></span>

```powershell
Get-AzureRMSubscription
```

<span data-ttu-id="a1edf-142">Ez a parancs elérhető Azure-előfizetések listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a1edf-142">This command returns a list of available Azure subscriptions.</span></span> <span data-ttu-id="a1edf-143">Válasszon egy előfizetést az aktuális munkamenet a hello hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="a1edf-143">Choose a subscription for hello current session by running hello following command.</span></span> <span data-ttu-id="a1edf-144">Cserélje le `<YourSubscriptionId>` , az Azure-előfizetés hello GUID hello toouse szeretne.</span><span class="sxs-lookup"><span data-stu-id="a1edf-144">Replace `<YourSubscriptionId>` with hello GUID for hello Azure subscription you want toouse.</span></span>

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-hello-resource-group"></a><span data-ttu-id="a1edf-145">Hello erőforrás csoport beállítása</span><span class="sxs-lookup"><span data-stu-id="a1edf-145">Set hello resource group</span></span>

<span data-ttu-id="a1edf-146">Ha nem rendelkezik meglévő erőforrás csoport, hozzon létre egy új erőforráscsoportot hello ** New-AzureRmResourceGroup ** parancsot.</span><span class="sxs-lookup"><span data-stu-id="a1edf-146">If you do not have an existing resource group, create a new resource group with hello **New-AzureRmResourceGroup ** command.</span></span> <span data-ttu-id="a1edf-147">Adja meg a hello erőforráscsoportot és helyet szeretne toouse hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a1edf-147">Provide hello name of hello resource group and location you want toouse.</span></span> <span data-ttu-id="a1edf-148">Példa:</span><span class="sxs-lookup"><span data-stu-id="a1edf-148">For example:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

<span data-ttu-id="a1edf-149">Ha sikeres, hello új erőforráscsoport összegzését jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a1edf-149">If successful, a summary of hello new resource group is displayed.</span></span>

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-hello-deployment"></a><span data-ttu-id="a1edf-150">Hello központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a1edf-150">Test hello deployment</span></span>

<span data-ttu-id="a1edf-151">A telepítés ellenőrzése hello futtatásával `Test-AzureRmResourceGroupDeployment` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a1edf-151">Validate your deployment by running hello `Test-AzureRmResourceGroupDeployment` cmdlet.</span></span> <span data-ttu-id="a1edf-152">Hello központi telepítés tesztelésekor meg paramétereket pontosan ugyanúgy hello központi telepítés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="a1edf-152">When testing hello deployment, provide parameters exactly as you would when executing hello deployment.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="create-hello-deployment"></a><span data-ttu-id="a1edf-153">Hello telepítésének létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1edf-153">Create hello deployment</span></span>

<span data-ttu-id="a1edf-154">toocreate hello új központi telepítés, futtatási hello `New-AzureRmResourceGroupDeployment` parancsmagot, és adja meg a hello szükséges paramétert, ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="a1edf-154">toocreate hello new deployment, run hello `New-AzureRmResourceGroupDeployment` cmdlet, and provide hello necessary parameters when prompted.</span></span> <span data-ttu-id="a1edf-155">hello paraméternek számít a nevet a telepítésnek, a erőforráscsoport, és hello elérési útja vagy URL-cím toohello sablonfájl hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a1edf-155">hello parameters include a name for your deployment, hello name of your resource group, and hello path or URL toohello template file.</span></span> <span data-ttu-id="a1edf-156">Ha hello **mód** paraméter nincs megadva, alapértelmezett értéke hello **növekményes** szolgál.</span><span class="sxs-lookup"><span data-stu-id="a1edf-156">If hello **Mode** parameter is not specified, hello default value of **Incremental** is used.</span></span> <span data-ttu-id="a1edf-157">További információkért lásd: [növekményes és teljes telepítések](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span><span class="sxs-lookup"><span data-stu-id="a1edf-157">For more information, see [Incremental and complete deployments](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).</span></span>

<span data-ttu-id="a1edf-158">hello következő parancssorok meg hello három paraméterek hello PowerShell-ablakban:</span><span class="sxs-lookup"><span data-stu-id="a1edf-158">hello following command prompts you for hello three parameters in hello PowerShell window:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

<span data-ttu-id="a1edf-159">Paraméterfájl toospecify inkább a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="a1edf-159">toospecify a parameters file instead, use hello following command.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -TemplateParameterFile <path tooparameters file>\azuredeploy.parameters.json
```

<span data-ttu-id="a1edf-160">Beágyazott paraméterek hello központi telepítési parancsmag futtatása esetén is használható.</span><span class="sxs-lookup"><span data-stu-id="a1edf-160">You can also use inline parameters when you run hello deployment cmdlet.</span></span> <span data-ttu-id="a1edf-161">hello parancs a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="a1edf-161">hello command is as follows:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json -parameterName "parameterValue"
```

<span data-ttu-id="a1edf-162">toorun egy [teljes](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) a központi telepítés, a set hello **mód** paraméter túl**Complete**:</span><span class="sxs-lookup"><span data-stu-id="a1edf-162">toorun a [complete](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) deployment, set hello **Mode** parameter too**Complete**:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path tootemplate file>\azuredeploy.json
```

### <a name="verify-hello-deployment"></a><span data-ttu-id="a1edf-163">Hello telepítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a1edf-163">Verify hello deployment</span></span>
<span data-ttu-id="a1edf-164">Ha hello erőforrások telepítése sikeres volt, hello telepítés összegzését hello PowerShell ablakban jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a1edf-164">If hello resources are deployed successfully, a summary of hello deployment is displayed in hello PowerShell window:</span></span>

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a><span data-ttu-id="a1edf-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1edf-165">Next steps</span></span>
<span data-ttu-id="a1edf-166">Most láthatta hello alapvető munkafolyamattal és parancsok telepítése Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="a1edf-166">You've now seen hello basic workflow and commands for deploying an Azure Resource Manager template.</span></span> <span data-ttu-id="a1edf-167">További részletes információkért látogasson el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="a1edf-167">For more detailed information, visit hello following links:</span></span>

* <span data-ttu-id="a1edf-168">[Az Azure Resource Manager áttekintése][Azure Resource Manager overview]</span><span class="sxs-lookup"><span data-stu-id="a1edf-168">[Azure Resource Manager overview][Azure Resource Manager overview]</span></span>
* <span data-ttu-id="a1edf-169">[Erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése][Deploy resources with Azure Resource Manager templates]</span><span class="sxs-lookup"><span data-stu-id="a1edf-169">[Deploy resources with Resource Manager templates and Azure PowerShell][Deploy resources with Azure Resource Manager templates]</span></span>
* [<span data-ttu-id="a1edf-170">Azure Resource Manager-sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="a1edf-170">Authoring Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
