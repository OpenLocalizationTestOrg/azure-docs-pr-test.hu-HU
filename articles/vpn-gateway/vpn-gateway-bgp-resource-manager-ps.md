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
ms.openlocfilehash: b00a3fe7ba4b12c2e9c486188c292cd6fafb60a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a><span data-ttu-id="b4897-103">A BGP konfigurálása az Azure VPN Gatewayek PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b4897-103">How to configure BGP on Azure VPN Gateways using PowerShell</span></span>
<span data-ttu-id="b4897-104">Ez a cikk végigvezeti a BGP engedélyezéséhez a létesítmények közötti pont-pont (S2S) VPN-kapcsolat és egy VNet – VNet-kapcsolatot a Resource Manager üzembe helyezési modellben és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b4897-104">This article walks you through the steps to enable BGP on a cross-premises Site-to-Site (S2S) VPN connection and a VNet-to-VNet connection using the Resource Manager deployment model and PowerShell.</span></span>

## <a name="about-bgp"></a><span data-ttu-id="b4897-105">A BGP ismertetése</span><span class="sxs-lookup"><span data-stu-id="b4897-105">About BGP</span></span>
<span data-ttu-id="b4897-106">A BGP az interneten gyakran használt szabványos útválasztási protokoll az útválasztási és elérhetőségi információcserére két vagy több hálózat között.</span><span class="sxs-lookup"><span data-stu-id="b4897-106">BGP is the standard routing protocol commonly used in the Internet to exchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="b4897-107">A BGP lehetővé teszi, hogy az Azure VPN-átjáróként és BGP-társakat vagy szomszédok, az exchange-"irányítja a", amely tájékoztatja a rendelkezésre állási és az adott előtagok haladhat végig az átjárók és az útválasztók érintett reachability mindkét átjárók a helyszíni VPN-eszközök.</span><span class="sxs-lookup"><span data-stu-id="b4897-107">BGP enables the Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, to exchange "routes" that will inform both gateways on the availability and reachability for those prefixes to go through the gateways or routers involved.</span></span> <span data-ttu-id="b4897-108">A BGP lehetővé teszi a több hálózat közötti tranzit útválasztást azon útvonalak propagálásával az összes többi BGP-társ számára, amelyeket a BGP-átjáró az egyik BGP-társtól vesz át.</span><span class="sxs-lookup"><span data-stu-id="b4897-108">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer to all other BGP peers.</span></span>

<span data-ttu-id="b4897-109">Lásd: [áttekintése a BGP az Azure VPN Gatewayek](vpn-gateway-bgp-overview.md) további vitafórum előnyeit a BGP, valamint a műszaki követelményeiben és a BGP használatával kapcsolatos szempontok megismerése.</span><span class="sxs-lookup"><span data-stu-id="b4897-109">See [Overview of BGP with Azure VPN Gateways](vpn-gateway-bgp-overview.md) for more discussion on benefits of BGP and to understand the technical requirements and considerations of using BGP.</span></span>

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a><span data-ttu-id="b4897-110">Az Azure VPN gatewayek a BGP-első lépések</span><span class="sxs-lookup"><span data-stu-id="b4897-110">Getting started with BGP on Azure VPN gateways</span></span>

<span data-ttu-id="b4897-111">Ez a cikk végigvezeti a lépéseit a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="b4897-111">This article walks you through the steps to do the following tasks:</span></span>

* [<span data-ttu-id="b4897-112">Engedélyezze az Azure VPN gateway a BGP, 1 -. rész</span><span class="sxs-lookup"><span data-stu-id="b4897-112">Part 1 - Enable BGP on your Azure VPN gateway</span></span>](#enablebgp)
* [<span data-ttu-id="b4897-113">2. rész – a BGP-létesítmények közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-113">Part 2 - Establish a cross-premises connection with BGP</span></span>](#crossprembgp)
* [<span data-ttu-id="b4897-114">3. rész - BGP VNet – VNet-kapcsolatot létrehozni</span><span class="sxs-lookup"><span data-stu-id="b4897-114">Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>](#v2vbgp)

<span data-ttu-id="b4897-115">Az utasítások részét képező összes a BGP engedélyezve van a hálózati kapcsolatot az alapvető építőelemre képezi.</span><span class="sxs-lookup"><span data-stu-id="b4897-115">Each part of the instructions forms a basic building block for enabling BGP in your network connectivity.</span></span> <span data-ttu-id="b4897-116">Ha befejezte az összes három részből, a topológia elkészítette a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="b4897-116">If you complete all three parts, you build the topology as shown in the following diagram:</span></span>

![BGP-topológiákkal](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

<span data-ttu-id="b4897-118">Egyesítheti a kijelzők együtt egy összetettebb, Többugrásos, átmenő hálózatról az igényeinek megfelelő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b4897-118">You can combine parts together to build a more complex, multi-hop, transit network that meets your needs.</span></span>

## <span data-ttu-id="b4897-119"><a name ="enablebgp"></a>1. rész – az Azure VPN Gateway a BGP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4897-119"><a name ="enablebgp"></a>Part 1 - Configure BGP on the Azure VPN Gateway</span></span>
<span data-ttu-id="b4897-120">A konfigurációs lépéseket az Azure VPN gateway a BGP paramétereinek beállítása, a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="b4897-120">The configuration steps set up the BGP parameters of the Azure VPN gateway as shown in the following diagram:</span></span>

![BGP-átjáró](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a><span data-ttu-id="b4897-122">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b4897-122">Before you begin</span></span>
* <span data-ttu-id="b4897-123">Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="b4897-123">Verify that you have an Azure subscription.</span></span> <span data-ttu-id="b4897-124">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4897-124">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="b4897-125">Az Azure Resource Manager PowerShell-parancsmagjainak telepítése.</span><span class="sxs-lookup"><span data-stu-id="b4897-125">Install the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="b4897-126">További információ a PowerShell-parancsmagok telepítéséről: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b4897-126">For more information about installing the PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 

### <a name="step-1---create-and-configure-vnet1"></a><span data-ttu-id="b4897-127">1. lépés – létrehozása és VNet1 konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4897-127">Step 1 - Create and configure VNet1</span></span>
#### <a name="1-declare-your-variables"></a><span data-ttu-id="b4897-128">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="b4897-128">1. Declare your variables</span></span>
<span data-ttu-id="b4897-129">Ehhez a gyakorlathoz először is deklarálni kell a változókat.</span><span class="sxs-lookup"><span data-stu-id="b4897-129">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="b4897-130">Az alábbi példa azt deklarálja a változókat, az értékek a ehhez a gyakorlathoz.</span><span class="sxs-lookup"><span data-stu-id="b4897-130">The following example declares the variables using the values for this exercise.</span></span> <span data-ttu-id="b4897-131">Az éles konfigurációhoz ne felejtse el ezeket az értékeket a saját értékeire cserélni.</span><span class="sxs-lookup"><span data-stu-id="b4897-131">Be sure to replace the values with your own when configuring for production.</span></span> <span data-ttu-id="b4897-132">Ezeket a változókat akkor használhatja, ha azért hajtja végre a lépéseket, hogy megismerje ezt a konfigurációtípust.</span><span class="sxs-lookup"><span data-stu-id="b4897-132">You can use these variables if you are running through the steps to become familiar with this type of configuration.</span></span> <span data-ttu-id="b4897-133">Módosítsa a változókat, majd másolja és illessze be őket a PowerShell-konzolra.</span><span class="sxs-lookup"><span data-stu-id="b4897-133">Modify the variables, and then copy and paste into your PowerShell console.</span></span>

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a><span data-ttu-id="b4897-134">2. Csatlakozás az előfizetéshez, és hozzon létre egy új erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b4897-134">2. Connect to your subscription and create a new resource group</span></span>
<span data-ttu-id="b4897-135">A Resource Manager parancsmagjainak használatához győződjön meg arról, hogy a PowerShell üzemmódra vált.</span><span class="sxs-lookup"><span data-stu-id="b4897-135">To use the Resource Manager cmdlets, Make sure you switch to PowerShell mode.</span></span> <span data-ttu-id="b4897-136">További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b4897-136">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="b4897-137">Nyissa meg a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="b4897-137">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="b4897-138">A következő minta segíthet a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="b4897-138">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a><span data-ttu-id="b4897-139">3. A TestVNet1 létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-139">3. Create TestVNet1</span></span>
<span data-ttu-id="b4897-140">Az alábbi minta létrehoz egy TestVNet1 és három alhálózatok, egy hívott GatewaySubnet, egy hívott előtér és egy hívott háttér nevű virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="b4897-140">The following sample creates a virtual network named TestVNet1 and three subnets, one called GatewaySubnet, one called FrontEnd, and one called Backend.</span></span> <span data-ttu-id="b4897-141">Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen.</span><span class="sxs-lookup"><span data-stu-id="b4897-141">When substituting values, it's important that you always name your gateway subnet specifically GatewaySubnet.</span></span> <span data-ttu-id="b4897-142">Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="b4897-142">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a><span data-ttu-id="b4897-143">2. lépés – a VPN-átjáró BGP-paraméterekkel rendelkező TestVNet1 létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-143">Step 2 - Create the VPN Gateway for TestVNet1 with BGP parameters</span></span>
#### <a name="1-create-the-ip-and-subnet-configurations"></a><span data-ttu-id="b4897-144">1. Az IP-cím és alhálózat-konfigurációk létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-144">1. Create the IP and subnet configurations</span></span>
<span data-ttu-id="b4897-145">Kérje egy nyilvános IP-cím kiosztását a virtuális hálózat számára létrehozni kívánt átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="b4897-145">Request a public IP address to be allocated to the gateway you will create for your VNet.</span></span> <span data-ttu-id="b4897-146">A szükséges alhálózatot és IP-konfigurációk is fogja definiálni.</span><span class="sxs-lookup"><span data-stu-id="b4897-146">You'll also define the required subnet and IP configurations.</span></span>

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a><span data-ttu-id="b4897-147">2. Hozzon létre a VPN-átjáró AS száma</span><span class="sxs-lookup"><span data-stu-id="b4897-147">2. Create the VPN gateway with the AS number</span></span>
<span data-ttu-id="b4897-148">Hozza létre a TestVNet1 virtuális hálózati átjáróját.</span><span class="sxs-lookup"><span data-stu-id="b4897-148">Create the virtual network gateway for TestVNet1.</span></span> <span data-ttu-id="b4897-149">Útvonalalapú VPN-átjáró, valamint is a hozzáadása paraméter - ASN-t, az az ASN (AS-szám) beállítását TestVNet1 BGP kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="b4897-149">BGP requires a Route-Based VPN gateway, and also the addition parameter, -Asn, to set the ASN (AS Number) for TestVNet1.</span></span> <span data-ttu-id="b4897-150">Ha a ASN paraméter nincs megadva, a ASN 65515 hozzá van rendelve.</span><span class="sxs-lookup"><span data-stu-id="b4897-150">If you do not set the ASN parameter, ASN 65515 is assigned.</span></span> <span data-ttu-id="b4897-151">Az átjáró létrehozása akár 30 percet vagy hosszabb időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b4897-151">Creating a gateway can take a while (30 minutes or more to complete).</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a><span data-ttu-id="b4897-152">3. Az Azure BGP-Társgép IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="b4897-152">3. Obtain the Azure BGP Peer IP address</span></span>
<span data-ttu-id="b4897-153">Az átjáró létrehozása után be kell szereznie az Azure VPN Gateway a BGP társ IP-címe.</span><span class="sxs-lookup"><span data-stu-id="b4897-153">Once the gateway is created, you need to obtain the BGP Peer IP address on the Azure VPN Gateway.</span></span> <span data-ttu-id="b4897-154">Ez a cím konfigurálása az Azure VPN Gateway a BGP-partner a helyszíni VPN-eszközök esetében van szükség.</span><span class="sxs-lookup"><span data-stu-id="b4897-154">This address is needed to configure the Azure VPN Gateway as a BGP Peer for your on-premises VPN devices.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

<span data-ttu-id="b4897-155">Az utolsó parancs a megfelelő BGP-konfigurációk jeleníti meg az Azure VPN Gateway; Példa:</span><span class="sxs-lookup"><span data-stu-id="b4897-155">The last command shows the corresponding BGP configurations on the Azure VPN Gateway; for example:</span></span>

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

<span data-ttu-id="b4897-156">Az átjáró létrehozása után a létesítmények közötti vagy a BGP-VNet – VNet-kapcsolatot létesíteni használhatja ezt az átjárót.</span><span class="sxs-lookup"><span data-stu-id="b4897-156">Once the gateway is created, you can use this gateway to establish cross-premises connection or VNet-to-VNet connection with BGP.</span></span> <span data-ttu-id="b4897-157">A következő szakaszok végezze el a szükséges lépések a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="b4897-157">The following sections walk through the steps to complete the exercise.</span></span>

## <span data-ttu-id="b4897-158"><a name ="crossprembbgp"></a>2. rész – a BGP-létesítmények közötti kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-158"><a name ="crossprembbgp"></a>Part 2 - Establish a cross-premises connection with BGP</span></span>

<span data-ttu-id="b4897-159">A létesítmények közötti kapcsolatot szüksége a helyszíni VPN-eszköz képviselő helyi hálózati átjáró és a kapcsolatot a VPN-átjárót, csatlakozzon a helyi hálózati átjáró létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b4897-159">To establish a cross-premises connection, you need to create a Local Network Gateway to represent your on-premises VPN device, and a Connection to connect the VPN gateway with the local network gateway.</span></span> <span data-ttu-id="b4897-160">Nincsenek cikkek, amely ismerteti a fenti lépéseket, amíg ez a cikk a BGP-konfigurációs paraméterek megadása szükséges további tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4897-160">While there are articles that walk you through these steps, this article contains the additional properties required to specify the BGP configuration parameters.</span></span>

![A létesítmények közötti BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

<span data-ttu-id="b4897-162">A továbblépés előtt ellenőrizze, hogy befejeződött [1. rész](#enablebgp) ebben a gyakorlatban.</span><span class="sxs-lookup"><span data-stu-id="b4897-162">Before proceeding, make sure you have completed [Part 1](#enablebgp) of this exercise.</span></span>

### <a name="step-1---create-and-configure-the-local-network-gateway"></a><span data-ttu-id="b4897-163">1. lépés – létrehozása és a helyi hálózati átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4897-163">Step 1 - Create and configure the local network gateway</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="b4897-164">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="b4897-164">1. Declare your variables</span></span>

<span data-ttu-id="b4897-165">Ebben a gyakorlatban továbbra is a konfiguráció az ábrán is látható.</span><span class="sxs-lookup"><span data-stu-id="b4897-165">This exercise continues to build the configuration shown in the diagram.</span></span> <span data-ttu-id="b4897-166">Ne felejtse el az értékeket olyanokra cserélni, amelyeket a saját konfigurációjához kíván használni.</span><span class="sxs-lookup"><span data-stu-id="b4897-166">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

<span data-ttu-id="b4897-167">Több helyi hálózati átjáró paraméterek kapcsolatban ügyeljen a következőkre:</span><span class="sxs-lookup"><span data-stu-id="b4897-167">A couple of things to note regarding the local network gateway parameters:</span></span>

* <span data-ttu-id="b4897-168">A helyi hálózati átjáró lehet az ugyanazon vagy másik helyen és az erőforráscsoport a VPN-átjáróként.</span><span class="sxs-lookup"><span data-stu-id="b4897-168">The local network gateway can be in the same or different location and resource group as the VPN gateway.</span></span> <span data-ttu-id="b4897-169">Ez a példa bemutatja azokat a különböző erőforráscsoportokra különböző helyeken.</span><span class="sxs-lookup"><span data-stu-id="b4897-169">This example shows them in different resource groups in different locations.</span></span>
* <span data-ttu-id="b4897-170">A minimális előtagot kell deklarálni helyi hálózati átjáró pedig a BGP-Társgép IP-cím, a VPN-eszköz állomás címe.</span><span class="sxs-lookup"><span data-stu-id="b4897-170">The minimum prefix you need to declare for the local network gateway is the host address of your BGP Peer IP address on your VPN device.</span></span> <span data-ttu-id="b4897-171">Egy /32 ebben az esetben a "10.52.255.254/32" előtaggal.</span><span class="sxs-lookup"><span data-stu-id="b4897-171">In this case, it's a /32 prefix of "10.52.255.254/32".</span></span>
* <span data-ttu-id="b4897-172">Ne feledje a helyszíni hálózatokhoz és az Azure VNet közötti különböző BGP ASN kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b4897-172">As a reminder, you must use different BGP ASNs between your on-premises networks and Azure VNet.</span></span> <span data-ttu-id="b4897-173">Ha a egyeznek, a virtuális hálózat ASN módosításához, ha a helyszíni VPN-eszköz már használja az ASN a más BGP szomszédok egyenrangú szüksége.</span><span class="sxs-lookup"><span data-stu-id="b4897-173">If they are the same, you need to change your VNet ASN if your on-premises VPN device already uses the ASN to peer with other BGP neighbors.</span></span>

<span data-ttu-id="b4897-174">Mielőtt folytatja, győződjön meg arról, hogy továbbra is csatlakozik az 1. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b4897-174">Before you continue, make sure you are still connected to Subscription 1.</span></span>

#### <a name="2-create-the-local-network-gateway-for-site5"></a><span data-ttu-id="b4897-175">2. A helyi hálózati átjáró létrehozása Site5</span><span class="sxs-lookup"><span data-stu-id="b4897-175">2. Create the local network gateway for Site5</span></span>

<span data-ttu-id="b4897-176">Győződjön meg arról, ha ez nem jön létre, a helyi hálózati átjáró létrehozása előtt az erőforráscsoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b4897-176">Be sure to create the resource group if it is not created, before you create the local network gateway.</span></span> <span data-ttu-id="b4897-177">Figyelje meg a helyi hálózati átjáró két további paraméterek: Asn és BgpPeerAddress.</span><span class="sxs-lookup"><span data-stu-id="b4897-177">Notice the two additional parameters for the local network gateway: Asn and BgpPeerAddress.</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a><span data-ttu-id="b4897-178">2. lépés – a virtuális hálózat átjáró és a helyi hálózati átjáró</span><span class="sxs-lookup"><span data-stu-id="b4897-178">Step 2 - Connect the VNet gateway and local network gateway</span></span>

#### <a name="1-get-the-two-gateways"></a><span data-ttu-id="b4897-179">1. A két átjáró beolvasása</span><span class="sxs-lookup"><span data-stu-id="b4897-179">1. Get the two gateways</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a><span data-ttu-id="b4897-180">2. A TestVNet1 Site5 kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-180">2. Create the TestVNet1 to Site5 connection</span></span>

<span data-ttu-id="b4897-181">Ebben a lépésben hoz létre a kapcsolat TestVNet1 Site5 számára.</span><span class="sxs-lookup"><span data-stu-id="b4897-181">In this step, you create the connection from TestVNet1 to Site5.</span></span> <span data-ttu-id="b4897-182">Adjon meg "-EnableBGP $True" BGP engedélyezése ehhez a kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="b4897-182">You must specify "-EnableBGP $True" to enable BGP for this connection.</span></span> <span data-ttu-id="b4897-183">A fentiekben taglaltak, akkor lehetséges, hogy az ugyanazon Azure VPN Gateway a BGP- és -BGP kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="b4897-183">As discussed earlier, it is possible to have both BGP and non-BGP connections for the same Azure VPN Gateway.</span></span> <span data-ttu-id="b4897-184">A BGP engedélyezve van a connection tulajdonság, ha Azure teszik lehetővé a BGP ehhez a kapcsolathoz annak ellenére, hogy a BGP-paraméterek már be van állítva mindkét átjárókon.</span><span class="sxs-lookup"><span data-stu-id="b4897-184">Unless BGP is enabled in the connection property, Azure will not enable BGP for this connection even though BGP parameters are already configured on both gateways.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

<span data-ttu-id="b4897-185">A következő példa az ebben a gyakorlatban a helyszíni VPN-eszköz a BGP konfigurációs szakasz kezdenek paramétereket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b4897-185">The following example lists the parameters you enter into the BGP configuration section on your on-premises VPN device for this exercise:</span></span>

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

<span data-ttu-id="b4897-186">A kapcsolat létrejötte után néhány percet, és a BGP társviszony-létesítési munkamenetet egyszer elindul az IPsec-kapcsolat létrejön.</span><span class="sxs-lookup"><span data-stu-id="b4897-186">The connection is established after a few minutes, and the BGP peering session starts once the IPsec connection is established.</span></span>

## <span data-ttu-id="b4897-187"><a name ="v2vbgp"></a>3. rész - BGP VNet – VNet-kapcsolatot létrehozni</span><span class="sxs-lookup"><span data-stu-id="b4897-187"><a name ="v2vbgp"></a>Part 3 - Establish a VNet-to-VNet connection with BGP</span></span>

<span data-ttu-id="b4897-188">Ez a szakasz ad hozzá egy VNet – VNet-kapcsolatot, a BGP-vel, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="b4897-188">This section adds a VNet-to-VNet connection with BGP, as shown in the following diagram:</span></span>

![A VNet – VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

<span data-ttu-id="b4897-190">Az alábbi utasítások alapján az előző lépésekben továbbra is.</span><span class="sxs-lookup"><span data-stu-id="b4897-190">The following instructions continue from the previous steps.</span></span> <span data-ttu-id="b4897-191">Meg kell adnia a [i. rész](#enablebgp) létrehozása és konfigurálása a TestVNet1 és a VPN-átjáró BGP-hez.</span><span class="sxs-lookup"><span data-stu-id="b4897-191">You must complete [Part I](#enablebgp) to create and configure TestVNet1 and the VPN Gateway with BGP.</span></span> 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a><span data-ttu-id="b4897-192">1. lépés – TestVNet2 és a VPN-átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-192">Step 1 - Create TestVNet2 and the VPN gateway</span></span>

<span data-ttu-id="b4897-193">Győződjön meg arról, hogy az új virtuális hálózat, TestVNet2, az IP-címtér nem fedi át a virtuális hálózat tartomány egyik fontos.</span><span class="sxs-lookup"><span data-stu-id="b4897-193">It is important to make sure that the IP address space of the new virtual network, TestVNet2, does not overlap with any of your VNet ranges.</span></span>

<span data-ttu-id="b4897-194">Ebben a példában a virtuális hálózatok ugyanahhoz az előfizetéshez tartozik.</span><span class="sxs-lookup"><span data-stu-id="b4897-194">In this example, the virtual networks belong to the same subscription.</span></span> <span data-ttu-id="b4897-195">Beállíthatja a VNet – VNet kapcsolatokhoz különböző előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="b4897-195">You can set up VNet-to-VNet connections between different subscriptions.</span></span> <span data-ttu-id="b4897-196">További információkért lásd: [VNet – VNet-kapcsolatot konfiguráló](vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b4897-196">For more information, see [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md).</span></span> <span data-ttu-id="b4897-197">Mindenképpen adja hozzá a "-EnableBgp $True" a BGP engedélyezéséhez kapcsolatok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b4897-197">Make sure you add the "-EnableBgp $True" when creating the connections to enable BGP.</span></span>

#### <a name="1-declare-your-variables"></a><span data-ttu-id="b4897-198">1. A változók deklarálása</span><span class="sxs-lookup"><span data-stu-id="b4897-198">1. Declare your variables</span></span>

<span data-ttu-id="b4897-199">Ne felejtse el az értékeket olyanokra cserélni, amelyeket a saját konfigurációjához kíván használni.</span><span class="sxs-lookup"><span data-stu-id="b4897-199">Be sure to replace the values with the ones that you want to use for your configuration.</span></span>

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

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a><span data-ttu-id="b4897-200">2. TestVNet2 az új erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-200">2. Create TestVNet2 in the new resource group</span></span>

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a><span data-ttu-id="b4897-201">3. A VPN-átjáró BGP-paraméterekkel rendelkező TestVNet2 létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-201">3. Create the VPN gateway for TestVNet2 with BGP parameters</span></span>

<span data-ttu-id="b4897-202">Igényeljen egy osztható ki a Vnethez tartozó létrehoz, és adja meg a szükséges alhálózatot és IP-konfigurációk az átjáró nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b4897-202">Request a public IP address to be allocated to the gateway you will create for your VNet and define the required subnet and IP configurations.</span></span>

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

<span data-ttu-id="b4897-203">Hozzon létre a VPN-átjáró a AS számot.</span><span class="sxs-lookup"><span data-stu-id="b4897-203">Create the VPN gateway with the AS number.</span></span> <span data-ttu-id="b4897-204">Az Azure VPN gatewayek a felül kell bírálnia az alapértelmezett ASN.</span><span class="sxs-lookup"><span data-stu-id="b4897-204">You must override the default ASN on your Azure VPN gateways.</span></span> <span data-ttu-id="b4897-205">Az ASN-eket a csatlakoztatott Vnetek a BGP és a tranzit Útválasztás engedélyezése különbözőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b4897-205">The ASNs for the connected VNets must be different to enable BGP and transit routing.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a><span data-ttu-id="b4897-206">2. lépés - a TestVNet1 és TestVNet2 átjárók csatlakozás</span><span class="sxs-lookup"><span data-stu-id="b4897-206">Step 2 - Connect the TestVNet1 and TestVNet2 gateways</span></span>

<span data-ttu-id="b4897-207">Ebben a példában két átjáró ugyanahhoz az előfizetéshez vannak.</span><span class="sxs-lookup"><span data-stu-id="b4897-207">In this example, both gateways are in the same subscription.</span></span> <span data-ttu-id="b4897-208">Ebben a lépésben a PowerShell-munkamenetben hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="b4897-208">You can complete this step in the same PowerShell session.</span></span>

#### <a name="1-get-both-gateways"></a><span data-ttu-id="b4897-209">1. Mindkét átjárók beolvasása</span><span class="sxs-lookup"><span data-stu-id="b4897-209">1. Get both gateways</span></span>

<span data-ttu-id="b4897-210">Jelentkezzen be, és kapcsolódjon az 1. előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b4897-210">Make sure you log in and connect to Subscription 1.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a><span data-ttu-id="b4897-211">2. Mindkét kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4897-211">2. Create both connections</span></span>

<span data-ttu-id="b4897-212">Ebben a lépésben hoz létre a kapcsolat TestVNet1 és TestVNet2, és a kapcsolat TestVNet2 TestVNet1 számára.</span><span class="sxs-lookup"><span data-stu-id="b4897-212">In this step, you create the connection from TestVNet1 to TestVNet2, and the connection from TestVNet2 to TestVNet1.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> <span data-ttu-id="b4897-213">Győződjön meg arról, a BGP engedélyezéséhez mindkét kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="b4897-213">Be sure to enable BGP for BOTH connections.</span></span>
> 
> 

<span data-ttu-id="b4897-214">A lépések elvégzése után létrejön a kapcsolat néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="b4897-214">After completing these steps, the connection is established after a few minutes.</span></span> <span data-ttu-id="b4897-215">A BGP társviszony-létesítési munkamenetet működik, amikor befejeződött a VNet – VNet-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="b4897-215">The BGP peering session is up once the VNet-to-VNet connection is completed.</span></span>

<span data-ttu-id="b4897-216">Ha befejezte az ebben a gyakorlatban három része, a következő hálózati topológia hozott létre:</span><span class="sxs-lookup"><span data-stu-id="b4897-216">If you completed all three parts of this exercise, you have established the following network topology:</span></span>

![A VNet – VNet BGP](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a><span data-ttu-id="b4897-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4897-218">Next steps</span></span>

<span data-ttu-id="b4897-219">Miután a kapcsolat létrejött, hozzáadhat virtuális gépeket a virtuális hálózataihoz.</span><span class="sxs-lookup"><span data-stu-id="b4897-219">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="b4897-220">A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b4897-220">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>