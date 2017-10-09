---
title: "SQL virtuális gép (klasszikus) aaaAutomate felügyeleti feladatai |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toomanage hello SQL Server agent-kiterjesztés, automatizálja az adott SQL Server felügyeleti feladatokat. Ezek közé tartoznak az automatikus biztonsági mentés, automatikus javítás és az Azure Key Vault-integráció. Ez a témakör hello klasszikus üzembe helyezési módot használ."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a9bda2e7-cdba-427c-bc30-77cde4376f3a
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1dc011e0526845701eaf0c365007938f9ee32ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-hello-sql-server-agent-extension-classic"></a><span data-ttu-id="fc5fe-105">Hello SQL Server Agent Extension (klasszikus) Azure virtuális gépeken felügyeleti műveletek automatizálása</span><span class="sxs-lookup"><span data-stu-id="fc5fe-105">Automate management tasks on Azure Virtual Machines with hello SQL Server Agent Extension (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fc5fe-106">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fc5fe-106">Resource Manager</span></span>](../sql/virtual-machines-windows-sql-server-agent-extension.md)
> * [<span data-ttu-id="fc5fe-107">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="fc5fe-107">Classic</span></span>](../classic/sql-server-agent-extension.md)
> 
>
 
<span data-ttu-id="fc5fe-108">SQL Server infrastruktúra-szolgáltatási ügynök Extension (SQLIaaSAgent) hello futó Azure virtuális gépek tooautomate felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-108">hello SQL Server IaaS Agent Extension (SQLIaaSAgent) runs on Azure virtual machines tooautomate administration tasks.</span></span> <span data-ttu-id="fc5fe-109">Ez a témakör áttekintést hello szolgáltatások hello bővítményt, valamint utasításokat a telepítési, állapot és eltávolítási által támogatott.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-109">This topic provides an overview of hello services supported by hello extension as well as instructions for installation, status, and removal.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="fc5fe-110">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-110">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fc5fe-111">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-111">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="fc5fe-112">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-112">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="fc5fe-113">tooview hello erőforrás-kezelő változata ebből a cikkből: [SQL Server Agent bővítményt az SQL Server virtuális gépek erőforrás-kezelő](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-113">tooview hello Resource Manager version of this article, see [SQL Server Agent Extension for SQL Server VMs Resource Manager](../sql/virtual-machines-windows-sql-server-agent-extension.md).</span></span>

## <a name="supported-services"></a><span data-ttu-id="fc5fe-114">Támogatott szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="fc5fe-114">Supported services</span></span>
<span data-ttu-id="fc5fe-115">SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello felügyeleti feladatok a következő hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="fc5fe-115">hello SQL Server IaaS Agent Extension supports hello following administration tasks:</span></span>

| <span data-ttu-id="fc5fe-116">Felügyeleti szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="fc5fe-116">Administration feature</span></span> | <span data-ttu-id="fc5fe-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="fc5fe-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fc5fe-118">**SQL automatikus biztonsági mentés**</span><span class="sxs-lookup"><span data-stu-id="fc5fe-118">**SQL Automated Backup**</span></span> |<span data-ttu-id="fc5fe-119">Automatikusan hello ütemezés a biztonsági mentések az összes olyan adatbázis hello alapértelmezett példány az SQL Server a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-119">Automates hello scheduling of backups for all databases for hello default instance of SQL Server in hello VM.</span></span> <span data-ttu-id="fc5fe-120">További információkért lásd: [automatikus biztonsági mentés az SQL Server az Azure virtuális gépek (klasszikus)](../classic/sql-automated-backup.md).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-120">For more information, see [Automated backup for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-backup.md).</span></span> |
| <span data-ttu-id="fc5fe-121">**SQL automatikus javítás**</span><span class="sxs-lookup"><span data-stu-id="fc5fe-121">**SQL Automated Patching**</span></span> |<span data-ttu-id="fc5fe-122">Konfigurálja a karbantartási időszak során mely frissítések VM is igénybe vehet tooyour helyezze el, a munkaterhelés csúcsidőben frissítések elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-122">Configures a maintenance window during which updates tooyour VM can take place, so  you can avoid updates during peak times for your workload.</span></span> <span data-ttu-id="fc5fe-123">További információkért lásd: [automatikus javítás az SQL Server az Azure virtuális gépek (klasszikus)](../classic/sql-automated-patching.md).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-123">For more information, see [Automated patching for SQL Server in Azure Virtual Machines (Classic)](../classic/sql-automated-patching.md).</span></span> |
| <span data-ttu-id="fc5fe-124">**Azure Key Vault-integráció**</span><span class="sxs-lookup"><span data-stu-id="fc5fe-124">**Azure Key Vault Integration**</span></span> |<span data-ttu-id="fc5fe-125">Lehetővé teszi, hogy tooautomatically telepítse és konfigurálja az Azure Key Vault az az SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-125">Enables you tooautomatically install and configure Azure Key Vault on your SQL Server VM.</span></span> <span data-ttu-id="fc5fe-126">További információkért lásd: [konfigurálása Azure Key Vault-integráció az SQL Server Azure virtuális gépeken (klasszikus)](../classic/ps-sql-keyvault.md).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-126">For more information, see [Configure Azure Key Vault Integration for SQL Server on Azure VMs (Classic)](../classic/ps-sql-keyvault.md).</span></span> |

## <a name="prerequisites"></a><span data-ttu-id="fc5fe-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fc5fe-127">Prerequisites</span></span>
<span data-ttu-id="fc5fe-128">Követelmények toouse hello SQL Server IaaS ügynök bővítményt a virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="fc5fe-128">Requirements toouse hello SQL Server IaaS Agent Extension on your VM:</span></span>

### <a name="operating-system"></a><span data-ttu-id="fc5fe-129">Operációs rendszer:</span><span class="sxs-lookup"><span data-stu-id="fc5fe-129">Operating System:</span></span>
* <span data-ttu-id="fc5fe-130">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="fc5fe-130">Windows Server 2012</span></span>
* <span data-ttu-id="fc5fe-131">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="fc5fe-131">Windows Server 2012 R2</span></span>
* <span data-ttu-id="fc5fe-132">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="fc5fe-132">Windows Server 2016</span></span>

### <a name="sql-server-versions"></a><span data-ttu-id="fc5fe-133">SQL Server-verziók:</span><span class="sxs-lookup"><span data-stu-id="fc5fe-133">SQL Server versions:</span></span>
* <span data-ttu-id="fc5fe-134">SQL Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="fc5fe-134">SQL Server 2012</span></span>
* <span data-ttu-id="fc5fe-135">SQL Server 2014</span><span class="sxs-lookup"><span data-stu-id="fc5fe-135">SQL Server 2014</span></span>
* <span data-ttu-id="fc5fe-136">SQL Server 2016</span><span class="sxs-lookup"><span data-stu-id="fc5fe-136">SQL Server 2016</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="fc5fe-137">Az Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="fc5fe-137">Azure PowerShell:</span></span>
<span data-ttu-id="fc5fe-138">[Töltse le és konfigurálja a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-138">[Download and configure hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>

<span data-ttu-id="fc5fe-139">Indítsa el a Windows Powershellt, valamint elérheti az Azure-előfizetés tooyour hello **Add-AzureAccount** parancsot.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-139">Start Windows PowerShell, and connect it tooyour Azure subscription with hello **Add-AzureAccount** command.</span></span>

    Add-AzureAccount

<span data-ttu-id="fc5fe-140">Ha több előfizetéssel rendelkezik, **válasszon-AzureSubscription** tooselect hello előfizetés, amely tartalmazza a cél klasszikus virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-140">If you have multiple subscriptions, use **Select-AzureSubscription** tooselect hello subscription that contains your target classic VM.</span></span>

    Select-AzureSubscription -SubscriptionName <subscriptionname>

<span data-ttu-id="fc5fe-141">Ezen a ponton kaphat a hello klasszikus virtuális gépek és a társított szolgáltatás nevüket hello **Get-AzureVM** parancsot.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-141">At this point, you can get a list of hello classic virtual machines and their associated service names with hello **Get-AzureVM** command.</span></span>

    Get-AzureVM

## <a name="installation"></a><span data-ttu-id="fc5fe-142">Telepítés</span><span class="sxs-lookup"><span data-stu-id="fc5fe-142">Installation</span></span>
<span data-ttu-id="fc5fe-143">Klasszikus virtuális gépekhez PowerShell tooinstall hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény használatára és annak társított szolgáltatásaihoz konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-143">For classic VMs, you must use PowerShell tooinstall hello SQL Server IaaS Agent Extension and configure its associated services.</span></span> <span data-ttu-id="fc5fe-144">Használjon hello **Set-AzureVMSqlServerExtension** PowerShell parancsmag tooinstall hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-144">Use hello **Set-AzureVMSqlServerExtension** PowerShell cmdlet tooinstall hello extension.</span></span> <span data-ttu-id="fc5fe-145">Például a következő parancs hello hello bővítmény telepítését a Windows Server virtuális gép (klasszikus) és ennek a "SQLIaaSExtension".</span><span class="sxs-lookup"><span data-stu-id="fc5fe-145">For example, hello following command installs hello extension on a Windows Server VM (classic) and names it "SQLIaaSExtension".</span></span>

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

<span data-ttu-id="fc5fe-146">Ha toohello hello SQL infrastruktúra-szolgáltatási ügynök bővítmény legújabb verziójára frissíti, hello-bővítmény frissítése után újra kell indítania a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-146">If you update toohello latest version of hello SQL IaaS Agent Extension, you must restart your virtual machine after updating hello extension.</span></span>

> [!NOTE]
> <span data-ttu-id="fc5fe-147">Klasszikus virtuális gépek nem egy beállítás tooinstall és hello portálon keresztül SQL infrastruktúra-szolgáltatási ügynök bővítmény hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-147">Classic virtual machines do not have an option tooinstall and configure hello SQL IaaS Agent Extension through hello portal.</span></span>
> 
> 

## <a name="status"></a><span data-ttu-id="fc5fe-148">status</span><span class="sxs-lookup"><span data-stu-id="fc5fe-148">Status</span></span>
<span data-ttu-id="fc5fe-149">Egyirányú tooverify, hogy telepítve van-e a hello bővítmény hello Azure Portal tooview hello ügynök állapotát.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-149">One way tooverify that hello extension is installed is tooview hello agent status in hello Azure Portal.</span></span> <span data-ttu-id="fc5fe-150">Válasszon ki egy virtuális gépet felsorolt hello virtuális gépek paneljét, és kattintson a **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-150">Select a virtual machine listed in hello virtual machine blade, and then click on **Extensions**.</span></span> <span data-ttu-id="fc5fe-151">Megtekintheti az hello **SQLIaaSAgent** felsorolt bővítmény.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-151">You should see hello **SQLIaaSAgent** extension listed.</span></span>

![SQL Server IaaS-ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

<span data-ttu-id="fc5fe-153">Is használhatja a hello **Get-AzureVMSqlServerExtension** Azure Powershell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-153">You can also use hello **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a><span data-ttu-id="fc5fe-154">Eltávolítás</span><span class="sxs-lookup"><span data-stu-id="fc5fe-154">Removal</span></span>
<span data-ttu-id="fc5fe-155">Hello Azure portál, eltávolíthatja hello bővítmény hello három pont a hello kattintva **bővítmények** panelen található a virtuálisgép-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-155">In hello Azure Portal, you can uninstall hello extension by clicking hello ellipsis on hello **Extensions** blade of your virtual machine properties.</span></span> <span data-ttu-id="fc5fe-156">Kattintson a **Eltávolítás**.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-156">Then click **Uninstall**.</span></span>

![Távolítsa el a hello SQL Server infrastruktúra-szolgáltatási ügynök bővítmény Azure-portálon](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

<span data-ttu-id="fc5fe-158">Is használhatja a hello **Remove-AzureVMSqlServerExtension** Powershell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-158">You can also use hello **Remove-AzureVMSqlServerExtension** Powershell cmdlet.</span></span>

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="fc5fe-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fc5fe-159">Next Steps</span></span>
<span data-ttu-id="fc5fe-160">Hello bővítmény által támogatott hello szolgáltatást használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-160">Begin using one of hello services supported by hello extension.</span></span> <span data-ttu-id="fc5fe-161">További részletekért lásd: hello hivatkozott hello témakörök [támogatott szolgáltatások](#supported-services) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="fc5fe-161">For more details, see hello topics referenced in hello [Supported services](#supported-services) section of this article.</span></span>

<span data-ttu-id="fc5fe-162">További információ az SQL Servert futtató Azure virtuális gépeken: [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fc5fe-162">For more information about running SQL Server on Azure Virtual Machines, see [SQL Server on Azure Virtual Machines overview](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>

