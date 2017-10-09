---
title: "aaaService busz aszinkron üzenetkezelési |} Microsoft Docs"
description: "Azure Service Bus aszinkron üzenetkezelési leírása."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: sethm
ms.openlocfilehash: 5ab6ddf052155a9dd975b413cfaf393119c1999d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Aszinkron üzenetkezelési minták és magas rendelkezésre állás

Számos különböző módon aszinkron üzenetkezelési megvalósítható. Az üzenetsorok, témakörök és előfizetések Azure Service Bus asynchronism a tároló és a továbbítási mechanizmus segítségével támogatja. A normál (szinkron) műveletet akkor üzenetek tooqueues és -témákból, üzeneteket küldjön és fogadjon a üzenetsorokból és -előfizetések. Írt alkalmazások ezeket az entitásokat mindig alatt a rendelkezésre álló függ. Hello entitás állapota megváltozik, miatt tooa különböző körülmények között, meg kell egy módon tooprovide olyan csökkentett funkció entitás, aki többségéhez felel meg.

Alkalmazások általában aszinkron üzenetkezelési minták tooenable több kommunikációs forgatókönyvet használja. Alkalmazások, amelyben ügyfelek küldhetnek üzeneteket tooservices, akkor is, ha hello szolgáltatás nem fut hozhat létre. Az, hogy a kommunikáció felszakadásáig alkalmazások várólista, adja meg a hely toobuffer kommunikációs betöltése szintű hello segítségével. Végezetül is ki lehet egy egyszerű, de hatékony terheléselosztási terheléselosztó toodistribute üzenetek több számítógépen.

Rendelés toomaintain rendelkezésre állási bármely ezeket az entitásokat fontolja meg az számos különböző módon, amelyben ezeket az entitásokat is megjelennek a tartós üzenetkezelő rendszer nem érhető el. Általánosságban véve hello entitás nem érhető el tooapplications azt írja be a következő különböző módokon hello lesz látható:

* Nem lehet toosend üzeneteket.
* Nem lehet tooreceive üzeneteket.
* Nem lehet toomanage entitások (létrehozása, beolvasása, frissítése vagy törlése entitások).
* Nem lehet toocontact hello szolgáltatást.

Minden ilyen hibához különböző meghibásodás vannak, amelyek lehetővé teszik az alkalmazás toocontinue tooperform munkahelyi csökkentett funkció szint. Például egy rendszer, amely képes üzeneteket küldeni, de nem kapott rendelések továbbra is kaphat az ügyfelektől, de nem tudja feldolgozni a rendeléseket. Ez a témakör ismerteti a lehetséges problémák kezeléséhez, amik akkor léphetnek fel, és hogyan ismertetünk problémák elhárításáról. A Service Bus vezetett egy megoldást, amely bírálhatja felül a számát, és ez a témakör emellett ismerteti a hello hello vonatkozó szabályokat használja az adott részt megoldást.

## <a name="reliability-in-service-bus"></a>A Service Bus megbízhatóság
Számos módon toohandle üzenet és egyéb entitások problémákat, és vannak-e azok mérséklési hello megfelelő használatára vonatkozó irányelveket. toounderstand hello útmutatást, először ismernie kell Mi a Service Bus sikertelen lehet. Azure rendszerek toohello kialakítása miatt ezek a problémák általában a rövid élettartamú toobe. Magas szinten hello különböző oka hiányában a következőképpen jelenik meg:

* Sávszélesség-szabályozás a Service Bus függ külső rendszerekből. Sávszélesség-szabályozás a tárolási és számítási erőforrásokat interakció következik be.
* A probléma a rendszert, amelyen a Service Bus függ. Például egy adott részét tárolási is problémák merülhetnek fel.
* Egyetlen alrendszeren Service Bus sikertelen. Ebben a helyzetben a számítási csomópont kaphat inkonzisztens állapotba kerülnek, és újra kell indítani magát, minden entitás, amely szolgál tooload egyenleg tooother csomópontok. Rövid idő alatt lassú üzenet feldolgozása pedig okozhatnak.
* Sikertelen a Service Bus egy Azure-adatközpontban belül. Ez a "Végzetes hiba" időintervalluma hello rendszer nem érhető el a rendszer hány perc vagy néhány óra.

> [!NOTE]
> hello kifejezés **tárolási** Azure Storage mind az SQL Azure azt.
> 
> 

A Service Bus azok mérséklési lehetőségeit a problémát számos tartalmaz. hello a következő szakaszok ismertetik az egyes a problémákra és a megfelelő megoldást.

### <a name="throttling"></a>Szabályozás
A Service busszal sávszélesség-szabályozás lehetővé teszi az együttműködési üzenet sebessége. Minden egyes Service Bus-csomópont Kezelőkód számos entitás. Processzor, memória, tárolási és egyéb értékkorlátozás hello rendszernek egyes entitásokból teszi iránti igények kielégítése érdekében. Ha ezek értékkorlátozás bármelyikét használati észlel, amely meghaladja a meghatározott küszöbértékeket, Service Bus megtagadhatja egy adott kérelem. hello hívó kap egy [ServerBusyException] [ ServerBusyException] és újrapróbálkozások 10 másodperc után.

A megoldás hello kódot kell olvasási hello hiba és bármely újrapróbálkozások hello üzenet halt legalább 10 másodpercet. Hello hiba akkor fordulhat elő, kódrészletek hello vevő alkalmazások között, mivel várható, hogy minden egyes adatra hello újrapróbálkozási logika egymástól függetlenül hajt végre. hello kód csökkentheti az üzenetsor vagy témakör particionálás engedélyezésével szabályozva hello valószínűségét.

### <a name="issue-for-an-azure-dependency"></a>Egy Azure-függőségének probléma
Az Azure egyéb összetevők alkalmanként rendelkezhet szolgáltatásokkal kapcsolatos problémákról. Például egy Service Bus használó rendszer frissítésekor a rendszer is ideiglenesen csökkentett képességeket tapasztal. ilyen jellegű problémák, a Service Bus rendszeresen körül toowork pedig megvizsgálja a, és azok mérséklési megvalósítja. Ezek azok mérséklési a hatásai szerepelhet. Például toohandle átmeneti tárolási érintő problémákat, a Service Bus egy rendszer, amely lehetővé teszi, hogy üzenet küldése műveletek toowork következetesen valósítja meg. Hello megoldás toohello természetétől miatt elküldött üzenet too15 perc tooappear hatással hello várólista vagy előfizetés eltarthat, és készen áll a fogadási művelet. A legtöbb entitások általánosságban véve nem fog tapasztalni a probléma. Azonban az Azure Service Bus entitások hello számát figyelembe véve, ez a megoldás néha szükség van a Service Bus-ügyfelek csak kis részét.

### <a name="service-bus-failure-on-a-single-subsystem"></a>A Service Bus hiba egyetlen alrendszeren
Minden más alkalmazáshoz körülmények között egy belső inkonzisztens a Service Bus toobecome összetevője okozhat. A Service Bus észleli ezt, ha adatait gyűjti hello alkalmazás tooaid megállapításában, mi történt. Hello adatgyűjtés, miután hello alkalmazás van-e indítani a egy kísérlet tooreturn azt tooa konzisztens állapotú. Ez a folyamat meglehetősen gyorsan végbemegy, és sokkal rövidebb jelenik meg toobe nem érhető el az tooa fel néhány percet, bár a tipikus üzemen kívül töltött idejét entitás eredményez.

Ezekben az esetekben hello ügyfélalkalmazás hoz létre egy [System.TimeoutException] [ System.TimeoutException] vagy [MessagingException] [ MessagingException] kivétel. A Service Bus tartalmaz a megoldás a probléma, az automatikus ügyfél újrapróbálkozási logika hello formában. Miután hello újrapróbálkozási időszak kimerül, és nem érkezik üdvözlőüzenetére, ismerje meg a más funkciókat használ, például a [névterek párosítva][paired namespaces]. Párhuzamos névterek ebben a cikkben említett egyéb korlátozásokkal rendelkezik.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Sikertelen volt-e a Service Bus egy Azure-adatközpontban belül
hello legvalószínűbb egy Azure-adatközpontban a hiba oka, hogy a sikertelen frissítés telepítése a Service Bus vagy egy függő rendszer. Mivel hello platform van jelen, hello annak valószínűségét, hogy ez a típus nem rendelkezik csökken. Datacenter hiba is megtörténhet, amely hello következők lehetnek az okai:

* Elektromos kimaradás (tápegység és eltűnnek előállítása power).
* A kapcsolat (internet break az ügyfelek és az Azure között).

Mindkét esetben a természetes és mesterséges katasztrófa hello hibát okozó. toowork kerülheti és győződjön meg arról, hogy továbbra is küldhet üzeneteket, használhat [névterek párosítva] [ paired namespaces] tooenable üzenetekben küldött toobe tooa másik helyen közben hello elsődleges hely újra kifogástalan állapotú legyen. További információkért lásd: [ajánlott eljárások az alkalmazások a Service Bus kimaradások és vészhelyzetek szigetelő][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Párosított névterek
Hello [névterek párosítva] [ paired namespaces] szolgáltatás forgatókönyveket támogat, amelyben egy Szolgáltatásbusz-entitás vagy az adatközponton belüli központi telepítési nem érhető el. Ez az esemény ritkán kerül sor, elosztott rendszerek továbbra is elő kell készíteni toohandle legrosszabb esetben forgatókönyvek. Általában ez az esemény történik, mert néhány elem, amelytől függ a Service Bus egy rövid távú problémát tapasztal. toomaintain rendelkezésre állásának kimaradás során a Service Bus felhasználók használhatják az lehetőleg külön adatközpontok, toohost a két különálló névtér az üzenetküldési entitások. Ez a szakasz többi hello hello terminológia a következő használja:

* Elsődleges névtér: hello névteret, amelyhez az alkalmazás együttműködik a küldési és fogadási műveletek.
* Másodlagos névtér: hello névtér, amely a biztonsági mentési toohello elsődleges névtér funkcionál. Úgy az alkalmazáslogikát nem működik együtt a névtérhez.
* Feladatátvételi időköz: hello elsődleges névtér toohello másodlagos névtér vált hello mennyi idő tooaccept normál hibák hello alkalmazása előtt.

Névterek támogatási párosítva *rendelkezésre állási küldése*. Rendelkezésre állási megőrzi az üdvözlő képességét toosend üzenetek küldése toouse küldési rendelkezésre állás, az alkalmazás hello követelményeknek kell megfelelniük:

1. Hello elsődleges névtér csak érkező üzenetek.
2. Üzenetsor vagy témakör megadott küldött üzenetek tooa érkeznek, előfordulhat, hogy nem megfelelő sorrendben.
3. Üzenet-munkameneten belül előfordulhat, hogy érkezésekor sorrendje. Ez a normál működésének munkamenetek szünetet. Ez azt jelenti, hogy az alkalmazás által munkamenetek toologically csoport üzeneteket.
4. A munkamenet-állapot kezelése a hello elsődleges névtér csak történik meg.
5. elsődleges queue hello online állapotba, és indítsa el az üzenetek fogadását, mielőtt hello másodlagos várólista lekéri az összes üzeneteket hello elsődleges várólistán.

hello a következő szakaszok ismertetik a hello API-k, hogyan vannak megvalósítva hello API-k és hello szolgáltatást használó példakód azt mutatja be. Vegye figyelembe, hogy vannak-e ez a szolgáltatás társított számlázási megvalósítását.

### <a name="hello-messagingfactorypairnamespaceasync-api"></a>hello MessagingFactory.PairNamespaceAsync API
hello párosított névterek funkció tartalmazza hello [PairNamespaceAsync] [ PairNamespaceAsync] hello metódusa [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] osztály:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Hello feladat befejezése után hello névtér párosítás egyben bármely után elkészültét és tooact [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient], vagy [TopicClient] [ TopicClient] hello létre [MessagingFactory] [ MessagingFactory] példány. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] [ Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] hello alaposztály hello különböző típusú párosítása, hogy van egy [MessagingFactory] [ MessagingFactory] objektum. Hello csak osztály származtatott jelenleg nevű [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], amely megvalósítja az hello küldési rendelkezésre állási követelményeinek. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] tartozik egy konstruktorokat, amelyek létrehozása másik kiszolgálón. Megnézi hello konstruktor hello a legtöbb paraméterek, értelmezni lehet hello hello viselkedését más konstruktor.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Ezek a paraméterek jelentése a következő hello rendelkezik:

* *secondaryNamespaceManager*: egy inicializált [NamespaceManager] [ NamespaceManager] hello másodlagos névtér példányt adott hello [PairNamespaceAsync] [ PairNamespaceAsync] módszer használható tooset hello másodlagos névtér fel. hello névtér manager rendszer használt tooobtain hello hello névtér és, hogy létezik-e szükséges hello várakozó várólisták toomake várólistákból. Ha ezek a várólisták nem léteznek, a rendszer hozza létre őket. [NamespaceManager] [ NamespaceManager] hello képességét toocreate hello jogkivonatot igényel **kezelése** jogcímek.
* *messagingFactory*: hello [MessagingFactory] [ MessagingFactory] hello másodlagos névtér-példányt. Hello [MessagingFactory] [ MessagingFactory] objektum használt toosend, és ha hello [EnableSyphon] [ EnableSyphon] tulajdonsága túl**igaz** , hello várakozó várólisták érkező üzenetek fogadására.
* *backlogQueueCount*: hello várakozó fájlok számát a várólisták toocreate. Ennek az értéknek legalább 1 kell lennie. Toohello várakozó üzenetek küldésekor ezeket a várólistákat egyik véletlenszerűen választott. Ha hello érték too1, majd csak egy sor valaha is használható. Ez akkor fordul elő, és hello egy várakozó várólista hibák állít elő, amikor hello ügyfél nem tud tootry egy másik várakozó várólista és meghiúsulhat toosend az üzenetet. Azt javasoljuk, hogy az érték toosome nagyobb érték és az alapértelmezett hello érték too10 beállítása. A magasabb tooa módosíthatja, vagy attól függően, hogy mennyi adatot alacsonyabb értéket az alkalmazás küld naponta. Minden tartalék várólista be az üzenetek too5 GB tárolására képes.
* *failoverInterval*: hello időn időintervalluma fogad hello elsődleges névtéren hibák előtt minden egyetlen entitás váltás toohello másodlagos névtér fölé. A feladatátvétel entitás által entitás alapon. Egyetlen névtér az entitás gyakran megmarad a Service Bus egy másik csomópontja. Egy entitás hibája miatt nem feltétlenül jelenti azt egy másik hibát. Ez az érték túl állíthatja be[System.TimeSpan.Zero] [ System.TimeSpan.Zero] toofailover toohello másodlagos közvetlenül az első, nem átmeneti hiba után. Hello feladatátvételi időzítő kiváltó hibák a [MessagingException] [ MessagingException] a mely hello [IsTransient] [ IsTransient] tulajdonság értéke HAMIS, vagy egy [System.TimeoutException][System.TimeoutException]. Más kivételek, például a [UnauthorizedAccessException] [ UnauthorizedAccessException] nem okozhatja feladatátvétel esetén, mert azt jelzik, hogy az ügyfél által hello helytelenül van konfigurálva. A [ServerBusyException] [ ServerBusyException] nem nem ok feladatátvételi mert hello helyes mintát toowait 10 másodpercet, majd küldje el üdvözlőüzenetére újra.
* *enableSyphon*: azt jelzi, hogy az adott párosítás kell is syphon üzenetek hello másodlagos névtér hátsó toohello elsődleges névtér. Általában alkalmazások által küldött üzenetek kell beállítania ezt az értéket túl**hamis**; alkalmazásokat, amelyek az üzenetek fogadásához-et kell beállítania ezt az értéket túl**igaz**. hello ennek oka, hogy gyakran, nincsenek üzenetküldők-nál kevesebb üzenetet fogadó. Hello több fogadóval dönthet úgy toohave egy egyetlen alkalmazás példány leíró hello Szifonos feladatokat. Segítségével számos fogadók számlázási hatással van a minden várakozó várólista.

toouse hello kódot, és hozzon létre egy elsődleges [MessagingFactory] [ MessagingFactory] példány, egy másodlagos [MessagingFactory] [ MessagingFactory] példány, egy másodlagos [NamespaceManager] [ NamespaceManager] példányt, és egy [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] példány. hello hívás egyszerűen hello következő lehet:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Ha a feladat hello által visszaadott hello [PairNamespaceAsync] [ PairNamespaceAsync] metódus befejeződött, minden be van állítva, és készen áll a toouse. Mielőtt hello feladat lett visszaadva, akkor előfordulhat, hogy nem befejeződött összes hello háttérműveletek toowork jobb párosítás hello szükséges. Emiatt nem webalkalmazásokba üzenetküldésre, amíg hello tevékenység adja vissza. Hiba történt, például rossz hitelesítő adatait, vagy hiba toocreate hello várakozó várólisták, ha a kivételek fog jelezni, hello feladat befejeződése után. Ha ismét a hello feladat, ellenőrizze, hogy hello várólisták volt található vagy hello megvizsgálásával létrehozott [BacklogQueueCount] [ BacklogQueueCount] tulajdonságát a [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] példány. A kód megelőző hello ennek a műveletnek a következőképpen jelenik meg:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Service Bus üzenetkezelés aszinkron hello alapjait, olvassa el, további részleteket [névterek párosítva][paired namespaces].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md
