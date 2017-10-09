---
title: "aaaAutomated javítás az SQL Server virtuális géphez (Resource Manager) |} Microsoft Docs"
description: "Ismerteti, az SQL Server rendszeren futó virtuális gépek az Azure Resource Manager használatával hello automatikus javítás funkció."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: 8bb8d0fb265e69d7bbf1fa047f5ceef02e7c56fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a><span data-ttu-id="b295a-103">Az SQL Server automatikus javítása az Azure Virtual Machines szolgáltatásban (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="b295a-103">Automated Patching for SQL Server in Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b295a-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b295a-104">Resource Manager</span></span>](virtual-machines-windows-sql-automated-patching.md)
> * [<span data-ttu-id="b295a-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="b295a-105">Classic</span></span>](../classic/sql-automated-patching.md)
> 
> 

<span data-ttu-id="b295a-106">Automatikus javítás karbantartási időszak az az Azure virtuális gép fut az SQL Server hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b295a-106">Automated Patching establishes a maintenance window for an Azure Virtual Machine running SQL Server.</span></span> <span data-ttu-id="b295a-107">Az automatikus frissítések csak a karbantartási időszak alatt lesznek telepítve.</span><span class="sxs-lookup"><span data-stu-id="b295a-107">Automated Updates can only be installed during this maintenance window.</span></span> <span data-ttu-id="b295a-108">Az SQL Server a rescriction biztosítja, hogy a rendszerfrissítések és minden kapcsolódó újraindítások halasztja hello lehetséges érdemes hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b295a-108">For SQL Server, this rescriction ensures that system updates and any associated restarts occur at hello best possible time for hello database.</span></span> <span data-ttu-id="b295a-109">Automatikus javítás függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b295a-109">Automated Patching depends on hello [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="b295a-110">tooview hello klasszikus ebben a cikkben forduljon [automatikus javítás az SQL Server Azure virtuális gépek Classic](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="b295a-110">tooview hello classic version of this article, see [Automated Patching for SQL Server in Azure Virtual Machines Classic](../classic/sql-automated-patching.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b295a-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b295a-111">Prerequisites</span></span>
<span data-ttu-id="b295a-112">toouse automatikus javítás, fontolja meg a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="b295a-112">toouse Automated Patching, consider hello following prerequisites:</span></span>

<span data-ttu-id="b295a-113">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="b295a-113">**Operating System**:</span></span>

* <span data-ttu-id="b295a-114">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="b295a-114">Windows Server 2012</span></span>
* <span data-ttu-id="b295a-115">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="b295a-115">Windows Server 2012 R2</span></span>
* <span data-ttu-id="b295a-116">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="b295a-116">Windows Server 2016</span></span>

<span data-ttu-id="b295a-117">**SQL Server-verzió**:</span><span class="sxs-lookup"><span data-stu-id="b295a-117">**SQL Server version**:</span></span>

* <span data-ttu-id="b295a-118">SQL Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="b295a-118">SQL Server 2012</span></span>
* <span data-ttu-id="b295a-119">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="b295a-119">SQL Server 2014</span></span>
* <span data-ttu-id="b295a-120">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="b295a-120">SQL Server 2016</span></span>

<span data-ttu-id="b295a-121">**Az Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="b295a-121">**Azure PowerShell**:</span></span>

* <span data-ttu-id="b295a-122">[Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview) Ha azt tervezi, hogy tooconfigure automatikus javítás a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b295a-122">[Install hello latest Azure PowerShell commands](/powershell/azure/overview) if you plan tooconfigure Automated Patching with PowerShell.</span></span>

> [!NOTE]
> <span data-ttu-id="b295a-123">Automatikus javítás az SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="b295a-123">Automated Patching relies on hello SQL Server IaaS Agent Extension.</span></span> <span data-ttu-id="b295a-124">A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="b295a-124">Current SQL virtual machine gallery images add this extension by default.</span></span> <span data-ttu-id="b295a-125">További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b295a-125">For more information, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>
> 
> 

## <a name="settings"></a><span data-ttu-id="b295a-126">Beállítások</span><span class="sxs-lookup"><span data-stu-id="b295a-126">Settings</span></span>
<span data-ttu-id="b295a-127">hello következő táblázatban hello-beállítások, amelyek képesek automatikus javítás céljából.</span><span class="sxs-lookup"><span data-stu-id="b295a-127">hello following table describes hello options that can be configured for Automated Patching.</span></span> <span data-ttu-id="b295a-128">hello tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e hello Azure-portálon vagy az Azure PowerShell-parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="b295a-128">hello actual configuration steps vary depending on whether you use hello Azure portal or Azure Windows PowerShell commands.</span></span>

| <span data-ttu-id="b295a-129">Beállítás</span><span class="sxs-lookup"><span data-stu-id="b295a-129">Setting</span></span> | <span data-ttu-id="b295a-130">Lehetséges értékek</span><span class="sxs-lookup"><span data-stu-id="b295a-130">Possible values</span></span> | <span data-ttu-id="b295a-131">Leírás</span><span class="sxs-lookup"><span data-stu-id="b295a-131">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b295a-132">**Automatikus javítás**</span><span class="sxs-lookup"><span data-stu-id="b295a-132">**Automated Patching**</span></span> |<span data-ttu-id="b295a-133">Engedélyezi/letiltja (letiltva)</span><span class="sxs-lookup"><span data-stu-id="b295a-133">Enable/Disable (Disabled)</span></span> |<span data-ttu-id="b295a-134">Engedélyezi vagy letiltja az automatikus javítás egy Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="b295a-134">Enables or disables Automated Patching for an Azure virtual machine.</span></span> |
| <span data-ttu-id="b295a-135">**Karbantartási ütemezését**</span><span class="sxs-lookup"><span data-stu-id="b295a-135">**Maintenance schedule**</span></span> |<span data-ttu-id="b295a-136">Mindennap, hétfő, kedd, szerda, csütörtök, péntek, szombat, vasárnap</span><span class="sxs-lookup"><span data-stu-id="b295a-136">Everyday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</span></span> |<span data-ttu-id="b295a-137">hello ütemezése a virtuális gép Windows, az SQL Server és a Microsoft-frissítések letöltése és telepítése.</span><span class="sxs-lookup"><span data-stu-id="b295a-137">hello schedule for downloading and installing Windows, SQL Server, and Microsoft updates for your virtual machine.</span></span> |
| <span data-ttu-id="b295a-138">**A karbantartás indításának időpontja**</span><span class="sxs-lookup"><span data-stu-id="b295a-138">**Maintenance start hour**</span></span> |<span data-ttu-id="b295a-139">0-24</span><span class="sxs-lookup"><span data-stu-id="b295a-139">0-24</span></span> |<span data-ttu-id="b295a-140">hello helyi kezdési idő tooupdate hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b295a-140">hello local start time tooupdate hello virtual machine.</span></span> |
| <span data-ttu-id="b295a-141">**Karbantartási ablak időtartama**</span><span class="sxs-lookup"><span data-stu-id="b295a-141">**Maintenance window duration**</span></span> |<span data-ttu-id="b295a-142">30-180</span><span class="sxs-lookup"><span data-stu-id="b295a-142">30-180</span></span> |<span data-ttu-id="b295a-143">hello hány perc engedélyezett toocomplete hello letöltése és a frissítések telepítését.</span><span class="sxs-lookup"><span data-stu-id="b295a-143">hello number of minutes permitted toocomplete hello download and installation of updates.</span></span> |
| <span data-ttu-id="b295a-144">**Javítás kategória**</span><span class="sxs-lookup"><span data-stu-id="b295a-144">**Patch Category**</span></span> |<span data-ttu-id="b295a-145">Fontos</span><span class="sxs-lookup"><span data-stu-id="b295a-145">Important</span></span> |<span data-ttu-id="b295a-146">hello kategóriája frissítések toodownload és telepítése.</span><span class="sxs-lookup"><span data-stu-id="b295a-146">hello category of updates toodownload and install.</span></span> |

## <a name="configuration-in-hello-portal"></a><span data-ttu-id="b295a-147">A portál hello konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b295a-147">Configuration in hello Portal</span></span>
<span data-ttu-id="b295a-148">Használhatja az Azure portál tooconfigure hello kiépítése során, vagy meglévő virtuális gépek automatikus javítás.</span><span class="sxs-lookup"><span data-stu-id="b295a-148">You can use hello Azure portal tooconfigure Automated Patching during provisioning or for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="b295a-149">Új virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b295a-149">New VMs</span></span>
<span data-ttu-id="b295a-150">Használjon hello Azure portál tooconfigure automatikus javítás, amikor létrehoz egy új SQL Server virtuális gép hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b295a-150">Use hello Azure portal tooconfigure Automated Patching when you create a new SQL Server Virtual Machine in hello Resource Manager deployment model.</span></span>

<span data-ttu-id="b295a-151">A hello **SQL Server-beállítások** panelen válassza **automatikus javítás**.</span><span class="sxs-lookup"><span data-stu-id="b295a-151">In hello **SQL Server settings** blade, select **Automated patching**.</span></span> <span data-ttu-id="b295a-152">hello Azure portál következő képernyőfelvételen látható hello **SQL automatikus javítás** panelen.</span><span class="sxs-lookup"><span data-stu-id="b295a-152">hello following Azure portal screenshot shows hello **SQL Automated Patching** blade.</span></span>

![SQL automatikus javítás Azure-portálon](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

<span data-ttu-id="b295a-154">A környezetben, témakörében hello teljes [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="b295a-154">For context, see hello complete topic on [provisioning a SQL Server virtual machine in Azure](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="b295a-155">Meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b295a-155">Existing VMs</span></span>
<span data-ttu-id="b295a-156">Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b295a-156">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="b295a-157">Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="b295a-157">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![SQL automatikus javítás meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

<span data-ttu-id="b295a-159">A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello gombjára automatikus javítási szakasz.</span><span class="sxs-lookup"><span data-stu-id="b295a-159">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated patching section.</span></span>

![Meglévő virtuális gépek SQL automatikus javítás beállítása](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

<span data-ttu-id="b295a-161">Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b295a-161">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

<span data-ttu-id="b295a-162">Ha engedélyezi az automatikus javítás hello az első alkalommal, Azure SQL Server IaaS-ügynök hello hello háttérben konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="b295a-162">If you are enabling Automated Patching for hello first time, Azure configures hello SQL Server IaaS Agent in hello background.</span></span> <span data-ttu-id="b295a-163">Ebben az időszakban hello Azure-portálon előfordulhat, hogy jelenjen meg, hogy konfigurálva van-e az automatikus javítás.</span><span class="sxs-lookup"><span data-stu-id="b295a-163">During this time, hello Azure portal might not show that Automated Patching is configured.</span></span> <span data-ttu-id="b295a-164">Várjon egy pár percet hello ügynök toobe telepítve, a konfigurált.</span><span class="sxs-lookup"><span data-stu-id="b295a-164">Wait several minutes for hello agent toobe installed, configured.</span></span> <span data-ttu-id="b295a-165">Követően, hogy hello Azure portal tükrözi hello új beállítások.</span><span class="sxs-lookup"><span data-stu-id="b295a-165">After that hello Azure portal reflects hello new settings.</span></span>

> [!NOTE]
> <span data-ttu-id="b295a-166">Automatikus javítás sablon használatával is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="b295a-166">You can also configure Automated Patching using a template.</span></span> <span data-ttu-id="b295a-167">További információkért lásd: [Azure gyors üzembe helyezés sablon automatikus javítás céljából](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).</span><span class="sxs-lookup"><span data-stu-id="b295a-167">For more information, see [Azure quickstart template for Automated Patching](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).</span></span>
> 
> 

## <a name="configuration-with-powershell"></a><span data-ttu-id="b295a-168">PowerShell-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b295a-168">Configuration with PowerShell</span></span>
<span data-ttu-id="b295a-169">Miután az SQL virtuális gép kiépítése, használja a PowerShell tooconfigure automatikus javítás.</span><span class="sxs-lookup"><span data-stu-id="b295a-169">After provisioning your SQL VM, use PowerShell tooconfigure Automated Patching.</span></span>

<span data-ttu-id="b295a-170">A következő példa hello, a PowerShell használt tooconfigure egy meglévő SQL Server virtuális Gépen az automatikus javítás.</span><span class="sxs-lookup"><span data-stu-id="b295a-170">In hello following example, PowerShell is used tooconfigure Automated Patching on an existing SQL Server VM.</span></span> <span data-ttu-id="b295a-171">Hello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** parancs konfigurálja az automatikus frissítések egy új karbantartási időszakot.</span><span class="sxs-lookup"><span data-stu-id="b295a-171">hello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** command configures a new maintenance window for automatic updates.</span></span>

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

<span data-ttu-id="b295a-172">Ez a példa alapján, hello következő táblázatban hello gyakorlati hatással hello cél Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="b295a-172">Based on this example, hello following table describes hello practical effect on hello target Azure VM:</span></span>

| <span data-ttu-id="b295a-173">Paraméter</span><span class="sxs-lookup"><span data-stu-id="b295a-173">Parameter</span></span> | <span data-ttu-id="b295a-174">Következmény</span><span class="sxs-lookup"><span data-stu-id="b295a-174">Effect</span></span> |
| --- | --- |
| <span data-ttu-id="b295a-175">**DayOfWeek**</span><span class="sxs-lookup"><span data-stu-id="b295a-175">**DayOfWeek**</span></span> |<span data-ttu-id="b295a-176">Javítások minden csütörtök telepítve.</span><span class="sxs-lookup"><span data-stu-id="b295a-176">Patches installed every Thursday.</span></span> |
| <span data-ttu-id="b295a-177">**MaintenanceWindowStartingHour**</span><span class="sxs-lookup"><span data-stu-id="b295a-177">**MaintenanceWindowStartingHour**</span></span> |<span data-ttu-id="b295a-178">A kezdő frissítések 11:00 órakor.</span><span class="sxs-lookup"><span data-stu-id="b295a-178">Begin updates at 11:00am.</span></span> |
| <span data-ttu-id="b295a-179">**MaintenanceWindowsDuration**</span><span class="sxs-lookup"><span data-stu-id="b295a-179">**MaintenanceWindowsDuration**</span></span> |<span data-ttu-id="b295a-180">Javítások 120 percen belül kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="b295a-180">Patches must be installed within 120 minutes.</span></span> <span data-ttu-id="b295a-181">Hello kezdési ideje alapján, el kell végezniük 1:00 pm által.</span><span class="sxs-lookup"><span data-stu-id="b295a-181">Based on hello start time, they must complete by 1:00pm.</span></span> |
| <span data-ttu-id="b295a-182">**PatchCategory**</span><span class="sxs-lookup"><span data-stu-id="b295a-182">**PatchCategory**</span></span> |<span data-ttu-id="b295a-183">hello kapcsolatos Ez a paraméter csak lehetséges beállítása **fontos**.</span><span class="sxs-lookup"><span data-stu-id="b295a-183">hello only possible setting for this parameter is **Important**.</span></span> |

<span data-ttu-id="b295a-184">Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b295a-184">It could take several minutes tooinstall and configure hello SQL Server IaaS Agent.</span></span>

<span data-ttu-id="b295a-185">toodisable automatikus javítás, azonos hello nélkül parancsfájl futtatási hello **-engedélyezése** paraméter toohello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**.</span><span class="sxs-lookup"><span data-stu-id="b295a-185">toodisable Automated Patching, run hello same script without hello **-Enable** parameter toohello **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig**.</span></span> <span data-ttu-id="b295a-186">hello hiányában hello **-engedélyezése** paraméter jelek hello parancs toodisable hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b295a-186">hello absence of hello **-Enable** parameter signals hello command toodisable hello feature.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b295a-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b295a-187">Next steps</span></span>
<span data-ttu-id="b295a-188">Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b295a-188">For information about other available automation tasks, see [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md).</span></span>

<span data-ttu-id="b295a-189">További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b295a-189">For more information about running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

