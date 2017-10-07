---
title: "Csatlakozás egy számítógép tooan Azure virtuális hálózat használatával a pont-pont és a Tanúsítványalapú hitelesítés: PowerShell |} Microsoft Docs"
description: "Biztonságos kapcsolódás egy számítógép tooyour virtuális hálózatot hozzon létre egy pont – hely típusú VPN gateway-kapcsolatot tanúsítvány alapú hitelesítést használ. Ez a cikk toohello Resource Manager üzembe helyezési modellben vonatkozik, és használja a PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3eddadf6-2e96-48c4-87c6-52a146faeec6
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: b962e4b1946a4ae17d4eb2b920ed54437bc26b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-powershell"></a>Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása tanúsítvány hitelesítése: PowerShell

Ez a cikk bemutatja, hogyan toocreate egy Vnetet egy pont – hely kapcsolat hello Resource Manager üzembe a modellhez tartozó PowerShell-lel. Ez a konfiguráció tanúsítványok tooauthenticate hello csatlakozó ügyfél használja. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Pont-pont (P2S) VPN-átjáró lehetővé teszi a biztonságos kapcsolat tooyour virtuális hálózat létrehozása az egyéni ügyfél-számítógépről. Pont-pont VPN-kapcsolatok akkor hasznos, ha azt szeretné, hogy a virtuális hálózat egy távoli helyről, például amikor, amelyek dolgozzon home vagy konferencia tooconnect tooyour. P2S VPN esetén is egy hasznos megoldás toouse helyett a telephelyek közötti VPN tooconnect tooa VNet kell csak néhány ügyféllel.

P2S használja a Secure Socket Tunneling Protocol (SSTP), amely SSL-alapú VPN-protokoll hello. P2S VPN-kapcsolatot létesít hello ügyfélszámítógépről elindításával.

![Csatlakozás egy számítógép tooan Azure VNet - pont – hely kapcsolat diagramja](./media/vpn-gateway-howto-point-to-site-rm-ps/point-to-site-diagram.png)

Pont-pont tanúsítvány hitelesítési kapcsolatok hello következő szükséges:

* Útvonalalapú VPN-átjáró.
* hello nyilvános kulcsát (.cer-fájl) egy legfelső szintű tanúsítvány, amely feltöltött tooAzure. Hello tanúsítványt a feltöltést követően megbízható tanúsítvány minősül, és használják a hitelesítéshez.
* Hello legfelső szintű tanúsítvány által létrehozott és telepített minden egyes ügyfélszámítógépen toohello VNet csatlakozó ügyféltanúsítványt. A rendszer ezt a tanúsítványt használja ügyfélhitelesítéshez.
* A VPN-ügyfél konfigurációs csomagja. hello VPN-ügyfélcsomag konfigurációs hello ügyfél tooconnect toohello VNet hello szükséges információkat tartalmaz. hello csomag hello meglévő VPN-ügyfél, amely natív toohello Windows operációs rendszer konfigurálja. Minden ügyfél hello konfigurációs csomag használatával kell konfigurálni.

A pont–hely kapcsolatok nem igényelnek VPN-eszközt vagy helyszíni nyilvános IP-címet. hello VPN-kapcsolaton keresztül SSTP (Secure Socket Tunneling Protocol) jön létre. Hello kiszolgáló oldalán 1.0-s, 1.1-es és 1.2-es SSTP verziója támogatott. hello ügyfél úgy dönt, hogy melyik verzió toouse. Windows 8.1 és újabb kiadások esetén az SSTP alapértelmezés szerint az 1.2 verziót használja. 

Pont – hely kapcsolatok kapcsolatos további információkért lásd: hello [pont-pont – gyakori kérdések](#faq) hello Ez a cikk végén.

## <a name="before-beginning"></a>Mielőtt hozzálát

* Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial).
* Hello hello Azure Resource Manager PowerShell-parancsmagok legújabb verziójának telepítéséhez. PowerShell-parancsmagok telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

### <a name="example"></a>Példaértékek

Hello példa értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothese értékek toobetter hello jelen cikk példái a megismeréséhez. Hello változók szakaszban hivatott [1](#declare) hello cikk. Hello lépésekkel egy útmutató, és hello értékeket használja őket módosítása nélkül, vagy módosítsa őket tooreflect a környezetben. 

* **Név: VNet1**
* **Címtartomány: 192.168.0.0/16** és **10.254.0.0/16**<br>Az ebben a példában használjuk, amely ezt a konfigurációt az több címterek egynél több címet terület tooillustrate. Azonban nem kötelező több címtartományt megadni ehhez a konfigurációhoz.
* **Alhálózat neve: FrontEnd**
  * **Alhálózati címtartomány: 192.168.1.0/24**
* **Alhálózat neve: BackEnd**
  * **Alhálózati címtartomány: 10.254.1.0/24**
* **Alhálózat neve: GatewaySubnet**<br>hello alhálózati név *GatewaySubnet* hello VPN gateway toowork esetén kötelező.
  * **Átjáró-alhálózat címtartománya: 192.168.200.0/24** 
* **VPN-ügyfelek címkészlete: 172.16.201.0/24**<br>A VPN-ügyfelek toohello VNet a pont-pont kapcsolattal csatlakozó fogadása hello Magánhálózati ügyfélcímkészlete IP-címet.
* **Előfizetés:** Ha egynél több előfizetéssel, győződjön meg arról, hogy rendelkezik a megfelelő hello.
* **Erőforráscsoport: TestRG**
* **Hely: East US**
* **DNS-kiszolgáló: IP-cím** hello DNS-kiszolgáló, amelyet az toouse a névfeloldáshoz.
* **Átjáró neve: Vnet1GW**
* **Nyilvános IP-név: VNet1GWPIP**
* **VPN típusa: RouteBased** 

## <a name="declare"></a>1. Bejelentkezés és a változók beállítása

Ebben a szakaszban be, és ehhez a konfigurációhoz használt hello értékek deklarálható. hello deklarált értékek hello mintaparancsfájlok használatosak. Hello értékek tooreflect módosítsa a saját környezetben. Vagy használhat deklarált értékek hello és végrehajtania hello lépéseket, mint egy gyakorlatot.

1. Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és jelentkezzen be Azure-fiók tooyour. Ez a parancsmag kéri hello bejelentkezési hitelesítő adatokat. A bejelentkezés után az tölti le a fiókbeállításoknál, hogy-e elérhető tooAzure PowerShell.

  ```powershell
  Login-AzureRmAccount
  ```
2. Olvassa be az Azure-előfizetések listáját.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Adja meg, hogy szeretné-e toouse hello előfizetés.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
4. Deklarálja, hogy szeretné-e toouse hello változók. Hello használja, a következő mintát, és hello értékeket a saját, amikor erre szükség van.

  ```powershell
  $VNetName  = "VNet1"
  $FESubName = "FrontEnd"
  $BESubName = "Backend"
  $GWSubName = "GatewaySubnet"
  $VNetPrefix1 = "192.168.0.0/16"
  $VNetPrefix2 = "10.254.0.0/16"
  $FESubPrefix = "192.168.1.0/24"
  $BESubPrefix = "10.254.1.0/24"
  $GWSubPrefix = "192.168.200.0/26"
  $VPNClientAddressPool = "172.16.201.0/24"
  $RG = "TestRG"
  $Location = "East US"
  $DNS = "10.1.1.3"
  $GWName = "VNet1GW"
  $GWIPName = "VNet1GWPIP"
  $GWIPconfName = "gwipconf"
  ```

## <a name="ConfigureVNet"></a>2. Virtuális hálózat konfigurálása

1. Hozzon létre egy erőforráscsoportot.

  ```powershell
  New-AzureRmResourceGroup -Name $RG -Location $Location
  ```
2. Hozzon létre hello alhálózati beállítások hello virtuális hálózathoz, azok elnevezési *előtér*, *háttér*, és *GatewaySubnet*. Ezeket az előtagokat hello meg deklarált VNet címtér részének kell lennie.

  ```powershell
  $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
  $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
  $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
  ```
3. Hello virtuális hálózat létrehozása.

  Ebben a példában a hello DNS-kiszolgáló nem kötelező. Az érték megadásával nem jön létre új DNS-kiszolgáló. hello DNS kiszolgáló IP-cím megadott kell egy DNS-kiszolgáló, amely képes névfeloldásra hello hello erőforrásokhoz való kapcsolódás esetén. Ebben a példában a magánhálózati IP-cím használtuk, de valószínű, hogy ez nem hello IP-címet a DNS-kiszolgáló. Lehet, hogy toouse a saját értékeit.

  ```powershell
  New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS
  ```
4. Adja meg a létrehozott virtuális hálózat hello hello változók.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  ```
5. Egy VPN Gateway-nek rendelkeznie kell nyilvános IP-címmel. Először igényelnie hello IP-cím erőforrás, és ezután tooit hivatkozni, a virtuális hálózati átjáró létrehozása során. hello IP-cím dinamikusan toohello erőforrás van hozzárendelve, hello VPN-átjáró létrehozásakor. A VPN Gateway jelenleg csak a *Dinamikus* nyilvános IP-cím lefoglalását támogatja. Nem kérheti statikus IP-cím hozzárendelését. Azonban ez nem jelenti azt, hogy hello IP-cím hozzá van rendelve tooyour VPN-átjáró után módosítja. hello egyetlen alkalom hello nyilvános IP-cím módosításainak mikor van hello átjáró törlődik, és újból létrehozza. Nem módosul átméretezés, alaphelyzetbe állítás, illetve a VPN Gateway belső karbantartása/frissítése során.

  Kérjen egy dinamikusan hozzárendelt nyilvános IP-címet.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
  $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
  ```

## <a name="creategateway"></a>3. Hello VPN-átjáró létrehozása

Konfigurálja, és a Vnethez tartozó hello virtuális hálózati átjáró létrehozása.

* Hello *- GatewayType* kell **Vpn** és hello *– VpnType* kell **RouteBased**.
* VPN-átjáró is eltarthat, too45 perc toocomplete, attól függően, hogy hello [átjáró-termékváltozat](vpn-gateway-about-vpn-gateway-settings.md) választja.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
-VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 `
```

## <a name="addresspool"></a>4. Hello Magánhálózati ügyfélcímkészlete hozzáadása

Miután hello VPN-átjáró végzett a létrehozással, hello Magánhálózati ügyfélcímkészlete is hozzáadhat. hello Magánhálózati ügyfélcímkészlete, amelyből a hello VPN-ügyfelek IP-címet kap, kapcsolódáskor hello tartományon. Használjon egy privát IP-címtartományt, amely nem fedi át hello helyszíni helyre történő csatlakozás vagy hello tooconnect a kívánt virtuális hálózat. Ebben a példában hello Magánhálózati ügyfélcímkészlete van deklarálva, egy [változó](#declare) 1. lépésben.

```powershell
$Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
```

## <a name="Certificates"></a>5. Tanúsítványok előállítása

Tanúsítványok pont-pont VPN Azure tooauthenticate VPN-ügyfelek által használt. Hello nyilvánoskulcs-adatokat a hello legfelső szintű tanúsítvány tooAzure feltöltése. nyilvános kulcs hello majd minősül "megbízható". Ügyféltanúsítványok kell kell hello megbízható legfelső szintű tanúsítvány jön létre, és csak utána települ a hello tanúsítványok-aktuális felhasználó/személyes tanúsítványtárolóba minden egyes ügyfélszámítógépre. hello tanúsítvány használt tooauthenticate hello ügyfél, amikor kezdeményezik a kapcsolat toohello virtuális hálózat. 

Önaláírt tanúsítványok használata esetén azokat megadott paraméterekkel kell létrehozni. Létrehozhat egy önaláírt tanúsítványt a hello utasításokat követve [PowerShell és Windows 10](vpn-gateway-certificates-point-to-site.md), vagy ha nincs Windows 10, [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Fontos lépéseket hello hello utasítások önaláírt legfelső szintű tanúsítványok és az ügyféltanúsítványok létrehozásakor. Ellenkező esetben létrehozhat hello tanúsítvány nem lesz kompatibilis a P2S-kapcsolatok, és hibaüzenet jelenik meg a kapcsolat.

### <a name="cer"></a>1. Hello .cer fájl hello legfelső szintű tanúsítvány beszerzése

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]


### <a name="generate"></a>2. Ügyféltanúsítvány létrehozása

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>6. Hello legfelső szintű tanúsítvány nyilvános kulcs adatok feltöltése

Ellenőrizze, hogy a VPN-átjáró létrehozása befejeződött-e. Miután befejezte, hello .cer fájlját (hello nyilvánoskulcs-adatokat tartalmazó) a megbízható legfelső szintű tanúsítvány tooAzure feltölthet. A.cer fájl a feltöltést követően Azure használható tooauthenticate ügyfelek, amelyek telepítették a hello megbízható legfelső szintű tanúsítvány által létrehozott ügyféltanúsítványt. Szükség esetén további megbízható legfelső szintű tanúsítvány fájlok - tooa összesen 20 - később fel feltölthet.

1. A tanúsítvány neve hello érték helyett a saját hello változó deklarálható.

  ```powershell
  $P2SRootCertName = "P2SRootCert.cer"
  ```
2. Cserélje le hello fájl elérési útját a saját, és futtassa a hello parancsmagok.

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
  ```
3. Hello nyilvánoskulcs-adatokat tooAzure feltöltése. Hello tanúsítvány adatait a feltöltést követően Azure úgy ítéli meg, ez toobe megbízható főtanúsítványt.

   ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
  ```

## <a name="clientconfig"></a>7. Hello VPN-ügyfélcsomag konfigurációs letöltése

tooconnect tooa egy pont – hely VPN hálózatok, minden ügyfél telepítenie kell egy konfigurációs ügyfélcsomagot, amely hello beállításokkal konfigurálja a hello natív VPN-ügyfél és a szükséges tooconnect toohello virtuális hálózati fájlokat. hello VPN-ügyfélcsomag konfigurációs hello natív Windows VPN-ügyfél konfigurálja, egy másik VPN-ügyfél nem telepít. 

Minden egyes ügyfélszámítógépre csomag azonos VPN-ügyfél konfigurációja hello mindaddig, amíg hello verzióegyezéseket hello architektúra hello ügyfél használhatja. Ügyfél által támogatott operációs rendszerek hello listájáért lásd: hello [pont – hely kapcsolatok gyakran ismételt kérdések](#faq) hello Ez a cikk végén.

1. Hello átjáró létrehozása után létrehozhat és hello ügyfél konfigurációs csomag. Ez a példa hello csomagot tölti le a 64 bites ügyfeleken. Ha azt szeretné, hogy toodownload hello 32 bites ügyfél, a "Amd64" lecserélése "x86". Emellett letöltheti hello VPN-ügyfél hello Azure-portál használatával.

  ```powershell
  Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
  -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64
  ```
2. Másolással illessze be a visszaadott tooa webes böngésző toodownload hello csomag, ügyelve tooremove hello ajánlatok hello hivatkozás körülvevő hello hivatkozásra. 
3. Töltse le, és telepítse a hello csomag hello ügyfélszámítógépen. Ha megjelenik a SmartScreen egy előugró ablaka, kattintson a **További információ**, majd a **Futtatás mindenképpen** elemre. Egyéb ügyfélszámítógépeire hello csomag tooinstall is mentheti.
4. Hello ügyfélszámítógépen nyissa meg túl**hálózati beállítások** kattintson **VPN**. VPN-kapcsolat hello hello csatlakozó virtuális hálózati hello nevét jeleníti meg.

## <a name="clientcertificate"></a>8. Exportált ügyféltanúsítvány telepítése

Ha azt szeretné, hogy egy P2S toocreate kapcsolat eltérő hello ügyfélszámítógépről egy használt toogenerate hello ügyféltanúsítványokat, tooinstall ügyféltanúsítvány szükséges. Ügyfél-tanúsítvány telepítése, úgy kell hello jelszó hello ügyfél tanúsítvány exportálása során létrejött. Ez általában csak egy függetlenül attól, hogy duplán hello tanúsítványt, és telepíti azt.

Ellenőrizze, hogy hello ügyféltanúsítvány egy .pfx együtt hello teljes láncát (amely hello alapértelmezett) típusúként lett exportálva. Ellenkező esetben hello legfelső szintű tanúsítvány adatait nincs jelen hello ügyfélszámítógépen, és hello ügyfél megfelelően nem fogja tudni tooauthenticate. További információkért lásd az [exportált ügyféltanúsítványok telepítését](vpn-gateway-certificates-point-to-site.md#install) ismertető cikket. 

## <a name="connect"></a>9. Csatlakozás tooAzure

1. tooconnect tooyour VNet hello ügyfélszámítógépen nyissa meg a tooVPN kapcsolatok, és keresse meg a létrehozott hello VPN-kapcsolatot. Hello azonos nevet a virtuális hálózatnak nevezik. Kattintson a **Connect** (Csatlakozás) gombra. Előugró üzenet jelenhet meg, hogy toousing hello tanúsítvány hivatkozik. Kattintson a **Folytatás** toouse emelt szintű jogosultságokkal. 
2. A hello **kapcsolat** állapotlapon, kattintson a **Connect** toostart hello kapcsolat. Ha megjelenik egy **tanúsítvány kiválasztása** képernyőn, győződjön meg arról, hogy hello ügyfél tanúsítvány ábrázoló, amelyet az toouse tooconnect egy hello. Ha nem, hello nyílra tooselect hello megfelelő tanúsítványt használjon, és kattintson a **OK**.

  ![VPN-ügyfél kapcsolódik tooAzure](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png)
3. A kapcsolat létrejött.

  ![A kapcsolat létrejött](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Pont–hely kapcsolatok hibaelhárítása

[!INCLUDE [client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>10. A kapcsolat ellenőrzése

1. tooverify, hogy a VPN-kapcsolatot az aktív, nyisson meg egy rendszergazda jogú parancssort, és futtassa *ipconfig/all*.
2. Hello eredményeinek megtekintése. Láthatja, hogy hello IP-cím kapott hello hello pont-pont Magánhálózati Ügyfélcímkészlete a konfigurációban megadott címek egyikét. hello eredményei hasonló toothis példa:

  ```
  PPP adapter VNet1:
      Connection-specific DNS Suffix .:
      Description.....................: VNet1
      Physical Address................:
      DHCP Enabled....................: No
      Autoconfiguration Enabled.......: Yes
      IPv4 Address....................: 172.16.201.3(Preferred)
      Subnet Mask.....................: 255.255.255.255
      Default Gateway.................:
      NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Csatlakoztassa tooa virtuális gépet

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="addremovecert"></a>Főtanúsítvány hozzáadása vagy eltávolítása

A megbízható főtanúsítványokat felveheti vagy el is távolíthatja az Azure-ban. Ha eltávolít egy legfelső szintű tanúsítványt, hello legfelső szintű tanúsítványt generált tanúsítvánnyal rendelkező ügyfelek nem tudják hitelesíteni magukat, és nem fogja tudni tooconnect. Ha szeretné, hogy egy ügyfél tooauthenticate, és csatlakozni tud kell tooinstall (feltöltött) tooAzure megbízható legfelső szintű tanúsítványokat létre egy új ügyféltanúsítványt.

### <a name="addtrustedroot"></a>a megbízható legfelső szintű tanúsítvány tooadd

Másolatot too20 legfelső szintű tanúsítvány .cer fájlok tooAzure adhat hozzá. hello alábbi lépéseit súgó legfelső szintű tanúsítvány hozzáadása:

#### <a name="certmethod1"></a>1. módszer:

Ez a hello leghatékonyabb módszer tooupload egy legfelső szintű tanúsítványt.

1. Hello .cer fájl tooupload előkészítése:

  ```powershell
  $filePathForCert = "C:\cert\P2SRootCert3.cer"
  $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
  $CertBase64_3 = [system.convert]::ToBase64String($cert.RawData)
  $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64_3
  ```
2. Hello-fájl feltöltése. Egyszerre csak egy fájlt tölthet fel.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64_3
  ```

3. tooverify adott hello tanúsítványfájl feltöltött:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

#### <a name="certmethod2"></a>2. módszer:

Ez a módszer akkor metódus 1-nél több lépésből áll, de van hello ugyanazt az eredményt. Szerepel abban az esetben szüksége tooview hello tanúsítványának adatait.

1. Hozzon létre, és készítse elő a hello új legfelső szintű tanúsítvány tooadd tooAzure. Hello nyilvános kulcsának exportálásához, mint egy Base-64 kódolású X.509 (. CER), majd nyissa meg szövegszerkesztőben. Hello értékek, másolása, ahogy az alábbi példa hello:

  ![tanúsítvány](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png)

  > [!NOTE]
  > Ha hello Tanúsítványadatok másol, győződjön meg arról, hogy egy folyamatos sorba kocsivissza és soremelés nélkül hello szöveg másolása. Szükség lehet toomodify a nézeten belül hello text editor too'Show szimbólum/megjelenítése összes karakter toosee hello kocsivissza értéket ad vissza, és hírcsatornák sor.
  >
  >

2. Adja meg a hello tanúsítvány és kulcs adatai változóként. Cserélje le a saját hello az alábbi példa hello információkat:

  ```powershell
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
3. Hello új legfelső szintű tanúsítvány hozzáadása. Egyszerre csak egy tanúsítványt adhat hozzá.

  ```powershell
  Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
4. Hello új tanúsítvány helyesen lett-e hozzáadva a következő példa hello segítségével ellenőrizheti:

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

### <a name="removerootcert"></a>egy legfelső szintű tanúsítványt tooremove

1. Hello változó deklarálható.

  ```powershell
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  $P2SRootCertName2 = "ARMP2SRootCert2.cer"
  $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"
  ```
2. Távolítsa el a hello tanúsítványt.

  ```powershell
  Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
  ```
3. A következő példa tooverify, hogy a tanúsítvány hello használata hello sikeresen el lett távolítva.

  ```powershell
  Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
  -VirtualNetworkGatewayName "VNet1GW"
  ```

## <a name="revoke"></a>Ügyféltanúsítvány visszavonása

Az ügyféltanúsítványokat vissza lehet vonni. hello tanúsítvány-visszavonási lista lehetővé teszi a tooselectively visszautasítja a pont – hely kapcsolat egyedi ügyféltanúsítványok alapján. Ez a folyamat eltér a megbízható főtanúsítvány eltávolításától. Ha eltávolítja a megbízható legfelső szintű tanúsítvány .cer az Azure-ból, azt minden tanúsítványt generált/aláírt hello visszavont legfelső szintű tanúsítvány hello hozzáférés visszavonása. Ügyfél-tanúsítvány visszavonásával, ahelyett, hogy a főtanúsítvány hello, lehetővé teszi, hogy hello más is létrehozott, hello legfelső szintű tanúsítvány toocontinue toobe hitelesítéshez használt tanúsítványok.

hello általános gyakorlat toouse hello legfelső szintű tanúsítvány toomanage hozzáférés csapat vagy szervezet szinten egyéni felhasználók számára a minden részletre kiterjedő hozzáférés-vezérléshez visszavont ügyféltanúsítványok használata során.

### <a name="revokeclientcert"></a>toorevoke ügyféltanúsítványt

1. Hello ügyfél tanúsítványának ujjlenyomata beolvasása. További információkért lásd: [hogyan tooretrieve hello tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695.aspx).
2. Másolja a hello információk tooa szövegszerkesztőben, és úgy, hogy egy folyamatos karakterláncként, távolítsa el az összes szóközöket. Ez a karakterlánc a következő lépésben hello van deklarálva, egy változó.
3. Hello változó deklarálható. Győződjön meg arról, hogy toodeclare hello ujjlenyomat lekért hello előző lépésben.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
4. Adja hozzá a hello ujjlenyomat toohello visszavont tanúsítványok listáját. Megjelenik az "Succeeded" hello ujjlenyomat hozzáadásakor.

  ```powershell
  Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG `
  -Thumbprint $RevokedThumbprint1
  ```
5. Győződjön meg arról, hogy hello ujjlenyomat hozzá lett adva toohello visszavont tanúsítványok listáját.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```
6. Hello ujjlenyomat hozzáadását követően hello tanúsítvány már nem használt tooconnect lehet. Ügyfelek, amelyek ezzel a tanúsítvánnyal tooconnect kap üzenetet kap arról, hogy hello tanúsítvány hatályát veszti.

### <a name="reinstateclientcert"></a>tooreinstate ügyféltanúsítványt

Ügyféltanúsítvány visszaállíthatja hello ujjlenyomat eltávolítása a visszavont ügyféltanúsítványok hello listája.

1. Hello változó deklarálható. Ellenőrizze, hogy deklarálhatja hello megfelelő, amelyet az tooreinstate hello-tanúsítványának ujjlenyomata.

  ```powershell
  $RevokedClientCert1 = "NameofCertificate"
  $RevokedThumbprint1 = "‎51ab1edd8da4cfed77e20061c5eb6d2ef2f778c7"
  $GWName = "Name_of_virtual_network_gateway"
  $RG = "Name_of_resource_group"
  ```
2. Távolítsa el a hello tanúsítvány ujjlenyomata a hello visszavont tanúsítványok listáját.

  ```powershell
  Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
  -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1
  ```
3. Ellenőrizze, hogy hello ujjlenyomata a rendszer eltávolítja a hello lista visszavonva.

  ```powershell
  Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG
  ```

## <a name="faq"></a>Pont–hely kapcsolatok – gyakori kérdések

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Következő lépések
Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). További információ a hálózati és a virtuális gépek toounderstand lásd: [Azure és a Linux virtuális gép hálózati áttekintés](../virtual-machines/linux/azure-vm-network-overview.md).
