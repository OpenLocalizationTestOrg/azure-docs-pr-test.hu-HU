---
title: "Az Azure VPN Gatewayek beállítani a BGP Protokollt: erőforrás-kezelő: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan BGP konfigurálása az Azure VPN Gatewayek Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: a9d13ae6b319e2efa8965dc2955c9b89ac3fd12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-bgp-on-azure-vpn-gateways-using-powershell"></a><span data-ttu-id="ce06b-103">Hogyan tooconfigure BGP az Azure VPN Gatewayek PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="ce06b-103">How tooconfigure BGP on Azure VPN Gateways using PowerShell</span></span>
<span data-ttu-id="ce06b-104">Ez a cikk bemutatja, hogyan hello lépéseket tooenable BGP létesítmények közötti pont-pont (S2S) VPN-kapcsolat és egy VNet – VNet-kapcsolatot hello Resource Manager üzembe helyezési modellben és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="ce06b-104">This article walks you through hello steps tooenable BGP on a cross-premises Site-to-Site (S2S) VPN connection and a VNet-to-VNet connection using hello Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-bgp"></a><span data-ttu-id="ce06b-105">A BGP ismertetése</span><span class="sxs-lookup"><span data-stu-id="ce06b-105">About BGP</span></span>
<span data-ttu-id="ce06b-106">A BGP egy szabványos útválasztási protokoll hello általánosan használt az hello Internet tooexchange az Útválasztás és reachability information legalább két hálózat között.</span><span class="sxs-lookup"><span data-stu-id="ce06b-106">BGP is hello standard routing protocol commonly used in hello Internet tooexchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="ce06b-107">BGP hello Azure VPN Gatewayek és a helyszíni VPN-eszközök, a BGP-társakat vagy szomszédok teszi lehetővé, tooexchange "irányítja a", amely tájékoztatja a mindkét átjárók hello rendelkezésre állásáról és azok reachability előtagok toogo hello átjárók és az érintett útválasztók keresztül.</span><span class="sxs-lookup"><span data-stu-id="ce06b-107">BGP enables hello Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, tooexchange "routes" that will inform both gateways on hello availability and reachability for those prefixes toogo through hello gateways or routers involved.</span></span> <span data-ttu-id="ce06b-108">BGP is engedélyezhető tranzit útválasztást több hálózat között útvonalak BGP átjáró egy BGP-társ tooall való megtanulja propagálására más BGP-társak.</span><span class="sxs-lookup"><span data-stu-id="ce06b-108">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer tooall other BGP peers.</span></span>

<span data-ttu-id="ce06b-109">Lásd: [áttekintése a BGP az Azure VPN Gatewayek](vpn-gateway-bgp-overview.md) előnyeit a BGP és toounderstand hello technikai követelmények és szempontok BGP használatával további leírását.</span><span class="sxs-lookup"><span data-stu-id="ce06b-109">See [Overview of BGP with Azure VPN Gateways](vpn-gateway-bgp-overview.md) for more discussion on benefits of BGP and toounderstand hello technical requirements and considerations of using BGP.</span></span>

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a><span data-ttu-id="ce06b-110">Az Azure VPN gatewayek a BGP-első lépések</span><span class="sxs-lookup"><span data-stu-id="ce06b-110">Getting started with BGP on Azure VPN gateways</span></span>

<span data-ttu-id="ce06b-111">Ez a cikk bemutatja, hogyan hello lépéseket toodo hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="ce06b-111">This article walks you through hello steps toodo hello following tasks:</span></span>

* [<span data-ttu-id="ce06b-112">Engedélyezze az Azure VPN gateway a BGP, 1 -. rész</span><span class="sxs-lookup"><span data-stu-id="ce06b-112">Part 1 - Enable BGP on your Azure VPN gateway</span></span>](#enablebgp)
* [<span data-ttu-id="ce06b-113">2. rész – a BGP-létesítmények közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-113">Part 2 - Establish a cross-premises connection with BGP</span></span>](#crossprembgp)
* [<span data-ttu-id="ce06b-114">3. rész - BGP VNet – VNet-kapcsolatot létrehozni</span><span class="sxs-lookup"><span data-stu-id="ce06b-114">Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>](#v2vbgp)

<span data-ttu-id="ce06b-115">Hello utasításokat részét képező összes a BGP engedélyezve van a hálózati kapcsolatot az alapvető építőelemre képezi.</span><span class="sxs-lookup"><span data-stu-id="ce06b-115">Each part of hello instructions forms a basic building block for enabling BGP in your network connectivity.</span></span> <span data-ttu-id="ce06b-116">Ha befejezte az összes három részből, hello topológia elkészítette a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="ce06b-116">If you complete all three parts, you build hello topology as shown in hello following diagram:</span></span>

![BGP-topológiákkal](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

<span data-ttu-id="ce06b-118">Egyesítheti a kijelzők együtt toobuild egy összetettebb, Többugrásos, átmenő hálózatról, amely megfelel az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="ce06b-118">You can combine parts together toobuild a more complex, multi-hop, transit network that meets your needs.</span></span>

## <span data-ttu-id="ce06b-119"><a name ="enablebgp"></a>1. rész – hello Azure VPN Gateway a BGP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ce06b-119"><a name ="enablebgp"></a>Part 1 - Configure BGP on hello Azure VPN Gateway</span></span>
<span data-ttu-id="ce06b-120">hello konfigurációs lépések beállítása hello hello Azure VPN-átjáró BGP-paraméterek a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="ce06b-120">hello configuration steps set up hello BGP parameters of hello Azure VPN gateway as shown in hello following diagram:</span></span>

![BGP-átjáró](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a><span data-ttu-id="ce06b-122">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ce06b-122">Before you begin</span></span>
* <span data-ttu-id="ce06b-123">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="ce06b-123">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="ce06b-124">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ce06b-124">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ce06b-125">Hello Azure Resource Manager PowerShell-parancsmagjainak telepítése.</span><span class="sxs-lookup"><span data-stu-id="ce06b-125">Install hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="ce06b-126">PowerShell-parancsmagok hello telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ce06b-126">For more information about installing hello PowerShell cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="ce06b-127">1. lépés – létrehozása és VNet1 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ce06b-127">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="ce06b-128">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="ce06b-128">1. Declare your variables</span></span>
<span data-ttu-id="ce06b-129">Ehhez a gyakorlathoz először is deklarálni kell a változókat.</span><span class="sxs-lookup"><span data-stu-id="ce06b-129">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="ce06b-130">hello alábbi példa azt deklarálja hello változók ehhez a gyakorlathoz hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="ce06b-130">hello following example declares hello variables using hello values for this exercise.</span></span> <span data-ttu-id="ce06b-131">Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="ce06b-131">Be sure tooreplace hello values with your own when configuring for production.</span></span> <span data-ttu-id="ce06b-132">Ezek a változók is használhatja, ha ismeri a konfiguráció az ilyen típusú hello lépéseket toobecome keresztül futtatja.</span><span class="sxs-lookup"><span data-stu-id="ce06b-132">You can use these variables if you are running through hello steps toobecome familiar with this type of configuration.</span></span> <span data-ttu-id="ce06b-133">Hello változók módosítása, majd másolja és illessze be a PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="ce06b-133">Modify hello variables, and then copy and paste into your PowerShell console.</span></span>

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="ce06b-134">2. Csatlakozás tooyour előfizetés, és hozzon létre egy új erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ce06b-134">2. Connect tooyour subscription and create a new resource group</span></span>
<span data-ttu-id="ce06b-135">Erőforrás-kezelő parancsmagok toouse hello váltson át tooPowerShell mód.</span><span class="sxs-lookup"><span data-stu-id="ce06b-135">toouse hello Resource Manager cmdlets, Make sure you switch tooPowerShell mode.</span></span> <span data-ttu-id="ce06b-136">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ce06b-136">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="ce06b-137">Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="ce06b-137">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="ce06b-138">A következő minta toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="ce06b-138">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="ce06b-139">3. A TestVNet1 létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-139">3. Create TestVNet1</span></span>
<span data-ttu-id="ce06b-140">hello alábbi minta létrehoz egy TestVNet1 és három alhálózatok, egy hívott GatewaySubnet, egy hívott előtér és egy hívott háttér nevű virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="ce06b-140">hello following sample creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="ce06b-141">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="ce06b-141">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="ce06b-142">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="ce06b-142">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-bgp-parameters"></a><span data-ttu-id="ce06b-143">2. lépés – hello VPN-átjáró BGP-paraméterekkel rendelkező TestVNet1 létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-143">Step 2 - Create hello VPN Gateway for TestVNet1 with BGP parameters</span></span>
#### <a name="1-create-hello-ip-and-subnet-configurations"></a><span data-ttu-id="ce06b-144">1. Hello IP-cím és alhálózat-konfigurációk létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-144">1. Create hello IP and subnet configurations</span></span>
<span data-ttu-id="ce06b-145">A nyilvános IP-cím toobe lefoglalt toohello átjáró fog létrehozni a Vnethez tartozó kérelmet.</span><span class="sxs-lookup"><span data-stu-id="ce06b-145">Request a public IP address toobe allocated toohello gateway you will create for your VNet.</span></span> <span data-ttu-id="ce06b-146">Szükséges hello alhálózatot és IP-konfigurációk is fogja definiálni.</span><span class="sxs-lookup"><span data-stu-id="ce06b-146">You'll also define hello required subnet and IP configurations.</span></span>

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-hello-vpn-gateway-with-hello-as-number"></a><span data-ttu-id="ce06b-147">2. Hozzon létre hello VPN-átjáró hello SZÁMOT</span><span class="sxs-lookup"><span data-stu-id="ce06b-147">2. Create hello VPN gateway with hello AS number</span></span>
<span data-ttu-id="ce06b-148">TestVNet1 hello virtuális hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-148">Create hello virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="ce06b-149">A BGP TestVNet1 egy Útvonalalapú VPN-átjáró, valamint a hello hozzáadása paraméter - Asn-tooset hello ASN (AS-szám) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="ce06b-149">BGP requires a Route-Based VPN gateway, and also hello addition parameter, -Asn, tooset hello ASN (AS Number) for TestVNet1.</span></span> <span data-ttu-id="ce06b-150">Ha hello ASN paraméter nincs megadva, a ASN 65515 hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="ce06b-150">If you do not set hello ASN parameter, ASN 65515 is assigned.</span></span> <span data-ttu-id="ce06b-151">Átjáró létrehozása időt vehet igénybe (30 perc vagy több toocomplete).</span><span class="sxs-lookup"><span data-stu-id="ce06b-151">Creating a gateway can take a while (30 minutes or more toocomplete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-hello-azure-bgp-peer-ip-address"></a><span data-ttu-id="ce06b-152">3. Hello Azure BGP-Társgép IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="ce06b-152">3. Obtain hello Azure BGP Peer IP address</span></span>
<span data-ttu-id="ce06b-153">Hello átjáró létrehozása után kell tooobtain hello hello Azure VPN Gateway a BGP-Társgép IP-cím.</span><span class="sxs-lookup"><span data-stu-id="ce06b-153">Once hello gateway is created, you need tooobtain hello BGP Peer IP address on hello Azure VPN Gateway.</span></span> <span data-ttu-id="ce06b-154">Ez a cím szükséges tooconfigure hello Azure VPN Gateway, mint a BGP-partner a helyszíni VPN-eszközök.</span><span class="sxs-lookup"><span data-stu-id="ce06b-154">This address is needed tooconfigure hello Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

<span data-ttu-id="ce06b-155">hello utolsó parancs hello megfelelő BGP konfigurációk láthatók a hello Azure VPN Gateway; Példa:</span><span class="sxs-lookup"><span data-stu-id="ce06b-155">hello last command shows hello corresponding BGP configurations on hello Azure VPN Gateway; for example:</span></span>

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

<span data-ttu-id="ce06b-156">Hello átjáró létrehozása után is használhatja az átjáró tooestablish létesítmények közötti vagy VNet – VNet-kapcsolatot a BGP.</span><span class="sxs-lookup"><span data-stu-id="ce06b-156">Once hello gateway is created, you can use this gateway tooestablish cross-premises connection or VNet-to-VNet connection with BGP.</span></span> <span data-ttu-id="ce06b-157">a következő szakaszok a lépésein végighaladva hello lépéseket toocomplete hello gyakorlat hello.</span><span class="sxs-lookup"><span data-stu-id="ce06b-157">hello following sections walk through hello steps toocomplete hello exercise.</span></span>

## <span data-ttu-id="ce06b-158"><a name ="crossprembbgp"></a>2. rész – a BGP-létesítmények közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-158"><a name ="crossprembbgp"></a>Part 2 - Establish a cross-premises connection with BGP</span></span>

<span data-ttu-id="ce06b-159">tooestablish létesítmények közötti kapcsolatot, a helyi hálózati átjáró toorepresent toocreate szüksége, és a helyszíni VPN-eszköz kapcsolat tooconnect hello VPN-átjáró hello helyi hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="ce06b-159">tooestablish a cross-premises connection, you need toocreate a Local Network Gateway toorepresent your on-premises VPN device, and a Connection tooconnect hello VPN gateway with hello local network gateway.</span></span> <span data-ttu-id="ce06b-160">Nincsenek cikkek, amely ismerteti a fenti lépéseket, amíg a cikkben hello további tulajdonságok szükséges toospecify hello BGP konfigurációs paramétereket.</span><span class="sxs-lookup"><span data-stu-id="ce06b-160">While there are articles that walk you through these steps, this article contains hello additional properties required toospecify hello BGP configuration parameters.</span></span>

![A létesítmények közötti BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

<span data-ttu-id="ce06b-162">A továbblépés előtt ellenőrizze, hogy befejeződött [1. rész](#enablebgp) ebben a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="ce06b-162">Before proceeding, make sure you have completed [Part 1](#enablebgp) of this exercise.</span></span>

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a><span data-ttu-id="ce06b-163">1. lépés – létrehozása és hello helyi hálózati átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ce06b-163">Step 1 - Create and configure hello local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="ce06b-164">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="ce06b-164">1. Declare your variables</span></span>

<span data-ttu-id="ce06b-165">Ebben a gyakorlatban továbbra is toobuild hello konfigurációs hello ábrán is látható.</span><span class="sxs-lookup"><span data-stu-id="ce06b-165">This exercise continues toobuild hello configuration shown in hello diagram.</span></span> <span data-ttu-id="ce06b-166">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="ce06b-166">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

<span data-ttu-id="ce06b-167">Néhány dolgot toonote hello helyi hálózati átjáró paraméterek kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="ce06b-167">A couple of things toonote regarding hello local network gateway parameters:</span></span>

* <span data-ttu-id="ce06b-168">hello helyi hálózati átjáró lehetnek azonos hello, vagy más helyre és az erőforrás csoport mint hello VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="ce06b-168">hello local network gateway can be in hello same or different location and resource group as hello VPN gateway.</span></span> <span data-ttu-id="ce06b-169">Ez a példa bemutatja azokat a különböző erőforráscsoportokra különböző helyeken.</span><span class="sxs-lookup"><span data-stu-id="ce06b-169">This example shows them in different resource groups in different locations.</span></span>
* <span data-ttu-id="ce06b-170">a előtag minimális hello toodeclare hello helyi hálózati átjáró van szüksége az hello állomás cím a VPN-eszköz a BGP-Társgép IP-cím.</span><span class="sxs-lookup"><span data-stu-id="ce06b-170">hello minimum prefix you need toodeclare for hello local network gateway is hello host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="ce06b-171">Egy /32 ebben az esetben a "10.52.255.254/32" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="ce06b-171">In this case, it's a /32 prefix of "10.52.255.254/32".</span></span>
* <span data-ttu-id="ce06b-172">Ne feledje a helyszíni hálózatokhoz és az Azure VNet közötti különböző BGP ASN kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ce06b-172">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="ce06b-173">Ha azok hello vannak ugyanaz, meg kell toochange a VNet ASN Ha a helyszíni VPN-eszköz már használ hello ASN toopeer más BGP szomszédok.</span><span class="sxs-lookup"><span data-stu-id="ce06b-173">If they are hello same, you need toochange your VNet ASN if your on-premises VPN device already uses hello ASN toopeer with other BGP neighbors.</span></span>

<span data-ttu-id="ce06b-174">A folytatás előtt győződjön meg arról, hogy továbbra is csatlakoztatott tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="ce06b-174">Before you continue, make sure you are still connected tooSubscription 1.</span></span>

#### <a name="2-create-hello-local-network-gateway-for-site5"></a><span data-ttu-id="ce06b-175">2. Site5 hello helyi hálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-175">2. Create hello local network gateway for Site5</span></span>

<span data-ttu-id="ce06b-176">Lehet, hogy toocreate hello erőforráscsoport, abban az esetben, ha nem létrehozott hello helyi hálózati átjáró létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="ce06b-176">Be sure toocreate hello resource group if it is not created, before you create hello local network gateway.</span></span> <span data-ttu-id="ce06b-177">Figyelje meg hello két további paraméterek hello helyi hálózati átjáró: Asn és BgpPeerAddress.</span><span class="sxs-lookup"><span data-stu-id="ce06b-177">Notice hello two additional parameters for hello local network gateway: Asn and BgpPeerAddress.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="ce06b-178">2. lépés – hello hálózatok átjáró és a helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="ce06b-178">Step 2 - Connect hello VNet gateway and local network gateway</span></span>

#### <a name="1-get-hello-two-gateways"></a><span data-ttu-id="ce06b-179">1. Hello két átjáró beolvasása</span><span class="sxs-lookup"><span data-stu-id="ce06b-179">1. Get hello two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a><span data-ttu-id="ce06b-180">2. Hello TestVNet1 tooSite5 kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-180">2. Create hello TestVNet1 tooSite5 connection</span></span>

<span data-ttu-id="ce06b-181">Ebben a lépésben készíthet TestVNet1 tooSite5 hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ce06b-181">In this step, you create hello connection from TestVNet1 tooSite5.</span></span> <span data-ttu-id="ce06b-182">Adjon meg "-EnableBGP $True" tooenable BGP ehhez a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="ce06b-182">You must specify "-EnableBGP $True" tooenable BGP for this connection.</span></span> <span data-ttu-id="ce06b-183">A fentiekben taglaltak, akkor lehetséges toohave BGP- és -BGP kapcsolatokat ugyanazon Azure VPN Gateway hello.</span><span class="sxs-lookup"><span data-stu-id="ce06b-183">As discussed earlier, it is possible toohave both BGP and non-BGP connections for hello same Azure VPN Gateway.</span></span> <span data-ttu-id="ce06b-184">Ha BGP hello kapcsolat tulajdonságban nincs engedélyezve, Azure teszik lehetővé a BGP ehhez a kapcsolathoz annak ellenére, hogy a BGP-paraméterek már be van állítva mindkét átjárókon.</span><span class="sxs-lookup"><span data-stu-id="ce06b-184">Unless BGP is enabled in hello connection property, Azure will not enable BGP for this connection even though BGP parameters are already configured on both gateways.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

<span data-ttu-id="ce06b-185">hello alábbi példa felsorolja a helyszíni VPN-eszköz ebben a gyakorlatban hello BGP konfigurációs szakasz kezdenek hello paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ce06b-185">hello following example lists hello parameters you enter into hello BGP configuration section on your on-premises VPN device for this exercise:</span></span>

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being hello VPN tunnel interface on your device
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="ce06b-186">hello kapcsolat létrejötte után néhány percet, és hello BGP társviszony-létesítési munkamenet indítása hello IPsec-kapcsolat létrejötte után.</span><span class="sxs-lookup"><span data-stu-id="ce06b-186">hello connection is established after a few minutes, and hello BGP peering session starts once hello IPsec connection is established.</span></span>

## <span data-ttu-id="ce06b-187"><a name ="v2vbgp"></a>3. rész - BGP VNet – VNet-kapcsolatot létrehozni</span><span class="sxs-lookup"><span data-stu-id="ce06b-187"><a name ="v2vbgp"></a>Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>

<span data-ttu-id="ce06b-188">Ez a szakasz egy VNet – VNet-kapcsolatot, a BGP-vel ad hozzá, ahogy az ábra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ce06b-188">This section adds a VNet-to-VNet connection with BGP, as shown in hello following diagram:</span></span>

![A VNet – VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

<span data-ttu-id="ce06b-190">az utasításoknak hello hello előző lépéseiből továbbra is.</span><span class="sxs-lookup"><span data-stu-id="ce06b-190">hello following instructions continue from hello previous steps.</span></span> <span data-ttu-id="ce06b-191">Meg kell adnia a [i. rész](#enablebgp) toocreate hello VPN Gateway a BGP és TestVNet1 konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="ce06b-191">You must complete [Part I](#enablebgp) toocreate and configure TestVNet1 and hello VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a><span data-ttu-id="ce06b-192">1. lépés – TestVNet2 és hello VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-192">Step 1 - Create TestVNet2 and hello VPN gateway</span></span>

<span data-ttu-id="ce06b-193">Fontos, hogy egyetlen virtuális hálózat tartományt nem átfedésben hello IP-címtér hello új virtuális hálózat TestVNet2, toomake.</span><span class="sxs-lookup"><span data-stu-id="ce06b-193">It is important toomake sure that hello IP address space of hello new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="ce06b-194">Ebben a példában a virtuális hálózatok hello toohello tartozik ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ce06b-194">In this example, hello virtual networks belong toohello same subscription.</span></span> <span data-ttu-id="ce06b-195">Beállíthatja a VNet – VNet kapcsolatokhoz különböző előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="ce06b-195">You can set up VNet-to-VNet connections between different subscriptions.</span></span> <span data-ttu-id="ce06b-196">További információkért lásd: [VNet – VNet-kapcsolatot konfiguráló](vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ce06b-196">For more information, see [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md).</span></span> <span data-ttu-id="ce06b-197">Ellenőrizze, hogy hello "-EnableBgp $True" Ha hello kapcsolatok tooenable BGP létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ce06b-197">Make sure you add hello "-EnableBgp $True" when creating hello connections tooenable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="ce06b-198">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="ce06b-198">1. Declare your variables</span></span>

<span data-ttu-id="ce06b-199">Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="ce06b-199">Be sure tooreplace hello values with hello ones that you want toouse for your configuration.</span></span>

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a><span data-ttu-id="ce06b-200">2. TestVNet2 hello új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-200">2. Create TestVNet2 in hello new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-vpn-gateway-for-testvnet2-with-bgp-parameters"></a><span data-ttu-id="ce06b-201">3. Hello VPN-átjáró BGP-paraméterekkel rendelkező TestVNet2 létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-201">3. Create hello VPN gateway for TestVNet2 with BGP parameters</span></span>

<span data-ttu-id="ce06b-202">A nyilvános IP-cím toobe lefoglalt toohello átjáró a Vnethez tartozó létrehoz, és adja meg a szükséges hello alhálózatot és IP-konfigurációk kérelmet.</span><span class="sxs-lookup"><span data-stu-id="ce06b-202">Request a public IP address toobe allocated toohello gateway you will create for your VNet and define hello required subnet and IP configurations.</span></span>

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

<span data-ttu-id="ce06b-203">Hozzon létre hello VPN-átjáró hello SZÁMOT.</span><span class="sxs-lookup"><span data-stu-id="ce06b-203">Create hello VPN gateway with hello AS number.</span></span> <span data-ttu-id="ce06b-204">Az Azure VPN gatewayek a hello alapértelmezett ASN felül kell bírálnia.</span><span class="sxs-lookup"><span data-stu-id="ce06b-204">You must override hello default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="ce06b-205">hello ASN-eket a hello csatlakoztatott Vnetekhez különböző tooenable BGP és a tranzit útválasztást kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ce06b-205">hello ASNs for hello connected VNets must be different tooenable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="ce06b-206">2. lépés – hello TestVNet1 és TestVNet2 átjárók csatlakozás</span><span class="sxs-lookup"><span data-stu-id="ce06b-206">Step 2 - Connect hello TestVNet1 and TestVNet2 gateways</span></span>

<span data-ttu-id="ce06b-207">Ebben a példában mindkét átjárók vannak hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ce06b-207">In this example, both gateways are in hello same subscription.</span></span> <span data-ttu-id="ce06b-208">Ennek végrehajtásáról a hello ugyanazon PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="ce06b-208">You can complete this step in hello same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="ce06b-209">1. Mindkét átjárók beolvasása</span><span class="sxs-lookup"><span data-stu-id="ce06b-209">1. Get both gateways</span></span>

<span data-ttu-id="ce06b-210">Ellenőrizze, hogy jelentkezzen be, és csatlakozzon a tooSubscription 1.</span><span class="sxs-lookup"><span data-stu-id="ce06b-210">Make sure you log in and connect tooSubscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="ce06b-211">2. Mindkét kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce06b-211">2. Create both connections</span></span>

<span data-ttu-id="ce06b-212">Ebben a lépésben hoz létre TestVNet1 tooTestVNet2 hello kapcsolatot, és hello kapcsolat TestVNet2 tooTestVNet1.</span><span class="sxs-lookup"><span data-stu-id="ce06b-212">In this step, you create hello connection from TestVNet1 tooTestVNet2, and hello connection from TestVNet2 tooTestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="ce06b-213">Lehet, hogy tooenable BGP mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="ce06b-213">Be sure tooenable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="ce06b-214">A lépések elvégzése után hello kapcsolat létrejön, néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="ce06b-214">After completing these steps, hello connection is established after a few minutes.</span></span> <span data-ttu-id="ce06b-215">hello BGP társviszony-létesítési munkamenetet működik, amikor befejeződött a hello VNet – VNet-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ce06b-215">hello BGP peering session is up once hello VNet-to-VNet connection is completed.</span></span>

<span data-ttu-id="ce06b-216">Ha befejezte az ebben a gyakorlatban három része, a következő hálózati topológia hello hozott létre:</span><span class="sxs-lookup"><span data-stu-id="ce06b-216">If you completed all three parts of this exercise, you have established hello following network topology:</span></span>

![A VNet – VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a><span data-ttu-id="ce06b-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ce06b-218">Next steps</span></span>

<span data-ttu-id="ce06b-219">Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ce06b-219">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="ce06b-220">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ce06b-220">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>
