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
# <a name="configure-forced-tunneling-using-hello-classic-deployment-model"></a>Konfigurálja a kényszerített bújtatás használata hello klasszikus telepítési modell

A kényszerített bújtatás lehetővé átirányítási vagy "kényszerített" minden Internet adathoz kötött forgalom hátsó tooyour helyszíni hely vizsgálati és naplózási pont-pont VPN-alagúton keresztül. Ez az kritikus fontosságú biztonsági előfeltétele annak, hogy a legtöbb vállalati informatikai házirendek. Kényszerített bújtatás nélkül internetre irányuló forgalomnak a virtuális gépek Azure-ban lesz mindig haladnak át közvetlenül kimenő Internet, toohello nélkül hello beállítás tooallow Azure hálózati infrastruktúráról, tooinspect vagy a napló hello forgalom. Jogosulatlan Internet-hozzáférés is eredményezhet, tooinformation nyilvánosságra vagy más típusú biztonsági problémákat.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Ez a cikk bemutatja, hogyan konfigurálása, a kényszerített bújtatás hello klasszikus telepítési modellel készült virtuális hálózatok. A kényszerített bújtatás powershellel hello portálon keresztül nem konfigurálható. Ha a kényszerített bújtatás hello Resource Manager üzembe helyezési modellben tooconfigure, jelölje be a klasszikus cikk a legördülő listából a következő hello:

> [!div class="op_single_selector"]
> * [PowerShell – Klasszikus](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Követelmények és szempontok
Az Azure-ban kényszerített bújtatás konfigurálása virtuális hálózati felhasználó által definiált útvonalak (UDR) keresztül. Irányít át forgalmat tooan helyszíni hely alapértelmezett útvonalat toohello Azure VPN-átjáróként van kifejezve. hello következő szakaszban azok hello aktuális hello útválasztási táblázathoz és korlátozása útvonalak egy Azure virtuális hálózat:

* Minden egyes virtuális hálózati alhálózat van egy beépített, rendszer-útválasztási táblázatához. hello rendszer útvonaltábla rendelkezik a következő három csoport útvonalak hello:

  * **Helyi VNet útvonalak:** közvetlenül toohello cél virtuális gépek hello az azonos virtuális hálózatban.
  * **A helyi útvonalak:** toohello Azure VPN gateway.
  * **Alapértelmezett útvonal:** közvetlenül toohello Internet. A program eltávolítja a csomagok toohello magánhálózati IP-címek nem fedi le hello előző két útvonalak.
* Felhasználó által definiált útvonalak hello számára készült hozzon létre egy útválasztási táblázat tooadd alapértelmezett útvonalat, és társíthatja hello útválasztási táblázat tooyour VNet alhálózat tooenable kényszerített bújtatás ezek alhálózatok.
* Egy "alapértelmezett"nevű hely tooset kell hello létesítmények közötti helyi helyek csatlakoztatott toohello virtuális hálózat között.
* A kényszerített bújtatás kell rendelni egy Vnetet, amely egy dinamikus útválasztási VPN-átjáró (nem a statikus átjárók) rendelkezik.
* A kényszerített bújtatás ExpressRoute a mechanizmus révén nincs konfigurálva, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat hirdet keresztül hello ExpressRoute BGP társviszony-létesítési munkameneteket. Tekintse meg a hello [ExpressRoute dokumentációja](https://azure.microsoft.com/documentation/services/expressroute/) további információt.

## <a name="configuration-overview"></a>Konfigurálása – áttekintés
A következő példa hello hello Frontend alhálózathoz nem kényszeríti a bújtatott. hello munkaterhelések hello Frontend alhálózaton továbbra is tooaccept és hello Internet toocustomer kérelmek közvetlenül válaszol. hello közepes réteg és a háttérkiszolgáló alhálózatok kényszerítve vannak-e bújtatott. A kimenő kapcsolatokat, az alábbi két alhálózat toohello Internet lesz kényszerített vagy átirányított vissza tooan hely helyszíni S2S VPN-je bújtatja hello keresztül.

Ez lehetővé teszi a toorestrict és vizsgálhatja meg a virtuális gépek Internet-hozzáféréssel, vagy felhőalapú szolgáltatások Azure, miközben továbbra tooenable a többrétegű szolgáltatások architektúra szükséges. Is alkalmazhat a kényszerített bújtatás toohello teljes virtuális hálózatok esetén egyetlen internetre munkaterhelés a virtuális hálózatokon.

![Alagúthasználat kényszerítése](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy rendelkezik-e a következő elemek kezdete előtt hello.

* Azure-előfizetés. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
* A konfigurált virtuális hálózati. 
* hello legújabb verziójának hello Azure PowerShell-parancsmagokat. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.

## <a name="configure-forced-tunneling"></a>Kényszerített bújtatás konfigurálása
a következő eljárás hello segítségével adja meg a virtuális hálózat kényszerített bújtatás. hello konfigurációs lépések toohello virtuális hálózat hálózati konfigurációs fájl tartozik.

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

Ebben a példában hello virtuális hálózati MultiTier – VNet három alhálózatot tartalmaz: "Háttér", "Előtér" és "Midtier" alhálózatok, négy helyszínek közötti kapcsolatok: "DefaultSiteHQ", és három ágak. 

hello lépéseket hello "DefaultSiteHQ" állítja be a kényszerített bújtatás hello alapértelmezett hely kapcsolatot, és hello Midtier és a háttérkiszolgáló alhálózatok toouse kényszerített bújtatás konfigurálása.

1. Hozzon létre egy útválasztási táblázatban. A következő parancsmag toocreate hello az útválasztási táblázatot használja.

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. Adjon hozzá egy alapértelmezett útválasztási toohello útválasztási táblázatot. 

  hello következő példakóddal felveheti egy alapértelmezett útválasztási toohello útválasztási táblázatot az 1. Vegye figyelembe, hogy csak a támogatott útvonal hello hello cél előtagja "0.0.0.0/0" toohello "A(z)" következő ugrás értéke.

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. Hello útválasztási táblázat toohello alhálózatok társítása. 

  Miután létre lett hozva egy útválasztási táblázat és egy útvonalat, használja a következő példa tooadd hello, vagy hello útvonal tábla tooa VNet alhálózatot társítani. hello példa ad hozzá a hello útvonal tábla "MyRouteTable" toohello Midtier és a háttérkiszolgáló alhálózatok MultiTier-VNet hálózatok.

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. Rendeljen hozzá egy alapértelmezett webhelyet, a kényszerített bújtatást. 

  Az előző lépésben hello hello parancsmag mintaparancsfájlok hello útválasztási tábla létrehozása, és társított hello útvonal tábla tootwo hello VNet alhálózatokat. hello további lépései tooselect között hello többhelyes kapcsolatok hello virtuális hálózat hello alapértelmezett webhely vagy alagút helyszíni hely.

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a>További PowerShell-parancsmagok
### <a name="toodelete-a-route-table"></a>egy útválasztási táblázatot toodelete

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="toolist-a-route-table"></a>egy útválasztási táblázatot toolist

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="toodelete-a-route-from-a-route-table"></a>toodelete egy útvonalat az útvonal táblából

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="tooremove-a-route-from-a-subnet"></a>tooremove alhálózatból származó útvonal

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="toolist-hello-route-table-associated-with-a-subnet"></a>alhálózat társított toolist hello útvonaltábla

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="tooremove-a-default-site-from-a-vnet-vpn-gateway"></a>a virtuális hálózat VPN-átjáró alapértelmezett helyek tooremove

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```