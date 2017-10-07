---
title: "Számítógép tooa virtuális hálózatot pont-hely- és Tanúsítványalapú hitelesítés használatával: a klasszikus Azure portálon |} Microsoft Docs"
description: "Biztonságos kapcsolódás tooyour klasszikus Azure virtuális hálózatot hozzon létre egy pont – hely típusú VPN gateway-kapcsolatot hello Azure-portál használatával."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: cherylmc
ms.openlocfilehash: 9b53ba43ee4dfb61defeec458905fb1f1b18c3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-classic-azure-portal"></a>Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása (klasszikus)-alapú hitelesítést használó: Azure-portálon

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Ez a cikk bemutatja, hogyan toocreate egy Vnetet az a pont – hely kapcsolat hello klasszikus telepítési modell használatával hello Azure-portálon. Ez a konfiguráció tanúsítványok tooauthenticate hello csatlakozó ügyfél használja. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>

Pont-pont (P2S) VPN-átjáró lehetővé teszi a biztonságos kapcsolat tooyour virtuális hálózat létrehozása az egyéni ügyfél-számítógépről. Pont-pont VPN-kapcsolatok akkor hasznos, ha azt szeretné, hogy a virtuális hálózat egy távoli helyről, például amikor, amelyek dolgozzon home vagy konferencia tooconnect tooyour. P2S VPN esetén is egy hasznos megoldás toouse helyett a telephelyek közötti VPN tooconnect tooa VNet kell csak néhány ügyféllel. 

P2S használja a Secure Socket Tunneling Protocol (SSTP), amely SSL-alapú VPN-protokoll hello. P2S VPN-kapcsolatot létesít hello ügyfélszámítógépről elindításával.


![Pont–hely diagram](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)


Pont-pont tanúsítvány hitelesítési kapcsolatok hello következő szükséges:

* Egy dinamikus VPN-átjáró.
* hello nyilvános kulcsát (.cer-fájl) egy legfelső szintű tanúsítvány, amely feltöltött tooAzure. A rendszer ezt megbízható tanúsítványnak tekinti, és ezt használja hitelesítéshez.
* Ügyféltanúsítvány hello legfelső szintű tanúsítványt generált telepítve, és minden olyan ügyfélszámítógépen, amelyek csatlakozni fognak. A rendszer ezt a tanúsítványt használja ügyfélhitelesítéshez.
* Minden csatlakozó ügyfélszámítógépen létre kell hozni és telepíteni kell egy VPN-ügyfélkonfigurációs csomagot. ügyfél-konfigurációs csomag hello hello natív VPN-ügyfél, amely már a hello szükséges információkat tooconnect toohello VNet hello operációs rendszer konfigurálja.

A pont–hely kapcsolatok nem igényelnek VPN-eszközt vagy helyszíni nyilvános IP-címet. hello VPN-kapcsolaton keresztül SSTP (Secure Socket Tunneling Protocol) jön létre. Hello kiszolgáló oldalán 1.0-s, 1.1-es és 1.2-es SSTP verziója támogatott. hello ügyfél úgy dönt, hogy melyik verzió toouse. Windows 8.1 és újabb kiadások esetén az SSTP alapértelmezés szerint az 1.2 verziót használja. 

Pont – hely kapcsolatok kapcsolatos további információkért lásd: hello [pont-pont – gyakori kérdések](#faq) hello Ez a cikk végén.

### <a name="example-settings"></a>Példabeállítások

A következő értékek toocreate egy tesztkörnyezetben hello használhatja, vagy tekintse meg a toothese értékek toobetter megérteni a cikkben szereplő példák hello:

* **Név: VNet1**
* **Címtér: 192.168.0.0/16**<br>Ebben a példában csak egy címteret használunk. Azonban a virtuális hálózatához több címteret is használhat.
* **Alhálózat neve: FrontEnd**
* **Alhálózati címtartomány: 192.168.1.0/24**
* **Előfizetés:** Ha egynél több előfizetéssel, győződjön meg arról, hogy rendelkezik a megfelelő hello.
* **Erőforráscsoport: TestRG**
* **Hely: East US**
* **Kapcsolat típusa: pont–hely**
* **Ügyfélcímtér: 172.16.201.0/24**. A VPN-ügyfelek a pont – hely kapcsolatot VNet IP-címet kap toohello csatlakozó hello megadott készlet.
* **Átjáró-alhálózat: 192.168.200.0/24**. hello átjáróalhálózatot hello neve "GatewaySubnet" kell használnia.
* **Méret:** válassza hello átjáró, amelyet az toouse Termékváltozat.
* **Útválasztási típus: dinamikus**

## <a name="vnetvpn"></a>1. Virtuális hálózat és VPN-átjáró létrehozása

Mielőtt elkezdi végrehajtani a lépéseket, győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial).

### <a name="createvnet"></a>1. rész: Virtuális hálózat létrehozása

Ha még nem rendelkezik virtuális hálózattal, akkor hozzon létre egyet. A képernyőképek csak példaként szolgálnak. Lehet, hogy tooreplace hello értékeket a saját. egy virtuális hálózat használatával toocreate hello Azure-portálon a lépéseket követve használata hello:

1. Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.
2. Kattintson az **Új** lehetőségre. A hello **hello a piactéren** mezőbe írja be a "Virtuális hálózat". Keresse meg **virtuális hálózati** hello adott vissza a listán, és kattintson a tooopen hello **virtuális hálózati** lap.

  ![Virtuális hálózat keresése lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Hello virtuális hálózat lap, a hello hello alján közelében **telepítési modell kiválasztása** listára, válassza ki **klasszikus**, és kattintson a **létrehozása**.

  ![Üzemi modell kiválasztása](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. A hello **virtuális hálózat létrehozása** lapján hello hálózatok beállításainak konfigurálása. Ezen a lapon adhatja hozzá az első címterét és egy önálló alhálózati címtartományt. Hello VNet létrehozása után visszaléphet, és adja hozzá a további alhálózatokat és címtereket.

  ![Virtuális hálózat létrehozása lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Győződjön meg arról, hogy hello **előfizetés** hello, helyes-e egy. Előfizetések hello legördülő lista használatával módosíthatja.
6. Kattintson az **Erőforráscsoport** elemre, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat az új erőforráscsoport nevének beírásával. Ha hoz létre egy új erőforráscsoportot, neve hello erőforráscsoport tooyour megfelelően tervezett konfigurációs értékeket. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md#resource-groups) (Az Azure Resource Manager áttekintése).
7. Ezután válassza ki a hello **hely** a VNet beállításait. hello hely határozza meg, központi telepítését toothis VNet hello erőforrások tároló.
8. Válassza ki **PIN-kód toodashboard** Ha szeretné, hogy a virtuális hálózat egyszerűen a hello irányítópulton toobe képes toofind, és kattintson a **létrehozása**.

  ![PIN-kód toodashboard](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Hozzon létre kattintást követően egy csempe megjelenik az irányítópulton, amely hello a VNet állapotát mutatja. hello csempe változik hello virtuális hálózat létrehozása folyamatban van.

  ![Virtuális hálózat csempéjének létrehozása](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. A virtuális hálózat létrehozása után megjelenik **Created** tartozó **állapot** hello hálózatok lapján hello a klasszikus Azure portálon.
11. Adjon meg egy DNS-kiszolgálót (nem kötelező). A virtuális hálózat létrehozása után hozzáadhat hello IP-címet a DNS-kiszolgáló a névfeloldáshoz. hello DNS kiszolgáló IP-cím megadott hello-egy DNS-kiszolgáló, amely képes névfeloldásra hello hello erőforrások a Vneten belül kell lennie.<br>tooadd egy DNS-kiszolgálót, nyissa meg a virtuális hálózat hello beállításait, kattintson a DNS-kiszolgálókat, és hello IP-cím, amelyet az toouse hello DNS-kiszolgáló hozzáadása.

### <a name="gateway"></a>2. rész: Átjáróalhálózat és dinamikus útválasztású átjáró létrehozása

Ebben a lépésben létrehoz egy átjáró-alhálózatot és egy dinamikus útválasztású átjárót. Hello Azure-portál a hello klasszikus üzembe helyezési modellel, az átjáró alhálózatának hello és hello átjáró létrehozása végezhető el hello ugyanaz a konfigurációs lapok.

1. Hello portálon lépjen toohello virtuális hálózati átjáró toocreate kívánt.
2. A virtuális hálózat hello hello oldalon **áttekintése** hello VPN-kapcsolatok területen kattintson **átjáró**.

  ![Kattintson az átjáró toocreate](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. A hello **új VPN-kapcsolat** lapon jelölje be **pont-pont**.

  ![Pont–hely kapcsolat típusa](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. A **ügyfél Címterület**, hello IP-címtartomány hozzáadása. Ez az hello tartománya, amelyből a hello VPN-ügyfelek IP-címet kap, kapcsolódás esetén. Használjon egy privát IP-címtartományt, amely nem fedi át hello helyszíni hely, amely a kapcsolódni fog, vagy hello tooconnect a kívánt virtuális hálózat. Hello automatikusan kitöltött tartomány törlése, majd adja hozzá a hello privát IP-címtartományt, amelyet az toouse.

  ![Ügyfélcímtér](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Jelölje be hello **átjáró létrehozása azonnal** jelölőnégyzetet.

  ![Átjáró azonnali létrehozása](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Kattintson a **választható átjáró konfigurációs** tooopen hello **átjáró konfigurációs** lap.

  ![Kattintson az Optional gateway configuration (Átjáró opcionális konfigurálása) lehetőségre](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Kattintson a **alhálózati konfigurálása kötelező beállítások** tooadd hello **átjáróalhálózatot**. Habár lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy hozzon létre egy nagyobb alhálózat több címet legalább /28 vagy /27 kiválasztásával. Ez lehetővé teszi elég címek tooaccommodate lehetséges olyan beállításokat, amelyeket érdemes lehet a hello jövőbeli. Az átjáró alhálózatok használatakor ne társítása egy hálózati biztonsági csoport (NSG) toohello átjáró-alhálózatot. A hálózati biztonsági csoport toothis alhálózati társítása, előfordulhat, hogy a VPN-átjáró toostop, az elvárt módon működik.

  ![Átjáró-alhálózat hozzáadása](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Jelölje be hello átjáró **mérete**. hello mérete hello gateway SKU a virtuális hálózati átjáró. Hello portálon hello alapértelmezett termékváltozata **alapvető**. További információk a Gateway termékváltozatokról: [Tudnivalók a VPN Gateway beállításairól](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

  ![Az átjáró mérete](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Jelölje be hello **útválasztási típus** az átjáró. A pont–hely konfigurációhoz a **Dynamic** (Dinamikus) útválasztási típusra van szükség. Miután befejezte a lap konfigurálását, kattintson az **OK** gombra.

  ![Az útválasztási típus konfigurálása](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. A hello **új VPN-kapcsolat** kattintson **OK** hello lap toobegin a virtuális hálózati átjáró létrehozása hello alján. VPN-átjáró too45 perc toocomplete, attól függően, hogy a kiválasztott hello átjáró-termékváltozat is eltarthat.

## <a name="generatecerts"></a>2. Tanúsítványok létrehozása

Tanúsítványok pont-pont VPN Azure tooauthenticate VPN-ügyfelek által használt. Hello nyilvánoskulcs-adatokat a hello legfelső szintű tanúsítvány tooAzure feltöltése. nyilvános kulcs hello majd minősül "megbízható". Ügyféltanúsítványok kell kell hello megbízható legfelső szintű tanúsítvány jön létre, és csak utána települ a hello tanúsítványok-aktuális felhasználó/személyes tanúsítványtárolóba minden egyes ügyfélszámítógépre. hello tanúsítvány használt tooauthenticate hello ügyfél, amikor kezdeményezik a kapcsolat toohello virtuális hálózat. 

Önaláírt tanúsítványok használata esetén azokat megadott paraméterekkel kell létrehozni. Létrehozhat egy önaláírt tanúsítványt a hello utasításokat követve [PowerShell és Windows 10](vpn-gateway-certificates-point-to-site.md), vagy [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Fontos, hogy ha önaláírt legfelső szintű tanúsítványok használata, és az ügyféltanúsítványok generálása hello önaláírt legfelső szintű tanúsítvány ezeket az utasításokat a hello lépések végrehajtásával. Ellenkező esetben hello létrehozott tanúsítványok nem lesz kompatibilis a P2S-kapcsolatok, és hibaüzenet jelenik meg a kapcsolat.

### <a name="cer"></a>1. rész: Hello nyilvános kulcsát (.cer) hello legfelső szintű tanúsítvány beszerzése

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="genclientcert"></a>2. rész: Ügyféltanúsítvány létrehozása

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="upload"></a>3. Hello legfelső szintű tanúsítvány .cer-fájl feltöltése

Hello átjáró létrehozása után feltöltheti hello .cer fájlját (hello nyilvánoskulcs-adatokat tartalmazó) a megbízható legfelső szintű tanúsítvány tooAzure. Nem töltse fel hello legfelső szintű tanúsítvány tooAzure hello titkos kulcsát. A.cer fájl a feltöltést követően Azure használható tooauthenticate ügyfelek, amelyek telepítették a hello megbízható legfelső szintű tanúsítvány által létrehozott ügyféltanúsítványt. Szükség esetén további megbízható legfelső szintű tanúsítvány fájlok - tooa összesen 20 - később fel feltölthet.  

1. A hello **VPN-kapcsolatok** szakasz a Vnethez tartozó hello lap, kattintson a hello **ügyfelek** grafikus tooopen hello **pont-pont VPN kapcsolatot** lap.

  ![Ügyfelek](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. A hello **pont – hely kapcsolat** kattintson **tanúsítványok kezelése** tooopen hello **tanúsítványok** lap.<br>

  ![Tanúsítványok lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. A hello **tanúsítványok** kattintson **feltöltése** tooopen hello **feltöltés tanúsítvány** lap.<br>

    ![Tanúsítványok feltöltése lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Kattintson a hello mappa grafikus toobrowse hello .cer fájl. Hello fájlt, majd kattintson **OK**. Frissítési hello lap toosee hello feltöltött tanúsítvány hello **tanúsítványok** lap.

  ![Tanúsítvány feltöltése](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>4. Hello ügyfél konfigurálása

tooconnect tooa egy pont – hely VPN hálózatok, minden ügyfél telepíteni kell a csomag tooconfigure hello natív Windows VPN-ügyfél. hello konfigurációs csomag hello natív Windows VPN-ügyfél hello beállítások szükséges tooconnect toohello virtuális hálózaton konfigurálja.

Minden egyes ügyfélszámítógépre csomag azonos VPN-ügyfél konfigurációja hello mindaddig, amíg hello verzióegyezéseket hello architektúra hello ügyfél használhatja. Ügyfél által támogatott operációs rendszerek hello listájáért lásd: hello [pont – hely kapcsolatok gyakran ismételt kérdések](#faq) hello Ez a cikk végén.

### <a name="generateconfigpackage"></a>1. rész: Készítése és hello VPN-ügyfélcsomag konfigurációs telepítése

1. Az Azure portálon hello hello **áttekintése** lapját a Vnethez tartozó **VPN-kapcsolatok**, hello ügyfél grafikus tooopen hello kattintson **pont-pont VPN kapcsolat** lap.
2. Hello hello tetején **pont-pont virtuális Magánhálózati kapcsolat** lapján kattintson a hello letöltőcsomag megfelelő toohello-ügyfél operációs rendszerét, amely akkor lesz telepítve:

  * 64 bites ügyfelek esetén válassza a **VPN Client (64-bit)** (VPN-ügyfél (64 bit)) lehetőséget.
  * 32 bites ügyfelek esetén válassza a **VPN Client (32-bit)** (VPN-ügyfél (32 bit)) lehetőséget.

  ![A VPN-ügyfél konfigurációs csomagjának letöltése](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Csomagolt hello állít elő, ha töltse le és telepítse az ügyfélszámítógépen. Ha megjelenik a SmartScreen egy előugró ablaka, kattintson a **További információ**, majd a **Futtatás mindenképpen** elemre. Egyéb ügyfélszámítógépeire hello csomag tooinstall is mentheti.

### <a name="installclientcert"></a>2. lépés: Telepítse hello ügyféltanúsítványt

Ha azt szeretné, hogy egy P2S toocreate kapcsolat eltérő hello ügyfélszámítógépről egy használt toogenerate hello ügyféltanúsítványokat, tooinstall ügyféltanúsítvány szükséges. Ügyfél-tanúsítvány telepítése, úgy kell hello jelszó hello ügyfél tanúsítvány exportálása során létrejött. Ez általában csak egy függetlenül attól, hogy duplán hello tanúsítványt, és telepíti azt. További információkért lásd az [exportált ügyféltanúsítványok telepítését](vpn-gateway-certificates-point-to-site.md#install) ismertető cikket.

## <a name="connect"></a>5. Csatlakozás tooAzure

### <a name="connect-tooyour-vnet"></a>Csatlakozás a virtuális hálózat tooyour

1. tooconnect tooyour VNet hello ügyfélszámítógépen nyissa meg a tooVPN kapcsolatok, és keresse meg a létrehozott hello VPN-kapcsolatot. Hello azonos nevet a virtuális hálózatnak nevezik. Kattintson a **Connect** (Csatlakozás) gombra. Előugró üzenet jelenhet meg, hogy toousing hello tanúsítvány hivatkozik. Ha ez történik, kattintson a **Folytatás** toouse emelt szintű jogosultságokkal.
2. A hello **kapcsolat** állapotlapon, kattintson a **Connect** toostart hello kapcsolat. Ha megjelenik egy **tanúsítvány kiválasztása** képernyőn, győződjön meg arról, hogy hello ügyfél tanúsítvány ábrázoló, amelyet az toouse tooconnect egy hello. Ha nem, hello nyílra tooselect hello megfelelő tanúsítványt használjon, és kattintson a **OK**.

  ![VPN-ügyfélkapcsolat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. A kapcsolat létrejött.

  ![Létrehozott kapcsolat](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Pont–hely kapcsolatok hibaelhárítása

[!INCLUDE [verify-client-certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

### <a name="verifyvpnconnect"></a>Hello VPN-kapcsolat ellenőrzése

1. tooverify, hogy a VPN-kapcsolatot az aktív, nyisson meg egy rendszergazda jogú parancssort, és futtassa *ipconfig/all*.
2. Hello eredményeinek megtekintése. Láthatja, hogy kapott hello IP-címet a virtuális hálózat létrehozásakor adott hello pont – hely kapcsolat címtartományán belül hello címek egyikére. hello eredmények hasonló toothis példa kell lennie:

  ```
    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled
  ```

## <a name="connectVM"></a>Csatlakoztassa tooa virtuális gépet

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-p2s-classic-include.md)]

## <a name="add"></a>Megbízható főtanúsítványok hozzáadása vagy eltávolítása

A megbízható főtanúsítványokat felveheti vagy el is távolíthatja az Azure-ban. Ha eltávolít egy legfelső szintű tanúsítványt, jön létre, hogy a legfelső szintű tanúsítvánnyal rendelkező ügyfelek nem fogja tudni tooauthenticate, és így nem lesz képes tooconnect. Ha szeretné, hogy egy ügyfél tooauthenticate, és csatlakozni tud kell tooinstall (feltöltött) tooAzure megbízható legfelső szintű tanúsítványokat létre egy új ügyféltanúsítványt.

### <a name="addtrustedroot"></a>a megbízható legfelső szintű tanúsítvány tooadd

Másolatot too20 megbízható legfelső szintű tanúsítvány .cer fájlok tooAzure adhat hozzá. Útmutatásért lásd: [szakasz 3 - feltöltési hello legfelső szintű tanúsítvány .cer fájl](#upload).

### <a name="removetrustedroot"></a>a megbízható legfelső szintű tanúsítvány tooremove

1. A hello **VPN-kapcsolatok** szakasz a Vnethez tartozó hello lap, kattintson a hello **ügyfelek** grafikus tooopen hello **pont-pont VPN kapcsolatot** lap.

  ![Ügyfelek](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. A hello **pont – hely kapcsolat** kattintson **tanúsítványok kezelése** tooopen hello **tanúsítványok** lap.<br>

  ![Tanúsítványok lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. A hello **tanúsítványok** lapján kattintson a hello három pont tovább toohello tanúsítványt, hogy szeretné, hogy tooremove, majd kattintson a **törlése**.

  ![Főtanúsítvány törlése](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>

## <a name="revokeclient"></a>Ügyféltanúsítvány visszavonása

Az ügyféltanúsítványokat vissza lehet vonni. hello tanúsítvány-visszavonási lista lehetővé teszi a tooselectively visszautasítja a pont – hely kapcsolat egyedi ügyféltanúsítványok alapján. Ez a folyamat eltér a megbízható főtanúsítvány eltávolításától. Ha eltávolítja a megbízható legfelső szintű tanúsítvány .cer az Azure-ból, azt minden tanúsítványt generált/aláírt hello visszavont legfelső szintű tanúsítvány hello hozzáférés visszavonása. Ügyfél-tanúsítvány visszavonásával, ahelyett, hogy a főtanúsítvány hello, lehetővé teszi, hogy hello egyéb tanúsítványokat is létrehozott, hello legfelső szintű tanúsítvány toocontinue toobe hello pont – hely kapcsolat hitelesítéséhez használt.

hello általános gyakorlat toouse hello legfelső szintű tanúsítvány toomanage hozzáférés csapat vagy szervezet szinten egyéni felhasználók számára a minden részletre kiterjedő hozzáférés-vezérléshez visszavont ügyféltanúsítványok használata során.

### <a name="revokeclientcert"></a>toorevoke ügyféltanúsítványt

Ügyféltanúsítvány visszavonhatja hello ujjlenyomat toohello visszavont tanúsítványok listájának hozzáadásával.

1. Hello ügyfél tanúsítványának ujjlenyomata beolvasása. További információkért lásd: [hogyan: lekérése hello tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695.aspx).
2. Másolja a hello információk tooa szövegszerkesztőben, és úgy, hogy egy folyamatos karakterláncként, távolítsa el az összes szóközöket.
3. Keresse meg a toohello **"klasszikus virtuális hálózat neve" > pont – hely típusú VPN-kapcsolat > Tanúsítványok** lapon, majd kattintson **visszavont tanúsítványok listájának** tooopen hello visszavonási lista oldal. 
4. A hello **visszavont tanúsítványok listájának** kattintson **+ Hozzáadás tanúsítvány** tooopen hello **Hozzáadás toorevocation tanúsítványlista** lap.
5. A hello **Hozzáadás toorevocation tanúsítványlista** lapon, a tanúsítvány-ujjlenyomat hello beilleszthet egy folyamatos szövegsor, szóközök nélkül. Kattintson a **OK** hello lap hello alján.
6. Miután a frissítés befejezése után hello tanúsítvány már nem lehet használt tooconnect. Ügyfelek, amelyek ezzel a tanúsítvánnyal tooconnect kap üzenetet kap arról, hogy hello tanúsítvány hatályát veszti.

## <a name="faq"></a>Pont–hely kapcsolatok – gyakori kérdések

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Következő lépések
Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). További információ a hálózati és a virtuális gépek toounderstand lásd: [Azure és a Linux virtuális gép hálózati áttekintés](../virtual-machines/linux/azure-vm-network-overview.md).
