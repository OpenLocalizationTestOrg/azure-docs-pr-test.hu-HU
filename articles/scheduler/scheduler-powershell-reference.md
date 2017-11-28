---
title: "Feladatütemező – PowerShell-parancsmagok referencia"
description: "Feladatütemező – PowerShell-parancsmagok referencia"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 141919ab4506b3de4c4a69670dcf54c60ee6409c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-powershell-cmdlets-reference"></a><span data-ttu-id="e7196-103">Feladatütemező – PowerShell-parancsmagok referencia</span><span class="sxs-lookup"><span data-stu-id="e7196-103">Scheduler PowerShell Cmdlets Reference</span></span>
<span data-ttu-id="e7196-104">A következő táblázat ismerteti, és hivatkozásokat tartalmaz a fő parancsmagok Azure Scheduler a hivatkozás lapján.</span><span class="sxs-lookup"><span data-stu-id="e7196-104">The following table describes and links to the reference page of each of the major cmdlets in Azure Scheduler.</span></span>

<span data-ttu-id="e7196-105">Az Azure PowerShell telepítésérről és az Azure-előfizetéssel való társításáról további információt [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című cikkben találhat.</span><span class="sxs-lookup"><span data-stu-id="e7196-105">To install Azure PowerShell and associate it with your Azure subscription, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

<span data-ttu-id="e7196-106">További információ [Azure Resource Manager parancsmagjainak](/powershell/azure/overview), lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="e7196-106">For more information about [Azure Resource Manager cmdlets](/powershell/azure/overview), see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

| <span data-ttu-id="e7196-107">Parancsmag</span><span class="sxs-lookup"><span data-stu-id="e7196-107">Cmdlet</span></span> | <span data-ttu-id="e7196-108">A parancsmag leírása</span><span class="sxs-lookup"><span data-stu-id="e7196-108">Cmdlet Description</span></span> |
| --- | --- |
| [<span data-ttu-id="e7196-109">Disable-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="e7196-109">Disable-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |<span data-ttu-id="e7196-110">A feladatgyűjtemény letiltása.</span><span class="sxs-lookup"><span data-stu-id="e7196-110">Disables a job collection.</span></span> |
| [<span data-ttu-id="e7196-111">Enable-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="e7196-111">Enable-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |<span data-ttu-id="e7196-112">Lehetővé teszi, hogy a feladatgyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="e7196-112">Enables a job collection.</span></span> |
| [<span data-ttu-id="e7196-113">Get-AzureRmSchedulerJob</span><span class="sxs-lookup"><span data-stu-id="e7196-113">Get-AzureRmSchedulerJob</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |<span data-ttu-id="e7196-114">A Feladatütemező szolgáltatás lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="e7196-114">Gets Scheduler jobs.</span></span> |
| [<span data-ttu-id="e7196-115">Get-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="e7196-115">Get-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |<span data-ttu-id="e7196-116">Lekérdezi a gyűjtemények feladat.</span><span class="sxs-lookup"><span data-stu-id="e7196-116">Gets job collections.</span></span> |
| [<span data-ttu-id="e7196-117">Get-AzureRmSchedulerJobHistory</span><span class="sxs-lookup"><span data-stu-id="e7196-117">Get-AzureRmSchedulerJobHistory</span></span>](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |<span data-ttu-id="e7196-118">Lekérdezi a feladatelőzmények.</span><span class="sxs-lookup"><span data-stu-id="e7196-118">Gets job history.</span></span> |
| [<span data-ttu-id="e7196-119">Új AzureRmSchedulerHttpJob</span><span class="sxs-lookup"><span data-stu-id="e7196-119">New-AzureRmSchedulerHttpJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |<span data-ttu-id="e7196-120">Létrehoz egy HTTP-feladatot.</span><span class="sxs-lookup"><span data-stu-id="e7196-120">Creates an HTTP job.</span></span> |
| [<span data-ttu-id="e7196-121">Új AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="e7196-121">New-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |<span data-ttu-id="e7196-122">A feladatgyűjtemény hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e7196-122">Creates a job collection.</span></span> |
| [<span data-ttu-id="e7196-123">Új AzureRmSchedulerServiceBusQueueJob</span><span class="sxs-lookup"><span data-stu-id="e7196-123">New-AzureRmSchedulerServiceBusQueueJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |<span data-ttu-id="e7196-124">Létrehoz egy service bus várólista feladatot.</span><span class="sxs-lookup"><span data-stu-id="e7196-124">Creates a service bus queue job.</span></span> |
| [<span data-ttu-id="e7196-125">Új AzureRmSchedulerServiceBusTopicJob</span><span class="sxs-lookup"><span data-stu-id="e7196-125">New-AzureRmSchedulerServiceBusTopicJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |<span data-ttu-id="e7196-126">Létrehoz egy service bus témakör feladatot.</span><span class="sxs-lookup"><span data-stu-id="e7196-126">Creates a service bus topic job.</span></span> |
| [<span data-ttu-id="e7196-127">Új AzureRmSchedulerStorageQueueJob</span><span class="sxs-lookup"><span data-stu-id="e7196-127">New-AzureRmSchedulerStorageQueueJob</span></span>](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |<span data-ttu-id="e7196-128">Létrehoz egy tároló várólista feladatot.</span><span class="sxs-lookup"><span data-stu-id="e7196-128">Creates a storage queue job.</span></span> |
| [<span data-ttu-id="e7196-129">Remove-AzureRmSchedulerJob</span><span class="sxs-lookup"><span data-stu-id="e7196-129">Remove-AzureRmSchedulerJob</span></span>](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |<span data-ttu-id="e7196-130">Eltávolítja a Scheduler feladatot.</span><span class="sxs-lookup"><span data-stu-id="e7196-130">Removes a Scheduler job.</span></span> |
| [<span data-ttu-id="e7196-131">Remove-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="e7196-131">Remove-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |<span data-ttu-id="e7196-132">A feladatgyűjtemény eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="e7196-132">Removes a job collection.</span></span> |
| [<span data-ttu-id="e7196-133">Set-AzureRmSchedulerHttpJob</span><span class="sxs-lookup"><span data-stu-id="e7196-133">Set-AzureRmSchedulerHttpJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |<span data-ttu-id="e7196-134">A Feladatütemező HTTP feladat módosítása.</span><span class="sxs-lookup"><span data-stu-id="e7196-134">Modifies a Scheduler HTTP job.</span></span> |
| [<span data-ttu-id="e7196-135">Set-AzureRmSchedulerJobCollection</span><span class="sxs-lookup"><span data-stu-id="e7196-135">Set-AzureRmSchedulerJobCollection</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |<span data-ttu-id="e7196-136">A feladatgyűjtemény módosítja.</span><span class="sxs-lookup"><span data-stu-id="e7196-136">Modifies a job collection.</span></span> |
| [<span data-ttu-id="e7196-137">Set-AzureRmSchedulerServiceBusQueueJob</span><span class="sxs-lookup"><span data-stu-id="e7196-137">Set-AzureRmSchedulerServiceBusQueueJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |<span data-ttu-id="e7196-138">Módosítja egy service bus várólista feladat.</span><span class="sxs-lookup"><span data-stu-id="e7196-138">Modifies a service bus queue job.</span></span> |
| [<span data-ttu-id="e7196-139">Set-AzureRmSchedulerServiceBusTopicJob</span><span class="sxs-lookup"><span data-stu-id="e7196-139">Set-AzureRmSchedulerServiceBusTopicJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |<span data-ttu-id="e7196-140">Módosítja egy service bus témakör feladat.</span><span class="sxs-lookup"><span data-stu-id="e7196-140">Modifies a service bus topic job.</span></span> |
| [<span data-ttu-id="e7196-141">Set-AzureRmSchedulerStorageQueueJob</span><span class="sxs-lookup"><span data-stu-id="e7196-141">Set-AzureRmSchedulerStorageQueueJob</span></span>](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |<span data-ttu-id="e7196-142">Módosítja egy sor feladat.</span><span class="sxs-lookup"><span data-stu-id="e7196-142">Modifies a storage queue job.</span></span> |

<span data-ttu-id="e7196-143">További részletes információt a következő parancsmagokat futtathatja:</span><span class="sxs-lookup"><span data-stu-id="e7196-143">For more detailed information, you can run any of the following cmdlets:</span></span> 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a><span data-ttu-id="e7196-144">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e7196-144">See Also</span></span>
 [<span data-ttu-id="e7196-145">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="e7196-145">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="e7196-146">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="e7196-146">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="e7196-147">Ismerkedés a Scheduler szolgáltatás Azure Portalon való használatával</span><span class="sxs-lookup"><span data-stu-id="e7196-147">Get started using Scheduler in the Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="e7196-148">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="e7196-148">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="e7196-149">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="e7196-149">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="e7196-150">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="e7196-150">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="e7196-151">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="e7196-151">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="e7196-152">Kimenő hitelesítés az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="e7196-152">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

