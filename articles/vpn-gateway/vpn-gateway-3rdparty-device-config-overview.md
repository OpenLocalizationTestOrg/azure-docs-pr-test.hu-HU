---
title: "aaaAbout 3. fél VPN eszköz konfigurációs tooconnect tooAzure VPN-átjárók |} Microsoft Docs"
description: "Ez a cikk áttekintést 3. fél VPN eszközkonfigurációk tooAzure VPN-átjárók csatlakozni."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 3bb4fc94bc625386c2d0a02e1dcbdeb38ee0665e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-3rd-party-vpn-device-configurations"></a><span data-ttu-id="6c0b0-103">3. fél VPN eszközkonfigurációk áttekintése</span><span class="sxs-lookup"><span data-stu-id="6c0b0-103">Overview of 3rd party VPN device configurations</span></span>
<span data-ttu-id="6c0b0-104">Ez a cikk áttekintést a helyszíni VPN eszközkonfigurációk tooAzure VPN-átjárók csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-104">This article provides an overview of on-premises VPN device configurations for connecting tooAzure VPN gateways.</span></span> <span data-ttu-id="6c0b0-105">hello minta Azure-beli virtuális hálózat és a VPN gateway telepítő lesz használt tooconnect toodifferent a helyszíni VPN-eszközök hello ugyanazokkal a paraméterekkel lehet.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-105">hello sample Azure virtual network and VPN gateway setup will be used tooconnect toodifferent on-premises VPN devices with hello same parameters.</span></span>

## <a name="device-requirements"></a><span data-ttu-id="6c0b0-106">Eszközkövetelmények</span><span class="sxs-lookup"><span data-stu-id="6c0b0-106">Device requirements</span></span>
<span data-ttu-id="6c0b0-107">Az Azure VPN gatewayek szabványos IPsec/IKE protokoll csomagok használata S2S VPN-alagutat.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-107">Azure VPN gateways use standard IPsec/IKE protocol suites for S2S VPN tunnels.</span></span> <span data-ttu-id="6c0b0-108">Tekintse meg a túl[kapcsolatos VPN-eszközök](vpn-gateway-about-vpn-devices.md) hello részletes IPsec/IKE protokoll paraméterek és az alapértelmezett titkosítási algoritmusok az Azure VPN gatewayek.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-108">Refer too[About VPN devices](vpn-gateway-about-vpn-devices.md) for hello detailed IPsec/IKE protocol parameters and default cryptographic algorithms for Azure VPN gateways.</span></span> <span data-ttu-id="6c0b0-109">Opcionálisan megadhat titkosítási algoritmusok és egy adott kapcsolat kulcs szintjeiről pontos kombinációja hello a leírtak szerint [titkosítási követelményekkel kapcsolatos](vpn-gateway-about-compliance-crypto.md).</span><span class="sxs-lookup"><span data-stu-id="6c0b0-109">You can optionally specify hello exact combination of cryptographic algorithms and key strengths for a specific connection as described in [About cryptographic requirements](vpn-gateway-about-compliance-crypto.md).</span></span>

## <span data-ttu-id="6c0b0-110"><a name ="singletunnel"></a>Egy VPN-alagút</span><span class="sxs-lookup"><span data-stu-id="6c0b0-110"><a name ="singletunnel"></a>Single VPN tunnel</span></span>
<span data-ttu-id="6c0b0-111">az Azure VPN gateway és a helyszíni VPN-eszköz között egyetlen S2S VPN-alagúton hello első topológia áll.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-111">hello first topology consists of a single S2S VPN tunnel between an Azure VPN gateway and your on-premises VPN device.</span></span> <span data-ttu-id="6c0b0-112">Konfigurálhatja BGP hello VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-112">You can optionally configure BGP across hello VPN tunnel.</span></span>

![az egyalagutas](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

<span data-ttu-id="6c0b0-114">Tekintse meg a túl[webhelyek kapcsolat beállítása](vpn-gateway-howto-site-to-site-resource-manager-portal.md) részletes, lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-114">Refer too[Configure site-to-site connection](vpn-gateway-howto-site-to-site-resource-manager-portal.md) for detailed, step-by-step guidance.</span></span> <span data-ttu-id="6c0b0-115">hello alábbi szakaszok felsorolják hello paraméterek és adjon meg egy minta PowerShell parancsfájl toohelp használatának első lépéseit.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-115">hello following sections list hello parameters and provide a sample PowerShell script toohelp you get started.</span></span>

### <a name="network-and-vpn-gateway-information"></a><span data-ttu-id="6c0b0-116">Hálózati és a VPN-átjáró adatai</span><span class="sxs-lookup"><span data-stu-id="6c0b0-116">Network and VPN gateway information</span></span>
<span data-ttu-id="6c0b0-117">Ez a szakasz fenti hello példák hello paramétereinek felsorolása.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-117">This section list hello parameters for hello examples above.</span></span>

| <span data-ttu-id="6c0b0-118">**A paraméter**</span><span class="sxs-lookup"><span data-stu-id="6c0b0-118">**Parameter**</span></span>                | <span data-ttu-id="6c0b0-119">**Érték**</span><span class="sxs-lookup"><span data-stu-id="6c0b0-119">**Value**</span></span>                    |
| ---                          | ---                          |
| <span data-ttu-id="6c0b0-120">VNet-címelőtagjait</span><span class="sxs-lookup"><span data-stu-id="6c0b0-120">VNet address prefixes</span></span>        | <span data-ttu-id="6c0b0-121">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c0b0-121">10.11.0.0/16</span></span><br><span data-ttu-id="6c0b0-122">10.12.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c0b0-122">10.12.0.0/16</span></span> |
| <span data-ttu-id="6c0b0-123">Az Azure VPN gateway IP-címet</span><span class="sxs-lookup"><span data-stu-id="6c0b0-123">Azure VPN gateway IP</span></span>         | <span data-ttu-id="6c0b0-124">Az Azure VPN Gateway IP</span><span class="sxs-lookup"><span data-stu-id="6c0b0-124">Azure VPN Gateway IP</span></span>         |
| <span data-ttu-id="6c0b0-125">A helyszíni címelőtagokat</span><span class="sxs-lookup"><span data-stu-id="6c0b0-125">On-premises address prefixes</span></span> | <span data-ttu-id="6c0b0-126">10.51.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c0b0-126">10.51.0.0/16</span></span><br><span data-ttu-id="6c0b0-127">10.52.0.0/16</span><span class="sxs-lookup"><span data-stu-id="6c0b0-127">10.52.0.0/16</span></span> |
| <span data-ttu-id="6c0b0-128">A helyszíni VPN-eszköz IP-címet</span><span class="sxs-lookup"><span data-stu-id="6c0b0-128">On-premises VPN device IP</span></span>    | <span data-ttu-id="6c0b0-129">A helyszíni VPN-eszköz IP-címet</span><span class="sxs-lookup"><span data-stu-id="6c0b0-129">On-premises VPN device IP</span></span>    |
| <span data-ttu-id="6c0b0-130">* VNet BGP ASN</span><span class="sxs-lookup"><span data-stu-id="6c0b0-130">*VNet BGP ASN</span></span>                | <span data-ttu-id="6c0b0-131">65010</span><span class="sxs-lookup"><span data-stu-id="6c0b0-131">65010</span></span>                        |
| <span data-ttu-id="6c0b0-132">* Az azure BGP-Társgép IP-</span><span class="sxs-lookup"><span data-stu-id="6c0b0-132">*Azure BGP peer IP</span></span>           | <span data-ttu-id="6c0b0-133">10.12.255.30</span><span class="sxs-lookup"><span data-stu-id="6c0b0-133">10.12.255.30</span></span>                 |
| <span data-ttu-id="6c0b0-134">* A helyszíni BGP ASN</span><span class="sxs-lookup"><span data-stu-id="6c0b0-134">*On-premises BGP ASN</span></span>         | <span data-ttu-id="6c0b0-135">65050</span><span class="sxs-lookup"><span data-stu-id="6c0b0-135">65050</span></span>                        |
| <span data-ttu-id="6c0b0-136">* A helyszíni BGP-Társgép IP-</span><span class="sxs-lookup"><span data-stu-id="6c0b0-136">*On-premises BGP peer IP</span></span>     | <span data-ttu-id="6c0b0-137">10.52.255.254</span><span class="sxs-lookup"><span data-stu-id="6c0b0-137">10.52.255.254</span></span>                |

* <span data-ttu-id="6c0b0-138">(*) Választható paraméterek: a BGP csak</span><span class="sxs-lookup"><span data-stu-id="6c0b0-138">(*) Optional parameters for BGP only</span></span>

### <a name="sample-powershell-script"></a><span data-ttu-id="6c0b0-139">PowerShell-parancsfájlpélda</span><span class="sxs-lookup"><span data-stu-id="6c0b0-139">Sample PowerShell script</span></span>
<span data-ttu-id="6c0b0-140">[PowerShell-lel S2S VPN-kapcsolatot](vpn-gateway-create-site-to-site-rm-powershell.md) rendelkezik hello részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-140">[Create a S2S VPN connection using PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) has hello detailed instructions.</span></span> <span data-ttu-id="6c0b0-141">Ez a témakör egy minta parancsfájlt tooget-t elindította.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-141">This section provides a sample script tooget you started.</span></span>

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
$RG1           = "TestRG1"
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
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect tooyour subscription and create a new resource group

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create hello S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <span data-ttu-id="6c0b0-142"><a name ="policybased"></a>[Választható] A "UsePolicyBasedTrafficSelectors" egyéni IPsec/IKE házirend</span><span class="sxs-lookup"><span data-stu-id="6c0b0-142"><a name ="policybased"></a> [Optional] Use custom IPsec/IKE policy with "UsePolicyBasedTrafficSelectors"</span></span>
<span data-ttu-id="6c0b0-143">Ha a VPN-eszközök nem támogatják a "bármely közöttiként" forgalom választók (útválasztó-alapú/VTI-alapú configuration), akkor toocreate IPsec/IKE egyéni házirendet kell és konfigurálását elvégzi "UsePolicyBasedTrafficSelectors" beállítás leírtak [Ez a cikk ](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6c0b0-143">If your VPN devices do not support "any-to-any" traffic selectors (route-based/VTI-based configuration), you will need toocreate a custom IPsec/IKE policy and configure "UsePolicyBasedTrafficSelectors" option as described in [this article](vpn-gateway-connect-multiple-policybased-rm-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6c0b0-144">Rendelés tooenable hello kapcsolat "UsePolicyBasedTrafficSelectors" kapcsolót egy IPsec-/ h.rend toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-144">You need toocreate an IPsec/IKE policy in order tooenable "UsePolicyBasedTrafficSelectors" option on hello connection.</span></span>

<span data-ttu-id="6c0b0-145">az alábbi hello mintaparancsfájl házirendet hoz létre IPsec/IKE hello algoritmusok és a paraméterek a következő:</span><span class="sxs-lookup"><span data-stu-id="6c0b0-145">hello sample script below creates an IPsec/IKE policy with hello following algorithms and parameters:</span></span>
* <span data-ttu-id="6c0b0-146">IKEv2: AES256, SHA384 DHGroup24</span><span class="sxs-lookup"><span data-stu-id="6c0b0-146">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="6c0b0-147">IPsec: AES256, SHA1, PFS24, SA élettartama 7200 másodperc & 20480000KB (20GB)</span><span class="sxs-lookup"><span data-stu-id="6c0b0-147">IPsec: AES256, SHA1, PFS24, SA Lifetime 7200 seconds & 20480000KB (20GB)</span></span>

<span data-ttu-id="6c0b0-148">Majd hello házirend vonatkozik, és lehetővé teszi, hogy a "UesPolicyBasedTrafficSelectors" hello kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-148">It then applies hello policy and enables "UesPolicyBasedTrafficSelectors" on hello connection.</span></span>

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <span data-ttu-id="6c0b0-149"><a name ="bgp"></a>[Választható] A BGP S2S VPN-kapcsolat használata</span><span class="sxs-lookup"><span data-stu-id="6c0b0-149"><a name ="bgp"></a>[Optional] Use BGP on S2S VPN connection</span></span>
<span data-ttu-id="6c0b0-150">Is használhat BGP hello kapcsolaton.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-150">You can optionally use BGP on hello connection.</span></span> <span data-ttu-id="6c0b0-151">Lásd: [VPN Gateway a BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6c0b0-151">See [BGP for VPN gateway](vpn-gateway-bgp-resource-manager-ps.md).</span></span> <span data-ttu-id="6c0b0-152">Két különbségek vannak:</span><span class="sxs-lookup"><span data-stu-id="6c0b0-152">There are two differences:</span></span>

<span data-ttu-id="6c0b0-153">hello helyszíni címelőtagokat lehet egy állomáshoz cím, hello a helyszíni BGP-Társgép IP-címe:</span><span class="sxs-lookup"><span data-stu-id="6c0b0-153">hello on-premises address prefixes can be a single host address, hello on-premises BGP peer IP address:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

<span data-ttu-id="6c0b0-154">Meg kell adni "-EnableBGP" túl$ True hello kapcsolat létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="6c0b0-154">You must set "-EnableBGP" too$True when creating hello connection:</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a><span data-ttu-id="6c0b0-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c0b0-155">Next steps</span></span>
<span data-ttu-id="6c0b0-156">Lásd: [aktív-aktív VPN-átjárók konfigurálása a létesítmények közötti és VNet – VNet kapcsolatokhoz](vpn-gateway-activeactive-rm-powershell.md) lépéseit tooconfigure aktív-aktív létesítmények közötti és VNet – VNet kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="6c0b0-156">See [Configuring Active-Active VPN Gateways for Cross-Premises and VNet-to-VNet Connections](vpn-gateway-activeactive-rm-powershell.md) for steps tooconfigure active-active cross-premises and VNet-to-VNet connections.</span></span>

