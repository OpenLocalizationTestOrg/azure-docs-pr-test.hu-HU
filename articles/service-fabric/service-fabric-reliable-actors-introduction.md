---
title: "aaaService háló megbízható szereplője áttekintése |} Microsoft Docs"
description: "Bevezetés toohello Service Fabric Reliable Actors programozási modell."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab010cbf936c6cf723b3d453ef95a9bf51f76c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooservice-fabric-reliable-actors"></a>Bevezetés tooService háló Reliable Actors
Reliable Actors a Service Fabric alkalmazási keretrendszer hello alapján [virtuális szereplő](http://research.microsoft.com/en-us/projects/orleans/) mintát. hello megbízható Actors API skálázhatóságát és megbízhatóságát Service Fabric által nyújtott garanciák hello épülő programozási egyszálas modellt biztosít.

## <a name="what-are-actors"></a>Mik azok a szereplője?
Egy szereplő a számítási műveletek és rendelkező egyszálas végrehajtási állapot egy elkülönített, független egység. Hello [szereplő mintát](https://en.wikipedia.org/wiki/Actor_model) olyan számítási modellt egyidejű vagy elosztott rendszerek, amelyek nagyszámú ezek szereplője végrehajtható egyidejűleg, és egymástól függetlenül. Szereplője kommunikálhatnak egymással, és további szereplője hozhatnak létre.

### <a name="when-toouse-reliable-actors"></a>Amikor toouse Reliable Actors
Service Fabric Reliable Actors hello szereplő kialakítási mintája megvalósítása. A szoftver a kialakítási mintában a hello döntés, hogy toouse egy adott minta történik-e a szoftverfrissítések tervezése probléma alapján megfelelő hello mintát.

Bár hello szereplő kialakítási mintában a helyes illeszkedő tooa, számát, elosztott rendszerek problémák és forgatókönyvek alapos megfontolás hello megkötéseket hello mintát és hello keretrendszer végrehajtási készült. Általános útmutatást fontolja meg a hello szereplő mintát toomodel a probléma vagy forgatókönyv ha:

* A probléma tárhely magában foglalja a sok (több ezer vagy több) kis, független és elkülönített állapot és a logikai egységek.
* Azt szeretné, hogy a külső összetevők, beleértve az állapot lekérdezése szereplője csoportja között jelentős beavatkozást nem igénylő egyszálas objektummal toowork.
* A szereplő példányok nem blokkolja a hívók előre nem látható késlelteti az i/o-műveletek kiállításával.

## <a name="actors-in-service-fabric"></a>A Service Fabric actors
A Service Fabric szereplője valósíthatók meg hello Reliable Actors keretrendszer: a beépített szereplő minta-alapú alkalmazás-keretrendszer [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md). Minden megbízható szereplő szolgáltatás ír egy ténylegesen egy particionált, állapot-nyilvántartó megbízható szolgáltatás.

Minden aktor egy szereplő típusú típusúként van definiálva, azonos toohello egy .NET-objektum módja a .NET-típus példánya. Például előfordulhat, hogy egy szereplő típus, amely megvalósítja a Számológép hello funkcióit, és előfordulhat, hogy sok szereplője az adott típusú fürt különböző csomópontokon elosztott. Minden ilyen szereplő egyedileg azonosít egy szereplő.

### <a name="actor-lifetime"></a>Aktor élettartama
A Service Fabric szereplője virtuális, ami azt jelenti, hogy az élettartamuk nem kapcsolt tootheir memórián belüli ábrázolását. Ennek eredményeképpen nincs szükségük toobe explicit módon létrehozott vagy megsemmisül. hello Reliable Actors futásidejű automatikusan aktiválja az aktor hello először kap egy adott szereplő azonosítóval. Ha egy szereplő nem használják-e egy adott időn belül, hello Reliable Actors futásidejű szemétgyűjtési-gyűjti hello memórián belüli objektum. Is megőrzi a hello szereplő fenntartása ismerete kell azt toobe később újra aktiválni kell. További részletekért lásd: [szereplő életciklust és a szemétgyűjtési gyűjtemény](service-fabric-reliable-actors-lifecycle.md).

A virtuális szereplő élettartama absztrakciós hello virtuális szereplő modell eredményeképpen bizonyos korlátozásokkal tartozik, és ténylegesen hello Reliable Actors megvalósítási néha eltér a modell.

* Egy szereplő automatikusan aktiválódik (ami egy szereplő objektum toobe összeállított) hello először egy üzenetet kap tooits szereplő azonosítóját. Néhány bizonyos idő eltelte után hello szereplő objektum szemétgyűjtő. Hello a jövőben azonosítójával hello szereplő újra, összeállított objektum toobe eredményezi egy új aktor. Egy aktorállapot outlives hello objektum élettartama az állapotkezelő hello található.
* Adott szereplő bármely aktormetódus hívja a szereplő Azonosítóval aktiválja. Emiatt a szereplő típusokhoz hello futásidejű implicit módon hívja az konstruktor. Ezért Ügyfélkód nem adható át paraméterek toohello szereplő típus konstruktora, bár paraméterek teljesíthetők toohello szereplő konstruktor hello szolgáltatás magát. hello eredménye, hogy gyakrabban lehet úgy részben inicializált állapotban hello időpontjára más módszerekkel hívják meg, ha hello szereplő hello ügyfélről inicializálási paramétereket igényel. Nincs egyetlen belépési pont egy szereplő hello ügyfélről hello aktiválásához.
* Bár a Reliable Actors implicit létrehozása szereplő objektumok; hello képességét tooexplicitly szereplő és állapotában törlése rendelkeznek.

### <a name="distribution-and-failover"></a>Telepítési és a feladatátvétel
tooprovide skálázhatóságát és megbízhatóságát, a Service Fabric szereplője hello fürt alatt, és automatikusan továbbítja őket a meghibásodott csomópontok toohealthy megfelelően szükség szerint áttelepíti. Ez az absztrakciós keresztül egy [particionált, állapot-nyilvántartó megbízható szolgáltatás](service-fabric-concepts-partitioning.md). Terjesztési, méretezhetőség, megbízhatóság és automatikus feladatátvételt összes biztosított hello tény szereplője belül futó hello nevű állapot-nyilvántartó megbízható szolgáltatás alapján *szereplő szolgáltatás*.

Szereplője hello partíciója hello szereplő szolgáltatás különböző pontjain, és ezek a partíciók elosztott hello a Service Fabric-fürt csomópontja. Minden szolgáltatás partíció szereplője tartalmaz. Kezeli a Service Fabric terjesztési és feladatátvételi hello szolgáltatást a partíciók száma.

Például egy szereplő szolgáltatás kilenc partíciókkal rendelkező telepített toothree hello alapértelmezett szereplő partíció elhelyezési használt csomópontok thusly volna terjeszteni:

![Megbízható szereplője terjesztési][2]

hello szereplő keretrendszer partíciós séma és a kulcs tartomány beállításainak kezelése meg. Ez egyszerűbbé teszi a néhány, de is hordoz magában, ha néhány szempont:

* Megbízható szolgáltatások lehetővé teszi toochoose particionálási séma, kulcs tartományon (particionálási sémát széles használatakor), és a partíció száma. Reliable Actors korlátozott toohello tartomány particionálási sémát (hello egységes Int64 sémával), és igényel a hello teljes Int64 kulcs címtartományt használhat.
* Alapértelmezés szerint szereplője véletlenszerűen kerülnek, ami egységes terjesztési partíciókra.
* Szereplője véletlenszerűen kerülnek, mert ez várható szereplő műveletek mindig megkövetelik a hálózati kommunikációt, beleértve a szerializálás és a deszerializálás metódus hívása adatok nélül késleltetés és a terhelést.
* Speciális forgatókönyvek, az lehetséges toocontrol szereplő partíció elhelyezési Int64 szereplő, amelyek kapcsolódnak toospecific partíciókat azonosítók használatával is. Azonban ezzel úgy eredményezheti egy egyenetlen eloszlását szereplője partíciók között.

Aktorszolgáltatások particionálásáról további információkért tekintse meg túl[fogalmak particionálás szereplője](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

### <a name="actor-communication"></a>Aktor kommunikáció
Aktor kapcsolati definiált hello szereplő hello felületet megvalósító, és lekérdezi a proxy tooan szereplő keresztül hello hello ügyfél által közösen használt illesztőfelület ugyanazon a felületen. Mivel ez az interfész használt tooinvoke szereplő módszerek aszinkron módon történik, hello felület minden metódusa feladatot visszaadó kell lennie.

Metódus meghívásához és a válaszok végső soron eredményez hálózati kérelmek hello fürt, Igen hello argumentumok és visszatérési lenniük szerializálható hello platform hello feladatok hello eredménytípusai között. Különösen kell [adategyezmény-szerializálható](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

#### <a name="hello-actor-proxy"></a>hello szereplő proxy
hello Reliable Actors ügyfél API-ja biztosít szereplő példány és egy szereplő ügyfél közötti kommunikációhoz. egy résztvevővel toocommunicate, egy ügyfél szereplő proxy hello szereplő felületet megvalósító objektum hoz létre. hello szereplő hello proxy objektumon hívása módszerekkel hello ügyfél kommunikál. hello szereplő proxy ügyfél-aktor és aktor szereplő kommunikációhoz használható.

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on hello actor. If an actor with hello given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

```java
// Create actor ID with some name
ActorId actorId = new ActorId("Actor1");

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
MyActor myActor = ActorProxyBase.create(actorId, new URI("fabric:/MyApp/MyActorService"), MyActor.class);

// This will invoke a method on hello actor. If an actor with hello given ID does not exist, it will be activated by this method call.
myActor.DoWorkAsync().get();
```


Vegye figyelembe, hogy a két hello adatra toocreate hello szereplő proxy objektum használja a következők: hello szereplő azonosító és hello alkalmazás neve. hello szereplő azonosító egyedileg azonosítja hello szereplő hello alkalmazás neve azonosítja a hello [Service Fabric-alkalmazás](service-fabric-reliable-actors-platform.md#application-model) hello szereplő telepítési helyét.

Hello `ActorProxy`(C#) / `ActorProxyBase`(Java) osztály hello ügyféloldalon hello szükséges feloldási toolocate hello szereplő azonosító alapján hajtja végre, és nyissa meg azt a kommunikációs csatornát. Kommunikációs hibák és a feladatátvétel hello esetekben toolocate hello szereplő is újrapróbálkozik. Ennek eredményeképpen a üzenetkézbesítést hello a következő jellemzőkkel rendelkezik:

* Üzenet kézbesítési a lehető legkedvezőbb módon.
* Gyakrabban is duplikált üzenetek fogadása hello ugyanazt az ügyfelet.

### <a name="concurrency"></a>Egyidejűség
hello Reliable Actors futásidejű szereplő módszerek eléréséhez egyszerű kapcsolja-alapú hozzáférés modellt biztosít. Ez azt jelenti, hogy legfeljebb egy szál lehet aktív belső az aktor objektum kódot bármikor. Kapcsolja a szerepköralapú hozzáférés egyidejű rendszerek jelentősen egyszerűbb, mivel nincs szükség a szinkronizálási mechanizmus az adatelérési. Azt is jelenti, rendszerek minden szereplő példány hello egyszálas hozzáférés jellegű szempontot kell megtervezni.

* A szereplő egyetlen példánya több kérelem nem dolgozható fel egyszerre. Aktor példánya okozhat a teljesítmény szűk keresztmetszetek várt toohandle egyidejű kérelmek esetén.
* Gyakrabban is kölcsönös kizárás egymástól, ha két szereplője, amíg egy külső kérelem tooone hello szereplője az egyidejűleg között körkörös kérelmet. hello szereplő futásidejű lesz automatikusan idő ki szereplő hívja, és egy kivétel toohello hívó toointerrupt holtpont lehetséges helyzetben kivételt.

![Megbízható szereplője kommunikációt][3]

#### <a name="turn-based-access"></a>Kapcsolja a szerepköralapú hozzáférés
Egy teljes végrehajtási hello más szereplője vagy az ügyfelek a válasz tooa kérelemben szereplő metódus, vagy hello teljes végrehajtását tartalmaz egy [időzítő/emlékeztető](service-fabric-reliable-actors-timers-reminders.md) visszahívás. Annak ellenére, hogy ezen metódusok és a visszahívások aszinkron, hello szereplője futásidejű nem interleave őket. Egy teljesen kész kell, mielőtt új kapcsolja engedélyezett. Más szóval egy szereplő metódus vagy időzítő/emlékeztető visszahívást, amelyet éppen végrehajtás alatt teljesen befejezése előtt egy új tooa telefonhívás vagy visszahívási engedélyezett. A metódus vagy a visszahívási tekinteni toohave befejeződött, ha hello rendelkezik adott eredményül hello metódusból vagy hello metódust vagy a visszahívási által visszaadott visszahívási és hello feladat befejezése után. Érdemes fogalmazás, hogy kapcsolja-alapú feldolgozási tiszteletben tartják még különböző módszereket, időzítők és visszahívások között.

hello szereplője futásidejű kapcsolja-alapú feldolgozási kikényszeríti által az beszerzése egy aktoronkénti zárolásra egy kapcsolja a hello elején, és kapcsolja hello hello végén hello zárolás feloldása. Ebből kifolyólag kapcsolja-alapú feldolgozási aktoronkénti alapon és szereplője között nem érvényes. Aktor módszerek egyidejűleg végrehajtható időzítő/emlékeztető visszahívások, különböző szereplője nevében.

a következő példa hello fent fogalmak hello mutatja be. Vegye figyelembe az aktor típusa, amely megvalósítja az két aszinkron metódusok (tegyük fel például, *Method1* és *Method2*), időzítő, valamint egy emlékeztető. az alábbi ábrán hello hello végrehajtásra ezen módszerek és a visszahívások két szereplője nevében ütemterv példáját mutatja be (*ActorId1* és *ActorId2*), amely toothis szereplő típus tartozik.

![Megbízható szereplője futásidejű kapcsolja-alapú feldolgozási és a hozzáférés][1]

Ez az ábra ezeket a szabályokat követi:

* Minden egyes függőleges vonal szemlélteti hello logikai metódust, illetve egy visszahívás végrehajtása egy adott szereplő nevében.
* minden egyes függőleges vonal tüntetve hello esemény újabb régieket alatt bekövetkező események időrendi sorrendben következik be.
* Különböző színek ütemtervek megfelelő toodifferent gyakrabban használ.
* Használt tooindicate hello időtartam, mely hello az aktoronkénti zárolásra nevében metódust vagy visszahívási tárolt kiemelés.

Néhány fontos tényezőt tooconsider:

* Amíg *Method1* metódus végrehajtása a következő nevében: *ActorId2* válasz tooclient kérelem *xyz789*, egy másik ügyfélkérés (*abc123*) érkezik, amely megköveteli azt is, *Method1* hajtja végre a toobe *ActorId2*. Azonban a második végrehajtásának hello *Method1* nem kezdődik, amíg hello előzetes végrehajtása nem fejeződött be. Ehhez hasonlóan emlékeztető által regisztrált *ActorId2* következik be, amikor *Method1* válasz tooclient kérelem végrehajtott *xyz789*. hello emlékeztető visszahívás végrehajtása mindkét végrehajtások, miután *Method1* befejeződött. Mindezt a kényszerítést tooturn alapú párhuzamosság miatt van *ActorId2*.
* Hasonlóképpen, kapcsolja-alapú feldolgozási is az érvényes *ActorId1*, amint azt a hello végrehajtásának *Method1*, *Method2*, és hello időzítő visszahívási nevében *ActorId1* soros módon történik.
* Végrehajtásának *Method1* nevében *ActorId1* átfedésben van a végrehajtása a következő nevében: *ActorId2*. Ennek az az oka kapcsolja-alapú feldolgozási csak egy szereplő belül és szereplője között nem kényszeríti ki.
* Egyes hello metódus/visszahívási végrehajtások hello `Task`(C#) / `CompletableFuture`(Java) hello metódus/visszahívási befejeződik által visszaadott, miután hello metódus ad vissza. Más, a hello aszinkron művelet már befejeződött hello időpontjára hello metódus/visszahívási adja vissza. Mindkét esetben a hello aktoronkénti zárolásra csak mindkét hello metódus/visszahívási adja vissza, és hello aszinkron művelet befejeződése után felszabadul.

#### <a name="reentrancy"></a>Rögzítve
hello szereplője futásidejű rögzítve alapértelmezés szerint lehetővé teszi. Ez azt jelenti, hogy ha egy aktormetódus a *Aktor A* metódus meghívja *szereplő B*, amely meghívja a másik módszer a *Aktor A*, hogy metódus toorun engedélyezett. Ennek az az oka hello része azonos logikai hívás-lánc környezetben. Az összes időzítő és felszólítás hívás hello új logikai hívás környezetben kezdődik. Lásd: hello [Reliable Actors rögzítve](service-fabric-reliable-actors-reentrancy.md) további részleteket.

#### <a name="scope-of-concurrency-guarantees"></a>Párhuzamossági garanciák hatókör
hello szereplője futásidejű ezek párhuzamossági garanciákat olyan esetekben, ahol az általa szabályozott hello meghívása az alábbi módszerek nyújt. Például azzal a garanciákat nyújt, amely választ tooa ügyfélkérelemben végzett hello metódus meghívásához, valamint időzítő és felszólítás visszahívások. Azonban hello szereplő kód közvetlenül ezen módszerek kívül hello mechanizmusok hello szereplője futtatókörnyezet által biztosított hív, ha majd hello futásidejű nem adhatók meg bármely párhuzamossági garanciát. Például ha hello metódus hivatkoznak, néhány feladatot, amely nincs társítva a hello tevékenység aktor módszerek hello által visszaadott hello környezetében, majd hello futásidejű nem tud párhuzamossági garanciát. Ha hello metódust egy olyan szálból hívják adott hello szereplő hoz létre a saját, majd hello futásidejű is nem adhatók meg a feldolgozási garanciát. Ezért tooperform háttérbeli műveletek végrehajtását, szereplője használandó [szereplő időzítők és szereplő emlékeztetők](service-fabric-reliable-actors-timers-reminders.md) , figyelembe vegyék kapcsolja-alapú feldolgozási.

## <a name="next-steps"></a>Következő lépések
* Ismerkedés az első Reliable Actors szolgáltatás épület:
   * [Bevezetés a Reliable Actors a .NET használatába](service-fabric-reliable-actors-get-started.md)
   * [Ismerkedés a Java a Reliable Actors](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
