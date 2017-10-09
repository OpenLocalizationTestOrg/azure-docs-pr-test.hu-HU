---
title: "aaaUse PowerShell toomanage Azure Event Hubs erőforrások |} Microsoft Docs"
description: "PowerShell modul toocreate használatát és kezelését az Event Hubs"
services: event-hubs
documentationcenter: .NET
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: d79cb307c2b4a031d059ce6ca67117ffc0b4600b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toomanage-event-hubs-resources"></a><span data-ttu-id="7d8a1-103">PowerShell toomanage Event Hubs-erőforrások használata</span><span class="sxs-lookup"><span data-stu-id="7d8a1-103">Use PowerShell toomanage Event Hubs resources</span></span>

<span data-ttu-id="7d8a1-104">A Microsoft Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és automatizálni hello telepítése és kezelése az Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-104">Microsoft Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of Azure services.</span></span> <span data-ttu-id="7d8a1-105">Ez a cikk ismerteti, hogyan toouse hello [Event Hubs Resource Manager PowerShell-modul](/powershell/module/azurerm.eventhub) tooprovision és kezelheti az Event Hubs entitások (a névterek, a különálló eseményközpontokhoz és a fogyasztói csoportok) helyi Azure PowerShell-konzollal vagy parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-105">This article describes how toouse hello [Event Hubs Resource Manager PowerShell module](/powershell/module/azurerm.eventhub) tooprovision and manage Event Hubs entities (namespaces, individual event hubs, and consumer groups) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="7d8a1-106">Az Event Hubs-erőforrások Azure Resource Manager-sablonok segítségével is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-106">You can also manage Event Hubs resources using Azure Resource Manager templates.</span></span> <span data-ttu-id="7d8a1-107">További információkért lásd: hello cikk [az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal event hub és fogyasztói csoport](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="7d8a1-107">For more information, see hello article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d8a1-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7d8a1-108">Prerequisites</span></span>

<span data-ttu-id="7d8a1-109">Mielőtt elkezdené, hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="7d8a1-109">Before you begin, you'll need hello following:</span></span>

* <span data-ttu-id="7d8a1-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-110">An Azure subscription.</span></span> <span data-ttu-id="7d8a1-111">Előfizetés beszerzésével kapcsolatos további információkért lásd: [beszerzési lehetőségek][purchase options], [ajánlatok][member offers], vagy [ingyenes fiókot][free account].</span><span class="sxs-lookup"><span data-stu-id="7d8a1-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="7d8a1-112">Az Azure PowerShell számítógép.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="7d8a1-113">Útmutatásért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="7d8a1-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="7d8a1-114">A PowerShell-parancsfájlok, a NuGet-csomagok és a .NET-keretrendszer hello általános ismertetése.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-114">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="7d8a1-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="7d8a1-115">Get started</span></span>

<span data-ttu-id="7d8a1-116">hello első lépése pedig toouse PowerShell toolog tooyour Azure-fiókot az Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-116">hello first step is toouse PowerShell toolog in tooyour Azure account and Azure subscription.</span></span> <span data-ttu-id="7d8a1-117">Hello utasításait követve [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps) toolog a tooyour Azure-fiókra, majd beolvasása és az Azure-előfizetéshez hello erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-117">Follow hello instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) toolog in tooyour Azure account, then retrieve and access hello resources in your Azure subscription.</span></span>

## <a name="provision-an-event-hubs-namespace"></a><span data-ttu-id="7d8a1-118">Az Event Hubs névtér kiépítése</span><span class="sxs-lookup"><span data-stu-id="7d8a1-118">Provision an Event Hubs namespace</span></span>

<span data-ttu-id="7d8a1-119">Az Event Hubs névterek használatakor hello használhatja [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace) , és [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-119">When working with Event Hubs namespaces, you can use hello [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), and [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.</span></span>

<span data-ttu-id="7d8a1-120">Ebben a példában néhány helyi változók hello parancsfájlban; hoz létre. `$Namespace` és `$Location`.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-120">This example creates a few local variables in hello script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="7d8a1-121">`$Namespace`hello Event Hubs névteret, amelyhez toowork szeretnénk hello neve van.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-121">`$Namespace` is hello name of hello Event Hubs namespace with which we want toowork.</span></span>
* <span data-ttu-id="7d8a1-122">`$Location`azonosítja a hello adatközpont, amelyen kiépíti a Microsoft hello névtér.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-122">`$Location` identifies hello data center in which will we provision hello namespace.</span></span>
* <span data-ttu-id="7d8a1-123">`$CurrentNamespace`hello hivatkozás névtér, amely azt lekérése (vagy hozzon létre) tárolja.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-123">`$CurrentNamespace` stores hello reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="7d8a1-124">Egy tényleges parancsfájlban `$Namespace` és `$Location` paraméterként kell.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="7d8a1-125">Ez a kijelző hello parancsfájl hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7d8a1-125">This part of hello script does hello following:</span></span>

1. <span data-ttu-id="7d8a1-126">Az Event Hubs névtér hello kísérletek tooretrieve adta meg.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-126">Attempts tooretrieve an Event Hubs namespace with hello specified name.</span></span>
2. <span data-ttu-id="7d8a1-127">Ha hello névtérben található, mi található jelenti.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-127">If hello namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="7d8a1-128">Ha hello névtér nem található, akkor hello névteret hoz létre, és majd beolvassa az újonnan létrehozott névtér hello.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-128">If hello namespace is not found, it creates hello namespace and then retrieves hello newly created namespace.</span></span>

    ```powershell
    # Query toosee if hello namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if hello namespace already exists or needs toobe created
    if ($CurrentNamespace)
    {
        Write-Host "hello namespace $Namespace already exists in hello $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "hello $Namespace namespace does not exist."
        Write-Host "Creating hello $Namespace namespace in hello $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "hello $Namespace namespace in Resource Group $ResGrpName in hello $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a><span data-ttu-id="7d8a1-129">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="7d8a1-129">Create an event hub</span></span>

<span data-ttu-id="7d8a1-130">az eseményközpontok toocreate ellenőriznie a névtér hello parancsfájllal hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-130">toocreate an event hub, perform a namespace check using hello script in hello previous section.</span></span> <span data-ttu-id="7d8a1-131">Ezután használja az hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) parancsmag toocreate hello eseményközpont:</span><span class="sxs-lookup"><span data-stu-id="7d8a1-131">Then, use hello [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet toocreate hello event hub:</span></span>

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "hello event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $EventHubName event hub does not exist."
    Write-Host "Creating hello $EventHubName event hub in hello $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $EventHubName event hub in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a><span data-ttu-id="7d8a1-132">Egy felhasználói csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="7d8a1-132">Create a consumer group</span></span>

<span data-ttu-id="7d8a1-133">egy fogyasztói csoporton belül az eseményközpontok toocreate hello névtér és az esemény hub ellenőrzések elvégzéséhez hello parancsfájlokkal hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-133">toocreate a consumer group within an event hub, perform hello namespace and event hub checks using hello scripts in hello previous section.</span></span> <span data-ttu-id="7d8a1-134">Ezután használja az hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) parancsmag toocreate hello fogyasztói csoporton belül hello eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-134">Then, use hello [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet toocreate hello consumer group within hello event hub.</span></span> <span data-ttu-id="7d8a1-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="7d8a1-135">For example:</span></span>

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "hello consumer group $ConsumerGroupName in event hub $EventHubName already exists in hello $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "hello $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating hello $ConsumerGroupName consumer group in hello $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "hello $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in hello $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a><span data-ttu-id="7d8a1-136">Készlet felhasználói metaadatok</span><span class="sxs-lookup"><span data-stu-id="7d8a1-136">Set user metadata</span></span>

<span data-ttu-id="7d8a1-137">Után hello parancsfájlok végrehajtása az előző szakaszokban hello, használhatja a hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) parancsmag tooupdate hello tulajdonságait egy felhasználói csoport, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="7d8a1-137">After executing hello scripts in hello preceding sections, you can use hello [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet tooupdate hello properties of a consumer group, as in hello following example:</span></span>

```powershell
# Set some user metadata on hello CG
Write-Host "Setting hello UserMetadata field too'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a><span data-ttu-id="7d8a1-138">Távolítsa el az event hubs</span><span class="sxs-lookup"><span data-stu-id="7d8a1-138">Remove event hub</span></span>

<span data-ttu-id="7d8a1-139">tooremove hello event hubs létrehozott hello használható `Remove-*` parancsmagok, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="7d8a1-139">tooremove hello event hubs you created, you can use hello `Remove-*` cmdlets, as in hello following example:</span></span>

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a><span data-ttu-id="7d8a1-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d8a1-140">Next steps</span></span>

- <span data-ttu-id="7d8a1-141">A dokumentációban hello teljes Event Hubs Resource Manager PowerShell modul [Itt](/powershell/module/azurerm.eventhub).</span><span class="sxs-lookup"><span data-stu-id="7d8a1-141">See hello complete Event Hubs Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.eventhub).</span></span> <span data-ttu-id="7d8a1-142">Ezen a lapon az összes elérhető parancsmagok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="7d8a1-142">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="7d8a1-143">Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: hello cikk [az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal event hub és fogyasztói csoport](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="7d8a1-143">For information about using Azure Resource Manager templates, see hello article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>
- <span data-ttu-id="7d8a1-144">Információ a [Event Hubs .NET kezelési kódtárakat](event-hubs-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="7d8a1-144">Information about [Event Hubs .NET management libraries](event-hubs-management-libraries.md).</span></span>

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
