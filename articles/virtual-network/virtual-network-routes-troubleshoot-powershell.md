---
title: "aaaTroubleshoot útvonalak - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot irányítja a hello Azure Resource Manager üzembe helyezési modellel Azure PowerShell használatával."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: bf7dc5e7-9399-460e-8e0d-8992dbed98a6
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 7a07806df5c1d0caee921187e6ad29f6755ab535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-routes-using-azure-powershell"></a><span data-ttu-id="86b41-103">Az Azure PowerShell útvonalak hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="86b41-103">Troubleshoot routes using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="86b41-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="86b41-104">Azure Portal</span></span>](virtual-network-routes-troubleshoot-portal.md)
> * [<span data-ttu-id="86b41-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="86b41-105">PowerShell</span></span>](virtual-network-routes-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="86b41-106">Ha a hálózati csatlakozási problémák tooor az az Azure virtuális gép (VM) tapasztal, útvonalak előfordulhat, hogy mely negatív hatással lehet a VM forgalmat.</span><span class="sxs-lookup"><span data-stu-id="86b41-106">If you are experiencing network connectivity issues tooor from your Azure Virtual Machine (VM), routes may be impacting your VM traffic flows.</span></span> <span data-ttu-id="86b41-107">Ez a cikk áttekintést diagnosztikai képességek útvonalak toohelp további hibaelhárítást kell végeznie.</span><span class="sxs-lookup"><span data-stu-id="86b41-107">This article provides an overview of diagnostics capabilities for routes toohelp troubleshoot further.</span></span>

<span data-ttu-id="86b41-108">Az útvonaltáblák alhálózatok tartoznak, és minden hálózati interfészen (NIC) alhálózat érvényben.</span><span class="sxs-lookup"><span data-stu-id="86b41-108">Route tables are associated with subnets and are effective on all network interfaces (NIC) in that subnet.</span></span> <span data-ttu-id="86b41-109">a következő típusú útvonalak hello lehet alkalmazott tooeach hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="86b41-109">hello following types of routes can be applied tooeach network interface:</span></span>

* <span data-ttu-id="86b41-110">**Rendszerútvonalak:** alapértelmezés szerint az Azure Virtual Network (VNet) létrehozott összes alhálózatot rendelkezik rendszer útvonaltábláit, amelyek lehetővé teszik a helyi VNet forgalmát, a helyszíni forgalom keresztül VPN-átjárók és internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="86b41-110">**System routes:** By default, every subnet created in an Azure Virtual Network (VNet) has system route tables that allow local VNet traffic, on-premises traffic via VPN gateways, and Internet traffic.</span></span> <span data-ttu-id="86b41-111">Rendszerútvonalak társviszonyban Vnetek esetében is létezik.</span><span class="sxs-lookup"><span data-stu-id="86b41-111">System routes also exist for peered VNets.</span></span>
* <span data-ttu-id="86b41-112">**BGP-útvonalakat:** propagálása toonetwork felületek expressroute-on vagy VPN-kapcsolatok pont-pont keresztül.</span><span class="sxs-lookup"><span data-stu-id="86b41-112">**BGP routes:** Propagated toonetwork interfaces through ExpressRoute or site-to-site VPN connections.</span></span> <span data-ttu-id="86b41-113">Tudjon meg többet a BGP-útválasztás hello olvasásával [BGP a VPN-átjárók](../vpn-gateway/vpn-gateway-bgp-overview.md) és [ExpressRoute – áttekintés](../expressroute/expressroute-introduction.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="86b41-113">Learn more about BGP routing by reading hello [BGP with VPN gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) and [ExpressRoute overview](../expressroute/expressroute-introduction.md) articles.</span></span>
* <span data-ttu-id="86b41-114">**Felhasználó által definiált útvonalak (UDR):** Ha hálózati virtuális készülékek használata, vagy a rendszer kényszerített bújtatás forgalom tooan a helyi hálózaton keresztül a telephelyek közötti VPN, előfordulhat, hogy felhasználói (udr-EK) rendelt útvonalakat a alhálózati útválasztási táblázatot.</span><span class="sxs-lookup"><span data-stu-id="86b41-114">**User-defined routes (UDR):** If you are using network virtual appliances or are forced-tunneling traffic tooan on-premises network via a site-to-site VPN, you may have user-defined routes (UDRs) associated with your subnet route table.</span></span> <span data-ttu-id="86b41-115">Ha még nem ismeri a udr-EK, olvassa el a hello [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md#user-defined-routes) cikk.</span><span class="sxs-lookup"><span data-stu-id="86b41-115">If you're not familiar with UDRs, read hello [user-defined routes](virtual-networks-udr-overview.md#user-defined-routes) article.</span></span>

<span data-ttu-id="86b41-116">Hello alkalmazott tooa hálózati kapcsolat lehet különböző útvonalakat után lehet nehéz toodetermine összesített útvonalakat érvényben.</span><span class="sxs-lookup"><span data-stu-id="86b41-116">With hello various routes that can be applied tooa network interface, it can be difficult toodetermine which aggregate routes are effective.</span></span> <span data-ttu-id="86b41-117">toohelp hibáinak elhárítása a virtuális gép hálózati kapcsolatot, akkor az összes hello hatékony útvonal egy adott hálózati csatoló hello Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="86b41-117">toohelp troubleshoot VM network connectivity, you can view all hello effective routes for a network interface in hello Azure Resource Manager deployment model.</span></span>

## <a name="using-effective-routes-tootroubleshoot-vm-traffic-flow"></a><span data-ttu-id="86b41-118">Hatékony útvonalak tootroubleshoot VM adatforgalmat használatával</span><span class="sxs-lookup"><span data-stu-id="86b41-118">Using Effective Routes tootroubleshoot VM traffic flow</span></span>
<span data-ttu-id="86b41-119">Ez a cikk hello forgatókönyv szerint egy példa tooillustrate hogyan tootroubleshoot hello hatályos irányítja a hálózati illesztő a következő használja:</span><span class="sxs-lookup"><span data-stu-id="86b41-119">This article uses hello following scenario as an example tooillustrate how tootroubleshoot hello effective routes for a network interface:</span></span>

<span data-ttu-id="86b41-120">A virtuális gép (*VM1*) toohello VNet csatlakoztatva (*VNet1*, előtag: 10.9.0.0/16) tooconnect tooa újonnan peered vnet VM(VM3) sikertelen (*VNet3*, 10.10.0.0/16 előtag).</span><span class="sxs-lookup"><span data-stu-id="86b41-120">A VM (*VM1*) connected toohello VNet (*VNet1*, prefix: 10.9.0.0/16) fails tooconnect tooa VM(VM3) in a newly peered VNet (*VNet3*, prefix 10.10.0.0/16).</span></span> <span data-ttu-id="86b41-121">Nincsenek nem udr-EK vagy a BGP irányítja a alkalmazott tooVM1-NIC1 hálózathoz csatlakozó adapter toohello VM, csak a rendszerútvonalak érvényesek.</span><span class="sxs-lookup"><span data-stu-id="86b41-121">There are no UDRs or BGP routes applied tooVM1-NIC1 network interface connected toohello VM, only system routes are applied.</span></span>

<span data-ttu-id="86b41-122">Ez a cikk azt ismerteti, hogyan toodetermine hello oka hello csatlakozási hiba, hatékony útvonalak funkció használata Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="86b41-122">This article explains how toodetermine hello cause of hello connection failure, using effective routes capability in Azure Resource Management deployment model.</span></span>
<span data-ttu-id="86b41-123">Amíg hello példában csak a rendszerútvonalak, ugyanezek a lépések hello lehet használt toodetermine bejövő és kimenő kapcsolódási hibák útvonal bármilyen keresztül.</span><span class="sxs-lookup"><span data-stu-id="86b41-123">While hello example uses only system routes, hello same steps can be used toodetermine inbound and outbound connection failures over any route type.</span></span>

> [!NOTE]
> <span data-ttu-id="86b41-124">Ha a virtuális Géphez kapcsolt egynél több hálózati Adaptert, ellenőrizze a hatékony útvonalak az egyes hálózati adapterek toodiagnose hálózati csatlakozási problémák tooand VM hello.</span><span class="sxs-lookup"><span data-stu-id="86b41-124">If your VM has more than one NIC attached, check effective routes for each of hello NICs toodiagnose network connectivity issues tooand from a VM.</span></span>
> 
> 

### <a name="view-effective-routes-for-a-virtual-machine"></a><span data-ttu-id="86b41-125">A virtuális gépek hatékony útvonalak megtekintése</span><span class="sxs-lookup"><span data-stu-id="86b41-125">View effective routes for a virtual machine</span></span>
<span data-ttu-id="86b41-126">toosee hello összesített útvonalakat, amelyek alkalmazott tooa VM, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="86b41-126">toosee hello aggregate routes that are applied tooa VM, complete hello following steps:</span></span>

### <a name="view-effective-routes-for-a-network-interface"></a><span data-ttu-id="86b41-127">Egy adott hálózati csatoló hatékony útvonalak megtekintése</span><span class="sxs-lookup"><span data-stu-id="86b41-127">View effective routes for a network interface</span></span>
<span data-ttu-id="86b41-128">toosee hello összesített útvonalakat, amelyek alkalmazott tooa hálózati kapcsolat teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="86b41-128">toosee hello aggregate routes that are applied tooa network interface, complete hello following steps:</span></span>

1. <span data-ttu-id="86b41-129">Indítsa el az Azure PowerShell-munkamenet és bejelentkezési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="86b41-129">Start an Azure PowerShell session and login tooAzure.</span></span> <span data-ttu-id="86b41-130">Ha nem ismeri az Azure PowerShell, olvassa el a hello [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="86b41-130">If you’re not familiar with Azure PowerShell, read hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="86b41-131">hello parancs hatására az eredményobjektumoknak minden alkalmazott útvonalak tooa hálózati illesztő nevű *VM1-NIC1* hello erőforráscsoportban *RG1*.</span><span class="sxs-lookup"><span data-stu-id="86b41-131">hello following command returns all routes applied tooa network interface named *VM1-NIC1* in hello resource group *RG1*.</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="86b41-132">Ha egy adott hálózati csatoló hello neve nem tudja, írja be a következő parancs tooretrieve hello nevét az összes hálózati adapter van egy erőforrás group.* hello</span><span class="sxs-lookup"><span data-stu-id="86b41-132">If you don’t know hello name of a network interface, type hello following command tooretrieve hello names of all network interfaces in a resource group.*</span></span>
   > 
   > 
   
       Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name
   
   <span data-ttu-id="86b41-133">hello következő kimeneti keresi hasonló toohello kimeneti minden alkalmazott útvonal toohello alhálózati hello hálózati adapter csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="86b41-133">hello following output looks similar toohello output for each route applied toohello subnet hello NIC is connected to:</span></span>
   
       Name :
       State : Active
       AddressPrefix : {10.9.0.0/16}
       NextHopType : VNetLocal
       NextHopIpAddress : {}
   
       Name :
       State : Active
       AddressPrefix : {0.0.0.0/16}
       NextHopType : Internet
       NextHopIpAddress : {}
   
   <span data-ttu-id="86b41-134">Figyelje meg a következő hello hello kimeneti:</span><span class="sxs-lookup"><span data-stu-id="86b41-134">Notice hello following in hello output:</span></span>
   
   * <span data-ttu-id="86b41-135">**Név**: hello hatékony útvonal lehet neve üres, kivéve, ha explicit módon megadott, felhasználó által definiált útvonalak.</span><span class="sxs-lookup"><span data-stu-id="86b41-135">**Name**: Name of hello effective route may be empty, unless explicitly specified, for user-defined routes.</span></span> 
   * <span data-ttu-id="86b41-136">**Állapot**: hello hatékony útvonal állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="86b41-136">**State**: Indicates state of hello effective route.</span></span> <span data-ttu-id="86b41-137">Lehetséges értékek: "Active" vagy "Érvénytelen"</span><span class="sxs-lookup"><span data-stu-id="86b41-137">Possible values are "Active" or "Invalid"</span></span>
   * <span data-ttu-id="86b41-138">**AddressPrefixes**: hello hatékony útvonal hello címelőtagot CIDR-formátumban határozza meg.</span><span class="sxs-lookup"><span data-stu-id="86b41-138">**AddressPrefixes**: Specifies hello address prefix of hello effective route in CIDR notation.</span></span> 
   * <span data-ttu-id="86b41-139">**nextHopType**: hello következő ugrást az adott útvonal hello jelzi.</span><span class="sxs-lookup"><span data-stu-id="86b41-139">**nextHopType**: Indicates hello next hop for hello given route.</span></span> <span data-ttu-id="86b41-140">A lehetséges értékek: *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, vagy *Null*.</span><span class="sxs-lookup"><span data-stu-id="86b41-140">Possible values are *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*, or *Null*.</span></span> <span data-ttu-id="86b41-141">Érték *Null* a **nextHopType** egy UDR jelezhetik érvénytelen útvonal.</span><span class="sxs-lookup"><span data-stu-id="86b41-141">A value of *Null* for **nextHopType** in a UDR may indicate an invalid route.</span></span> <span data-ttu-id="86b41-142">Például ha **nextHopType** van *VirtualAppliance* és hello hálózati virtuális készülék virtuális gép nem létesített/futó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="86b41-142">For example, if **nextHopType** is *VirtualAppliance* and hello network virtual appliance VM is not in a provisioned/running state.</span></span> <span data-ttu-id="86b41-143">Ha **nextHopType** van *A(z)* és nincs átjáró kiépítése/fut a virtuális hálózat megadott hello, hello útvonal érvénytelen válhat.</span><span class="sxs-lookup"><span data-stu-id="86b41-143">If **nextHopType** is *VPNGateway* and there is no gateway provisioned/running in hello given VNet, hello route may become invalid.</span></span>
   * <span data-ttu-id="86b41-144">**NextHopIpAddress**: hello következő ugrás hello hatékony útvonal hello IP-címet adja meg.</span><span class="sxs-lookup"><span data-stu-id="86b41-144">**NextHopIpAddress**: Specifies hello IP address of hello next hop of hello effective route.</span></span>
   
   <span data-ttu-id="86b41-145">hello parancs hatására az eredményobjektumoknak hello útvonalak egy egyszerűbb tooview táblázatban:</span><span class="sxs-lookup"><span data-stu-id="86b41-145">hello following command returns hello routes in an easier tooview table:</span></span>
   
       Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table
   
   <span data-ttu-id="86b41-146">hello következő kimenete hello kimeneti hello forgatókönyvhöz a fentiekben ismertetett kapott egy részénél:</span><span class="sxs-lookup"><span data-stu-id="86b41-146">hello following output is some of hello output received for hello scenario described previously:</span></span>
   
       Name State AddressPrefix NextHopType NextHopIpAddress
       ---- ----- ------------- ----------- ----------------
       Active {10.9.0.0/16} VnetLocal {}
       Active {0.0.0.0/0} Internet {}
3. <span data-ttu-id="86b41-147">Nincs a nem felsorolt útvonal toohello *WestUS-VNet3* VNet (a 10.10.0.0/16)** előtag *WestUS-VNet1* (előtag 10.9.0.0/16) hello kimenet hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="86b41-147">There is no route listed toohello *WestUS-VNet3* VNet (Prefix 10.10.0.0/16)** from *WestUS-VNet1* (Prefix 10.9.0.0/16) in hello output from hello previous step.</span></span> <span data-ttu-id="86b41-148">Ahogy az alábbi képen hello, hello hello a Vnetben társviszony-létesítési hivatkozás *WestUS-VNet3* VNet van hello *Disconnected* állapotát.</span><span class="sxs-lookup"><span data-stu-id="86b41-148">As shown in hello following picture, hello VNet peering link with hello *WestUS-VNet3* VNet is in hello *Disconnected* state.</span></span>
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)
   
    <span data-ttu-id="86b41-149">hello kétirányú hivatkozás a hello társviszony megszakad, amely ismerteti, miért VM1 nem tudott csatlakozni a hello tooVM3 *WestUS-VNet3* virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="86b41-149">hello bi-directional link for hello peering is broken, which explains why VM1 could not connect tooVM3 in hello *WestUS-VNet3* VNet.</span></span> <span data-ttu-id="86b41-150">A telepítő egy kétirányú VNet társviszony-létesítési hivatkozást újra a *WestUS-VNet1* és *WestUS-VNet3* Vnetek.</span><span class="sxs-lookup"><span data-stu-id="86b41-150">Setup a bi-directional VNet peering link again for *WestUS-VNet1* and *WestUS-VNet3* VNets.</span></span> <span data-ttu-id="86b41-151">hello kimeneti visszaadott hello Vnetben társviszony-létesítési hivatkozás megfelelően létrejötte után a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="86b41-151">hello output returned after hello VNet peering link is correctly established follows:</span></span>
   
        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
   
    <span data-ttu-id="86b41-152">Miután hello probléma megállapításához is hozzáadhat, eltávolításához, vagy módosítsa a útvonalakat, és útvonaltáblát.</span><span class="sxs-lookup"><span data-stu-id="86b41-152">Once you determine hello issue, you can add, remove, or change routes and route tables.</span></span> <span data-ttu-id="86b41-153">Írja be a következő parancs toosee hello használt parancsok toodo listája úgy hello:</span><span class="sxs-lookup"><span data-stu-id="86b41-153">Type hello following command toosee a list of hello commands used toodo so:</span></span>
   
        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a><span data-ttu-id="86b41-154">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="86b41-154">Considerations</span></span>
<span data-ttu-id="86b41-155">Útvonalak listája hello felülvizsgálata során szem előtt néhány dolgot tookeep adott vissza:</span><span class="sxs-lookup"><span data-stu-id="86b41-155">A few things tookeep in mind when reviewing hello list of routes returned:</span></span>

* <span data-ttu-id="86b41-156">Útválasztás alapján történik a leghosszabb előtag-megfeleltetés (LPM) között udr-EK, útvonalakat a BGP és a rendszer.</span><span class="sxs-lookup"><span data-stu-id="86b41-156">Routing is based on Longest Prefix Match (LPM) among UDRs, BGP and system routes.</span></span> <span data-ttu-id="86b41-157">Ha egynél több útvonal rendelkezik hello azonos LPM egyezik, majd egy útvonal kiválasztása sorrend hello Kiindulás alapján történik:</span><span class="sxs-lookup"><span data-stu-id="86b41-157">If there is more than one route with hello same LPM match, then a route is selected based on its origin in hello following order:</span></span>
  
  * <span data-ttu-id="86b41-158">Felhasználó által megadott útvonal</span><span class="sxs-lookup"><span data-stu-id="86b41-158">User-defined route</span></span>
  * <span data-ttu-id="86b41-159">BGP-útvonal</span><span class="sxs-lookup"><span data-stu-id="86b41-159">BGP route</span></span>
  * <span data-ttu-id="86b41-160">Rendszerútvonal (alapértelmezett)</span><span class="sxs-lookup"><span data-stu-id="86b41-160">System (Default) route</span></span>
    
    <span data-ttu-id="86b41-161">Hatékony útvonalakat hatékony útvonalakat, amelyek alapján az összes hello mavenen útvonal az LPM megfeleltetéssel csak kaphat.</span><span class="sxs-lookup"><span data-stu-id="86b41-161">With effective routes, you can only see effective routes that are LPM match based on all hello availble routes.</span></span> <span data-ttu-id="86b41-162">Jelenít meg, hogyan hello útvonalak ténylegesen kiértékeli a megadott hálózati adapter, így sokkal könnyebb tootroubleshoot adott útvonalak, előfordulhat, hogy mely negatív hatással lehet a virtuális gép és a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="86b41-162">By showing how hello routes are actually evaluated for a given NIC, this makes it a lot easier tootroubleshoot specific routes that may be impacting connectivity to/from your VM.</span></span>
* <span data-ttu-id="86b41-163">Ha udr-EK és a forgalom tooa hálózati virtuális készülék (NVA) üzenetet küld *VirtualAppliance* , **nextHopType**, győződjön meg arról, hogy az IP-továbbítás engedélyezve van-e a hello NVA fogadó hello forgalom vagy Eldobott csomagok.</span><span class="sxs-lookup"><span data-stu-id="86b41-163">If you have UDRs and are sending traffic tooa network virtual appliance (NVA), with *VirtualAppliance* as **nextHopType**, ensure that IP forwarding is enabled on hello NVA receiving hello traffic or packets are dropped.</span></span> 
* <span data-ttu-id="86b41-164">Ha kényszerített bújtatás engedélyezve van, minden kimenő internetforgalom nem útválasztásos tooon helyszíni.</span><span class="sxs-lookup"><span data-stu-id="86b41-164">If Forced tunneling is enabled, all outbound Internet traffic will be routed tooon-premises.</span></span> <span data-ttu-id="86b41-165">A virtuális gép nem működik ez a beállítás attól függően, hogy hogyan hello helyszíni Internet tooyour RDP/SSH a forgalmat kezeli.</span><span class="sxs-lookup"><span data-stu-id="86b41-165">RDP/SSH from Internet tooyour VM may not work with this setting, depending on how hello on-premises handles this traffic.</span></span> 
  <span data-ttu-id="86b41-166">A kényszerített bújtatás engedélyezhető:</span><span class="sxs-lookup"><span data-stu-id="86b41-166">Forced-tunneling can be enabled:</span></span>
  * <span data-ttu-id="86b41-167">Ha használja a telephelyek közötti VPN, úgy, hogy egy felhasználó által megadott útvonal (UDR) a VPN-átjáróként nextHopType</span><span class="sxs-lookup"><span data-stu-id="86b41-167">If using site-to-site VPN, by setting a user-defined route (UDR) with nextHopType as VPN Gateway</span></span>
  * <span data-ttu-id="86b41-168">Ha az alapértelmezett útvonal BGP keresztül lett hirdetve.</span><span class="sxs-lookup"><span data-stu-id="86b41-168">If a default route is advertised over BGP</span></span>
* <span data-ttu-id="86b41-169">Vnet-társviszony létesítése – forgalom toowork megfelelően, a rendszer útvonal **nextHopType** *VNetPeering* léteznie kell a hello társviszonyban virtuális hálózat előtag tartományon.</span><span class="sxs-lookup"><span data-stu-id="86b41-169">For VNet peering traffic toowork correctly, a system route with **nextHopType** *VNetPeering* must exist for hello peered VNet’s prefix range.</span></span> <span data-ttu-id="86b41-170">Ha ilyen útvonal nem létezik, majd a Vnetben társviszony-létesítés hello hivatkozás OK néz ki:</span><span class="sxs-lookup"><span data-stu-id="86b41-170">If such a route doesn’t exist and hello VNet peering link looks OK:</span></span>
  * <span data-ttu-id="86b41-171">Várjon néhány másodpercet, amíg, és próbálja meg újra, ha az újonnan létrehozott társviszony-létesítési hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="86b41-171">Wait a few seconds and retry if it's a newly established peering link.</span></span> <span data-ttu-id="86b41-172">Alkalmanként tart hosszabb toopropagate útvonalak tooall hello hálózati illesztők alhálózat.</span><span class="sxs-lookup"><span data-stu-id="86b41-172">It occasionally takes longer toopropagate routes tooall hello network interfaces in a subnet.</span></span>
  * <span data-ttu-id="86b41-173">Hálózati biztonsági csoport (NSG) szabályok hello forgalom lehet, hogy érinti.</span><span class="sxs-lookup"><span data-stu-id="86b41-173">Network Security Group (NSG) rules may be impacting hello traffic flows.</span></span> <span data-ttu-id="86b41-174">További információkért lásd: hello [hibaelhárítása hálózati biztonsági csoportok](virtual-network-nsg-troubleshoot-powershell.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="86b41-174">For more information, see hello [Troubleshoot Network Security Groups](virtual-network-nsg-troubleshoot-powershell.md) article.</span></span>

