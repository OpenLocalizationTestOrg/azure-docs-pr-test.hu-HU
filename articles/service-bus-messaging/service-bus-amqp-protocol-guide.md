---
title: "aaaAMQP 1.0 Azure Service Bus és az Event Hubs protokoll útmutató |} Microsoft Docs"
description: "Protokoll útmutató tooexpressions és az Azure Service Bus és az Event Hubs AMQP 1.0 leírása"
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: clemensv
manager: timlt
editor: 
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: clemensv;hillaryc;sethm
ms.openlocfilehash: 882ce0fc84af11d9f61bc95dc3e4db0b67b2b020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# AMQP 1.0 Azure Service Bus és az Event Hubs protokoll útmutató

hello speciális üzenet Várólistázást protokoll 1.0 egy olyan szabványosított keretezési és átviteli protokoll aszinkron módon biztonságosan és megbízhatóan a felek közötti üzenetek átvitelére. Elsődleges protokoll hello Azure Service Bus üzenetkezelés és az Azure Event Hubs. Mindkét szolgáltatás is a HTTPS támogatására. hello egyéni SBMP protokollt is támogatja az hamarosan megszűnnek, AMQP helyett.

AMQP 1.0-s, amellyel elindította együtt köztes szállítók, például a Microsoft és a Red Hat több felhasználós üzenetkezelési köztes például JP Morgan Chase hello pénzügyi iparági képviselő széleskörű iparági együttműködés hello eredménye. hello műszaki szabványosítás fórum hello AMQP protokoll és a bővítmény leírásában OASIS, és elért formális jóváhagyási, ISO/IEC 19494 nemzetközi szabvány szerint.

## Célok

Ez a cikk röviden összefoglalja hello alapfogalmak hello AMQP 1.0 üzenetkezelési specifikáció együtt a bővítmény specifikációt, amely a hello OASIS AMQP Technikai Bizottság jelenleg véglegesített egy kis készletét, és elmagyarázza, az Azure Service Bus valósítja meg, és ezek a specifikációk épül.

hello célja a bármelyik fejlesztő bármely létező AMQP 1.0-ügyfél verem bármely platformra toobe képes toointeract az Azure Service Bus keresztül AMQP 1.0 használatával.

Általános célú AMQP 1.0 csomagokat, például az Apache Proton vagy AMQP.NET Lite már az összes mag AMQP 1.0 kézmozdulatok valósítja meg. E eligazodást kézmozdulatok néha csomagolt be egy magasabb szintű API-t; Két, még akkor is, ajánlatok Apache Proton hello imperatív Messenger API, és reaktív reaktor API hello.

Az ismertető a következő hello feltételezzük, hogy az AMQP kapcsolatok, a munkamenetek és a hivatkozások és hello kezelésének keret átvitel és az adatfolyam vezérlés hello felügyelete hello megfelelő verem (például az Apache Proton-C) kezeli, és nem igényel sok esetleges specifikus az alkalmazásfejlesztők figyelmet. Absztraktként feltételezzük, hogy néhány API primitívek például hello képességét tooconnect és toocreate hello megléte valamilyen *küldő* és *fogadó* absztrakciós objektumokat, amely már rendelkezik néhány alakját`send()`és `receive()` műveleteket, illetve.

Üzenet megkeresésével, vagy a felügyeleti munkamenetek, például az Azure Service Bus fejlett funkcióinak ismertetésekor ezekről a szolgáltatásokról magyarázatát AMQP feltételeket, de is, a réteges látszólagosan megvalósítása a feltételezett API absztrakciós fölött.

## Mi az az AMQP?

AMQP egy olyan keretezési és átviteli protokoll. A keretezési azt jelenti, hogy a struktúra biztosítja, hogy a hálózati kapcsolat mindkét irányban bináris adatok adatfolyamok. hello struktúra biztosítja a különböző blokk ismert körülhatárolásához *keretek*, toobe kicserélt csatlakoztatott hello felek között. hello átviteli lehetőségeket győződjön meg arról, hogy a kommunikáló felek létesíthet egy megosztott ismertetése, amikor keretek át kell, és ha átvitelek teljes tekintendő.

Lejárt vázlat korábbi verzióktól eltérően hello AMQP-munkacsoportja által előállított, amelyek még a néhány üzenet brókerek használják hello működő csoport végleges, és a szabványos AMQP 1.0 protokoll nem írja elő egy üzenet broker vagy az összes adott topológia hello jelenléte az entitások üzenet közvetítő belül.

hello protokoll, amely támogatja a várólisták és a közzétételi/előfizetési entitások, mint Azure Service Bus message brókerek való együttműködéshez szükséges szimmetrikus társ-társ kommunikációhoz használható. Azt is használható az üzenetkezelési infrastruktúra való együttműködéshez ahol hello interakció minták eltérnek rendszeres várólisták, mint hello az Azure Event Hubs. Az Eseményközpontok az üzenetsorokhoz hasonlóan működik események tooit elküldött, de úgy viselkedik, több mint egy soros társzolgáltatás események olvasása, a következőhöz hasonló szalagos meghajtó némileg. hello ügyfél eltolással szerzi hello rendelkezésre álló adatok adatfolyamba, és majd kiszolgált adott eltolási toohello legújabb érhető el az összes eseményt.

hello AMQP 1.0 protokoll tervezett toobe bővíthető további engedélyezése specifikációk tooenhance képességeit. hello a dokumentumban ismertetett három bővítmény specifikációk ezt mutatják be. Ahol hello natív AMQP TCP-portok konfigurálása nehéz lehet a meglévő HTTPS/websocket elemek infrastruktúrára kommunikációhoz, a kötés specifikációval meghatározása hogyan toolayer AMQP keresztül websocket elemeket. Hello üzenetkezelési infrastruktúra a kérelem/válasz való kommunikáció felügyeleti szempontból vagy tooprovide speciális funkció, hello AMQP management specifikáció módon hello szükséges alapvető interakció primitívek határozza meg. Az összevont engedélyezési modell integráció, hello AMQP jogcím-alapú biztonsági specification meghatározása hogyan tooassociate megújítja a hitelesítési tokenek hivatkozások társított.

## Alapszintű AMQP-forgatókönyvek

Ez a szakasz ismerteti az AMQP 1.0-s, Azure Service Bus, beleértve a kapcsolatok, munkamenetek és hivatkozások létrehozása és üzenetek tooand visz át a Service Bus-entitások, például várólisták, témakörök és előfizetések hello alapvető használatát.

hello mérvadó forrás toolearn AMQP működésével kapcsolatos az AMQP 1.0 hello által kidolgozott szabvány, de hello specification írt tooprecisely útmutató végrehajtása és tooteach hello protokoll. Ez a szakasz összpontosít mértékű terminológia keresztül mutatja, hogyan használja a Service Bus az AMQP 1.0 igény szerint bemutatása. Egy átfogóbb bemutatása tooAMQP, valamint egy szélesebb körű leírását az AMQP 1.0-s, tekintse át [ezt a videót megoldást][this video course].

### Kapcsolatok száma és a munkamenetek

Programok kommunikáció AMQP hívások hello *tárolók*; ezek tartalmazhat *csomópontok*, mely hello kommunikációra entitás összes blobhoz belül. A várólista ilyen egy csomópont lehet. AMQP lehetővé teszi, hogy multiplexáló, így egyetlen kapcsolatból használható sok kommunikációs útvonala; csomópontok között például egy alkalmazás ügyfél egyszerre fogadhat egy üzenetsor és a küldési tooanother sor hello keresztül azonos hálózati kapcsolat.

![][1]

hello hálózati kapcsolat így rögzített hello tárolójához. Egy kimenő TCP szoftvercsatorna kapcsolat tooa tároló létrehozása hello fogadó szerepkört, amely figyeli, és elfogadja a bejövő TCP-kapcsolatok a hello ügyfél szerepkörben hello tároló kezdeményezi. hello kapcsolati kézfogás egyeztetése hello címprotokoll-verziójával, deklaráló vagy egyeztetése Transport Level Security (TLS/SSL) és egy hitelesítési/engedélyezési kézfogás hello kapcsolat hatókörből SASL alapuló hello használata tartalmazza.

Az Azure Service Bus mindig a TLS hello használata szükséges. Akkor támogatja a kapcsolatokat keresztül TCP-port: 5671, amellyel hello TCP-kapcsolatot a TLS hello AMQP protokoll kézfogás megadása előtt először átfedett, és amellyel hello kiszolgáló azonnal nyújt egy kötelező frissítés, az TCP-porton 5672 kapcsolatokat is támogatja kapcsolat tooTLS hello AMQP-előírt modell használatával. hello AMQP websocket elemek kötés alagutat hoz létre a TCP 443-as portot, amely majd egyenértékű tooAMQP 5671 kapcsolatok keresztül.

Miután beállította a hello kapcsolat és a TLS, Service Bus két SASL mechanizmus lehetőségeket tesz lehetővé:

* SASL egyszerű felhasználónév és jelszó hitelesítő adatok tooa kiszolgáló sikeres gyakran használják. A Service Bus nincs partnerekkel, de elnevezett [megosztott hozzáférési szabályok](service-bus-sas.md), amely jogosultsággal rendelkezik, és társított kulcs. a szabály neve hello használatos hello felhasználónevet és a hello kulcs (a base64 kódolású szöveget) a hello jelszó szolgál. a kiválasztott szabály hello kapcsolódó hello jogosultságok hello kapcsolat engedélyezett hello műveletek tekintetében.
* NÉVTELEN SASL használható SASL engedélyezési megkerülésében hello ügyfél szeretne toouse hello jogcím-alapú biztonsági (CBS) modellt, amely a későbbiekben olvashat. Ezzel a beállítással egy ügyfél kapcsolat névtelenül rövid időre során melyik hello ügyfél csak használhatják hello CBS végpont és hello CBS kézfogás kell végrehajtania.

Hello szállítási kapcsolat létrejötte után hello tárolók egyes deklarálható hello keret maximális mérete hajlandó toohandle, és üresjárati időkorlátot után lesz egyoldalúan megszakad nincs tevékenység esetén hello kapcsolaton.

Ezek deklarálhatják hány egyidejű csatornák használata támogatott. A csatorna egy olyan hello kapcsolat fölött egy egyirányú, kimenő, virtuális átviteli elérési utat. A munkamenet egy csatornát, az egyes hello összekapcsolt tárolók tooform egy kétirányú kommunikációs útvonalat vesz igénybe.

Munkamenetek rendelkezik egy Windows-alapú vezérlő folyamatmodell; egy munkamenet létrejöttekor felek kijelenti, hány keretek azt hajlandó tooaccept azokat a fogadási ablak. Hello felek exchange keretek, átvitel keretek töltse ki, hogy ablakot, és az átvitel le, amikor hello ablak megtelik, és addig, amíg hello ablak lekérdezi alaphelyzetbe vagy kibontva hello segítségével *performative folyamata* (*performative*hello AMQP kifejezés a protokoll-szintű kézmozdulatok hello a két fél között).

A Windows-alapú modell nagyjából megfelel toohello TCP koncepció Windows-alapú átvitelvezérlés, de belül hello szoftvercsatorna hello munkamenet szinten. hello protokoll koncepcióján alapulva lehetővé teszi több egyidejű munkamenetek a létezik-e, hogy a magas prioritású virtuális gép forgalom sikerült kell rushed túli szabályozottan halmozott normál forgalom, például a highway expressz lane.

Az Azure Service Bus pontosan egy munkamenet jelenleg használ minden egyes kapcsolathoz. a Service Bus maximális keret méretét hello a Service Bus szabványos és az Event Hubs 262 144 bájt (256 KB). 1 048 576 (1 MB), a Service Bus prémium. A Service Bus nem ír elő az adott munkamenet szintű szabályozási időszakokon, de alaphelyzetbe állítását hello ablak rendszeresen hivatkozás szintű átvitelvezérlés részeként (lásd: [hello a következő szakaszban](#links)).

Kapcsolatok száma, a csatornák és a munkamenetek nincs rövid élettartamú. Ha az alapul szolgáló kapcsolatot hello összecsukó, kapcsolatok, TLS-alagútjának SASL engedélyezési környezetet és munkamenetek kell hozni.

### Hivatkozások

AMQP-kapcsolaton keresztül üzenetek átvitelére. Egy hivatkozást, amely lehetővé teszi egy irányban; átadó üzenetek munkameneten keresztül létrehozott kommunikációs elérési úttal hello adatátviteli állapot egyeztetés hello hivatkozást és a kétirányú csatlakozó hello felek közötti felett van.

![][2]

Hivatkozások hozhat létre vagy tároló bármikor, valamint egy meglévő munkamenetben, így az AMQP eltér sok más protokoll, beleértve a HTTP és a MQTT átvitel és az átvitel elérési hello kezdeményezése esetén egy kizárólagos jogosultsággal hello fél létrehozása hello szoftvercsatorna-kapcsolat.

hello hivatkozás kezdeményezése tároló kéri ellentétes tároló tooaccept hello egy hivatkozást, és azt úgy dönt, hogy egy szerepkör vagy a feladó, vagy a címzett. Ezért tároló is kezdeményezhető létrehozása egyirányú vagy kétirányú kommunikáció elérési utakat, ez utóbbi hello hivatkozások értékpár modellezve.

Hivatkozások vannak nevű társított és csomópontok. Hello kezdete leírtaknak csomópontja hello entitások tárolója belüli kommunikáció.

A Service Bus a csomópont közvetlenül egyenértékű tooa várólista, a témakör, előfizetés vagy a kézbesítetlen levelek várakozási sor vagy előfizetés. hello csomópont nevét a AMQP ezért hello relatív név hello entitás hello Service Bus-névtér belül. Ha a várólista neve **Várólista_neve**, ez is a AMQP csomópont neve. A következő hello HTTP API egyezmény üzenettémakör-előfizetésben szerint rendezve alatt álló "előfizetések" erőforrás-gyűjteményt, és így előfizetés **sub** vagy egy téma **mytopic** hello AMQP csomópont neve **mytopic/előfizetések/sub**.

hello kapcsolódó ügyfél is szükség toouse hivatkozások; létrehozása helyi csomópont nevét A Service Bus nincs csomópont neveket kapcsolatos előírásoknak megfelelő, és azok nem értelmezhetők. AMQP 1.0-ügyfél verem általában egy séma tooassure, hogy a rövid élettartamú csomópont nevek egyediek hello hatókörében hello ügyfél használja.

### Átvitel

Hivatkozás létrehozása után üzenetek átvihetők adott hivatkozáson keresztül. AMQP, egy átviteli egy explicit protokoll hitelesítési módok végre (hello *átviteli* performative), amely egy üzenetet helyezi a küldő tooreceiver kapcsolaton keresztül. Egy átviteli kész, amikor az "rendezik", ami azt jelenti, hogy a két fél rendelkezik-e létre a megosztott ismeretének átvitel hello eredményeit.

![][3]

Hello legegyszerűbb esetben hello küldő választhatja ki, amely hello ügyfél nem hello eredménye iránt érdeklődik, és hello fogadó nem észrevétele bármely hello hello művelet eredménye "előre rendezni," toosend üzeneteket. Ebben a módban hello AMQP protokoll szint Service Bus által támogatott, de nincs felfedve hello ügyfél API-k valamelyikében.

hello rendszeres eset az, hogy üzenetek küldése határidőig és hello fogadó megjeleníti elfogadása vagy elutasítása hello használata *törlése* performative. Elutasítás következik be, amikor hello fogadó bármilyen okból nem tudja elfogadni üdvözlőüzenetére, és elutasítása üdvözlőüzenetére hello OK, amely egy AMQP által definiált hiba struktúra információt tartalmaz. Üzenetek toointernal hibák Service Bus belül lejáró utasítja el, ha hello szolgáltatást biztosítani a mutatók toosupport személyzet diagnosztika Ha támogatási kérelmek használható struktúra lévő további információkat ad vissza. Később megismerheti hibákkal kapcsolatos további részletekért.

Egy különös elutasítási formátuma hello *kiadott* állapotba kerül, ami azt jelenti, hogy hello fogadó nem műszaki kifogást toohello átviteli, de a is rendezése nem érdekelt hello átvitel. Eset létezéséről, például amikor egy üzenetet kézbesíteni a tooa Service Bus-ügyfélalkalmazást és hello előnyben túl "abandon" üdvözlőüzenetére származó üdvözlőüzenetére; hello munkahelyi nem hajtható végre, mert maga hello üzenetkézbesítése nincs hibás. Egy adott állapotban változata hello *módosított* állapotát, amely lehetővé teszi, hogy módosításokat toohello üzenet megjelent szerint. Az állapotban nem jelenleg a Service Bus használják.

hello AMQP 1.0-specifikáció határozza meg a további törlése állapot nevű *kapott*, kifejezetten hozzájárul toohandle hivatkozás helyreállítási. Hivatkozás helyreállítási lehetővé teszi, hogy újbóli megállapításának módjaival hello állapotát egy hivatkozást, és minden függőben lévő kézbesítések fölött egy új kapcsolat és a munkamenet, ha a hello előzetes kapcsolat és a munkamenet elvesztek.

A Service Bus nem támogatja a helyreállítási hivatkozásra; Ha hello ügyfél elveszti hello kapcsolat tooService Bus határidőig üzenetet átviteli függőben lévő, üzenet átvitel elvész, és hello ügyfél kell újracsatlakozni, újra létrehozni a hello hivatkozásra, és próbálja megismételni a hello átvitel.

A Service Bus és az Event Hubs támogatja a "legalább egyszeri" átvitelek, ahol hello küldő üdvözlőüzenetére tárolja, és amelyek fogadták el a biztos lehet benne, de nem támogatják a "pontosan egyszeri" átvitelek hello AMQP szintjét, amelyben hello rendszer toorecover kísérletet hello hivatkozásra, és továbbra is toonegotiate hello szállítási állapota tooavoid párhuzamos hello üzenetátvitel.

a lehetséges duplikált toocompensate küldi el, a Service Bus által támogatott egyik opcionális szolgáltatása, kettős észlelés üzenetsorok és témakörök. Kettős észlelés rögzíti az összes bejövő üzenetek üzenet azonosítók hello felhasználó által megadott időszak alatt, majd csendes küldött összes üzenet elhagyta hello adott azonos időszakban ugyanazokat az üzenet-azonosítókat.

### Adatfolyam vezérlés

Ezenkívül toohello kapcsolati szintű átvitelvezérlés modell, amely a korábban tárgyalt, minden egyes hivatkozás rendelkezik saját folyamatmodell vezérlő. Kapcsolati szintű átvitelvezérlés hello tároló kelljen toohandle túl sok keretet, miután hivatkozás szintű átvitelvezérlés hello alkalmazás hány üzenetek azt részlege azt szeretné toohandle hivatkozás feladata helyezi, és ha védelmet nyújt.

![][4]

Hivatkozásra, átvitelek csak akkor szükséges, ha hello feladó rendelkezik-e elegendő *jóváírás hivatkozás*. Hivatkozás követel hello fogadó hello segítségével állítja be a számláló értéke *folyamata* performative, amelynek hatóköre tooa hivatkozásra. Hello küldő hivatkozás jóváírás van hozzárendelve, akkor próbálja toouse be, hogy a jóváírási által az üzenetek kézbesítése. Minden üzenet kézbesítési csökkenti fennmaradó hivatkozást jóváírás hello 1. Hello hivatkozás jóváírás használatakor kézbesítések leállítása.

Hello fogadó szerepkör Service Bus esetén azonnal biztosít hello küldő bőséges hivatkozás kreditet, hogy az üzenetek is küldhetők el azonnal. Hivatkozás jóváírás használt, a Service Bus alkalmanként küld egy *folyamata* performative toohello küldő tooupdate hello hivatkozás kreditegyenlege.

Hello küldő szerepkör a Service Bus küld üzenetek toouse másolatot kreditegyenlege a függőben lévő hivatkozásra.

Hello API szinten "kap" hívás fordítja le a *folyamata* performative hello várólista, zárolással, üzenetet küldött tooService Bus hello ügyfél és a Service Bus-t használ fel, amelyek először credit véve hello által elérhető, nyitása és áthelyezte azt. Nincs hibaüzenet a kézbesítés azonnal elérhetők, ha bármely csatlakozásonkénti fennmaradó kreditegyenlege létrehozni, hogy adott entitás rögzített érkezési sorrendben marad, és üzenetek zárolva van, és át azok válnak elérhetővé, toouse fennmaradó kreditegyenlege a.

hello üzenetben zárolás úgy hello átviteli kiegyenlítését hello terminál állapotokba *elfogadott*, *elutasított*, vagy *kiadott*. hello üzenetet eltávolítja a Service Bus Ha terminál állapota hello *elfogadott*. A Service Bus marad, és toohello következő fogadó kerül, ha hello átvitel eléri hello bármelyikét állapotokat. A Service Bus hello entitás megfelelő toorepeated elutasítások vagy kiadásokban engedélyezett hello maximális számának elérésekor automatikusan áthelyezi üdvözlőüzenetére hello entitás kézbesítetlen levelek várólistájára.

Annak ellenére, hogy a Service Bus alkalmazásprogramozási hello közvetlenül nem fedi fel az ilyen lehetőség ma, alacsonyabb szintű AMQP protokoll ügyfél használhatja-e a hello hivatkozás-jóváírási modell tooturn hello "pull-style stílust" interakció minden fogadási kérést kreditet egy egységének kibocsátó a "leküldéses-style stílust" modellbe nagyszámú hivatkozás kreditek és majd üzeneteket fogadni, amint azok elérhetővé további beavatkozás nélkül válnak. Leküldéses keresztül hello támogatott [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PrefetchCount) vagy [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) tulajdonságbeállítások. Ha nullától eltérő, hello AMQP ügyfél használja, hello hivatkozás jóváírás.

Ebben a környezetben annak fontos toounderstand, amely üdvözlőüzenetére belül hello entitás hello zárolást hello lejáratának órája hello kezdődik, amikor hello üzenet forrása hello entitás nem üdvözlőüzenetére az üzembe helyezés hello keresztülhaladnak a hálózaton. Amikor hello ügyfél készültségi tooreceive üzenetek hivatkozás jóváírás kiállításával azt jelzi, ezért várt toobe aktívan adatlekérő üzenetek hello hálózaton keresztül, és készen áll a toohandle kell őket. Ellenkező esetben hello üzenet zárolási lehet, hogy lejárt előtt üdvözlőüzenetére akkor kerül. hivatkozás-jóváírási átvitelvezérlés hello használata a rendelkezésre álló küldött üzenetek toohello fogadó hello azonnali készültségi toodeal közvetlenül tükröznie kell azt.

Az összegzés hello következő szakaszokban hello performative folyamata vázlatos áttekintése különböző API interakció során. Az egyes szakaszokon egy másik logikai működését ismerteti. Lehet, hogy egy adott kapcsolati "Lusta," tehát, hogy azok csak hajtható végre, ha szükséges. Egy üzenet küldője létrehozása nem okozhat a hálózati kommunikáció mindaddig, amíg az első üdvözlőüzenetére küldik vagy kért.

hello hello a következő táblázatban szereplő nyilak hello performative folyamat iránya.

#### Üzenet fogadó létrehozása

| Ügyfél | Service Bus |
| --- | --- |
| --> () csatolása<br/>Name = {link name}.<br/>kezelni = {numerikus leíró},<br/>szerepkör =**fogadó**,<br/>forrás = {egyednév}<br/>cél = {Ügyfélazonosító hivatkozás}<br/>) |Ügyfél tooentity fogadni kapcsolódni fog. |
| A Service Bus válaszok hello hivatkozás végét csatolása |<--(csatolása<br/>Name = {link name}.<br/>kezelni = {numerikus leíró},<br/>szerepkör =**küldő**,<br/>forrás = {egyednév}<br/>cél = {Ügyfélazonosító hivatkozás}<br/>) |

#### Üzenet küldőjének létrehozása

| Ügyfél | Service Bus |
| --- | --- |
| --> () csatolása<br/>Name = {link name}.<br/>kezelni = {numerikus leíró},<br/>szerepkör =**küldő**,<br/>forrás = {Ügyfélazonosító hivatkozás}<br/>cél = {entitás neve}<br/>) |Nincs művelet |
| Nincs művelet |<--(csatolása<br/>Name = {link name}.<br/>kezelni = {numerikus leíró},<br/>szerepkör =**fogadó**,<br/>forrás = {Ügyfélazonosító hivatkozás}<br/>cél = {entitás neve}<br/>) |

#### Hozzon létre az üzenet feladójának (hiba)

| Ügyfél | Service Bus |
| --- | --- |
| --> () csatolása<br/>Name = {link name}.<br/>kezelni = {numerikus leíró},<br/>szerepkör =**küldő**,<br/>forrás = {Ügyfélazonosító hivatkozás}<br/>cél = {entitás neve}<br/>) |Nincs művelet |
| Nincs művelet |<--(csatolása<br/>Name = {link name}.<br/>kezelni = {numerikus leíró},<br/>szerepkör =**fogadó**,<br/>forrás = null,<br/>cél = null<br/>)<br/><br/><--leválasztani ()<br/>kezelni = {numerikus leíró},<br/>lezárt =**igaz**,<br/>Hiba = {hibaadatainak}<br/>) |

#### Bezárási üzenetet fogadó/feladó

| Ügyfél | Service Bus |
| --- | --- |
| --> () leválasztása<br/>kezelni = {numerikus leíró},<br/>lezárt =**igaz**<br/>) |Nincs művelet |
| Nincs művelet |<--leválasztani ()<br/>kezelni = {numerikus leíró},<br/>lezárt =**igaz**<br/>) |

#### Küldési (sikeres)

| Ügyfél | Service Bus |
| --- | --- |
| --> átvitel)<br/>kézbesítési-id = {numerikus leíró},<br/>kézbesítési-címke = {bináris leíró},<br/>rendezni =**hamis**,, több =**hamis**,<br/>állapot =**null**,<br/>folytatása =**hamis**<br/>) |Nincs művelet |
| Nincs művelet |<--törlése ()<br/>szerepkör = fogadó,<br/>első = {kézbesítési azonosító}<br/>utolsó = {kézbesítési azonosító}<br/>rendezni =**igaz**,<br/>állapot =**elfogadva**<br/>) |

#### Küldési (hiba)

| Ügyfél | Service Bus |
| --- | --- |
| --> átvitel)<br/>kézbesítési-id = {numerikus leíró},<br/>kézbesítési-címke = {bináris leíró},<br/>rendezni =**hamis**,, több =**hamis**,<br/>állapot =**null**,<br/>folytatása =**hamis**<br/>) |Nincs művelet |
| Nincs művelet |<--törlése ()<br/>szerepkör = fogadó,<br/>első = {kézbesítési azonosító}<br/>utolsó = {kézbesítési azonosító}<br/>rendezni =**igaz**,<br/>állapot =**elutasított**()<br/>Hiba = {hibaadatainak}<br/>)<br/>) |

#### Fogadás

| Ügyfél | Service Bus |
| --- | --- |
| --> folyamata)<br/>hivatkozás-jóváírási = 1<br/>) |Nincs művelet |
| Nincs művelet |< transfer ()<br/>kézbesítési-id = {numerikus leíró},<br/>kézbesítési-címke = {bináris leíró},<br/>rendezni =**hamis**,<br/>több =**hamis**,<br/>állapot =**null**,<br/>folytatása =**hamis**<br/>) |
| --> törlése)<br/>szerepkör =**fogadó**,<br/>első = {kézbesítési azonosító}<br/>utolsó = {kézbesítési azonosító}<br/>rendezni =**igaz**,<br/>állapot =**elfogadva**<br/>) |Nincs művelet |

#### Több üzenet fogadása

| Ügyfél | Service Bus |
| --- | --- |
| --> folyamata)<br/>hivatkozás-jóváírási = 3<br/>) |Nincs művelet |
| Nincs művelet |< transfer ()<br/>kézbesítési-id = {numerikus leíró},<br/>kézbesítési-címke = {bináris leíró},<br/>rendezni =**hamis**,<br/>több =**hamis**,<br/>állapot =**null**,<br/>folytatása =**hamis**<br/>) |
| Nincs művelet |< transfer ()<br/>kézbesítési-id = {numerikus leíró + 1},<br/>kézbesítési-címke = {bináris leíró},<br/>rendezni =**hamis**,<br/>több =**hamis**,<br/>állapot =**null**,<br/>folytatása =**hamis**<br/>) |
| Nincs művelet |< transfer ()<br/>kézbesítési-id = {numerikus leíró + 2},<br/>kézbesítési-címke = {bináris leíró},<br/>rendezni =**hamis**,<br/>több =**hamis**,<br/>állapot =**null**,<br/>folytatása =**hamis**<br/>) |
| --> törlése)<br/>szerepkör = fogadó,<br/>első = {kézbesítési azonosító}<br/>utolsó = {kézbesítési azonosító + 2},<br/>rendezni =**igaz**,<br/>állapot =**elfogadva**<br/>) |Nincs művelet |

### Üzenetek

hello alábbi szakaszok ismertetik a Service Bus hello szabványos AMQP üzenet szakaszok tulajdonságok használja, és hogyan leképezik toohello Service Bus API-készlet.

#### header

| Mező neve | Használat | API-név |
| --- | --- | --- |
| tartós |- |- |
| Prioritás |- |- |
| élettartam |Ez az üzenet toolive ideje |[A TimeToLive tulajdonság](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| első-beszerző |- |- |
| kézbesítési-száma |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) |

#### properties

| Mező neve | Használat | API-név |
| --- | --- | --- |
| üzenetazonosító |Alkalmazás által meghatározott, szabad formátumú azonosító az üzenethez. Használja az ismétlődő észlelésére. |[MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| felhasználói azonosító |Alkalmazás által meghatározott felhasználói azonosítóját, a Service Bus által nem értelmezhető. |Nem érhető el a Service Bus API hello záradékkal. |
| túl|Alkalmazás által meghatározott cél azonosítója, a Service Bus által nem értelmezhető. |[A](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| Tulajdonos |Alkalmazás által meghatározott célú üzenetazonosító, Service Bus által nem értelmezhető. |[Címke](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| válasz túl|Alkalmazás által meghatározott válasz-elérési út mutató, Service Bus által nem értelmezhető. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| korrelációs azonosító |Alkalmazás által meghatározott korrelációs azonosító, a Service Bus által nem értelmezhető. |[CorrelationId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CorrelationId) |
| tartalomtípus |Alkalmazás által meghatározott tartalomtípus mutató a hello törzs, Service Bus által nem értelmezhető. |[A ContentType](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType) |
| tartalom kódolása |Alkalmazás által meghatározott tartalom kódolása mutató a hello törzs, Service Bus által nem értelmezhető. |Nem érhető el a Service Bus API hello záradékkal. |
| abszolút-lejárati-időpont |Azt deklarálja, mely abszolút azonnali hello üzenet lejár. A bemeneti (TTL követi fejlécet), figyelmen kívül hagyja a kimenetet mérvadó. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| Létrehozás-időpontja |Azt deklarálja, mely idő hello üzenet jött létre. Nem használja a Service Bus |Nem érhető el a Service Bus API hello záradékkal. |
| csoport-azonosítója |Az üzenetek kapcsolódó készletének, alkalmazásszinten megadott azonosító. A Service Bus-munkamenetek használt. |[Munkamenet-azonosító](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| csoport-sorozat |A számláló azonosító hello relatív sorszámát üdvözlőüzenetére munkamenet belül. A Service Bus figyelmen kívül hagyja. |Nem érhető el a Service Bus API hello záradékkal. |
| válasz a csoport azonosítója |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyToSessionId) |

## Speciális Service Bus-képességek

Ez a fejezet speciális képességek az Azure Service Bus vázlat bővítmények tooAMQP jelenleg fejlesztés alatt áll a hello OASIS Technikai Bizottság az AMQP alapuló. A Service Bus valósítja meg az ezen az előzetes példányok konvertálása hello legújabb verzióját, és fogad el, ezek Piszkozatok elérni a normál üzenetterhelésen bevezetett változások.

> [!NOTE]
> Service Bus üzenetkezelés egy kérelem-válasz mintát keresztül speciális műveletek támogatottak. Ezek a műveletek hello részleteit ismerteti a hello dokumentum [AMQP 1.0 a Service Bus: kérelem-válasz-alapú műveletek](service-bus-amqp-request-response.md).
> 
> 

### AMQP kezelése

hello AMQP-management specifikáció hello első hello vázlat bővítmények Microsofttól. Ez az előírás protokoll kézmozdulatok hello AMQP protokoll rétegre, amelyek lehetővé teszik az üzenetkezelési infrastruktúra AMQP keresztül hello történő felügyeleti interakciók álló készletet határoz meg. hello specification meghatározása általános műveletek, mint *létrehozása*, *olvasási*, *frissítése*, és *törlése* entitások belüli kezelésére szolgáló egy üzenetkezelési infrastruktúra és a lekérdezési műveletek.

E kézmozdulatok közreműködése egy kérelem-válasz hello ügyfél és a hello üzenetkezelési infrastruktúra között, és ezért hello specification határozza meg, hogyan toomodel adott interakció mintát AMQP fölött: hello ügyfél kapcsolódik toohello üzenetkezelés infrastruktúra, indít el egy munkamenetet, és létrehozza a hivatkozások két. Egy hivatkozásra hello ügyfél úgy működik, mint a küldő és a többi hello úgy működik, mint fogadó, így a két hivatkozásokat tartalmaz, amelyek működhet, és a kétirányú csatorna létrehozása.

| Logikai művelet | Ügyfél | Service Bus |
| --- | --- | --- |
| Kérelem-válasz elérési utat hoz létre |--> () csatolása<br/>name = {*hivatkozásnév*},<br/>kezelni = {*numerikus leíró*},<br/>szerepkör =**küldő**,<br/>forrás =**null**,<br/>target = "myentity / $kezelése"<br/>) |Nincs művelet |
| Kérelem-válasz elérési utat hoz létre |Nincs művelet |\<--(csatolása<br/>name = {*hivatkozásnév*},<br/>kezelni = {*numerikus leíró*},<br/>szerepkör =**fogadó**,<br/>forrás = null,<br/>target = "myentity"<br/>) |
| Kérelem-válasz elérési utat hoz létre |--> () csatolása<br/>name = {*hivatkozásnév*},<br/>kezelni = {*numerikus leíró*},<br/>szerepkör =**fogadó**,<br/>forrás = "myentity / $felügyelet",<br/>target = "myclient$ id"<br/>) | |
| Kérelem-válasz elérési utat hoz létre |Nincs művelet |\<--(csatolása<br/>name = {*hivatkozásnév*},<br/>kezelni = {*numerikus leíró*},<br/>szerepkör =**küldő**,<br/>forrás = "myentity"<br/>target = "myclient$ id"<br/>) |

Hivatkozások, hogy a két helyen, hello kérelem/válasz végrehajtására, akkor az egyszerű: egy kérelem egy levelet tooan entitás hello üzenetkezelési infrastruktúra, amely együttműködik a ebben a mintában belül. Kérelem-üzenetben, hello *Válaszcím* hello mezőjét *tulajdonságok* toohello állítja *cél* , mely toodeliver hello válasz hello hivatkozás azonosítóját. entitás kezelése hello hello kérést dolgoz fel, és majd kézbesíti hello válasz hello keresztül kapcsolja, amelynek *cél* azonosítója megegyezik a megadott hello *Válaszcím* azonosítója.

hello mintát nyilvánvalóan megköveteli, hogy hello ügyfél tároló és hello ügyfél által generált azonosítójának hello válasz cél legyenek egyedi összes ügyfélre, és a biztonsági okokból is nehéz toopredict.

hello üzenetváltásokban használt hello felügyeleti protokoll és az összes többi protokoll, hogy használjon hello ugyanilyen mintájú fordulhat elő, hello alkalmazás szinten; Ezek új AMQP protokoll szintű kézmozdulatok nem határoznak meg. Ez szándékos, úgy, hogy az alkalmazások azonnali használhatják a megfelelő AMQP 1.0 verem bővítmény.

A Service Bus jelenleg implementálja hello management specifikáció hello alapvető szolgáltatások egyikét sem, de ha hello kérelem/válasz mintát hello management specifikáció által meghatározott eligazodást hello jogcím-alapú biztonsági funkció, és szinte minden hello speciális hello a következő részekben ismertetett lehetőségeket.

### Jogcímalapú engedélyezési

hello AMQP jogcímek alapú engedélyezési (CBS) specifikáció vázlat hello management specifikáció kérelem/válasz mintát épül, és ismerteti, hogyan toouse összevont biztonsági jogkivonatokat az AMQP általánosított modellt.

hello alapértelmezett biztonsági modellje hello bemutatása tárgyalt AMQP SASL alapul, és jól integrálható az hello AMQP kapcsolati kézfogás. SASL használatával van hello előnye, hogy egy bővíthető, amelynek mechanizmusok készlete minden protokoll, amely a SASL hivatalosan leans kihasználhassa a meghatározott modellt biztosít. Ezek a mechanizmusok közé tartoznak a felhasználónevek és a jelszavak, a "EXTERNAL" toobind tooTLS szintű biztonság, a "Névtelen" tooexpress hello hiányában explicit hitelesítési/engedélyezési és kiegészítő mechanizmusok, amelyek lehetővé teszik széles választékában átviteli "Egyszerű" sikeres hitelesítési vagy engedélyezési hitelesítő adatok és a jogkivonatok.

Az AMQP tartozó SASL integráció két hátrányai is tartalmaz:

* Hitelesítő adatok és a tokeneket olyan hatókörrel rendelkező toohello kapcsolat. Üzenetkezelési infrastruktúra azt szeretné, hogy egy entitás alapon; megkülönböztetett tooprovide hozzáférés-vezérlés például így hello tulajdonosi egy token toosend tooqueue A, de nem tooqueue a b kiszolgálóra. Hello engedélyezési kontextusú hello kapcsolaton rögzített nem lehetséges toouse internetkapcsolat, és különböző hozzáférési jogkivonatok várólista A és b várólista még használni
* Hozzáférési jogkivonatok érvényesek általában csak korlátozott ideig. Az érvényességi hello felhasználói tooperiodically újbóli jogkivonatok igényel, és egy új jogkivonatot kibocsátó hello felhasználói hozzáférési engedélyek változása esetén lehetőség toohello jogkivonatot kibocsátó toorefuse biztosít. AMQP kapcsolatok hosszú ideig tarthatnak. hello SASL modell csak biztosít egy alkalommal tooset jogkivonat kapcsolat során, ami azt jelenti, hogy az üzenetkezelési infrastruktúra vagy hello rendelkezik a toodisconnect hello ügyfél hello-token érvényessége lejár, illetve tooaccept hello kockázatot jelent, ha engedélyezi a folyamatos kommunikációt igényel a ügyfél ki rendelkezik hozzáférési jogosultsággal a közbenső hello visszavont előfordulhat, hogy.

hello AMQP CBS megadását, a Service Bus által megvalósított mindkét ismertetünk lehetővé teszi egy elegáns megkerülő megoldás: minden csomópont, és azok jogkivonatok előtt járnak, hello üzenet folyamat megszakítása nélkül tooupdate tooassociate hozzáférési jogkivonatok segítségével az ügyfél.

CBS határozza meg a virtuális felügyeleti nevű csomópont *$cbs*, toobe hello üzenetkezelési infrastruktúra által biztosított. hello csomópontot nevében bármely más csomópontok az üzenetküldési infrastruktúra hello jogkivonatokat fogad el.

hello protokoll hitelesítési mód egy kérelem/válasz exchange hello management specifikáció által definiált konfigurációjának kialakításához. Hello való kapcsolatok párból, hogy azt jelenti, hogy hello ügyfél létesít *$cbs* csomópont és a kérelmek hello kimenő hivatkozásra, és megvárja a hello választ a fázisok hello bejövő hivatkozás.

hello kérelemüzenet rendelkezik a következő alkalmazás tulajdonságai hello:

| Kulcs | Optional | Érték típusa | Érték tartalma |
| --- | --- | --- | --- |
| művelet |Nem |Karakterlánc |**a PUT-jogkivonat** |
| type |Nem |Karakterlánc |mivel ez egy put hello token hello típusa. |
| név |Nem |Karakterlánc |hello "célközönség" toowhich hello token érvényes. |
| lejárati |Igen |időbélyeg |hello hello jogkivonat lejárati idejét. |

Hello *neve* tulajdonság hello entitás melyik hello a token bekapcsolódik azonosítja. A Service Bus azt a hello elérési toohello várólistát, vagy témakört/előfizetést. Hello *típus* tulajdonság azonosítja hello a jogkivonat típusa:

| A jogkivonat típusa | Token leírása | Törzs típusa | Megjegyzések |
| --- | --- | --- | --- |
| amqp:jwt |JSON webes jogkivonat (JWT) |AMQP érték (karakterlánc) |Még nem érhető el. |
| amqp:swt |Egyszerű webes jogkivonat (SWT) |AMQP érték (karakterlánc) |Csak a támogatott AAD/ACS által kiállított SWT jogkivonatokat |
| servicebus.Windows.NET:sastoken |Service Bus SAS-jogkivonat |AMQP érték (karakterlánc) |- |

Jogkivonatok jogosultságokkal rendelkezik. A Service Bus ismer három alapvető jogokat: "Send" lehetővé teszi a "Figyelés" lehetővé teszi, hogy a fogadás, és a "Kezelése" lehetővé teszi, hogy a testreszabhatóvá entitások küld. SWT tokenek kifejezetten az AAD/ACS által kiadott jogcímeket ezeket a jogokat közé. Service Bus SAS-tokenje tekintse meg a konfigurált hello névtér vagy entitás toorules, és ezeket a szabályokat jogosultságokkal van konfigurálva. Aláíró hello jogkivonat hello kulccsal, hogy a szabályhoz társított így teszi hello token expressz hello jogaikat. egy entitás használatkor hello token *put-jogkivonat* engedélyek hello hello entitás / token jogok hello ügyfél toointeract kapcsolódnak. Egy hivatkozás, amelyben hello ügyfél időt vesz igénybe az hello *küldő* szerepkör szükséges jogosultsággal; "Send" hello hello véve *fogadó* szerepkör hello "Figyelő" jogosultság szükséges.

hello válaszüzenet rendelkezik hello következő *alkalmazástulajdonságok* értékek

| Kulcs | Optional | Érték típusa | Érték tartalma |
| --- | --- | --- | --- |
| állapotkód-: |Nem |int |HTTP-válaszkód **[RFC2616]**. |
| állapot-leírása |Igen |Karakterlánc |Hello állapot leírása. |

hello ügyfél *put-jogkivonat* ismételten és egyetlen entitás az üzenetkezelési infrastruktúra hello. hello jogkivonatok hatókörön belüli toohello jelenlegi ügyfélalkalmazást és rögzített hello aktuális kapcsolaton, azaz a hello kiszolgáló megszakítja bármely megtartott jogkivonatok amikor hello kapcsolat csökken.

hello aktuális Service Bus megvalósítása csak lehetővé teszi, hogy CBS együtt hello SASL metódus "Névtelen". SSL/TLS-kapcsolatot előzetes toohello SASL kézfogás mindig tartalmazniuk kell.

NÉVTELEN mechanizmus hello ezért támogatnia kell a kiválasztott AMQP 1.0-ügyfél hello. Névtelen hozzáférés azt jelenti, hogy a kezdeti kapcsolati kézfogás, beleértve a hello kezdeti munkamenet létrehozása hello anélkül, hogy tudnák, akik hello kapcsolatot hoz létre a Service Bus történik.

Miután hello kapcsolat és a munkamenet létrejött, csatolja hello csatolása toohello *$cbs* csomópont és a Küldés hello *put-jogkivonat* kérelem hello, csak az engedélyezett műveletek. Egy érvényes tokent sikeresen használatával állíthatók be a *put-jogkivonat* kérelem néhány entitás csomóponton belül 20 másodperc után hello kapcsolat létrejött, ellenkező esetben hello kapcsolat egyoldalúan megszakad Service Bus által.

hello ügyfél ezt követően felelős nyomon követése céljából jogkivonat lejáratáról. A Service Bus egy jogkivonat lejár, azonnal csökken minden kapcsolat hello kapcsolat toohello megfelelő entitás. tooprevent a, hello ügyfél hello token hello csomópont lecserélheti egy új virtuális hello keresztül bármikor *$cbs* felügyeleti csomópont hello azonos *put-jogkivonat* akár többérintéses kézmozdulatokkal is, és nélkül kapják meg a hello hello hasznos módja, hogy a különböző kapcsolatokon adatfolyamok forgalom.

## Következő lépések

toolearn AMQP, kapcsolatos további információkért látogasson el a következő hivatkozások hello:

* [Service Bus AMQP áttekintése]
* [Particionált Service Bus-üzenetsorok és témakörök AMQP 1.0 támogatása]
* [A Service Bus a Windows Server AMQP]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Service Bus AMQP áttekintése]: service-bus-amqp-overview.md
[Particionált Service Bus-üzenetsorok és témakörök AMQP 1.0 támogatása]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[A Service Bus a Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
