---
title: "Csatlakozás az Azure VPN átjárók toomultiple a helyi csoportházirend-alapú VPN-eszközök: Azure Resource Manager: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan konfigurálása Azure útvonalalapú VPN gateway toomultiple házirendalapú VPN-eszközök Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/27/2017
ms.author: yushwang
ms.openlocfilehash: 866c78d96305207106a66cc3300c355e4b6bfbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-vpn-gateways-toomultiple-on-premises-policy-based-vpn-devices-using-powershell"></a><span data-ttu-id="cc22f-103">Csatlakozás Azure VPN-átjárók toomultiple helyszíni házirendalapú VPN eszközök PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="cc22f-103">Connect Azure VPN gateways toomultiple on-premises policy-based VPN devices using PowerShell</span></span>

<span data-ttu-id="cc22f-104">Ez a cikk segítséget nyújt az Azure útválasztó-alapú VPN átjáró tooconnect toomultiple a helyi csoportházirend-alapú VPN-eszközök egyéni IPsec/IKE-házirendek S2S VPN-kapcsolatok számát, ami konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="cc22f-104">This article helps you configure an Azure route-based VPN gateway tooconnect toomultiple on-premises policy-based VPN devices leveraging custom IPsec/IKE policies on S2S VPN connections.</span></span>

## <a name="about-policy-based-and-route-based-vpn-gateways"></a><span data-ttu-id="cc22f-105">Házirend- és útvonal-alapú VPN gatewayek kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="cc22f-105">About policy-based and route-based VPN gateways</span></span>

<span data-ttu-id="cc22f-106">Házirend - *és* útvonalalapú VPN-eszközök nem egyezik a hello IPsec forgalom választók kapcsolat beállításának módját:</span><span class="sxs-lookup"><span data-stu-id="cc22f-106">Policy- *vs.* route-based VPN devices differ in how hello IPsec traffic selectors are set on a connection:</span></span>

* <span data-ttu-id="cc22f-107">**Csoportházirend-alapú** VPN-eszközök használata mindkét hálózatok toodefine hogyan forgalom titkosítása/visszafejtése IPsec-alagutakon keresztül előtagjait hello kombinációi.</span><span class="sxs-lookup"><span data-stu-id="cc22f-107">**Policy-based** VPN devices use hello combinations of prefixes from both networks toodefine how traffic is encrypted/decrypted through IPsec tunnels.</span></span> <span data-ttu-id="cc22f-108">Hálózaticsomag-szűrés végző tűzfal eszközök általában épül.</span><span class="sxs-lookup"><span data-stu-id="cc22f-108">It is typically built on firewall devices that perform packet filtering.</span></span> <span data-ttu-id="cc22f-109">IPsec-alagút titkosítási és visszafejtési kerülnek toohello hálózaticsomag-szűrés és a program.</span><span class="sxs-lookup"><span data-stu-id="cc22f-109">IPsec tunnel encryption and decryption are added toohello packet filtering and processing engine.</span></span>
* <span data-ttu-id="cc22f-110">**Útválasztó-alapú** VPN-eszközök használata bármilyen közöttiként (helyettesítő) forgalmat választók, és lehetővé teszik útválasztás/továbbító táblák közvetlen forgalom toodifferent IPsec-alagutak.</span><span class="sxs-lookup"><span data-stu-id="cc22f-110">**Route-based** VPN devices use any-to-any (wildcard) traffic selectors, and let routing/forwarding tables direct traffic toodifferent IPsec tunnels.</span></span> <span data-ttu-id="cc22f-111">Ha minden IPsec-alagutat van modellezve a hálózati adaptert vagy VTI (virtuális alagút-illesztő) útválasztó platformokon általában be van építve.</span><span class="sxs-lookup"><span data-stu-id="cc22f-111">It is typically built on router platforms where each IPsec tunnel is modeled as a network interface or VTI (virtual tunnel interface).</span></span>

<span data-ttu-id="cc22f-112">hello alábbi ábrák jelöljön ki két modell hello:</span><span class="sxs-lookup"><span data-stu-id="cc22f-112">hello following diagrams highlight hello two models:</span></span>

### <a name="policy-based-vpn-example"></a><span data-ttu-id="cc22f-113">A házirendalapú VPN – példa</span><span class="sxs-lookup"><span data-stu-id="cc22f-113">Policy-based VPN example</span></span>
![Csoportházirend-alapú](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a><span data-ttu-id="cc22f-115">Útvonalalapú VPN – példa</span><span class="sxs-lookup"><span data-stu-id="cc22f-115">Route-based VPN example</span></span>
![útválasztó-alapú](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a><span data-ttu-id="cc22f-117">Azure házirendalapú VPN-támogatás</span><span class="sxs-lookup"><span data-stu-id="cc22f-117">Azure support for policy-based VPN</span></span>
<span data-ttu-id="cc22f-118">Jelenleg Azure támogatja-e a VPN-átjárók mindkét módnál: útvonalalapú VPN-átjárók és a csoportházirend-alapú VPN gatewayek.</span><span class="sxs-lookup"><span data-stu-id="cc22f-118">Currently, Azure supports both modes of VPN gateways: route-based VPN gateways and policy-based VPN gateways.</span></span> <span data-ttu-id="cc22f-119">A beépített különböző platformokon belső, a különböző specifikációihoz eredmények:</span><span class="sxs-lookup"><span data-stu-id="cc22f-119">They are built on different internal platforms, which result in different specifications:</span></span>

|                          | <span data-ttu-id="cc22f-120">**PolicyBased VPN-átjáró**</span><span class="sxs-lookup"><span data-stu-id="cc22f-120">**PolicyBased VPN Gateway**</span></span> | <span data-ttu-id="cc22f-121">**RouteBased VPN-átjáró**</span><span class="sxs-lookup"><span data-stu-id="cc22f-121">**RouteBased VPN Gateway**</span></span>               |
| ---                      | ---                         | ---                                      |
| <span data-ttu-id="cc22f-122">**Az Azure átjáró-Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="cc22f-122">**Azure Gateway SKU**</span></span>    | <span data-ttu-id="cc22f-123">Basic</span><span class="sxs-lookup"><span data-stu-id="cc22f-123">Basic</span></span>                       | <span data-ttu-id="cc22f-124">Basic, Standard, a HighPerformance</span><span class="sxs-lookup"><span data-stu-id="cc22f-124">Basic, Standard, HighPerformance</span></span>         |
| <span data-ttu-id="cc22f-125">**IKE-verzió**</span><span class="sxs-lookup"><span data-stu-id="cc22f-125">**IKE version**</span></span>          | <span data-ttu-id="cc22f-126">IKEv1</span><span class="sxs-lookup"><span data-stu-id="cc22f-126">IKEv1</span></span>                       | <span data-ttu-id="cc22f-127">IKEv2</span><span class="sxs-lookup"><span data-stu-id="cc22f-127">IKEv2</span></span>                                    |
| <span data-ttu-id="cc22f-128">**Max. S2S-kapcsolatok**</span><span class="sxs-lookup"><span data-stu-id="cc22f-128">**Max. S2S connections**</span></span> | <span data-ttu-id="cc22f-129">**1**</span><span class="sxs-lookup"><span data-stu-id="cc22f-129">**1**</span></span>                       | <span data-ttu-id="cc22f-130">Basic vagy Standard: 10</span><span class="sxs-lookup"><span data-stu-id="cc22f-130">Basic/Standard: 10</span></span><br> <span data-ttu-id="cc22f-131">HighPerformance: 30</span><span class="sxs-lookup"><span data-stu-id="cc22f-131">HighPerformance: 30</span></span> |
|                          |                             |                                          |

<span data-ttu-id="cc22f-132">Az egyéni IPsec/IKE házirend hello, most már beállíthatja Azure útvonalalapú VPN átjárók toouse előtag-alapú forgalom választók kapcsolóval "**PolicyBasedTrafficSelectors**", tooconnect tooon helyi csoportházirend-alapú VPN-eszközök.</span><span class="sxs-lookup"><span data-stu-id="cc22f-132">With hello custom IPsec/IKE policy, you can now configure Azure route-based VPN gateways toouse prefix-based traffic selectors with option "**PolicyBasedTrafficSelectors**", tooconnect tooon-premises policy-based VPN devices.</span></span> <span data-ttu-id="cc22f-133">Ez a funkció lehetővé teszi az Azure virtuális hálózat tooconnect és VPN-átjáró toomultiple a helyszíni házirendalapú VPN/tűzfal eszközök, hello aktuális Azure házirendalapú VPN gatewayek eltávolítása a hello egyetlen kapcsolathoz megadott korlátot.</span><span class="sxs-lookup"><span data-stu-id="cc22f-133">This capability allows you tooconnect from an Azure virtual network and VPN gateway toomultiple on-premises policy-based VPN/firewall devices, removing hello single connection limit from hello current Azure policy-based VPN gateways.</span></span>

> [!IMPORTANT]
> 1. <span data-ttu-id="cc22f-134">tooenable a kapcsolatot, a helyi csoportházirend-alapú VPN-eszközök támogatnia kell a **IKEv2** tooconnect toohello Azure VPN gatewayek útválasztó-alapú.</span><span class="sxs-lookup"><span data-stu-id="cc22f-134">tooenable this connectivity, your on-premises policy-based VPN devices must support **IKEv2** tooconnect toohello Azure route-based VPN gateways.</span></span> <span data-ttu-id="cc22f-135">Ellenőrizze a VPN-eszköz specifikációja.</span><span class="sxs-lookup"><span data-stu-id="cc22f-135">Check your VPN device specifications.</span></span>
> 2. <span data-ttu-id="cc22f-136">a mechanizmus a házirendalapú VPN-eszközök keresztül csatlakozó hello helyszíni hálózatokhoz csak kapcsolódhatnak toohello Azure virtuális hálózat; **azok nem tranzit tooother a helyszíni hálózatokban vagy keresztül virtuális hálózatok ugyanazon Azure VPN gatewaynél hello**.</span><span class="sxs-lookup"><span data-stu-id="cc22f-136">hello on-premises networks connecting through policy-based VPN devices with this mechanism can only connect toohello Azure virtual network; **they cannot transit tooother on-premises networks or virtual networks via hello same Azure VPN gateway**.</span></span>
> 3. <span data-ttu-id="cc22f-137">hello konfigurációs beállítás hello egyéni IPsec/IKE kapcsolatkezelési házirendet részét képezi.</span><span class="sxs-lookup"><span data-stu-id="cc22f-137">hello configuration option is part of hello custom IPsec/IKE connection policy.</span></span> <span data-ttu-id="cc22f-138">Ha hello csoportházirend-alapú forgalom választó lehetőséget választja, meg kell adnia a teljes házirend hello (a IPsec/IKE titkosítás és sértetlenség algoritmusokról, a kulcs szintjeiről és a biztonsági Társítások élettartama).</span><span class="sxs-lookup"><span data-stu-id="cc22f-138">If you enable hello policy-based traffic selector option, you must specify hello complete policy (IPsec/IKE encryption and integrity algorithms, key strengths, and SA lifetimes).</span></span>

<span data-ttu-id="cc22f-139">a következő diagram hello jeleníti meg, miért tranzit útválasztást Azure VPN-átjárón keresztül nem működik együtt a házirend alapú beállítás hello:</span><span class="sxs-lookup"><span data-stu-id="cc22f-139">hello following diagram shows why transit routing via Azure VPN gateway doesn't work with hello policy-based option:</span></span>

![Csoportházirend-alapú átvitel közben](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

<span data-ttu-id="cc22f-141">Hello ábrán is látható, hello Azure VPN gateway hello virtuális hálózati tooeach hello a helyszíni hálózat előtagjai, de nem hello kereszt-kapcsolat előtagok a forgalom választók van.</span><span class="sxs-lookup"><span data-stu-id="cc22f-141">As shown in hello diagram, hello Azure VPN gateway has traffic selectors from hello virtual network tooeach of hello on-premises network prefixes, but not hello cross-connection prefixes.</span></span> <span data-ttu-id="cc22f-142">Például a helyszíni hely 2, 3 hely és hely 4 egyes kommunikálhatnak tooVNet1 rendre, de hello Azure VPN gateway tooeach más keresztül nem tud csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="cc22f-142">For example, on-premises site 2, site 3, and site 4 can each communicate tooVNet1 respectively, but cannot connect via hello Azure VPN gateway tooeach other.</span></span> <span data-ttu-id="cc22f-143">hello látható hello-elosztó, amely nem szerepel a hello Azure VPN gateway ilyen konfiguráció forgalom választók.</span><span class="sxs-lookup"><span data-stu-id="cc22f-143">hello diagram shows hello cross-connect traffic selectors that are not available in hello Azure VPN gateway under this configuration.</span></span>

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="cc22f-144">Csoportházirend-alapú forgalom választók a kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc22f-144">Configure policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="cc22f-145">hello Ez a cikk kövesse az utasításokat hello azonos példában ismertetett módon [konfigurálása IPsec/IKE házirend S2S és VNet – VNet kapcsolatokhoz](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish S2S VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="cc22f-145">hello instructions in this article follow hello same example as described in [Configure IPsec/IKE policy for S2S or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish a S2S VPN connection.</span></span> <span data-ttu-id="cc22f-146">Ez a következő diagram hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="cc22f-146">This is shown in hello following diagram:</span></span>

![s2s-házirend](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

<span data-ttu-id="cc22f-148">munkafolyamat tooenable Ez az összekapcsolhatóság hello:</span><span class="sxs-lookup"><span data-stu-id="cc22f-148">hello workflow tooenable this connectivity:</span></span>
1. <span data-ttu-id="cc22f-149">Hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró a létesítmények közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc22f-149">Create hello virtual network, VPN gateway, and local network gateway for your cross-premises connection</span></span>
2. <span data-ttu-id="cc22f-150">IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc22f-150">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="cc22f-151">Hello házirend alkalmazása, ha S2S vagy VNet – VNet-kapcsolatot, és **hello csoportházirend-alapú forgalom választók engedélyezése** hello kapcsolaton</span><span class="sxs-lookup"><span data-stu-id="cc22f-151">Apply hello policy when you create a S2S or VNet-to-VNet connection, and **enable hello policy-based traffic selectors** on hello connection</span></span>
4. <span data-ttu-id="cc22f-152">Ha hello kapcsolatot hozott létre, alkalmazása vagy hello házirend tooan meglévő kapcsolat frissítése</span><span class="sxs-lookup"><span data-stu-id="cc22f-152">If hello connection is already created, you can apply or update hello policy tooan existing connection</span></span>

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="cc22f-153">Csoportházirend-alapú forgalom választók kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cc22f-153">Enable policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="cc22f-154">Győződjön meg arról, hogy befejeződött [3. rész hello konfigurálása IPsec/IKE házirend cikk](vpn-gateway-ipsecikepolicy-rm-powershell.md) az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cc22f-154">Make sure you have completed [Part 3 of hello Configure IPsec/IKE policy article](vpn-gateway-ipsecikepolicy-rm-powershell.md) for this section.</span></span> <span data-ttu-id="cc22f-155">a következő példában hello hello azonos paraméterek és lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cc22f-155">hello following example uses hello same parameters and steps:</span></span>

### <a name="step-1---create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="cc22f-156">1. lépés – hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc22f-156">Step 1 - Create hello virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables--connect-tooyour-subscription"></a><span data-ttu-id="cc22f-157">1. Deklarálja a változókat, és csatlakoztathatja tooyour előfizetés</span><span class="sxs-lookup"><span data-stu-id="cc22f-157">1. Declare your variables & connect tooyour subscription</span></span>
<span data-ttu-id="cc22f-158">Ehhez a gyakorlathoz először is deklarálni kell a változókat.</span><span class="sxs-lookup"><span data-stu-id="cc22f-158">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="cc22f-159">Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="cc22f-159">Be sure tooreplace hello values with your own when configuring for production.</span></span>

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```
<span data-ttu-id="cc22f-160">Erőforrás-kezelő parancsmagok toouse hello váltson át tooPowerShell mód.</span><span class="sxs-lookup"><span data-stu-id="cc22f-160">toouse hello Resource Manager cmdlets, make sure you switch tooPowerShell mode.</span></span> <span data-ttu-id="cc22f-161">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="cc22f-161">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="cc22f-162">Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="cc22f-162">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="cc22f-163">A következő minta toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="cc22f-163">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="cc22f-164">2. Hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc22f-164">2. Create hello virtual network, VPN gateway, and local network gateway</span></span>
<span data-ttu-id="cc22f-165">a következő példa hello hello virtuális hálózatot, három alhálózatot, és hello VPN-átjáró TestVNet1 hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cc22f-165">hello following example creates hello virtual network, TestVNet1 with three subnets, and hello VPN gateway.</span></span> <span data-ttu-id="cc22f-166">És az értékeket, fontos, hogy mindig nevezze el az átjáró alhálózatának kifejezetten a "GatewaySubnet".</span><span class="sxs-lookup"><span data-stu-id="cc22f-166">When substituting values, it's important that you always name your gateway subnet specifically 'GatewaySubnet'.</span></span> <span data-ttu-id="cc22f-167">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="cc22f-167">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a><span data-ttu-id="cc22f-168">2. lépés - az IPsec/IKE házirendjével S2S VPN-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc22f-168">Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="cc22f-169">1. IPsec/IKE-házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc22f-169">1. Create an IPsec/IKE policy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cc22f-170">Rendelés tooenable hello kapcsolat "UsePolicyBasedTrafficSelectors" kapcsolót egy IPsec-/ h.rend toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="cc22f-170">You need toocreate an IPsec/IKE policy in order tooenable "UsePolicyBasedTrafficSelectors" option on hello connection.</span></span>

<span data-ttu-id="cc22f-171">hello alábbi példa házirendet hoz létre IPsec/IKE ezeket az algoritmusokat és paraméterek:</span><span class="sxs-lookup"><span data-stu-id="cc22f-171">hello following example creates an IPsec/IKE policy with these algorithms and parameters:</span></span>
* <span data-ttu-id="cc22f-172">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="cc22f-172">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="cc22f-173">IPsec: AES256, SHA-256, PFS24, 2048KB & SA élettartam 3600 másodperc</span><span class="sxs-lookup"><span data-stu-id="cc22f-173">IPsec: AES256, SHA256, PFS24, SA Lifetime 3600 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a><span data-ttu-id="cc22f-174">2. Hello S2S VPN-kapcsolat létrehozása a csoportházirend-alapú forgalom választók és IPsec/IKE házirend</span><span class="sxs-lookup"><span data-stu-id="cc22f-174">2. Create hello S2S VPN connection with policy-based traffic selectors and IPsec/IKE policy</span></span>
<span data-ttu-id="cc22f-175">S2S VPN-kapcsolat létrehozásához, és hello előző lépésben létrehozott hello IPsec/IKE házirend alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="cc22f-175">Create an S2S VPN connection and apply hello IPsec/IKE policy created in hello previous step.</span></span> <span data-ttu-id="cc22f-176">Ügyeljen a hello további paraméter "-UsePolicyBasedTrafficSelectors $True" lehetővé teszi a csoportházirend-alapú forgalom választók hello kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="cc22f-176">Be aware of hello additional parameter "-UsePolicyBasedTrafficSelectors $True"  which enables policy-based traffic selectors on hello connection.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="cc22f-177">Hello lépések végrehajtását követően hello S2S VPN-kapcsolat lesz hello IPsec/IKE házirend lett meghatározva, és engedélyezheti a házirend-alapú forgalom választók hello kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="cc22f-177">After completing hello steps, hello S2S VPN connection will use hello IPsec/IKE policy defined, and enable policy-based traffic selectors on hello connection.</span></span> <span data-ttu-id="cc22f-178">Ismételje meg az azonos lépések tooadd további kapcsolatok tooadditional a helyi csoportházirend-alapú VPN-eszközök a hello hello ugyanazon Azure VPN gatewaynél.</span><span class="sxs-lookup"><span data-stu-id="cc22f-178">You can repeat hello same steps tooadd more connections tooadditional on-premises policy-based VPN devices from hello same Azure VPN gateway.</span></span>

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a><span data-ttu-id="cc22f-179">Csoportházirend-alapú forgalom választók kapcsolat frissítése</span><span class="sxs-lookup"><span data-stu-id="cc22f-179">Update policy-based traffic selectors for a connection</span></span>
<span data-ttu-id="cc22f-180">hello utolsó szakasza bemutatja, hogyan tooupdate hello csoportházirend-alapú forgalom választók lehetőséget egy meglévő S2S VPN-kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="cc22f-180">hello last section shows you how tooupdate hello policy-based traffic selectors option for an existing  S2S VPN connection.</span></span>

### <a name="1-get-hello-connection"></a><span data-ttu-id="cc22f-181">1. Hello-kapcsolat lekérdezése</span><span class="sxs-lookup"><span data-stu-id="cc22f-181">1. Get hello connection</span></span>
<span data-ttu-id="cc22f-182">Hello kapcsolati erőforrást beolvasni.</span><span class="sxs-lookup"><span data-stu-id="cc22f-182">Get hello connection resource.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-hello-policy-based-traffic-selectors-option"></a><span data-ttu-id="cc22f-183">2. Jelölje be a házirend-alapú forgalom hello választók beállítást</span><span class="sxs-lookup"><span data-stu-id="cc22f-183">2. Check hello policy-based traffic selectors option</span></span>
<span data-ttu-id="cc22f-184">hello következő sor látható használja-e hello csoportházirend-alapú forgalom választók hello kapcsolathoz:</span><span class="sxs-lookup"><span data-stu-id="cc22f-184">hello following line shows whether hello policy-based traffic selectors are used for hello connection:</span></span>

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

<span data-ttu-id="cc22f-185">Ha hello sort adja vissza "**igaz**", majd csoportházirend-alapú forgalom választók hello kapcsolat van konfigurálva; egyéb esetben ad vissza "**hamis**."</span><span class="sxs-lookup"><span data-stu-id="cc22f-185">If hello line returns "**True**", then policy-based traffic selectors are configured on hello connection; otherwise it returns "**False**."</span></span>

### <a name="3-update-hello-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="cc22f-186">3. Hello csoportházirend-alapú forgalom választók a kapcsolat frissítése</span><span class="sxs-lookup"><span data-stu-id="cc22f-186">3. Update hello policy-based traffic selectors on a connection</span></span>
<span data-ttu-id="cc22f-187">Miután beszerezte hello kapcsolati erőforrást, engedélyezheti vagy hello tiltható le.</span><span class="sxs-lookup"><span data-stu-id="cc22f-187">Once you obtain hello connection resource, you can enable or disable hello option.</span></span>

#### <a name="disable-usepolicybasedtrafficselectors"></a><span data-ttu-id="cc22f-188">Tiltsa le a UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="cc22f-188">Disable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="cc22f-189">hello következő példa letiltja hello csoportházirend-alapú forgalom választók beállítást, de hagyja változatlanul IPsec/IKE házirend hello:</span><span class="sxs-lookup"><span data-stu-id="cc22f-189">hello following example disables hello policy-based traffic selectors option, but leaves hello IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a><span data-ttu-id="cc22f-190">UsePolicyBasedTrafficSelectors engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cc22f-190">Enable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="cc22f-191">hello következő példa engedélyezi hello csoportházirend-alapú forgalom választók, de hagyja változatlanul IPsec/IKE házirend hello:</span><span class="sxs-lookup"><span data-stu-id="cc22f-191">hello following example enables hello policy-based traffic selectors option, but leaves hello IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a><span data-ttu-id="cc22f-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc22f-192">Next steps</span></span>
<span data-ttu-id="cc22f-193">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="cc22f-193">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="cc22f-194">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cc22f-194">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>

<span data-ttu-id="cc22f-195">Emellett nézze át az [konfigurálása IPsec/IKE házirend S2S VPN- és VNet – VNet kapcsolatokhoz](vpn-gateway-ipsecikepolicy-rm-powershell.md) egyéni IPsec/IKE-házirendekkel kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="cc22f-195">Also review [Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) for more details on custom IPsec/IKE policies.</span></span>
