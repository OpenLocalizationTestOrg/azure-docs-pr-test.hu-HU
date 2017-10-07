---
title: "Számítógép tooa virtuális hálózatot pont-hely- és Tanúsítványalapú hitelesítés használatával: Azure portálon |} Microsoft Docs"
description: "Biztonságos kapcsolódás egy számítógép tooyour Azure virtuális hálózatot hozzon létre egy pont – hely típusú VPN gateway-kapcsolatot tanúsítvány alapú hitelesítést használ. Ez a cikk toohello Resource Manager üzembe helyezési modellben vonatkozik, és hello Azure-portált használja."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/10/2017
ms.author: cherylmc
ms.openlocfilehash: 1419d6b4c160140b62d656b25bd02f6af7fd6655
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-point-to-site-connection-tooa-vnet-using-certificate-authentication-azure-portal"></a>Egy pont – hely kapcsolat tooa virtuális hálózat konfigurálása-alapú hitelesítést használó: Azure-portálon

Ez a cikk bemutatja, hogyan toocreate egy Vnetet egy pont – hely kapcsolatot hello erőforrás-kezelő telepítési modell segítségével az hello Azure-portálon. Ez a konfiguráció tanúsítványok tooauthenticate hello csatlakozó ügyfél használja. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Pont-pont (P2S) VPN-átjáró lehetővé teszi a biztonságos kapcsolat tooyour virtuális hálózat létrehozása az egyéni ügyfél-számítógépről. Pont-pont VPN-kapcsolatok akkor hasznos, ha azt szeretné, hogy a virtuális hálózat egy távoli helyről, például amikor, amelyek dolgozzon home vagy konferencia tooconnect tooyour. P2S VPN esetén is egy hasznos megoldás toouse helyett a telephelyek közötti VPN tooconnect tooa VNet kell csak néhány ügyféllel. 

P2S használja a Secure Socket Tunneling Protocol (SSTP), amely SSL-alapú VPN-protokoll hello. P2S VPN-kapcsolatot létesít hello ügyfélszámítógépről elindításával.

![Pont–hely diagram](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Pont-pont tanúsítvány hitelesítési kapcsolatok hello következő szükséges:

* Útvonalalapú VPN-átjáró.
* hello nyilvános kulcsát (.cer-fájl) egy legfelső szintű tanúsítvány, amely feltöltött tooAzure. Hello tanúsítványt a feltöltést követően megbízható tanúsítvány minősül, és használják a hitelesítéshez.
* Hello legfelső szintű tanúsítvány által létrehozott és telepített minden egyes ügyfélszámítógépen toohello VNet csatlakozó ügyféltanúsítványt. A rendszer ezt a tanúsítványt használja ügyfélhitelesítéshez.
* A VPN-ügyfél konfigurációs csomagja. hello VPN-ügyfélcsomag konfigurációs hello ügyfél tooconnect toohello VNet hello szükséges információkat tartalmaz. hello csomag hello meglévő VPN-ügyfél, amely natív toohello Windows operációs rendszer konfigurálja. Minden ügyfél hello konfigurációs csomag használatával kell konfigurálni.

A pont–hely kapcsolatok nem igényelnek VPN-eszközt vagy helyszíni nyilvános IP-címet. hello VPN-kapcsolaton keresztül SSTP (Secure Socket Tunneling Protocol) jön létre. Hello kiszolgáló oldalán 1.0-s, 1.1-es és 1.2-es SSTP verziója támogatott. hello ügyfél úgy dönt, hogy melyik verzió toouse. Windows 8.1 és újabb kiadások esetén az SSTP alapértelmezés szerint az 1.2 verziót használja.

Pont – hely kapcsolatok kapcsolatos további információkért lásd: hello [pont-pont – gyakori kérdések](#faq) hello Ez a cikk végén.

#### <a name="example"></a>Példaértékek

A következő értékek toocreate egy tesztkörnyezetben hello használhatja, vagy tekintse meg a toothese értékek toobetter megérteni a cikkben szereplő példák hello:

* **Virtuális hálózat neve:** VNet1
* **Címtér:** 192.168.0.0/16<br>Ebben a példában csak egy címteret használunk. Azonban a virtuális hálózatához több címteret is használhat.
* **Alhálózat neve:** FrontEnd
* **Alhálózati címtartomány:** 192.168.1.0/24
* **Előfizetés:** Ha egynél több előfizetéssel, győződjön meg arról, hogy rendelkezik a megfelelő hello.
* **Erőforráscsoport:** TestRG
* **Hely:** az USA keleti régiója
* **Átjáró-alhálózat:** 192.168.200.0/24<br>
* **DNS-kiszolgáló:** toouse kívánt névfeloldás hello DNS-kiszolgáló IP-címe (nem kötelező).
* **Virtuális hálózati átjáró neve:** VNet1GW
* **Átjáró típusa:** VPN
* **VPN típusa:** útvonalalapú
* **Nyilvános IP-cím neve:** VNet1GWpip
* **Kapcsolat típusa:** pont–hely
* **Ügyfélcímkészlet:** 172.16.201.0/24<br>A VPN-ügyfelek toohello VNet a pont-pont kapcsolattal csatlakozó fogadása hello ügyfélcímkészlete IP-címet.

## <a name="createvnet"></a>1. Virtuális hálózat létrehozása

Mielőtt elkezdi végrehajtani a lépéseket, győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Átjáróalhálózat hozzáadása

A virtuális hálózati tooa átjáró csatlakozás előtt először toocreate hello átjáróalhálózatot hello virtuális hálózati toowhich tooconnect keresi. hello átjáró szolgáltatás hello átjáróalhálózatot megadott hello IP-címeket használ. Ha lehetséges, hozzon létre egy átjáró-alhálózatot CIDR-blokkja /28 vagy /27 használatával tooprovide elegendő IP-címek tooaccommodate jövőbeli további konfigurációs követelményeket.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. DNS-kiszolgáló megadása (nem kötelező)

Miután létrehozta a virtuális hálózat, a DNS-kiszolgáló toohandle névfeloldás hello IP-címét is hozzáadhat. hello DNS-kiszolgáló esetén ez a konfiguráció nem kötelező, de kötelező, ha a névfeloldás. Az érték megadásával nem jön létre új DNS-kiszolgáló. hello DNS kiszolgáló IP-cím megadott kell egy DNS-kiszolgáló, amely képes névfeloldásra hello hello erőforrásokhoz való kapcsolódás esetén. Ebben a példában a magánhálózati IP-cím használtuk, de valószínű, hogy ez nem hello IP-címet a DNS-kiszolgáló. Lehet, hogy toouse a saját értékeit.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Virtuális hálózati átjáró létrehozása

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Tanúsítványok előállítása

Tooa VNet egy pont – hely típusú VPN-kapcsolaton keresztül csatlakozó Azure tooauthenticate ügyfelek által használt tanúsítványok. Miután beszerezte a legfelső szintű tanúsítványt, [feltöltése](#uploadfile) hello tooAzure nyilvánoskulcs-adatokat. hello legfelső szintű tanúsítvány majd tekinthető "megbízható" az Azure-kapcsolat P2S toohello virtuális hálózaton keresztül. Ügyféltanúsítványok is generálása hello megbízható legfelső szintű tanúsítvánnyal, majd azok telepítése minden egyes ügyfélszámítógépre. hello ügyféltanúsítvány használt tooauthenticate hello ügyfél, amikor kezdeményezik a kapcsolat toohello virtuális hálózat. 

### <a name="getcer"></a>1. Hello .cer fájl hello legfelső szintű tanúsítvány beszerzése

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Ügyféltanúsítvány létrehozása

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Hello ügyfélcímkészlete hozzáadása

hello ügyfélcímkészlete, amely egy privát IP-címek megadott. egy pont – hely VPN-kapcsolaton keresztül csatlakozó hello ügyfelek IP-címet kapnak az ebben a tartományban. A privát IP-címtartományt, amely nem fedi át használata hello a helyszíni helyről származó csatlakozó vagy hello tooconnect a kívánt VNet.

1. Hello virtuális hálózati átjáró létrehozása után, nyissa meg a toohello **beállítások** hello virtuális hálózati átjáró lap szakasza. A hello **beállítások** kattintson **pont-hely konfigurációs** tooopen hello **-a-webhely-konfiguráció** lap.

  ![Pont–hely lap](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. A hello **-a-webhely-konfiguráció** lapon hello automatikusan kitöltött tartomány törlése, és vegye fel a hello privát IP-címtartományt, amelyet az toouse. Kattintson a **mentése** toovalidate és hello beállítás mentése.

  ![Ügyfélcímkészlet](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Hello legfelső szintű tanúsítvány nyilvános Tanúsítványadatok feltöltése

Hello átjáró létrehozása után hello tartozó nyilvánoskulcs-adatokat hello legfelső szintű tanúsítvány tooAzure töltse fel. Hello nyilvános tanúsítványának adatait a feltöltést követően Azure használható tooauthenticate ügyfelek, amelyek telepítették a hello megbízható legfelső szintű tanúsítvány által létrehozott ügyféltanúsítványt. További megbízható legfelső szintű tanúsítványok felfelé tooa összesen 20 feltölthet.

1. Hozzáadja a tanúsítványokat a hello **pont-hely konfigurációs** hello lap **legfelső szintű tanúsítvány** szakasz.  
2. Győződjön meg arról, hogy egy Base-64 kódolású X.509 (.cer) fájl, exportálva hello legfelső szintű tanúsítvány. Tanúsítványra van szüksége tooexport hello ebben a formátumban, hello tanúsítvány szövegszerkesztőben tekinthetők meg.
3. Nyisson meg egy szövegszerkesztőt, például a Jegyzettömbben hello tanúsítvány. Ha hello Tanúsítványadatok másol, győződjön meg arról, hogy egy folyamatos sorba kocsivissza és soremelés nélkül hello szöveg másolása. Szükség lehet toomodify a nézeten belül hello text editor too'Show szimbólum/megjelenítése összes karakter toosee hello kocsivissza értéket ad vissza, és hírcsatornák sor. Másolja a következő szakasz egy folyamatos sorba csak hello:

  ![Tanúsítványadatok](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Hello Tanúsítványadatok beillesztése hello **nyilvános Tanúsítványadatok** mező. **Név** hello tanúsítványt, és kattintson a **mentése**. Másolatot too20 megbízható legfelső szintű tanúsítványok is hozzáadhat.

  ![Tanúsítvány feltöltése](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>8. Létrehozni és telepíteni hello VPN-ügyfélcsomag konfigurációs

tooconnect tooa egy pont – hely VPN hálózatok, minden ügyfél telepítenie kell egy konfigurációs ügyfélcsomagot, amely hello beállításokkal konfigurálja a hello natív VPN-ügyfél és a szükséges tooconnect toohello virtuális hálózati fájlokat. hello VPN-ügyfélcsomag konfigurációs hello natív Windows VPN-ügyfél konfigurálja, egy másik VPN-ügyfél nem telepít.

Minden egyes ügyfélszámítógépre csomag azonos VPN-ügyfél konfigurációja hello mindaddig, amíg hello verzióegyezéseket hello architektúra hello ügyfél használhatja. Ügyfél által támogatott operációs rendszerek hello listájáért lásd: hello [pont – hely kapcsolatok gyakran ismételt kérdések](#faq) hello Ez a cikk végén.

### <a name="step-1---generate-and-download-hello-client-configuration-package"></a>1. lépés – készítése és hello ügyfél konfigurációs csomag

1. A hello **pont-hely konfigurációs** kattintson **letöltése VPN-ügyfél** tooopen hello **letöltése VPN-ügyfél** lap. Egy-két hello csomag toogenerate a percet vesz igénybe.

  ![VPN-ügyfél letöltése, 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Válassza ki a hello helyes csomagot az ügyfél számára, és kattintson **letöltése**. Hello konfigurációs csomag fájl mentéséhez. Minden olyan ügyfélszámítógépen, amely a virtuális hálózati toohello hello VPN-ügyfélcsomag konfigurációs telepítése.

  ![VPN-ügyfél letöltése, 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-hello-client-configuration-package"></a>2. lépés – telepítés hello konfigurációs ügyfélcsomag

1. Hello konfigurációs fájl másolása helyi toohello számítógép, amelyet az tooconnect tooyour virtuális hálózat. 
2. Kattintson duplán a hello .exe fájl tooinstall hello csomag hello ügyfélszámítógépen. Hello konfigurációs csomagot hozta létre, mert nincs aláírva, és megjelenik egy figyelmeztetés. Ha a Windows SmartScreen előugró ablak, kattintson a **információ** (a hello balra), majd **mégis futtatni** tooinstall hello csomag.
3. Hello telepítéséhez hello ügyfélszámítógépen. Ha a Windows SmartScreen előugró ablak, kattintson a **információ** (a hello balra), majd **mégis futtatni** tooinstall hello csomag.
4. Hello ügyfélszámítógépen nyissa meg túl**hálózati beállítások** kattintson **VPN**. VPN-kapcsolat hello hello csatlakozó virtuális hálózati hello nevét jeleníti meg.

## <a name="installclientcert"></a>9. Exportált ügyféltanúsítvány telepítése

Ha azt szeretné, hogy egy P2S toocreate kapcsolat eltérő hello ügyfélszámítógépről egy használt toogenerate hello ügyféltanúsítványokat, tooinstall ügyféltanúsítvány szükséges. Ügyfél-tanúsítvány telepítése, úgy kell hello jelszó hello ügyfél tanúsítvány exportálása során létrejött. Ez általában csak egy függetlenül attól, hogy duplán hello tanúsítványt, és telepíti azt.

Ellenőrizze, hogy hello ügyféltanúsítvány egy .pfx együtt hello teljes láncát (amely hello alapértelmezett) típusúként lett exportálva. Ellenkező esetben hello legfelső szintű tanúsítvány adatait nincs jelen hello ügyfélszámítógépen, és hello ügyfél megfelelően nem fogja tudni tooauthenticate. További információkért lásd az [exportált ügyféltanúsítványok telepítését](vpn-gateway-certificates-point-to-site.md#install) ismertető cikket.

## <a name="connect"></a>10. Csatlakozás tooAzure

1. tooconnect tooyour VNet hello ügyfélszámítógépen nyissa meg a tooVPN kapcsolatok, és keresse meg a létrehozott hello VPN-kapcsolatot. Hello azonos nevet a virtuális hálózatnak nevezik. Kattintson a **Connect** (Csatlakozás) gombra. Előugró üzenet jelenhet meg, hogy toousing hello tanúsítvány hivatkozik. Kattintson a **Folytatás** toouse emelt szintű jogosultságokkal.

2. A hello **kapcsolat** állapotlapon, kattintson a **Connect** toostart hello kapcsolat. Ha megjelenik egy **tanúsítvány kiválasztása** képernyőn, győződjön meg arról, hogy hello ügyfél tanúsítvány ábrázoló, amelyet az toouse tooconnect egy hello. Ha nem, hello nyílra tooselect hello megfelelő tanúsítványt használjon, és kattintson a **OK**.

  ![VPN-ügyfél kapcsolódik tooAzure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. A kapcsolat létrejött.

  ![A kapcsolat létrejött](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Pont–hely kapcsolatok hibaelhárítása

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>11. A kapcsolat ellenőrzése

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

## <a name="add"></a>Megbízható főtanúsítványok hozzáadása vagy eltávolítása

A megbízható főtanúsítványokat felveheti vagy el is távolíthatja az Azure-ban. Ha eltávolít egy legfelső szintű tanúsítványt, jön létre, hogy a legfelső szintű tanúsítvánnyal rendelkező ügyfelek nem fogja tudni tooauthenticate, és így nem lesz képes tooconnect. Ha szeretné, hogy egy ügyfél tooauthenticate, és csatlakozni tud kell tooinstall (feltöltött) tooAzure megbízható legfelső szintű tanúsítványokat létre egy új ügyféltanúsítványt.

### <a name="tooadd-a-trusted-root-certificate"></a>a megbízható legfelső szintű tanúsítvány tooadd

Másolatot too20 megbízható legfelső szintű tanúsítvány .cer fájlok tooAzure adhat hozzá. Útmutatásért lásd: hello szakasz [egy megbízható legfelső szintű tanúsítvány feltöltése](#uploadfile) ebben a cikkben.

### <a name="tooremove-a-trusted-root-certificate"></a>a megbízható legfelső szintű tanúsítvány tooremove

1. tooremove egy megbízható legfelső szintű tanúsítványt, nyissa meg a toohello **pont-hely konfigurációs** a virtuális hálózati átjáró lap.
2. A hello **legfelső szintű tanúsítvány** szakasz hello lap, keresse meg a megjeleníteni kívánt tooremove hello tanúsítványt.
3. Toohello tanúsítvány tovább hello három pont gombra, és kattintson az "Eltávolítás".

## <a name="revokeclient"></a>Ügyféltanúsítvány visszavonása

Az ügyféltanúsítványokat vissza lehet vonni. hello tanúsítvány-visszavonási lista lehetővé teszi a tooselectively visszautasítja a pont – hely kapcsolat egyedi ügyféltanúsítványok alapján. Ez a folyamat eltér a megbízható főtanúsítvány eltávolításától. Ha eltávolítja a megbízható legfelső szintű tanúsítvány .cer az Azure-ból, azt minden tanúsítványt generált/aláírt hello visszavont legfelső szintű tanúsítvány hello hozzáférés visszavonása. Ügyfél-tanúsítvány visszavonásával, ahelyett, hogy a főtanúsítvány hello, lehetővé teszi, hogy hello más is létrehozott, hello legfelső szintű tanúsítvány toocontinue toobe hitelesítéshez használt tanúsítványok.

hello általános gyakorlat toouse hello legfelső szintű tanúsítvány toomanage hozzáférés csapat vagy szervezet szinten egyéni felhasználók számára a minden részletre kiterjedő hozzáférés-vezérléshez visszavont ügyféltanúsítványok használata során.

### <a name="toorevoke-a-client-certificate"></a>toorevoke ügyféltanúsítványt

Ügyféltanúsítvány visszavonhatja hello ujjlenyomat toohello visszavont tanúsítványok listájának hozzáadásával.

1. Hello ügyfél tanúsítványának ujjlenyomata beolvasása. További információkért lásd: [hogyan tooretrieve hello tanúsítvány ujjlenyomata](https://msdn.microsoft.com/library/ms734695.aspx).
2. Másolja a hello információk tooa szövegszerkesztőben, és úgy, hogy egy folyamatos karakterláncként, távolítsa el az összes szóközöket.
3. Keresse meg a virtuális hálózati átjáró toohello **-a-webhely-konfiguráció** lap. Ez a hello ugyanazon az oldalon túl használt[egy megbízható legfelső szintű tanúsítvány feltöltése](#uploadfile).
4. A hello **visszavont tanúsítványai** területen adjon meg egy rövid nevet a hello tanúsítvány (toobe hello tanúsítvány neve nincs beállítva).
5. Másolja és illessze be a hello ujjlenyomat karakterlánc toohello **ujjlenyomat** mező.
6. hello ujjlenyomat érvényesíti, és automatikusan fel lesz véve toohello visszavont tanúsítványok listáját. Hello képernyőn megjelenik egy üzenet, hogy hello frissíti a listán. 
7. Miután a frissítés befejezése után hello tanúsítvány már nem lehet használt tooconnect. Ügyfelek, amelyek ezzel a tanúsítvánnyal tooconnect kap üzenetet kap arról, hogy hello tanúsítvány hatályát veszti.

## <a name="faq"></a>Pont–hely kapcsolatok – gyakori kérdések

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Következő lépések
Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). További információ a hálózati és a virtuális gépek toounderstand lásd: [Azure és a Linux virtuális gép hálózati áttekintés](../virtual-machines/linux/azure-vm-network-overview.md).
