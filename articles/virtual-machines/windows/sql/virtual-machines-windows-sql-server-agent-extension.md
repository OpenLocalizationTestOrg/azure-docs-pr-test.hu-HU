---
title: "SQL virtuális gépek (erőforrás-kezelő) aaaAutomate fájlkezelési feladatai |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toomanage hello SQL Server agent-kiterjesztés, automatizálja az adott SQL Server felügyeleti feladatokat. Ezek közé tartoznak az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció. Ez a témakör hello erőforrás-kezelő telepítési módot használ."
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
ms.openlocfilehash: ae917612c4af59f12c0b083440673bdc555e9d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-resource-manager"></a><span data-ttu-id="cf404-105">SQL Server Agent bővítmény (erőforrás-kezelő) hello Azure virtuális gépeken felügyeleti műveletek automatizálása</span><span class="sxs-lookup"><span data-stu-id="cf404-105">Automate management tasks on Azure Virtual Machines with hello SQL Server Agent Extension (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cf404-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cf404-106">Resource Manager</span></span>](virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="cf404-107">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="cf404-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
> 

<span data-ttu-id="cf404-108">SQL Server infrastruktúra-szolgáltatási ügynök Extension (SQLIaaSExtension) hello futó Azure virtuális gépek tooautomate felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="cf404-108">hello SQL Server IaaS Agent Extension (SQLIaaSExtension) runs on Azure virtual machines tooautomate administration tasks.</span></span> <span data-ttu-id="cf404-109">Ez a témakör áttekintést hello szolgáltatások hello bővítményt, valamint utasításokat a telepítési, állapot és eltávolítási által támogatott.</span><span class="sxs-lookup"><span data-stu-id="cf404-109">This topic provides an overview of hello services supported by hello extension as well as instructions for installation, status, and removal.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="cf404-110">tooview hello klasszikus ebben a cikkben forduljon [SQL Server Agent bővítmény SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="cf404-110">tooview hello classic version of this article, see [SQL Server Agent Extension for SQL Server VMs Classic](../classic/sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="cf404-111">Támogatott szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cf404-111">Supported services</span></span>
<span data-ttu-id="cf404-112">SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello felügyeleti feladatok a következő hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="cf404-112">hello SQL Server IaaS Agent Extension supports hello following administration tasks:</span></span>

| <span data-ttu-id="cf404-113">Felügyeleti szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cf404-113">Administration feature</span></span> | <span data-ttu-id="cf404-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="cf404-114">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cf404-115">**SQL automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="cf404-115">**SQL Automated Backup**</span></span> |<span data-ttu-id="cf404-116">Automatikusan hello ütemezés a biztonsági mentések az összes olyan adatbázis hello alapértelmezett példány az SQL Server a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="cf404-116">Automates hello scheduling of backups for all databases for hello default instance of SQL Server in hello VM.</span></span> <span data-ttu-id="cf404-117">További információkért lásd: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek (erőforrás-kezelő)](virtual-machines-windows-sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="cf404-117">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).</span></span> |
| <span data-ttu-id="cf404-118">**SQL automatikus javítás**</span><span class="sxs-lookup"><span data-stu-id="cf404-118">**SQL Automated Patching**</span></span> |<span data-ttu-id="cf404-119">Konfigurálja a karbantartási időszak során mely frissítések VM is igénybe vehet tooyour helyezze el, a munkaterhelés csúcsidőben frissítések elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="cf404-119">Configures a maintenance window during which updates tooyour VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="cf404-120">További információkért lásd: [automatikus javítás az SQL Server az Azure virtuális gépek (erőforrás-kezelő)](virtual-machines-windows-sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="cf404-120">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).</span></span> |
| <span data-ttu-id="cf404-121">**Azure Key Vault-integráció**</span><span class="sxs-lookup"><span data-stu-id="cf404-121">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="cf404-122">Lehetővé teszi, hogy tooautomatically telepítse és konfigurálja az Azure Key Vault az az SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cf404-122">Enables you tooautomatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="cf404-123">További információkért lásd: [konfigurálása Azure Key Vault-integráció az SQL Server Azure virtuális gépeken (erőforrás-kezelő)](virtual-machines-windows-ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="cf404-123">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).</span></span> |

<span data-ttu-id="cf404-124">Amikor telepítve és fut, hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény elérhetővé teszi ezeket a felügyeleti funkciókat hello SQL Server panelen hello virtuális gép hello Azure portálon, és Azure PowerShell használatával az SQL Server piactéren elérhető rendszerkép és keresztül Az Azure PowerShell hello bővítmény manuális telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="cf404-124">Once installed and running, hello SQL Server IaaS Agent Extension makes these administration features available on hello SQL Server panel of hello virtual machine in hello Azure Portal and through Azure PowerShell for SQL Server marketplace images, and through Azure PowerShell for manual installations of hello extension.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cf404-125">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cf404-125">Prerequisites</span></span>
<span data-ttu-id="cf404-126">Követelmények toouse hello SQL Server IaaS ügynök bővítményt a virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="cf404-126">Requirements toouse hello SQL Server IaaS Agent Extension on your VM:</span></span>

<span data-ttu-id="cf404-127">**Operációs rendszer**:</span><span class="sxs-lookup"><span data-stu-id="cf404-127">**Operating System**:</span></span>

* <span data-ttu-id="cf404-128">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="cf404-128">Windows Server 2012</span></span>
* <span data-ttu-id="cf404-129">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="cf404-129">Windows Server 2012 R2</span></span>
* <span data-ttu-id="cf404-130">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="cf404-130">Windows Server 2016</span></span>

<span data-ttu-id="cf404-131">**SQL Server-verziók**:</span><span class="sxs-lookup"><span data-stu-id="cf404-131">**SQL Server versions**:</span></span>

* <span data-ttu-id="cf404-132">SQL Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="cf404-132">SQL Server 2012</span></span>
* <span data-ttu-id="cf404-133">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="cf404-133">SQL Server 2014</span></span>
* <span data-ttu-id="cf404-134">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="cf404-134">SQL Server 2016</span></span>

<span data-ttu-id="cf404-135">**Az Azure PowerShell**:</span><span class="sxs-lookup"><span data-stu-id="cf404-135">**Azure PowerShell**:</span></span>

* [<span data-ttu-id="cf404-136">Töltse le és konfigurálja a hello legújabb Azure PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="cf404-136">Download and configure hello latest Azure PowerShell commands</span></span>](/powershell/azure/overview)

## <a name="installation"></a><span data-ttu-id="cf404-137">Telepítés</span><span class="sxs-lookup"><span data-stu-id="cf404-137">Installation</span></span>
<span data-ttu-id="cf404-138">hello SQL Server virtuális gép a gyűjtemény lemezképei kiépítése a rendszer automatikusan telepíti az SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello.</span><span class="sxs-lookup"><span data-stu-id="cf404-138">hello SQL Server IaaS Agent Extension is automatically installed when you provision one of hello SQL Server virtual machine gallery images.</span></span> <span data-ttu-id="cf404-139">Ha tooreinstall hello bővítmény manuálisan az SQL Server virtuális gépeken egyik van szüksége, használja a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="cf404-139">If you need tooreinstall hello extension manually on one of these SQL Server VMs, use hello following PowerShell command:</span></span>

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

<span data-ttu-id="cf404-140">Akkor is lehetséges tooinstall hello SQL Server infrastruktúra-szolgáltatási ügynök kiterjesztés csak az operációs rendszer Windows Server virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="cf404-140">It is also possible tooinstall hello SQL Server IaaS Agent Extension on an OS-only Windows Server virtual machine.</span></span> <span data-ttu-id="cf404-141">Ez csak akkor támogatott, ha manuálisan is, hogy a gép van telepítve az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cf404-141">This is only supported if you have also manually installed SQL Server on that machine.</span></span> <span data-ttu-id="cf404-142">Majd a telepítés hello bővítmény használatával manuálisan hello azonos **Set-AzureVMSqlServerExtension** PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="cf404-142">Then install hello extension manually by using hello same **Set-AzureVMSqlServerExtension** PowerShell cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="cf404-143">Ha manuálisan telepíti az SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello egy csak az operációs rendszer Windows Server virtuális gépen, nem kezelheti hello SQL Server-konfigurációs beállítások hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="cf404-143">If you manually install hello SQL Server IaaS Agent Extension on an OS-only Windows Server VM, you can not manage hello SQL Server configuration settings through hello Azure portal.</span></span> <span data-ttu-id="cf404-144">Ebben a forgatókönyvben módosítania kell az összes a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="cf404-144">In this scenario, you must make all changes with PowerShell.</span></span>

## <a name="status"></a><span data-ttu-id="cf404-145">status</span><span class="sxs-lookup"><span data-stu-id="cf404-145">Status</span></span>
<span data-ttu-id="cf404-146">Egyirányú tooverify, hogy telepítve van-e a hello bővítmény hello Azure Portal tooview hello ügynök állapotát.</span><span class="sxs-lookup"><span data-stu-id="cf404-146">One way tooverify that hello extension is installed is tooview hello agent status in hello Azure Portal.</span></span> <span data-ttu-id="cf404-147">Válassza ki **összes beállítás** a hello virtuális gépek paneljét, és kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="cf404-147">Select **All settings** in hello virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="cf404-148">Megtekintheti az hello **SQLIaaSExtension** felsorolt bővítmény.</span><span class="sxs-lookup"><span data-stu-id="cf404-148">You should see hello **SQLIaaSExtension** extension listed.</span></span>

![SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

<span data-ttu-id="cf404-150">Is használhatja a hello **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="cf404-150">You can also use hello **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

<span data-ttu-id="cf404-151">előző parancs hello megerősíti, hogy hello ügynök telepítve van, és az általános állapotadatokat szolgáltat.</span><span class="sxs-lookup"><span data-stu-id="cf404-151">hello previous command confirms hello agent is installed and provides general status information.</span></span> <span data-ttu-id="cf404-152">Automatikus biztonsági mentés és javításokkal bizonyos állapot információt a következő parancsok hello is lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="cf404-152">You can also get specific status information about Automated Backup and Patching with hello following commands.</span></span>

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a><span data-ttu-id="cf404-153">Eltávolítás</span><span class="sxs-lookup"><span data-stu-id="cf404-153">Removal</span></span>
<span data-ttu-id="cf404-154">Hello Azure portál, eltávolíthatja hello bővítmény hello három pont a hello kattintva **bővítmények** panelen található a virtuálisgép-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="cf404-154">In hello Azure Portal, you can uninstall hello extension by clicking hello ellipsis on hello **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="cf404-155">Kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="cf404-155">Then click **Delete**.</span></span>

![Távolítsa el a hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="cf404-157">Is használhatja a hello **Remove-AzureRmVMSqlServerExtension** Powershell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="cf404-157">You can also use hello **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet.</span></span>

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a><span data-ttu-id="cf404-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf404-158">Next Steps</span></span>
<span data-ttu-id="cf404-159">Hello bővítmény által támogatott hello szolgáltatást használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="cf404-159">Begin using one of hello services supported by hello extension.</span></span> <span data-ttu-id="cf404-160">További részletekért lásd: hello hivatkozott hello témakörök [támogatott szolgáltatások](#supported-services) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="cf404-160">For more details, see hello topics referenced in hello [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="cf404-161">További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cf404-161">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

