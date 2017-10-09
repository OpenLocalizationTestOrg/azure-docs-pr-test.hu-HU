---
title: "Azure virtuális gépeken Server FCI - aaaSQL |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan toocreate SQL Server feladatátvevő fürt példány Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bee3b27805c5f6cc02a43b25d480c129c254cb90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a><span data-ttu-id="b3a74-103">Azure virtuális gépeken futó SQL Server-példány feladatátvevő fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b3a74-103">Configure SQL Server Failover Cluster Instance on Azure Virtual Machines</span></span>

<span data-ttu-id="b3a74-104">Ez a cikk azt ismerteti, hogyan toocreate SQL Server feladatátvevő fürt-példány (FCI) Resource Manager modellt az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="b3a74-104">This article explains how toocreate a SQL Server Failover Cluster Instance (FCI) on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="b3a74-105">Ez a megoldás használ [közvetlen tárolóhelyek a Windows Server 2016 Datacenter edition \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) szoftveres virtuális TÁROLÓHÁLÓZATTAL, amely szinkronizálja hello storage (az adatlemezek) (Azure virtuális gépeken) csomópontjai hello közötti, egy Windows-fürt.</span><span class="sxs-lookup"><span data-stu-id="b3a74-105">This solution uses [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) as a software-based virtual SAN that synchronizes hello storage (data disks) between hello nodes (Azure VMs) in a Windows Cluster.</span></span> <span data-ttu-id="b3a74-106">S2D 's new in Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="b3a74-106">S2D is new in Windows Server 2016.</span></span>

<span data-ttu-id="b3a74-107">hello alábbi ábrán látható hello teljes megoldás az Azure virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="b3a74-107">hello following diagram shows hello complete solution on Azure virtual machines:</span></span>

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

<span data-ttu-id="b3a74-109">hello a fenti ábrán látható:</span><span class="sxs-lookup"><span data-stu-id="b3a74-109">hello preceding diagram shows:</span></span>

- <span data-ttu-id="b3a74-110">Két Azure virtuális gépeken a Windows feladatátvevő fürtben.</span><span class="sxs-lookup"><span data-stu-id="b3a74-110">Two Azure virtual machines in a Windows Failover Cluster.</span></span> <span data-ttu-id="b3a74-111">Ha egy virtuális gép feladatátvevő fürtben is nevezzük egy *fürtcsomópont*, vagy *csomópontok*.</span><span class="sxs-lookup"><span data-stu-id="b3a74-111">When a virtual machine is in a failover cluster it is also called a *cluster node*, or *nodes*.</span></span>
- <span data-ttu-id="b3a74-112">Minden virtuális gép két vagy több adatlemezek van.</span><span class="sxs-lookup"><span data-stu-id="b3a74-112">Each virtual machine has two or more data disks.</span></span>
- <span data-ttu-id="b3a74-113">S2D hello adatlemez hello adatokat szinkronizálja, és megadja a szinkronizált hello adattároló mint egy tárolókészletet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-113">S2D synchronizes hello data on hello data disk and presents hello synchronized storage as a storage pool.</span></span>
- <span data-ttu-id="b3a74-114">hello tárolókészlet feladatátvevő fürt fürt megosztott kötet (CSV) toohello mutatja be.</span><span class="sxs-lookup"><span data-stu-id="b3a74-114">hello storage pool presents a cluster shared volume (CSV) toohello failover cluster.</span></span>
- <span data-ttu-id="b3a74-115">SQL Server FCI fürtszerepkör hello hello CSV hello adatmeghajtók használ.</span><span class="sxs-lookup"><span data-stu-id="b3a74-115">hello SQL Server FCI cluster role uses hello CSV for hello data drives.</span></span>
- <span data-ttu-id="b3a74-116">Azure load balancer toohold hello IP-címet az SQL Server FCI hello.</span><span class="sxs-lookup"><span data-stu-id="b3a74-116">An Azure load balancer toohold hello IP address for hello SQL Server FCI.</span></span>
- <span data-ttu-id="b3a74-117">Az Azure rendelkezésre állási csoport összes hello erőforrásokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3a74-117">An Azure availability set holds all hello resources.</span></span>

   >[!NOTE]
   ><span data-ttu-id="b3a74-118">Az összes Azure-erőforrások hello ábrán vannak a hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b3a74-118">All Azure resources are in hello diagram are in hello same resource group.</span></span>

<span data-ttu-id="b3a74-119">S2D kapcsolatos részletekért lásd: [közvetlen tárolóhelyek a Windows Server 2016 Datacenter edition \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span><span class="sxs-lookup"><span data-stu-id="b3a74-119">For details about S2D, see [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span></span>

<span data-ttu-id="b3a74-120">S2D kétféle típusú architektúrák - átszervezett és többszörösen összevont támogatja.</span><span class="sxs-lookup"><span data-stu-id="b3a74-120">S2D supports two types of architectures - converged and hyper-converged.</span></span> <span data-ttu-id="b3a74-121">Ez a dokumentum hello architektúrájáról többszörösen összevont.</span><span class="sxs-lookup"><span data-stu-id="b3a74-121">hello architecture in this document is hyper-converged.</span></span> <span data-ttu-id="b3a74-122">A többszörösen összevont infrastruktúra helyek hello tárolási hello ugyanazokat a kiszolgálókat, hogy fürtözött hello gazdaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b3a74-122">A hyper-converged infrastructure places hello storage on hello same servers that host hello clustered application.</span></span> <span data-ttu-id="b3a74-123">Ebben az architektúrában hello tárhelyre van, minden egyes SQL Server FCI csomóponton.</span><span class="sxs-lookup"><span data-stu-id="b3a74-123">In this architecture, hello storage is on each SQL Server FCI node.</span></span>

### <a name="example-azure-template"></a><span data-ttu-id="b3a74-124">Példa Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="b3a74-124">Example Azure template</span></span>

<span data-ttu-id="b3a74-125">Az Azure-ban hello teljes megoldás létrehozhat egy sablont.</span><span class="sxs-lookup"><span data-stu-id="b3a74-125">You can create hello entire solution in Azure from a template.</span></span> <span data-ttu-id="b3a74-126">A sablon példa, hogy elérhető a Githubon hello [Azure gyors üzembe helyezési sablonokat](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span><span class="sxs-lookup"><span data-stu-id="b3a74-126">An example of a template is available in hello GitHub [Azure Quickstart Templates](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span></span> <span data-ttu-id="b3a74-127">Ebben a példában nem tervezett, vagy bármely adott munkaterhelés tesztelve.</span><span class="sxs-lookup"><span data-stu-id="b3a74-127">This example is not designed or tested for any specific workload.</span></span> <span data-ttu-id="b3a74-128">Futtathatja hello sablon toocreate egy SQL Server FCI S2D tárolási csatlakoztatott tooyour tartománnyal.</span><span class="sxs-lookup"><span data-stu-id="b3a74-128">You can run hello template toocreate a SQL Server FCI with S2D storage connected tooyour domain.</span></span> <span data-ttu-id="b3a74-129">Hello sablon értékelje ki, és módosítsa a célokra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-129">You can evaluate hello template, and modify it for your purposes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b3a74-130">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b3a74-130">Before you begin</span></span>

<span data-ttu-id="b3a74-131">Néhány dolgot kell tooknow és a különböző dolgok állíthatók, amelyekre szüksége van érvényben, a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="b3a74-131">There are a few things you need tooknow and a couple of things that you need in place before you proceed.</span></span>

### <a name="what-tooknow"></a><span data-ttu-id="b3a74-132">Milyen tooknow</span><span class="sxs-lookup"><span data-stu-id="b3a74-132">What tooknow</span></span>
<span data-ttu-id="b3a74-133">A következő technológiák hello működési ismeretét kell rendelkezniük:</span><span class="sxs-lookup"><span data-stu-id="b3a74-133">You should have an operational understanding of hello following technologies:</span></span>

- [<span data-ttu-id="b3a74-134">Windows-fürt technológiák</span><span class="sxs-lookup"><span data-stu-id="b3a74-134">Windows cluster technologies</span></span>](http://technet.microsoft.com/library/hh831579.aspx)
-  <span data-ttu-id="b3a74-135">[SQL Server feladatátvevő fürt példányok](http://msdn.microsoft.com/library/ms189134.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3a74-135">[SQL Server Failover Cluster Instances](http://msdn.microsoft.com/library/ms189134.aspx).</span></span>

<span data-ttu-id="b3a74-136">Emellett rendelkeznie kell egy általános ismeretekkel rendelkezik a következő technológiák hello:</span><span class="sxs-lookup"><span data-stu-id="b3a74-136">Also, you should have a general understanding of hello following technologies:</span></span>

- [<span data-ttu-id="b3a74-137">A Windows Server 2016 közvetlen tárolóhelyek használatával Hyper átszervezett megoldás</span><span class="sxs-lookup"><span data-stu-id="b3a74-137">Hyper-converged solution using Storage Spaces Direct in Windows Server 2016</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [<span data-ttu-id="b3a74-138">Azure erőforráscsoport-sablonok</span><span class="sxs-lookup"><span data-stu-id="b3a74-138">Azure resource groups</span></span>](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-toohave"></a><span data-ttu-id="b3a74-139">Milyen toohave</span><span class="sxs-lookup"><span data-stu-id="b3a74-139">What toohave</span></span>

<span data-ttu-id="b3a74-140">Hello jelen cikkben lévő utasítások követése, előtt már rendelkeznie kell:</span><span class="sxs-lookup"><span data-stu-id="b3a74-140">Before following hello instructions in this article, you should already have:</span></span>

- <span data-ttu-id="b3a74-141">A Microsoft Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b3a74-141">A Microsoft Azure subscription.</span></span>
- <span data-ttu-id="b3a74-142">Azure virtuális gépeken futó Windows-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="b3a74-142">A Windows domain on Azure virtual machines.</span></span>
- <span data-ttu-id="b3a74-143">Engedély toocreate objektumokkal hello Azure virtuális gép a fiók.</span><span class="sxs-lookup"><span data-stu-id="b3a74-143">An account with permission toocreate objects in hello Azure virtual machine.</span></span>
- <span data-ttu-id="b3a74-144">Egy Azure-beli virtuális hálózat és alhálózat a következő összetevők hello megfelelő IP-címtér:</span><span class="sxs-lookup"><span data-stu-id="b3a74-144">An Azure virtual network and subnet with sufficient IP address space for hello following components:</span></span>
   - <span data-ttu-id="b3a74-145">Mindkét virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b3a74-145">Both virtual machines.</span></span>
   - <span data-ttu-id="b3a74-146">hello feladatátvételi fürt IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-146">hello failover cluster IP address.</span></span>
   - <span data-ttu-id="b3a74-147">Minden egyes FCI IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-147">An IP address for each FCI.</span></span>
- <span data-ttu-id="b3a74-148">A DNS-konfigurált hello Azure hálózati toohello tartományvezérlők mutat.</span><span class="sxs-lookup"><span data-stu-id="b3a74-148">DNS configured on hello Azure Network, pointing toohello domain controllers.</span></span>

<span data-ttu-id="b3a74-149">Az előfeltételek teljesülnek folytassa a a feladatátvevő fürt.</span><span class="sxs-lookup"><span data-stu-id="b3a74-149">With these prerequisites in place, you can proceed with building your failover cluster.</span></span> <span data-ttu-id="b3a74-150">első lépés hello toocreate hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="b3a74-150">hello first step is toocreate hello virtual machines.</span></span>

## <a name="step-1-create-virtual-machines"></a><span data-ttu-id="b3a74-151">1. lépés: Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-151">Step 1: Create virtual machines</span></span>

1. <span data-ttu-id="b3a74-152">Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com) az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b3a74-152">Log in toohello [Azure portal](http://portal.azure.com) with your subscription.</span></span>

1. <span data-ttu-id="b3a74-153">[Egy Azure rendelkezésre állási csoport létrehozása](../tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="b3a74-153">[Create an Azure availability set](../tutorial-availability-sets.md).</span></span>

   <span data-ttu-id="b3a74-154">hello rendelkezésre állási csoportok virtuális gépek be tartalék tartományokban, és a tartományok frissítése.</span><span class="sxs-lookup"><span data-stu-id="b3a74-154">hello availability set groups virtual machines across fault domains and update domains.</span></span> <span data-ttu-id="b3a74-155">hello rendelkezésre állási csoport gondoskodik arról, hogy az alkalmazás egyetlen ponton felmerülő hibákat, például hello hálózati kapcsoló vagy hello power egysége kiszolgálók állvány nincs hatással.</span><span class="sxs-lookup"><span data-stu-id="b3a74-155">hello availability set makes sure that your application is not affected by single points of failure, like hello network switch or hello power unit of a rack of servers.</span></span>

   <span data-ttu-id="b3a74-156">Ha még nem hozott hello erőforráscsoportot a virtuális gépek, elvégezhető az Azure rendelkezésre állási csoport létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b3a74-156">If you have not created hello resource group for your virtual machines, do it when you create an Azure availability set.</span></span> <span data-ttu-id="b3a74-157">Hello Azure portál toocreate hello rendelkezésre állási csoport használata, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b3a74-157">If you're using hello Azure portal toocreate hello availability set, do hello following steps:</span></span>

   - <span data-ttu-id="b3a74-158">Hello Azure-portálon, kattintson  **+**  tooopen hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="b3a74-158">In hello Azure portal, click **+** tooopen hello Azure Marketplace.</span></span> <span data-ttu-id="b3a74-159">Keresse meg **rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-159">Search for **Availability set**.</span></span>
   - <span data-ttu-id="b3a74-160">Kattintson a **rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-160">Click **Availability set**.</span></span>
   - <span data-ttu-id="b3a74-161">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-161">Click **Create**.</span></span>
   - <span data-ttu-id="b3a74-162">A hello **rendelkezésre állási csoport létrehozása** panelen, a következő értékek set hello:</span><span class="sxs-lookup"><span data-stu-id="b3a74-162">On hello **Create availability set** blade, set hello following values:</span></span>
      - <span data-ttu-id="b3a74-163">**Név**: hello rendelkezésre állási csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-163">**Name**: A name for hello availability set.</span></span>
      - <span data-ttu-id="b3a74-164">**Előfizetés**: az Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b3a74-164">**Subscription**: Your Azure subscription.</span></span>
      - <span data-ttu-id="b3a74-165">**Erőforráscsoport**: toouse egy meglévő csoporthoz, kattintson a **meglévő** és hello legördülő listából válassza hello csoport.</span><span class="sxs-lookup"><span data-stu-id="b3a74-165">**Resource group**: If you want toouse an existing group, click **Use existing** and select hello group from hello drop-down list.</span></span> <span data-ttu-id="b3a74-166">Máskülönben válassza **hozzon létre új** , és írja be a hello csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-166">Otherwise choose **Create New** and type a name for hello group.</span></span>
      - <span data-ttu-id="b3a74-167">**Hely**: állítsa be a hello helyet, ahol toocreate tervezi a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="b3a74-167">**Location**: Set hello location where you plan toocreate your virtual machines.</span></span>
      - <span data-ttu-id="b3a74-168">**Tartományok fault**: hello alapértelmezett (3) használja.</span><span class="sxs-lookup"><span data-stu-id="b3a74-168">**Fault domains**: Use hello default (3).</span></span>
      - <span data-ttu-id="b3a74-169">**Tartományok frissítése**: hello alapértelmezett (5) használja.</span><span class="sxs-lookup"><span data-stu-id="b3a74-169">**Update domains**: Use hello default (5).</span></span>
   - <span data-ttu-id="b3a74-170">Kattintson a **létrehozása** toocreate hello rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-170">Click **Create** toocreate hello availability set.</span></span>

1. <span data-ttu-id="b3a74-171">Hozzon létre virtuális gépek hello hello rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="b3a74-171">Create hello virtual machines in hello availability set.</span></span>

   <span data-ttu-id="b3a74-172">Hello Azure rendelkezésre állási készlet két SQL Server virtuális gép kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3a74-172">Provision two SQL Server virtual machines in hello Azure availability set.</span></span> <span data-ttu-id="b3a74-173">Útmutatásért lásd: [egy SQL Server rendszerű virtuális gép az Azure-portálon hello](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="b3a74-173">For instructions, see [Provision a SQL Server virtual machine in hello Azure portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

   <span data-ttu-id="b3a74-174">Mindkét virtuális gép helye:</span><span class="sxs-lookup"><span data-stu-id="b3a74-174">Place both virtual machines:</span></span>

   - <span data-ttu-id="b3a74-175">A hello azonos Azure erőforráscsoport, amely a rendelkezésre állási csoportban van.</span><span class="sxs-lookup"><span data-stu-id="b3a74-175">In hello same Azure resource group that your availability set is in.</span></span>
   - <span data-ttu-id="b3a74-176">Hello azonos hálózati a tartományvezérlővel.</span><span class="sxs-lookup"><span data-stu-id="b3a74-176">On hello same network as your domain controller.</span></span>
   - <span data-ttu-id="b3a74-177">Az alhálózat IP-címterület elegendő a virtuális gépek és az összes példányoktól végül használhat ezen a fürtön.</span><span class="sxs-lookup"><span data-stu-id="b3a74-177">On a subnet with sufficient IP address space for both virtual machines, and all FCIs that you may eventually use on this cluster.</span></span>
   - <span data-ttu-id="b3a74-178">A hello Azure rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-178">In hello Azure availability set.</span></span>   

      >[!IMPORTANT]
      ><span data-ttu-id="b3a74-179">Nem állíthatók be vagy rendelkezésre állási csoport egy virtuális gép létrehozása után módosítsa.</span><span class="sxs-lookup"><span data-stu-id="b3a74-179">You cannot set or change availability set after a virtual machine has been created.</span></span>

   <span data-ttu-id="b3a74-180">Kép kiválasztása a hello Azure piactéren.</span><span class="sxs-lookup"><span data-stu-id="b3a74-180">Choose an image from hello Azure Marketplace.</span></span> <span data-ttu-id="b3a74-181">Használhatja a Piactéri lemezkép, amely tartalmazza a Windows Server és SQL Server, vagy csak hello Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b3a74-181">You can use a Marketplace image with that includes Windows Server and SQL Server, or just hello Windows Server.</span></span> <span data-ttu-id="b3a74-182">További információkért lásd: [áttekintése az SQL Server Azure virtuális gépeken](../../virtual-machines-windows-sql-server-iaas-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b3a74-182">For details, see [Overview of SQL Server on Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)</span></span>

   <span data-ttu-id="b3a74-183">hello hivatalos SQL Server-rendszerképeit hello Azure-katalógus tartalmazhatnak a telepített SQL Server-példányt, és hello SQL Server telepítési szoftver és hello szükséges kulcs.</span><span class="sxs-lookup"><span data-stu-id="b3a74-183">hello official SQL Server images in hello Azure Gallery include an installed SQL Server instance, plus hello SQL Server installation software, and hello required key.</span></span>

   <span data-ttu-id="b3a74-184">Kattintson a hello jobb kép kívánt toohow az SQL Server licence hello toopay szerint:</span><span class="sxs-lookup"><span data-stu-id="b3a74-184">Choose hello right image according toohow you want toopay for hello SQL Server license:</span></span>

   - <span data-ttu-id="b3a74-185">**Használati licencelési kell fizetnie**: hello perc költségét, ezeket a lemezképeket tartalmaz hello SQL Server licencelési:</span><span class="sxs-lookup"><span data-stu-id="b3a74-185">**Pay per usage licensing**: hello per-minute cost of these images includes hello SQL Server licensing:</span></span>
      - <span data-ttu-id="b3a74-186">**A Windows Server Datacenter 2016 SQL Server 2016 Enterprise**</span><span class="sxs-lookup"><span data-stu-id="b3a74-186">**SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="b3a74-187">**Az SQL Server 2016 Standard a Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="b3a74-187">**SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="b3a74-188">**SQL Server 2016 fejlesztői a Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="b3a74-188">**SQL Server 2016 Developer on Windows Server Datacenter 2016**</span></span>

   - <span data-ttu-id="b3a74-189">**Bring your-saját-licencet (BYOL)**</span><span class="sxs-lookup"><span data-stu-id="b3a74-189">**Bring-your-own-license (BYOL)**</span></span>

      - <span data-ttu-id="b3a74-190">**{BYOL} A Windows Server Datacenter 2016 SQL Server 2016 Enterprise**</span><span class="sxs-lookup"><span data-stu-id="b3a74-190">**{BYOL} SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="b3a74-191">**{BYOL} Az SQL Server 2016 Standard a Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="b3a74-191">**{BYOL} SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>

   >[!IMPORTANT]
   ><span data-ttu-id="b3a74-192">Hello virtuális gép létrehozása után távolítsa el a hello előre telepített önálló SQL Server-példányt.</span><span class="sxs-lookup"><span data-stu-id="b3a74-192">After you create hello virtual machine, remove hello pre-installed standalone SQL Server instance.</span></span> <span data-ttu-id="b3a74-193">Miután konfigurálta a hello feladatátvevő fürt és S2D hello előre telepített SQL Server media toocreate hello SQL Server FCI fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b3a74-193">You will use hello pre-installed SQL Server media toocreate hello SQL Server FCI after you configure hello failover cluster and S2D.</span></span>

   <span data-ttu-id="b3a74-194">Másik lehetőségként használata Azure piactéren elérhető rendszerkép csak hello operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="b3a74-194">Alternatively, you can use Azure Marketplace images with just hello operating system.</span></span> <span data-ttu-id="b3a74-195">Válasszon egy **Windows Server 2016 Datacenter** rendszerképet, SQL Server FCI hello telepítése hello feladatátvevő fürt és S2D konfigurálása után.</span><span class="sxs-lookup"><span data-stu-id="b3a74-195">Choose a **Windows Server 2016 Datacenter** image and install hello SQL Server FCI after you configure hello failover cluster and S2D.</span></span> <span data-ttu-id="b3a74-196">A kép nem tartalmaz az SQL Server telepítési adathordozóról.</span><span class="sxs-lookup"><span data-stu-id="b3a74-196">This image does not contain SQL Server installation media.</span></span> <span data-ttu-id="b3a74-197">Hello telepítési adathordozón egy SQL Server-telepítés hello kiszolgálónként futtató helyen helyezze el.</span><span class="sxs-lookup"><span data-stu-id="b3a74-197">Place hello installation media in a location where you can run hello SQL Server installation for each server.</span></span>

1. <span data-ttu-id="b3a74-198">Miután a virtuális gépeket az Azure létrehoz, csatlakoztassa tooeach virtuális gép RDP.</span><span class="sxs-lookup"><span data-stu-id="b3a74-198">After Azure creates your virtual machines, connect tooeach virtual machine with RDP.</span></span>

   <span data-ttu-id="b3a74-199">Amikor első alkalommal csatlakoztatja tooa virtuális gép RDP-, hello számítógép megkérdezi tooallow a PC-toobe felderíthető hello hálózaton.</span><span class="sxs-lookup"><span data-stu-id="b3a74-199">When you first connect tooa virtual machine with RDP, hello computer asks if you want tooallow this PC toobe discoverable on hello network.</span></span> <span data-ttu-id="b3a74-200">Kattintson a **Yes** (Igen) gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-200">Click **Yes**.</span></span>

1. <span data-ttu-id="b3a74-201">Ha egy hello SQL Server-alapú virtuális gépek lemezképet használ, távolítsa el a hello SQL Server-példány.</span><span class="sxs-lookup"><span data-stu-id="b3a74-201">If you are using one of hello SQL Server-based virtual machine images, remove hello SQL Server instance.</span></span>

   - <span data-ttu-id="b3a74-202">A **programok és szolgáltatások**, kattintson a jobb gombbal **Microsoft SQL Server 2016 (64 bites)** kattintson **Eltávolítás/módosítás**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-202">In **Programs and Features**, right-click **Microsoft SQL Server 2016 (64-bit)** and click **Uninstall/Change**.</span></span>
   - <span data-ttu-id="b3a74-203">Kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-203">Click **Remove**.</span></span>
   - <span data-ttu-id="b3a74-204">Válassza ki a hello alapértelmezett példány.</span><span class="sxs-lookup"><span data-stu-id="b3a74-204">Select hello default instance.</span></span>
   - <span data-ttu-id="b3a74-205">Távolítsa el az összes szolgáltatás **adatbázismotor-szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-205">Remove all features under **Database Engine Services**.</span></span> <span data-ttu-id="b3a74-206">Ne távolítsa el az **közös szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-206">Do not remove **Shared Features**.</span></span> <span data-ttu-id="b3a74-207">Tekintse meg az alábbi képen hello:</span><span class="sxs-lookup"><span data-stu-id="b3a74-207">See hello following picture:</span></span>

      ![Szolgáltatások eltávolítása](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - <span data-ttu-id="b3a74-209">Kattintson a **következő**, és kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-209">Click **Next**, and then click **Remove**.</span></span>

1. <span data-ttu-id="b3a74-210"><a name="ports"></a>Nyissa meg a hello tűzfal portjai.</span><span class="sxs-lookup"><span data-stu-id="b3a74-210"><a name="ports"></a>Open hello firewall ports.</span></span>

   <span data-ttu-id="b3a74-211">Minden egyes virtuális gépen nyissa meg a következő portokat a Windows tűzfal hello hello.</span><span class="sxs-lookup"><span data-stu-id="b3a74-211">On each virtual machine, open hello following ports on hello Windows Firewall.</span></span>

   | <span data-ttu-id="b3a74-212">Cél</span><span class="sxs-lookup"><span data-stu-id="b3a74-212">Purpose</span></span> | <span data-ttu-id="b3a74-213">TCP-Port</span><span class="sxs-lookup"><span data-stu-id="b3a74-213">TCP Port</span></span> | <span data-ttu-id="b3a74-214">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b3a74-214">Notes</span></span>
   | ------ | ------ | ------
   | <span data-ttu-id="b3a74-215">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b3a74-215">SQL Server</span></span> | <span data-ttu-id="b3a74-216">1433</span><span class="sxs-lookup"><span data-stu-id="b3a74-216">1433</span></span> | <span data-ttu-id="b3a74-217">Normál port az SQL Server alapértelmezett példánya esetében.</span><span class="sxs-lookup"><span data-stu-id="b3a74-217">Normal port for default instances of SQL Server.</span></span> <span data-ttu-id="b3a74-218">Ha lemezkép hello gyűjteményből, ezt a portot automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="b3a74-218">If you used an image from hello gallery, this port is automatically opened.</span></span>
   | <span data-ttu-id="b3a74-219">Állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="b3a74-219">Health probe</span></span> | <span data-ttu-id="b3a74-220">59999</span><span class="sxs-lookup"><span data-stu-id="b3a74-220">59999</span></span> | <span data-ttu-id="b3a74-221">Bármely nyitott TCP-portot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-221">Any open TCP port.</span></span> <span data-ttu-id="b3a74-222">Egy későbbi lépésben konfigurálja a terheléselosztó hello [állapotmintáihoz](#probe) és hello fürt toouse ezt a portot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-222">In a later step, configure hello load balancer [health probe](#probe) and hello cluster toouse this port.</span></span>  

1. <span data-ttu-id="b3a74-223">Adja hozzá a tárolási toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-223">Add storage toohello virtual machine.</span></span> <span data-ttu-id="b3a74-224">Részletes információkért lásd: [tároló](../../../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="b3a74-224">For detailed information, see [add storage](../../../storage/common/storage-premium-storage.md).</span></span>

   <span data-ttu-id="b3a74-225">Mindkét virtuális gépek legalább két lemez szükséges.</span><span class="sxs-lookup"><span data-stu-id="b3a74-225">Both virtual machines need at least two data disks.</span></span>

   <span data-ttu-id="b3a74-226">Csatlakoztassa a lemezeket nyers - nem NTFS formátumú lemezek.</span><span class="sxs-lookup"><span data-stu-id="b3a74-226">Attach raw disks - not NTFS formatted disks.</span></span>
      >[!NOTE]
      ><span data-ttu-id="b3a74-227">Ha NTFS fájlrendszerű lemezek csatlakoztat, nem szabad jogosultsági ellenőrzéssel csak engedélyezheti a S2D.</span><span class="sxs-lookup"><span data-stu-id="b3a74-227">If you attach NTFS-formatted disks, you can only enable S2D with no disk eligibility check.</span></span>  

   <span data-ttu-id="b3a74-228">Legalább két prémium szintű Storage (SSD lemezek) tooeach VM csatolni.</span><span class="sxs-lookup"><span data-stu-id="b3a74-228">Attach a minimum of two Premium Storage (SSD disks) tooeach VM.</span></span> <span data-ttu-id="b3a74-229">Ajánlott legalább P30 (1 TB) lemezek.</span><span class="sxs-lookup"><span data-stu-id="b3a74-229">We recommend at least P30 (1 TB) disks.</span></span>

   <span data-ttu-id="b3a74-230">Set-állomás gyorsítótárazását túl**írásvédett**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-230">Set host caching too**Read-only**.</span></span>

   <span data-ttu-id="b3a74-231">éles környezetben használhat hello tárolókapacitás attól függ, hogy a számítási feladatok.</span><span class="sxs-lookup"><span data-stu-id="b3a74-231">hello storage capacity you use in production environments depends on your workload.</span></span> <span data-ttu-id="b3a74-232">hello cikkben leírt értékei demonstrációs és tesztelésére.</span><span class="sxs-lookup"><span data-stu-id="b3a74-232">hello values described in this article are for demonstration and testing.</span></span>

1. <span data-ttu-id="b3a74-233">[Hello virtuális gépek tooyour már meglévő tartomány hozzáadása](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="b3a74-233">[Add hello virtual machines tooyour pre-existing domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

<span data-ttu-id="b3a74-234">Után hello virtuális gépek létrehozása és konfigurálva, konfigurálhat hello feladatátvevő fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b3a74-234">After hello virtual machines are created and configured, you can configure hello failover cluster.</span></span>

## <a name="step-2-configure-hello-windows-failover-cluster-with-s2d"></a><span data-ttu-id="b3a74-235">2. lépés: Feladatátvevő fürt Windows hello konfigurálása S2D</span><span class="sxs-lookup"><span data-stu-id="b3a74-235">Step 2: Configure hello Windows Failover Cluster with S2D</span></span>

<span data-ttu-id="b3a74-236">hello tovább S2D tooconfigure hello feladatátvevő fürtön.</span><span class="sxs-lookup"><span data-stu-id="b3a74-236">hello next step is tooconfigure hello failover cluster with S2D.</span></span> <span data-ttu-id="b3a74-237">Ebben a lépésben ennek következő részlépések hello:</span><span class="sxs-lookup"><span data-stu-id="b3a74-237">In this step, you will do hello following substeps:</span></span>

1. <span data-ttu-id="b3a74-238">Adja hozzá a Windows feladatátvételi fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3a74-238">Add Windows Failover Clustering feature</span></span>
1. <span data-ttu-id="b3a74-239">Hello fürt ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b3a74-239">Validate hello cluster</span></span>
1. <span data-ttu-id="b3a74-240">Hello feladatátvevő fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-240">Create hello failover cluster</span></span>
1. <span data-ttu-id="b3a74-241">Hello felhő tanúsító létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-241">Create hello cloud witness</span></span>
1. <span data-ttu-id="b3a74-242">Tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b3a74-242">Add storage</span></span>

### <a name="add-windows-failover-clustering-feature"></a><span data-ttu-id="b3a74-243">Adja hozzá a Windows feladatátvételi fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3a74-243">Add Windows Failover Clustering feature</span></span>

1. <span data-ttu-id="b3a74-244">toobegin, csatlakozás toohello első virtuális gép RDP-a helyi rendszergazdák tagja, amely rendelkezik engedélyekkel toocreate objektumok az Active Directory tartományi fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="b3a74-244">toobegin, connect toohello first virtual machine with RDP using a domain account that is a member of local administrators, and has permissions toocreate objects in Active Directory.</span></span> <span data-ttu-id="b3a74-245">Ezt a fiókot használják a hello részeinek hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="b3a74-245">Use this account for hello rest of hello configuration.</span></span>

1. <span data-ttu-id="b3a74-246">[Adja hozzá a Feladatátvételi fürtszolgáltatás funkció tooeach virtuális gép](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="b3a74-246">[Add Failover Clustering feature tooeach virtual machine](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

   <span data-ttu-id="b3a74-247">tooinstall Feladatátvételi fürtszolgáltatást a felhasználói felület, hello hello mindkét virtuális gépekre lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="b3a74-247">tooinstall Failover Clustering feature from hello UI, do hello following steps on both virtual machines.</span></span>
   - <span data-ttu-id="b3a74-248">A **Kiszolgálókezelő**, kattintson a **kezelése**, és kattintson a **szerepkörök és szolgáltatások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-248">In **Server Manager**, click **Manage**, and then click **Add Roles and Features**.</span></span>
   - <span data-ttu-id="b3a74-249">A **hozzáadása szerepkörök és szolgáltatások varázsló**, kattintson a **következő** amíg elér túl**szolgáltatások kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-249">In **Add Roles and Features Wizard**, click **Next** until you get too**Select Features**.</span></span>
   - <span data-ttu-id="b3a74-250">A **szolgáltatások kiválasztása**, kattintson a **feladatátvételi fürtszolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-250">In **Select Features**, click **Failover Clustering**.</span></span> <span data-ttu-id="b3a74-251">Az összes szükséges szolgáltatásokat és hello felügyeleti eszközök közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="b3a74-251">Include all required features and hello management tools.</span></span> <span data-ttu-id="b3a74-252">Kattintson a **szolgáltatások hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-252">Click **Add Features**.</span></span>
   - <span data-ttu-id="b3a74-253">Kattintson a **következő** majd **Befejezés** tooinstall hello szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="b3a74-253">Click **Next** and then click **Finish** tooinstall hello features.</span></span>

   <span data-ttu-id="b3a74-254">tooinstall hello Feladatátvételi fürtszolgáltatást a PowerShell-lel, futtassa a következő parancsfájl egy rendszergazda PowerShell-munkamenetben hello virtuális gépek egyik hello.</span><span class="sxs-lookup"><span data-stu-id="b3a74-254">tooinstall hello Failover Clustering feature with PowerShell, run hello following script from an administrator PowerShell session on one of hello virtual machines.</span></span>

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

<span data-ttu-id="b3a74-255">Referenciaként hello lépések követésével hello a 3. lépés [közvetlen tárolóhelyek használata a Windows Server 2016 Hyper átszervezett megoldás](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="b3a74-255">For reference, hello next steps follow hello instructions under Step 3 of [Hyper-converged solution using Storage Spaces Direct in Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span></span>

### <a name="validate-hello-cluster"></a><span data-ttu-id="b3a74-256">Hello fürt ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b3a74-256">Validate hello cluster</span></span>

<span data-ttu-id="b3a74-257">Ez az útmutató alapján tooinstructions hivatkozik [fürt ellenőrzése](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span><span class="sxs-lookup"><span data-stu-id="b3a74-257">This guide refers tooinstructions under [validate cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span></span>

<span data-ttu-id="b3a74-258">Hello felhasználói felületén vagy a PowerShell-lel hello fürt ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b3a74-258">Validate hello cluster in hello UI or with PowerShell.</span></span>

<span data-ttu-id="b3a74-259">hello felhasználói felületén toovalidate hello fürt hello lépések hello virtuális gépek egyikét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-259">toovalidate hello cluster with hello UI, do hello following steps from one of hello virtual machines.</span></span>

1. <span data-ttu-id="b3a74-260">A **Kiszolgálókezelő**, kattintson a **eszközök**, majd kattintson a **Feladatátvevőfürt-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-260">In **Server Manager**, click **Tools**, then click **Failover Cluster Manager**.</span></span>
1. <span data-ttu-id="b3a74-261">A **Feladatátvevőfürt-kezelő**, kattintson a **művelet**, majd kattintson a **konfiguráció ellenőrzése...** .</span><span class="sxs-lookup"><span data-stu-id="b3a74-261">In **Failover Cluster Manager**, click **Action**, then click **Validate Configuration...**.</span></span>
1. <span data-ttu-id="b3a74-262">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-262">Click **Next**.</span></span>
1. <span data-ttu-id="b3a74-263">A **kiszolgálók kiválasztása vagy a fürt**, mindkét virtuális gép hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-263">On **Select Servers or a Cluster**, type hello name of both virtual machines.</span></span>
1. <span data-ttu-id="b3a74-264">A **tesztelési beállítások**, válassza a **csak a kijelölt tesztek futtatása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-264">On **Testing options**, choose **Run only tests I select**.</span></span> <span data-ttu-id="b3a74-265">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-265">Click **Next**.</span></span>
1. <span data-ttu-id="b3a74-266">A **kijelölés tesztelése**, következők kivételével az összes teszt **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-266">On **Test selection**, include all tests except **Storage**.</span></span> <span data-ttu-id="b3a74-267">Tekintse meg az alábbi képen hello:</span><span class="sxs-lookup"><span data-stu-id="b3a74-267">See hello following picture:</span></span>

   ![Tesztek ellenőrzése](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. <span data-ttu-id="b3a74-269">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-269">Click **Next**.</span></span>
1. <span data-ttu-id="b3a74-270">A **megerősítő**, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-270">On **Confirmation**, click **Next**.</span></span>

<span data-ttu-id="b3a74-271">Hello **konfiguráció ellenőrzése varázsló** futtatása hello ellenőrző tesztek.</span><span class="sxs-lookup"><span data-stu-id="b3a74-271">hello **Validate a Configuration Wizard** runs hello validation tests.</span></span>

<span data-ttu-id="b3a74-272">a PowerShell-lel, futtassa a következő parancsfájl egy rendszergazda PowerShell-munkamenetben hello virtuális gépek egyik hello toovalidate a hello fürt.</span><span class="sxs-lookup"><span data-stu-id="b3a74-272">toovalidate hello cluster with PowerShell, run hello following script from an administrator PowerShell session on one of hello virtual machines.</span></span>

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

<span data-ttu-id="b3a74-273">Miután hello fürt ellenőrzéséhez hello feladatátvevő fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-273">After you validate hello cluster, create hello failover cluster.</span></span>

### <a name="create-hello-failover-cluster"></a><span data-ttu-id="b3a74-274">Hello feladatátvevő fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-274">Create hello failover cluster</span></span>

<span data-ttu-id="b3a74-275">Ez az útmutató hivatkozik túl[hello feladatátvevő fürt létrehozása](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span><span class="sxs-lookup"><span data-stu-id="b3a74-275">This guide refers too[Create hello failover cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span></span>

<span data-ttu-id="b3a74-276">toocreate hello feladatátvevő fürt lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b3a74-276">toocreate hello failover cluster, you need:</span></span>
- <span data-ttu-id="b3a74-277">hello virtuális gépek fürtcsomópontok hello váló hello nevei.</span><span class="sxs-lookup"><span data-stu-id="b3a74-277">hello names of hello virtual machines that become hello cluster nodes.</span></span>
- <span data-ttu-id="b3a74-278">Hello feladatátvevő fürt nevét</span><span class="sxs-lookup"><span data-stu-id="b3a74-278">A name for hello failover cluster</span></span>
- <span data-ttu-id="b3a74-279">Hello feladatátvevő fürt IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-279">An IP address for hello failover cluster.</span></span> <span data-ttu-id="b3a74-280">Használhatja a hello nem használt IP-címet azonos Azure virtuális hálózat és alhálózat, a fürtcsomópontok hello.</span><span class="sxs-lookup"><span data-stu-id="b3a74-280">You can use an IP address that is not used on hello same Azure virtual network and subnet as hello cluster nodes.</span></span>

<span data-ttu-id="b3a74-281">a következő PowerShell hello egy feladatátvevő fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b3a74-281">hello following PowerShell creates a failover cluster.</span></span> <span data-ttu-id="b3a74-282">Hello parancsfájl hello nevekkel hello csomópontok (hello a virtuális gép neve) és egy szabad IP-cím hello Azure virtuális hálózat frissítése:</span><span class="sxs-lookup"><span data-stu-id="b3a74-282">Update hello script with hello names of hello nodes (hello virtual machine names) and an available IP address from hello Azure VNET:</span></span>

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a><span data-ttu-id="b3a74-283">A felhő tanú létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-283">Create a cloud witness</span></span>

<span data-ttu-id="b3a74-284">Felhő tanúsító is egy új fürt kvórum tanúsítójának tárolódnak az Azure Storage-Blobba.</span><span class="sxs-lookup"><span data-stu-id="b3a74-284">Cloud Witness is a new type of cluster quorum witness stored in an Azure Storage Blob.</span></span> <span data-ttu-id="b3a74-285">Ezzel eltávolítja a különálló virtuális gépek üzemeltetéséhez a tanúsító fájlmegosztás hello kell.</span><span class="sxs-lookup"><span data-stu-id="b3a74-285">This removes hello need of a separate VM hosting a witness share.</span></span>

1. <span data-ttu-id="b3a74-286">[Hozzon létre egy felhő hello feladatátvevő fürt tanúsító](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="b3a74-286">[Create a cloud witness for hello failover cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span>

1. <span data-ttu-id="b3a74-287">A blob-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-287">Create a blob container.</span></span>

1. <span data-ttu-id="b3a74-288">Hello hívóbetűk és hello tároló URL-cím mentése.</span><span class="sxs-lookup"><span data-stu-id="b3a74-288">Save hello access keys and hello container URL.</span></span>

1. <span data-ttu-id="b3a74-289">Hello feladatátvevő fürt fürt kvórum tanúsító konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-289">Configure hello failover cluster cluster quorum witness.</span></span> <span data-ttu-id="b3a74-290">Lásd például a [konfigurálása hello kvórumtanúsító hello felhasználói felületen]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) a felhasználói felület hello.</span><span class="sxs-lookup"><span data-stu-id="b3a74-290">See, [Configure hello quorum witness in hello user interface].(http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) in hello UI.</span></span>

### <a name="add-storage"></a><span data-ttu-id="b3a74-291">Tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b3a74-291">Add storage</span></span>

<span data-ttu-id="b3a74-292">S2D hello lemezeket kell toobe, üres, partíciók vagy egyéb adatok nélkül.</span><span class="sxs-lookup"><span data-stu-id="b3a74-292">hello disks for S2D need toobe empty and without partitions or other data.</span></span> <span data-ttu-id="b3a74-293">hajtsa végre a tooclean lemezek [hello lépések útmutatóban](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span><span class="sxs-lookup"><span data-stu-id="b3a74-293">tooclean disks follow [hello steps in this guide](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span></span>

1. <span data-ttu-id="b3a74-294">[Engedélyezési tároló közvetlen tárolóhelyek \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="b3a74-294">[Enable Store Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span></span>

   <span data-ttu-id="b3a74-295">a következő PowerShell hello lehetővé teszi, hogy a tárolóhelyek – közvetlen.</span><span class="sxs-lookup"><span data-stu-id="b3a74-295">hello following PowerShell enables storage spaces direct.</span></span>  

   ```PowerShell
   Enable-ClusterS2D
   ```

   <span data-ttu-id="b3a74-296">A **Feladatátvevőfürt-kezelő**, most már megtekintheti hello a tárolókészletben.</span><span class="sxs-lookup"><span data-stu-id="b3a74-296">In **Failover Cluster Manager**, you can now see hello storage pool.</span></span>

1. <span data-ttu-id="b3a74-297">[Hozzon létre egy kötetet](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span><span class="sxs-lookup"><span data-stu-id="b3a74-297">[Create a volume](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span></span>

   <span data-ttu-id="b3a74-298">Hello funkcióit S2D egyike, hogy automatikusan létrehoz egy tárolókészletet engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b3a74-298">One of hello features of S2D is that it automatically creates a storage pool when you enable it.</span></span> <span data-ttu-id="b3a74-299">Most már áll készen toocreate egy köteten.</span><span class="sxs-lookup"><span data-stu-id="b3a74-299">You are now ready toocreate a volume.</span></span> <span data-ttu-id="b3a74-300">PowerShell-parancsmag segítségével hello `New-Volume` automatizálja a hello kötet létrehozási folyamata, beleértve a formázás, toohello fürt hozzáadása, és a fürt megosztott kötetei (CSV) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-300">hello PowerShell commandlet `New-Volume` automates hello volume creation process, including formatting, adding toohello cluster, and creating a cluster shared volume (CSV).</span></span> <span data-ttu-id="b3a74-301">a következő példa hello egy 800 gigabájt (GB) CSV hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b3a74-301">hello following example creates an 800 gigabyte (GB) CSV.</span></span>

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   <span data-ttu-id="b3a74-302">Ez a parancs után egy 800 GB-os kötet fürterőforrásként csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="b3a74-302">After this command completes, an 800 GB volume is mounted as a cluster resource.</span></span> <span data-ttu-id="b3a74-303">hello kötet jelenleg `C:\ClusterStorage\Volume1\`.</span><span class="sxs-lookup"><span data-stu-id="b3a74-303">hello volume is at `C:\ClusterStorage\Volume1\`.</span></span>

   <span data-ttu-id="b3a74-304">a következő diagram hello jeleníti meg a fürt megosztott kötetei S2D rendelkező:</span><span class="sxs-lookup"><span data-stu-id="b3a74-304">hello following diagram shows a cluster shared volume with S2D:</span></span>

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a><span data-ttu-id="b3a74-306">3. lépés: Feladatátvevő fürtön belüli feladatátvétel tesztelése</span><span class="sxs-lookup"><span data-stu-id="b3a74-306">Step 3: Test failover cluster failover</span></span>

<span data-ttu-id="b3a74-307">A Feladatátvevőfürt-kezelőben, győződjön meg arról, hogy átvihető hello tárolási erőforrás toohello másik fürtcsomópontra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-307">In Failover Cluster Manager, verify that you can move hello storage resource toohello other cluster node.</span></span> <span data-ttu-id="b3a74-308">Ha a kapcsolódás toohello feladatátvevő fürt **Feladatátvevőfürt-kezelő** és hello tárolási áthelyezését más egy csomópont toohello, készen áll a tooconfigure hello FCI áll.</span><span class="sxs-lookup"><span data-stu-id="b3a74-308">If you can connect toohello failover cluster with **Failover Cluster Manager** and move hello storage from one node toohello other, you are ready tooconfigure hello FCI.</span></span>

## <a name="step-4-create-sql-server-fci"></a><span data-ttu-id="b3a74-309">4. lépés: Az SQL Server FCI létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-309">Step 4: Create SQL Server FCI</span></span>

<span data-ttu-id="b3a74-310">Miután konfigurálta a hello feladatátvevő fürt és a fürt-összetevők, beleértve a tárolási, SQL Server FCI hello hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b3a74-310">After you have configured hello failover cluster and all cluster components including storage, you can create hello SQL Server FCI.</span></span>

1. <span data-ttu-id="b3a74-311">Csatlakozás RDP toohello első virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="b3a74-311">Connect toohello first virtual machine with RDP.</span></span>

1. <span data-ttu-id="b3a74-312">A **Feladatátvevőfürt-kezelő**, győződjön meg arról, hogy minden fürt alapvető erőforrásai hello első virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="b3a74-312">In **Failover Cluster Manager**, make sure all cluster core resources are on hello first virtual machine.</span></span> <span data-ttu-id="b3a74-313">Ha szükséges, helyezze az összes erőforrások toothis virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-313">If necessary, move all resources toothis virtual machine.</span></span>

1. <span data-ttu-id="b3a74-314">Hello telepítési adathordozóján található.</span><span class="sxs-lookup"><span data-stu-id="b3a74-314">Locate hello installation media.</span></span> <span data-ttu-id="b3a74-315">Virtuális gép hello hello Azure piactéren elérhető rendszerkép valamelyikét használja, ha hello media itt található: `C:\SQLServer_<version number>_Full`.</span><span class="sxs-lookup"><span data-stu-id="b3a74-315">If hello virtual machine uses one of hello Azure Marketplace images, hello media is located at `C:\SQLServer_<version number>_Full`.</span></span> <span data-ttu-id="b3a74-316">Kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-316">Click **Setup**.</span></span>

1. <span data-ttu-id="b3a74-317">A hello **SQL Server telepítési központjának**, kattintson a **telepítési**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-317">In hello **SQL Server Installation Center**, click **Installation**.</span></span>

1. <span data-ttu-id="b3a74-318">Kattintson a **új SQL Server feladatátvevő fürt telepítése**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-318">Click **New SQL Server failover cluster installation**.</span></span> <span data-ttu-id="b3a74-319">Hello varázsló tooinstall hello SQL Server FCI hello utasításait kövesse.</span><span class="sxs-lookup"><span data-stu-id="b3a74-319">Follow hello instructions in hello wizard tooinstall hello SQL Server FCI.</span></span>

   <span data-ttu-id="b3a74-320">hello FCI adatkönyvtárak kell toobe fürtözött tárhelyen.</span><span class="sxs-lookup"><span data-stu-id="b3a74-320">hello FCI data directories need toobe on clustered storage.</span></span> <span data-ttu-id="b3a74-321">A S2D már nem egy megosztott lemez, de a csatlakoztatási pont tooa kötet minden kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b3a74-321">With S2D, it's not a shared disk, but a mount point tooa volume on each server.</span></span> <span data-ttu-id="b3a74-322">S2D szinkronizálja hello kötet mindkét csomópont között.</span><span class="sxs-lookup"><span data-stu-id="b3a74-322">S2D synchronizes hello volume between both nodes.</span></span> <span data-ttu-id="b3a74-323">hello kötet toohello fürt jeleníti meg egy megosztott fürtkötethez.</span><span class="sxs-lookup"><span data-stu-id="b3a74-323">hello volume is presented toohello cluster as a cluster shared volume.</span></span> <span data-ttu-id="b3a74-324">Hello adatkönyvtárak hello CSV csatlakoztatási pont használatára.</span><span class="sxs-lookup"><span data-stu-id="b3a74-324">Use hello CSV mount point for hello data directories.</span></span>

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. <span data-ttu-id="b3a74-326">Hello varázsló befejezése után telepíti egy SQL Server FCI hello első csomóponton.</span><span class="sxs-lookup"><span data-stu-id="b3a74-326">After you complete hello wizard, Setup will install a SQL Server FCI on hello first node.</span></span>

1. <span data-ttu-id="b3a74-327">A telepítő sikeres telepítése után hello FCI hello első csomóponton csatlakozás toohello második csomópont RDP.</span><span class="sxs-lookup"><span data-stu-id="b3a74-327">After Setup successfully installs hello FCI on hello first node, connect toohello second node with RDP.</span></span>

1. <span data-ttu-id="b3a74-328">Nyissa meg hello **SQL Server telepítési központjának**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-328">Open hello **SQL Server Installation Center**.</span></span> <span data-ttu-id="b3a74-329">Kattintson a **telepítési**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-329">Click **Installation**.</span></span>

1. <span data-ttu-id="b3a74-330">Kattintson a **Hozzáadás csomópont tooa SQL Server feladatátvevő fürt**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-330">Click **Add node tooa SQL Server failover cluster**.</span></span> <span data-ttu-id="b3a74-331">Hello varázsló tooinstall SQL server hello utasításokat követve, majd adja meg a kiszolgáló toohello FCI.</span><span class="sxs-lookup"><span data-stu-id="b3a74-331">Follow hello instructions in hello wizard tooinstall SQL server and add this server toohello FCI.</span></span>

   >[!NOTE]
   ><span data-ttu-id="b3a74-332">Ha SQL Server Azure piactér gyűjtemény kép használt, SQL Server-eszközök szerepeltek hello lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="b3a74-332">If you used an Azure Marketplace gallery image with SQL Server, SQL Server tools were included with hello image.</span></span> <span data-ttu-id="b3a74-333">Ha nem használja ezt a képet, SQL Server-eszközök hello külön kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="b3a74-333">If you did not use this image, install hello SQL Server tools separately.</span></span> <span data-ttu-id="b3a74-334">Lásd: [töltse le az SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3a74-334">See [Download SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="step-5-create-azure-load-balancer"></a><span data-ttu-id="b3a74-335">5. lépés: Az Azure terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3a74-335">Step 5: Create Azure load balancer</span></span>

<span data-ttu-id="b3a74-336">Az Azure virtuális gépeken fürtök egy terheléselosztási terheléselosztó toohold egyszerre egy csomóponton toobe igénylő IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="b3a74-336">On Azure virtual machines, clusters use a load balancer toohold an IP address that needs toobe on one cluster node at a time.</span></span> <span data-ttu-id="b3a74-337">Ebben a megoldásban hello terheléselosztó hello SQL Server FCI hello IP-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3a74-337">In this solution, hello load balancer holds hello IP address for hello SQL Server FCI.</span></span>

<span data-ttu-id="b3a74-338">[Hozza létre és konfigurálja az Azure terheléselosztó](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="b3a74-338">[Create and configure an Azure load balancer](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

### <a name="create-hello-load-balancer-in-hello-azure-portal"></a><span data-ttu-id="b3a74-339">Hozzon létre hello terheléselosztó hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b3a74-339">Create hello load balancer in hello Azure portal</span></span>

<span data-ttu-id="b3a74-340">toocreate hello terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="b3a74-340">toocreate hello load balancer:</span></span>

1. <span data-ttu-id="b3a74-341">A hello Azure-portálon válassza a toohello erőforráscsoport hello virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="b3a74-341">In hello Azure portal, go toohello Resource Group with hello virtual machines.</span></span>

1. <span data-ttu-id="b3a74-342">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-342">Click **+ Add**.</span></span> <span data-ttu-id="b3a74-343">Keresési hello piactér a **terheléselosztó**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-343">Search hello Marketplace for **Load Balancer**.</span></span> <span data-ttu-id="b3a74-344">Kattintson a **terheléselosztó**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-344">Click **Load Balancer**.</span></span>

1. <span data-ttu-id="b3a74-345">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-345">Click **Create**.</span></span>

1. <span data-ttu-id="b3a74-346">Rendelkező terheléselosztó hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="b3a74-346">Configure hello load balancer with:</span></span>

   - <span data-ttu-id="b3a74-347">**Név**: hello terheléselosztó azonosító nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-347">**Name**: A name that identifies hello load balancer.</span></span>
   - <span data-ttu-id="b3a74-348">**Típus**: hello terheléselosztó nyilvános vagy titkos lehet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-348">**Type**: hello load balancer can be either public or private.</span></span> <span data-ttu-id="b3a74-349">Személyes terheléselosztó belül elérhető hello ugyanazt a virtuális Hálózatot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-349">A private load balancer can be accessed from within hello same VNET.</span></span> <span data-ttu-id="b3a74-350">Az Azure alkalmazások használhatják a saját terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="b3a74-350">Most Azure applications can use a private load balancer.</span></span> <span data-ttu-id="b3a74-351">Ha az alkalmazásnak hozzáféréssel tooSQL Server hello interneten keresztül közvetlenül, használjon egy nyilvános terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="b3a74-351">If your application needs access tooSQL Server directly over hello Internet, use a public load balancer.</span></span>
   - <span data-ttu-id="b3a74-352">**Virtuális hálózati**: hello azonos hálózati hello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="b3a74-352">**Virtual Network**: hello same network as hello virtual machines.</span></span>
   - <span data-ttu-id="b3a74-353">**Alhálózati**: hello hello virtuális gépek azonos alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="b3a74-353">**Subnet**: hello same subnet as hello virtual machines.</span></span>
   - <span data-ttu-id="b3a74-354">**Magánhálózati IP-cím**: hello toohello SQL Server FCI fürt hálózati erőforráshoz rendelt IP-cím.</span><span class="sxs-lookup"><span data-stu-id="b3a74-354">**Private IP address**: hello same IP address that you assigned toohello SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="b3a74-355">**előfizetés**: az Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b3a74-355">**subscription**: Your Azure subscription.</span></span>
   - <span data-ttu-id="b3a74-356">**Erőforráscsoport**: használata hello azonos erőforráscsoporthoz tartozik, mint a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b3a74-356">**Resource Group**: Use hello same resource group as your virtual machines.</span></span>
   - <span data-ttu-id="b3a74-357">**Hely**: használata hello ugyanaz a virtuális gépek Azure-beli helyhez.</span><span class="sxs-lookup"><span data-stu-id="b3a74-357">**Location**: Use hello same Azure location as your virtual machines.</span></span>
   <span data-ttu-id="b3a74-358">Tekintse meg az alábbi képen hello:</span><span class="sxs-lookup"><span data-stu-id="b3a74-358">See hello following picture:</span></span>

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-hello-load-balancer-backend-pool"></a><span data-ttu-id="b3a74-360">A terheléselosztó háttérkészletének hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b3a74-360">Configure hello load balancer backend pool</span></span>

1. <span data-ttu-id="b3a74-361">Térjen vissza az Azure erőforráscsoport toohello hello virtuális gépekkel, és keresse meg az új terheléselosztó hello.</span><span class="sxs-lookup"><span data-stu-id="b3a74-361">Return toohello Azure Resource Group with hello virtual machines and locate hello new load balancer.</span></span> <span data-ttu-id="b3a74-362">Előfordulhat, hogy toorefresh hello nézet a hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-362">You may have toorefresh hello view on hello Resource Group.</span></span> <span data-ttu-id="b3a74-363">Kattintson a hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b3a74-363">Click hello load balancer.</span></span>

1. <span data-ttu-id="b3a74-364">Hello load balancer paneljén kattintson **háttérkészletek**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-364">On hello load balancer blade, click **Backend pools**.</span></span>

1. <span data-ttu-id="b3a74-365">Kattintson a **+ Hozzáadás** tooadd háttérkészlet.</span><span class="sxs-lookup"><span data-stu-id="b3a74-365">Click **+ Add** tooadd a backend pool.</span></span>

1. <span data-ttu-id="b3a74-366">Írja be a hello háttérkészlet nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-366">Type a name for hello backend pool.</span></span>

1. <span data-ttu-id="b3a74-367">Kattintson a **adja hozzá a virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-367">Click **Add a virtual machine**.</span></span>

1. <span data-ttu-id="b3a74-368">A hello **válassza ki a virtuális gépek** panelen kattintson a **rendelkezésre állási csoport kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-368">On hello **Choose virtual machines** blade, click **Choose an availability set**.</span></span>

1. <span data-ttu-id="b3a74-369">Válassza ki a hello rendelkezésre állási csoportban, hogy a hello SQL Server virtuális gépeket helyez.</span><span class="sxs-lookup"><span data-stu-id="b3a74-369">Choose hello availability set that you placed hello SQL Server virtual machines in.</span></span>

1. <span data-ttu-id="b3a74-370">A hello **válassza ki a virtuális gépek** panelen kattintson a **válassza ki a virtuális gépek hello**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-370">On hello **Choose virtual machines** blade, click **Choose hello virtual machines**.</span></span>

   <span data-ttu-id="b3a74-371">Az Azure-portálon kép a következő hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="b3a74-371">Your Azure portal should look like hello following picture:</span></span>

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. <span data-ttu-id="b3a74-373">Kattintson a **válasszon** a hello **válassza ki a virtuális gépek** panelen.</span><span class="sxs-lookup"><span data-stu-id="b3a74-373">Click **Select** on hello **Choose virtual machines** blade.</span></span>

1. <span data-ttu-id="b3a74-374">Kattintson a **OK** kétszer.</span><span class="sxs-lookup"><span data-stu-id="b3a74-374">Click **OK** twice.</span></span>

### <a name="configure-a-load-balancer-health-probe"></a><span data-ttu-id="b3a74-375">Terheléselosztói állapotfigyelő mintavétel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b3a74-375">Configure a load balancer health probe</span></span>

1. <span data-ttu-id="b3a74-376">Hello load balancer paneljén kattintson **állapot-mintavételi csomagjai**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-376">On hello load balancer blade, click **Health probes**.</span></span>

1. <span data-ttu-id="b3a74-377">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-377">Click **+ Add**.</span></span>

1. <span data-ttu-id="b3a74-378">A hello **Hozzáadás állapotmintáihoz** panelen <a name="probe"> </a>hello állapotfigyelő mintavételi paraméterek beállítása:</span><span class="sxs-lookup"><span data-stu-id="b3a74-378">On hello **Add health probe** blade, <a name="probe"></a>Set hello health probe parameters:</span></span>

   - <span data-ttu-id="b3a74-379">**Név**: hello állapotmintáihoz nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-379">**Name**: A name for hello health probe.</span></span>
   - <span data-ttu-id="b3a74-380">**Protokoll**: TCP.</span><span class="sxs-lookup"><span data-stu-id="b3a74-380">**Protocol**: TCP.</span></span>
   - <span data-ttu-id="b3a74-381">**Port**: tooan elérhető TCP-port beállítása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-381">**Port**: Set tooan available TCP port.</span></span> <span data-ttu-id="b3a74-382">Ez a port egy megnyitott tűzfal portra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b3a74-382">This port requires an open firewall port.</span></span> <span data-ttu-id="b3a74-383">Használjon hello [ugyanazt a portot](#ports) hello tűzfalon hello állapotmintáihoz beállítása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-383">Use hello [same port](#ports) you set for hello health probe at hello firewall.</span></span>
   - <span data-ttu-id="b3a74-384">**Időköz**: 5 másodperc.</span><span class="sxs-lookup"><span data-stu-id="b3a74-384">**Interval**: 5 Seconds.</span></span>
   - <span data-ttu-id="b3a74-385">**Sérült küszöbérték**: 2 egymást követő hibák.</span><span class="sxs-lookup"><span data-stu-id="b3a74-385">**Unhealthy threshold**: 2 consecutive failures.</span></span>

1. <span data-ttu-id="b3a74-386">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-386">Click OK.</span></span>

### <a name="set-load-balancing-rules"></a><span data-ttu-id="b3a74-387">Terheléselosztási szabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="b3a74-387">Set load balancing rules</span></span>

1. <span data-ttu-id="b3a74-388">Hello load balancer paneljén kattintson **terheléselosztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-388">On hello load balancer blade, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="b3a74-389">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-389">Click **+ Add**.</span></span>

1. <span data-ttu-id="b3a74-390">Hello terhelés terheléselosztási szabályok paraméterek beállítása:</span><span class="sxs-lookup"><span data-stu-id="b3a74-390">Set hello load balancing rules parameters:</span></span>

   - <span data-ttu-id="b3a74-391">**Név**: hello terheléselosztási szabályok nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-391">**Name**: A name for hello load balancing rules.</span></span>
   - <span data-ttu-id="b3a74-392">**Előtérbeli IP-cím**: SQL Server FCI fürt hálózati erőforrás hello hello IP-címet használni.</span><span class="sxs-lookup"><span data-stu-id="b3a74-392">**Frontend IP address**: Use hello IP address for hello SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="b3a74-393">**Port**: hello SQL Server FCI TCP-port beállítása.</span><span class="sxs-lookup"><span data-stu-id="b3a74-393">**Port**: Set for hello SQL Server FCI TCP port.</span></span> <span data-ttu-id="b3a74-394">hello alapértelmezett példány portja az 1433-as.</span><span class="sxs-lookup"><span data-stu-id="b3a74-394">hello default instance port is 1433.</span></span>
   - <span data-ttu-id="b3a74-395">**A háttérportot**: Ez az érték hello ugyanazt a portot használja, mint a hello **Port** értéke, ha engedélyezi a **fix IP-Címek (közvetlen kiszolgálói válasz)**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-395">**Backend port**: This value uses hello same port as hello **Port** value when you enable **Floating IP (direct server return)**.</span></span>
   - <span data-ttu-id="b3a74-396">**Háttérkészlet**: használata hello háttér alkalmazáskészlet neve korábban megadott értékektől.</span><span class="sxs-lookup"><span data-stu-id="b3a74-396">**Backend pool**: Use hello backend pool name that you configured earlier.</span></span>
   - <span data-ttu-id="b3a74-397">**Állapotmintáihoz**: használata hello állapotmintáihoz korábban megadott értékektől.</span><span class="sxs-lookup"><span data-stu-id="b3a74-397">**Health probe**: Use hello health probe that you configured earlier.</span></span>
   - <span data-ttu-id="b3a74-398">**Munkamenet megőrzését**: nincs.</span><span class="sxs-lookup"><span data-stu-id="b3a74-398">**Session persistence**: None.</span></span>
   - <span data-ttu-id="b3a74-399">**Üresjárat időkorlátja (perc)**: 4.</span><span class="sxs-lookup"><span data-stu-id="b3a74-399">**Idle timeout (minutes)**: 4.</span></span>
   - <span data-ttu-id="b3a74-400">**Lebegőpontos IP (közvetlen kiszolgálói válasz)**: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="b3a74-400">**Floating IP (direct server return)**: Enabled</span></span>

1. <span data-ttu-id="b3a74-401">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b3a74-401">Click **OK**.</span></span>

## <a name="step-6-configure-cluster-for-probe"></a><span data-ttu-id="b3a74-402">6. lépés: A fürt mintavétel konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b3a74-402">Step 6: Configure cluster for probe</span></span>

<span data-ttu-id="b3a74-403">PowerShell hello fürt mintavételi portot paraméterrel állítható be.</span><span class="sxs-lookup"><span data-stu-id="b3a74-403">Set hello cluster probe port parameter in PowerShell.</span></span>

<span data-ttu-id="b3a74-404">tooset hello fürt mintavételi portot paraméter, a változók a parancsfájl a környezetben a következő hello frissíti.</span><span class="sxs-lookup"><span data-stu-id="b3a74-404">tooset hello cluster probe port parameter, update variables in hello following script from your environment.</span></span>

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "IP Address Resource Name" # hello IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a><span data-ttu-id="b3a74-405">: 7. lépés az FCI-feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="b3a74-405">Step 7: Test FCI failover</span></span>

<span data-ttu-id="b3a74-406">Teszt feladatátvétele hello FCI toovalidate fürt működését.</span><span class="sxs-lookup"><span data-stu-id="b3a74-406">Test failover of hello FCI toovalidate cluster functionality.</span></span> <span data-ttu-id="b3a74-407">Hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b3a74-407">Do hello following steps:</span></span>

1. <span data-ttu-id="b3a74-408">Csatlakozás RDP tooone hello SQL Server FCI-csomópont.</span><span class="sxs-lookup"><span data-stu-id="b3a74-408">Connect tooone of hello SQL Server FCI cluster nodes with RDP.</span></span>

1. <span data-ttu-id="b3a74-409">Nyissa meg **Feladatátvevőfürt-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-409">Open **Failover Cluster Manager**.</span></span> <span data-ttu-id="b3a74-410">Kattintson a **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-410">Click **Roles**.</span></span> <span data-ttu-id="b3a74-411">Figyelje meg, melyik csomóponton hello SQL Server FCI szerepkör.</span><span class="sxs-lookup"><span data-stu-id="b3a74-411">Notice which node owns hello SQL Server FCI role.</span></span>

1. <span data-ttu-id="b3a74-412">Kattintson a jobb gombbal hello SQL Server FCI szerepkör.</span><span class="sxs-lookup"><span data-stu-id="b3a74-412">Right-click hello SQL Server FCI role.</span></span>

1. <span data-ttu-id="b3a74-413">Kattintson a **áthelyezése** kattintson **lehető legalkalmasabb csomópontra**.</span><span class="sxs-lookup"><span data-stu-id="b3a74-413">Click **Move** and click **Best Possible Node**.</span></span>

<span data-ttu-id="b3a74-414">**Feladatátvevőfürt-kezelő** mutat be hello szerepkört és erőforrást kapcsolat nélküli módba.</span><span class="sxs-lookup"><span data-stu-id="b3a74-414">**Failover Cluster Manager** shows hello role and its resources go offline.</span></span> <span data-ttu-id="b3a74-415">hello erőforrások áthelyezése, majd online állapotba a hello a másik csomóponton.</span><span class="sxs-lookup"><span data-stu-id="b3a74-415">hello resources then move and come online on hello other node.</span></span>

### <a name="test-connectivity"></a><span data-ttu-id="b3a74-416">Kapcsolat tesztelése</span><span class="sxs-lookup"><span data-stu-id="b3a74-416">Test connectivity</span></span>

<span data-ttu-id="b3a74-417">tootest kapcsolatot, hello tooanother virtuális gépre való bejelentkezéshez ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-417">tootest connectivity, log in tooanother virtual machine in hello same virtual network.</span></span> <span data-ttu-id="b3a74-418">Nyissa meg **SQL Server Management Studio** , és csatlakozzon a toohello SQL Server FCI nevét.</span><span class="sxs-lookup"><span data-stu-id="b3a74-418">Open **SQL Server Management Studio** and connect toohello SQL Server FCI name.</span></span>

>[!NOTE]
><span data-ttu-id="b3a74-419">Ha szükséges, akkor [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3a74-419">If necessary, you can [download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="limitations"></a><span data-ttu-id="b3a74-420">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="b3a74-420">Limitations</span></span>
<span data-ttu-id="b3a74-421">Az Azure virtuális gépeken a Microsoft elosztott tranzakciók koordinátora (DTC) nem támogatott a példányoktól mert hello terheléselosztó által nem támogatott a hello RPC-portot.</span><span class="sxs-lookup"><span data-stu-id="b3a74-421">On Azure virtual machines, Microsoft Distributed Transaction Coordinator (DTC) is not supported on FCIs because hello RPC port is not supported by hello load balancer.</span></span>

## <a name="see-also"></a><span data-ttu-id="b3a74-422">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b3a74-422">See Also</span></span>

[<span data-ttu-id="b3a74-423">A telepítő S2D a távoli asztal (Azure)</span><span class="sxs-lookup"><span data-stu-id="b3a74-423">Setup S2D with remote desktop (Azure)</span></span>](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

<span data-ttu-id="b3a74-424">[Hyper-összevont megoldás a tárolóhelyek közvetlen](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="b3a74-424">[Hyper-converged solution with storage spaces direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span></span>

[<span data-ttu-id="b3a74-425">Közvetlen tárolóhelyek áttekintése</span><span class="sxs-lookup"><span data-stu-id="b3a74-425">Storage Space Direct Overview</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[<span data-ttu-id="b3a74-426">S2D SQL Server támogatása</span><span class="sxs-lookup"><span data-stu-id="b3a74-426">SQL Server support for S2D</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
