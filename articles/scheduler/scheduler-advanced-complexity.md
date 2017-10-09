---
title: "aaaHow tooBuild ütemezése összetett és speciális ismétlődési és Azure-ütemező"
description: "Hogyan tooBuild komplex ütemezi és speciális ismétlődési és Azure-ütemező"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 02172791978b12be0ccb3078125d057b2efe8523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobuild-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Hogyan tooBuild komplex ütemezi és speciális ismétlődési és Azure-ütemező
## <a name="overview"></a>Áttekintés
Egy Azure Scheduler hello lelke a folyamat hello *ütemezés*. hello ütemezése határozza meg, mikor és hogyan hello Feladatütemező végrehajtja a hello feladat.

Azure Schedulerrel toospecify különböző egyszeri ütemezések és az ismétlődő feladat lehetővé teszi. *Egyszeri* egyszer, egy megadott időpontban – hatékonyan érvényesítést ütemezéseket, olyan *ismétlődő* ütemezéseket csak egyszer hajtható végre. Ismétlődő ütemezés tűz az előre meghatározott gyakoriságát.

Ez rugalmasságot Azure Scheduler lehetővé teszi a üzleti forgatókönyvek széles választékát támogatja:

* Rendszeres adat-karbantartási – például a minden nap 3 hónapnál régebbi összes Twitter-üzeneteket törlése
* Archiválás – például minden hónap, leküldéses számla előzmények toobackup szolgáltatás
* A kéréseket a külső adatokat – például 15 percenként, lekérés új ski időjárás-jelentés a NOAA
* Kép feldolgozás – pl. minden hétköznap, amikor kevesen, használja a felhő számítási toocompress képek feltöltése nap

Ez a cikk azt bízná példa feladatokat hozhat létre Azure Scheduler is. Azt adja meg, amely leírja azokat hello JSON-adatokat. Ha hello [Feladatütemező REST API](https://msdn.microsoft.com/library/mt629143.aspx), használhat ez az azonos JSON [egy Azure Scheduler-feladat létrehozása](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Támogatott helyzetek
Ebben a témakörben számos példák mutatják be, amely támogatja az Azure Scheduler forgatókönyvek hello mélysége hello. Széles körű ezek a példák azt mutatják be, hogyan toocreate ütemezése a viselkedés sok kombinációját, beleértve azokat, az alábbi hello:

* Egy adott dátummal és időponttal egyszeri futtatás
* Futtassa, és explicit többször ismétlődik.
* Azonnal futtatni, és ismétlődéshez
* Futtassa és megismétlődik minden  *n*  perc, óra, nap, hét és hónap, egy adott időpontban indítása
* Futtatás és ismétlődéshez heti vagy havi gyakorisággal, hanem csak az adott napokra, a hét meghatározott napjain vagy a hónap adott napjaira
* Futtassa, és a – például utolsó péntek és minden hónapban, vagy 5 15 perckor és délután 5:15 óra naponta hétfő időn belül többször ismétlődik

## <a name="dates-and-datetimes"></a>A dátumok és időpontok
Azure ütemezőjének dátumok kövesse hello [ISO-8601 specification](http://en.wikipedia.org/wiki/ISO_8601) és csak hello dátumát.

Dátum-idő hivatkozások Azure ütemezőjének kövesse hello [ISO-8601 specification](http://en.wikipedia.org/wiki/ISO_8601) , és tartalmazzák a dátum és idő is részei. Egy dátum-idő nem adja meg a UTC eltolás toobe UTC feltételezi.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Útmutató: JSON és a REST API használata az ütemezés létrehozása
egy egyszerű ütemezés használatával toocreate hello [Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143), első [az előfizetés regisztrálása egy erőforrás-szolgáltató](https://msdn.microsoft.com/library/azure/dn790548.aspx) (hello szolgáltató neve az ütemező  *Microsoft.Scheduler*), majd [hozzon létre egy feladatgyűjtemény](https://msdn.microsoft.com/library/mt629159.aspx), és végül [hozzon létre egy feladatot](https://msdn.microsoft.com/library/mt629145.aspx). Amikor feladatot hoz létre, megadhatja a ütemezését és az ismétlődési JSON, például egy alább látható hello használata:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often toofire (default too1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default toorecur infinitely)
            "endTime": "2012-11-04",      // optional (default toorecur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Áttekintés: Feladat séma alapjai
a következő táblázat hello magas szintű áttekintést nyújt az hello fő elemet kapcsolódó toorecurrence és a feladatok ütemezése:

| **JSON-név** | **Leírás** |
|:--- |:--- |
| ***Kezdő időpont*** |*startTime* egy dátum-idő. Az egyszerű ütemezés *startTime* hello első előfordulási, és az összetett ütemezések hello kezdjen feladat legkorábban *startTime*. |
| ***Ismétlődés*** |Hello *ismétlődési* hello és hello ismétlődési hello feladat hajtja végre a megadja ismétlődési szabályok objektum. hello ütemezésismétlődési objektum támogatja hello elemek *gyakorisága, időköz, endTime, count,* és *ütemezés*. Ha *ismétlődési* van definiálva, *gyakoriság* szükség; egyéb elemeinek hello *ismétlődési* megadása nem kötelező. |
| ***gyakoriság*** |Hello *gyakoriság* karakterlánc, amely hello gyakoriság egység, amely hello feladat ismétlési idejét. Támogatott értékek a következők *"perc", "hour", "day", "hetente"* vagy *"honap."* |
| ***időköz*** |Hello *időköz* pozitív egész szám, és azt jelzi, hello hello intervallumát *gyakoriság* , amely meghatározza, hogy milyen gyakran hello feladat fog futni. Például ha *időköz* 3 és *gyakoriság* "hét" hello feladat ismétlési idejét minden három hetet. Azure Schedulerrel legfeljebb *időköz* 18 hónapig havi gyakoriság 78 hét heti gyakoriság, vagy a napi gyakoriságú 548 nap. Az órában és percben gyakoriság hello támogatott értéktartomány: 1 < = *időköz* < = 1000. |
| ***Befejezés időpontja*** |Hello *endTime* karakterlánc meg hello dátuma és időpontja múltbeli mely hello feladatot kell nem hajtható végre. Nincs érvényes toohave egy *endTime* az elmúlt hello. Ha nincs *endTime* vagy szám van megadva, a hello feladat futtatása végtelenül. Mindkét *endTime* és *száma* nem lehet része a hello ugyanazt a feladatot. |
| ***száma*** |<p>Hello *száma* pozitív egész szám (nullánál nagyobb), amely meghatározza a hello szám, ahányszor a feladat végrehajtása előtt fusson.</p><p>Hello *száma* jelöli hello hányszor hello feladat futtatása előtt befejeződött határozzák meg. Például egy feladatot, amely a naponta végrehajtása *száma* 5 és kezdő dátuma hétfő, hello feladat befejeződik, péntek végrehajtása után. Ha hello start dátuma múltbeli hello, hello első végrehajtási alapján van kiszámítva hello létrehozásának ideje.</p><p>Ha nincs *endTime* vagy *száma* van megadva, hello feladat futtatása végtelenül. Mindkét *endTime* és *száma* nem lehet része a hello ugyanazt a feladatot.</p> |
| ***ütemezés*** |Egy feladat a megadott gyakorisággal megváltoztatja az ismétlődési a ismétlődési ütemezés szerint. A *ütemezés* perc, a üzemideje (óra), a hét nap, a hónap és a hetek száma alapján módosításokat tartalmazza. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Áttekintés: A feladat alapértelmezett séma, korlátok és példák
Ez az áttekintés után most tárgyalják részletesen ezeket az elemeket.

| **JSON-név** | **Érték típusa** | **Kötelező megadni?** | **Alapértelmezett érték** | **Érvényes értékek** | **Példa** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***Kezdő időpont*** |Karakterlánc |Nem |None |ISO-8601 dátum-idők |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***Ismétlődés*** |Objektum |Nem |None |Recurrence objektum |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***gyakoriság*** |Karakterlánc |Igen |None |"perc", "hour", "day", "hetente", "honap" |<code>"frequency" : "hour"</code> |
| ***időköz*** |Szám |Nem |1 |1 too1000. |<code>"interval":10</code> |
| ***Befejezés időpontja*** |Karakterlánc |Nem |None |Hello jövőbeli időpontra jelölő dátum-idő érték |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***száma*** |Szám |Nem |None |>= 1 |<code>"count": 5</code> |
| ***ütemezés*** |Objektum |Nem |None |Schedule objektum |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Részletes bemutatója: *kezdő időpont*
hello alább táblázat rögzítésekre hogyan *startTime* egy feladat futtatásának módját szabályozza.

| **startTime érték** | **Nincs ismétlődés.** | **Ismétlődés. Ütemezés nélkül** | **Ismétlődés ütemezéssel** |
|:--- |:--- |:--- |:--- |
| **A Kezdés időpontja** |Egyszer, azonnal futtatni |Egyszer, azonnal futtatni. Futtassa a későbbi végrehajtások során a legutóbbi végrehajtásának időpontja kiszámítása alapján |<p>Egyszer, azonnal futtatni</p><p>Az azt követő végrehajtások az ismétlődési ütemezés alapján futnak</p> |
| **Kezdési időpontja múltbeli** |Egyszer, azonnal futtatni |<p>Első jövőbeli végrehajtási idő kiszámítása a kezdési idő utánra, és akkor futnak</p><p>Futtassa a későbbi végrehajtások során alapú oncalculating legutóbbi végrehajtásának időpontja</p><p>Lásd a példát kapcsolódó további tájékoztatást a táblázat után</p> |<p>Sikertelen feladat indítása *nem sooner mint* hello megadott kezdési ideje. hello első előfordulási hello kezdési időponttól számított hello ütemezés alapján</p><p>Az azt követő végrehajtások az ismétlődési ütemezés alapján futnak</p> |
| **Kezdési idő későbbi vagy a jelenlegi** |Futtassa egyszer a megadott kezdési időpontban |<p>Futtassa egyszer a megadott kezdési időpontban</p><p>Futtassa a későbbi végrehajtások során a legutóbbi végrehajtásának időpontja kiszámítása alapján</p> |<p>Sikertelen feladat indítása *nem sooner mint* hello megadott kezdési ideje. hello első előfordulási hello kezdési időponttól számított hello ütemezés alapján</p><p>Az azt követő végrehajtások az ismétlődési ütemezés alapján futnak</p> |

Nézzük meg, mi történik, például ha *startTime* hello korábbi, a rendszer a *ismétlődési* , de nincs *ütemezés*.  Feltételezik, hogy hello aktuális idő 2015-04-08 13:00, *startTime* 2015-04-07 14:00 és *ismétlődési* minden 2 nap (definiálva *gyakoriság*: nap és *időköz*: 2.) Vegye figyelembe, hogy hello *startTime* múltbeli hello van, és hello aktuális ideje előtt

Ilyen körülmények hello *első végrehajtási* 2015-04-09 lesz 14:00\. hello Feladatütemező motor hello kezdési ideje a végrehajtási előfordulások számítja ki.  Az elmúlt hello példányok a rendszer elveti. hello motor, amely a jövőbeni hello hello következő példány használja.  Így ebben az esetben *startTime* 2015-04-07 jelenleg 2:00 pm, így hello következő példány kezdve, amely 2015-04-09, 2:00 pm 2 nap.

Vegye figyelembe, hogy hello első végrehajtási lenne hello azonos még akkor is, ha a startTime 2015-04-05 hello 14:00 vagy 2015-04-01 14:00\. Után hello első végrehajtási, a későbbi végrehajtások során kiszámítása hello segítségével ütemezett – így azok lenne 2015-04-11, 2:00 pm, majd 2015-04-13 2:00 pm, majd 2015-04-15 2:00 pm, stb.

Végül, ha a feladat rendelkezik ütemterv óra és/vagy perc nincsenek beállítva a hello ütemezést, akkor alapértelmezett toohello óra és/vagy percek száma, az első végrehajtása hello, illetve ha.

## <a name="deep-dive-schedule"></a>Részletes bemutatója: *ütemezése*
Egyrészről a egy *ütemezés* is *korlát* hello feladat végrehajtások száma.  Például ha egy feladatot az "honap" gyakoriságát rendelkezik egy *ütemezés* , amelyen fut a csak a 31 napra esnek, hello feladat fut, csak a 31 rendelkezik hónapok<sup>st</sup> nap.

A hello, ugyanakkor egy *ütemezés* is *bontsa ki a* hello feladat végrehajtások száma. Például ha egy feladatot az "honap" gyakoriságát rendelkezik egy *ütemezés* , hogy fut a hónap napjain 1 és 2, hello feladat fut hello 1<sup>st</sup> és 2<sup>nd</sup> helyett csak egyszer hello hónap napjai egy a hónap.

Ha több ütemezést elem meg van adva, a kiértékelés sorrendjét hello származik hello legnagyobb toosmallest – a hét számát, hónap és nap, hét nap, óra és perc.

hello következő táblázatban található *ütemezés* elemek részletei.

| **JSON-név** | **Leírás** | **Érvényes értékek** |
|:--- |:--- |:--- |
| **perc** |Mely hello feladat fog futni hello óra perc |<ul><li>Egész szám, vagy</li><li>Egész számok tömbje</li></ul> |
| **üzemideje (óra)** |Hello nap melyik hello feladat fog futni üzemideje (óra) |<ul><li>Egész szám, vagy</li><li>Egész számok tömbje</li></ul> |
| **hétköznapokon** |Hello hét hello feladat napjain fog futni. Csak heti gyakoriság mellett adható meg. |<ul><li>"Hétfő", "Kedd", "Szerda", "Csütörtök", "Péntek", "Szombat" vagy "Vasárnap"</li><li>A tömb bármely hello meghaladja értékeket (maximális tömbméret 7)</li></ul>*Nem* kis-és nagybetűket |
| **monthlyOccurrences** |Meghatározza, hogy melyik napon hello hónap hello feladat fog futni. Csak havi gyakoriság mellett adható meg. |<ul><li>MonthlyOccurrence objektumokból álló tömb:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *nap* hello hét hello feladat hello napján fog futni, pl. {vasárnap} minden vasárnap hello hónapon belül. Kötelező.</p><p>Az előfordulási érték *előfordulási* hello nap hello hónapban, pl. {vasárnap, -1} van hello hello hónap utolsó vasárnap. Választható.</p> |
| **monthDays** |Hello hónap hello feladat napján fog futni. Csak havi gyakoriság mellett adható meg. |<ul><li>Bármely értéke < = -1 és > =-31.</li><li>Bármely érték > = 1 és < = 31.</li><li>A fenti értékek tömbje</li></ul> |

## <a name="examples-recurrence-schedules"></a>Példák: Ismétlődési ütemezés
hello következő példákban különböző ismétlődés ütemezésének – hello ütemterv objektum és az alárendelt elemei.

hello alatti összes ütemezések azt feltételezik, hogy hello *időköz* too1 van beállítva.\. Is, egy részlegnek feltételeznie kell hello jobb gyakoriságának megfelelően toowhat van hello *ütemezés* – például egy nem használja a "day" gyakoriságát, és "monthDays" módosítása a hello ütemezés. Az ilyen korlátozások fent ismerteti.

| **Példa** | **Leírás** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |5 órakor Futtatás minden nap. Azure Schedulerrel másolatot minden egyes értéknek felel meg a "minutes", egyenként, minden hello alkalommal mely hello feladat toobe listája futtatása toocreate mindegyik érték "hours". |
| <code>{"minutes":[15], "hours":[5]}</code> |5:15 órakor Futtatás minden nap |
| <code>{"minutes":[15], "hours":[5,17]}</code> |5:15 Reggel 5:15 előtti futtatását minden nap |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |Futtatás éjfélre (5:15), 5 óra 45 Perckor, 5:15 előtti, és 5:45 PM minden nap |
| <code>{"minutes":[0,15,30,45]}</code> |15 percenként futtatása |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Minden órában futtatva. Ez a feladat óránként fut. hello perc hello vezérli *startTime*, ha egy meg van adva, vagy ha nincs megadva, hello létrehozásának ideje szerint. Például ha hello kezdési ideje vagy létrehozásának idejét (amelyik vonatkozik) 12:25 PM, hello feladat futni fog 00:25, 01:25, 02:25,..., 23:25. hello ütemezésének megadása egyenértékű toohaving egy feladat *gyakoriság* "hour", az egy *időköz* 1, és nem *ütemezés*. hello különbség az, hogy az ütemezés használható másik *gyakoriság* és *időköz* toocreate más feladatok túl. Például, ha hello *gyakoriság* "honap" volt, hello ütemezés futna helyett minden nap havonta egyszer csak ha *gyakoriság* volt "day" |
| <code>{minutes:[0]}</code> |Minden órában hello óra fut. Ez a feladat minden órában fut, de a hello óra (pl. 12 AM, 13: 00, hajnali 2 Órakor, stb.) Ez a feladat egyenértékű tooa gyakoriság "hour", nulla perc, és nincs ütemezés a startTime hello gyakoriság "day", de ha hello gyakoriság "hét" vagy "honap", hello ütemezés volna rendre végrehajtani egy hét vagy havi, egy nap csak egy nap. |
| <code>{"minutes":[15]}</code> |15 percenként futtatásra óránként túlra óránként. Óránként fut le, 0:15-kor, 01:15-kor. 02:15-kor stb., egészen 22:15-ig és 23:15-ig. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |Szombaton délután 5 óra futtatni minden héten |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |Futtassa a következő hétfőn, szerdán és pénteken délután 5 óra minden héten |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |5:15 előtti 5:45 PM hétfőn, szerdán és pénteken futtatását minden héten |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |5 óra és 5 hétfőn, szerdán és pénteken futtatni minden héten |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |Futtatás éjfélre (5:15), 5 óra 45 Perckor, 5:15 előtti, és délután 5:45 óra hétfőn, szerdán és pénteken minden héten |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Hétköznapokon 15 percenként fut le |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |15 percenként futtatnak hétköznapokon Reggel 9 és du. 4:45 között |
| <code>{"weekDays":["sunday"]}</code> |Futtassa a kezdési időpontban vasárnap |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |Futtassa a keddi és csütörtöki napokon kezdési időpontban |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |Futtassa a 6 órakor hello 28 nap az minden hónap (feltéve, hogy a gyakoriság hónap) |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Hello hello hónap utolsó napján 6 órakor futtassa. Ha szeretné toorun hello egy feladat a hónap utolsó napján, használja a -1 nap 28, 29, 30 és 31 helyett. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |Futó 6 órakor hello első, valamint minden hónap utolsó napja |
| <code>{monthDays":[1,-1]}</code> |Futtassa a hello első, valamint minden hónap utolsó napja kezdete |
| <code>{monthDays":[1,14]}</code> |Futtassa a hello első, valamint minden hónap napja tizennegyedik kezdete |
| <code>{monthDays":[2]}</code> |Futtassa a hello hello Start időpontban hónap második napja |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |5 órakor minden hónap első péntekén futtatása |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: Futtassa a kezdési időpontban minden hónap első péntek |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Futtassa a hónap végét harmadik péntek minden hónapban, kezdési időpontban |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Futtassa az első és utolsó péntekén legyen havonta 5:15 órakor |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |Első és utolsó péntekén legyen havonta Futtatás kezdési időpontban |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Minden hónap kezdési időpontban ötödik péntek futtatható. Ha nincs ötödik péntek hónap, ez nem fut, mert nincs ütemezett toorun csak ötödik péntekenként. Érdemes helyett-1 érték 5 hello előfordulásakor Ha azt szeretné, hogy toorun hello feladat hello utolsó bekövetkező hello hónap péntekig. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Hello hónap utolsó péntekén át 15 percenként futtatása |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |Futtatás éjfélre (5:15), 5 óra 45 Perckor, 5:15 előtti, és délután 5:45 óra a hello minden hónap 3. szerda |

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Az Azure Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

