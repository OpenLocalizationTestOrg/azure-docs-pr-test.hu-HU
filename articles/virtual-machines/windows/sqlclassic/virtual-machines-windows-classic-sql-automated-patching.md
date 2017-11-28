---
title: "aaaAutomated javítás az SQL Server VMs (klasszikus) |} Microsoft Docs"
description: "Ismerteti, az SQL Server rendszeren futó virtuális gépek az Azure-ban hello klasszikus üzembe helyezési mód hello automatikus javítás funkció."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 737b2f65-08b9-4f54-b867-e987730265a8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 2ef06b95705fc457605d6eb2fbc0afd490321843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a><span data-ttu-id="7d430-103">Automatikus javítás az SQL Server Azure virtuális gépekben (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="7d430-103">Automated Patching for SQL Server in Azure Virtual Machines (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7d430-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7d430-104">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="7d430-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="7d430-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="7d430-106">Automatikus javítás karbantartási időszak az az Azure virtuális gép fut az SQL Server hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7d430-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="7d430-107">Az automatikus frissítések csak a karbantartási időszak alatt lesznek telepítve.</span><span class="sxs-lookup"><span data-stu-id="7d430-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="7d430-108">Az SQL Server Ez biztosítja, hogy a rendszerfrissítések és minden kapcsolódó újraindítások halasztja hello lehetséges érdemes hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7d430-108">For SQL Server, this ensures that system updates and any associated restarts occur at hello best possible time for hello database.</span></span> <span data-ttu-id="7d430-109">Automatikus javítás függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="7d430-109">Automated Patching depends on hello [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="7d430-110">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7d430-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7d430-111">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="7d430-111">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="7d430-112">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="7d430-112">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="7d430-113">tooview hello erőforrás-kezelő változata ebből a cikkből: [automatikus javítás az SQL Server az Azure virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="7d430-113">tooview hello Resource Manager version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Resource Manager](../sql/virtual-machines-windows-sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d430-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7d430-114">Prerequisites</span></span>
<span data-ttu-id="7d430-115">toouse automatikus javítás, fontolja meg a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="7d430-115">toouse Automated Patching, consider hello following prerequisites:</span></span>

<span data-ttu-id="7d430-116">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="7d430-116">**Operating System**:</span></span>

* <span data-ttu-id="7d430-117">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="7d430-117">Windows Server 2012</span></span>
* <span data-ttu-id="7d430-118">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="7d430-118">Windows Server 2012 R2</span></span>
* <span data-ttu-id="7d430-119">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="7d430-119">Windows Server 2016</span></span>

<span data-ttu-id="7d430-120">**SQL Server-verzió**:</span><span class="sxs-lookup"><span data-stu-id="7d430-120">**SQL Server version**:</span></span>

* <span data-ttu-id="7d430-121">SQL Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="7d430-121">SQL Server 2012</span></span>
* <span data-ttu-id="7d430-122">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="7d430-122">SQL Server 2014</span></span>
* <span data-ttu-id="7d430-123">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="7d430-123">SQL Server 2016</span></span>

<span data-ttu-id="7d430-124">**Az Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="7d430-124">**Azure PowerShell**:</span></span>

* <span data-ttu-id="7d430-125">[Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7d430-125">[Install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="7d430-126">**SQL Server IaaS bővítmény**:</span><span class="sxs-lookup"><span data-stu-id="7d430-126">**SQL Server IaaS Extension**:</span></span>

* <span data-ttu-id="7d430-127">[Telepítse az SQL Server IaaS bővítmény hello](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="7d430-127">[Install hello SQL Server IaaS Extension](../classic/sql-server-agent-extension.md).</span></span>

## <a name="settings"></a><span data-ttu-id="7d430-128">Beállítások</span><span class="sxs-lookup"><span data-stu-id="7d430-128">Settings</span></span>
<span data-ttu-id="7d430-129">hello következő táblázatban hello-beállítások, amelyek képesek automatikus javítás céljából.</span><span class="sxs-lookup"><span data-stu-id="7d430-129">hello following table describes hello options that can be configured for Automated Patching.</span></span> <span data-ttu-id="7d430-130">Klasszikus virtuális gépekhez PowerShell tooconfigure ezeket a beállításokat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="7d430-130">For classic VMs, you must use PowerShell tooconfigure these settings.</span></span>

| <span data-ttu-id="7d430-131">Beállítás</span><span class="sxs-lookup"><span data-stu-id="7d430-131">Setting</span></span> | <span data-ttu-id="7d430-132">Lehetséges értékek</span><span class="sxs-lookup"><span data-stu-id="7d430-132">Possible values</span></span> | <span data-ttu-id="7d430-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="7d430-133">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7d430-134">**Automatikus javítás**</span><span class="sxs-lookup"><span data-stu-id="7d430-134">**Automated Patching**</span></span> |<span data-ttu-id="7d430-135">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="7d430-135">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="7d430-136">Engedélyezi vagy letiltja az automatikus javítás egy Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="7d430-136">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="7d430-137">**Karbantartási ütemezését**</span><span class="sxs-lookup"><span data-stu-id="7d430-137">**Maintenance schedule**</span></span> |<span data-ttu-id="7d430-138">Mindennap, hétfő, kedd, szerda, csütörtök, péntek, szombat, vasárnap</span><span class="sxs-lookup"><span data-stu-id="7d430-138">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="7d430-139">hello ütemezése a virtuális gép Windows, az SQL Server és a Microsoft-frissítések letöltése és telepítése.</span><span class="sxs-lookup"><span data-stu-id="7d430-139">hello schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="7d430-140">**A karbantartás indításának időpontja**</span><span class="sxs-lookup"><span data-stu-id="7d430-140">**Maintenance start hour**</span></span> |<span data-ttu-id="7d430-141">0-24</span><span class="sxs-lookup"><span data-stu-id="7d430-141">0-24</span></span> |<span data-ttu-id="7d430-142">hello helyi kezdési idő tooupdate hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7d430-142">hello local start time tooupdate hello virtual machine.</span></span> |
| <span data-ttu-id="7d430-143">**Karbantartási ablak időtartama**</span><span class="sxs-lookup"><span data-stu-id="7d430-143">**Maintenance window duration**</span></span> |<span data-ttu-id="7d430-144">30-180</span><span class="sxs-lookup"><span data-stu-id="7d430-144">30-180</span></span> |<span data-ttu-id="7d430-145">hello hány perc engedélyezett toocomplete hello letöltése és a frissítések telepítését.</span><span class="sxs-lookup"><span data-stu-id="7d430-145">hello number of minutes permitted toocomplete hello download and installation of updates.</span></span> |
| <span data-ttu-id="7d430-146">**Javítás kategória**</span><span class="sxs-lookup"><span data-stu-id="7d430-146">**Patch Category**</span></span> |<span data-ttu-id="7d430-147">Fontos</span><span class="sxs-lookup"><span data-stu-id="7d430-147">Important</span></span> |<span data-ttu-id="7d430-148">hello kategóriája frissítések toodownload és telepítése.</span><span class="sxs-lookup"><span data-stu-id="7d430-148">hello category of updates toodownload and install.</span></span> |

## <a name="configuration-with-powershell"></a><span data-ttu-id="7d430-149">PowerShell-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7d430-149">Configuration with PowerShell</span></span>
<span data-ttu-id="7d430-150">A következő példa hello, a PowerShell használt tooconfigure egy meglévő SQL Server virtuális Gépen az automatikus javítás.</span><span class="sxs-lookup"><span data-stu-id="7d430-150">In hello following example, PowerShell is used tooconfigure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="7d430-151">Hello **New-AzureVMSqlServerAutoPatchingConfig** parancs konfigurálja az automatikus frissítések egy új karbantartási időszakot.</span><span class="sxs-lookup"><span data-stu-id="7d430-151">hello **New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

<span data-ttu-id="7d430-152">Ez a példa alapján, hello következő táblázatban hello gyakorlati hatással hello cél Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="7d430-152">Based on this example, hello following table describes hello practical effect on hello target Azure VM:</span></span>

| <span data-ttu-id="7d430-153">Paraméter</span><span class="sxs-lookup"><span data-stu-id="7d430-153">Parameter</span></span> | <span data-ttu-id="7d430-154">Következmény</span><span class="sxs-lookup"><span data-stu-id="7d430-154">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="7d430-155">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="7d430-155">**DayOfWeek**</span></span> |<span data-ttu-id="7d430-156">Javítások minden csütörtök telepítve.</span><span class="sxs-lookup"><span data-stu-id="7d430-156">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="7d430-157">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="7d430-157">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="7d430-158">A kezdő frissítések 11:00 órakor.</span><span class="sxs-lookup"><span data-stu-id="7d430-158">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="7d430-159">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="7d430-159">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="7d430-160">Javítások 120 percen belül kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="7d430-160">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="7d430-161">Hello kezdési ideje alapján, el kell végezniük 1:00 pm által.</span><span class="sxs-lookup"><span data-stu-id="7d430-161">Based on hello start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="7d430-162">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="7d430-162">**PatchCategory**</span></span> |<span data-ttu-id="7d430-163">hello csak lehetséges Ez a paraméter értéke "Fontos".</span><span class="sxs-lookup"><span data-stu-id="7d430-163">hello only possible setting for this parameter is “Important”.</span></span> |

<span data-ttu-id="7d430-164">Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7d430-164">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="7d430-165">toodisable automatikus javítás, azonos nélkül parancsfájl futtatási hello hello - Enable paramétert a New-AzureVMSqlServerAutoPatchingConfig toohello.</span><span class="sxs-lookup"><span data-stu-id="7d430-165">toodisable Automated Patching, run hello same script without hello -Enable parameter toohello New-AzureVMSqlServerAutoPatchingConfig.</span></span> <span data-ttu-id="7d430-166">Mivel a telepítés folytatásához is igénybe vehet néhány percet toodisable automatikus javítás.</span><span class="sxs-lookup"><span data-stu-id="7d430-166">As with installation, it can take several minutes toodisable Automated Patching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d430-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d430-167">Next steps</span></span>
<span data-ttu-id="7d430-168">Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="7d430-168">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](../classic/sql-server-agent-extension.md).</span></span>

<span data-ttu-id="7d430-169">További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7d430-169">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

