---
title: "hello szolgáltatás megbízható Hálószolgáltatás programozási modell aaaOverview |} Microsoft Docs"
description: "További tudnivalók a Service Fabric megbízható programozási modell, valamint Ismerkedés a saját szolgáltatások írása."
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: masnider;
ms.openlocfilehash: 41d1826df902b1f1845c4702bf2567e6b9ca1f1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-overview"></a>A Reliable Services áttekintése
Az Azure Service Fabric leegyszerűsíti az írást, és az állapotmentes és állapotalapú Reliable Services kezelése. Ez a témakör ismerteti:

* hello Reliable Services programozási modellt állapotmentes és állapotalapú szolgáltatással.
* hello lehetősége van toomake működnie írása közben.
* Bizonyos esetekben és példák a toouse megbízható szolgáltatását, és megírásának módjától.

Megbízható szolgáltatások az egyik programozási modellek érhető el a Service Fabric hello. más hello hello megbízható szereplő programozási modell, amely fölött hello Reliable Services modell virtuális szereplő programozási modellt biztosít. Hello Reliable Actors programozási modell további információkért lásd: [bemutatása tooService háló Reliable Actors](service-fabric-reliable-actors-introduction.md).

Kezeli a Service Fabric hello élettartamát szolgáltatások kiépítése és frissítését és törlését, a központi telepítés keresztül [Service Fabric Alkalmazáskezelés](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Mik azok a Reliable Services?
Megbízható szolgáltatások lehetővé teszi egy egyszerű, hatékony, a legfelső szintű programozási modell toohelp, mi az az alkalmazás fontos tooyour express. Hello Reliable Services programozási modell beolvasása:

* Hozzáférés toohello részeinek hello Service Fabric programozási API-k. Service Fabric szolgáltatások modellezve eltérően [Vendég végrehajtható fájlok](service-fabric-deploy-existing-app.md), Reliable Services toouse hello többi hello Service Fabric API-k közvetlenül lekérni. Ez lehetővé teszi, hogy a szolgáltatások:
  * Lekérdezés hello rendszer
  * a jelentés állapotának kapcsolatos hello fürt entitások
  * konfigurációs és a kód módosítása értesítések fogadása
  * Keresse meg és más szolgáltatásokkal kommunikálni
  * (opcionális) használja a hello [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md)
  * .. engedélyezése és hozzáférési toomany egyéb funkciói mellett minden a számos programozási nyelven első osztályú programozási modellt.
* A futó kód, amely egyszerű modell néz programozási modellek segítségével meg. A kód jól meghatározott belépési pont és egyszerűen kezelhető életciklus rendelkezik.
* A moduláris kommunikációs modellt. Az Ön által választott, például a HTTP és a hello átvitelt használ [Web API](service-fabric-reliable-services-communication-webapi.md), websocket elemeket, egyéni TCP protokollt, vagy bármely más. Megbízható szolgáltatások néhány nagy out-of-az-box beállítások is használhatja, vagy megadhatja a saját adja meg.
* Állapotalapú szolgáltatások esetén hello Reliable Services programozási modell lehetővé teszi a tooconsistently és megbízhatóan tárolja az állapot jobbra a szolgáltatás belső használatával [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md). Megbízható gyűjtemények olyan egyszerű magas rendelkezésre állású és megbízható gyűjteményosztály, amely jól ismert tooanyone, aki használta a C# gyűjtemények lesz. Hagyományosan szolgáltatások megbízható állapotkezelés szükséges külső rendszerekkel. Megbízható gyűjteményeket is tárolhatja a állapot következő tooyour számítási hello az azonos magas rendelkezésre állásának és megbízhatóságának származnak tooexpect megismerte a magas rendelkezésre állású külső tárolókat. Ebben a modellben a késés is támogatja, mert meg vannak elhelyezése hello számítási és toofunction kell állapota.

Ezt a videót a Microsoft Virtual Academy Reliable services áttekintése:<center>
<a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=HhD9566yC_4106218965">
<img src="./media/service-fabric-reliable-services-introduction/ReliableServicesVid.png" WIDTH="360" HEIGHT="244" />
</a>
</center>

## <a name="what-makes-reliable-services-different"></a>Hasznossá a Reliable Services különböző?
A Service Fabric Reliable Services szolgáltatások előtt lehet, hogy írt eltérnek. A Service Fabric megbízhatóságát, rendelkezésre állási, konzisztencia- és méretezhetőséget biztosít.

* **Megbízhatóság** – a szolgáltatás marad fel még akkor is, ha nem a gépeket, vagy kattintson a hálózati problémák, nem megbízható környezetek vagy azokban az esetekben, ahol hello szolgáltatások maguk előforduló hibák és összeomlások vagy sikertelen. Állapotalapú szolgáltatások esetén a megőrzi a állapotát, még akkor is, hálózati vagy más hibák hello jelenlétét.
* **Rendelkezésre állási** – a szolgáltatás elérhetőségét és a gyors. Service Fabric fenntartja a másolatok futó kívánt száma.
* **Méretezhetőség** - szolgáltatások rendszer leválasztja a adott hardverekhez, és növekedhet és zsugorítása keresztül hello hozzáadását és eltávolítását hardver- vagy egyéb erőforrásokat szükség szerint. Szolgáltatások könnyen particionált (különösen a hello állapot-nyilvántartó eset) tooensure hello szolgáltatást is méretezhető és leíró részleges hibák. Szolgáltatások hozhatók létre, és dinamikusan keresztül kódot hoz létre, szükség esetén további példányokat toobe engedélyezése törölt mondja ki a válasz toocustomer kérésekben. Végezetül a Service Fabric szolgáltatások toobe lightweight javasolja. A Service Fabric lehetővé teszi, hogy a szolgáltatások toobe belül egyetlen folyamat kiépített helyett az kötelezik vagy teljes operációs rendszer példányok vagy folyamatok tooa egyetlen szolgáltatáspéldány, hogy több ezer.
* **Konzisztencia** -a szolgáltatásban tárolt összes adatot garantálható toobe konzisztens. Ez érvényét veszti, még akkor is a szolgáltatáson belül több megbízható gyűjtemények között. A szolgáltatásokon belüli gyűjtemények között módosításokat hajthat atomi tranzakciós úton módon.

## <a name="service-lifecycle"></a>Szolgáltatás életciklusa
Hogy a szolgáltatási állapot-nyilvántartó vagy állapotmentes, a Reliable Services adjon meg egy egyszerű életciklussal, amely lehetővé teszi, hogy gyorsan beépülő modul a kódot és az első lépések.  Módon csak egy vagy két telepíteni kell, amely tooimplement tooget a szolgáltatás használatra.

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** – Ez a módszer, ahol hello szolgáltatást határozza meg, hogy szeretne-e toouse hello kommunikációs stack(s). például a kommunikációs verem hello [Web API](service-fabric-reliable-services-communication-webapi.md), a figyelő végpont hello Mi határozza meg, vagy végpontjainak hello szolgáltatást (hogyan ügyfelek elérni hello szolgáltatás). Is meghatározza, hogyan működnek együtt megjelenő köszönőüzenetei hello többi hello szolgáltatás kódot.
* **RunAsync** – Ez a módszer, ahol a szolgáltatás fut-e az üzleti logika, és amennyiben azt bármely háttérfeladatok hello élettartama hello szolgáltatást kell futtató indítsa lenne. hello cancellation token biztosított egy jel amikor munka le kell állnia. Például ha hello szolgáltatást kell egy megbízható várósorból kirakni toopull üzeneteket, és dolgozza fel őket, ez az adott munka történik.

Ha Ön most megtanulni hello megbízható szolgáltatások először, olvassa el a! Részletes útmutató hello életciklus megbízható szolgáltatásokat keres, ha akkor is lépjen túl[Ez a cikk](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Példa szolgáltatások
Ezen programozási modell ismerete, hogyan ezek illeszkednek egymáshoz az egyes két különböző szolgáltatások toosee gyorsan át vessen.

### <a name="stateless-reliable-services"></a>Állapot nélküli megbízható szolgáltatások
Egy állapotmentes szolgáltatások akkor egy, ha nincs állapot között hívások belül hello szolgáltatás tartani. Található állapotokat teljesen rendelkezésre álló, és nem igényel, szinkronizálás, a replikáció, a adatmegőrzési vagy a magas rendelkezésre állású.

Tegyük fel, amelynek nincs elég memória, és minden feltételek és a műveletek tooperform egyszerre kap egy Számológép.

Ebben az esetben hello `RunAsync()` (C#) vagy `runAsync()` (Java) hello szolgáltatást lehet üres, mivel nincs háttér tevékenység – a feldolgozás hello szolgáltatás toodo kell-e. Hello Számológép szolgáltatás létrehozásakor adja vissza egy `ICommunicationListener` (C#) vagy `CommunicationListener` (Java) (például [Web API](service-fabric-reliable-services-communication-webapi.md)), megnyílik egy figyelő végpont néhány port. Ez a figyelő végpont toohello különböző számítási módszereket csatlakozik (Példa: "(n1, n2) hozzáadása") definiáló hello Számológép nyilvános API.

Egy ügyfél a rendszer telefonhívást indít, amikor hello megfelelő módszert meghívták, és hello Számológép szolgáltatás hello megadott adatokat, és hello eredményt adja vissza, hello műveleteket hajt végre. Az olyan állapotokat nem tárolja.

Ne tárolja az összes belső állapot is leegyszerűsíti a példa Számológép. De a legtöbb szolgáltatások nem valóban állapot nélküli. Ehelyett azok externalize az állapot toosome más tárolására. (Például a webes alkalmazás, amely megőrzi a munkamenet-állapot biztonsági vagy gyorsítótár támaszkodik nincs állapotmentes.)

Hogyan állapotmentes szolgáltatások ilyenek például a Service Fabric vannak használatban, mint egy előtér-, hogy tesz elérhetővé hello nyilvánosan elérhető API-t a webalkalmazást. hello előtér-szolgáltatás majd kiszolgálóhoz toostateful szolgáltatások toocomplete a felhasználó kérésére. Ebben az esetben ügyfelektől érkező hívások olyan irányított tooa ismert port, pl. 80, ahol hello állapotmentes szolgáltatások figyel. Az állapotmentes szolgáltatások hello hívást kap, és meghatározza azokat hello hívás a megbízható entitások van-e, és azt kiszolgálni szánt.  Ezután hello állapotmentes szolgáltatások hello hívás toohello megfelelő partíció hello állapotalapú szolgáltatás továbbítja, és választ vár. Hello állapotmentes szolgáltatások választ kap, amikor az válaszol az ügyfélgépről toohello eredeti ügyfél. Az általunk biztosított mintákat van ilyen szolgáltatás például [C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Actors/VisualObjectActor/VisualObjectWebService). Ez csak egy példa az ebben a mintában a hello mintában, valamint egyéb minták mások szerepelnek.

### <a name="stateful-reliable-services"></a>Állapotalapú Reliable Services
Az állapotalapú szolgáltatás egyike, hogy az egy egységes és hello szolgáltatás toofunction tartozó állapot részét. Érdemes lehet olyan szolgáltatás, amely folyamatosan kiszámítja az egyes frissítések kap alapján érték mozgóátlaga. toodo, kell rendelkeznie hello aktuális tooprocess szükséges bejövő kérelmek és aktuális átlagos hello. Bármely szolgáltatás beolvassa, feldolgozza és információkat tárol egy külső tároló (például egy Azure blob vagy tábla tároló még ma) egy állapotfüggő. Csak tartja állapotában az külső hello tárolóban.

A legtöbb szolgáltatások ma tárolására állapotukra kívülről, mivel hello külső áruházban megbízhatóság, rendelkezésre állását, méretezhetőségét és konzisztencia tartalma az adott állapotban. A Service Fabric-szolgáltatások nem szükséges toostore állapotukra kívülről. A Service Fabric ezeket a követelményeket mind a hello szolgáltatást kód hello szolgáltatás állapotának gondoskodik.

> [!NOTE]
> Állapotalapú Reliable Services támogatása Linux még (a C# vagy Java) nem érhető el.
>

Tegyük fel, azt szeretnénk, ha egy szolgáltatás, amely feldolgozza a képek toowrite. toodo ezt, a rendszerképen átalakítások tooperform lemezképet és hello sorozatát hello szolgáltatás vesz igénybe. Ez a szolgáltatás adja vissza egy kommunikációs figyelő (Most tegyük fel, hogy egy WebAPI), hogy az API-k tesz elérhetővé, például `ConvertImage(Image i, IList<Conversion> conversions)`. Ha kap, hello szolgáltatás tárolja el azt a egy `IReliableQueue`, és néhány azonosító toohello ügyfél adja vissza, így akkor is hello kérelmek nyomon követésére.

Ez a szolgáltatás a `RunAsync()` összetett lehet. hello szolgáltatás esetében egy hurok található az `RunAsync()` , amely lekéri a kérelmek `IReliableQueue` és hajtja végre a kért hello átalakításra. hello eredmények lekérése tárolja egy `IReliableDictionary` , hogy mikor hello ügyfél ismét elérhető lesz a konvertált képek kaphatnak. tooensure, amely akkor is, ha valami nem sikerül hello kép nem vesznek el, a megbízható szolgáltatás volna hello várósorból kirakni lekéréses, hello átalakítást végrehajtani, hello eredménye egy tranzakción belül minden tárolja. Ebben az esetben üdvözlőüzenetére hello várólista törlődik, és hello eredmények tárolódnak hello eredmény szótár csak akkor, ha hello átalakítások be nem fejeződik. Azt is megteheti hello szolgáltatás sikerült hello kép hello várósorból kirakni lekéréses, és azonnal távoli tárolójában tárolja. Ez csökkenti a hello hello állapotszolgáltatás mennyisége toomanage rendelkezik, de növeli a összetettségét, mivel hello szolgáltatás csak a tookeep hello szükséges metaadatok toomanage hello távoli tároló. Bármelyik módszert használja Ha valami nem tudott hello középső hello kérelem marad hello várólista várakozási toobe feldolgozása.

Ezzel a szolgáltatással kapcsolatos egyik dolog toonote, úgy tűnik, mint egy normál .NET szolgáltatás! hello csak különbség az, hogy hello adatstruktúrákhoz használt (`IReliableQueue` és `IReliableDictionary`) a Service Fabric által biztosított, és megbízható, elérhető, és egységes.

## <a name="when-toouse-reliable-services-apis"></a>Amikor toouse megbízható szolgáltatások API-k
Ha a alkalmazás szolgáltatási igények hello következő jellemző, akkor érdemes lehet megbízható szolgáltatások API-kat:

* Szeretné, hogy a szolgáltatás kódot (és nem kötelezően állapot) toobe magas rendelkezésre állású és megbízható
* Tranzakciós garanciák kell több egységre is kiterjed (például rendeléseket és a sorrend) állapot között.
* Az alkalmazás állapota természetes modellezhető megbízható szótárakat és a várólisták.
* Az alkalmazások kód vagy állapot kell a kis késleltetésű olvasási és írási magas rendelkezésre állású toobe.
* Az alkalmazás toocontrol hello párhuzamossági vagy tranzakciós műveletek granularitása kell egy vagy több megbízható gyűjtemények között.
* Érdemes toomanage hello kommunikáció vagy a szolgáltatás a séma particionálás vezérlő hello.
* A kódot kell szabadszálas futtatókörnyezetben.
* Az alkalmazás toodynamically kell létrehozni, vagy szüntesse meg a megbízható szótárak vagy várólisták, vagy teljes szolgáltatások futási időben.
* A szolgáltatás állapotának tooprogrammatically Alkalmazásvezérlési Service Fabric által biztosított biztonsági mentési és helyreállítási funkciókat van szükség.
* Az alkalmazás igényeinek toomaintain változások nyomon követése az egységek állapot.
* Szeretné, hogy toodevelop vagy harmadik féltől származó fejlett, egyéni állapotszolgáltatója felhasználását.

## <a name="next-steps"></a>Következő lépések
* [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
* [Megbízható szolgáltatás használati speciális](service-fabric-reliable-services-advanced-usage.md)
* [hello Reliable Actors programozási modell](service-fabric-reliable-actors-introduction.md)
