---
title: "Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózat: telephelyek közötti VPN (klasszikus): portál |} Microsoft Docs"
description: "Az IPsec-kapcsolat a helyszíni hálózati tooan Azure-beli virtuális hálózat felett lépéseket toocreate hello nyilvános internethez. A lépések segítségével hozhat létre egy hello portál használatával létesítmények közötti pont-pont VPN Gateway-kapcsolatot."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/010/2017
ms.author: cherylmc
ms.openlocfilehash: b260bdf610b264458660b278bd32bf0fc5b519ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-using-hello-azure-portal-classic"></a>Pont-pont kapcsolatot hello (klasszikus) Azure-portál használatával

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Ez a cikk bemutatja, hogyan toouse hello Azure portál toocreate a helyszíni hálózati toohello VNet a pont-pont VPN gateway-kapcsolattal. a cikkben ismertetett hello toohello klasszikus üzembe helyezési modellel alkalmazni. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Parancssori felület](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Pont-pont VPN gateway-kapcsolattal használt tooconnect a helyszíni hálózati tooan Azure-beli virtuális hálózat (IKEv1 vagy IKEv2) IPsec/IKE VPN-alagúton keresztül. Ilyen típusú kapcsolat egy VPN található helyszíni Eszközkezelési, amely rendelkezik egy külsőleg irányuló nyilvános IP-cím tooit igényel. További információk a VPN-átjárókról: [Információk a VPN Gatewayről](vpn-gateway-about-vpngateways.md).

![Helyek közötti VPN Gateway létesítmények közötti kapcsolathoz – diagram](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Előkészületek

Győződjön meg arról, hogy teljesül-e az hello feltételek konfigurációs megkezdése előtt a következő:

* Győződjön meg arról, hogy szeretné-e toowork hello klasszikus üzembe helyezési modellben. Ha azt szeretné, hogy toowork hello Resource Manager üzembe helyezési modellel, lásd: [pont-pont kapcsolatot (erőforrás-kezelő)](vpn-gateway-howto-site-to-site-resource-manager-portal.md). Ha lehetséges, ajánlott hello Resource Manager üzembe helyezési modellben.
* Győződjön meg arról, hogy egy kompatibilis VPN-eszköz és a rendszer képes tooconfigure azt. További információk a kompatibilis VPN-eszközökről és az eszközkonfigurációról: [Tudnivalók a VPN-eszközökről](vpn-gateway-about-vpn-devices.md).
* Győződjön meg arról, hogy rendelkezik egy kifelé irányuló, nyilvános IPv4-címmel a VPN-eszköz számára. Ez az IP-cím nem lehet NAT mögötti.
* Ha nem ismeri a hello IP-címtartományok található, a helyszíni hálózati konfiguráció, a szükséges toocoordinate ezeket az információkat is tartalmazza, aki. Ez a konfiguráció létrehozásakor meg kell adnia hello IP-címtartomány címelőtagokat, hogy Azure tooyour helyszíni hely fogja átirányítani. A helyszíni hálózat alhálózatainak hello egyike is lap tooconnect a kívánt hello virtuális hálózati alhálózatokon keresztül.
* PowerShell jelenleg szükséges toospecify hello megosztott kulcsot, és hello VPN gateway-kapcsolatot létrehozni. Hello hello Azure Service Management (SM) PowerShell-parancsmagok legújabb verziójának telepítéséhez. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Ahhoz, hogy ezt a konfigurációt elvégezhesse, a PowerShellt rendszergazdaként kell futtatnia. 

### <a name="values"></a>Konfigurációs mintaértékek ehhez a gyakorlathoz

a cikkben szereplő példák hello hello a következő értékeket használja. Ezen értékek toocreate egy tesztkörnyezetben használhatja, vagy tekintse meg a toothem toobetter hello jelen cikk példái a megismeréséhez.

* **Virtuális hálózat neve:** TestVNet1
* **Címtér:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (nem kötelező ehhez a gyakorlathoz)
* **Alhálózatok:**
  * Előtér: 10.11.0.0/24
  * Háttér: 10.12.0.0/24 (nem kötelező ehhez a gyakorlathoz)
* **Átjáróalhálózat:** 10.11.255.0/27
* **Erőforráscsoport:** TestRG1
* **Hely:** az USA keleti régiója
* **DNS-kiszolgáló:** 10.11.0.3 (nem kötelező ehhez a gyakorlathoz)
* **Helyi hely neve:** Site2
* **Ügyfél-címtartomány:** hello címterületek használatát, amelyek a helyszíni-webhelyen található.

## <a name="CreatVNet"></a>1. Virtuális hálózat létrehozása

A virtuális hálózati toouse S2S kapcsolat létrehozásakor kell, hogy a megadott hello címterek nem átfedésben a hello ügyfél-címterét hello helyi helyekhez való tooconnect egyik toomake. Egymást átfedő alhálózatok esetén a kapcsolat nem fog megfelelően működni.

* Ha már rendelkezik egy Vnetet, győződjön meg arról, hogy a VPN-átjáró kialakítás kompatibilisek legyenek-e hello-beállítások. Különösen figyeljen az egyéb hálózatokkal átfedhetik tooany alhálózatokat. 

* Ha még nem rendelkezik virtuális hálózattal, akkor hozzon létre egyet. A képernyőképek csak példaként szolgálnak. Lehet, hogy tooreplace hello értékeket a saját.

### <a name="toocreate-a-virtual-network"></a>a virtuális hálózati toocreate

1. Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és szükség esetén jelentkezzen be az Azure-fiókjával.
2. Kattintson az **+** elemre. A hello **hello a piactéren** mezőbe írja be a "Virtuális hálózat". Keresse meg **virtuális hálózati** hello adott vissza a listán, és kattintson a tooopen hello **virtuális hálózati** lap.

  ![Virtuális hálózat keresése lap](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Hello virtuális hálózat lap, a hello hello alján közelében **telepítési modell kiválasztása** legördülő listából válassza ki **klasszikus**, és kattintson a **létrehozása**.

  ![Üzemi modell kiválasztása](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. A hello **hozzon létre virtuális network(classic)** lapján hello hálózatok beállításainak konfigurálása. Ezen a lapon adhatja hozzá az első címterét és egy önálló alhálózati címtartományt. Hello VNet létrehozása után visszaléphet, és adja hozzá a további alhálózatokat és címtereket.

  ![Virtuális hálózat létrehozása lap](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Virtuális hálózat létrehozása lap")
5. Győződjön meg arról, hogy hello **előfizetés** hello, helyes-e egy. Előfizetések hello legördülő lista használatával módosíthatja.
6. Kattintson az **Erőforráscsoport** elemre, és válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat egy név beírásával. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md#resource-groups) (Az Azure Resource Manager áttekintése).
7. Ezután válassza ki a hello **hely** a VNet beállításait. hello hely határozza meg, központi telepítését toothis VNet hello erőforrások tároló.
8. Ha toobe képes toofind egyszerűen a hello irányítópulton a Vnetet, jelölje be **PIN-kód toodashboard**. Kattintson a **létrehozása** toocreate a virtuális hálózat.

  ![PIN-kód toodashboard](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "PIN-kód toodashboard")
9. Miután rákattintott a "Létrehozás", egy csempe meg nem jelenik hello irányítópult lefedő hello a VNet állapotát mutatja. hello csempe változik hello virtuális hálózat létrehozása folyamatban van.

  ![Virtuális hálózat létrehozása csempe](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Virtuális hálózat létrehozása")

A virtuális hálózat létrehozása után megjelenik **Created** tartozó **állapot** hello hálózatok lapján hello a klasszikus Azure portálon.

## <a name="additionaladdress"></a>2. További címterek felvétele

Miután létrehozta a virtuális hálózatot, hozzáadhat további címtereket. További címtartományok hozzáadása része nem szükséges egy S2S konfigurációs, de ha több címterek van szüksége, hello a következő lépéseket:

1. Keresse meg a virtuális hálózatok hello hello portálon.
2. A virtuális hálózat, a hello hello oldalon **beállítások** területén kattintson **Címtéren**.
3. Kattintson hello cím terület lap **+ Hozzáadás** , és írja be a további címtartományt.

## <a name="dns"></a>3. DNS-kiszolgáló megadása

A DNS-beállítások megadása nem kötelező lépése az S2S-konfigurációnak, de a névfeloldáshoz szükség van DNS-re. Az érték megadásával nem jön létre új DNS-kiszolgáló. hello DNS kiszolgáló IP-cím megadott kell egy DNS-kiszolgáló, amely képes névfeloldásra hello hello erőforrásokhoz való kapcsolódás esetén. Hello például beállítások egy magánhálózati IP-cím használtuk. használjuk hello IP-cím nincs valószínűleg hello IP-címet a DNS-kiszolgáló. Lehet, hogy toouse a saját értékeit.

Miután létrehozta a virtuális hálózat, a DNS-kiszolgáló toohandle névfeloldás hello IP-címét is hozzáadhat. Nyissa meg a virtuális hálózat hello beállításait, majd a DNS-kiszolgálók hello IP-cím, amelyet az toouse névfeloldás hello DNS-kiszolgáló hozzáadása.

1. Keresse meg a virtuális hálózatok hello hello portálon.
2. A virtuális hálózat, a hello hello oldalon **beállítások** kattintson **DNS-kiszolgálók**.
3. Adjon meg egy DNS-kiszolgálót.
4. toosave a beállításokat, kattintson a **mentése** hello oldal hello tetején.

## <a name="localsite"></a>4. Hello helyi webhely konfigurálása

hello helyi webhely általában tooyour helyszíni helyre hivatkozik. Létre fog hozni egy kapcsolat, illetve hello IP-címtartományok hello VPN gateway toohello VPN-eszközön keresztül történik hello VPN-eszköz toowhich hello IP-címét tartalmazza.

1. Hello portálon lépjen toohello virtuális hálózati átjáró toocreate kívánt.
2. A virtuális hálózat hello hello oldalon **áttekintése** hello VPN-kapcsolatok területen kattintson **átjáró** tooopen hello **új VPN-kapcsolat** lap.

  ![Kattintson az átjáró beállításainak tooconfigure](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "tooconfigure átjáró-beállítások gombra")
3. A hello **új VPN-kapcsolat** lapon jelölje be **pont-pont**.
4. Kattintson a **helyi hely - kötelező beállítások konfigurálása** tooopen hello **helyi** lap. Hello-beállítások konfigurálása, és kattintson a **OK** toosave hello beállításait.
  - **Name:** hozzon létre egy nevet a helyi webhelyhez toomake megkönnyítik meg tooidentify.
  - **VPN-átjáró IP-címe:** Ez az hello VPN-eszköz a helyszíni hálózat hello nyilvános IP-címét. hello VPN-eszköz nyilvános IP-cím IPv4 szükséges. Adjon meg egy érvényes nyilvános IP-címet hello tooconnect kívánt VPN-eszköz toowhich. Nem lehet NAT mögött, és az Azure-ban elérhető toobe rendelkezik. Ha nem tudja hello IP-címe a VPN-eszköz, is mindig put helyőrző értékét (feltéve, egy érvényes nyilvános IP-cím formátuma hello van), majd később módosítható.
  - **Ügyfél-címtartomány:** lista hello IP-címtartományok, amelyet irányítása toohello helyi a helyi hálózaton keresztül ezt az átjárót. Több címtartományt is felvehet. Győződjön meg arról, hogy az itt megadott hello tartományok nincsenek átfedésben címtartományai más hálózatok, a virtuális hálózathoz csatlakozik, vagy hello címtartományai hello virtuális hálózat magát.

  ![Helyi hely](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Helyi hely konfigurálása")

## <a name="gatewaysubnet"></a>5. Átjáró alhálózati hello konfigurálása

Létre kell hoznia egy átjáróalhálózatot a VPN-átjáróhoz. hello átjáróalhálózatot hello IP-címek, amelyek hello VPN-átjáró szolgáltatásokat tartalmazza.

1. A hello **új VPN-kapcsolat** lapra, jelölje be hello jelölőnégyzet **átjáró létrehozása azonnal**. hello "választható átjáró konfiguráció" lap jelenik meg. Hello jelölőnégyzet bejelölésekor nem hello lap tooconfigure hello átjáróalhálózatot nem fog látni.

  ![Átjáró konfigurálása – Alhálózat, méret, útválasztási típus](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Átjáró konfigurálása – Alhálózat, méret, útválasztási típus")
2. tooopen hello **átjáró konfigurációs** kattintson **konfigurációs választható átjáró - alhálózat, méret és útválasztási típus**.
3. A hello **átjáró konfigurációs** kattintson **alhálózat - kötelező beállítások konfigurálása** tooopen hello **alhálózat hozzáadása** lap.

  ![Átjáró konfigurációja – átjáróalhálózat](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Átjáró konfigurációja – átjáró-alhálózat")
4. A hello **alhálózat hozzáadása** hello átjáró alhálózatának hozzáadása párbeszédpanelen. hello átjáróalhálózatot megadott hello mérete függ hello VPN-átjáró konfigurációs, amelyet az toocreate. Is lehetséges toocreate mérete /29 legyen egy átjáró-alhálózatot, azt javasoljuk, hogy /27 vagy /28. Ez nagyobb, több címet tartalmazó alhálózatot hoz létre. Egy nagyobb átjáróalhálózat használata lehetővé teszi a elegendő IP-címek tooaccommodate lehetséges jövőbeli konfigurációk.

  ![Átjáróalhálózat hozzáadása](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Átjáróalhálózat hozzáadása")

## <a name="sku"></a>6. Adja meg a hello SKU- és VPN-típus

1. Jelölje be hello átjáró **mérete**. Ez a hello átjáró használata toocreate a virtuális hálózati átjáró Termékváltozat. Hello portálon hello Termékváltozat alapértelmezett = **alapvető**. Hagyományos VPN-átjárók hello régi (örökölt) átjáró termékváltozatok használatára. Hello örökölt gateway SKU kapcsolatos további információkért lásd: [termékváltozatok (régi SKU) virtuális hálózati átjáró használata](vpn-gateway-about-skus-legacy.md).

  ![A termékváltozat és a VPN típusának kiválasztása](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "A termékváltozat és a VPN típusának kiválasztása")
2. Jelölje be hello **útválasztási típus** az átjáró. Ez más néven a hello VPN-típus. Mivel hello átjáró konvertálása nem egy típus tooanother fontos tooselect hello megfelelő átjáró típusa. A VPN-eszköz hello útválasztási típusa kompatibilisnek kell lennie. További, a VPN-típusokra vonatkozó információkért lásd [a VPN-átjárók beállításaival](vpn-gateway-about-vpn-gateway-settings.md#vpntype) foglalkozó témakört. Cikkek too'RouteBased utaló jelenhetnek meg "és"PolicyBased"VPN típusok. "Dinamikus"megfelel-e too'RouteBased", és a"Static"megfelel-e"PolicyBased".
3. Kattintson a **OK** toosave hello beállításait.
4. A hello **új VPN-kapcsolat** kattintson **OK** hello lap toobegin a virtuális hálózati átjáró létrehozása hello alján. Attól függően, hogy hello SKU választja is eltarthat, too45 perc toocreate a virtuális hálózati átjáró.

## <a name="vpndevice"></a>7. VPN-eszköz konfigurálása

Pont-pont kapcsolatok tooan a helyi hálózaton egy VPN-eszköz szükség. Ebben a lépésben a VPN-eszköz konfigurálása következik. A VPN-eszköz konfigurálásakor hello a következőkre lesz szüksége:

- Megosztott kulcs. Ez az azonos megosztott hello a telephelyek közötti VPN-kapcsolat létrehozásakor megadott kulcs. A példákban alapvető megosztott kulcsot használunk. Azt javasoljuk, hogy létrehozhat egy összetett kulcs toouse.
- hello a virtuális hálózati átjáró nyilvános IP-címét. Hello nyilvános IP-cím hello Azure-portálon, a PowerShell vagy a parancssori felület használatával tekintheti meg.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8. Hello kapcsolat létrehozása
Ebben a lépésben hello megosztott kulcsot, és hello kapcsolat létrehozásához. hello kulcsot meg kell hello ugyanazzal a kulccsal, amelyet a VPN-eszköz konfigurációjában használt.

> [!NOTE]
> Ez a lépés jelenleg nem elérhető hello Azure-portálon. Hello Azure PowerShell-parancsmagok hello szolgáltatás-felügyeleti (SM) verzióját kell használnia.
>

### <a name="step-1-connect-tooyour-azure-account"></a>1. lépés Csatlakozás Azure-fiók tooyour

1. Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon a tooyour fiók. A következő példa toohelp csatlakozás hello használata:

  ```powershell
  Add-AzureAccount
  ```
2. Hello előfizetések hello fiók ellenőrzése.

  ```powershell
  Get-AzureSubscription
  ```
3. Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello előfizetést.

  ```powershell
  Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_ID"
  ```

### <a name="step-2-set-hello-shared-key-and-create-hello-connection"></a>2. lépés Hello megosztott kulcsot, és hello kapcsolat létrehozása

Amikor olyan PowerShell és hello klasszikus üzembe helyezési modellel, néha hello nevében hello portálon erőforrások a rendszer nem hello nevek hello Azure toosee vár, amikor a PowerShell használatával. hello következő lépésekkel exportálhatja hello hálózati konfigurációs fájl tooobtain hello pontos értékek hello nevek.

1. Hozzon létre egy könyvtárat a számítógépen, és exportálhatja a hálózati konfiguráció hello toohello könyvtára. Ebben a példában a hello hálózati konfigurációs fájl nem exportált tooC:\AzureNet.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Nyissa meg a hello hálózati konfigurációs fájlt egy xml-szerkesztővel, és ellenőrizze a "LocalNetworkSite" és "VirtualNetworkSite neve" hello értékeit. Hello példa tooreflect hello értékeket kell módosítani. Ha neve szóközt tartalmaz, idézőjelekbe egyetlen hello érték.

3. Hello megosztott kulcsot, és hello kapcsolat létrehozásához. hello "-SharedKey" érték, amely Ön hozza létre, és adja meg. Hello példában használtuk "abc123", de hozhat létre (és kell) összetettebb használja. fontos dolog, hogy az itt megadott hello érték hello azonos értéket, a VPN-eszköz beállításakor megadott hello kell lennie.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
  -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
  ```
Hello kapcsolat létrehozásakor hello eredménye: **állapota: sikeres**.

## <a name="verify"></a>9. A kapcsolat ellenőrzése

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

Ha a hiba történt a kapcsolódás, lásd: hello **kapcsolatos problémák elhárítása** szakasz hello tartalomjegyzék hello bal oldali ablaktáblán.

## <a name="reset"></a>Hogyan tooreset VPN-átjáró

Az Azure VPN Gateway alaphelyzetbe állítása akkor hasznos, ha egy vagy több helyek közötti VPN-alagúton elveszíti a létesítmények közötti VPN-kapcsolatot. Ebben a helyzetben a helyszíni VPN-eszközök minden megfelelően működik-e, de nem tudta tooestablish hello Azure VPN gatewayek az IPsec-alagutak. A lépéseket lásd: [VPN Gateway alaphelyzetbe állítása](vpn-gateway-resetgw-classic.md).

## <a name="changesku"></a>Hogyan toochange egy átjáró-Termékváltozat

Hello lépések toochange egy átjáró-Termékváltozat, a következő témakörben: [egy átjáró-Termékváltozat átméretezése](vpn-gateway-about-SKUS-legacy.md).

## <a name="next-steps"></a>Következő lépések

* Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. További információkért lásd: [Virtuális gépek](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Információk a kényszerített bújtatásról: [Információk a kényszerített bújtatásról](vpn-gateway-about-forced-tunneling.md).