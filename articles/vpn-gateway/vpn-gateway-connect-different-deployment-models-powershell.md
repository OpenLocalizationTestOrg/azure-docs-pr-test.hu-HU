---
title: "Csatlakoztassa a klasszikus virtuális hálózatok tooAzure erőforrás-kezelő Vnetek: PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate klasszikus virtuális hálózatokat és erőforrás-kezelő Vnetek VPN-átjáró és a PowerShell használatával közötti VPN-kapcsolat"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
ms.openlocfilehash: 8b1cf6ae4becf1829fa99961c5dd09a422fcc1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Különböző üzemi modellekből származó virtuális hálózatok összekapcsolása a PowerShell-lel



Ez a cikk bemutatja, hogyan tooconnect hagyományos Vneteket tooResource Manager Vnetek tooallow hello hello külön telepítési modellek toocommunicate egymás mellett található erőforrásokhoz. hello cikkben leírt lépéseket a PowerShell segítségével, de ez a konfiguráció hello cikk jelöl ki a listában hello Azure-portál használatával is létrehozhat.

> [!div class="op_single_selector"]
> * [Portál](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Csatlakozás egy klasszikus virtuális hálózat tooa erőforrás-kezelő virtuális hálózat hasonló tooconnecting egy VNet tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat. Létrehozhat, amelyek különböző előfizetésekhez és különböző régiókban Vnetek közötti kapcsolat. Kapcsolatok tooon helyszíni hálózatokban, már rendelkező Vnetek mindaddig, amíg hello átjáró, amely a konfigurálva a dinamikus- vagy útválasztó-alapú is csatlakoztathatja. VNet – VNet kapcsolatokhoz kapcsolatos további információkért lásd: hello [VNet – VNet – gyakori kérdések](#faq) hello Ez a cikk végén. 

Ha a Vnetek hello ugyanabban a régióban, érdemes lehet tooinstead vegye figyelembe, hogy csatlakoztatja őket a Vnetben társviszony-létesítés használatával. A virtuális hálózatok közötti társviszony nem használ VPN-átjárót. További információ: [Társviszony létesítése virtuális hálózatok között](../virtual-network/virtual-network-peering-overview.md). 

## <a name="before-beginning"></a>Mielőtt hozzálát

hello következő lépések végigvezetik Önt hello beállítások szükséges tooconfigure dinamikus- vagy útválasztó-alapú átjáró minden vnet és hello átjárók közötti VPN-kapcsolatot. Ez a konfiguráció nem támogatja a statikus vagy csoportházirend-alapú átjárók.

### <a name="prerequisites"></a>Előfeltételek

* Mindkét Vnetek létre lett hozva.
* hello címtartományát hello Vnetek nem lehetnek átfedésben vannak egymással, vagy átfedésben vannak egyetlen hello tartományok hello átjárók csatlakoztathatók más kapcsolatok esetében.
* Hello legújabb PowerShell-parancsmagok telepítése. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) további információt. Ellenőrizze, hogy hello szolgáltatás-felügyeleti (SM) és erőforrás-kezelő (RM) parancsmagok hello telepítenie. 

### <a name="exampleref"></a>Példabeállítások

Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.

**Klasszikus virtuális hálózat beállításai**

Virtuális hálózat neve = ClassicVNet <br>
Hely = USA nyugati régiója <br>
Virtuális hálózat = 10.0.0.0/24 <br>
Alhálózat-1 = 10.0.0.0/27 <br>
A GatewaySubnet = 10.0.0.32/29 <br>
Helyi hálózati név = RMVNetLocal <br>
GatewayType = DynamicRouting

**Erőforrás-kezelő VNet beállításait**

Virtuális hálózat neve = RMVNet <br>
Erőforráscsoport = RG1 <br>
Virtuális hálózati IP-címterek = 192.168.0.0/16 <br>
Alhálózat-1 = 192.168.1.0/24 <br>
A GatewaySubnet = 192.168.0.0/26 <br>
Hely = USA keleti régiója <br>
Átjáró nyilvános IP-név = gwpip <br>
Helyi hálózati átjáró = ClassicVNetLocal <br>
A virtuális hálózati átjáró neve = RMGateway <br>
Átjáró IP-címzési beállításokat = gwipconfig

## <a name="createsmgw"></a>1. szakasz - konfigurálása hello klasszikus virtuális hálózaton
### <a name="part-1---download-your-network-configuration-file"></a>1. rész – a hálózati konfigurációs fájl letöltése
1. Jelentkezzen be tooyour Azure-fiók hello PowerShell-konzolban emelt szintű jogosultságokkal. hello következő parancsmag kéri hello bejelentkezési hitelesítő adatokat az Azure-fiók. A bejelentkezés után az tölti le a fiókbeállításoknál, hogy-e elérhető tooAzure PowerShell. Service Manager PowerShell-parancsmagok toocomplete hello hello konfigurációs ezen része használja.

  ```powershell
  Add-AzureAccount
  ```
2. Az Azure-hálózat konfigurációs fájl exportálása hello a következő parancs futtatásával. Hello helyét hello fájl tooexport tooa máshová szükség esetén módosíthatja.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. Nyitott hello tartalmazó .xml-fájlt, hogy a tooedit le azt. Hello hálózati konfigurációs fájl példa, lásd: hello [hálózati konfigurációs séma](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="part-2--verify-hello-gateway-subnet"></a>Rész 2 - hello átjáróalhálózatot ellenőrzése
A hello **VirtualNetworkSites** elemet, adja hozzá egy átjáró alhálózati tooyour VNet, ha nem már létrehozták. Az hello hálózati konfigurációs fájl használatakor hello átjáróalhálózatot névvel kell ellátni "GatewaySubnet" vagy az Azure nem ismeri fel és nem használható egy átjáró-alhálózatot.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Példa**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-hello-local-network-site"></a>3. rész – hello helyi hálózati hely hozzáadása
hozzáadhat hello helyi hálózati telephely hello tooconnect kívánt erőforrás-kezelő virtuális hálózat toowhich jelöli. Adja hozzá a **LocalNetworkSites** elem toohello fájlt, ha egy már nem létezik. Ezen a ponton hello a konfigurációban az hello VPNGatewayAddress lehet egy másik érvényes nyilvános IP-címet, mert azt még nem hozta létre az erőforrás-kezelő virtuális hálózat hello hello átjáró. Miután létrehozhatunk hello átjáró, azt cserélje le a helyőrző IP-cím hello megfelelő nyilvános IP-címet, amelyet toohello RM átjáró rendelt.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-hello-vnet-with-hello-local-network-site"></a>Rész 4 – hello hálózatok társítása hello helyi hálózati telephely
Ebben a szakaszban adtuk meg, hogy szeretné-e tooconnect hello helyi hálózati telephely hello VNet. Ebben az esetben is, amely korábban hivatkozott erőforrás-kezelő VNet hello. Győződjön meg arról, hogy hello nevének felel meg. Ez a lépés nem hoz létre egy átjáró. Meghatározza az hello helyi hálózati átjáró hello fog csatlakozni.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-hello-file-and-upload"></a>5 - rész mentés hello fájl- és feltöltése
Hello fájl mentéséhez, majd importálja azt hello a következő parancs futtatásával tooAzure. Ellenőrizze, hogy módosítja a környezetben a megfelelő hello fájl elérési útját.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

Látni fogja, hogy sikerült-e hello importálás megjelenítő hasonló eredményt.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-hello-gateway"></a>6. rész – hello átjáró létrehozása

Mielőtt futtatná az ebben a példában, tekintse meg a toohello hálózati konfigurációs fájl a pontos hello nevét, hogy Azure letöltött toosee vár. hello hálózati konfigurációs fájlt a klasszikus virtuális hálózatok hello értéket tartalmaz. Egyes esetekben Vnetek hello hálózati konfigurációs fájlban, módosulnak, a klasszikus virtuális hálózat beállítások létrehozásakor klasszikus hello neveit hello Azure-portálon hello üzembe helyezési modellel toohello eltérései miatt. Például egy klasszikus virtuális hálózat "Klasszikus virtuális hálózat" nevű és "ClassicRG" nevű erőforráscsoportban létrehozta az Azure portál toocreate hello használva hello neve hello hálózati konfigurációs fájlban lévő akkor konvertált too'Group ClassicRG klasszikus virtuális hálózat ". A szóközöket tartalmazó VNet neve hello megadásakor idézőjelekbe hello érték.


A következő példa toocreate dinamikus útválasztó átjáró hello használata:

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

Hello segítségével ellenőrizheti a hello átjáró hello állapotának **Get-AzureVNetGateway** parancsmag.

## <a name="creatermgw"></a>2. rész: Hello RM hálózatok átjáró konfigurálása
toocreate hello erőforrás-kezelő virtuális hálózat, a VPN-átjáró kövesse az utasításokat követve hello. Hello klasszikus virtuális hálózat átjáró hello nyilvános IP-cím beolvasása után hello lépéseket addig ne indítsa el. 

1. Jelentkezzen be tooyour Azure-fiók hello PowerShell-konzolban. hello következő parancsmag kéri hello bejelentkezési hitelesítő adatokat az Azure-fiók. A bejelentkezés után a fiók beállításait, hogy-e elérhető tooAzure PowerShell lesznek letöltve.

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  Az Azure-előfizetések listájának lekérdezése, ha egynél több előfizetéssel rendelkezik.

  ```powershell
  Get-AzureRmSubscription
  ```
   
  Adja meg, hogy szeretné-e toouse hello előfizetés.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. A helyi hálózati átjáró létrehozása. Egy virtuális hálózatban hello helyi hálózati átjáró általában tooyour helyszíni helyre hivatkozik. Ebben az esetben a hello helyi hálózati átjáró tooyour klasszikus virtuális hálózatot jelenti. Adjon neki egy nevet, amellyel Azure is jelenti tooit és hello terület címelőtag is megadhatja. Azure hello IP-cím előtagján megadhatja, mely forgalom toosend tooyour helyszíni hely tooidentify használja. Ha tájékoztatásra van szüksége tooadjust hello itt később, az átjáró létrehozása előtt módosíthatja hello értékek és a futási hello minta újra.
   
   **-Név** tooassign toorefer toohello helyi hálózati átjáró kívánt hello neve.<br>
   **-AddressPrefix** hello a klasszikus virtuális hálózat-tartomány.<br>
   **-GatewayIpAddress** hello klasszikus virtuális hálózat átjáró hello nyilvános IP-címe. Győződjön meg arról, hogy toochange hello következő minta tooreflect hello megfelelő IP-címet.<br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. Egy nyilvános IP-cím toobe lefoglalt toohello virtuális hálózati átjáró hello erőforrás-kezelő virtuális hálózat a kérelmet. Nem adható meg, hogy szeretné-e toouse hello IP-címet. hello IP-cím dinamikusan lefoglalt toohello virtuális hálózati átjáró. Azonban ez nem jelenti hello IP-címet érintő módosításait. hello egyetlen alkalom hello virtuális hálózati átjáró IP-cím módosításainak mikor van hello átjáró törlődik, és újra létrehozza. Átméretezése, alaphelyzetbe állítása vagy más belső karbantartás vagy frissítés hello átjáró nem módosítja.

  Ebben a lépésben is hivatott egy változó, amely egy későbbi lépésben lesz szükség.

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. Ellenőrizze, hogy a virtuális hálózati átjáró-alhálózatot. Ha nincs átjáró-alhálózat létezik-e, vegyen fel egyet. Ellenőrizze, hogy hello átjáró alhálózatának elnevezése *GatewaySubnet*.
5. Hello a következő parancs futtatásával hello átjáróként használt hello alhálózat beolvasása. Ebben a lépésben azt is beállíthatja a egy változó toobe hello következő lépésben szükség.
   
   **-Név** hello az erőforrás-kezelő virtuális hálózat neve.<br>
   **-ResourceGroupName** hello erőforráscsoportban van, hogy hello virtuális hálózat társítva van. hello átjáró-alhálózatot a virtuális hálózat már léteznie kell, és névvel kell ellátni *GatewaySubnet* toowork megfelelően.<br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. Hello átjáró IP-konfiguráció címzési létrehozása. hello átjáró konfigurációs hello alhálózatot és a hello nyilvános IP-cím toouse határoz meg. A következő minta toocreate hello használata az átjáró konfigurációját.

  Ebben a lépésben hello **- SubnetId** és **- PublicIpAddressId** paramétereket kell átadni hello azonosítóját megadó tulajdonságot hello alhálózatot és IP-cím objektumok, illetve. Nem használhat egy egyszerű karakterlánc. Ezek a változók a hello lépés toorequest egy nyilvános IP-cím és lépés tooretrieve hello alhálózati hello.

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. Hello erőforrás-kezelő virtuális hálózati átjáró létrehozása hello a következő parancs futtatásával. Hello `-VpnType` kell *RouteBased*. Vagy akár többet hello átjáró toocreate 45 percet is igénybe vehet.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. Hello VPN-átjáró létrehozása után, másolja a hello nyilvános IP-cím. Használja a klasszikus virtuális hálózat hello helyi hálózati beállításainak konfigurálásakor. A következő parancsmag tooretrieve hello nyilvános IP-cím hello is használhatja. hello nyilvános IP-cím szerepel, visszatérési hello *IP-cím*.

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="section-3-modify-hello-classic-vnet-local-site-settings"></a>3. rész: Hello klasszikus virtuális hálózat helyi webhely beállításainak módosítása

Ez a szakasz használata hello klasszikus virtuális hálózatot. Lecseréli a hello helyőrző IP-címet, amely lesz használt tooconnect toohello erőforrás-kezelő hálózatok átjáró hello helyi helybeállításokat megadásakor használt. 

1. Hello hálózati konfigurációs fájl exportálása.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Egy szövegszerkesztővel, módosíthatók hello érték vpngatewayaddress elemmel. Cserélje le hello helyőrző IP-cím erőforrás-kezelő átjáró hello hello nyilvános IP-címét, és mentse a hello módosításokat.

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. Importálás hello módosítani a hálózati konfigurációs fájl tooAzure.

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <a name="connect"></a>4. szakasz: Hello átjárók közötti kapcsolat létrehozása
PowerShell hello átjárók közötti kapcsolat létrehozásához szükséges. Szükség lehet tooadd az Azure-fiók toouse hello klasszikus verziója hello PowerShell-parancsmagokkal. Igen, használjon toodo **Add-AzureAccount**.

1. Hello PowerShell-konzolban a megosztott kulcs beállítása. Hello parancsmagok futtatása előtt tekintse meg a toohello hálózati konfigurációs fájl a pontos hello nevét, hogy Azure letöltött toosee vár. A szóközöket tartalmazó VNet neve hello megadásakor idézőjelekbe egyetlen hello érték.<br><br>A következő példában **- VNetName** hello hello neve klasszikus virtuális hálózatot és **- LocalNetworkSiteName** hello helyi hálózati telephelyhez megadott hello neve. Hello **- SharedKey** érték, amely Ön hozza létre, és adja meg. Hello példában használtuk "abc123", de Ön hozza létre és összetettebb használja. fontos dolog, hogy az itt megadott hello érték hello hello azonos értéket adjon meg a következő lépésben hello a kapcsolat létrehozásakor kell lennie. megjelennek az visszatérési hello **állapota: sikeres**.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. Hello VPN-kapcsolat létrehozása hello a következő parancsok futtatásával:
   
  Hello változók megadása.

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  Hello kapcsolat létrehozása. Figyelje meg, hogy hello **- ConnectionType** IPsec, nem Vnet2Vnet van.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="section-5-verify-your-connections"></a>Szakasz 5: Ellenőrizze a kapcsolatokat.

### <a name="tooverify-hello-connection-from-your-classic-vnet-tooyour-resource-manager-vnet"></a>a klasszikus virtuális hálózat tooyour erőforrás-kezelő virtuális hálózat tooverify hello kapcsolat

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="tooverify-hello-connection-from-your-resource-manager-vnet-tooyour-classic-vnet"></a>az erőforrás-kezelő virtuális hálózat tooyour tooverify hello kapcsolatot klasszikus virtuális hálózaton

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Virtuális hálózatok közötti kapcsolat – gyakori kérdések

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

