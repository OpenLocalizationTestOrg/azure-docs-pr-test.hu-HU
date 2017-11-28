---
title: "SQL virtuális gépeken (erőforrás-kezelő) felügyeleti feladatok automatizálására |} Microsoft Docs"
description: "Ez a témakör az SQL Server agent-kiterjesztés, automatizálja az adott SQL Server felügyeleti feladatok kezelését ismerteti. Ezek közé tartoznak az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció. Ez a témakör a Resource Manager telepítési módot használ."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7152d184bb6d1d4b81aeb47e2c7c9160ada36023
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a><span data-ttu-id="80547-105">Azure virtuális gépeken kiterjesztésű SQL Server Agent (erőforrás-kezelő) felügyeleti feladatok automatizálásához</span><span class="sxs-lookup"><span data-stu-id="80547-105">Automate management tasks on Azure Virtual Machines with the SQL Server Agent Extension (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="80547-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="80547-106">Resource Manager</span></span>](virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="80547-107">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="80547-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
> 

<span data-ttu-id="80547-108">Az SQL Server IaaS ügynök Extension (SQLIaaSExtension) fut az Azure virtuális gépeken felügyeleti feladatok automatizálására.</span><span class="sxs-lookup"><span data-stu-id="80547-108">The SQL Server IaaS Agent Extension (SQLIaaSExtension) runs on Azure virtual machines to automate administration tasks.</span></span> <span data-ttu-id="80547-109">Ez a témakör a bővítményt, valamint a vonatkozó telepítési, állapot és eltávolítási utasításokat támogatja a szolgáltatások áttekintését.</span><span class="sxs-lookup"><span data-stu-id="80547-109">This topic provides an overview of the services supported by the extension as well as instructions for installation, status, and removal.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="80547-110">Ez a cikk a klasszikus verzióra megtekintése: [SQL Server Agent bővítmény SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="80547-110">To view the classic version of this article, see [SQL Server Agent Extension for SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="80547-111">Támogatott szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="80547-111">Supported services</span></span>
<span data-ttu-id="80547-112">Az SQL Server IaaS ügynöke bővítmény a következő felügyeleti feladatokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="80547-112">The SQL Server IaaS Agent Extension supports the following administration tasks:</span></span>

| <span data-ttu-id="80547-113">Felügyeleti szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="80547-113">Administration feature</span></span> | <span data-ttu-id="80547-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="80547-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="80547-115">**SQL automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="80547-115">**SQL Automated Backup**</span></span> |<span data-ttu-id="80547-116">Automatizálja az ütemezés a biztonsági mentések az összes olyan adatbázis az alapértelmezett példány az SQL Server, a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="80547-116">Automates the scheduling of backups for all databases for the default instance of SQL Server in the VM.</span></span> <span data-ttu-id="80547-117">További információkért lásd: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek (erőforrás-kezelő)](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="80547-117">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span></span> |
| <span data-ttu-id="80547-118">**SQL automatikus javítás**</span><span class="sxs-lookup"><span data-stu-id="80547-118">**SQL Automated Patching**</span></span> |<span data-ttu-id="80547-119">Konfigurálja a karbantartási időszak során, ami a virtuális gép kerül sor frissítéseire, a munkaterheléshez csúcsidőben frissítések elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="80547-119">Configures a maintenance window during which updates to your VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="80547-120">További információkért lásd: [automatikus javítás az SQL Server az Azure virtuális gépek (erőforrás-kezelő)](virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="80547-120">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span></span> |
| <span data-ttu-id="80547-121">**Azure Key Vault-integráció**</span><span class="sxs-lookup"><span data-stu-id="80547-121">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="80547-122">Lehetővé teszi, hogy automatikusan telepítse és konfigurálja az Azure Key Vault az SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="80547-122">Enables you to automatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="80547-123">További információkért lásd: [konfigurálása Azure Key Vault-integráció az SQL Server Azure virtuális gépeken (erőforrás-kezelő)](virtual-machines-windows-ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="80547-123">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span></span> |

<span data-ttu-id="80547-124">Miután telepített és futó, az SQL Server IaaS ügynöke bővítmény elérhetővé teszi ezeket a felügyeleti szolgáltatásokat az SQL Server panelen, a virtuális gép az Azure portál és az SQL Server piactéren elérhető rendszerkép Azure PowerShell használatával, és Azure szolgáltatáson keresztül A bővítmény manuális telepítésekor PowerShell.</span><span class="sxs-lookup"><span data-stu-id="80547-124">Once installed and running, the SQL Server IaaS Agent Extension makes these administration features available on the SQL Server panel of the virtual machine in the Azure Portal and through Azure PowerShell for SQL Server marketplace images, and through Azure PowerShell for manual installations of the extension.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="80547-125">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="80547-125">Prerequisites</span></span>
<span data-ttu-id="80547-126">A virtuális gép az SQL Server IaaS ügynöke bővítmény használatára vonatkozó követelményeket:</span><span class="sxs-lookup"><span data-stu-id="80547-126">Requirements to use the SQL Server IaaS Agent Extension on your VM:</span></span>

<span data-ttu-id="80547-127">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="80547-127">**Operating System**:</span></span>

* <span data-ttu-id="80547-128">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="80547-128">Windows Server 2012</span></span>
* <span data-ttu-id="80547-129">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="80547-129">Windows Server 2012 R2</span></span>
* <span data-ttu-id="80547-130">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="80547-130">Windows Server 2016</span></span>

<span data-ttu-id="80547-131">**SQL Server-verziók**:</span><span class="sxs-lookup"><span data-stu-id="80547-131">**SQL Server versions**:</span></span>

* <span data-ttu-id="80547-132">SQL Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="80547-132">SQL Server 2012</span></span>
* <span data-ttu-id="80547-133">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="80547-133">SQL Server 2014</span></span>
* <span data-ttu-id="80547-134">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="80547-134">SQL Server 2016</span></span>

<span data-ttu-id="80547-135">**Az Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="80547-135">**Azure PowerShell**:</span></span>

* [<span data-ttu-id="80547-136">Töltse le és konfigurálja a legújabb Azure PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="80547-136">Download and configure the latest Azure PowerShell commands</span></span>](/powershell/azure/overview)

## <a name="installation"></a><span data-ttu-id="80547-137">Telepítés</span><span class="sxs-lookup"><span data-stu-id="80547-137">Installation</span></span>
<span data-ttu-id="80547-138">Az SQL Server IaaS ügynöke bővítmény automatikusan települ, amikor egy SQL Server virtuális gép gyűjtemény lemezképet.</span><span class="sxs-lookup"><span data-stu-id="80547-138">The SQL Server IaaS Agent Extension is automatically installed when you provision one of the SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="80547-139">Ha szeretné manuálisan újra a bővítményt a virtuális gépeken SQL Server egyik, használja a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="80547-139">If you need to reinstall the extension manually on one of these SQL Server VMs, use the following PowerShell command:</span></span>

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

<span data-ttu-id="80547-140">Akkor is telepíteni az SQL Server infrastruktúra-szolgáltatási ügynök kiterjesztés csak az operációs rendszer Windows Server virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="80547-140">It is also possible to install the SQL Server IaaS Agent Extension on an OS-only Windows Server virtual machine.</span></span> <span data-ttu-id="80547-141">Ez csak akkor támogatott, ha manuálisan is, hogy a gép van telepítve az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="80547-141">This is only supported if you have also manually installed SQL Server on that machine.</span></span> <span data-ttu-id="80547-142">Majd telepítse a bővítmény manuálisan azonos **Set-AzureVMSqlServerExtension** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="80547-142">Then install the extension manually by using the same **Set-AzureVMSqlServerExtension** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="80547-143">Ha manuálisan telepíti az SQL Server IaaS ügynöke bővítmény egy csak az operációs rendszer Windows Server virtuális gépen, az SQL Server-konfigurációs beállítások, az Azure portálon keresztül nem kezelheti.</span><span class="sxs-lookup"><span data-stu-id="80547-143">If you manually install the SQL Server IaaS Agent Extension on an OS-only Windows Server VM, you can not manage the SQL Server configuration settings through the Azure portal.</span></span> <span data-ttu-id="80547-144">Ebben a forgatókönyvben módosítania kell az összes a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="80547-144">In this scenario, you must make all changes with PowerShell.</span></span>

## <a name="status"></a><span data-ttu-id="80547-145">status</span><span class="sxs-lookup"><span data-stu-id="80547-145">Status</span></span>
<span data-ttu-id="80547-146">Egy győződjön meg arról, hogy telepítve van-e a bővítmény módja az ügynök állapotát megtekintheti az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="80547-146">One way to verify that the extension is installed is to view the agent status in the Azure Portal.</span></span> <span data-ttu-id="80547-147">Válassza ki **összes beállítás** a virtuális gépek paneljét, majd a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="80547-147">Select **All settings** in the virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="80547-148">Megjelenik a **SQLIaaSExtension** felsorolt bővítmény.</span><span class="sxs-lookup"><span data-stu-id="80547-148">You should see the **SQLIaaSExtension** extension listed.</span></span>

![SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

<span data-ttu-id="80547-150">Használhatja a **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="80547-150">You can also use the **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

<span data-ttu-id="80547-151">Az előző parancs megerősíti, hogy az ügynök telepítve van, és az általános állapotadatokat szolgáltat.</span><span class="sxs-lookup"><span data-stu-id="80547-151">The previous command confirms the agent is installed and provides general status information.</span></span> <span data-ttu-id="80547-152">Adott állapotinformációról automatikus biztonsági mentés és a javítás az alábbi parancsokkal is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="80547-152">You can also get specific status information about Automated Backup and Patching with the following commands.</span></span>

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a><span data-ttu-id="80547-153">Eltávolítás</span><span class="sxs-lookup"><span data-stu-id="80547-153">Removal</span></span>
<span data-ttu-id="80547-154">Az Azure portálon, eltávolíthatja a bővítményt a három pont parancsával a **bővítmények** panelen található a virtuálisgép-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="80547-154">In the Azure Portal, you can uninstall the extension by clicking the ellipsis on the **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="80547-155">Kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="80547-155">Then click **Delete**.</span></span>

![Távolítsa el az SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="80547-157">Használhatja a **Remove-AzureRmVMSqlServerExtension** Powershell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="80547-157">You can also use the **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet.</span></span>

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a><span data-ttu-id="80547-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80547-158">Next Steps</span></span>
<span data-ttu-id="80547-159">A bővítmény által támogatott szolgáltatások használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="80547-159">Begin using one of the services supported by the extension.</span></span> <span data-ttu-id="80547-160">A hivatkozott témakörökben talál további részleteket a [támogatott szolgáltatások](#supported-services) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="80547-160">For more details, see the topics referenced in the [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="80547-161">További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="80547-161">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

