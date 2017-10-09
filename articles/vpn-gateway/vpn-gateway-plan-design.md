---
title: "Tervezési és kialakítási létesítmények közötti kapcsolatok: Azure VPN Gateway |} Microsoft Docs"
description: "További tudnivalók a VPN-átjáró tervezése és kialakítása létesítmények közötti, hibrid és VNet – VNet kapcsolatokhoz"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 3d4587ba31d163384212eca88a7e2c0ba8f3b21f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a><span data-ttu-id="3a5a2-103">A VPN Gateway tervezése és kialakítása</span><span class="sxs-lookup"><span data-stu-id="3a5a2-103">Planning and design for VPN Gateway</span></span>

<span data-ttu-id="3a5a2-104">Megtervezéséről és kialakításáról a létesítmények közötti és VNet – VNet konfigurációkkal lehet egyszerű vagy összetett, hálózati igényeitől függően.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-104">Planning and designing your cross-premises and VNet-to-VNet configurations can be either simple, or complicated, depending on your networking needs.</span></span> <span data-ttu-id="3a5a2-105">Ez a cikk bemutatja, hogyan alapvető tervezési és kialakítási szempontjai.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-105">This article walks you through basic planning and design considerations.</span></span>

## <span data-ttu-id="3a5a2-106"><a name="planning"></a>Tervezése</span><span class="sxs-lookup"><span data-stu-id="3a5a2-106"><a name="planning"></a>Planning</span></span>

### <span data-ttu-id="3a5a2-107"><a name="compare"></a>Létesítmények közötti kapcsolati lehetőségek</span><span class="sxs-lookup"><span data-stu-id="3a5a2-107"><a name="compare"></a>Cross-premises connectivity options</span></span>

<span data-ttu-id="3a5a2-108">Ha azt szeretné, hogy tooconnect a helyszíni helyek biztonságosan tooa virtuális hálózati telepítette, három különböző módon toodo ezért:-webhelyek, pont-pont és az ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-108">If you want tooconnect your on-premises sites securely tooa virtual network, you have three different ways toodo so: Site-to-Site, Point-to-Site, and ExpressRoute.</span></span> <span data-ttu-id="3a5a2-109">Hasonlítsa össze a rendelkezésre álló hello különböző létesítmények közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-109">Compare hello different cross-premises connections that are available.</span></span> <span data-ttu-id="3a5a2-110">a kiválasztott hello lehetőség például különböző szempontok függ:</span><span class="sxs-lookup"><span data-stu-id="3a5a2-110">hello option you choose can depend on various considerations, such as:</span></span>

* <span data-ttu-id="3a5a2-111">Mekkora átviteli sebesség szükséges a megoldásához?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-111">What kind of throughput does your solution require?</span></span>
* <span data-ttu-id="3a5a2-112">Szeretné, hogy toocommunicate keresztül hello nyilvános Internet biztonságos VPN-en keresztül, vagy egy személyes kapcsolaton keresztül?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-112">Do you want toocommunicate over hello public Internet via secure VPN, or over a private connection?</span></span>
* <span data-ttu-id="3a5a2-113">Nyilvános IP cím elérhető toouse van?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-113">Do you have a public IP address available toouse?</span></span>
* <span data-ttu-id="3a5a2-114">A VPN-eszköz toouse tervezési?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-114">Are you planning toouse a VPN device?</span></span> <span data-ttu-id="3a5a2-115">Ha igen, ez kompatibilis a rendszerrel?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-115">If so, is it compatible?</span></span>
* <span data-ttu-id="3a5a2-116">Csak néhány számítógépet csatlakoztatna, vagy állandó kapcsolatra van szüksége a helyéhez?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-116">Are you connecting just a few computers, or do you want a persistent connection for your site?</span></span>
* <span data-ttu-id="3a5a2-117">Milyen típusú VPN-átjárót kell hello megoldás toocreate keresi?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-117">What type of VPN gateway is required for hello solution you want toocreate?</span></span>
* <span data-ttu-id="3a5a2-118">Melyik átjárót SKU használja?</span><span class="sxs-lookup"><span data-stu-id="3a5a2-118">Which gateway SKU should you use?</span></span>

### <span data-ttu-id="3a5a2-119"><a name="planningtable"></a>Tervezési táblázat</span><span class="sxs-lookup"><span data-stu-id="3a5a2-119"><a name="planningtable"></a>Planning table</span></span>

<span data-ttu-id="3a5a2-120">hello a következő táblázat segítségével eldöntheti, hogy a megoldás legjobb hello kapcsolódási beállítást választja.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-120">hello following table can help you decide hello best connectivity option for your solution.</span></span>

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <span data-ttu-id="3a5a2-121"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="3a5a2-121"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <span data-ttu-id="3a5a2-122"><a name="wf"></a>Munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="3a5a2-122"><a name="wf"></a>Workflow</span></span>

<span data-ttu-id="3a5a2-123">hello következő felsorolás vázolja hello közös munkafolyamat felhő kapcsolathoz:</span><span class="sxs-lookup"><span data-stu-id="3a5a2-123">hello following list outlines hello common workflow for cloud connectivity:</span></span>

1. <span data-ttu-id="3a5a2-124">Tervezési és a kapcsolat topológia és a lista hello címterük összes hálózat akkor terv szeretné tooconnect.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-124">Design and plan your connectivity topology and list hello address spaces for all networks you want tooconnect.</span></span>
2. <span data-ttu-id="3a5a2-125">Hozzon létre egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-125">Create an Azure virtual network.</span></span> 
3. <span data-ttu-id="3a5a2-126">Hozzon létre egy VPN-átjáró hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-126">Create a VPN gateway for hello virtual network.</span></span>
4. <span data-ttu-id="3a5a2-127">Létrehozhat és konfigurálhat kapcsolatok tooon helyi hálózatok és a többi virtuális hálózatok (szükség szerint).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-127">Create and configure connections tooon-premises networks or other virtual networks (as needed).</span></span>
5. <span data-ttu-id="3a5a2-128">Létrehozhat és konfigurálhat egy pont – hely kapcsolat az Azure VPN gateway (szükség szerint).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-128">Create and configure a Point-to-Site connection for your Azure VPN gateway (as needed).</span></span>

## <span data-ttu-id="3a5a2-129"><a name="design"></a>Tervezési</span><span class="sxs-lookup"><span data-stu-id="3a5a2-129"><a name="design"></a>Design</span></span>
### <span data-ttu-id="3a5a2-130"><a name="topologies"></a>Kapcsolat topológiák</span><span class="sxs-lookup"><span data-stu-id="3a5a2-130"><a name="topologies"></a>Connection topologies</span></span>

<span data-ttu-id="3a5a2-131">Indítsa el a hello hello diagramok megtekintésével [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-131">Start by looking at hello diagrams in hello [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span> <span data-ttu-id="3a5a2-132">a cikkben hello egyszerű diagramokat, hello üzembe helyezési modellel minden topológia, és hello elérhető eszközök toodeploy a konfigurációt használhatja.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-132">hello article contains basic diagrams, hello deployment models for each topology, and hello available deployment tools you can use toodeploy your configuration.</span></span>

### <span data-ttu-id="3a5a2-133"><a name="designbasics"></a>Tervezési alapjai</span><span class="sxs-lookup"><span data-stu-id="3a5a2-133"><a name="designbasics"></a>Design basics</span></span>

<span data-ttu-id="3a5a2-134">a következő szakaszok hello hello VPN gateway alapjai tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-134">hello following sections discuss hello VPN gateway basics.</span></span> 

#### <span data-ttu-id="3a5a2-135"><a name="servicelimits"></a>Hálózatkezelési szolgáltatások korlátok</span><span class="sxs-lookup"><span data-stu-id="3a5a2-135"><a name="servicelimits"></a>Networking services limits</span></span>

<span data-ttu-id="3a5a2-136">Hello táblák tooview görgetéséhez [hálózati szolgáltatási korlátok](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-136">Scroll through hello tables tooview [networking services limits](../azure-subscription-service-limits.md#networking-limits).</span></span> <span data-ttu-id="3a5a2-137">felsorolt hello korlátok hatással lehet a tervező.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-137">hello limits listed may impact your design.</span></span>

#### <span data-ttu-id="3a5a2-138"><a name="subnets"></a>Alhálózatok kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="3a5a2-138"><a name="subnets"></a>About subnets</span></span>

<span data-ttu-id="3a5a2-139">Kapcsolatok létrehozásakor meg kell fontolnia alhálózati tartományt.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-139">When you are creating connections, you must consider your subnet ranges.</span></span> <span data-ttu-id="3a5a2-140">Nem lehet átfedésben lévő alhálózati címtartományt.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-140">You cannot have overlapping subnet address ranges.</span></span> <span data-ttu-id="3a5a2-141">Egy átfedő alhálózattal akkor, ha egy virtuális hálózat vagy a helyszíni helyen található hello ugyanazt a címtartományt, amely más helyen hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-141">An overlapping subnet is when one virtual network or on-premises location contains hello same address space that hello other location contains.</span></span> <span data-ttu-id="3a5a2-142">Ez azt jelenti, hogy van szüksége a hálózati mérnökök a helyszíni helyi hálózatok toocarve ki egy tartományt az Ön toouse a Azure IP-címzés terület/alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-142">This means that you need your network engineers for your local on-premises networks toocarve out a range for you toouse for your Azure IP addressing space/subnets.</span></span> <span data-ttu-id="3a5a2-143">Címterületek használatát, amelyek nem használják a hello helyi a helyi hálózaton van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-143">You need address space that is not being used on hello local on-premises network.</span></span>

<span data-ttu-id="3a5a2-144">Egymást átfedő alhálózatokat elkerülve az során is fontos dolgozik VNet – VNet kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-144">Avoiding overlapping subnets is also important when you are working with VNet-to-VNet connections.</span></span> <span data-ttu-id="3a5a2-145">Ha az alhálózat átfedésben vannak, és egy IP-cím szerepel a hello küld, és a célkiszolgáló Vnetek, VNet – VNet kapcsolatokhoz sikertelen.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-145">If your subnets overlap and an IP address exists in both hello sending and destination VNets, VNet-to-VNet connections fail.</span></span> <span data-ttu-id="3a5a2-146">Azure nem útvonal hello adatok toohello más VNet mert hello célcím hello küldése a virtuális hálózat része.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-146">Azure can't route hello data toohello other VNet because hello destination address is part of hello sending VNet.</span></span>

<span data-ttu-id="3a5a2-147">VPN-átjárók nevű egy átjáró-alhálózatot a megadott alhálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-147">VPN Gateways require a specific subnet called a gateway subnet.</span></span> <span data-ttu-id="3a5a2-148">Minden átjáró-alhálózat névvel kell ellátni a GatewaySubnet toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-148">All gateway subnets must be named GatewaySubnet toowork properly.</span></span> <span data-ttu-id="3a5a2-149">Ne feledje nem tooname az átjáró alhálózatának egy másik nevet, és nem telepítheti a virtuális gépek vagy semmi mást toohello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-149">Be sure not tooname your gateway subnet a different name, and don't deploy VMs or anything else toohello gateway subnet.</span></span> <span data-ttu-id="3a5a2-150">Lásd: [átjáró alhálózatok](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-150">See [Gateway Subnets](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span></span>

#### <span data-ttu-id="3a5a2-151"><a name="local"></a>Tudnivalók a helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="3a5a2-151"><a name="local"></a>About local network gateways</span></span>

<span data-ttu-id="3a5a2-152">hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-152">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="3a5a2-153">Hello klasszikus üzembe helyezési modellel hello helyi hálózati átjáró hivatkozott tooas egy helyi hálózati telephely.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-153">In hello classic deployment model, hello local network gateway is referred tooas a Local Network Site.</span></span> <span data-ttu-id="3a5a2-154">A helyi hálózati átjáró konfigurálásakor ehhez adjon neki egy nevet, adja meg a nyilvános IP-címe hello hello a helyszíni VPN-eszközön, és hello helyszíni helyen hello címelőtagok.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-154">When you configure a local network gateway, you give it a name, specify hello public IP address of hello on-premises VPN device, and specify hello address prefixes that are in hello on-premises location.</span></span> <span data-ttu-id="3a5a2-155">Azure ellenőrzi, hogy az hello cél címelőtagokat a hálózati forgalom, hello helyi hálózati átjáró megadott hello konfigurációs olvas, és ennek megfelelően irányítja a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-155">Azure looks at hello destination address prefixes for network traffic, consults hello configuration that you have specified for hello local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="3a5a2-156">Hello címelőtagokat igény szerint módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-156">You can modify hello address prefixes as needed.</span></span> <span data-ttu-id="3a5a2-157">További információkért lásd: [helyi hálózati átjárók](vpn-gateway-about-vpn-gateway-settings.md#lng).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-157">For more information, see [Local network gateways](vpn-gateway-about-vpn-gateway-settings.md#lng).</span></span>

#### <span data-ttu-id="3a5a2-158"><a name="gwtype"></a>Átjáró típusával kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="3a5a2-158"><a name="gwtype"></a>About gateway types</span></span>

<span data-ttu-id="3a5a2-159">A topológia hello megfelelő átjáró típusának kiválasztása fontos.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-159">Selecting hello correct gateway type for your topology is critical.</span></span> <span data-ttu-id="3a5a2-160">Ha hello megfelelő típusú választja, az átjáró nem fog megfelelően működni.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-160">If you select hello wrong type, your gateway won't work properly.</span></span> <span data-ttu-id="3a5a2-161">hello átjáró típusa határozza meg, hogyan hello átjáró csatlakozik, és a szükséges konfigurációs beállítást hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-161">hello gateway type specifies how hello gateway itself connects and is a required configuration setting for hello Resource Manager deployment model.</span></span>

<span data-ttu-id="3a5a2-162">hello átjáró típusok a következők:</span><span class="sxs-lookup"><span data-stu-id="3a5a2-162">hello gateway types are:</span></span>

* <span data-ttu-id="3a5a2-163">Vpn</span><span class="sxs-lookup"><span data-stu-id="3a5a2-163">Vpn</span></span>
* <span data-ttu-id="3a5a2-164">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="3a5a2-164">ExpressRoute</span></span>

#### <span data-ttu-id="3a5a2-165"><a name="connectiontype"></a>Kapcsolattípusok kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="3a5a2-165"><a name="connectiontype"></a>About connection types</span></span>

<span data-ttu-id="3a5a2-166">Minden egyes konfiguráció esetében egy adott kapcsolattípusra van szükség.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-166">Each configuration requires a specific connection type.</span></span> <span data-ttu-id="3a5a2-167">hello kapcsolattípusai a következők:</span><span class="sxs-lookup"><span data-stu-id="3a5a2-167">hello connection types are:</span></span>

* <span data-ttu-id="3a5a2-168">IPsec</span><span class="sxs-lookup"><span data-stu-id="3a5a2-168">IPsec</span></span>
* <span data-ttu-id="3a5a2-169">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="3a5a2-169">Vnet2Vnet</span></span>
* <span data-ttu-id="3a5a2-170">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="3a5a2-170">ExpressRoute</span></span>
* <span data-ttu-id="3a5a2-171">VPNClient</span><span class="sxs-lookup"><span data-stu-id="3a5a2-171">VPNClient</span></span>

#### <span data-ttu-id="3a5a2-172"><a name="vpntype"></a>Információ a VPN-típusai</span><span class="sxs-lookup"><span data-stu-id="3a5a2-172"><a name="vpntype"></a>About VPN types</span></span>

<span data-ttu-id="3a5a2-173">Minden egyes konfiguráció szükséges egy adott VPN-típus.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-173">Each configuration requires a specific VPN type.</span></span> <span data-ttu-id="3a5a2-174">Ha meg vannak kombinálásával két konfigurációkat, például egy webhelyek kapcsolat és egy pont – hely kapcsolat toohello hozhat létre ugyanazon virtuális kell használnia a VPN-típus, amely eleget tesz a mindkét kapcsolódási követelményeihez.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-174">If you are combining two configurations, such as creating a Site-to-Site connection and a Point-to-Site connection toohello same VNet, you must use a VPN type that satisfies both connection requirements.</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="3a5a2-175">hello alábbi táblázatokban hello VPN-típus, le kell képezni tooeach kapcsolat konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-175">hello following tables show hello VPN type as it maps tooeach connection configuration.</span></span> <span data-ttu-id="3a5a2-176">Győződjön meg arról, hogy hello az átjáró megfelel hello konfigurációjában, amelyet az toocreate VPN-típus.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-176">Make sure hello VPN type for your gateway matches hello configuration that you want toocreate.</span></span> 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <span data-ttu-id="3a5a2-177"><a name="devices"></a>A pont-pont kapcsolatok VPN-eszközök</span><span class="sxs-lookup"><span data-stu-id="3a5a2-177"><a name="devices"></a>VPN devices for Site-to-Site connections</span></span>

<span data-ttu-id="3a5a2-178">a következő elemek hello kell tooconfigure a pont-pont kapcsolat, függetlenül a telepítési modell:</span><span class="sxs-lookup"><span data-stu-id="3a5a2-178">tooconfigure a Site-to-Site connection, regardless of deployment model, you need hello following items:</span></span>

* <span data-ttu-id="3a5a2-179">A VPN-eszköz, amely kompatibilis a Azure VPN gatewayek</span><span class="sxs-lookup"><span data-stu-id="3a5a2-179">A VPN device that is compatible with Azure VPN gateways</span></span>
* <span data-ttu-id="3a5a2-180">Egy nyilvánosan elérhető IPv4 IP-címet, amely nincs NAT mögött.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-180">A public-facing IPv4 IP address that is not behind a NAT</span></span>

<span data-ttu-id="3a5a2-181">A VPN-eszköz konfigurálása toohave élmény van szüksége, vagy rendelkezik valaki, amely hello eszköz konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-181">You need toohave experience configuring your VPN device, or have someone that can configure hello device for you.</span></span>

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <span data-ttu-id="3a5a2-182"><a name="forcedtunnel"></a>Fontolja meg kényszerített bújtatás Útválasztás</span><span class="sxs-lookup"><span data-stu-id="3a5a2-182"><a name="forcedtunnel"></a>Consider forced tunnel routing</span></span>

<span data-ttu-id="3a5a2-183">A legtöbb konfigurációk esetén konfigurálhatja a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-183">For most configurations, you can configure forced tunneling.</span></span> <span data-ttu-id="3a5a2-184">A kényszerített bújtatás lehetővé átirányítási vagy "kényszerített" minden Internet adathoz kötött forgalom hátsó tooyour helyszíni hely vizsgálati és naplózási pont-pont VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-184">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="3a5a2-185">Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-185">This is a critical security requirement for most enterprise IT policies.</span></span> 

<span data-ttu-id="3a5a2-186">Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban lesz mindig haladnak át közvetlenül kimenő Internet, toohello nélkül hello beállítás tooallow Azure hálózati infrastruktúráról, tooinspect vagy a napló hello forgalom.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-186">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="3a5a2-187">Jogosulatlan Internet-hozzáférés is eredményezhet, tooinformation nyilvánosságra vagy más típusú biztonsági problémákat.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-187">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

<span data-ttu-id="3a5a2-188">A kényszerített bújtatás kapcsolat két üzembe helyezési modell és a különböző eszközök használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-188">A forced tunneling connection can be configured in both deployment models and by using different tools.</span></span> <span data-ttu-id="3a5a2-189">További információkért lásd: [kényszerített bújtatás konfigurálása](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-189">For more information, see [Configure forced tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>

<span data-ttu-id="3a5a2-190">**A kényszerített bújtatás diagramja**</span><span class="sxs-lookup"><span data-stu-id="3a5a2-190">**Forced tunneling diagram**</span></span>

![Az Azure VPN Gateway kényszerített bújtatás diagramja](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a><span data-ttu-id="3a5a2-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a5a2-192">Next steps</span></span>

<span data-ttu-id="3a5a2-193">Lásd: hello [VPN Gateway – gyakori kérdések](vpn-gateway-vpn-faq.md) és [VPN-átjáró](vpn-gateway-about-vpngateways.md) cikkek esetében további információk toohelp, a kialakítással.</span><span class="sxs-lookup"><span data-stu-id="3a5a2-193">See hello [VPN Gateway FAQ](vpn-gateway-vpn-faq.md) and [About VPN Gateway](vpn-gateway-about-vpngateways.md) articles for more information toohelp you with your design.</span></span>

<span data-ttu-id="3a5a2-194">Adott átjáró beállításaival kapcsolatos további információkért lásd: [kapcsolatos VPN-átjáró beállítások](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="3a5a2-194">For more information about specific gateway settings, see [About VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>