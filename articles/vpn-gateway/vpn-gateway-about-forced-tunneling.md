---
title: "Az Azure-webhelyek kapcsolatok kényszerített bújtatás konfigurálása: klasszikus |} Microsoft Docs"
description: "Hogyan lehet irányítani, vagy \"kényszerített\" minden internetre irányuló forgalomnak a helyszíni helyre."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 79bf6892c823da282c3e763921e830f986419854
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a><span data-ttu-id="e6e9b-103">Kényszerített bújtatás konfigurálása klasszikus üzemi modellel</span><span class="sxs-lookup"><span data-stu-id="e6e9b-103">Configure forced tunneling using the classic deployment model</span></span>

<span data-ttu-id="e6e9b-104">A kényszerített bújtatás lehetővé teszi az átirányítási vagy "kényszerített" minden internetre irányuló forgalomnak biztonsági másolatot a helyszíni helyre vizsgálati és naplózási pont-pont VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="e6e9b-105">Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="e6e9b-106">Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban lesz mindig haladnak át Azure hálózati infrastruktúráról közvetlenül kimenő csatlakozik az internethez, a beállítás lehetővé teszi vizsgálja meg, vagy a forgalom naplózása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="e6e9b-107">Jogosulatlan Internet-hozzáférés is eredményezhet, információfelfedés vagy más típusú biztonsági problémákat.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-107">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="e6e9b-108">Ez a cikk bemutatja, hogyan konfigurálása, a kényszerített bújtatás a klasszikus telepítési modellel létrehozott virtuális hálózatokat.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-108">This article walks you through configuring forced tunneling for virtual networks created using the classic deployment model.</span></span> <span data-ttu-id="e6e9b-109">A kényszerített bújtatás PowerShell, nem a portálon keresztül használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-109">Forced tunneling can be configured by using PowerShell, not through the portal.</span></span> <span data-ttu-id="e6e9b-110">Ha azt szeretné, a Resource Manager üzembe helyezési modellben a kényszerített bújtatás konfigurálása, a következő legördülő listából válassza a klasszikus cikk:</span><span class="sxs-lookup"><span data-stu-id="e6e9b-110">If you want to configure forced tunneling for the Resource Manager deployment model, select classic article from the following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e6e9b-111">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="e6e9b-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="e6e9b-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e6e9b-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a><span data-ttu-id="e6e9b-113">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="e6e9b-113">Requirements and considerations</span></span>
<span data-ttu-id="e6e9b-114">Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak (UDR) keresztül.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-114">Forced tunneling in Azure is configured via virtual network user-defined routes (UDR).</span></span> <span data-ttu-id="e6e9b-115">Egy helyszíni hely irányít át forgalmat az Azure VPN gatewayhez alapértelmezett útvonalat kifejezve.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-115">Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway.</span></span> <span data-ttu-id="e6e9b-116">A következő szakaszban azok az aktuális útválasztási táblázat és korlátozása útvonalak egy Azure virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="e6e9b-116">The following section lists the current limitation of the routing table and routes for an Azure Virtual Network:</span></span>

* <span data-ttu-id="e6e9b-117">Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-117">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="e6e9b-118">A rendszer útválasztási táblázatban az útvonalak a következő három csoport rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e6e9b-118">The system routing table has the following three groups of routes:</span></span>

  * <span data-ttu-id="e6e9b-119">**Helyi VNet útvonalak:** közvetlenül és a cél virtuális gépek ugyanabban a virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-119">**Local VNet routes:** Directly to the destination VMs in the same virtual network.</span></span>
  * <span data-ttu-id="e6e9b-120">**A helyi útvonalak:** számára az Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-120">**On-premises routes:** To the Azure VPN gateway.</span></span>
  * <span data-ttu-id="e6e9b-121">**Alapértelmezett útvonal:** közvetlenül kell az internethez.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-121">**Default route:** Directly to the Internet.</span></span> <span data-ttu-id="e6e9b-122">A privát IP-címek nem fedi le az előző két útvonalak szánt csomagok törlődnek.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-122">Packets destined to the private IP addresses not covered by the previous two routes will be dropped.</span></span>
* <span data-ttu-id="e6e9b-123">Felhasználó által definiált útvonalak kiadása létrehoz egy útválasztási táblázatot egy alapértelmezett útvonal hozzáadása, és majd társítani a virtuális hálózat alhálózat ezek alhálózatok kényszerített bújtatás engedélyezése az útválasztási táblázatban.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-123">With the release of user-defined routes, you can create a routing table to add a default route, and then associate the routing table to your VNet subnet(s) to enable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="e6e9b-124">Egy "alapértelmezett"nevű hely a létesítmények közötti helyi helyek között a virtuális hálózathoz be kell.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-124">You need to set a "default site" among the cross-premises local sites connected to the virtual network.</span></span>
* <span data-ttu-id="e6e9b-125">A kényszerített bújtatás kell rendelni egy Vnetet, amely egy dinamikus útválasztási VPN-átjáró (nem a statikus átjárók) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-125">Forced tunneling must be associated with a VNet that has a dynamic routing VPN gateway (not a static gateway).</span></span>
* <span data-ttu-id="e6e9b-126">A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet a ExpressRoute BGP társviszony-létesítési munkameneteket keresztül.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-126">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="e6e9b-127">Tekintse meg a [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/) további információt.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-127">Please see the [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="e6e9b-128">Konfigurálása – áttekintés</span><span class="sxs-lookup"><span data-stu-id="e6e9b-128">Configuration overview</span></span>
<span data-ttu-id="e6e9b-129">A következő példában a Frontend alhálózathoz nem kényszeríti a bújtatott.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-129">In the following example, the Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="e6e9b-130">A munkaterhelések Frontend alhálózaton fogadja el, és a felhasználói kérésekre válaszol az internetről közvetlenül is.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-130">The workloads in the Frontend subnet can continue to accept and respond to customer requests from the Internet directly.</span></span> <span data-ttu-id="e6e9b-131">A közepes réteg és a háttérkiszolgáló alhálózatok kényszerítve vannak bújtatott.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-131">The Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="e6e9b-132">A kimenő kapcsolatokat ezek két alhálózat-Internet kell kényszerített vagy egy helyszíni hely keresztül az S2S VPN-alagutat átirányítva.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-132">Any outbound connections from these two subnets to the Internet will be forced or redirected back to an on-premises site via one of the S2S VPN tunnels.</span></span>

<span data-ttu-id="e6e9b-133">Ez lehetővé teszi, hogy korlátozza és vizsgálja meg a virtuális gépek Internet-hozzáféréssel, vagy miközben továbbra is a többrétegű szolgáltatás architektúrája szükséges engedélyezése a felhőalapú Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-133">This allows you to restrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing to enable your multi-tier service architecture required.</span></span> <span data-ttu-id="e6e9b-134">Is alkalmazhat kényszerített bújtatás a teljes virtuális hálózatokra Ha szerepel a virtuális hálózatok nem internetre munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-134">You also can apply forced tunneling to the entire virtual networks if there are no Internet-facing workloads in your virtual networks.</span></span>

![Alagúthasználat kényszerítése](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a><span data-ttu-id="e6e9b-136">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e6e9b-136">Before you begin</span></span>
<span data-ttu-id="e6e9b-137">A konfigurálás megkezdése előtt győződjön meg arról, hogy rendelkezik a következő elemekkel.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-137">Verify that you have the following items before beginning configuration.</span></span>

* <span data-ttu-id="e6e9b-138">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-138">An Azure subscription.</span></span> <span data-ttu-id="e6e9b-139">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e6e9b-139">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e6e9b-140">A konfigurált virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-140">A configured virtual network.</span></span> 
* <span data-ttu-id="e6e9b-141">Az Azure PowerShell-parancsmagok legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-141">The latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="e6e9b-142">A PowerShell-parancsmagok telepítésével kapcsolatban további információ: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e6e9b-142">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

## <a name="configure-forced-tunneling"></a><span data-ttu-id="e6e9b-143">Kényszerített bújtatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6e9b-143">Configure forced tunneling</span></span>
<span data-ttu-id="e6e9b-144">Az alábbi eljárás segítségével megadhatja, hogy kényszerített bújtatást a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-144">The following procedure will help you specify forced tunneling for a virtual network.</span></span> <span data-ttu-id="e6e9b-145">A konfigurálás lépéseinek végrehajtásához a virtuális hálózat hálózati konfigurációs fájl tartozik.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-145">The configuration steps correspond to the VNet network configuration file.</span></span>

```
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

<span data-ttu-id="e6e9b-146">Ebben a példában a virtuális hálózat MultiTier – VNet három alhálózatok rendelkezik: "Háttér", "Előtér" és "Midtier" alhálózatok, négy helyszínek közötti kapcsolatok: "DefaultSiteHQ", és három ágak.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-146">In this example, the virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend' subnets, with four cross premises connections: 'DefaultSiteHQ', and three Branches.</span></span> 

<span data-ttu-id="e6e9b-147">A lépések állítja be a "DefaultSiteHQ" az alapértelmezett hely kapcsolat a kényszerített bújtatás, és konfigurálja a Midtier és háttér-alhálózatok használata a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-147">The steps will set the 'DefaultSiteHQ' as the default site connection for forced tunneling, and configure the Midtier and Backend subnets to use forced tunneling.</span></span>

1. <span data-ttu-id="e6e9b-148">Hozzon létre egy útválasztási táblázatban.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-148">Create a routing table.</span></span> <span data-ttu-id="e6e9b-149">Az útvonaltábla létrehozásához használja a következő parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-149">Use the following cmdlet to create your route table.</span></span>

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. <span data-ttu-id="e6e9b-150">Adjon hozzá egy alapértelmezett útvonalat az útválasztási táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-150">Add a default route to the routing table.</span></span> 

  <span data-ttu-id="e6e9b-151">A következő példa egy alapértelmezett útvonalat ad az 1. lépésben létrehozott útválasztási táblázathoz.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-151">The following example adds a default route to the routing table created in Step 1.</span></span> <span data-ttu-id="e6e9b-152">Vegye figyelembe, hogy az csak útvonal támogatott "0.0.0.0/0" a "A(z)" következő ugrás a cél-előtagot.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-152">Note that the only route supported is the destination prefix of "0.0.0.0/0" to the "VPNGateway" NextHop.</span></span>

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. <span data-ttu-id="e6e9b-153">Az útválasztási táblázatban az alhálózatok társítása.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-153">Associate the routing table to the subnets.</span></span> 

  <span data-ttu-id="e6e9b-154">Után létre lett hozva egy útválasztási táblázat és egy útvonalat, az alábbi példát követve adja hozzá, vagy rendeljen hozzá egy virtuális hálózat alhálózathoz útvonaltábla.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-154">After a routing table is created and a route added, use the following example to add or associate the route table to a VNet subnet.</span></span> <span data-ttu-id="e6e9b-155">A példa az útvonaltáblát "MyRouteTable" hozzáadása a Midtier és a háttérkiszolgáló alhálózatok MultiTier-VNet hálózatok.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-155">The example adds the route table "MyRouteTable" to the Midtier and Backend subnets of VNet MultiTier-VNet.</span></span>

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. <span data-ttu-id="e6e9b-156">Rendeljen hozzá egy alapértelmezett webhelyet, a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-156">Assign a default site for forced tunneling.</span></span> 

  <span data-ttu-id="e6e9b-157">Az előző lépésben parancsmag mintaparancsfájlok az útvonaltábla létrehozása, és két VNet alhálózatok útvonaltábla társítva.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-157">In the preceding step, the sample cmdlet scripts created the routing table and associated the route table to two of the VNet subnets.</span></span> <span data-ttu-id="e6e9b-158">A többi lépés helyet kell kijelölnie egy helyi a többhelyes kapcsolatok a virtuális hálózat között az alapértelmezett hely, vagy alagút.</span><span class="sxs-lookup"><span data-stu-id="e6e9b-158">The remaining step is to select a local site among the multi-site connections of the virtual network as the default site or tunnel.</span></span>

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a><span data-ttu-id="e6e9b-159">További PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="e6e9b-159">Additional PowerShell cmdlets</span></span>
### <a name="to-delete-a-route-table"></a><span data-ttu-id="e6e9b-160">Új útvonaltábla törlése</span><span class="sxs-lookup"><span data-stu-id="e6e9b-160">To delete a route table</span></span>

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a><span data-ttu-id="e6e9b-161">Egy útválasztási táblázatot listájára</span><span class="sxs-lookup"><span data-stu-id="e6e9b-161">To list a route table</span></span>

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a><span data-ttu-id="e6e9b-162">Egy útválasztási táblázatot egy útvonal törlése</span><span class="sxs-lookup"><span data-stu-id="e6e9b-162">To delete a route from a route table</span></span>

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a><span data-ttu-id="e6e9b-163">Egy alhálózat egy útvonal eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e6e9b-163">To remove a route from a subnet</span></span>

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a><span data-ttu-id="e6e9b-164">Alhálózat társított útvonaltábla listázásához</span><span class="sxs-lookup"><span data-stu-id="e6e9b-164">To list the route table associated with a subnet</span></span>

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a><span data-ttu-id="e6e9b-165">Az alapértelmezett hely eltávolítása a virtuális hálózat VPN-átjáró</span><span class="sxs-lookup"><span data-stu-id="e6e9b-165">To remove a default site from a VNet VPN gateway</span></span>

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```