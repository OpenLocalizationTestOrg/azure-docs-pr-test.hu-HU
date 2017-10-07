---
title: "aaaUse PowerShell toomanage Azure Service Bus erőforrásainak |} Microsoft Docs"
description: "PowerShell modul toocreate használja, és a Service Bus-erőforrások kezelése"
services: service-bus-messaging
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/06/2017
ms.author: sethm
ms.openlocfilehash: 737044def913c5798e7e05fc4f1aeece76c8f4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-service-bus-resources"></a><span data-ttu-id="cbb43-103">PowerShell toomanage Service Bus erőforrásainak használata</span><span class="sxs-lookup"><span data-stu-id="cbb43-103">Use PowerShell toomanage Service Bus resources</span></span>

<span data-ttu-id="cbb43-104">A Microsoft Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és automatizálni hello telepítése és kezelése az Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="cbb43-104">Microsoft Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of Azure services.</span></span> <span data-ttu-id="cbb43-105">Ez a cikk ismerteti, hogyan toouse hello [Service Bus Resource Manager PowerShell-modul](/powershell/module/azurerm.servicebus) tooprovision és kezelheti a Service Bus-entitások (névterek, üzenetsorok, témakörök és előfizetések) helyi Azure PowerShell-konzollal vagy parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="cbb43-105">This article describes how toouse hello [Service Bus Resource Manager PowerShell module](/powershell/module/azurerm.servicebus) tooprovision and manage Service Bus entities (namespaces, queues, topics, and subscriptions) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="cbb43-106">Service Bus-entitások Azure Resource Manager-sablonok segítségével is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="cbb43-106">You can also manage Service Bus entities using Azure Resource Manager templates.</span></span> <span data-ttu-id="cbb43-107">További információkért lásd: hello cikk [Azure Resource Manager-sablonok létrehozása a Service Bus-erőforrások](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cbb43-107">For more information, see hello article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbb43-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cbb43-108">Prerequisites</span></span>

<span data-ttu-id="cbb43-109">Mielőtt elkezdené, hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="cbb43-109">Before you begin, you'll need hello following:</span></span>

* <span data-ttu-id="cbb43-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="cbb43-110">An Azure subscription.</span></span> <span data-ttu-id="cbb43-111">Előfizetés beszerzésével kapcsolatos további információkért lásd: [beszerzési lehetőségek][purchase options], [ajánlatok][member offers], vagy [ingyenes fiókot][free account].</span><span class="sxs-lookup"><span data-stu-id="cbb43-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="cbb43-112">Az Azure PowerShell számítógép.</span><span class="sxs-lookup"><span data-stu-id="cbb43-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="cbb43-113">Útmutatásért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="cbb43-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="cbb43-114">A PowerShell-parancsfájlok, a NuGet-csomagok és a .NET-keretrendszer hello általános ismertetése.</span><span class="sxs-lookup"><span data-stu-id="cbb43-114">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="cbb43-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cbb43-115">Get started</span></span>

<span data-ttu-id="cbb43-116">hello első lépése pedig toouse PowerShell toolog tooyour Azure-fiókot az Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="cbb43-116">hello first step is toouse PowerShell toolog in tooyour Azure account and Azure subscription.</span></span> <span data-ttu-id="cbb43-117">Hello utasításait követve [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps) toolog tooyour Azure-fiókra, és olvassák be és hozzáférés az Azure-előfizetéshez hello erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="cbb43-117">Follow hello instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) toolog in tooyour Azure account, and retrieve and access hello resources in your Azure subscription.</span></span>

## <a name="provision-a-service-bus-namespace"></a><span data-ttu-id="cbb43-118">A Service Bus-névtér kiépítése</span><span class="sxs-lookup"><span data-stu-id="cbb43-118">Provision a Service Bus namespace</span></span>

<span data-ttu-id="cbb43-119">A Szolgáltatásbusz-névterek használatakor hello használhatja [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), és [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="cbb43-119">When working with Service Bus namespaces, you can use hello [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), and [Set-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlets.</span></span>

<span data-ttu-id="cbb43-120">Ebben a példában néhány helyi változók hello parancsfájlban; hoz létre. `$Namespace` és `$Location`.</span><span class="sxs-lookup"><span data-stu-id="cbb43-120">This example creates a few local variables in hello script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="cbb43-121">`$Namespace`hello nevét, amelyhez toowork szeretnénk hello Service Bus-névtér van.</span><span class="sxs-lookup"><span data-stu-id="cbb43-121">`$Namespace` is hello name of hello Service Bus namespace with which we want toowork.</span></span>
* <span data-ttu-id="cbb43-122">`$Location`azonosítja a hello adatközpont, amelyen kiépíti a Microsoft hello névtér.</span><span class="sxs-lookup"><span data-stu-id="cbb43-122">`$Location` identifies hello data center in which will we provision hello namespace.</span></span>
* <span data-ttu-id="cbb43-123">`$CurrentNamespace`hello hivatkozás névtér, amely azt lekérése (vagy hozzon létre) tárolja.</span><span class="sxs-lookup"><span data-stu-id="cbb43-123">`$CurrentNamespace` stores hello reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="cbb43-124">Egy tényleges parancsfájlban `$Namespace` és `$Location` paraméterként kell.</span><span class="sxs-lookup"><span data-stu-id="cbb43-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="cbb43-125">Ez a kijelző hello parancsfájl hello a következő:</span><span class="sxs-lookup"><span data-stu-id="cbb43-125">This part of hello script does hello following:</span></span>

1. <span data-ttu-id="cbb43-126">Kísérletek tooretrieve hello a Service Bus-névtér adta meg.</span><span class="sxs-lookup"><span data-stu-id="cbb43-126">Attempts tooretrieve a Service Bus namespace with hello specified name.</span></span>
2. <span data-ttu-id="cbb43-127">Ha hello névtérben található, mi található jelenti.</span><span class="sxs-lookup"><span data-stu-id="cbb43-127">If hello namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="cbb43-128">Ha hello névtér nem található, akkor hello névteret hoz létre, és majd beolvassa az újonnan létrehozott névtér hello.</span><span class="sxs-lookup"><span data-stu-id="cbb43-128">If hello namespace is not found, it creates hello namespace and then retrieves hello newly created namespace.</span></span>
   
    ``` powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a><span data-ttu-id="cbb43-129">Egy névtér engedélyezési szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbb43-129">Create a namespace authorization rule</span></span>

<span data-ttu-id="cbb43-130">hello következő példa bemutatja, hogyan toomanage névtér engedélyezési szabályok segítségével hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), és [Remove-AzureRmServiceBusNamespaceAuthorizationRule parancsmagok](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span><span class="sxs-lookup"><span data-stu-id="cbb43-130">hello following example shows how toomanage namespace authorization rules using hello [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), and [Remove-AzureRmServiceBusNamespaceAuthorizationRule cmdlets](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).</span></span>

```powershell
# Query toosee if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if hello rule already exists or needs toobe created
if ($CurrentRule)
{
    Write-Host "hello $AuthRule rule already exists for hello namespace $Namespace."
}
else
{
    Write-Host "hello $AuthRule rule does not exist."
    Write-Host "Creating hello $AuthRule rule for hello $Namespace namespace..."
    New-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "hello $AuthRule rule for hello $Namespace namespace has been successfully created."

    Write-Host "Setting rights on hello namespace"
    $authRuleObj = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights toohello namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusNamespaceKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
}
```

## <a name="create-a-queue"></a><span data-ttu-id="cbb43-131">Üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbb43-131">Create a queue</span></span>

<span data-ttu-id="cbb43-132">egy várólista toocreate vagy témakör ellenőriznie a névtér hello parancsfájllal hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cbb43-132">toocreate a queue or topic, perform a namespace check using hello script in hello previous section.</span></span> <span data-ttu-id="cbb43-133">Ezután hozzon létre hello várólista:</span><span class="sxs-lookup"><span data-stu-id="cbb43-133">Then, create hello queue:</span></span>

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "hello queue $QueueName already exists in hello $Location region:"
}
else
{
    Write-Host "hello $QueueName queue does not exist."
    Write-Host "Creating hello $QueueName queue in hello $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "hello $QueueName queue in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a><span data-ttu-id="cbb43-134">Várólista-tulajdonságok módosítása</span><span class="sxs-lookup"><span data-stu-id="cbb43-134">Modify queue properties</span></span>

<span data-ttu-id="cbb43-135">Után hello-parancsprogram végrehajtása az előző szakaszban hello, használhatja a hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) parancsmag tooupdate hello tulajdonságok várólista, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="cbb43-135">After executing hello script in hello preceding section, you can use hello [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) cmdlet tooupdate hello properties of a queue, as in hello following example:</span></span>

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a><span data-ttu-id="cbb43-136">Kiépítés más Service Bus-entitások</span><span class="sxs-lookup"><span data-stu-id="cbb43-136">Provisioning other Service Bus entities</span></span>

<span data-ttu-id="cbb43-137">Használhatja a hello [Service Bus PowerShell modul](/powershell/module/azurerm.servicebus) tooprovision egyéb entitások, például az üzenettémák és előfizetések.</span><span class="sxs-lookup"><span data-stu-id="cbb43-137">You can use hello [Service Bus PowerShell module](/powershell/module/azurerm.servicebus) tooprovision other entities, such as topics and subscriptions.</span></span> <span data-ttu-id="cbb43-138">Ezek a parancsmagok szintaktikailag hasonló toohello várólista létrehozása parancsmagok hello előző szakaszban bemutatott.</span><span class="sxs-lookup"><span data-stu-id="cbb43-138">These cmdlets are syntactically similar toohello queue creation cmdlets demonstrated in hello previous section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbb43-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbb43-139">Next steps</span></span>

- <span data-ttu-id="cbb43-140">A dokumentációban hello teljes Service Bus Resource Manager PowerShell modul [Itt](/powershell/module/azurerm.servicebus).</span><span class="sxs-lookup"><span data-stu-id="cbb43-140">See hello complete Service Bus Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.servicebus).</span></span> <span data-ttu-id="cbb43-141">Ezen a lapon az összes elérhető parancsmagok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="cbb43-141">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="cbb43-142">Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: hello cikk [Azure Resource Manager-sablonok létrehozása a Service Bus-erőforrások](service-bus-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cbb43-142">For information about using Azure Resource Manager templates, see hello article [Create Service Bus resources using Azure Resource Manager templates](service-bus-resource-manager-overview.md).</span></span>
- <span data-ttu-id="cbb43-143">Információ a [Service Bus .NET kezelési kódtárakat](service-bus-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="cbb43-143">Information about [Service Bus .NET management libraries](service-bus-management-libraries.md).</span></span>

<span data-ttu-id="cbb43-144">Van néhány alternatív módszert toomanage Service Bus-entitások, ezek a blogbejegyzések leírtak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbb43-144">There are some alternate ways toomanage Service Bus entities, as described in these blog posts:</span></span>

* [<span data-ttu-id="cbb43-145">Hogyan toocreate Service Bus üzenetsorok, témakörök és előfizetések egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="cbb43-145">How toocreate Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="cbb43-146">Hogyan toocreate a Service Bus Namespace, és az Event Hubs egy PowerShell-parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="cbb43-146">How toocreate a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [<span data-ttu-id="cbb43-147">Service Bus PowerShell parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="cbb43-147">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
