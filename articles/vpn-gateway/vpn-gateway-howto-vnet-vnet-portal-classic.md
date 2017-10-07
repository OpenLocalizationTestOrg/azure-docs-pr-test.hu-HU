---
title: "Kapcsolat létrehozása a Vnetek között: klasszikus: Azure-portál |} Microsoft Docs"
description: "Hogyan tooconnect Azure virtuális hálózatok együtt PowerShell használatával, és a klasszikus Azure portálon hello."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: f29c3c091d049ff96cf59f4c28ab86a100bcb5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vnet-to-vnet-connection-classic"></a>Konfigurálja a VNet – VNet-kapcsolatot (klasszikus)

[!INCLUDE [deployment models](../../includes/vpn-gateway-classic-deployment-model-include.md)]

Ez a cikk bemutatja, hogyan toocreate virtuális hálózatok közötti VPN gateway-kapcsolattal. hello virtuális hálózatok lehetnek azonos vagy különböző régiókban hello, és a hello azonos vagy különböző előfizetésekhez. a cikkben ismertetett hello toohello klasszikus telepítési modell és hello Azure-portálon vonatkoznak. Ezt a konfigurációt egy másik lehetőség kijelölésével a következő lista hello különböző központi telepítési eszköz vagy telepítési modell segítségével is létrehozhat:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [(Klasszikus) Azure Portal](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Különböző üzemi modellek összekapcsolása – Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Különböző üzemi modellek összekapcsolása – PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

![Virtuális hálózat tooVNet kapcsolati diagramja](./media/vpn-gateway-howto-vnet-vnet-portal-classic/v2vclassic.png)

## <a name="about-vnet-to-vnet-connections"></a>Tudnivalók a virtuális hálózatok közötti kapcsolatokról

Csatlakozás a virtuális hálózati tooanother virtuális hálózatot (VNet – VNet) hello klasszikus üzembe helyezési modellel VPN-átjáró használata hasonló tooconnecting egy virtuális hálózati tooan helyszíni hely. Mindkét csatlakozási típusok használja a VPN-átjáró tooprovide IPsec/IKE használatával biztonságos alagutat.

hello csatlakozás Vnetekhez különböző előfizetések és a különböző régiókban is szerepelhet. Virtuális hálózat tooVNet kommunikáció többhelyes konfigurációk kombinálhatja. Így létrehozhat olyan hálózati topológiákat, amelyek a létesítmények közötti kapcsolatokat a virtuális hálózatok közötti kapcsolatokkal kombinálják.

![Virtuális hálózat tooVNet kapcsolatok](./media/vpn-gateway-howto-vnet-vnet-portal-classic/aboutconnections.png)

### <a name="why"></a>Miért érdemes összekapcsolni a virtuális hálózatokat?

A következő okok miatt hello érdemes lehet tooconnect virtuális hálózatok:

* **Georedundancia és földrajzi jelenlét több régióban**

  * Beállíthatja a saját georeplikációját vagy szinkronizálását biztonságos kapcsolaton át, internetes végpontok használata nélkül.
  * Azure Load Balancer és a Microsoft vagy harmadik fél fürtözési technológiával állíthat be magas rendelkezésre állású munkaterhelés georedundancia több Azure-régiók között. Egy fontos példája tooset fel SQL Always On rendelkezésre állási csoportokkal több Azure-régiók keresztül terjednek.
* **Az erős elkülönítési határt alkotnak regionális Többrétegű alkalmazások**

  * Belül hello azonos régióban, állíthat be több rétegből álló alkalmazások az erős elkülönítést és a réteg közötti kommunikációt a csatlakoztatott több Vnetek.
* **Kereszt-előfizetés, a szervezet közötti kommunikáció az Azure-ban**

  * Ha több Azure-előfizetéssel rendelkezik, összekapcsolhatja a munkaterhelések különböző előfizetések a együtt biztonságosan virtuális hálózatok között.
  * Vállalatok és szolgáltatók engedélyezheti a biztonságos VPN-technológia az Azure közötti-szervezet kommunikációt.

További információ a VNet – VNet kapcsolatokhoz: [VNet – VNet szempontok](#faq) hello Ez a cikk végén.

### <a name="before-you-begin"></a>Előkészületek

Ebben a gyakorlatban megkezdése előtt töltse le és telepítse hello hello Azure Service Management (SM) PowerShell-parancsmagok legújabb verzióját. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Hello portal használjuk a legtöbb hello lépésből áll, de a PowerShell toocreate hello kapcsolatainak hello Vnetek kell használnia. Nem hozható létre hello kapcsolatok hello Azure-portál használatával.

## <a name="plan"></a>1. lépés – Az IP-címtartományok megtervezése

Fontos toodecide hello tartományokat, hogy azt ismertetjük tooconfigure a virtuális hálózatokat is. Ehhez a konfigurációhoz meg kell győződnie arról, hogy a virtuális hálózat tartományok egyike átfedésben vannak egymással, vagy az összes hello helyi hálózatokat, amelyekhez csatlakoznak.

hello alábbi táblázat szemlélteti, hogyan toodefine a Vnetek. Hello tartományok használata csak iránymutatásként. Jegyezze fel a virtuális hálózatok hello tartományokat. Ezt az információt a későbbi lépésekben szükség van.

**Példa**

| Virtual Network | Címterület | Régió | Toolocal hálózati hellyel összeköti |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |USA keleti régiója |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |USA nyugati régiója |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

## <a name="vnetvalues"></a>2. lépés – hello virtuális hálózatok létrehozása

Hozzon létre két virtuális hálózatok hello [Azure-portálon](https://portal.azure.com). Hello lépések toocreate klasszikus virtuális hálózatok, a következő témakörben: [klasszikus virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). 

Hello portál toocreate a klasszikus virtuális hálózatot használ, amikor az alábbi lépések, ellenkező esetben hello beállítás toocreate a klasszikus virtuális hálózatot nem jelenik meg hello segítségével keresse meg toohello virtuális hálózat panel:

1. Kattintson a hello "+" tooopen hello "New" panelen.
2. Hello a "Hello a piactéren" mezőben írja be a "Virtuális hálózat". Ha ehelyett hálózatkezelés virtuális hálózat ->, nem fog hello beállítás toocreate egy klasszikus virtuális hálózatot.
3. Keresse meg a "Virtuális hálózat" hello-listát adott vissza a, és kattintson rá a tooopen hello virtuális hálózat panel. 
4. A virtuális hálózat panel hello válassza a "Klasszikus" toocreate egy klasszikus virtuális hálózatot. 

Ha ez a cikk egy gyakorlatot használ, a következő példában szereplő értékeket hello is használhatja:

**TestVNet1 értékei**

Name: TestVNet1<br>
Címtér: 10.11.0.0/16, 10.12.0.0/16 (nem kötelező)<br>
Alhálózati név: alapértelmezett<br>
Alhálózati címtartományt: 10.11.0.1/24<br>
Erőforráscsoport: ClassicRG<br>
Hely: East US<br>
A GatewaySubnet: 10.11.1.0/27

**TestVNet4 értékei**

Name: TestVNet4<br>
Címtér: 10.41.0.0/16, 10.42.0.0/16 (nem kötelező)<br>
Alhálózati név: alapértelmezett<br>
Alhálózati címtartományt: 10.41.0.1/24<br>
Erőforráscsoport: ClassicRG<br>
Hely: West US<br>
A GatewaySubnet: 10.41.1.0/27

**A Vnetek létrehozásakor tartsa szem előtt tartva hello a következő beállításokat:**

* **Virtuális hálózati címtereket** – hello virtuális hálózati címtereket oldalon toouse szeretné a virtuális hálózat hello-címtartományt adjon meg. Ezek a hello dinamikus IP-címek, amely toohello virtuális gépek és a többi szerepkörpéldányon központi telepítését toothis virtuális hálózat társítható.<br>hello cím választja szóköz bármely nem lehet átfedésben hello címterek hello más Vnetekről vagy a helyszíni helyeken, amelyhez ez a virtuális hálózat csatlakozni fognak.

* **Hely** – egy virtuális hálózatban, létrehozásakor hozzá kell rendelni egy Azure-beli hely (régió). Például ha azt szeretné, hogy a virtuális gépek, amelyek telepített tooyour virtuális hálózati toobe USA nyugati régiója fizikailag található, válassza ki az adott helyre. Létrehozása után a virtuális hálózathoz tartozó hello helye nem módosítható.

**Miután létrehozta a Vneteket, adja hozzá a következő beállítások hello:**

* **Címtér** – további címtartományok nincs szükség ehhez a konfigurációhoz, de további címtartományok hello VNet létrehozása után is hozzáadhat.

* **Alhálózatok** – további alhálózatokat nem szükségesek ehhez a konfigurációhoz, de célszerű toohave a virtuális gépek, a többi szerepkörpéldányon külön alhálózatokon.

* **DNS-kiszolgálók** – hello DNS-kiszolgáló neve és IP-címet adjon meg. A beállítás nem hoz létre DNS-kiszolgálót. Ez lehetővé teszi toospecify hello DNS-kiszolgálók, amelyet az toouse névfeloldás ehhez a virtuális hálózathoz.

Ebben a szakaszban hello kapcsolattípus, hello helyi webhelyhez, konfigurálni és hello átjáró létrehozása.

## <a name="localsite"></a>3. lépés - a helyi webhely hello beállítása

Az Azure által használt hello megadott beállítások minden egyes helyi hálózati hely toodetermine hogyan tooroute hello Vnetek közötti forgalmat. Minden egyes virtuális hálózat toohello megfelelő helyi hálózati forgalom tooroute kívánt kell mutatnia. Azt határozza meg a toouse toorefer tooeach helyi hálózati telephely kívánt hello nevét. Valami leíró legjobb toouse.

Például TestVNet1 tooa helyi hálózati hellyel összeköti az Ön által létrehozott "VNet4Local" nevű. hello beállításainak VNet4Local hello címelőtagokat TestVNet4 tartalmaz.

minden egyes virtuális hálózat van hello más VNet hello helyi helyen. a konfiguráció a következő példában szereplő értékeket hello használják:

| Virtual Network | Címterület | Régió | Toolocal hálózati hellyel összeköti |
|:--- |:--- |:--- |:--- |
| TestVNet1 |TestVNet1<br>(10.11.0.0/16)<br>(10.12.0.0/16) |USA keleti régiója |VNet4Local<br>(10.41.0.0/16)<br>(10.42.0.0/16) |
| TestVNet4 |TestVNet4<br>(10.41.0.0/16)<br>(10.42.0.0/16) |USA nyugati régiója |VNet1Local<br>(10.11.0.0/16)<br>(10.12.0.0/16) |

1. Keresse meg TestVNet1 hello Azure-portálon. A hello **VPN-kapcsolatok** szakasz hello paneljén kattintson **átjáró**.

    ![Nincs átjáró](./media/vpn-gateway-howto-vnet-vnet-portal-classic/nogateway.png)
2. A hello **új VPN-kapcsolat** lapon jelölje be **pont-pont**.
3. Kattintson a **helyi** tooopen helyi hely lap hello és hello-beállítások konfigurálása.
4. A hello **helyi** lapon, a helyi webhely neve. A jelen példában "VNet4Local" hello helyi webhely azt nevet.
5. A **VPN-átjáró IP-címet**, használhatja a használni kívánt IP-címeket, amíg a egy érvényes formátumban. Általában a VPN-eszköz hello tényleges külső IP-cím volna használni. De klasszikus VNet – VNet-konfigurációt használ hello nyilvános IP-címet, amely a Vnethez tartozó toohello átjáró van hozzárendelve. Fényében, hogy még nem létrehozott hello virtuális hálózati átjáró, adjon meg érvényes nyilvános IP-cím helyőrzőként.<br>Ne hagyja üresen a mezőt,-nem választható, ehhez a konfigurációhoz. Egy későbbi lépésben térjen vissza ezeket a beállításokat, és konfigurálásuk hello megfelelő virtuális hálózati átjáró IP-címekkel rendelkező, amennyiben az azt létrehozó Azure.
6. A **ügyfél Címterület**, hello címterében hello más virtuális hálózatot használja. Példa tervezési tooyour hivatkozik. Kattintson a **OK** toosave a beállításokat és a visszatérési hátsó toohello **új VPN-kapcsolat** panelen.

    ![helyi webhelyhez](./media/vpn-gateway-howto-vnet-vnet-portal-classic/localsite.png)

## <a name="gw"></a>4. lépés – hello virtuális hálózati átjáró létrehozása

Minden virtuális hálózathoz rendelkeznie kell a virtuális hálózati átjáró. hello virtuális hálózati átjáró irányítja a, és titkosítja a forgalmat.

1. A hello **új VPN-kapcsolat** panelen, jelölje be hello jelölőnégyzet **átjáró létrehozása azonnal**.
2. Kattintson a **alhálózat, méret és útválasztási típus**. A hello **átjáró konfigurációs** panelen kattintson a **alhálózati**.
3. hello átjáró alhálózati név automatikusan kitölti hello szükséges "GatewaySubnet" nevű. Hello **-címtartományt** toohello VPN-átjáró szolgáltatásokat kiosztott hello IP-cím szerepel. Bizonyos konfigurációk engedélyezése egy átjáró alhálózatának /29, de az ajánlott toouse egy /28 vagy /27 tooaccommodate jövőbeli konfigurációk, amelyre szüksége lehet további IP-címek hello átjáró számára. A példa beállítások 10.11.1.0/27 használjuk. Hello címterület módosítsa, majd kattintson az **OK**.
4. Hello konfigurálása **átjáró mérete**. Ez a beállítás hivatkozik toohello [átjáró-Termékváltozat](vpn-gateway-about-vpn-gateway-settings.md#gwsku).
5. Hello konfigurálása **útválasztási típus**. hello útválasztás típusa, ezt a konfigurációt nem lehet **dinamikus**. Hello útválasztási típusa nem módosítható később, kivéve, ha szakadjon el hello átjáró működik, és hozzon létre egy újat.
6. Kattintson az **OK** gombra.
7. A hello **új VPN-kapcsolat** panelen kattintson a **OK** toobegin hello virtuális hálózati átjáró létrehozásához. Átjáró létrehozása gyakran eltarthat 45 perc vagy több, attól függően, hogy a kiválasztott átjáróeszközön hello Termékváltozat.

## <a name="vnet4settings"></a>5. lépés - TestVNet4 beállításainak konfigurálása

Ismételje meg a hello túl[helyi webhely létrehozása](#localsite) és [hello virtuális hálózati átjáró létrehozása](#gw) tooconfigure TestVNet4, és szükség esetén hello értékeket. Akkor használatos, ha ez egy gyakorlat szerint, használja a hello [példaértékeket](#vnetvalues).

## <a name="updatelocal"></a>Frissítés hello helyi webhelyek, 6 -. lépés

Miután a virtuális hálózati átjárók mindkét Vnetek létrejött, módosítania kell a helyi telephely hello **VPN-átjáró IP-címet** értékeket.

|VNet neve|Az összekapcsolt hely|Átjáró IP-címe|
|:--- |:--- |:--- |
|TestVNet1|VNet4Local|VPN-átjáró IP-címet a TestVNet4|
|TestVNet4|VNet1Local|VPN-átjáró IP-címet a TestVNet1|

### <a name="part-1---get-hello-virtual-network-gateway-public-ip-address"></a>Get hello virtuális hálózati átjáró nyilvános IP-cím, 1 -. rész

1. Keresse meg a virtuális hálózat hello Azure-portálon.
2. Kattintson a virtuális hálózat tooopen hello **áttekintése** panelen. Hello panelen a **VPN-kapcsolatok**, tekintse meg a hello IP-címet a virtuális hálózati átjáró.

  ![Nyilvános IP-cím](./media/vpn-gateway-howto-vnet-vnet-portal-classic/publicIP.png)
3. Másolja a hello IP-címet. A következő szakaszban hello még szüksége lesz rájuk.
4. Ismételje meg ezeket a lépéseket TestVNet4

### <a name="part-2---modify-hello-local-sites"></a>2. rész – hello helyi helyek módosítása

1. Keresse meg a virtuális hálózat hello Azure-portálon.
2. A virtuális hálózat hello **áttekintése** panelen kattintson hello helyi helyre.

  ![Helyi webhely létrehozása](./media/vpn-gateway-howto-vnet-vnet-portal-classic/local.png)
3. A hello **pont-pont VPN-kapcsolatok** panelen kattintson hello helyi webhely, amelyet az toomodify hello nevét.

  ![Nyissa meg a helyi webhelyhez](./media/vpn-gateway-howto-vnet-vnet-portal-classic/openlocal.png)
4. Kattintson a hello **helyi** , amelyet az toomodify.

  ![webhely](./media/vpn-gateway-howto-vnet-vnet-portal-classic/connections.png)
5. Frissítés hello **VPN-átjáró IP-címet** kattintson **OK** toosave hello beállításait.

  ![átjáró IP](./media/vpn-gateway-howto-vnet-vnet-portal-classic/gwupdate.png)
6. Zárja be hello más paneleken.
7. Ismételje meg ezeket a lépéseket TestVNet4.

## <a name="getvalues"></a>Hello hálózati konfigurációs fájlból olvasható be értékek, 7 -. lépés

Amikor a hagyományos Vneteket hello Azure-portálon létrehoz, hello megjelenített nincs neve hello teljes nevét, a PowerShell használt. Például egy virtuális hálózat nevű toobe megjelenő **TestVNet1** hello portálon, előfordulhat, hogy mekkora hosszabb nevet adni hello hálózati konfigurációs fájlban. hello neve lehet, hogy alábbihoz hasonló: **csoport ClassicRG TestVNet1**. A kapcsolat létesítése esetén fontos toouse hello értékeket, hogy hello hálózati konfigurációs fájlban.

Az alábbi lépésekkel hello Azure-fiók tooyour kapcsolódni fog, és letöltési és nézet hello hálózati konfigurációs fájl tooobtain hello értékek, amelyek szükségesek a kapcsolatokhoz.

1. Töltse le és telepítse hello hello Azure Service Management (SM) PowerShell-parancsmagok legújabb verzióját. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

2. Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon a tooyour fiók. A következő példa toohelp csatlakozás hello használata:

  ```powershell
  Login-AzureRmAccount
  ```

  Hello előfizetések hello fiók ellenőrzése.

  ```powershell
  Get-AzureRmSubscription
  ```

  Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello előfizetést.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```

  Ezután használhatja a következő parancsmag tooadd hello az Azure-előfizetés tooPowerShell hello klasszikus telepítési modell.

  ```powershell
  Add-AzureAccount
  ```
3. Az exportálás és nézet hello hálózati konfigurációs fájlt. Hozzon létre egy könyvtárat a számítógépen, és exportálhatja a hálózati konfiguráció hello toohello könyvtára. Ebben a példában hello hálózati konfigurációs fájlban túl van exportálva**C:\AzureNet**.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
4. Nyissa meg a szöveg-szerkesztő és a nézet hello nevét a virtuális hálózatokat és a helyek hello fájlt. Ezek a kapcsolatok létrehozásakor használt hello név lesz.<br>Virtuális hálózat nevének felsorolt **VirtualNetworkSite neve =**<br>Helyneveket felsorolt **LocalNetworkSiteRef neve =**

## <a name="createconnections"></a>8. lépés – hello VPN-átjáró kapcsolatok létrehozása

Minden hello előző lépést befejezésekor, beállíthatja hello IPsec/IKE előmegosztott kulccsal és hello kapcsolat létrehozásához. E lépések csoportja PowerShell használja. VNet – VNet kapcsolatokhoz hello klasszikus telepítési modell nem állítható be az hello Azure-portálon.

Hello példákban figyelje meg, hogy a megosztott kulcs hello pontosan hello van azonos. hello megosztott kulcsot mindig meg kell egyeznie. Lehet, hogy tooreplace hello értékek ezekben a példákban a Vnetek és a helyi hálózati helyek hello pontos névvel.

1. Hello TestVNet1 tooTestVNet4 kapcsolat létrehozása.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet1' `
  -LocalNetworkSiteName '17BE5E2C_VNet4Local' -SharedKey A1b2C3D4
  ```
2. Hello TestVNet4 tooTestVNet1 kapcsolat létrehozása.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName 'Group ClassicRG TestVNet4' `
  -LocalNetworkSiteName 'F7F7BFC7_VNet1Local' -SharedKey A1b2C3D4
  ```
3. Várjon, amíg hello kapcsolatok tooinitialize. Miután hello átjáró inicializálása, a hello állapota nem "Sikeres".

  ```
  Error          :
  HttpStatusCode : OK
  Id             :
  Status         : Successful
  RequestId      :
  StatusCode     : OK
  ```

## <a name="faq"></a>A hagyományos Vneteket VNet – VNet szempontjai
* virtuális hálózatok hello lehet hello ugyanazon vagy másik előfizetést.
* virtuális hálózatok hello lehet hello ugyanazon vagy másik Azure-régiók (hely).
* A felhőszolgáltatás és a terheléselosztási végpont nem ívelhet át több virtuális hálózaton, akkor sem, ha ezek össze vannak kapcsolva.
* Több virtuális hálózat összekapcsolása a VPN-eszközök nem igényel.
* VNet – VNet támogatja a kapcsolódó Azure virtuális hálózatok. Nem támogatja a csatlakozó virtuális gépek vagy felhőszolgáltatások, amelyek nincsenek telepítve tooa virtuális hálózat.
* VNet – VNet dinamikus útválasztó átjáró szükséges. Az Azure statikus útválasztó átjáró használata nem támogatott.
* A virtuális hálózati kapcsolat használható többhelyes virtuális VPN-ekkel együtt. Nincs legfeljebb 10 VPN-alagutat tooeither kapcsolódás más virtuális hálózatok, vagy a helyszíni virtuális hálózat VPN-átjáró számára.
* hello címterek hello virtuális hálózatok és a helyszíni helyi hálózati telephely nem lehet átfedésben. Átfedő címterek, akkor a virtuális hálózatok és a feltöltése netcfg konfigurációs fájlok toofail hello létrehozását.
* A virtuális hálózatok párjai közötti redundáns alagutak nem támogatottak.
* Az összes VPN bújtatja a hello VNet, beleértve a P2S VPN, megosztási hello rendelkezésre álló sávszélesség hello VPN Gateway és hello azonos VPN gateway hasznos üzemidő SLA-t az Azure-ban.
* VNet – VNet forgalmát áthaladó hello Azure gerincét.

## <a name="next-steps"></a>Következő lépések
Ellenőrizze a kapcsolatokat. Lásd: [ellenőrizni a VPN-átjáró kapcsolatot](vpn-gateway-verify-connection-resource-manager.md).
