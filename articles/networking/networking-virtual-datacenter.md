---
title: "Azure virtuális adatközpont aaaMicrosoft |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild a virtuális adatközpont az Azure-ban"
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jonor
ms.openlocfilehash: 84f77b16edaece202a6a94b6107f1c9585ec7f38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-virtual-data-center"></a>A Microsoft Azure virtuális adatközpont
**A Microsoft Azure**: gyorsabb, költségtakarékosabb munkavégzésben, integrálása a helyszíni alkalmazások és adatok

## <a name="overview"></a>Áttekintés
Az áttelepítést a helyszíni alkalmazások tooAzure, nélkül is jelentős módosításokat (egy, a "növekedési és az eltolás mértékét megadó" néven ismert megközelítés), a szervezetek hello előnyt kínál a biztonságos és költséghatékony infrastruktúra. Azonban toomake legtöbb hello hello agilitást felhőre, lehetséges, a vállalatok az Azure-képességek architektúrák tootake ki kell alakítani. A Microsoft Azure nyújt kapacitású szolgáltatások és a infrastruktúra, a vállalati szintű képességet és a megbízhatóság és a számos választhat való hibrid kapcsolathoz. Az ügyfelek ezeket a cloud services – akár keresztül tooaccess Internet hello, vagy az Azure ExpressRoute kapcsolattal biztosító választhat. hello Microsoft Azure platform lehetővé teszi az ügyfelek tooseamlessly bővítheti az infrastruktúrát hello felhőbe, és hozhat létre többrétegű architektúrák. Emellett Microsoft-partnerek biztonsági szolgáltatásokat kínáló bővített szolgáltatásokat biztosítani, és a virtuális készülékek, amelyek optimalizált toorun az Azure-ban.

Ez a cikk áttekintést mintákat és terveket, a használt toosolve lehet hello architekturális méretezés, a teljesítmény és a biztonsági aggályokon sok ügyfél szembesülhetnek menet masse toohello felhő áthelyezéséről végezni. Hogyan toofit különböző szervezeti az informatikai szerepkörök hello felügyeleti és irányítása a hello rendszer is tárgyalt, hangsúlyt toosecurity követelmények és költségek optimalizálási áttekintése.

## <a name="what-is-a-virtual-data-center"></a>Mi az az Adatközpont virtuális?
A hello korai nap, a felhőalapú megoldások tervezett toohost egyetlen, viszonylag elkülönített, alkalmazások, a hello nyilvános pontszámot volt. Ez a módszer jól néhány évig előre. Azonban hello felhő előnyeit, megoldások nyilvánvalóvá vált, és több nagy méretű munkaterhelés üzemeltetett hello felhő címzési biztonsági, megbízhatóságát, teljesítményét, és valamelyik központi telepítések vonatkozik költség vagy további régiókban létfontosságú teljes hello vált hello felhőszolgáltatás életciklusát.

hello következő felhő telepítési diagram azt ábrázolja néhány példa biztonsági réseinek (vörös téglalappal) és optimalizálási hálózati virtuális készülékeket illetve a munkaterhelések (sárga mezőben) között.

[![0]][0]

hello virtuális adatközpont (vDC) a szükségességét toosupport vállalati munkaterhelések skálázás született, és hello toodeal kell hello problémás vezetett be, amikor nagy méretű alkalmazásokat támogató hello nyilvános felhőben található.

A virtualizált tartományvezérlő nincs csak hello alkalmazások és szolgáltatások hello felhő, de is hello hálózati, biztonsági, felügyeleti és infrastruktúra (például a DNS- és a Directory Services). Általában is biztosít a magánhálózati kapcsolat hátsó tooan a helyszíni hálózati vagy data center. Egyre több munkaterhelések tooAzure mozgatása, akkor fontos toothink infrastruktúrájához és objektumaihoz támogató hello kapcsolatban, amelyek az ilyen terhelések kerülnek. Továbbléphetnek gondosan erőforrások felépítése hogyan elkerülheti a hello elterjedése több száz "munkaterhelés-szigetek", amely független adatfolyam, biztonsággal rendelkező modelljei és megfelelőségi kihívást külön-külön kell kezelni.

Egy virtuális adatközpont alapvetően a közös támogató funkciók, szolgáltatások és az infrastruktúra különálló, de kapcsolódó entitások gyűjteményét. A munkaterhelések megtekintésével, egy integrált virtualizált tartományvezérlő, vegye figyelembe költséghatékony miatt tooeconomies skála optimalizált biztonság az összetevő és az adatok áramlását központosítás könnyebb műveleteit, felügyeleti és megfelelőségi ellenőrzések együtt.

> [!NOTE]
> Fontos, hogy a virtualizált tartományvezérlő hello toounderstand **nem** egy különálló Azure terméket, de különböző szolgáltatásokat és képességeket hello kombinációja túl az igényeknek pontos. Virtualizált tartományvezérlő gondolat a számítási feladatok és az Azure használati toomaximize erőforrásait és képességek hello felhőben módja. hello virtuális tartományvezérlő ezért egy moduláris megközelítés hogyan toobuild hello Azure tiszteletben szervezeti szerepkörök és felelősségek meghatározása az informatikai szolgáltatások.

hello vDC vállalatok munkaterheléseket és alkalmazásokat az Azure lekérése a következő forgatókönyvek hello segítségével:

-   Több kapcsolódó munkaterhelések
-   Az egy a helyszíni környezet tooAzure áttelepítése munkaterhelések
-   Megosztott és központosított biztonsági és a hozzáférési követelmények megvalósítása a munkaterhelések között
-   DevOps és a központi IT vállalat a megfelelő keverése

hello vDC kulcs toounlock hello előnyeit, egy központi topológia (hub és küllők) Azure-szolgáltatások vegyesen: [Azure VNet][VNet], [NSG-k] [ NSG], [Vnetben társviszony-létesítés][VNetPeering], [felhasználó által definiált útvonalak (UDR)][UDR], és az Azure-identitás [ Szerepköralapú hozzáférés-vezérlést (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Kell egy virtuális adatközpont?
Minden Azure ügyfél több, mint az Azure-munkaterhelések néhány továbbléphetnek közös erőforrások használatával is kihasználhatja a toomove igénylő. Attól függően, hogy hello nagyságát sőt az egyedülálló alkalmazás is kihasználhatja a hello minták, és összetevők használt toobuild egy virtualizált tartományvezérlő.

Ha a szervezete egy központi informatikai, hálózati, biztonsági és/vagy a megfelelőségi csoport/szervezeti egység, egy a virtualizált tartományvezérlő segítségével kényszerítése házirend pontok elkülönítése szolgálati, és biztosítása az alapul szolgáló közös összetevők alkalmazásokat fejlesztő csapatoknak együtt, miközben hello szabad és vezérlés, a megfelelő igényeinek.

A szervezeteknek, amelyek tooDevOps hasznosítani nyújthatnak a hello vDC fogalmak tooprovide Azure-erőforrások zsebe engedélyezett, és győződjön meg arról, rendelkeznek az adott csoportban (előfizetés vagy az erőforrás csoport egy közös előfizetésben), teljes ellenőrzés hello hálózati de és biztonsági határokat maradnak megfelelő, egy hub virtuális hálózat és az erőforráscsoport egy központi házirend által definiált konfigurációjának kialakításához.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Szempontok a virtuális adatközpont megvalósítása
A virtualizált tartományvezérlő tervezésekor van több kulcsfontosságú problémák tooconsider:

-   Identitás- és címtárszolgáltatásokat
-   Biztonsági infrastruktúra
-   Kapcsolat toohello felhő
-   Kapcsolat hello felhőben

##### <a name="identity-and-directory-service"></a>*Identitás- és címtárszolgáltatás*
Identitás és szolgáltatások az összes adat kulcsfontosságú erőforrásokból, mind a helyszíni és felhőben hello. Identitás kapcsolódó tooall aspektusainak hozzáférési és engedélyezési tooservices hello vDC belül. toohelp győződjön meg arról, hogy csak a hitelesített felhasználók és a folyamatok férhet hozzá az Azure-fiókjához, és erőforrásokat, az Azure számos különböző típusú hitelesítő adatokat használ a hitelesítéshez. Ezek közé tartozik a jelszavak (tooaccess hello Azure-fiók), a titkosítási kulcsokat, a digitális aláírásokat és a tanúsítványok. [*Az Azure multi-factor Authentication* (MFA)] [ MFA] egy további rétegét biztonsági Azure-szolgáltatások eléréséhez. Az Azure MFA erős hitelesítés számos olyan egyszerű ellenőrzési lehetőséget kínál – a telefonhívás, szöveges üzenetet vagy mobilalkalmazásban megjelenő értesítést – és előnyben részesített toochoose hello módszer lehetővé teszi az ügyfeleknek.

A nagyvállalati toodefine egy identity management folyamat identitásokkal hello kezelését, a hitelesítési, engedélyezési, szerepkörök és jogosultságok belüli vagy hello vDC leíró kell. hello célja ezt a folyamatot, a költség, állásidő és ismétlődő manuális feladatokat csökkenést tooincrease biztonsági és a termelékenység kell lennie.

Vállalat vagy szervezet különböző sor-az-vállalatok (LOB) szolgáltatások kibővített vegyesen lehet szükség, és gyakran alkalmazottaknak különböző szerepkörök esetén, a másik projektet. A virtualizált tartományvezérlő megfelelő együttműködését a különböző csapatok, az adott szerepkör-definíciók, a megfelelő irányítás tooget rendszerekre van szükség. hello mátrix felelősségi, a hozzáférés és a jogok rendkívül összetett feladat lehet. A virtualizált tartományvezérlő Identitáskezelés keresztül valósul [ *Azure Active Directory* (AAD)] [ AAD] és a szerepköralapú hozzáférés-vezérlést (RBAC).

A címtárszolgáltatás egy megosztott információk keresése, kezelése, felügyelete és mindennapi elemek és a hálózati erőforrások rendszerezése infrastruktúra. Ezeket az erőforrásokat kötetek, mappák, fájlok, nyomtatók, felhasználók, csoportok, eszközök és egyéb objektumok lehetnek. Az egyes erőforrások hello hálózaton objektum számít hello címtárkiszolgálóra. Ezzel az erőforrás vagy objektum tartozó attribútumok gyűjteménye erőforrásra vonatkozó adatokat tárolja.

Az összes Microsoft online szolgáltatás támaszkodjon az Azure Active Directory (AAD) a bejelentkezéshez, és más az identitásukat. Az Azure Active Directory egy átfogó, magas rendelkezésre állású, felhőalapú identitás- és hozzáférés-kezelő megoldás, amely ötvözi az alapvető címtárszolgáltatásokat, a fejlett identitáskezelést és az alkalmazáshozzáférés-felügyeletet. Az AAD integrálható a helyszíni Active Directory tooenable egyetlen bejelentkezéshez az összes felhő alapú, és helyileg tárolt (helyszíni) alkalmazásokhoz. hello attribútumait a helyszíni Active Directory automatikusan szinkronizált tooAAD lehet.

Egyetlen globális rendszergazdája nem szükséges tooassign minden engedélyt a virtualizált tartományvezérlő van. Ehelyett minden egyes adott osztály (vagy a felhasználók és a címtárszolgáltatás hello szolgáltatáscsoport) lehet hello engedélyek szükséges toomanage saját erőforrásaikat egy virtualizált tartományvezérlő belül. Szerkezetének kialakítása engedélyek megköveteli a terheléselosztás. Túl sok engedélyek akadályozhatják a teljesítmény hatékonyságát, és túl kevés vagy laza engedélyek növelhető a biztonsági kockázatokat. Azure szerepköralapú hozzáférés-vezérlés (RBAC) a probléma, segít tooaddress részletes hozzáféréskezelést az vDC erőforrások felajánlásával.

##### <a name="security-infrastructure"></a>*Biztonsági infrastruktúra*
Biztonsági infrastruktúra, a virtualizált tartományvezérlő hello környezetében főként kapcsolódó toohello elkülönítése hello vDC adott virtuális hálózati szegmenst, és hogyan toocontrol bemenő és kimenő forgalmat hello virtualizált tartományvezérlő teljes hálózati forgalom. Azure alapul, amely megakadályozza a jogosulatlan és nem szándékos forgalom központi telepítések között több-bérlős architektúrák, használja a virtuális hálózatelkülönítés (VNet), hozzáférés-vezérlési listák (ACL), és töltse be a terheléselosztó és IP-szűrők forgalom folyamata házirendek együtt. Hálózati címfordítás (NAT) elválasztja a belső hálózati forgalmat a külső forgalom.

hello Azure-hálót tootenant munkaterhelések infrastruktúra-erőforrásokat foglal le, és a virtuális gépek (VM) kommunikáció tooand kezeli. hello Azure hipervizor érvénybe lépteti a memória és a folyamat elkülönítése virtuális gépek és biztonságosan útvonalak hálózati forgalom tooguest OS bérlők között.

##### <a name="connectivity-toohello-cloud"></a>*Kapcsolat toohello felhő*
hello vDC kell külső hálózatok toooffer szolgáltatások toocustomers, partnerek és/vagy a belső felhasználók kapcsolatát. Ez általában azt jelenti, kapcsolat nem csak internetes toohello, de tooon helyi hálózatok és adatközpontok is.

Ügyfelek összeállítása a biztonsági házirendek toocontrol mi, valamint hogyan adott vDC üzemeltetett szolgáltatások érhetők el belőle hello internetkapcsolat, és a hálózati virtuális készülékek (a szűrést és a forgalmi ellenőrzési), és egyéni útválasztási házirendek és hálózati szűrési) Felhasználó által definiált Útválasztás és a hálózati biztonsági csoportok).

A vállalatok gyakran kell tooconnect vDCs tooon helyszíni adatközpontok vagy más erőforrásokat. hello kapcsolatra Azure és a helyszíni hálózat között ezért fontos szempont egy hatékony architektúra tervezésekor. A vállalatok egy virtualizált tartományvezérlő és a helyszíni összekapcsolása rendelkezik két különböző módon toocreate Azure: hello interneten keresztül, vagy saját közvetlen kapcsolatot átvitel közben.

Egy [ **Azure telephelyek közötti VPN** ] [ VPN] összekapcsolási szolgáltatásnak a helyszíni hálózat között Internet hello keresztül, és hello virtualizált tartományvezérlő biztonságos keresztül titkosított kapcsolatok száma (alagutak IPsec/IKE). Az Azure-webhelyek kapcsolat rugalmas, gyors toocreate, és nincs szükség semmilyen további beszerzési, mivel keresztül csatlakozó összes kapcsolat hello internet.

[**ExpressRoute** ] [ ExR] az Azure-kapcsolat szolgáltatása, amely lehetővé teszi a virtualizált tartományvezérlő és hello közötti magánhálózati kapcsolatok létrehozása a helyszíni hálózatokban. ExpressRoute kapcsolatok ne lépje több mint hello a nyilvános interneten, és magasabb szintű biztonságra, megbízhatóságra és magasabb lerövidíti (too10 GB/s) együtt konzisztens késés kínálnak. ExpressRoute nagyon hasznos a vDCs, mint ExpressRoute hello előnyeit a megfelelőségi szabályok társított titkos kapcsolatok igényelhető.

ExpressRoute kapcsolatok foglal magában, amelyek révén az ExpressRoute-szolgáltatóhoz. A gyors toostart igénylő ügyfelek azt közös tooinitially felhasználási telephelyek közötti VPN tooestablish kapcsolat hello vDC és helyszíni erőforrások között, majd utána áttelepíteni az tooExpressRoute kapcsolat.

##### <a name="connectivity-within-hello-cloud"></a>*Kapcsolat hello felhőben*
[Vnetek] [ VNet] és [Vnetben társviszony-létesítés] [ VNetPeering] vannak hello alapvető hálózati kapcsolat szolgáltatások egy virtualizált tartományvezérlő belül. Egy Vnetet biztosítja, hogy a virtualizált tartományvezérlő erőforrások elkülönítési természetes határ, és a Vnetben társviszony-létesítés lehetővé teszi a hello belül különböző Vnetek közötti befolyásolása azonos Azure-régiót. Forgalomirányítás egy Vneten belül és között Vnetek kell toomatch biztonsági csoportja szabályok megadva a hozzáférés-vezérlési listái ([hálózati biztonsági csoport][NSG]), [hálózati virtuális készülékek ] [ NVA], és egyéni útválasztási táblázataiba ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Virtuális adatközpont – áttekintés

### <a name="topology"></a>topológia
Hub és küllők modell kiterjesztett hello virtuális adatközpont egyetlen Azure régión belül

[![1]][1]

hello hello központi zónákhoz szabályozza, és megvizsgálja a különböző zónákhoz közötti érkező és/vagy a kilépő forgalmat is: az Internet, a helyszíni és küllők hello. hello küllős topológia egy hatékony módot tooenforce biztonsági házirendek egy központi helyen ad hello IT-részleg hello lehetséges, hogy a hiba a konfiguráció és a kitettség csökkentése során.

hello hub hello közös szolgáltatás-összetevők hello küllők által felhasznált tartalmazza. Az alábbiakban néhány tipikus példája közös központi szolgáltatások:

-   hello Windows Active Directory-infrastruktúrát (hello az AD FS szolgáltatás kapcsolatos) a felhasználók hitelesítését a harmadik felek számára nem megbízható hálózatokon lévő első hozzáférés toohello munkaterhelések hello küllős előtt eléréséhez szükséges
-   A DNS szolgáltatás tooresolve elnevezési hello munkaterhelés hello küllők, tooaccess erőforrásokhoz a helyszínen és a hello Internet
-   A nyilvános kulcsokra ÉPÜLŐ infrastruktúra tooimplement egyszeri bejelentkezést a munkaterhelések
-   Folyamatvezérlés (TCP/UDP) hello küllők és az Internet között.
-   Hello küllős és a helyszíni közötti adatfolyam vezérlés
-   Ha szükséges, egy küllős közötti folyamatvezérlés

hello vDC csökkenti a teljes költség szempontjából között több küllők hello megosztott hub-infrastruktúra használatával.

minden egyes küllős hello szerepe toohost munkaterhelések különböző típusú lehet. hello küllők is biztosíthat a moduláris megközelítés az ismétlődő telepítések (például a fejlesztéshez és teszteléshez, felhasználói tesztelés az üzem előtti és éles) hello az azonos munkaterhelések. hello küllők is használt toosegregate és különböző csoportokat a szervezetben (például DevOps csoportok) engedélyezése. Egy küllős belül egy alapszintű munkaterhelés vagy összetett többrétegű munkaterhelések adatforgalom szabályozása hello rétegek közötti lehetséges toodeploy.

##### <a name="subscription-limits-and-multiple-hubs"></a>Előfizetési korlátozásait és több hubok
Azure-ban minden összetevő, bármilyen hello típusú van telepítve az Azure-előfizetéssel. az Azure-előfizetések az Azure összetevők hello elkülönítési is megfelelnek a hello különböző LOB, például a hozzáférési és engedélyezési szinteket beállítása.

Bár minden informatikai rendszer rendelkező vannak korlátai platformok költenie toolarge száma küllők, egyetlen vDC. hello központi telepítésnél: kötött tooa adott Azure-előfizetéssel, amelynek korlátozások és korlátozások (lásd például a Vnetben társviszony - maximális száma: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések] [ Limits] részletekért). Azokban az esetekben, ahol korlátokat lehet, hogy egy probléma hello architektúra méretezhető akár további hub és küllők egyetlen hub-küllők tooa fürtről hello modell kiterjesztésével. Egy vagy több Azure-régiók több központok az Express Route vagy telephelyek közötti VPN használatával kell egymáshoz.

[![2]][2]

hello több hubok hello költségek és felügyeleti elérhető hello rendszer növeli és méretezhetőség csak akkor indokolt (példák: rendszer korlátok vagy redundancia) és a regionális replikációs (példák: végfelhasználói teljesítmény- vagy katasztrófa utáni helyreállítás). Több hubok igénylő forgatókönyvek, az összes hello hubok törekedni kell toooffer hello azonos szolgáltatáskészletet működési megkönnyítése érdekében.

##### <a name="interconnection-between-spokes"></a>Küllők összekapcsolása
Egy egyetlen küllős belül is lehetséges tooimplement összetett több rétegek munkaterhelések. Többrétegű konfigurációk hello ugyanazt a virtuális hálózatot és szűrési hello zajlik az NSG-k használata az alhálózatok (egy minden szinthez) használatával valósítható meg.

A hello ugyanakkor, egy toodeploy egy többrétegű munkaterhelés érdemes több Vnetek között. Hálózatokból használva küllők kapcsolódhatnak a hello tooother küllők ugyanabban a központban vagy különböző hubok. Ebben a forgatókönyvben például helyzet hello feldolgozási alkalmazáskiszolgálók belül hol áll egy küllős (VNet), miközben hello adatbázis telepítve van egy másik küllős (VNet). Ebben az esetben egyszerűen toointerconnect hello küllők a Vnetben társviszony-létesítés és ezáltal a hello hub áthaladó elkerülése. Gondos architektúra és biztonsági felülvizsgálat hello hub megkerülése nem elkerülő fontos biztonsági vagy naplózási pontokat, előfordulhat, hogy csak létezik hello központban elvégzett tooensure kell lennie.

[![3]][3]

Küllők is lehet, hogy központjaként összekapcsolt tooa küllős. Ez a megközelítés létrehoz egy kétszintű hierarchia: hello küllős hello magasabb szintű (0. szint) lesz hello hub a hello hierarchia alacsonyabb küllők (1. szint). a virtualizált tartományvezérlő hello küllők kell tooforward hello forgalom toohello központi helyet alakíthatnak tooreach kimenő toohello a helyi hálózaton vagy az interneten. Két szintű központ architektúrája vezet be, összetett útválasztási, amely egy egyszerű hub-küllős kapcsolat hello előnyei eltávolítja.

Bár az Azure lehetővé teszi, hogy a komplex topológiákba, hello alapelvek hello vDC fogalmi egyik ismételhetőség és az egyszerűség kedvéért. toominimize energiabefektetést igénylő felügyelet, hello egyszerű hub-küllős kialakítása esetén ajánlott a virtualizált tartományvezérlő referencia-architektúrában hello.

### <a name="components"></a>Összetevők
Egy virtuális adatközpont négy alapvető összetevőtípus épül fel: **infrastruktúra**, **szegélyhálózat**, **munkaterhelések**, és **figyelés**.

Minden egyes összetevő típusa különböző Azure-szolgáltatások és erőforrások áll. A virtualizált tartományvezérlő tevődik össze többféle összetevők és hello több változatait azonos összetevő típusa. Például előfordulhat, hogy sok, amelyek megfelelnek a különböző alkalmazások különböző, logikailag elkülönített, a munkaterhelés-példány. Ezek különböző összetevő típusát használhatja, és példányok tooultimately hello vDC hozhat létre.

[![4]][4]

hello előző magas szintű egy virtualizált tartományvezérlő architektúrája látható különböző zónákhoz hello hub-küllők topológia használható különböző összetevő típusok. hello ábrán infrastruktúra-összetevőihez hello architektúra különböző részein.

Jó gyakorlat (egy helyi tartományvezérlő vagy a virtualizált tartományvezérlő) teszik lehetővé a hozzáférést és jogosultságokkal kell biztonságicsoport-alapú. Foglalkozó csoportok, ne pedig egyéni felhasználóknak segít a hozzáférési házirendek karbantartására következetesen csoportok és segédeszközök minimalizálja a konfigurációs hibák közötti. Hozzárendelés, illetve eltávolítása felhasználók tooand megfelelő csoportok segítségével egy adott felhasználó jogosultságával hello frissítése.

Minden egyes szerepkör csoporthoz lehetnek egy egyedi előtag a nevük, így könnyen tooidentify, mely alkalmazások és szolgáltatások mely csoport tartozik. Például egy olyan hitelesítési szolgáltatás üzemeltető munkaterhelés rendelkezhet nevű csoport *AuthServiceNetOps, AuthServiceSecOps, AuthServiceDevOps és AuthServiceInfraOps.* Hasonlóképpen a központosított szerepkör vagy szerepkörök nem tooa adott szolgáltatásokhoz kapcsolódó, "Corp", az tartalmazni sikerült *CorpNetOps* például.

Számos szervezet használ a csoportok tooprovide szerepkörök fő részletes információkat a következő hello változata:

-   Hello *központi IT-részleg (vállalati)* hello tulajdonosi jogok toocontrol (például a hálózati és biztonsági) összetevőket, és ezért toohave hello szerepe hello előfizetés közreműködőjének kell (és rendelkezik ellenőrzése hello hub) és a hálózati hello küllők közreműködői jogokat. Nagy szervezet gyakran e felügyeleti feladatokat, többek között a; több csapat közötti megosztása a hálózati műveletek (CorpNetOps) csoport (kizárólagos összpontosít a hálózatkezelés) és a biztonsági műveletek (CorpSecOps) csoport (tűzfal és a biztonsági házirend felelős). Ebben az esetben két különböző csoportok ezen egyéni szerepkörök hozzárendelése készült toobe kell.
-   Hello *fejlesztői & Tesztcsoport (AppDevOps)* hello felelősségi toodeploy munkaterhelések (alkalmazások vagy szolgáltatások) rendelkezik. Ez a csoport időt vesz igénybe, hello a virtuális gép közreműködő IaaS központi telepítések, illetve egy vagy több PaaS közreműködői szerepkört (lásd: [átruházásához hozzáférés-vezérlés beépített szerepkörök][Roles]). Opcionálisan a fejlesztői hello & teszt csoport esetleg biztonsági házirendek (NSG-k) és útválasztási házirend (UDR) belül hello hub vagy egy adott küllős toohave láthatóságát. Ezért a munkaterhelések közreműködői toohello szerepkörök hozzáadását, ennek a csoportnak kell hello szerepkör a hálózati olvasó.
-   Hello *és karbantartása csoport (CorpInfraOps vagy AppInfraOps)* hello felelős az éles környezetben munkaterhelések kezeléséhez. Ez a csoport toobe egy éles előfizetése munkaterhelések előfizetés közreműködőjének kell. Egyes szervezetek is előfordulhat, hogy kiértékelheti, hogy egy további eszkalációs támogatási csapatának csoportja hello szerepkörrel rendelkező előfizetés közreműködői éles és hello központi helyet alakíthatnak az előfizetéshez, sorrendben toofix lehetséges konfigurációs problémák hello éles környezetben van szükségük környezet.

A virtualizált tartományvezérlő van felépítve, hogy a hello hello hub kezelése központi informatikai csoportok számára létrehozott csoportok megfelelő csoportok hello munkaterhelés szinten. Ezenkívül toomanaging hub erőforrások csak hello központi informatikai csoportok lenne képes toocontrol külső hozzáférés és a legfelső szintű engedélyek hello az előfizetésben. Munkaterhelés-csoport azonban lenne képes toocontrol erőforrások és a virtuális hálózat, egymástól függetlenül a központi IT engedélyeit.

hello vDC igényeinek toobe toosecurely gazdagép több projektek között particionált különböző sor-az-vállalatok (LOB). Minden projekt különböző elkülönített környezetek (fejlesztői, ellenőrzését, éles) szükséges. Az egyes ezekben a környezetekben külön Azure-előfizetések természetes elkülönítést biztosítani.

[![5]][5]

hello fenti ábrán hello közötti kapcsolatot mutatja be egy szervezet projekteket, felhasználók, csoportok és hello környezetekben, ahol hello Azure összetevő van telepítve.

Általában az informatikai környezetben (vagy réteg), amelyben több alkalmazás telepített és hajtotta végre a rendszer. A nagyobb vállalatok használja a fejlesztési környezetet (ahol módosításait eredetileg és tesztelt) és egy éles környezetben (mi végfelhasználók használja). Adott környezetben választják el egymástól, gyakran több átmeneti környezet Between őket fokozatosan tooallow deployment (Bevezetés), a tesztelés és visszaállítási problémák. Központi telepítés architektúrák jelentős eltérések lehetnek, de általában hello alapvető folyamatainak fejlesztési (fejlesztés) kezdődő, és a záró dátum (termék) éles továbbra is követi.

Egy közös architektúra az ilyen típusú többrétegű környezetek DevOps (fejlesztéshez és teszteléshez), a UAT (átmeneti) és a termelési környezetben áll. A szervezetek kihasználhatják a egyetlen vagy több Azure AD bérlők toodefine hozzáférés és a jogok toothese környezetekben. hello előző ábrán látható egy esetben ha két különböző az Azure AD bérlők használják: egy a DevOps és ellenőrzését, és kizárólag a termelési hello.

hello jelenléte másik Azure ad bérlők érvénybe lépteti hello elkülönítése különböző környezetek között. hello (például központi informatikai) ugyanazt a felhasználói csoportot kell egy másik URI tooaccess egy másik AD-bérlő segítségével tooauthenticate hello szerepkörök vagy módosításával vagy hello DevOps engedélyei vagy üzemi környezetben a projekt. különböző felhasználói hitelesítési tooaccess különböző környezetek hello jelenléte csökkenti a lehetséges kimaradások és egyéb emberi hibák által okozott problémákat.

#### <a name="component-type-infrastructure"></a>Összetevő típusa: infrastruktúra
Ez az összetevő típus támogató infrastruktúra hello többségét tartalmazó. Is ahol a központi IT, biztonsági, megfelelőségi csapat a legmagasabbak az az idő, illetve.

[![6]][6]

Infrastruktúra-összetevőihez egy összekapcsolása egy virtualizált tartományvezérlő hello különböző összetevői közötti nyújtanak, és hello hub és hello küllők is szerepel. hello kezelése és hello infrastruktúra-összetevőihez karbantartásáért felelős lett rendelve toohello központi informatikai és/vagy biztonsági csoporttal.

Hello elsődleges feladatai hello informatikai infrastruktúra csapat egyik IP-cím sémák tooguarantee hello konzisztenciájának hello vállalaton belül. hello privát IP cím hozzárendelt hely toohello vDC kell toobe konzisztens, és nem átfedésben vannak hozzárendelve a helyszíni hálózatokon magánhálózati IP-címek.

Során NAT hello a helyszíni peremhálózati útválasztók, vagy az Azure-környezetek IP-címütközés elkerülése érdekében hozzáadja komplikációk tooyour infrastruktúra-összetevőihez. Egyszerű kezelés a egyike hello főbb célok a virtualizált tartományvezérlő, így NAT toohandle IP aggályokat használata nem ajánlott megoldás.

Infrastruktúra-összetevőihez hello a következő funkciókat tartalmazza:

-   [**Identitás- és a directory services**][AAD]. Hozzáférés tooevery erőforrástípus az Azure-ban tárolja a címtárszolgáltatások identitás vezérli. hello directory szolgáltatás tárolók nemcsak hello azoknak a felhasználóknak, de is hello hozzáférési jogok tooresources egy adott Azure-előfizetésben. Ezeket a szolgáltatásokat is létezik, csak felhőalapú, vagy azokat az Active Directoryban tárolja, a helyszíni identitás szinkronizálhatók.
-   [**Virtuális hálózati**][VPN]. Virtuális hálózatok a virtualizált tartományvezérlő fő összetevői, és a forgalom elkülönítési határt alkotnak, az Azure platformon hello toocreate engedélyezése. Virtuális hálózat egy vagy több virtuális hálózati szegmensen, az egy adott IP-hálózati előtag (alhálózat) áll. Virtuális hálózati hello egy belső szegélyhálózati területre, ahol IaaS virtuális gépek és PaaS szolgáltatások hozhatnak létre saját kommunikációs határozza meg. Virtuális gépek (és PaaS szolgáltatások) egy virtuális hálózat közvetlen tooVMs (és PaaS szolgáltatások) egy másik virtuális hálózatban, még akkor is, ha mindkét virtuális hálózat által létrehozott hello azonos felhasználói, a kommunikáció hello ugyanahhoz az előfizetéshez. Elkülönítési kritikus tulajdonság, amely biztosítja a felhasználói virtuális gépeket, kommunikációs titkos virtuális hálózaton belül marad.
-   [**UDR**][UDR]. Virtuális hálózati adatforgalom hello rendszer útválasztási táblázat alapján alapértelmezés szerint történik. Felhasználó megadása útvonal egy egyéni útválasztási táblázatban, hogy a hálózati rendszergazdák társítása tooone vagy hello rendszer útválasztási táblázat további alhálózatokat toooverwrite hello működését, és meg a kommunikációs elérési utat a virtuális hálózaton belül. hello jelenléte udr-EK biztosítja, hogy az, hogy a kimenő forgalom a meghatározott egyéni virtuális gépek és/vagy a virtuális hálózati berendezések és a jelen hello hub és hello küllők terheléselosztók hello küllős átvitel.
-   [**NSG**][NSG]. Hálózati biztonsági csoport, amely a szűrést az IP-adatforrások, IP-cél, protokollok, IP-forrás portok és IP-cél portok forgalom összekötőként biztonsági szabályok listáját. hello NSG lehet alkalmazott tooa alhálózati, egy Azure virtuális Gépen, vagy mindkettőt társított virtuális hálózati kártya. hello NSG-ket a megfelelő forgalomszabályozás hello hub és hello küllők alapvető tooimplement. hello hello NSG által biztosított biztonsági szintje, nyisson meg portokat, és milyen célból. Az ügyfelek kell további-a virtuális Gépenkénti szűrők állomásalapú tűzfalat IPtables például vagy a Windows tűzfal hello.
-   **DNS**. hello névfeloldás erőforrások hello Vnetek egy virtualizált tartományvezérlő, DNS-en keresztül valósul meg. hello hello alapértelmezett DNS névfeloldását hatóköre korlátozott toohello virtuális hálózat. Általában egy egyéni DNS-szolgáltatás telepítése hello központban közös szolgáltatások részeként toobe van szüksége, de a DNS-szolgáltatások fő fogyasztóinak hello hello küllős találhatók. Ha szükséges, az ügyfelek DNS hierarchikus tudja létrehozni a DNS-zónák toohello küllők delegálását.
-   [** Előfizetés] [ SubMgmt] és [erőforráscsoport felügyeleti][RGMgmt]**. Előfizetés az erőforrások több csoport definiál egy természetes határ toocreate az Azure-ban. Előfizetés az erőforrások erőforráscsoportok nevű logikai tárolókban lévő együtt tartanak. Erőforráscsoport hello egy logikai csoport tooorganize hello erőforrásainak egy virtualizált tartományvezérlő jelöli.
-   [**AZ RBAC**][RBAC]. Szerepalapú, keresztül lehetséges toomap szervezeti szerepekhez együtt jogok tooaccess adott Azure-erőforrások, így Ön toorestrict felhasználók tooonly műveletek egy bizonyos részét is. Az RBAC hello megfelelő szerepkört toousers, csoportok és alkalmazások hello vonatkozó hatókörén belül hozzárendelése szerint engedélyezheti a hozzáférést. szerepkör-hozzárendelés hello hatóköre lehet Azure-előfizetéssel, egy erőforráscsoport vagy egy erőforrást. Az RBAC lehetővé teszi, hogy az engedélyek öröklődése. Egy szülő hatókörben szerepkörrel is hozzáférést biztosít az abban szereplő toohello gyermekei. Szerepalapú használ, elkülönítse a feladatokat, és hozzáférési toousers csak hello összegét adja meg, hogy be kell tooperform a munkájukat. Például használja az RBAC toolet egy alkalmazott felügyelni a virtuális gépeket egy előfizetésben, amíg egy másik SQL-adatbázisok kezelhető belül hello ugyanahhoz az előfizetéshez.
-   [**VNet-társviszony létesítése –**][VNetPeering]. hello használt alapvető szolgáltatás toocreate hello infrastruktúráját a virtualizált tartományvezérlő Vnetben társviszony-létesítést, egy olyan mechanizmus, amely összeköti a két virtuális hálózatokról (Vnetekről) hello ugyanabban a régióban hello Azure adatközpont hello a hálózaton keresztül.

#### <a name="component-type-perimeter-networks"></a>Összetevő típusa: Szegélyhálózat
[Szegélyhálózaton] [ DMZ] összetevők (más néven DMZ-hálózat) lehetővé teszik tooprovide hálózati kapcsolat a helyszíni vagy a fizikai adatközpont-hálózatot, bármely a hello Internet kapcsolat tooand együtt. Akkor is ahol a hálózati és biztonsági csoportok, valószínűleg a legmagasabbak az időt.

Bejövő csomagok hello küllők hello háttér-kiszolgálók elérése előtt kell áramlása hello biztonsági készülékek hello tűzfal, Azonosítók és IP-CÍMEK, például a hello központban. Internet-kötött csomagok hello munkaterhelések is kell áramlása hello biztonsági készülékek hello peremhálózati a házirendek betartatását, vizsgálati és naplózási célokra, mielőtt elhagynák a hello hálózati.

Szegélyhálózati hálózati összetevők hello a következő szolgáltatásokat biztosítja:

-   [Virtuális hálózatok][VNet], [UDR][UDR], [NSG][NSG]
-   [Virtuális hálózati készülékek][NVA]
-   [Terheléselosztó][ALB]
-   [Alkalmazásátjáró][AppGW] / [WAF][WAF]
-   [Nyilvános IP-címek][PIP]

Általában hello központi informatikai és biztonsági csoportok követelmény definíciója és hello szegélyhálózat működésére.

[![7]][7]

hello előző ábrán látható hello kényszerítési, az access toohello két kialakítását internet és egy helyszíni hálózati, mindkettő rezidens hello központban. Egy központi, a hello szegélyhálózati hálózati toointernet költenie toosupport nagyszámú LOB, több farmok webalkalmazási tűzfalak (WAFs) és/vagy a tűzfalakat használ.

[**Virtuális hálózatok** ] [ VNet] hello hub általában épülő egy Vnetet több alhálózatok toohost hello különböző típusú szolgáltatásokat szűrés, és tanulmányozza a forgalom tooor hello az interneten keresztül NVAs, WAFs, és az Azure Alkalmazásátjárót.

[**UDR** ] [ UDR] használatával UDR központi telepítését a tűzfalakra, Azonosítók/IP-címet, és más virtuális készülékek és a hálózati forgalom e biztonsági készülékek a biztonsági határ házirend betartatása keresztül naplózás, és a vizsgálat. Mindkét hello hub és hello küllők tooguarantee, amely a forgalom továbbítására keresztül hello meghatározott egyéni virtuális gépek, virtuális hálózati berendezéseket és terheléselosztók hello virtualizált tartományvezérlő által használt udr-EK hozhatók létre. tooguarantee, amely a forgalom megtalálhatók hello küllős átvitel toohello megfelelő virtuális készülékek a virtuális gépek jönnek létre, egy UDR kell toobe beállítani a hello küllős hello alhálózatain hello előtérbeli IP-cím hello belső terheléselosztó hello a next-hop. belső terheléselosztó hello hello belső forgalom toohello virtuális készülékek (betöltés terheléselosztó háttér-készletből) továbbítja.

[![8]][8]

[**Virtuális készülékekre** ] [ NVA] hello központban hello internet általában kezelhetők a farmhoz, tűzfalak, illetve a webalkalmazási tűzfalak (WAFs) hozzáférési toohello a szegélyhálózaton.

Különböző LOB a gyakran használt sok webes alkalmazásokhoz, és ezeket az alkalmazásokat az egyes toosuffer különböző biztonsági réseket és a potenciális biztonsági réseket. Webes alkalmazások tűzfalak egy különös fajta termék toodetect támadások elleni webalkalmazások (HTTP/HTTPS) használt általános tűzfal mint mélyebben. Összehasonlítja hagyomány tűzfal technológia, WAFs rendelkeznek funkciók tooprotect belső webkiszolgálók fenyegetések ellen.

A tűzfal farmhoz egy csoport tűzfalak hello hello küllők, és a vezérlés hozzáférési tooon helyszíni hálózatokban tárolt azonos közös felügyeleti, biztonsági szabályok tooprotect hello munkaterhelések vannak beállítva a párhuzamosan működik. A tűzfal farmhoz rendelkezik kisebb kifejezetten egy WAF képest szoftver, de a széles körű alkalmazások toofilter hatókörét, és vizsgálja meg a hálózati forgalom kilépő és belépő típussal rendelkezik. Tűzfal farmok általában segítségével valósíthatók meg az Azure-hálózat virtuális készülékeket (NVAs) hello Azure piactéren elérhető.

Ajánlott toouse egy készletét NVAs a hello Internet származó forgalmat, és egy másik származó forgalmat a helyszíni. Biztonsági kockázatot jelent, csak egy NVAs használatával is, nincs biztonsági szegélyhálózati hello a hálózati forgalom két készlet között biztosít. Külön NVAs használata csökkenti a biztonsági szabályok keresése hello összetettsége, és teszi, hogy mely szabályokat megfelelnek toowhich bejövő hálózati kérelem törölje.

A nagyobb vállalatok több tartományok kezelése. Az Azure DNS használt toohost hello DNS-rekordok az egyes tartományokhoz tartozó lehet. Példa hello Virtual IP Address (VIP) hello Azure külső terheléselosztó (vagy hello WAFs) regisztrálni lehet az Azure DNS-rekord hello rekorddal.

[**Az Azure terheléselosztó** ] [ ALB] Azure terheléselosztó kínál a magas rendelkezésre állású réteg 4 (TCP, UDP) szolgáltatást, amely egy elosztott terhelésű készlet definiált szolgáltatáspéldányok között bejövő forgalom elosztását. A küldött forgalmat toohello terheléselosztó előtér-végpontok (nyilvános IP-végpontok vagy privát IP-végpontok) a szabadon terjeszthető, vagy anélkül tooa fordításgyűjtemény cím háttér IP-címkészlet (példák folyamatban; Virtuális hálózati berendezések vagy virtuális gépeken).

Az Azure terheléselosztó is mintavételi a hello hello állapotát, valamint a különböző kiszolgálópéldányok, és ha a vizsgálatok toorespond hello terhelés terheléselosztó leállítja toohello nem megfelelő állapotú példány forgalom küldése sikertelen. A virtualizált tartományvezérlő, a van egy külső terheléselosztó hello jelenléte hello hub (például egyenleg hello forgalom tooNVAs), és a hello küllők (tooperform feladatokat, például egy többrétegű alkalmazást a különböző virtuális gépek közötti forgalmat terheléselosztás).

[**Alkalmazásátjáró** ] [ AppGW] Microsoft Azure Application Gateway egy dedikált virtuális készülék biztosító alkalmazás kézbesítési vezérlő (LÉPETT) szolgáltatásként, az ajánlat különböző réteg 7 terheléselosztás az alkalmazás lehetőségeit. Lehetővé teszi toooptimize webkiszolgáló farm termelékenység kiürítésével a CPU intenzív SSL lezárást toohello Alkalmazásátjáró. Emellett biztosítja az egyéb réteg 7 útválasztási lehetőségeit, köztük a bejövő forgalmat, a munkamenet cookie-alapú kapcsolat, a URL-cím elérési út-alapú útválasztási és hello képességét toohost ciklikus multiplexelés terjesztési mögött egyetlen alkalmazás átjáró több webhelyek. Webalkalmazási tűzfal (WAF) is megadva hello Alkalmazásátjáró WAF SKU részeként. A Termékváltozat tooweb alkalmazások közös webes biztonsági rések és biztonsági rések védelmet biztosít. Az Application Gateway szolgáltatást internetes átjáróként, csak belső használatú átjáróként vagy a kettő kombinációjaként lehet konfigurálni. 

[**Nyilvános IP-címek** ] [ PIP] meg tooassociate szolgáltatás végpontok tooa nyilvános IP-címet, amely lehetővé teszi a tooyour erőforrás toobe elérhető az egyes Azure szolgáltatások engedélyezése az internet hello. Ehhez a végponthoz hello Azure-beli virtuális hálózat hálózati címfordítás (NAT) tooroute forgalom toohello belső címet és portot használja. Ez az elérési út hello elsődleges módja a külső forgalom toopass hello virtuális hálózathoz. hello nyilvános IP-címek mely forgalom átadott konfigurált toodetermine, és hogyan és hol toohello virtuális hálózaton lefordítva.

#### <a name="component-type-monitoring"></a>Összetevő típusa: figyelése
Figyelési összetevők adjon meg látható, és az összes riasztás hello egyéb összetevők típusú. Az összes csoport hozzáférés toomonitoring hello összetevők kell lennie, és szolgáltatások hozzáférhetnek. Ha egy központi súgó ügyfélszolgálat vagy a műveletek csapatok, ezek az összetevők által előállított integrált toohave hozzáférés toohello adatokat kell.

Naplózás és figyelés szolgáltatások tootrack hello viselkedését Azure különböző típusú Azure ajánlatok üzemeltetett erőforrásokhoz. Cégirányítási és vezérelhető a munkaterhelések az Azure-ban alapján csak a gyűjtését naplóadatokat, hanem hello képességét tootrigger műveletek adott jelentett események alapján.

A naplók az Azure-ban két fő típusa van:

-   [**Tevékenységi naplóit** ] [ ActLog] (említett is, mint a "Műveleti napló") a hello Azure-előfizetés az erőforrások végrehajtott műveletek hello betekintést nyújtanak. Ezek a naplók hello vezérlő-vezérlősík eseményeket az előfizetések jelentést. Minden Azure-erőforrás naplókat hoz létre.

-   [**Az Azure diagnosztikai naplók** ] [ DiagLog] hello művelet erőforrás gazdag, gyakori adatait adja meg az erőforrás által létrehozott naplók. Ezek a naplók tartalmának hello erőforrástípusok szerint változik.

[![9]][9]

Egy virtualizált tartományvezérlő hogy a rendszer rendkívül fontos tootrack hello NSG-ket naplókat, különösen ezt az információt:

-   [**Eseménynaplók**][NSGLog]: bemutatja, milyen NSG-szabályok a következők alkalmazott tooVMs és példány szerepkörök a MAC-címe alapján.
-   [**A teljesítményszámláló naplók**][NSGLog]: követi nyomon, hogy hányszor minden NSG lett hajtva toodeny szabály vagy -kezelési forgalom engedélyezése.

Összes naplófájlt is tárolható Azure Storage-fiókok, a naplózási, statikus elemzési vagy a biztonsági másolat létrehozása céljából. Amikor hello naplók az Azure storage-fiókok vannak tárolva, az ügyfelek használhatják-e a különböző keretrendszerek tooretrieve, előkészítése, elemzése, valamint az adatok tooreport hello állapotát és a felhőben lévő erőforrások állapotának megjelenítése.

A nagyobb vállalatok kell már szerezték be egy szabványos keretrendszert a figyelés a helyszíni rendszer és kiterjesztheti, hogy a keretrendszer toointegrate naplók elő alapú telepítések. Olyan szervezeteknek, amelyek minden hello hello felhőben naplózás tookeep kívánja [a Microsoft Operations Management Suite (OMS)] [ OMS] kiváló választás. Mivel az OMS felhőalapú szolgáltatás, gyorsan és az infrastruktúra-szolgáltatásokra fordított minimális mértékű befektetéssel üzembe helyezhető. OMS is integrálható a System Center-összetevők, például a System Center Operations Manager tooextend a meglévő felügyeleti beruházások hello felhőbe.

OMS naplóelemzési hello OMS keretrendszer toohelp összetevője gyűjtése, korrelálja, a Keresés és a napló és a teljesítményadatokat a törvény által létrehozott operációs rendszerek, alkalmazások, a felhőalapú infrastruktúra összetevőinek. Az ügyfelek biztosít a valós idejű operational insights szolgáltatással integrált keresés és egyéni irányítópultok tooanalyze összes hello rekordok használatával egy virtualizált tartományvezérlő a munkaterhelések között.

#### <a name="component-type-workloads"></a>Összetevő típusa: munkaterhelések
Munkaterhelés-összetevők, ahol a tényleges alkalmazásokhoz és szolgáltatásokhoz találhatók. Akkor is ahol az alkalmazást a fejlesztési csapat legmagasabbak az időt.

hello munkaterhelés lehetőségek valóban végtelen. hello az alábbiakban néhány olyan hello lehetséges számítási feladattípust:

**Belső ÜZLETÁGI alkalmazások**

Az üzletági alkalmazások számítógép alkalmazások kritikus toohello folyamatban lévő műveletet a vállalatok. LOB-alkalmazások néhány közös jellemzőkkel rendelkezik:

-   **Interaktív**. LOB-alkalmazások rendszer természetüknél interaktív: adatok, és a visszaadott eredmény/jelentéseket.
-   **Adatalapú**. LOB-alkalmazások olyan gyakori hozzáférési toohello adatbázisok vagy egyéb tárolási intenzív adatok.
-   **Integrált**. LOB-alkalmazások ajánlat integráció más rendszerekkel hello szervezeten kívül vagy belül.

**Ügyfelek által használt (Internet vagy belső hozzáférhető) webhelyek** kommunikáló hello Internet legtöbb alkalmazásokat olyan webhelyek. Az Azure kínál hello funkció toorun az infrastruktúra-szolgáltatási virtuális gép vagy a webhely egy [Azure Web Apps] [ WebApps] hely (PaaS). Az Azure Web Apps támogatja, amelyek lehetővé teszik a hello küllős egy virtualizált tartományvezérlő a Web Apps hello hello telepítését Vnetek integrálását. Hello virtuális integráció, az Internet végpont tooexpose nem kell az alkalmazások, de használhat hello erőforrások titkos nem internetes irányítható címet a személyes vnet helyette.

**A big Data/Analytics** Ha adatokat tooa nagyon nagy méretű kötet tooscale, adatbázisok előfordulhat, hogy nem vertikális felskálázás megfelelően. Hadoop-technológia kínál a rendszer csomópontok nagy száma párhuzamos toorun elosztott lekérdezések. A felhasználóknak kell hello beállítás toorun data-számítási feladatok IaaS virtuális gépeket vagy PaaS ([HDInsight][HDI]). HDInsight támogatja az olyan a helyalapú Vneten üzembe helyezve, egy küllős hello virtualizált tartományvezérlő, a telepített tooa tárolófürt is lehet.

**Események és üzenetekkel**
[Azure Event Hubs] [ EventHubs] kapacitású telemetriai adatfeldolgozást szolgáltatás, amely összegyűjti, átalakítja, és több millió esemény tárolja. Elosztott adatfolyam platformként azt biztosít alacsony késéssel és konfigurálható idő megőrzési, így lehetővé teszi nagy mennyiségű tooingest telemetriai adatokat az Azure és, hogy adatokat olvasni több alkalmazást. Az Event Hubs egy egyetlen adatfolyam valós időben és kötegelt alapú folyamatok is támogatja.

Üzenetküldési szolgáltatás között alkalmazásokat és szolgáltatásokat, nagymértékben megbízható felhő keresztül valósítható [Azure Service Bus] [ ServiceBus] , hogy aszinkron ajánlatok közvetítőalapú üzenetküldési ügyfél és kiszolgáló között valamint strukturált első-first out (FIFO) üzenetküldési és közzétételi/előfizetési képességeket.

[![10]][10]

### <a name="multiple-vdc"></a>Több virtualizált tartományvezérlő
Ebben a cikkben eddig egy virtualizált tartományvezérlő hello alapvető összetevői, és hogy járulnak hozzá tooa rugalmas virtualizált tartományvezérlő architektúrája leíró fordította. Azure-szolgáltatások például az Azure load balancer, NVAs, rendelkezésre állási csoportok esetében méretezési csoportok együtt más mechanizmusok, amelyek lehetővé teszik a termelési szolgáltatás toobuild teli SLA szintek tooa rendszer függ.

Azonban egyetlen vDC egyetlen régión belül található, és sebezhető toomajor kimaradás, amelyek hatással lehetnek a teljes régió. Szeretné, hogy tooachieve magas SLA szükséges tooprotect hello szolgáltatások kezelésében a hello megegyezik a két (vagy több) vDCs helyezni különböző régiókban projekt központi telepítéséhez.

Továbbá tooSLA problémákat nincsenek számos gyakori forgatókönyvek, ahol üzembe helyezése több vDCs szabálykészletében:

-   Területi/globális jelenlét
-   Vészhelyreállítás
-   Mechanizmus toodivert forgalom tartományvezérlő között

#### <a name="regionalglobal-presence"></a>Területi/globális jelenlét
Azure-adatközpont a világ számos régióban. Ha több Azure-adatközpont választja, a felhasználóknak kell tooconsider két kapcsolódó tényezők: földrajzi távolság és késés mellett. A felhasználóknak kell tooevaluate hello földrajzi közötti távolságot hello vDCs hello távolsága hello vDC hello felhasználó toooffer hello lehető legjobb felhasználói élmény.

hello Azure azon régióját vDCs a rendszer hol tárolja a bármely jogi joghatóság, amely alatt a szervezet működik által meghatározott szabályozási követelmények tooconform is szükséges.

#### <a name="disaster-recovery"></a>Vészhelyreállítás
a vész-helyreállítási terv hello végrehajtásának erősen kapcsolódó toohello típusú érintett, de hello képességét toosynchronize hello munkaterhelés állapot különböző vDCs között. Ideális esetben a legtöbb ügyfél szeretné toosynchronize alkalmazásadatok két különböző vDCs tooimplement gyors feladatátvétel mechanizmus futó telepítések között. A legtöbb alkalmazás bizalmas toolatency, és, amely eredményezheti lehetséges időtúllépés és késedelem adatok szinkronizálását.

Szinkronizálás vagy szívverésfigyelés különböző vDCs alkalmazásai igényel a köztük folyó kommunikációt. Két, különböző régiókban vDCs keresztül csatlakozhat:

-   ExpressRoute magánhálózati társviszony-létesítés Ha hello vDC hubok csatlakoztatott toohello azonos ExpressRoute-kapcsolatcsoportot
-   több ExpressRoute-Kapcsolatcsoportok össze a vállalati gerincét és a virtualizált tartományvezérlő háló keresztül csatlakoztatott toohello ExpressRoute-Kapcsolatcsoportok
-   A virtualizált tartományvezérlő hubok minden Azure-régióban közötti pont-pont VPN-kapcsolatok

Általában hello ExpressRoute-kapcsolat esetén előnyben részesített hello mechanizmus nagyobb sávszélességet és egységes késés miatt áthaladó hello Microsoft gerincét.

Nincs nincs magic módszereivel toovalidate különböző régiókban található két (vagy több) különböző vDCs között elosztott alkalmazás. Az ügyfelek futtasson-e hálózati minősítési tesztek tooverify hello késleltetés és a sávszélesség hello kapcsolatok és a célkiszolgáló megfelelő-e a szinkron vagy aszinkron adatreplikáció és milyen hello optimális helyreállítási idő célkitűzése (RTO) lehet a munkaterhelések.

#### <a name="mechanism-toodivert-traffic-between-dc"></a>Mechanizmus toodivert forgalom tartományvezérlő között
Egy hatékony módszer toodivert hello forgalmat egy tartományvezérlő tooanother a bejövő DNS alapul. [Az Azure Traffic Manager] [ TM] használ hello tartománynévrendszer (DNS) mechanizmus toodirect hello végfelhasználói forgalom toohello leginkább megfelelő nyilvános végpontot az egy adott virtualizált tartományvezérlő. Keresztül mintavételek menüpontban Traffic Manager rendszeresen ellenőrzi a nyilvános végpontok különböző vDCs hello szolgáltatás állapotát, és ezekre a végpontokra meghiúsulásának, esetén irányítja a automatikusan toohello másodlagos virtualizált tartományvezérlő.

A TRAFFIC Manager működik-e az Azure-végpontok nyilvános és használhat, például a forgalom tooAzure toocontrol/átirányít virtuális gépek és a Web Apps hello a megfelelő virtualizált tartományvezérlő. A TRAFFIC Manager is lehetséges, még akkor is, egy teljes Azure-régiót sikertelen hello tapasztalt, és szabályozhatja a felhasználói forgalomnak a végpontok különböző vDCs több feltétel alapján a hello terjesztési (például egy adott virtualizált tartományvezérlő szolgáltatás hibája, illetve kijelölése hello vDC a legkisebb hálózati késést hello hello ügyfél).

### <a name="conclusion"></a>Összegzés
hello virtuális adatközpont egy megközelítés toodata center áttelepítési hello felhőbe által használt funkciók és képességek toocreate kombinációja az Azure-ban, amely maximalizálja a felhőalapú erőforrások használatával, csökkenti a költségeket, és egyszerűsítse a rendszer egy méretezhető architektúra cégirányítási. hello vDC koncepció hub-küllők topológia, közös megosztott szolgáltatások hello központban, és lehetővé teszi bizonyos alkalmazások vagy munkaterhelések hello küllők a alapul. A virtualizált tartományvezérlő vállalati szerepkörök, ahol különböző részlegek számára (központi informatikai, DevOps, üzemeltetése és karbantartása) működnek együtt, egy adott szerepkörök listáját és feladatokat az hello szerkezete megfelel. A virtualizált tartományvezérlő hello megfelel a "Növekedési és Shift" áttelepítésre, de toonative felhőalapú telepítések számos előnye emellett.

## <a name="references"></a>Referencia
a következő funkciók hello a dokumentumban ismertetett volt. Kattintson a további hello hivatkozások toolearn.

| | | |
|-|-|-|
|Hálózati szolgáltatások|Terheléselosztás|Kapcsolatok|
|[Egy Azure virtuális hálózatot][VNet]</br>[Hálózati biztonsági csoportok][NSG]</br>[NSG-naplók][NSGLog]</br>[Felhasználó által definiált Útválasztás][UDR]</br>[Virtuális hálózati készülékek][NVA]</br>[Nyilvános IP-címek][PIP]|[Azure Load Balancer (3.)][ALB]</br>[Az Alkalmazásátjáró (7.)][AppGW]</br>[Webalkalmazási tűzfal][WAF]</br>[Az Azure Traffic Manager][TM] |[VNet-társviszony létesítése –][VNetPeering]</br>[Virtuális magánhálózat][VPN]</br>[ExpressRoute][ExR]
|Identitás</br>|Figyelés</br>|Ajánlott eljárások</br>|
|[Az Azure Active Directory][AAD]</br>[Többtényezős hitelesítés][MFA]</br>[Szerepkör alap hozzáférés-vezérlést][RBAC]</br>[Alapértelmezett AAD-szerepkörök][Roles] |[Tevékenység-naplók][ActLog]</br>[Diagnosztikai naplók][DiagLog]</br>[A Microsoft Operations Management Suite szolgáltatásban][OMS]</br> |[Külső hálózatok gyakorlati tanácsok][DMZ]</br>[Előfizetés-kezelés][SubMgmt]</br>[Erőforrás-csoportok kezelése][RGMgmt]</br>[Azure-előfizetésre vonatkozó korlátok][Limits] |
|Más Azure-szolgáltatásokkal|
|[Azure-webalkalmazásokban][WebApps]</br>[HDInsights (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Szolgáltatásbusz][ServiceBus]|



## <a name="next-steps"></a>Következő lépések
 - Fedezze fel [Vnetben társviszony-létesítés][VNetPeering], hello vDC központ megerősítő technológia, és tervek irány
 - Alkalmazzon [AAD] [ AAD] tooget használatába [RBAC] [ RBAC] feltárása
 - A előfizetésbe és erőforráscsoportba felügyeleti modellezése és RBAC modell toomeet hello struktúra, követelményeknek, és a szervezet házirendeket. hello legfontosabb tevékenység tervezi. Minél nagyobb gyakorlati tervezze meg a átszervezések, Összevonások, új sorokban, stb.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "Az összetevő átfedés példák" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "Magas szintű küllős vDC – példa"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "Fürt hubok és küllők"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "Küllős-küllős"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "Hello vDC blokk szintű ábrája"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "Felhasználók, csoportok, előfizetések és projektek"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "Magas szintű infrastruktúra diagramja"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "Magas szintű infrastruktúra diagramja"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "VNet-társviszony létesítése – és a külső hálózatok"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "A figyelés magas szintű diagramját"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "Az alkalmazások és szolgáltatások magas szintű diagramját"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg 
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[WebApps]: https://docs.microsoft.com/azure/app-service-web/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
