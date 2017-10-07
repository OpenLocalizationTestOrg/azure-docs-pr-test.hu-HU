---
title: "2-rétegbeli alkalmazás aaaHybrid kapcsolatot |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy virtuális készülékek és UDR toocreate egy többrétegű alkalmazást környezet az Azure-ban"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 1f509bec-bdd1-470d-8aa4-3cf2bb7f6134
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/05/2016
ms.author: jdial
ms.openlocfilehash: 53599862289dbf9c6ab3289b0cb8dda6430f20f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-appliance-scenario"></a>Virtuális készülék forgatókönyv
Egy általános forgatókönyv nagyobb Azure felhasználói között hello kell tooprovide a kétféle közzétett alkalmazáshoz toohello Internet, miközben lehetővé teszi a hozzáférési toohello hátsó réteg a egy helyszíni adatközpontot. Ez a dokumentum végigvezeti felhasználó definiált útvonalakat (UDR), a VPN-átjáró és a hálózati virtuális készülékek toodeploy használatát egy kétrétegű környezetben, amely megfelel a követelményeknek hello:

* Webalkalmazás elérhetőknek kell lenniük hello csak a nyilvános internethez.
* Web server üzemeltetési hello alkalmazás képes tooaccess egy háttér-alkalmazáskiszolgáló kell lennie.
* Hello Internet toohello webalkalmazás származó összes forgalmat a virtuális készülékként egy tűzfalat kell végrehajtania. A virtuális készüléknek csak az internetes forgalmat fog történni.
* Teljes forgalmat toohello alkalmazáskiszolgáló virtuális készülékként egy tűzfalat kell végrehajtania. A virtuális készüléknek hozzáférés toohello háttér-kiszolgálófiók, és hozzáférés adatforrásból származó hello a helyi hálózaton keresztül VPN-átjáró használható.
* Rendszergazdák képesek toomanage hello tűzfal virtuális készülékek a helyi számítógépükön kell lennie, a harmadik tűzfal kizárólag felügyeleti szempontból használt virtuális készülék.

Ez egy webfarmos szabványos DMZ DMZ és a védett hálózati. Ilyen eset lehet létrehozni az Azure-ban a NSG-ket, virtuális tűzfalkészülékek, vagy mindkettőt. hello az alábbi táblázat néhány hello előnyei és hátrányai NSG-k és a tűzfalon virtuális készülékek között.

|  | Informatikai szakemberek | Hátrányok |
| --- | --- | --- |
| NSG |Költség nélkül. <br/>Az Azure RBAC integrálva. <br/>Szabályok ARM-sablonokkal hozhatók létre. |Nagyobb környezetekben eltérőek lehetnek a összetettségét. |
| Tűzfal |Adatok vezérlősík teljes ellenőrzést. <br/>Központi felügyeleti tűzfal konzolon keresztül. |Tűzfal készülék költségét. <br/>Nincs integrálva az Azure RBAC. |

hello megoldás az alábbi tűzfal virtuális készülékek tooimplement egy Szegélyhálózaton/védett hálózati forgatókönyv használ.

## <a name="considerations"></a>Megfontolandó szempontok
Az Azure-ban elérhető különböző szolgáltatások ma, az alábbiak szerint fenti hello környezet telepítése.

* **Virtuális hálózathoz (VNet)**. Egy Azure virtuális hálózatot hasonló módon tooan a helyi hálózaton működik, és egy vagy több alhálózatok tooprovide adatforgalom-elkülönítést és aggályokat elválasztását is lehet szegmentált.
* **Virtuális készülék**. Több partnerek adja meg a virtuális készülékek hello Azure piactér hello a fent leírt három tűzfalak használható. 
* **Felhasználó által megadott útvonalak (UDR)**. Az útvonaltáblák tartalmazhat udr-EK Azure hálózati toocontrol hello áramló csomagok egy Vneten belül használják. Ezek az útvonaltáblák alkalmazott toosubnets lehet. Hello legújabb funkciókat az Azure-ban egyik hello képességét tooapply egy útvonal tábla toohello GatewaySubnet hello képességét tooforward összes adatforgalma hello Azure virtuális hálózatot az egy hibrid kapcsolat tooa virtuális készülék biztosítása.
* **IP-továbbítás**. Alapértelmezés szerint hello Azure hálózati motor továbbítsa a csomagokat toovirtual hálózati adapterek (NIC) csak akkor, ha a hello csomag cél IP-címe megegyezik hello hálózati adapter IP-címet. Ezért egy UDR határozza meg, hogy egy csomagot kell küldeni virtuális készülék megadott tooa, hello Azure hálózati motor csomagban csökkenne. tooensure hello csomagot a rendszer tooa virtuális Gépet (ebben az esetben a virtuális készülék), amely nincs hello tényleges cél hello csomagok számára, az IP-továbbítás tooenable hello virtuális készülékre van szüksége.
* **Hálózati biztonsági csoportokkal (NSG-k)**. az alábbi példa hello tegye az NSG-k használatát, de az NSG-k alkalmazott toohello alhálózatok és/vagy a hálózati adapterek használhatja a megoldás toofurther szűrő hello forgalom mindkét ezek alhálózatok és a hálózati adapterek.

![IPv6-kapcsolatot](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Ebben a példában nincs olyan előfizetést, amelyet hello következő tartalmazza:

* 2 erőforráscsoportok, hello ábrán nem látható. 
  * **ONPREMRG**. Minden erőforrások szükséges egy helyszíni hálózat toosimulate tartalmazza.
  * **AZURERG**. Hello Azure-beli virtuális hálózat környezethez szükséges összes erőforrást tartalmaz. 
* Egy VNet nevű **onpremvnet** egy helyszíni adatközpontot szegmentált az alább felsorolt toomimic használt.
  * **onpremsn1**. Ubuntu toomimic egy helyszíni kiszolgálón futó virtuális gép (VM) tartalmazó alhálózat.
  * **onpremsn2**. A rendszergazda által használt helyi számítógépet futtat Ubuntu toomimic virtuális gép tartalmazó alhálózat.
* Van egy tűzfal virtuális készülék nevű **OPFW** a **onpremvnet** toomaintain alagutat túl használt**azurevnet**.
* Egy VNet nevű **azurevnet** szegmentált az alábbiak szerint.
  * **azsn1**. Külső tűzfal alhálózati kizárólag hello külső tűzfal használja. Minden internetes forgalomhoz határozza meg az alhálózat keresztül. Az alhálózathoz társított hálózati adapter toohello külső tűzfalat csak tartalmaz.
  * **azsn2**. A kiszolgáló fogják elérni az internetes hello futtató virtuális gép futtató előtér alhálózat.
  * **azsn3**. Backend alhálózathoz üzemeltető virtuális gép fut, egy háttér-alkalmazáskiszolgáló hello előtér-webkiszolgálón kell elérnie.
  * **azsn4**. Felügyeleti alhálózati használt kizárólag tooprovide felügyeleti hozzáférés tooall tűzfal virtuális készülékek. Ez az alhálózat csak egy hálózati Adapterhez hello megoldásban használt egyes tűzfal virtuális készülék tartalmazza.
  * **A GatewaySubnet**. Azure hibrid kapcsolat alhálózat az Azure Vnetekhez és más hálózatokkal között ExpressRoute- és VPN-átjáró tooprovide kapcsolat szükséges. 
* Nincsenek 3 tűzfal virtuális készülékek hello **azurevnet** hálózati. 
  * **AZF1**. Külső tűzfal kitett toohello az Azure-ban egy nyilvános IP-cím erőforrás a nyilvános internethez. Tooensure hello piactér, illetve közvetlenül a készülék forgalmazójával, hogy rendelkezések sablon 3-NIC virtuális készülékre van szüksége.
  * **AZF2**. Belső tűzfal használt közötti forgalom toocontrol **azsn2** és **azsn3**. Ez egyben a 3-NIC virtuális készülék.
  * **AZF3**. Felügyeleti tűzfal elérhető tooadministrators hello a helyszíni adatközpontban, és csatlakoztatott tooa felügyeleti alhálózati használt toomanage összes tűzfalkészülékek. 2-NIC virtuális készülék sablonok hello piactér található, vagy közvetlenül a készülék szállítótól kérhet.

## <a name="user-defined-routing-udr"></a>Felhasználó által definiált útválasztási (UDR)
Minden Azure alhálózatban lehet csatolt tooa UDR használt tábla toodefine hogyan alhálózat kezdeményezett továbbítódik. Ha nincs udr-EK vannak meghatározva, Azure használja alapértelmezett útvonalak tooallow forgalom tooflow az egyik alhálózat tooanother. toobetter udr-EK megértéséhez, látogasson el [Mik azok a felhasználó által megadott útvonalak és IP-továbbítás](virtual-networks-udr-overview.md#ip-forwarding).

tooensure kommunikációs hello megfelelő tűzfal, rendszerre épülő készüléknek meg a fenti utolsó követelmény hello segítségével történik, toocreate hello következőkre lesz szüksége a udr-EK tartalmazó útvonaltábla **azurevnet**.

### <a name="azgwudr"></a>azgwudr
Ebben a forgatókönyvben hello csak a helyszíni tooAzure áramlanak forgalom lesz használt toomanage hello tűzfalak túl csatlakoztatásával**AZF3**, és hogy forgalom be kell lépnie hello belső tűzfalon keresztül, **AZF2**. Ezért csak egy útvonal hello szükség az **GatewaySubnet** alább látható módon.

| Cél | Következő ugrás | Magyarázat |
| --- | --- | --- |
| 10.0.4.0/24 |10.0.3.11 |Lehetővé teszi, hogy a helyszíni forgalom tooreach felügyeleti tűzfal **AZF3** |

### <a name="azsn2udr"></a>azsn2udr
| Cél | Következő ugrás | Magyarázat |
| --- | --- | --- |
| 10.0.3.0/24 |10.0.2.11 |Lehetővé teszi, hogy a forgalom toohello backend alhálózathoz hello alkalmazáskiszolgáló keresztül üzemeltető **AZF2** |
| 0.0.0.0/0 |10.0.2.10 |Lehetővé teszi, hogy minden keresztül más forgalom toobe **AZF1** |

### <a name="azsn3udr"></a>azsn3udr
| Cél | Következő ugrás | Magyarázat |
| --- | --- | --- |
| 10.0.2.0/24 |10.0.3.10 |Lehetővé teszi a forgalom túl**azsn2** app server toohello webkiszolgáló keresztül tooflow **AZF2** |

Hello alhálózatai toocreate útválasztási táblázattal is szüksége **onpremvnet** toomimic hello helyszíni adatközpontban.

### <a name="onpremsn1udr"></a>onpremsn1udr
| Cél | Következő ugrás | Magyarázat |
| --- | --- | --- |
| 192.168.2.0/24 |192.168.1.4 |Lehetővé teszi a forgalom túl**onpremsn2** keresztül **OPFW** |

### <a name="onpremsn2udr"></a>onpremsn2udr
| Cél | Következő ugrás | Magyarázat |
| --- | --- | --- |
| 10.0.3.0/24 |192.168.2.4 |Lehetővé teszi, hogy a forgalom biztonsági toohello alhálózati keresztül az Azure-ban **OPFW** |
| 192.168.1.0/24 |192.168.2.4 |Lehetővé teszi a forgalom túl**onpremsn1** keresztül **OPFW** |

## <a name="ip-forwarding"></a>IP-továbbítás
UDR és IP-továbbítás olyan funkciókat, amelyek kombinációja tooallow virtuális készülékek használt toobe toocontrol forgalom áramlását az egy Azure virtuális hálózatot is használhat.  A virtuális készülék értéke "Nothing" több mint egy virtuális Gépet, amelyen az alkalmazás toohandle hálózati forgalom valamilyen módon, például egy tűzfal vagy NAT-eszköz.

A virtuális gép kell lennie, amely képes tooreceive bejövő forgalom virtuális készüléknek tooitself nem foglalkozik. a virtuális gépek tooreceive forgalma tooallow címzett tooother a célok, engedélyeznie kell az IP-továbbítás az hello VM. Ez az egy Azure beállítás, nem a megfelelő értéket hello vendég operációs rendszer. A virtuális készülék továbbra is kell toorun bizonyos típusú alkalmazás toohandle hello bejövő forgalmat, és megfelelően irányítja.

További információ az IP-továbbítást, toolearn látogasson el [Mik azok a felhasználó által megadott útvonalak és IP-továbbítás](virtual-networks-udr-overview.md#ip-forwarding).

Tegyük fel képzelhető el, egy Azure virtuális hálózatot a telepítő a következő hello rendelkezik:

* Alhálózati **onpremsn1** tartalmaz egy nevű virtuális gép **onpremvm1**.
* Alhálózati **onpremsn2** tartalmaz egy nevű virtuális gép **onpremvm2**.
* A virtuális készülék nevű **OPFW** túl csatlakozik-e**onpremsn1** és **onpremsn2**.
* A felhasználó által megadott útvonal túl kapcsolódó**onpremsn1** határozza meg, hogy minden túl forgalomnak**onpremsn2** túl el kell küldeni**OPFW**.

At a pont, ha **onpremvm1** tooestablish megpróbál kapcsolatot **onpremvm2**, hello UDR fogja használni, és a forgalom túl küld**OPFW** hello következő ugrásként. Ne feledje, hogy a tényleges csomagban cél hello nem módosul, továbbra is felirat látható **onpremvm2** hello cél. 

Az IP-továbbítás engedélyezve nélkül **OPFW**, hello Azure virtuális hálózati logika eldobja hello csomagok, mivel csak lehetővé teszi a csomagok küldése toobe tooa VM Ha hello a virtuális gép IP-cím-e a hello csomagok hello célját.

Az IP-továbbítást hello Azure-beli virtuális hálózat logika továbbításához hello csomagok tooOPFW, eredeti cél címének módosítása nélkül. **OPFW** kell hello csomagok kezeli, és határozza meg, milyen toodo.

A fenti toowork hello esetben engedélyeznie kell az IP-továbbítás hello hálózati adaptereket a **OPFW**, **AZF1**, **AZF2**, és **AZF3** szolgáló az útválasztási (hálózati adapterek összes hello néhányat a meglévők közül csatolt toohello felügyeleti alhálózati kivételével). 

## <a name="firewall-rules"></a>Tűzfalszabályok
A fent leírtaknak megfelelően biztosítja az IP-továbbítás csak csomagok küldése toohello virtuális készülékek. A készülék továbbra is hozzá kell toodecide milyen toodo az ezeket a csomagokat. Hello a fenti esetben szüksége lesz toocreate hello készülékeinek szabályai a következő:

### <a name="opfw"></a>OPFW
OPFW egy a helyszíni eszközöket, amelyek szabályainak hello jelöli:

* **Útvonal**: minden forgalomnak too10.0.0.0/16 (**azurevnet**) alagúton keresztül kell küldeni **ONPREMAZURE**.
* **Házirend**: közötti összes kétirányú forgalmat engedélyezi **port2** és **ONPREMAZURE**.

### <a name="azf1"></a>AZF1
AZF1 tartalmazó szabályainak hello Azure virtuális készülék jelöli:

* **Házirend**: közötti összes kétirányú forgalmat engedélyezi **port1** és **port2**.

### <a name="azf2"></a>AZF2
AZF2 tartalmazó szabályainak hello Azure virtuális készülék jelöli:

* **Útvonal**: minden forgalomnak too10.0.0.0/16 (**onpremvnet**) el kell küldeni toohello Azure átjáró IP-címe (például 10.0.0.1) keresztül **port1**.
* **Házirend**: közötti összes kétirányú forgalmat engedélyezi **port1** és **port2**.

## <a name="network-security-groups-nsgs"></a>Hálózati biztonsági csoportokkal (NSG-k)
Ebben a forgatókönyvben az NSG-k nincsenek használatban. Azonban alkalmazhat az NSG-k tooeach toorestrict bejövő és kimenő forgalmat. Például alkalmazhat a következő NSG szabályok toohello külső FW alhálózati hello.

**Bejövő**

* Hello alhálózati hello Internet tooport 80 bármely virtuális gépen a TCP-forgalom engedélyezése.
* Hello Internet megtagadása az összes többi forgalom.

**Kimenő**

* Az összes forgalom toohello Internet megtagadja.

## <a name="high-level-steps"></a>Magas szintű lépései
toodeploy ebben az esetben kövesse hello magas szintű lépéseket.

1. Bejelentkezési tooyour Azure-előfizetést.
2. Ha azt szeretné, hogy a virtuális hálózat toomimic hello toodeploy a helyszíni hálózat, kiépítése hello erőforrások részét képező **ONPREMRG**.
3. Hello erőforrások részét képező biztosításához **AZURERG**.
4. Kiépítés hello alagút a **onpremvnet** túl**azurevnet**.
5. Amennyiben az összes erőforrás törlődnek, jelentkezzen be túl**onpremvm2** ping 10.0.3.101 tootest közötti kapcsolatot, **onpremsn2** és **azsn3**.

