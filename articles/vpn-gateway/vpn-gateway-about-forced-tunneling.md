---
title: "Az Azure-webhelyek kapcsolatok kényszerített bújtatás konfigurálása: klasszikus |} Microsoft Docs"
description: "Hogyan tooredirect vagy \"kényszerített\" az összes internetes adathoz kötött hátsó forgalom-tooyour helyszíni hely."
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
ms.openlocfilehash: 35b3a9ea370f9f962572629a69cc9aed16a87837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-classic-deployment-model"></a><span data-ttu-id="0b8ad-103">Konfigurálja a kényszerített bújtatás használata hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="0b8ad-103">Configure forced tunneling using hello classic deployment model</span></span>

<span data-ttu-id="0b8ad-104">A kényszerített bújtatás lehetővé átirányítási vagy "kényszerített" minden Internet adathoz kötött forgalom hátsó tooyour helyszíni hely vizsgálati és naplózási pont-pont VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="0b8ad-105">Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="0b8ad-106">Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban lesz mindig haladnak át közvetlenül kimenő Internet, toohello nélkül hello beállítás tooallow Azure hálózati infrastruktúráról, tooinspect vagy a napló hello forgalom.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="0b8ad-107">Jogosulatlan Internet-hozzáférés is eredményezhet, tooinformation nyilvánosságra vagy más típusú biztonsági problémákat.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-107">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="0b8ad-108">Ez a cikk bemutatja, hogyan konfigurálása, a kényszerített bújtatás hello klasszikus telepítési modellel készült virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-108">This article walks you through configuring forced tunneling for virtual networks created using hello classic deployment model.</span></span> <span data-ttu-id="0b8ad-109">A kényszerített bújtatás powershellel hello portálon keresztül nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-109">Forced tunneling can be configured by using PowerShell, not through hello portal.</span></span> <span data-ttu-id="0b8ad-110">Ha a kényszerített bújtatás hello Resource Manager üzembe helyezési modellben tooconfigure, jelölje be a klasszikus cikk a legördülő listából a következő hello:</span><span class="sxs-lookup"><span data-stu-id="0b8ad-110">If you want tooconfigure forced tunneling for hello Resource Manager deployment model, select classic article from hello following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b8ad-111">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="0b8ad-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="0b8ad-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0b8ad-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a><span data-ttu-id="0b8ad-113">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="0b8ad-113">Requirements and considerations</span></span>
<span data-ttu-id="0b8ad-114">Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak (UDR) keresztül.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-114">Forced tunneling in Azure is configured via virtual network user-defined routes (UDR).</span></span> <span data-ttu-id="0b8ad-115">Irányít át forgalmat tooan helyszíni hely alapértelmezett útvonalat toohello Azure VPN-átjáróként van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-115">Redirecting traffic tooan on-premises site is expressed as a Default Route toohello Azure VPN gateway.</span></span> <span data-ttu-id="0b8ad-116">hello következő szakaszban azok hello aktuális hello útválasztási táblázathoz és korlátozása útvonalak egy Azure virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="0b8ad-116">hello following section lists hello current limitation of hello routing table and routes for an Azure Virtual Network:</span></span>

* <span data-ttu-id="0b8ad-117">Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-117">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="0b8ad-118">hello rendszer útvonaltábla rendelkezik a következő három csoport útvonalak hello:</span><span class="sxs-lookup"><span data-stu-id="0b8ad-118">hello system routing table has hello following three groups of routes:</span></span>

  * <span data-ttu-id="0b8ad-119">**Helyi VNet útvonalak:** közvetlenül toohello cél virtuális gépek hello az azonos virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-119">**Local VNet routes:** Directly toohello destination VMs in hello same virtual network.</span></span>
  * <span data-ttu-id="0b8ad-120">**A helyi útvonalak:** toohello Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-120">**On-premises routes:** toohello Azure VPN gateway.</span></span>
  * <span data-ttu-id="0b8ad-121">**Alapértelmezett útvonal:** közvetlenül toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-121">**Default route:** Directly toohello Internet.</span></span> <span data-ttu-id="0b8ad-122">A program eltávolítja a csomagok toohello magánhálózati IP-címek nem fedi le hello előző két útvonalak.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-122">Packets destined toohello private IP addresses not covered by hello previous two routes will be dropped.</span></span>
* <span data-ttu-id="0b8ad-123">Felhasználó által definiált útvonalak hello számára készült hozzon létre egy útválasztási táblázat tooadd alapértelmezett útvonalat, és társíthatja hello útválasztási táblázat tooyour VNet alhálózat tooenable kényszerített bújtatás ezek alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-123">With hello release of user-defined routes, you can create a routing table tooadd a default route, and then associate hello routing table tooyour VNet subnet(s) tooenable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="0b8ad-124">Egy "alapértelmezett"nevű hely tooset kell hello létesítmények közötti helyi helyek csatlakoztatott toohello virtuális hálózat között.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-124">You need tooset a "default site" among hello cross-premises local sites connected toohello virtual network.</span></span>
* <span data-ttu-id="0b8ad-125">A kényszerített bújtatás kell rendelni egy Vnetet, amely egy dinamikus útválasztási VPN-átjáró (nem a statikus átjárók) rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-125">Forced tunneling must be associated with a VNet that has a dynamic routing VPN gateway (not a static gateway).</span></span>
* <span data-ttu-id="0b8ad-126">A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet keresztül hello ExpressRoute BGP társviszony-létesítési munkameneteket.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-126">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via hello ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="0b8ad-127">Tekintse meg a hello [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/) további információt.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-127">Please see hello [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="0b8ad-128">Konfigurálása – áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b8ad-128">Configuration overview</span></span>
<span data-ttu-id="0b8ad-129">A következő példa hello hello Frontend alhálózathoz nem kényszeríti a bújtatott.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-129">In hello following example, hello Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="0b8ad-130">hello munkaterhelések hello Frontend alhálózaton továbbra is tooaccept és hello Internet toocustomer kérelmek közvetlenül válaszol.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-130">hello workloads in hello Frontend subnet can continue tooaccept and respond toocustomer requests from hello Internet directly.</span></span> <span data-ttu-id="0b8ad-131">hello közepes réteg és a háttérkiszolgáló alhálózatok kényszerítve vannak-e bújtatott.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-131">hello Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="0b8ad-132">A kimenő kapcsolatokat, az alábbi két alhálózat toohello Internet lesz kényszerített vagy átirányított vissza tooan hely helyszíni S2S VPN-je bújtatja hello keresztül.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-132">Any outbound connections from these two subnets toohello Internet will be forced or redirected back tooan on-premises site via one of hello S2S VPN tunnels.</span></span>

<span data-ttu-id="0b8ad-133">Ez lehetővé teszi a toorestrict és vizsgálhatja meg a virtuális gépek Internet-hozzáféréssel, vagy felhőalapú szolgáltatások Azure, miközben továbbra tooenable a többrétegű szolgáltatások architektúra szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-133">This allows you toorestrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing tooenable your multi-tier service architecture required.</span></span> <span data-ttu-id="0b8ad-134">Is alkalmazhat a kényszerített bújtatás toohello teljes virtuális hálózatok esetén egyetlen internetre munkaterhelés a virtuális hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-134">You also can apply forced tunneling toohello entire virtual networks if there are no Internet-facing workloads in your virtual networks.</span></span>

![Alagúthasználat kényszerítése](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a><span data-ttu-id="0b8ad-136">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0b8ad-136">Before you begin</span></span>
<span data-ttu-id="0b8ad-137">Győződjön meg arról, hogy rendelkezik-e a következő elemek kezdete előtt hello.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-137">Verify that you have hello following items before beginning configuration.</span></span>

* <span data-ttu-id="0b8ad-138">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-138">An Azure subscription.</span></span> <span data-ttu-id="0b8ad-139">Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0b8ad-139">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="0b8ad-140">A konfigurált virtuális hálózati.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-140">A configured virtual network.</span></span> 
* <span data-ttu-id="0b8ad-141">hello legújabb verziójának hello Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-141">hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="0b8ad-142">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-142">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

## <a name="configure-forced-tunneling"></a><span data-ttu-id="0b8ad-143">Kényszerített bújtatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0b8ad-143">Configure forced tunneling</span></span>
<span data-ttu-id="0b8ad-144">a következő eljárás hello segítségével adja meg a virtuális hálózat kényszerített bújtatás.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-144">hello following procedure will help you specify forced tunneling for a virtual network.</span></span> <span data-ttu-id="0b8ad-145">hello konfigurációs lépések toohello virtuális hálózat hálózati konfigurációs fájl tartozik.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-145">hello configuration steps correspond toohello VNet network configuration file.</span></span>

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

<span data-ttu-id="0b8ad-146">Ebben a példában hello virtuális hálózati MultiTier – VNet három alhálózatot tartalmaz: "Háttér", "Előtér" és "Midtier" alhálózatok, négy helyszínek közötti kapcsolatok: "DefaultSiteHQ", és három ágak.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-146">In this example, hello virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend' subnets, with four cross premises connections: 'DefaultSiteHQ', and three Branches.</span></span> 

<span data-ttu-id="0b8ad-147">hello lépéseket hello "DefaultSiteHQ" állítja be a kényszerített bújtatás hello alapértelmezett hely kapcsolatot, és hello Midtier és a háttérkiszolgáló alhálózatok toouse kényszerített bújtatás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-147">hello steps will set hello 'DefaultSiteHQ' as hello default site connection for forced tunneling, and configure hello Midtier and Backend subnets toouse forced tunneling.</span></span>

1. <span data-ttu-id="0b8ad-148">Hozzon létre egy útválasztási táblázatban.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-148">Create a routing table.</span></span> <span data-ttu-id="0b8ad-149">A következő parancsmag toocreate hello az útválasztási táblázatot használja.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-149">Use hello following cmdlet toocreate your route table.</span></span>

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. <span data-ttu-id="0b8ad-150">Adjon hozzá egy alapértelmezett útválasztási toohello útválasztási táblázatot.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-150">Add a default route toohello routing table.</span></span> 

  <span data-ttu-id="0b8ad-151">hello következő példakóddal felveheti egy alapértelmezett útválasztási toohello útválasztási táblázatot az 1.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-151">hello following example adds a default route toohello routing table created in Step 1.</span></span> <span data-ttu-id="0b8ad-152">Vegye figyelembe, hogy csak a támogatott útvonal hello hello cél előtagja "0.0.0.0/0" toohello "A(z)" következő ugrás értéke.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-152">Note that hello only route supported is hello destination prefix of "0.0.0.0/0" toohello "VPNGateway" NextHop.</span></span>

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. <span data-ttu-id="0b8ad-153">Hello útválasztási táblázat toohello alhálózatok társítása.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-153">Associate hello routing table toohello subnets.</span></span> 

  <span data-ttu-id="0b8ad-154">Miután létre lett hozva egy útválasztási táblázat és egy útvonalat, használja a következő példa tooadd hello, vagy hello útvonal tábla tooa VNet alhálózatot társítani.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-154">After a routing table is created and a route added, use hello following example tooadd or associate hello route table tooa VNet subnet.</span></span> <span data-ttu-id="0b8ad-155">hello példa ad hozzá a hello útvonal tábla "MyRouteTable" toohello Midtier és a háttérkiszolgáló alhálózatok MultiTier-VNet hálózatok.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-155">hello example adds hello route table "MyRouteTable" toohello Midtier and Backend subnets of VNet MultiTier-VNet.</span></span>

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. <span data-ttu-id="0b8ad-156">Rendeljen hozzá egy alapértelmezett webhelyet, a kényszerített bújtatást.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-156">Assign a default site for forced tunneling.</span></span> 

  <span data-ttu-id="0b8ad-157">Az előző lépésben hello hello parancsmag mintaparancsfájlok hello útválasztási tábla létrehozása, és társított hello útvonal tábla tootwo hello VNet alhálózatokat.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-157">In hello preceding step, hello sample cmdlet scripts created hello routing table and associated hello route table tootwo of hello VNet subnets.</span></span> <span data-ttu-id="0b8ad-158">hello további lépései tooselect között hello többhelyes kapcsolatok hello virtuális hálózat hello alapértelmezett webhely vagy alagút helyszíni hely.</span><span class="sxs-lookup"><span data-stu-id="0b8ad-158">hello remaining step is tooselect a local site among hello multi-site connections of hello virtual network as hello default site or tunnel.</span></span>

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a><span data-ttu-id="0b8ad-159">További PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="0b8ad-159">Additional PowerShell cmdlets</span></span>
### <a name="toodelete-a-route-table"></a><span data-ttu-id="0b8ad-160">egy útválasztási táblázatot toodelete</span><span class="sxs-lookup"><span data-stu-id="0b8ad-160">toodelete a route table</span></span>

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="toolist-a-route-table"></a><span data-ttu-id="0b8ad-161">egy útválasztási táblázatot toolist</span><span class="sxs-lookup"><span data-stu-id="0b8ad-161">toolist a route table</span></span>

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="toodelete-a-route-from-a-route-table"></a><span data-ttu-id="0b8ad-162">toodelete egy útvonalat az útvonal táblából</span><span class="sxs-lookup"><span data-stu-id="0b8ad-162">toodelete a route from a route table</span></span>

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="tooremove-a-route-from-a-subnet"></a><span data-ttu-id="0b8ad-163">tooremove alhálózatból származó útvonal</span><span class="sxs-lookup"><span data-stu-id="0b8ad-163">tooremove a route from a subnet</span></span>

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="toolist-hello-route-table-associated-with-a-subnet"></a><span data-ttu-id="0b8ad-164">alhálózat társított toolist hello útvonaltábla</span><span class="sxs-lookup"><span data-stu-id="0b8ad-164">toolist hello route table associated with a subnet</span></span>

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="tooremove-a-default-site-from-a-vnet-vpn-gateway"></a><span data-ttu-id="0b8ad-165">a virtuális hálózat VPN-átjáró alapértelmezett helyek tooremove</span><span class="sxs-lookup"><span data-stu-id="0b8ad-165">tooremove a default site from a VNet VPN gateway</span></span>

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```