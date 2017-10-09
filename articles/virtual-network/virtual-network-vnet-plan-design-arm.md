---
title: "Virtual Network (VNet) aaaAzure a tervezési és kialakítási útmutató |} Microsoft Docs"
description: "Ismerje meg, hogyan tooplan és a Tervező virtuális hálózatok az Azure-ban a elkülönítési, a kapcsolat és a hely követelmények alapján."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2016
ms.author: jdial
ms.openlocfilehash: f3ffadf8cf254f64b1f86b44f90315d2bc679f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="plan-and-design-azure-virtual-networks"></a>Azure virtuális hálózatok megtervezése
Egy VNet tooexperiment rendelkező létrehozása elég egyszerűen, de valószínűleg több Vnetek keresztül idő toosupport hello éles a szervezet igényeinek központilag telepíti. Egyes tervezési és kialakítási először kell tudni toodeploy Vnetek, csatlakozásra hello erőforrások hatékonyabban van szüksége. Ha nem ismeri a Vneteket, javasoljuk, hogy Ön [Vnetek megismerése](virtual-networks-overview.md) és [hogyan toodeploy](virtual-networks-create-vnet-arm-pportal.md) egy, a folytatás előtt.

## <a name="plan"></a>Felkészülés
Azure-előfizetések, régiók és hálózati erőforrásokat alapos ismerete fontos a sikeres. Hello listája alább szempontok használhatja kiindulási pontként. E szempontok elsajátítása után hello követelmények hálózatterv adhat meg.

### <a name="considerations"></a>Megfontolandó szempontok
Az alábbi lehetőségei hello válasz, előtt vegye figyelembe a hello következőket:

* Minden Azure-ban létrehozhat egy vagy több erőforrás áll. Egy virtuális gép (VM) egy olyan erőforrás, egy virtuális gép által használt hello hálózati adapter kapcsolatának (NIC) egy erőforrást, hello egy hálózati adapter által használt nyilvános IP-cím erőforrás, hello VNet hello NIC csatlakoztatott toois erőforrás.
* Erőforrások létrehozása egy [az Azure-régió](https://azure.microsoft.com/regions/#services) és -előfizetése. Erőforrások csak lehet csatlakoztatott tooa virtuális hálózat már szerepel a hello azonos régióban és az előfizetés hello erőforrás.
* Más virtuális hálózatok tooeach használatával képes kapcsolódni:
    * **[Virtuális hálózati társviszony-létesítés](virtual-network-peering-overview.md)**: hello virtuális hálózatok már léteznie kell hello azonos Azure-régiót. Sávszélesség erőforrások között lévő virtuális hálózatok társviszonyban van hello ugyanaz, mintha hello erőforrások csatlakoztatott toohello ugyanazt a virtuális hálózatot.
    * **Egy Azure [VPN-átjáró](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)**: hello virtuális hálózatok létezhet hello azonos, vagy másik Azure-régiók. Hello sávszélesség hello VPN-átjárót az erőforrások egy VPN-átjárón keresztül csatlakozó virtuális hálózatok közötti sávszélesség korlátozza.
* Hello egyikével csatlakoztathatja Vnetek tooyour a helyszíni hálózat [kapcsolati lehetőségek](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) érhető el az Azure-ban.
* Különböző erőforrások foghatók egybe a [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md#resource-groups), így könnyebben toomanage hello erőforrás egységként. Erőforráscsoport is több régióba származó erőforrásokat tartalmaznak, mindaddig, amíg hello erőforrások tartoznak toohello ugyanahhoz az előfizetéshez.

### <a name="define-requirements"></a>Követelmények megadása
Alább hello kérdések használata kiindulási pontként a Azure hálózati kialakítását.    

1. Mi az Azure helyek fogja toohost Vnetek használni?
2. Kell-e tooprovide kommunikációs Azure között?
3. Szüksége van az Azure VNet(s) és a helyszíni datacenter(s) közötti tooprovide kommunikációs?
4. Egy szolgáltatási (IaaS) virtuális gép, hogy hány infrastruktúra cloud services – szerepkörök és web apps nincs szüksége a megoldás?
5. Kell-e a virtuális gépek (azaz előtér-webkiszolgáló és háttérbeli adatbázis-kiszolgálók) alapuló tooisolate forgalom?
6. Virtuális készülékek használata toocontrol adatforgalmat kell?
7. Hajtsa végre az Azure-erőforrások felhasználók kell különböző karakterkészleteket engedélyek toodifferent?

### <a name="understand-vnet-and-subnet-properties"></a>VNet és alhálózat tulajdonságok ismertetése
Hálózatok és alhálózatok erőforrások segítségével határozza meg az Azure-ban futó munkaterhelések biztonsági korlátot. Egy VNet jellemzőek, címterekhez, meghatározott CIDR-blokkok gyűjteménye.

> [!NOTE]
> A hálózati rendszergazdák jártas CIDR-formátumban. Ha nem ismeri a CIDR, [olvashat azokról bővebben](http://whatismyipaddress.com/cidr).
>
>

Vnetek hello következő tulajdonságai tartalmaznak.

| Tulajdonság | Leírás | Korlátozások |
| --- | --- | --- |
| **név** |VNet neve |Karakterlánc mentése too80 karaktereket. Betűk, számok, aláhúzásjelet, pontokat és kötőjeleket tartalmazhat. Betűvel vagy számmal kell kezdődnie. Betűvel, számmal vagy aláhúzásjellel kell végződnie. Felső vagy kisbetűket is tartalmaz. |
| **hely** |Azure-beli hely (is hivatkozott tooas régió). |Közül kell hello érvényes Azure helyekre. |
| **Címtartományt** |A CIDR jelölésrendszer VNet hello alkotó címelőtagokat gyűjteménye. |Érvényes CIDR címblokkokat, beleértve a nyilvános IP-címtartományok tömbje kell lennie. |
| **alhálózatok** |Hello virtuális hálózatot alkotó alhálózatok gyűjteménye |hello alhálózati tulajdonságok az alábbi táblázatban találja. |
| **dhcpOptions** |Egy kötelező tulajdonság nevű tartalmazó objektum **dnsServers**. | |
| **dnsServers** |Hello virtuális hálózat által használt DNS-kiszolgálók tömbje. Ha nincs megadva kiszolgáló, az Azure belső névfeloldást szolgál. |Too10 DNS-kiszolgálókat, IP-cím szerint tömbje kell lennie. |

Egy alhálózat egy Vnetet gyermek erőforrása, és segítségével meghatározhatja a címterek belül használja az IP-cím előtagokat CIDR-blokkja részeit. Hálózati adapter toosubnets, és a csatlakoztatott tooVMs biztosítható a kapcsolat a különböző munkaterhelések lehet hozzáadni.

Alhálózatok hello következő tulajdonságai tartalmaznak.

| Tulajdonság | Leírás | Korlátozások |
| --- | --- | --- |
| **név** |Alhálózat neve |Karakterlánc mentése too80 karaktereket. Betűk, számok, aláhúzásjelet, pontokat és kötőjeleket tartalmazhat. Betűvel vagy számmal kell kezdődnie. Betűvel, számmal vagy aláhúzásjellel kell végződnie. Felső vagy kisbetűket is tartalmaz. |
| **hely** |Azure-beli hely (is hivatkozott tooas régió). |Közül kell hello érvényes Azure helyekre. |
| **addressPrefix** |Egyetlen címelőtag fel hello alhálózat CIDR-jelöléssel |Egy egyetlen CIDR-blokkja valamelyik hello VNet-részét képező kell lennie. |
| **hálózati biztonsági csoporthoz tartozik** |Alkalmazott NSG toohello alhálózati | |
| **Migrálták** |Az útvonaltábla alkalmazott toohello alhálózati | |
| **IP-konfigurációk** |Hálózati adapter csatlakoztatott toohello alhálózati által használt IP-konfigurációs objektumok gyűjteménye | |

### <a name="name-resolution"></a>Névfeloldás
Alapértelmezés szerint a virtuális hálózat használja [Azure által biztosított névfeloldás](virtual-networks-name-resolution-for-vms-and-role-instances.md) tooresolve nevek hello virtuális hálózaton belül, és a nyilvános interneten hello. Azonban ha a Vnetek tooyour helyszíni adatközpontok, meg kell tooprovide [a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md) tooresolve nevek a hálózatok között.  

### <a name="limits"></a>Korlátok
Tekintse át a hálózati korlátok hello a hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikk tooensure a tervező nem ütköznek a hello korlátok bármelyikét. Néhány korlátot lehet növelni egy támogatási jegy megnyitásával.

### <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-vezérlés (RBAC)
Használhat [Azure RBAC](../active-directory/role-based-access-built-in-roles.md) toocontrol hello szintű hozzáférés különböző felhasználók rendelkezhetnek toodifferent erőforrások az Azure-ban. Ily módon is elkülönítse a csapat a igényeiknek hello dolgozott.

Csakúgy, mint a virtuális hálózatok az érintett felhasználók hello **hálózat közreműködő** szerepkör Azure Resource Manager virtuális hálózati erőforrások teljes hozzáféréssel rendelkeznek. Ehhez hasonlóan a hello felhasználók **klasszikus hálózat közreműködő** szerepkör klasszikus virtuális hálózati erőforrások teljes hozzáféréssel rendelkeznek.

> [!NOTE]
> Emellett [saját szerepköröket hozhat létre](../active-directory/role-based-access-control-configure.md) tooseparate felügyeleti igényeinek.
>
>

## <a name="design"></a>Tervezés
Miután eldöntötte hello válaszokat, hello toohello kérdéseire [megtervezése](#Plan) szakaszban, tekintse át a Vnetek definiálása előtt hello következő.

### <a name="number-of-subscriptions-and-vnets"></a>Az előfizetések és Vnetek száma
Érdemes lehet több Vnetek létrehozása a következő forgatókönyvek hello:

* **Virtuális gépek különböző Azure helyszíneken található toobe igénylő**. Az Azure Vnetekhez regionális. Ezek nem terjedhetnek ki helyét. Ezért szükség van legalább egy virtuális hálózat minden egyes toohost virtuális gépek a kívánt Azure-beli hely.
* **Egy másik teljesen izolált toobe igénylő munkaterhelések**. Külön Vnetek, hogy akkor is használható hello azonos IP-címterek, tooisolate különböző terhelésekhez egymástól hozhat létre.

Ne feledje, hogy a fent látható hello korlátok régiónként, amelyek. Ez azt jelenti, hogy több előfizetések tooincrease hello korlát az Azure-ban kezelheti erőforrások is használhatja. Használhatja a telephelyek közötti VPN vagy ExpressRoute-kapcsolatcsoportot, tooconnect Vnetekhez különböző előfizetésekhez.

### <a name="subscription-and-vnet-design-patterns"></a>Előfizetés és hálózatok tervezési minták
hello az alábbi táblázat néhány gyakori tervminták előfizetések és Vnetek használatával.

| Forgatókönyv | Ábra | Informatikai szakemberek | Hátrányok |
| --- | --- | --- | --- |
| Egyetlen előfizetés, két Vnetek alkalmazásonkénti |![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure1.png) |Csak egyetlen előfizetéssel toomanage. |Maximális száma Vnetek Azure-régiót. Legalább egy előfizetésre van szüksége, amely után. Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikkben alább. |
| Alkalmazásonkénti alkalmazásonkénti két Vnetek egy előfizetés |![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure2.png) |Előfizetésenként csak két Vnetek használja. |Ha túl sok alkalmazás nehezebben toomanage. |
| Egy előfizetés üzleti egység, két Vnetek alkalmazásonkénti. |![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure3.png) |Előfizetések számának és a Vnetek közötti egyensúly. |Maximális száma Vnetek részleg (előfizetés). Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikkben alább. |
| Egy előfizetés üzleti egység, az egyes alkalmazások két Vnetek. |![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure4.png) |Előfizetések számának és a Vnetek közötti egyensúly. |Alkalmazások kell lehet különíteni alhálózatokat és az NSG-ket. |

### <a name="number-of-subnets"></a>Alhálózatok száma
A következő forgatókönyvek hello a Vneten belül több alhálózaton kell figyelembe vennie:

* **Az alhálózat összes hálózati adapter nem elég saját IP-címek**. Az alhálózati címteret nem tartalmaz elegendő IP-cím hello hello alhálózatot a hálózati adapterek száma, ha szüksége toocreate több alhálózatra. Ne feledje, hogy Azure fenntartja az egyes alhálózatokon, amelyek nem használhatók 5 magánhálózati IP-címek: hello első és utolsó hello címtartomány (hello alhálózati címet, és a csoportos küldés) és belső (célokra DHCP és DNS-) 3 címek toobe címét.
* **Biztonság**. Alhálózatok tooseparate csoportok virtuális gépek egymástól használhatók, amelyek rendelkeznek egy többrétegű struktúra, és alkalmazza a különböző munkaterhelések [hálózati biztonsági csoportokkal (NSG-k)](virtual-networks-nsg.md#subnets) ezek alhálózatok esetében.
* **A hibrid kapcsolat**. Használható VPN-átjárók és az ExpressRoute-Kapcsolatcsoportok túl[csatlakozás](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) a Vnetek tooone egy másik, és tooyour a helyszíni adatok center(s). VPN-átjárók és az ExpressRoute-Kapcsolatcsoportok kell létrehozni a saját toobe alhálózatának.
* **Virtuális készülékek**. Egy Azure virtuális hálózatot a virtuális készülék, például tűzfalat, WAN gyorsító vagy VPN-átjáró használható. Ha így tesz, akkor túl kell[forgalmat](virtual-networks-udr-overview.md) toothose készülékek és a saját alhálózat elkülöníti azokat.

### <a name="subnet-and-nsg-design-patterns"></a>Alhálózat és NSG-tervezési minták
hello az alábbi táblázat néhány gyakori tervminták alhálózatok használatával.

| Forgatókönyv | Ábra | Informatikai szakemberek | Hátrányok |
| --- | --- | --- | --- |
| Egyetlen alhálózattal, NSG-k száma alkalmazásonként-alkalmazási rétegben |![Egyetlen alhálózattal](./media/virtual-network-vnet-plan-design-arm/figure5.png) |Csak egy alhálózat toomanage. |Több NSG-ket szükséges tooisolate minden alkalmazáshoz. |
| Egy alhálózat alkalmazásonkénti alkalmazás rétegenként NSG-k |![Alkalmazásonkénti alhálózati](./media/virtual-network-vnet-plan-design-arm/figure6.png) |Kevesebb NSG-k toomanage. |Több alhálózat toomanage. |
| Egy alhálózat alkalmazás rétegenként alkalmazásonkénti NSG-ket. |![Rétegenként alhálózati](./media/virtual-network-vnet-plan-design-arm/figure7.png) |Kiegyenlítheti száma alhálózat és NSG-k között. |Előfizetésenként NSG-k maximális száma. Felülvizsgálati hello [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) cikkben alább. |
| Alkalmazás rétegenként alkalmazásonkénti NSG-k száma alhálózat több alhálózattal |![Alkalmazásonkénti rétegenként alhálózati](./media/virtual-network-vnet-plan-design-arm/figure8.png) |Valószínűleg kisebb NSG-k száma. |Több alhálózat toomanage. |

## <a name="sample-design"></a>A minta-Tervező
tooillustrate hello alkalmazásának hello információkat ebben a cikkben vegye figyelembe a következő forgatókönyv hello.

A vállalatnál, amelyen 2 adatközpontok Észak-Amerikában, és két adatközpontok Európa dolgozik. 6 azonosított 2 különböző üzleti egységek által kezelt alkalmazások elérhető különböző ügyfél, amelyet az toomigrate tooAzure, egy kísérlet keretében. hello alapvető architektúráját hello alkalmazásokhoz a következők:

* Az App1, App2, App3 és App4 futtató Ubuntu Linux-kiszolgálókon lévő webalkalmazások. Minden alkalmazás a Linux-kiszolgálókon a RESTful-szolgáltatásokat futtató tooa különálló alkalmazáskészletekben kiszolgáló csatlakozik. hello RESTful-szolgáltatásokat csatlakozás tooa háttér MySQL-adatbázis.
* App5 és App6 is a Windows a Windows Server 2012 R2 rendszerű kiszolgálókon tárolt webes alkalmazásokhoz. Minden alkalmazás tooa háttérben SQL Server-adatbázishoz csatlakozik.
* Minden alkalmazás jelenleg tárolt hello vállalati adatközpontok Észak-Amerikában egyikét.
* a helyszíni adatközpont hello hello 10.0.0.0/8 címterület használata.

A virtuális hálózati megoldás, amely megfelel a követelményeknek hello toodesign lesz szüksége:

* Mindegyik üzleti egység más üzleti egységek hálózatierőforrás-fogyasztás nem érinti.
* Virtuális hálózatokat és alhálózatokat toomake felügyeleti könnyebb hello mennyisége kell minimálisra csökkenthető.
* Mindegyik üzleti egység egy teszt/fejlesztői VNet összes alkalmazás használt kell rendelkeznie.
* Minden alkalmazás 2 különböző Azure-adatközpont / kontinensen (Észak-Amerikából és Európából végzik a munkájukat) helyezkedik el.
* Minden alkalmazás teljesen elkülönülnek egymástól.
* Minden alkalmazás elérhető az ügyfelek által az hello Internet HTTP-n keresztül.
* Minden alkalmazás elérhetők, felhasználók csatlakoztatott toohello helyszíni adatközpontok egy titkosított alagút használatával.
* Kapcsolat tooon helyszíni adatközpontok használjon meglévő VPN-eszközök.
* hello vállalati hálózati csoport kell faxrendszergazdáknak teljes hozzáférésük van hello VNet konfigurációját.
* Mindegyik üzleti egység a fejlesztők használatuk csak akkor tudja toodeploy virtuális gépek tooexisting alhálózatokat.
* Minden alkalmazást telepíti át, mivel ezek tooAzure (növekedési-és-shift).
* az egyes helyeken hello adatbázisok kell replikálni tooother Azure helyek naponta egyszer.
* Minden alkalmazás kell használni, 5 előtér-webkiszolgálók, 2 alkalmazáskiszolgálók (ha szükséges) és 2 adatbázis-kiszolgálókhoz.

### <a name="plan"></a>Felkészülés
Először meg kell válaszolnia a Tervező hello hello kérdés megválaszolásával tervezési [követelményeinek meghatározása](#Define-requirements) szakaszban, ahogy alább látható.

1. Mi az Azure helyek fogja toohost Vnetek használni?

    Észak-Amerikában 2 helyeket, és az Európai 2 helyek. Ki kell választania azokat a meglévő helyszíni adatközpontok hello fizikai helye alapján. Ezzel a módszerrel a kapcsolatot a fizikai helyek tooAzure a jobb késleltetése lesz.
2. Kell-e tooprovide kommunikációs Azure között?

    Igen. Hello adatbázisok replikált tooall helyek kell lennie.
3. Szüksége van az Azure VNet(s) és a helyszíni adatok center(s) közötti tooprovide kommunikációs?

    Igen. Felhasználók csatlakoztatva toohello helyszíni adatközpontok kell tudni tooaccess hello alkalmazások egy titkosított alagúton keresztül.
4. Hány IaaS virtuális gépeket kell megoldást?

    200 IaaS virtuális gépeket. Az App1, App2, App3 és App4 minden 5 webkiszolgálók, minden egyes 2 alkalmazáskiszolgálók és szükséges 2 adatbázis-kiszolgálókhoz minden. Ez összesen alkalmazásonként 9 IaaS virtuális gépeket, vagy 36 IaaS virtuális gépeket. App5 és App6 5 webkiszolgálókat és a 2 adatbázis-kiszolgálókhoz minden szükséges. Ez összesen alkalmazásonként 7 IaaS virtuális gépeket, illetve 14 IaaS virtuális gépeket. Ezért figyelembe kell 50 IaaS virtuális gépek minden egyes Azure-régióban lévő alkalmazásokhoz. Toouse 4 régiók igazolnia kell, mert nem lesznek 200 IaaS virtuális gépeket.

    Az Azure IaaS virtuális gépeket és a helyszíni hálózat között is kell tooprovide DNS-kiszolgálók minden egyes virtuális hálózatot, vagy a helyszíni adatok központok tooresolve neve.
5. Kell-e a virtuális gépek (azaz előtér-webkiszolgáló és háttérbeli adatbázis-kiszolgálók) alapuló tooisolate forgalom?

    Igen. Minden alkalmazás legyen teljesen elkülönülnek egymástól, és minden alkalmazásréteg is el kell különíteni.
6. Virtuális készülékek használata toocontrol adatforgalmat kell?

    Nem. Virtuális készülékek lehet használt tooprovide több forgalom áramlását – beleértve a részletes adatok vezérlősík naplózás vezérlése.
7. Hajtsa végre az Azure-erőforrások felhasználók kell különböző karakterkészleteket engedélyek toodifferent?

    Igen. hello hálózati csapatának a következőket kell teljes hozzáférést hello virtuális hálózati beállítások, amíg a fejlesztők csak akkor tudja toodeploy a virtuális gépek toopre létező alhálózataikat.

### <a name="design"></a>Tervezés
Hello tervezési előfizetések, Vnetek, alhálózatok és egyéb NSG-ket kell követnie. Itt ismertetik az NSG-k, de kapcsolatos kell további [NSG-k](virtual-networks-nsg.md) a tervezés befejezése előtt.

**Az előfizetések és Vnetek száma**

hello követelményeknek kapcsolódó toosubscriptions és Vnetek:

* Mindegyik üzleti egység más üzleti egységek hálózatierőforrás-fogyasztás nem érinti.
* Hello mennyiségét virtuális hálózatokat és alhálózatokat kell minimálisra csökkenthető.
* Mindegyik üzleti egység egy teszt/fejlesztői VNet összes alkalmazás használt kell rendelkeznie.
* Minden alkalmazás 2 különböző Azure-adatközpont / kontinensen (Észak-Amerikából és Európából végzik a munkájukat) helyezkedik el.

Ezek a követelmények, meg kell egy előfizetési részlegek számára. Így az üzleti egységek az erőforrások fogyasztásának is nem beleszámít korlátok egyéb üzleti egységek számára. És mivel azt szeretné, hogy a Vnetek toominimize hello száma, érdemes hello **üzleti egység, az egyes alkalmazások két Vnetek egy előfizetés** mintát, az alább látható módon.

![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Minden egyes vnet toospecify hello címterület is kell. Mivel szüksége hello közötti kapcsolatot a helyszíni adatközpont hello Azure-régiók, használja az Azure Vnetekhez hello címterület nem ütközhetnek hello a helyszíni hálózat és minden egyes virtuális hálózat által használt hello címterület nem kell más meglévő Vnetek ütközhetnek. Ezek a követelmények használható hello címterek hello tábla toosatisfy alatt.  

| **Előfizetés** | **Virtuális hálózat** | **Az Azure-régió** | **Címtér** |
| --- | --- | --- | --- |
| BU1 |ProdBU1US1 |USA nyugati régiója |172.16.0.0/16 |
| BU1 |ProdBU1US2 |USA keleti régiója |172.17.0.0/16 |
| BU1 |ProdBU1EU1 |Észak-Európa |172.18.0.0/16 |
| BU1 |ProdBU1EU2 |Nyugat-Európa |172.19.0.0/16 |
| BU1 |TestDevBU1 |USA nyugati régiója |172.20.0.0/16 |
| BU2 |TestDevBU2 |USA nyugati régiója |172.21.0.0/16 |
| BU2 |ProdBU2US1 |USA nyugati régiója |172.22.0.0/16 |
| BU2 |ProdBU2US2 |USA keleti régiója |172.23.0.0/16 |
| BU2 |ProdBU2EU1 |Észak-Európa |172.24.0.0/16 |
| BU2 |ProdBU2EU2 |Nyugat-Európa |172.25.0.0/16 |

**Alhálózatokat és az NSG-k száma**

hello követelményeknek kapcsolódó toosubnets és NSG-k:

* Hello mennyiségét virtuális hálózatokat és alhálózatokat kell minimálisra csökkenthető.
* Minden alkalmazás teljesen elkülönülnek egymástól.
* Minden alkalmazás elérhető az ügyfelek által az hello Internet HTTP-n keresztül.
* Minden alkalmazás elérhetők, felhasználók csatlakoztatott toohello helyszíni adatközpontok egy titkosított alagút használatával.
* Kapcsolat tooon helyszíni adatközpontok használjon meglévő VPN-eszközök.
* az egyes helyeken hello adatbázisok kell replikálni tooother Azure helyek naponta egyszer.

Ezek a követelmények alapján, képes használni, egy alkalmazás rétegenként, és az NSG-k toofilter adatforgalom alkalmazásonként. Ezzel a módszerrel csak akkor kell minden egyes virtuális hálózat (előtér alkalmazásréteg és adatrétege) 3 alhálózatai és alkalmazásonként száma alhálózat egy NSG. Ebben az esetben érdemes hello **alkalmazás rétegenként alkalmazásonkénti NSG-ket egy alhálózat** tervezési minta alapján. hello az alábbi ábra hello tervezési minta hello képviselő hello használata **ProdBU1US1** virtuális hálózat.

![Rétegenként rétegenként alkalmazásonként egy NSG több alhálózattal](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Azonban szüksége is toocreate egy külön alhálózatot hello VPN-kapcsolat hello Vnetek, és a helyszíni adatközpont között. És az egyes alhálózatokon toospecify hello cím területre van szükség. hello az alábbi ábra egy minta megoldás **ProdBU1US1** virtuális hálózat. Ebben a forgatókönyvben minden vnet replikálna. A szín jelöli egy másik alkalmazás.

![A minta virtuális hálózat](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Hozzáférés-vezérlés**

hello következő követelményei kapcsolódó tooaccess vezérlő:

* hello vállalati hálózati csoport kell faxrendszergazdáknak teljes hozzáférésük van hello VNet konfigurációját.
* Mindegyik üzleti egység a fejlesztők használatuk csak akkor tudja toodeploy virtuális gépek tooexisting alhálózatokat.

Ezek a követelmények alapján, érdemes felvenni felhasználók a hálózati csoport toohello beépített hello **hálózat közreműködő** minden előfizetésben; szerepkör, és hozzon létre egy egyéni biztonsági szerepkört a hello alkalmazásfejlesztők minden előfizetés kiadása jogok nekik tooadd virtuális gépek tooexisting alhálózatokat.

## <a name="next-steps"></a>Következő lépések
* [Telepíthet egy virtuális hálózatot](virtual-networks-create-vnet-arm-template-click.md) eset alapján.
* Hogyan túl megértéséhez[terheléselosztásához](../load-balancer/load-balancer-overview.md) IaaS virtuális gépeket és [kezelése az útválasztást a több Azure-régiók](../traffic-manager/traffic-manager-overview.md).
* További információ [NSG-ket és hogyan tooplan és tervezési](virtual-networks-nsg.md) egy NSG-megoldáshoz.
* További információ a [létesítmények közötti és VNet kapcsolati lehetőségek](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti).
