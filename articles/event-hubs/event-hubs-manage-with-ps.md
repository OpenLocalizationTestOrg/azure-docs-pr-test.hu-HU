---
title: "Azure Event Hubs-erőforrások kezelése a PowerShell használatával |} Microsoft Docs"
description: "PowerShell-modul segítségével létrehozása és kezelése az Event Hubs"
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
ms.openlocfilehash: 2b49c01153b1104612e6ebf9c88566fc40d1f635
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-manage-event-hubs-resources"></a><span data-ttu-id="de705-103">Az Event Hubs-erőforrások kezelése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="de705-103">Use PowerShell to manage Event Hubs resources</span></span>

<span data-ttu-id="de705-104">A Microsoft Azure PowerShell egy parancsfájl-kezelési környezet, melyekkel automatizálhatja a központi telepítési és felügyeleti Azure-szolgáltatáshoz, és szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="de705-104">Microsoft Azure PowerShell is a scripting environment that you can use to control and automate the deployment and management of Azure services.</span></span> <span data-ttu-id="de705-105">Ez a cikk ismerteti, hogyan használható a [Event Hubs Resource Manager PowerShell-modul](/powershell/module/azurerm.eventhub) szeretnék telepíteni és kezelni az Event Hubs entitások (a névterek, a különálló eseményközpontokhoz és a fogyasztói csoportok) a helyi Azure PowerShell-konzolt vagy parancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="de705-105">This article describes how to use the [Event Hubs Resource Manager PowerShell module](/powershell/module/azurerm.eventhub) to provision and manage Event Hubs entities (namespaces, individual event hubs, and consumer groups) using a local Azure PowerShell console or script.</span></span>

<span data-ttu-id="de705-106">Az Event Hubs-erőforrások Azure Resource Manager-sablonok segítségével is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="de705-106">You can also manage Event Hubs resources using Azure Resource Manager templates.</span></span> <span data-ttu-id="de705-107">További információkért lásd: a cikk [az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal event hub és fogyasztói csoport](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="de705-107">For more information, see the article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de705-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de705-108">Prerequisites</span></span>

<span data-ttu-id="de705-109">Mielőtt elkezdené, a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="de705-109">Before you begin, you'll need the following:</span></span>

* <span data-ttu-id="de705-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="de705-110">An Azure subscription.</span></span> <span data-ttu-id="de705-111">Előfizetés beszerzésével kapcsolatos további információkért lásd: [beszerzési lehetőségek][purchase options], [ajánlatok][member offers], vagy [ingyenes fiókot][free account].</span><span class="sxs-lookup"><span data-stu-id="de705-111">For more information about obtaining a subscription, see [purchase options][purchase options], [member offers][member offers], or [free account][free account].</span></span>
* <span data-ttu-id="de705-112">Az Azure PowerShell számítógép.</span><span class="sxs-lookup"><span data-stu-id="de705-112">A computer with Azure PowerShell.</span></span> <span data-ttu-id="de705-113">Útmutatásért lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="de705-113">For instructions, see [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps).</span></span>
* <span data-ttu-id="de705-114">A PowerShell-parancsfájlok, NuGet-csomagok és a .NET-keretrendszer általános ismertetése.</span><span class="sxs-lookup"><span data-stu-id="de705-114">A general understanding of PowerShell scripts, NuGet packages, and the .NET Framework.</span></span>

## <a name="get-started"></a><span data-ttu-id="de705-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="de705-115">Get started</span></span>

<span data-ttu-id="de705-116">Az első lépés-e jelentkezni az Azure-fiókot és az Azure-előfizetés a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="de705-116">The first step is to use PowerShell to log in to your Azure account and Azure subscription.</span></span> <span data-ttu-id="de705-117">Kövesse az utasításokat a [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/get-started-azureps) próbál bejelentkezni az Azure-fiókjával, majd beolvasása és hozzáférés az erőforrásokhoz az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="de705-117">Follow the instructions in [Get started with Azure PowerShell cmdlets](/powershell/azure/get-started-azureps) to log in to your Azure account, then retrieve and access the resources in your Azure subscription.</span></span>

## <a name="provision-an-event-hubs-namespace"></a><span data-ttu-id="de705-118">Az Event Hubs névtér kiépítése</span><span class="sxs-lookup"><span data-stu-id="de705-118">Provision an Event Hubs namespace</span></span>

<span data-ttu-id="de705-119">Az Event Hubs névterek használatakor is használhatja a [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), és [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="de705-119">When working with Event Hubs namespaces, you can use the [Get-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/get-azurermeventhubnamespace), [New-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/new-azurermeventhubnamespace), [Remove-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/remove-azurermeventhubnamespace), and [Set-AzureRmEventHubNamespace](/powershell/module/azurerm.eventhub/set-azurermeventhubnamespace) cmdlets.</span></span>

<span data-ttu-id="de705-120">Ez a példa néhány helyi változók hoz létre a parancsfájl; `$Namespace` és `$Location`.</span><span class="sxs-lookup"><span data-stu-id="de705-120">This example creates a few local variables in the script; `$Namespace` and `$Location`.</span></span>

* <span data-ttu-id="de705-121">`$Namespace`a használt működéséhez szeretnénk az Event Hubs névtér esetén.</span><span class="sxs-lookup"><span data-stu-id="de705-121">`$Namespace` is the name of the Event Hubs namespace with which we want to work.</span></span>
* <span data-ttu-id="de705-122">`$Location`Az Adatközpont, amelyen azonosítja kiépíti azt a névteret.</span><span class="sxs-lookup"><span data-stu-id="de705-122">`$Location` identifies the data center in which will we provision the namespace.</span></span>
* <span data-ttu-id="de705-123">`$CurrentNamespace`a referencia-névtér, amely azt lekérése (vagy hozzon létre) tárolja.</span><span class="sxs-lookup"><span data-stu-id="de705-123">`$CurrentNamespace` stores the reference namespace that we retrieve (or create).</span></span>

<span data-ttu-id="de705-124">Egy tényleges parancsfájlban `$Namespace` és `$Location` paraméterként kell.</span><span class="sxs-lookup"><span data-stu-id="de705-124">In an actual script, `$Namespace` and `$Location` can be passed as parameters.</span></span>

<span data-ttu-id="de705-125">Ez a kijelző, a parancsfájl a következőket teszi:</span><span class="sxs-lookup"><span data-stu-id="de705-125">This part of the script does the following:</span></span>

1. <span data-ttu-id="de705-126">Megkísérli lekérni az Event Hubs névtér a megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="de705-126">Attempts to retrieve an Event Hubs namespace with the specified name.</span></span>
2. <span data-ttu-id="de705-127">Ha a névtérben található, mi található jelenti.</span><span class="sxs-lookup"><span data-stu-id="de705-127">If the namespace is found, it reports what was found.</span></span>
3. <span data-ttu-id="de705-128">Ha a névtér nem található, akkor hoz létre a névteret, és az újonnan létrehozott névtér ezután lekéri.</span><span class="sxs-lookup"><span data-stu-id="de705-128">If the namespace is not found, it creates the namespace and then retrieves the newly created namespace.</span></span>

    ```powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
    }
    ```

## <a name="create-an-event-hub"></a><span data-ttu-id="de705-129">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="de705-129">Create an event hub</span></span>

<span data-ttu-id="de705-130">Az eseményközpontok létrehozásához hajtsa végre a névtér-ellenőrzés az előző szakaszban a parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="de705-130">To create an event hub, perform a namespace check using the script in the previous section.</span></span> <span data-ttu-id="de705-131">Ezt követően a [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) parancsmaggal hozhat létre az event hubs:</span><span class="sxs-lookup"><span data-stu-id="de705-131">Then, use the [New-AzureRmEventHub](/powershell/module/azurerm.eventhub/new-azurermeventhub) cmdlet to create the event hub:</span></span>

```powershell
# Check if event hub already exists
$CurrentEH = Get-AzureRMEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName

if($CurrentEH)
{
    Write-Host "The event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $EventHubName event hub does not exist."
    Write-Host "Creating the $EventHubName event hub in the $Location region..."
    New-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -Location $Location -MessageRetentionInDays 3
    $CurrentEH = Get-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $EventHubName event hub in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="create-a-consumer-group"></a><span data-ttu-id="de705-132">Egy felhasználói csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="de705-132">Create a consumer group</span></span>

<span data-ttu-id="de705-133">Az egy fogyasztói csoporton belül az eseményközpontok létrehozásához hajtsa végre a névtér és az event hub a parancsfájlok segítségével az előző szakaszban ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="de705-133">To create a consumer group within an event hub, perform the namespace and event hub checks using the scripts in the previous section.</span></span> <span data-ttu-id="de705-134">Ezt követően a [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) parancsmaggal hozhat létre a fogyasztói csoporton belül az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="de705-134">Then, use the [New-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/new-azurermeventhubconsumergroup) cmdlet to create the consumer group within the event hub.</span></span> <span data-ttu-id="de705-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="de705-135">For example:</span></span>

```powershell
# Check if consumer group already exists
$CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -ErrorAction Ignore

if($CurrentCG)
{
    Write-Host "The consumer group $ConsumerGroupName in event hub $EventHubName already exists in the $Location region:"
    # Report what was found
    Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
}
else
{
    Write-Host "The $ConsumerGroupName consumer group does not exist."
    Write-Host "Creating the $ConsumerGroupName consumer group in the $Location region..."
    New-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
    $CurrentCG = Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
    Write-Host "The $ConsumerGroupName consumer group in event hub $EventHubName in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

#### <a name="set-user-metadata"></a><span data-ttu-id="de705-136">Készlet felhasználói metaadatok</span><span class="sxs-lookup"><span data-stu-id="de705-136">Set user metadata</span></span>

<span data-ttu-id="de705-137">A parancsfájlok végrehajtása az előző szakaszokban, után is használhatja a [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) parancsmagot, hogy egy felhasználói csoport, az alábbi példában látható módon tulajdonságainak frissítéséhez:</span><span class="sxs-lookup"><span data-stu-id="de705-137">After executing the scripts in the preceding sections, you can use the [Set-AzureRmEventHubConsumerGroup](/powershell/module/azurerm.eventhub/set-azurermeventhubconsumergroup) cmdlet to update the properties of a consumer group, as in the following example:</span></span>

```powershell
# Set some user metadata on the CG
Write-Host "Setting the UserMetadata field to 'Testing'"
Set-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName -UserMetadata "Testing"
# Show result
Get-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
```

## <a name="remove-event-hub"></a><span data-ttu-id="de705-138">Távolítsa el az event hubs</span><span class="sxs-lookup"><span data-stu-id="de705-138">Remove event hub</span></span>

<span data-ttu-id="de705-139">Az event hubs létrehozott eltávolításához használja a `Remove-*` parancsmagok, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="de705-139">To remove the event hubs you created, you can use the `Remove-*` cmdlets, as in the following example:</span></span>

```powershell
# Clean up
Remove-AzureRmEventHubConsumerGroup -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName -ConsumerGroupName $ConsumerGroupName
Remove-AzureRmEventHub -ResourceGroupName $ResGrpName -NamespaceName $Namespace -EventHubName $EventHubName
Remove-AzureRmEventHubNamespace -ResourceGroupName $ResGrpName -NamespaceName $Namespace
```

## <a name="next-steps"></a><span data-ttu-id="de705-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de705-140">Next steps</span></span>

- <span data-ttu-id="de705-141">A teljes Event Hubs Resource Manager PowerShell modul dokumentációjában [Itt](/powershell/module/azurerm.eventhub).</span><span class="sxs-lookup"><span data-stu-id="de705-141">See the complete Event Hubs Resource Manager PowerShell module documentation [here](/powershell/module/azurerm.eventhub).</span></span> <span data-ttu-id="de705-142">Ezen a lapon az összes elérhető parancsmagok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="de705-142">This page lists all available cmdlets.</span></span>
- <span data-ttu-id="de705-143">Azure Resource Manager-sablonok használatával kapcsolatos információkért lásd: a cikk [az Event Hubs-névtér létrehozása Azure Resource Manager-sablonnal event hub és fogyasztói csoport](event-hubs-resource-manager-namespace-event-hub.md).</span><span class="sxs-lookup"><span data-stu-id="de705-143">For information about using Azure Resource Manager templates, see the article [Create an Event Hubs namespace with event hub and consumer group using an Azure Resource Manager template](event-hubs-resource-manager-namespace-event-hub.md).</span></span>
- <span data-ttu-id="de705-144">Információ a [Event Hubs .NET kezelési kódtárakat](event-hubs-management-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="de705-144">Information about [Event Hubs .NET management libraries](event-hubs-management-libraries.md).</span></span>

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
