---
title: "Kiszolgáló rendelkezésre állási csoportok – Azure virtuális gépek - vész-helyreállítási aaaSQL |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooconfigure egy SQL Server rendelkezésre állási csoport és egy másik régióban egy replika Azure virtuális gépeken."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: df6238dc61c5a56879c75c9bf7314c618f43c0ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-always-on-availability-group-on-azure-virtual-machines-in-different-regions"></a><span data-ttu-id="ebea6-103">Always On rendelkezésre állási csoport konfigurálása az Azure virtuális gépeken különböző régiókban</span><span class="sxs-lookup"><span data-stu-id="ebea6-103">Configure an Always On availability group on Azure virtual machines in different regions</span></span>

<span data-ttu-id="ebea6-104">Ez a cikk azt ismerteti, hogyan tooconfigure egy SQL Server Always On rendelkezésre állási csoport replika Azure távolról Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="ebea6-104">This article explains how tooconfigure a SQL Server Always On availability group replica on Azure virtual machines in a remote Azure location.</span></span> <span data-ttu-id="ebea6-105">A konfigurációs toosupport katasztrófa utáni helyreállítás használata.</span><span class="sxs-lookup"><span data-stu-id="ebea6-105">Use this configuration toosupport disaster recovery.</span></span>

<span data-ttu-id="ebea6-106">Ez a cikk vonatkozik tooAzure virtuális gépek erőforrás-kezelő módban.</span><span class="sxs-lookup"><span data-stu-id="ebea6-106">This article applies tooAzure Virtual Machines in Resource Manager mode.</span></span>

<span data-ttu-id="ebea6-107">hello következő kép bemutatja egy rendelkezésre állási csoport gyakori telepítési Azure virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="ebea6-107">hello following image shows a common deployment of an availability group on Azure virtual machines:</span></span>

   ![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

<span data-ttu-id="ebea6-109">A központi telepítésben lévő összes virtuális gépet egy Azure-régiót szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="ebea6-109">In this deployment, all virtual machines are in one Azure region.</span></span> <span data-ttu-id="ebea6-110">hello rendelkezésre állási csoport replikái szinkron véglegesítésre SQL-1 és SQL-2 automatikus feladatátvétellel is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="ebea6-110">hello availability group replicas can have synchronous commit with automatic failover on SQL-1 and SQL-2.</span></span> <span data-ttu-id="ebea6-111">toobuild ebbe az architektúrába, lásd: [rendelkezésre állási csoport sablon vagy oktatóanyag](virtual-machines-windows-portal-sql-availability-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ebea6-111">toobuild this architecture, see [Availability Group template or tutorial](virtual-machines-windows-portal-sql-availability-group-overview.md).</span></span>

<span data-ttu-id="ebea6-112">Ez az architektúra sebezhető toodowntime esetén hello Azure-régió, elérhetetlenné válik.</span><span class="sxs-lookup"><span data-stu-id="ebea6-112">This architecture is vulnerable toodowntime if hello Azure region becomes inaccessible.</span></span> <span data-ttu-id="ebea6-113">tooovercome biztonsági rést, adja hozzá a replika egy másik Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="ebea6-113">tooovercome this vulnerability, add a replica in a different Azure region.</span></span> <span data-ttu-id="ebea6-114">a következő ábra azt mutatja be, hogyan hello új architektúra lenne hello:</span><span class="sxs-lookup"><span data-stu-id="ebea6-114">hello following diagram shows how hello new architecture would look:</span></span>

   ![Rendelkezésre állási csoport vész-Helyreállítási](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

<span data-ttu-id="ebea6-116">hello előző ábrán látható SQL-3 nevű új virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ebea6-116">hello preceding diagram shows a new virtual machine called SQL-3.</span></span> <span data-ttu-id="ebea6-117">SQL-3 egy másik Azure-régióban van.</span><span class="sxs-lookup"><span data-stu-id="ebea6-117">SQL-3 is in a different Azure region.</span></span> <span data-ttu-id="ebea6-118">SQL-3 toohello Windows Server feladatátvevő fürt kerül.</span><span class="sxs-lookup"><span data-stu-id="ebea6-118">SQL-3 is added toohello Windows Server Failover Cluster.</span></span> <span data-ttu-id="ebea6-119">SQL-3 rendelkezhet egy rendelkezésre állási csoportreplikákhoz.</span><span class="sxs-lookup"><span data-stu-id="ebea6-119">SQL-3 can host an availability group replica.</span></span> <span data-ttu-id="ebea6-120">Végül figyelje meg, hogy hello Azure-régió, az SQL-3 tartalmaz egy új Azure terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="ebea6-120">Finally, notice that hello Azure region for SQL-3 has a new Azure load balancer.</span></span>

>[!NOTE]
> <span data-ttu-id="ebea6-121">Egy Azure rendelkezésre állási csoportot, ha egynél több virtuális gép hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="ebea6-121">An Azure availability set is required when more than one virtual machine is in hello same region.</span></span> <span data-ttu-id="ebea6-122">Ha csak egy virtuális gép hello régióban van, majd hello rendelkezésre állási csoport nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="ebea6-122">If only one virtual machine is in hello region, then hello availability set is not required.</span></span> <span data-ttu-id="ebea6-123">Csak egy virtuális gép elhelyezheti rendelkezésre állási készlet létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="ebea6-123">You can only place a virtual machine in an availability set at creation time.</span></span> <span data-ttu-id="ebea6-124">Hello virtuális gép már a rendelkezésre állási csoportok, ha később további replika virtuális gép is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ebea6-124">If hello virtual machine is already in an availability set, you can add a virtual machine for an additional replica later.</span></span>

<span data-ttu-id="ebea6-125">Ebben az architektúrában hello replika hello távoli régióban általában konfigurálják, az aszinkron véglegesítésű rendelkezésre állási mód és a feladatátvételi módot kézire.</span><span class="sxs-lookup"><span data-stu-id="ebea6-125">In this architecture, hello replica in hello remote region is normally configured with asynchronous commit availability mode and manual failover mode.</span></span>

<span data-ttu-id="ebea6-126">Ha rendelkezésre állási csoport replikái az Azure virtuális gépek különböző Azure-régiókban, minden egyes régió van szükség:</span><span class="sxs-lookup"><span data-stu-id="ebea6-126">When availability group replicas are on Azure virtual machines in different Azure regions, each region requires:</span></span>

* <span data-ttu-id="ebea6-127">A virtuális hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="ebea6-127">A virtual network gateway</span></span>
* <span data-ttu-id="ebea6-128">A virtuális hálózati átjáró kapcsolat</span><span class="sxs-lookup"><span data-stu-id="ebea6-128">A virtual network gateway connection</span></span>

<span data-ttu-id="ebea6-129">hello alábbi ábrán látható hello hálózatok adatközpontok közötti kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="ebea6-129">hello following diagram shows how hello networks communicate between data centers.</span></span>

   ![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
><span data-ttu-id="ebea6-131">Ez az architektúra terhel kimenő adatok Azure-régiók közötti replikált adatokat.</span><span class="sxs-lookup"><span data-stu-id="ebea6-131">This architecture incurs outbound data charges for data replicated between Azure regions.</span></span> <span data-ttu-id="ebea6-132">Lásd: [sávszélesség árképzési](http://azure.microsoft.com/pricing/details/bandwidth/).</span><span class="sxs-lookup"><span data-stu-id="ebea6-132">See [Bandwidth Pricing](http://azure.microsoft.com/pricing/details/bandwidth/).</span></span>  

## <a name="create-remote-replica"></a><span data-ttu-id="ebea6-133">Távoli replika létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebea6-133">Create remote replica</span></span>

<span data-ttu-id="ebea6-134">a replika egy távoli adatközpontban toocreate hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="ebea6-134">toocreate a replica in a remote data center, do hello following steps:</span></span>

1. <span data-ttu-id="ebea6-135">[Virtuális hálózat létrehozása a hello új régió](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="ebea6-135">[Create a virtual network in hello new region](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).</span></span>

1. <span data-ttu-id="ebea6-136">[Konfigurálja a VNet – VNet-kapcsolatot, a hello Azure-portálon](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ebea6-136">[Configure a VNet-to-VNet connection using hello Azure portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).</span></span>

   >[!NOTE]
   ><span data-ttu-id="ebea6-137">Bizonyos esetekben előfordulhat, hogy toouse PowerShell toocreate hello VNet – VNet-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ebea6-137">In some cases, you may have toouse PowerShell toocreate hello VNet-to-VNet connection.</span></span> <span data-ttu-id="ebea6-138">Például ha különböző Azure-fiókra, hello kapcsolat hello portál nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="ebea6-138">For example, if you use different Azure accounts you cannot configure hello connection in hello portal.</span></span> <span data-ttu-id="ebea6-139">Ebben az esetben megtekintéséhez [konfigurálása egy VNet – VNet-kapcsolat használatával hello Azure-portálon](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ebea6-139">In this case see, [Configure a VNet-to-VNet connection using hello Azure portal](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

1. <span data-ttu-id="ebea6-140">[Hozzon létre egy tartományvezérlő hello új régióban](../../../active-directory/active-directory-new-forest-virtual-machine.md).</span><span class="sxs-lookup"><span data-stu-id="ebea6-140">[Create a domain controller in hello new region](../../../active-directory/active-directory-new-forest-virtual-machine.md).</span></span>

   <span data-ttu-id="ebea6-141">Ez a tartományvezérlő biztosítja a hitelesítést, ha hello tartományvezérlő hello elsődleges helyen nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ebea6-141">This domain controller provides authentication if hello domain controller in hello primary site is not available.</span></span>

1. <span data-ttu-id="ebea6-142">[SQL Server virtuális gép létrehozása hello új régióban](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="ebea6-142">[Create a SQL Server virtual machine in hello new region](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

1. <span data-ttu-id="ebea6-143">[Hozzon létre egy Azure terheléselosztó a hello új régió hello hálózat](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ebea6-143">[Create an Azure load balancer in hello network on hello new region](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

   <span data-ttu-id="ebea6-144">Ez a terheléselosztó kell:</span><span class="sxs-lookup"><span data-stu-id="ebea6-144">This load balancer must:</span></span>

   - <span data-ttu-id="ebea6-145">A kell hello azonos hálózati és alhálózati, új virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="ebea6-145">Be in hello same network and subnet as hello new virtual machine.</span></span>
   - <span data-ttu-id="ebea6-146">Egy statikus IP-címet hello rendelkezésre állási csoport figyelőjének rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ebea6-146">Have a static IP address for hello availability group listener.</span></span>
   - <span data-ttu-id="ebea6-147">Csak a virtuális gépek hello hello ugyanabban a régióban, a terheléselosztó hello álló háttérkészlet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ebea6-147">Include a backend pool consisting of only hello virtual machines in hello same region as hello load balancer.</span></span>
   - <span data-ttu-id="ebea6-148">A TCP port mintavételi adott toohello IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="ebea6-148">Use a TCP port probe specific toohello IP address.</span></span>
   - <span data-ttu-id="ebea6-149">Rendelkezik a terheléselosztási szabály adott toohello SQL Server a hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="ebea6-149">Have a load balancing rule specific toohello SQL Server in hello same region.</span></span>  

1. <span data-ttu-id="ebea6-150">[Adja hozzá a Feladatátvételi fürtszolgáltatás funkció toohello új SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="ebea6-150">[Add Failover Clustering feature toohello new SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

1. <span data-ttu-id="ebea6-151">[Tartományhoz hello új SQL Server toohello](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="ebea6-151">[Join hello new SQL Server toohello domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

1. <span data-ttu-id="ebea6-152">[Állítsa be a hello új SQL Server szolgáltatás fiók toouse egy olyan tartományi fiók](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).</span><span class="sxs-lookup"><span data-stu-id="ebea6-152">[Set hello new SQL Server service account toouse a domain account](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount).</span></span>

1. <span data-ttu-id="ebea6-153">[Adja hozzá a hello új SQL Server toohello Windows Server feladatátvevő fürt](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).</span><span class="sxs-lookup"><span data-stu-id="ebea6-153">[Add hello new SQL Server toohello Windows Server Failover Cluster](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode).</span></span>

1. <span data-ttu-id="ebea6-154">Hozzon létre egy IP-cím erőforrás hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="ebea6-154">Create an IP address resource on hello cluster.</span></span>

   <span data-ttu-id="ebea6-155">Létrehozhat hello IP-cím erőforrás a Feladatátvevőfürt-kezelőben.</span><span class="sxs-lookup"><span data-stu-id="ebea6-155">You can create hello IP address resource in Failover Cluster Manager.</span></span> <span data-ttu-id="ebea6-156">Hello rendelkezésre állási csoport ellenőrzéshez kattintson jobb gombbal kattintson **erőforrás hozzáadása**, **több erőforrások**, és kattintson a **IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="ebea6-156">Right-click hello availability group role, click **Add Resource**, **More Resources**, and click **IP Address**.</span></span>

   ![IP-cím létrehozása](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   <span data-ttu-id="ebea6-158">Az IP-cím konfigurálása a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="ebea6-158">Configure this IP address as follows:</span></span>

   - <span data-ttu-id="ebea6-159">Hello távoli adatközpontban hello hálózati használja.</span><span class="sxs-lookup"><span data-stu-id="ebea6-159">Use hello network from hello remote data center.</span></span>
   - <span data-ttu-id="ebea6-160">Hello új Azure terheléselosztó hello IP-címet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="ebea6-160">Assign hello IP address from hello new Azure load balancer.</span></span> 

1. <span data-ttu-id="ebea6-161">Az új SQL Server az SQL Server Configuration Manager, hello [engedélyezze az Always On rendelkezésre állási csoportok](http://msdn.microsoft.com/library/ff878259.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebea6-161">On hello new SQL Server in SQL Server Configuration Manager, [enable Always On Availability Groups](http://msdn.microsoft.com/library/ff878259.aspx).</span></span>

1. <span data-ttu-id="ebea6-162">[Nyissa meg tűzfalportjai hello új SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span><span class="sxs-lookup"><span data-stu-id="ebea6-162">[Open firewall ports on hello new SQL Server](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).</span></span>

   <span data-ttu-id="ebea6-163">hello portszámok tooopen van szüksége a környezettől függ.</span><span class="sxs-lookup"><span data-stu-id="ebea6-163">hello port numbers you need tooopen depend on your environment.</span></span> <span data-ttu-id="ebea6-164">Megnyitott portok hello tükrözési végpont és az Azure terheléselosztó betölteni.</span><span class="sxs-lookup"><span data-stu-id="ebea6-164">Open ports for hello mirroring endpoint and Azure load balancer health probe.</span></span>

1. <span data-ttu-id="ebea6-165">[Adja hozzá a replika toohello rendelkezésre állási csoport hello új SQL Server](http://msdn.microsoft.com/library/hh213239.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebea6-165">[Add a replica toohello availability group on hello new SQL Server](http://msdn.microsoft.com/library/hh213239.aspx).</span></span>

   <span data-ttu-id="ebea6-166">A replika a távoli Azure-régió állítsa kézi feladatátvételre aszinkron replikációra.</span><span class="sxs-lookup"><span data-stu-id="ebea6-166">For a replica in a remote Azure region, set it for asynchronous replication with manual failover.</span></span>  

1. <span data-ttu-id="ebea6-167">Adja hozzá hello IP-cím erőforrás hello figyelő ügyfél hozzáférési pont (hálózatnév) a fürt függőségei.</span><span class="sxs-lookup"><span data-stu-id="ebea6-167">Add hello IP address resource as a dependency for hello listener client access point (network name) cluster.</span></span>

   <span data-ttu-id="ebea6-168">hello következő képernyőfelvétel egy megfelelően konfigurált IP-cím fürt erőforrás:</span><span class="sxs-lookup"><span data-stu-id="ebea6-168">hello following screenshot shows a properly configured IP address cluster resource:</span></span>

   ![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   ><span data-ttu-id="ebea6-170">hello fürterőforrás-csoport mindkét IP-címeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ebea6-170">hello cluster resource group includes both IP addresses.</span></span> <span data-ttu-id="ebea6-171">Mindkét IP-címei hello figyelő ügyfél-hozzáférési pont a függőségeit.</span><span class="sxs-lookup"><span data-stu-id="ebea6-171">Both IP addresses are dependencies for hello listener client access point.</span></span> <span data-ttu-id="ebea6-172">Használjon hello **vagy** operátor szerepel a következő hello függőségi fürtkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ebea6-172">Use hello **OR** operator in hello cluster dependency configuration.</span></span>

1. <span data-ttu-id="ebea6-173">[A PowerShell hello fürt paraméterek beállítása](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).</span><span class="sxs-lookup"><span data-stu-id="ebea6-173">[Set hello cluster parameters in PowerShell](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam).</span></span>

<span data-ttu-id="ebea6-174">PowerShell-parancsprogrammal hello hello fürt hálózati név, IP-címet, és a mintavételi portot a hello konfigurált terheléselosztó hello új régióban.</span><span class="sxs-lookup"><span data-stu-id="ebea6-174">Run hello PowerShell script with hello cluster network name, IP address, and probe port that you configured on hello load balancer in hello new region.</span></span>

   ```PowerShell
   $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster name for hello network in hello new region (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "<IPResourceName>" # hello cluster name for hello new IP Address resource.
   $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB) in hello new region. This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <nnnnn> # hello probe port you set on hello ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

## <a name="set-connection-for-multiple-subnets"></a><span data-ttu-id="ebea6-175">Set-kapcsolatot a több alhálózattal</span><span class="sxs-lookup"><span data-stu-id="ebea6-175">Set connection for multiple subnets</span></span>

<span data-ttu-id="ebea6-176">hello replika hello távoli adatközpontban hello rendelkezésre állási csoport része, de egy másik alhálózat van.</span><span class="sxs-lookup"><span data-stu-id="ebea6-176">hello replica in hello remote data center is part of hello availability group but it is in a different subnet.</span></span> <span data-ttu-id="ebea6-177">Ha ez a másodpéldány hello elsődleges másodpéldány, alkalmazás időtúllépésnek fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="ebea6-177">If this replica becomes hello primary replica, application connection time-outs may occur.</span></span> <span data-ttu-id="ebea6-178">Ez a viselkedés van hello ugyanaz, mint a helyi rendelkezésre állási csoport egy több alhálózatos környezetben.</span><span class="sxs-lookup"><span data-stu-id="ebea6-178">This behavior is hello same as an on-premises availability group in a multi-subnet deployment.</span></span> <span data-ttu-id="ebea6-179">ügyfélalkalmazások, tooallow kapcsolódásának hello ügyfél kapcsolat frissítése, vagy konfigurálhatja a névfeloldást, gyorsítótárazás hello fürt hálózatnév-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ebea6-179">tooallow connections from client applications, either update hello client connection or configure name resolution caching on hello cluster network name resource.</span></span>

<span data-ttu-id="ebea6-180">Lehetőleg, frissítse a hello ügyfél kapcsolati karakterláncok tooset `MultiSubnetFailover=Yes`.</span><span class="sxs-lookup"><span data-stu-id="ebea6-180">Preferably, update hello client connection strings tooset `MultiSubnetFailover=Yes`.</span></span> <span data-ttu-id="ebea6-181">Lásd: [kapcsolódás a MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="ebea6-181">See [Connecting With MultiSubnetFailover](http://msdn.microsoft.com/library/gg471494#Anchor_0).</span></span>

<span data-ttu-id="ebea6-182">Hello kapcsolati karakterláncok nem módosítható, ha a megoldás-gyorsítótárazás is konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="ebea6-182">If you cannot modify hello connection strings, you can configure name resolution caching.</span></span> <span data-ttu-id="ebea6-183">Lásd: [kapcsolat időtúllépése több alhálózatot a rendelkezésre állási csoportban](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).</span><span class="sxs-lookup"><span data-stu-id="ebea6-183">See [Connection Timeouts in Multi-subnet Availability Group](http://blogs.msdn.microsoft.com/alwaysonpro/2014/06/03/connection-timeouts-in-multi-subnet-availability-group/).</span></span>

## <a name="fail-over-tooremote-region"></a><span data-ttu-id="ebea6-184">Feladatok átadása tooremote régió</span><span class="sxs-lookup"><span data-stu-id="ebea6-184">Fail over tooremote region</span></span>

<span data-ttu-id="ebea6-185">tootest figyelő kapcsolat toohello távoli régió, átveheti hello replika toohello távoli régióban.</span><span class="sxs-lookup"><span data-stu-id="ebea6-185">tootest listener connectivity toohello remote region, you can fail over hello replica toohello remote region.</span></span> <span data-ttu-id="ebea6-186">Míg a hello replika aszinkron, a feladatátvétel során sebezhető toopotential adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="ebea6-186">While hello replica is asynchronous, failover is vulnerable toopotential data loss.</span></span> <span data-ttu-id="ebea6-187">adatvesztés nélküli keresztül toofail hello rendelkezésre állási mód toosynchronous és hello feladatátvételi mód tooautomatic beállítása.</span><span class="sxs-lookup"><span data-stu-id="ebea6-187">toofail over without data loss, change hello availability mode toosynchronous and set hello failover mode tooautomatic.</span></span> <span data-ttu-id="ebea6-188">A lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="ebea6-188">Use hello following steps:</span></span>

1. <span data-ttu-id="ebea6-189">A **Object Explorer**, csatlakozzon toohello hello elsődleges másodpéldányt futtató SQL Server példányát.</span><span class="sxs-lookup"><span data-stu-id="ebea6-189">In **Object Explorer**, connect toohello instance of SQL Server that hosts hello primary replica.</span></span>
1. <span data-ttu-id="ebea6-190">A **AlwaysOn rendelkezésre állási csoportok**, **rendelkezésre állási csoportok**, kattintson a jobb gombbal a rendelkezésre állási csoportot, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="ebea6-190">Under **AlwaysOn Availability Groups**, **Availability Groups**, right-click your availability group and click **Properties**.</span></span>
1. <span data-ttu-id="ebea6-191">A hello **általános** lap **rendelkezésre állási másodpéldányok**, set hello másodlagos replika hello vész-Helyreállítási hely toouse **szinkron véglegesítés** rendelkezésre állási módjának és **Automatikus** feladatátvételi mód.</span><span class="sxs-lookup"><span data-stu-id="ebea6-191">On hello **General** page, under **Availability Replicas**, set hello secondary replica in hello DR site toouse **Synchronous Commit** availability mode and **Automatic** failover mode.</span></span>
1. <span data-ttu-id="ebea6-192">Ha egy másodlagos másodpéldány az elsődleges replika, a magas rendelkezésre állású azonos helyen található, állítsa be az adott replika túl**aszinkron véglegesítés** és **manuális**.</span><span class="sxs-lookup"><span data-stu-id="ebea6-192">If you have a secondary replica in same site as your primary replica for high availability, set this replica too**Asynchronous Commit** and **Manual**.</span></span>
1. <span data-ttu-id="ebea6-193">Kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="ebea6-193">Click OK.</span></span>
1. <span data-ttu-id="ebea6-194">A **Object Explorer**, kattintson a jobb gombbal a hello rendelkezésre állási csoportot, és kattintson a **irányítópult megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="ebea6-194">In **Object Explorer**, right-click hello availability group, and click **Show Dashboard**.</span></span>
1. <span data-ttu-id="ebea6-195">Az irányítópult hello, győződjön meg arról, hogy hello hello vész-Helyreállítási helyen replika szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="ebea6-195">On hello dashboard, verify that hello replica on hello DR site is synchronized.</span></span>
1. <span data-ttu-id="ebea6-196">A **Object Explorer**, kattintson a jobb gombbal a hello rendelkezésre állási csoportot, és kattintson a **feladatátvételi...** . SQL Server Management Studios megnyílik egy varázsló toofail keresztül SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ebea6-196">In **Object Explorer**, right-click hello availability group, and click **Failover...**. SQL Server Management Studios opens a wizard toofail over SQL Server.</span></span>  
1. <span data-ttu-id="ebea6-197">Kattintson a **következő**, és jelölje be hello SQL Server-példány hello vész-Helyreállítási helyen.</span><span class="sxs-lookup"><span data-stu-id="ebea6-197">Click **Next**, and select hello SQL Server instance in hello DR site.</span></span> <span data-ttu-id="ebea6-198">Kattintson a **következő** újra.</span><span class="sxs-lookup"><span data-stu-id="ebea6-198">Click **Next** again.</span></span>
1. <span data-ttu-id="ebea6-199">Csatlakozzon az SQL Server-példány toohello hello vész-Helyreállítási helyen, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ebea6-199">Connect toohello SQL Server instance in hello DR site and click **Next**.</span></span>
1. <span data-ttu-id="ebea6-200">A hello **összegzés** lapon ellenőrizze a hello beállításait, majd kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ebea6-200">On hello **Summary** page, verify hello settings and click **Finish**.</span></span>

<span data-ttu-id="ebea6-201">Kapcsolat tesztelése után helyezze át a hello elsődleges replika hátsó tooyour elsődleges data center és hello rendelkezésre állási mód hátsó tootheir normál működési beállításait.</span><span class="sxs-lookup"><span data-stu-id="ebea6-201">After testing connectivity, move hello primary replica back tooyour primary data center and set hello availability mode back tootheir normal operating settings.</span></span> <span data-ttu-id="ebea6-202">hello következő táblázatban hello normál működési beállításait a jelen dokumentumban ismertetett hello architektúra:</span><span class="sxs-lookup"><span data-stu-id="ebea6-202">hello following table shows hello normal operational settings for hello architecture described in this document:</span></span>

| <span data-ttu-id="ebea6-203">Hely</span><span class="sxs-lookup"><span data-stu-id="ebea6-203">Location</span></span> | <span data-ttu-id="ebea6-204">Server-példány</span><span class="sxs-lookup"><span data-stu-id="ebea6-204">Server Instance</span></span> | <span data-ttu-id="ebea6-205">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="ebea6-205">Role</span></span> | <span data-ttu-id="ebea6-206">Rendelkezésre állási mód</span><span class="sxs-lookup"><span data-stu-id="ebea6-206">Availability Mode</span></span> | <span data-ttu-id="ebea6-207">Feladatátvételi mód</span><span class="sxs-lookup"><span data-stu-id="ebea6-207">Failover Mode</span></span>
| ----- | ----- | ----- | ----- | -----
| <span data-ttu-id="ebea6-208">Elsődleges adatközpont</span><span class="sxs-lookup"><span data-stu-id="ebea6-208">Primary data center</span></span> | <span data-ttu-id="ebea6-209">SQL-1</span><span class="sxs-lookup"><span data-stu-id="ebea6-209">SQL-1</span></span> | <span data-ttu-id="ebea6-210">Elsődleges</span><span class="sxs-lookup"><span data-stu-id="ebea6-210">Primary</span></span> | <span data-ttu-id="ebea6-211">Szinkron</span><span class="sxs-lookup"><span data-stu-id="ebea6-211">Synchronous</span></span> | <span data-ttu-id="ebea6-212">Automatikus</span><span class="sxs-lookup"><span data-stu-id="ebea6-212">Automatic</span></span>
| <span data-ttu-id="ebea6-213">Elsődleges adatközpont</span><span class="sxs-lookup"><span data-stu-id="ebea6-213">Primary data center</span></span> | <span data-ttu-id="ebea6-214">SQL-2</span><span class="sxs-lookup"><span data-stu-id="ebea6-214">SQL-2</span></span> | <span data-ttu-id="ebea6-215">Másodlagos</span><span class="sxs-lookup"><span data-stu-id="ebea6-215">Secondary</span></span> | <span data-ttu-id="ebea6-216">Szinkron</span><span class="sxs-lookup"><span data-stu-id="ebea6-216">Synchronous</span></span> | <span data-ttu-id="ebea6-217">Automatikus</span><span class="sxs-lookup"><span data-stu-id="ebea6-217">Automatic</span></span>
| <span data-ttu-id="ebea6-218">Távoli vagy másodlagos adatközpont</span><span class="sxs-lookup"><span data-stu-id="ebea6-218">Secondary or remote data center</span></span> | <span data-ttu-id="ebea6-219">SQL-3</span><span class="sxs-lookup"><span data-stu-id="ebea6-219">SQL-3</span></span> | <span data-ttu-id="ebea6-220">Másodlagos</span><span class="sxs-lookup"><span data-stu-id="ebea6-220">Secondary</span></span> | <span data-ttu-id="ebea6-221">Aszinkron</span><span class="sxs-lookup"><span data-stu-id="ebea6-221">Asynchronous</span></span> | <span data-ttu-id="ebea6-222">Manuális</span><span class="sxs-lookup"><span data-stu-id="ebea6-222">Manual</span></span>


### <a name="more-information-about-planned-and-forced-manual-failover"></a><span data-ttu-id="ebea6-223">További információ a tervezett és a rendszer kényszerített kézi feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="ebea6-223">More information about planned and forced manual failover</span></span>

<span data-ttu-id="ebea6-224">További információkért tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="ebea6-224">For more information, see hello following topics:</span></span>

- [<span data-ttu-id="ebea6-225">Végezzen el egy tervezett kézi feladatátvételt egy rendelkezésre állási csoport (SQL Server)</span><span class="sxs-lookup"><span data-stu-id="ebea6-225">Perform a Planned Manual Failover of an Availability Group (SQL Server)</span></span>](http://msdn.microsoft.com/library/hh231018.aspx)
- [<span data-ttu-id="ebea6-226">Hajtsa végre a kényszerített kézi feladatátvétel egy rendelkezésre állási csoport (SQL Server)</span><span class="sxs-lookup"><span data-stu-id="ebea6-226">Perform a Forced Manual Failover of an Availability Group (SQL Server)</span></span>](http://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a><span data-ttu-id="ebea6-227">További hivatkozások</span><span class="sxs-lookup"><span data-stu-id="ebea6-227">Additional Links</span></span>

* [<span data-ttu-id="ebea6-228">Always On rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="ebea6-228">Always On Availability Groups</span></span>](http://msdn.microsoft.com/library/hh510230.aspx)
* [<span data-ttu-id="ebea6-229">Az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="ebea6-229">Azure Virtual Machines</span></span>](http://docs.microsoft.com/azure/virtual-machines/windows/)
* [<span data-ttu-id="ebea6-230">Azure Load Balancer Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="ebea6-230">Azure Load Balancers</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [<span data-ttu-id="ebea6-231">Az Azure rendelkezésre állási csoportok</span><span class="sxs-lookup"><span data-stu-id="ebea6-231">Azure Availability Sets</span></span>](../manage-availability.md)
