---
title: "a figyelő metrika - erőforrás típusonkénti támogatott metrikák aaaAzure |} Microsoft Docs"
description: "Minden erőforrás típusból Azure megfigyelővel metrikák listája."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/05/2017
ms.author: johnkem
ms.openlocfilehash: 66834238a1a4fcd7db1464cc023c18ee2563517a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Azure-figyelő támogatott metrikák
Az Azure biztosít több módon toointeract metrikák diagramkészítési őket hello portálon, ezek eléréséhez szükséges hello REST API-n keresztül vagy kérdez le azokat a PowerShell vagy a parancssori felület használatával. Alatt érhető el teljes listáját és az összes metrikák jelenleg Azure figyelő metrika folyamat.

> [!NOTE]
> Más metrikákkal hello portál vagy az örökölt API-k használatával érhetők el. Ez a lista csak nyilvános előzetes metrikák érhető el a nyilvános előzetes hello konszolidált hello Azure figyelő metrika feldolgozási folyamat használatával tartalmazza.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|qpu_metric|QPU|Darabszám|Átlagos|QPU. Tartomány 0 – 100 s1, 0-200 S2 és 0 – 400 S4|
|memory_metric|Memory (Memória)|Bájtok|Átlagos|A memória. S1 0-25 GB-ot, S2 0-50 GB-ot és 0 – 100 GB-ot S4|
|TotalConnectionRequests|Teljes csatlakozási kérések|Darabszám|Átlagos|Teljes csatlakozási kérések. Ezek a érkezők.|
|SuccessfullConnectionsPerSec|Sikeres kapcsolatok másodpercenkénti száma|CountPerSecond|Átlagos|Sikeres csatlakozás befejezésekhez aránya.|
|TotalConnectionFailures|Teljes kapcsolódási hibák|Darabszám|Átlagos|Összes sikertelen csatlakozási kísérletek.|
|CurrentUserSessions|Aktuális felhasználói munkamenetek|Darabszám|Átlagos|Létrehozott felhasználói munkamenetek jelenlegi száma.|
|QueryPoolBusyThreads|Lekérdezés készlet foglalt szálak|Darabszám|Átlagos|Hello lekérdezés szálkészlet foglalt szálak számát.|
|CommandPoolJobQueueLength|A parancs készlet feladat várólistájának hossza|Darabszám|Átlagos|Hello parancs szálkészlet hello sorban lévő feladatok száma.|
|ProcessingPoolJobQueueLength|Feldolgozási készlet feladat várólistájának hossza|Darabszám|Átlagos|Hello várólistában lévő hello szálkészlet feldolgozása nem-I/O-feladatok száma.|
|CurrentConnections|Kapcsolat: A jelenlegi kapcsolatok száma|Darabszám|Átlagos|Ügyfél fennálló kapcsolatok aktuális száma.|
|CleanerCurrentPrice|Memória: Tisztító aktuális árlista|Darabszám|Átlagos|Memória, a $/ bájt/idő, a normalizált too1000 aktuális ára.|
|CleanerMemoryShrinkable|Memória: Zsugoríthatósági tisztító memória|Bájtok|Átlagos|Memória mérete, tulajdonos toopurging által hello háttér tisztító mennyisége.|
|CleanerMemoryNonshrinkable|Memória: Tisztító memória nonshrinkable|Bájtok|Átlagos|Memória mérete, nem tulajdonos toopurging által hello háttér tisztító mennyisége.|
|MemoryUsage|Memória: Memória használata|Bájtok|Átlagos|Memóriahasználat hello server folyamat, tisztító memória ár kiszámításakor. Egyenlő toocounter Process\PrivateBytes plus hello memórialeképezési adatok méretétől, figyelmen kívül bármely memória leképezve, vagy le van foglalva hello xVelocity memórián belüli analytics motor (VertiPaq) meghaladja a hello xVelocity motor memóriára vonatkozó korlátozást.|
|MemoryLimitHard|Memória: Memóriakorlátot rögzített|Bájtok|Átlagos|Rögzített memóriakorlátot, konfigurációs fájlból.|
|MemoryLimitHigh|Memória: Memória korlátozása magas|Bájtok|Átlagos|Magas memóriakorlátot, konfigurációs fájlból.|
|MemoryLimitLow|Memória: Memória korlátja alacsony|Bájtok|Átlagos|Kevés a memória a határt, a konfigurációs fájlból.|
|MemoryLimitVertiPaq|Memória: Memória korlátja VertiPaq|Bájtok|Átlagos|A memória maximális mennyiségét, a konfigurációs fájlból.|
|Kvóta|Memória: kvóta|Bájtok|Átlagos|Aktuális memóriakvótáját, bájtban. Memóriakvótáját memória grant vagy memória foglalás is nevezik.|
|QuotaBlocked|Memória: Kvóta letiltva|Darabszám|Átlagos|Le vannak tiltva, amíg más memória kvóták felszabadítását kvóta kérelmek jelenlegi száma.|
|VertiPaqNonpaged|Memória: a nem lapozható VertiPaq|Bájtok|Átlagos|Bájt memóriát hello memóriabeli motorjához zárolt hello munkakészlet való használatra.|
|VertiPaqPaged|Memória: A VertiPaq lapozható|Bájtok|Átlagos|A memóriában levő használt lapozható memória bájt.|
|RowsReadPerSec|Feldolgozási: Sor olvasása másodpercenkénti száma|CountPerSecond|Átlagos|Sorok aránya az összes relációs adatbázisok olvasni.|
|RowsConvertedPerSec|Feldolgozási: Sorok konvertálni másodpercenkénti száma|CountPerSecond|Átlagos|Sorok aránya alakítja a feldolgozás során.|
|RowsWrittenPerSec|Feldolgozási: Másodpercenként írt sorok|CountPerSecond|Átlagos|Feldolgozás során írt sorok száma.|
|CommandPoolBusyThreads|Szálak: Parancs készlet foglalt szálak|Darabszám|Átlagos|Hello parancs szálkészlet foglalt szálak számát.|
|CommandPoolIdleThreads|Szálak: Parancs készlet üresjárati szálak|Darabszám|Átlagos|Hello parancs szálkészlet üresjárati szálak számát.|
|LongParsingBusyThreads|Szálak: Hosszú foglalt szálak elemzése|Darabszám|Átlagos|Hosszú elemzése a szálkészlet hello foglalt szálak számát.|
|LongParsingIdleThreads|Szálak: Hosszú üresjárati szálak elemzése|Darabszám|Átlagos|Hosszú elemzése a szálkészlet hello üresjárati szálak számát.|
|LongParsingJobQueueLength|Szálak: Hosszú elemzése feladat várólistájának hossza|Darabszám|Átlagos|A hosszú elemzése a szálkészlet hello hello sorban lévő feladatok száma.|
|ProcessingPoolBusyIOJobThreads|Szálak: Készlet foglalt i/o-feladat szálak feldolgozása|Darabszám|Átlagos|I/o-feladatok feldolgozási szálkészletben hello futó szálak száma.|
|ProcessingPoolBusyNonIOThreads|Szálak: Készlet foglalt nem-I/O-szálak feldolgozása|Darabszám|Átlagos|Nem-I/O-feladatok feldolgozási szálkészletben hello futó szálak száma.|
|ProcessingPoolIOJobQueueLength|Szálak: Feldolgozása készlet i/o-feldolgozás várólistájának hossza|Darabszám|Átlagos|Hello várólistában hello feldolgozási szálkészletben lévő i/o-feladatok száma.|
|ProcessingPoolIdleIOJobThreads|Szálak: Készlet üresjárati i/o-feladat szálak feldolgozása|Darabszám|Átlagos|A feldolgozási szálkészletben hello i/o-feladatok üresjárati szálak száma.|
|ProcessingPoolIdleNonIOThreads|Szálak: Készlet üresjárati nem-I/O-szálak feldolgozása|Darabszám|Átlagos|A feldolgozási szálkészletben hello üresjárati szálak száma dedikált toonon-I/O-feladatokat.|
|QueryPoolIdleThreads|Szálak: Lekérdezés készlet üresjárati szálak|Darabszám|Átlagos|A feldolgozási szálkészletben hello i/o-feladatok üresjárati szálak száma.|
|QueryPoolJobQueueLength|Szálak: Lekérdezés készlet feladat várólista lengt|Darabszám|Átlagos|Hello lekérdezés szálkészlet hello sorban lévő feladatok száma.|
|ShortParsingBusyThreads|Szálak: Rövid foglalt szálak elemzése|Darabszám|Átlagos|Rövid szálkészlet elemzése hello foglalt szálak számát.|
|ShortParsingIdleThreads|Szálak: Rövid üresjárati szálak elemzése|Darabszám|Átlagos|Rövid szálkészlet elemzése hello üresjárati szálak számát.|
|ShortParsingJobQueueLength|Szálak: Rövid elemzése feladat várólistájának hossza|Darabszám|Átlagos|A rövid szálkészlet elemzése hello hello sorban lévő feladatok száma.|
|memory_thrashing_metric|Memória Akadozások|Százalék|Átlagos|Átlagos memória thrashing.|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|TotalRequests|Teljes átjáró kérelmek|Darabszám|Összes|Átjáró kérelmek száma|
|SuccessfulRequests|Átjáró sikeres kérelmek|Darabszám|Összes|Átjáró sikeres kérelmek száma|
|UnauthorizedRequests|Jogosulatlan átjáró kérelmek|Darabszám|Összes|Jogosulatlan átjáró kérelmek száma|
|FailedRequests|Hibás átjáró kérelmek|Darabszám|Összes|Az átjáró kérelmekben hibák száma|
|OtherRequests|Más átjáró kérelmek|Darabszám|Összes|Más átjáró kérelmek száma|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|CoreCount|Dedikált magok száma|Darabszám|Összes|A batch-fiók hello dedikált magok száma összesen|
|TotalNodeCount|Dedikált csomópontok száma|Darabszám|Összes|Dedikált csomópontja hello batch-fiók teljes száma|
|LowPriorityCoreCount|LowPriority magok száma|Darabszám|Összes|A batch-fiók hello alacsony prioritású magok száma összesen|
|TotalLowPriorityNodeCount|Alacsony prioritású csomópontok száma|Darabszám|Összes|Alacsony prioritású csomópontok hello batch-fiók teljes száma|
|CreatingNodeCount|Csomópontok száma létrehozása|Darabszám|Összes|A létrehozandó csomópontok száma|
|StartingNodeCount|Kezdő csomópontok száma|Darabszám|Összes|Kezdési csomópontok száma|
|WaitingForStartTaskNodeCount|Várakozás a kezdő tevékenység csomópontok száma|Darabszám|Összes|Várakozás a feladat indítása toocomplete hello csomópontok száma|
|StartTaskFailedNodeCount|Kezdő tevékenység nem sikerült a csomópontok száma|Darabszám|Összes|Ha hello feladat indítása nem csomópontok száma|
|IdleNodeCount|Üresjárati csomópontok száma|Darabszám|Összes|Üresjárati csomópontok száma|
|OfflineNodeCount|Kapcsolat nélküli csomópontok száma|Darabszám|Összes|Kapcsolat nélküli csomópontok száma|
|RebootingNodeCount|Rendszer újraindítása a csomópontok száma|Darabszám|Összes|Újraindítás a csomópontok száma|
|ReimagingNodeCount|Különösen csomópontok száma|Darabszám|Összes|Különösen csomópontok száma|
|RunningNodeCount|Futó csomópontok száma|Darabszám|Összes|Futó csomópontok száma|
|LeavingPoolNodeCount|Kilépés a készlet csomópontok száma|Darabszám|Összes|Kilépés a készlet hello csomópontok száma|
|UnusableNodeCount|Használhatatlanná csomópontok száma|Darabszám|Összes|Használhatatlanná csomópontok száma|
|PreemptedNodeCount|Csomópont Count részesítés|Darabszám|Összes|A leállított csomópontok száma|
|TaskStartEvent|A feladat kezdési események|Darabszám|Összes|Kezdték feladatok száma összesen|
|TaskCompleteEvent|A feladat befejezése események|Darabszám|Összes|A befejezett feladatok száma összesen|
|TaskFailEvent|A feladat sikertelen események|Darabszám|Összes|Hibás állapotban befejeződött feladatok száma összesen|
|PoolCreateEvent|Alkalmazáskészlet létrehozása események|Darabszám|Összes|Létrehozott gyűjtők száma összesen|
|PoolResizeStartEvent|Készlet átméretezési Start események|Darabszám|Összes|Készlet átméretezi, amely kezdték száma összesen|
|PoolResizeCompleteEvent|Készlet átméretezési teljes események|Darabszám|Összes|Készlet átméretezi elvégző száma összesen|
|PoolDeleteStartEvent|Kezdő események készlet törlése|Darabszám|Összes|Teljes száma, amelyek kezdték készlet törlése|
|PoolDeleteCompleteEvent|Alkalmazáskészlet teljes események törlése|Darabszám|Összes|A befejezett készlet törlése száma összesen|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|connectedclients|Csatlakozott ügyfelek|Darabszám|Maximális||
|totalcommandsprocessed|Összes művelet|Darabszám|Összes||
|cachehits|Találatot eredményező gyorsítótárbeli kereséseinek|Darabszám|Összes||
|cachemisses|Gyorsítótár-tévesztései|Darabszám|Összes||
|getcommands|Beolvasása|Darabszám|Összes||
|setcommands|Beállítása|Darabszám|Összes||
|evictedkeys|Eltávolított kulcsok|Darabszám|Összes||
|totalkeys|Összes kulcs|Darabszám|Maximális||
|expiredkeys|Lejárt kulcsok|Darabszám|Összes||
|usedmemory|Használt memória|Bájtok|Maximális||
|usedmemoryRss|Használt memória RSS|Bájtok|Maximális||
|serverLoad|A kiszolgálóterhelés|Százalék|Maximális||
|cacheWrite|Gyorsítótár-írási|BytesPerSecond|Maximális||
|cacheRead|Gyorsítótár olvasása|BytesPerSecond|Maximális||
|PercentProcessorTime|CPU|Százalék|Maximális||
|connectedclients0|Csatlakozott ügyfelek (szilánkok 0)|Darabszám|Maximális||
|totalcommandsprocessed0|Összes művelet (szilánkok 0)|Darabszám|Összes||
|cachehits0|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 0)|Darabszám|Összes||
|cachemisses0|A gyorsítótárbeli (szilánkok 0)|Darabszám|Összes||
|getcommands0|Lekérdezi a (szilánkok 0)|Darabszám|Összes||
|setcommands0|Beállítása (szilánkok 0)|Darabszám|Összes||
|evictedkeys0|Eltávolított kulcsok (szilánkok 0)|Darabszám|Összes||
|totalkeys0|Összes kulcs (szilánkok 0)|Darabszám|Maximális||
|expiredkeys0|Lejárt kulcsokkal (szilánkok 0)|Darabszám|Összes||
|usedmemory0|Használt memória (szilánkok 0)|Bájtok|Maximális||
|usedmemoryRss0|Használt memória RSS (szilánkok 0)|Bájtok|Maximális||
|serverLoad0|Kiszolgáló terhelését (szilánkok 0)|Százalék|Maximális||
|cacheWrite0|Gyorsítótár-írási (szilánkok 0)|BytesPerSecond|Maximális||
|cacheRead0|Gyorsítótár olvasható (szilánkok 0)|BytesPerSecond|Maximális||
|percentProcessorTime0|Processzor (szilánkok 0)|Százalék|Maximális||
|connectedclients1|Csatlakozott ügyfelek (szilánkok 1)|Darabszám|Maximális||
|totalcommandsprocessed1|Összes művelet (szilánkok 1)|Darabszám|Összes||
|cachehits1|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 1)|Darabszám|Összes||
|cachemisses1|A gyorsítótárbeli (szilánkok 1)|Darabszám|Összes||
|getcommands1|(Szilánkok 1) beolvasása|Darabszám|Összes||
|setcommands1|(1 a Shard) beállítása|Darabszám|Összes||
|evictedkeys1|Eltávolított kulcsok (szilánkok 1)|Darabszám|Összes||
|totalkeys1|Összes kulcs (szilánkok 1)|Darabszám|Maximális||
|expiredkeys1|Lejárt kulcsokkal (szilánkok 1)|Darabszám|Összes||
|usedmemory1|Használt memória (szilánkok 1)|Bájtok|Maximális||
|usedmemoryRss1|Használt memória RSS (szilánkok 1)|Bájtok|Maximális||
|serverLoad1|Kiszolgáló terhelését (szilánkok 1)|Százalék|Maximális||
|cacheWrite1|Gyorsítótár-írási (szilánkok 1)|BytesPerSecond|Maximális||
|cacheRead1|Gyorsítótár olvasható (szilánkok 1)|BytesPerSecond|Maximális||
|percentProcessorTime1|Processzor (szilánkok 1)|Százalék|Maximális||
|connectedclients2|Csatlakozott ügyfelek (szilánkok 2)|Darabszám|Maximális||
|totalcommandsprocessed2|Összes művelet (szilánkok 2)|Darabszám|Összes||
|cachehits2|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 2)|Darabszám|Összes||
|cachemisses2|A gyorsítótárbeli (szilánkok 2)|Darabszám|Összes||
|getcommands2|Lekérdezi a (szilánkok 2)|Darabszám|Összes||
|setcommands2|Beállítása (szilánkok 2)|Darabszám|Összes||
|evictedkeys2|Eltávolított kulcsok (szilánkok 2)|Darabszám|Összes||
|totalkeys2|Összes kulcs (szilánkok 2)|Darabszám|Maximális||
|expiredkeys2|Lejárt kulcsokkal (szilánkok 2)|Darabszám|Összes||
|usedmemory2|Használt memória (szilánkok 2)|Bájtok|Maximális||
|usedmemoryRss2|Használt memória RSS (szilánkok 2)|Bájtok|Maximális||
|serverLoad2|Kiszolgáló terhelését (szilánkok 2)|Százalék|Maximális||
|cacheWrite2|Gyorsítótár-írási (szilánkok 2)|BytesPerSecond|Maximális||
|cacheRead2|Gyorsítótár olvasható (szilánkok 2)|BytesPerSecond|Maximális||
|percentProcessorTime2|Processzor (szilánkok 2)|Százalék|Maximális||
|connectedclients3|Csatlakozott ügyfelek (szilánkok 3)|Darabszám|Maximális||
|totalcommandsprocessed3|Összes művelet (szilánkok 3)|Darabszám|Összes||
|cachehits3|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 3)|Darabszám|Összes||
|cachemisses3|A gyorsítótárbeli (szilánkok 3)|Darabszám|Összes||
|getcommands3|Lekérdezi a (szilánkok 3)|Darabszám|Összes||
|setcommands3|(3 szilánkok) beállítása|Darabszám|Összes||
|evictedkeys3|Eltávolított kulcsok (szilánkok 3)|Darabszám|Összes||
|totalkeys3|Összes kulcs (szilánkok 3)|Darabszám|Maximális||
|expiredkeys3|Lejárt kulcsokkal (szilánkok 3)|Darabszám|Összes||
|usedmemory3|Használt memória (szilánkok 3)|Bájtok|Maximális||
|usedmemoryRss3|Használt memória RSS (szilánkok 3)|Bájtok|Maximális||
|serverLoad3|Kiszolgáló terhelését (szilánkok 3)|Százalék|Maximális||
|cacheWrite3|Gyorsítótár-írási (szilánkok 3)|BytesPerSecond|Maximális||
|cacheRead3|Gyorsítótár olvasható (szilánkok 3)|BytesPerSecond|Maximális||
|percentProcessorTime3|Processzor (szilánkok 3)|Százalék|Maximális||
|connectedclients4|Csatlakozott ügyfelek (szilánkok 4)|Darabszám|Maximális||
|totalcommandsprocessed4|Összes művelet (szilánkok 4)|Darabszám|Összes||
|cachehits4|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 4)|Darabszám|Összes||
|cachemisses4|A gyorsítótárbeli (szilánkok 4)|Darabszám|Összes||
|getcommands4|Lekérdezi a (szilánkok 4)|Darabszám|Összes||
|setcommands4|(4 szilánkok) beállítása|Darabszám|Összes||
|evictedkeys4|Eltávolított kulcsok (szilánkok 4)|Darabszám|Összes||
|totalkeys4|Összes kulcs (szilánkok 4)|Darabszám|Maximális||
|expiredkeys4|Lejárt kulcsokkal (szilánkok 4)|Darabszám|Összes||
|usedmemory4|Használt memória (szilánkok 4)|Bájtok|Maximális||
|usedmemoryRss4|Használt memória RSS (szilánkok 4)|Bájtok|Maximális||
|serverLoad4|Kiszolgáló terhelését (szilánkok 4)|Százalék|Maximális||
|cacheWrite4|Gyorsítótár-írási (szilánkok 4)|BytesPerSecond|Maximális||
|cacheRead4|Gyorsítótár olvasható (szilánkok 4)|BytesPerSecond|Maximális||
|percentProcessorTime4|Processzor (szilánkok 4)|Százalék|Maximális||
|connectedclients5|Csatlakozott ügyfelek (szilánkok 5)|Darabszám|Maximális||
|totalcommandsprocessed5|Összes művelet (szilánkok 5)|Darabszám|Összes||
|cachehits5|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 5)|Darabszám|Összes||
|cachemisses5|A gyorsítótárbeli (szilánkok 5)|Darabszám|Összes||
|getcommands5|Lekérdezi a (szilánkok 5)|Darabszám|Összes||
|setcommands5|Beállítása (szilánkok 5)|Darabszám|Összes||
|evictedkeys5|Eltávolított kulcsok (szilánkok 5)|Darabszám|Összes||
|totalkeys5|Összes kulcs (szilánkok 5)|Darabszám|Maximális||
|expiredkeys5|Lejárt kulcsokkal (szilánkok 5)|Darabszám|Összes||
|usedmemory5|Használt memória (szilánkok 5)|Bájtok|Maximális||
|usedmemoryRss5|Használt memória RSS (szilánkok 5)|Bájtok|Maximális||
|serverLoad5|Kiszolgáló terhelését (szilánkok 5)|Százalék|Maximális||
|cacheWrite5|Gyorsítótár-írási (szilánkok 5)|BytesPerSecond|Maximális||
|cacheRead5|Gyorsítótár olvasható (szilánkok 5)|BytesPerSecond|Maximális||
|percentProcessorTime5|Processzor (szilánkok 5)|Százalék|Maximális||
|connectedclients6|Csatlakozott ügyfelek (szilánkok 6)|Darabszám|Maximális||
|totalcommandsprocessed6|Összes művelet (szilánkok 6)|Darabszám|Összes||
|cachehits6|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 6)|Darabszám|Összes||
|cachemisses6|A gyorsítótárbeli (szilánkok 6)|Darabszám|Összes||
|getcommands6|Lekérdezi a (szilánkok 6)|Darabszám|Összes||
|setcommands6|Beállítása (szilánkok 6)|Darabszám|Összes||
|evictedkeys6|Eltávolított kulcsok (szilánkok 6)|Darabszám|Összes||
|totalkeys6|Összes kulcs (szilánkok 6)|Darabszám|Maximális||
|expiredkeys6|Lejárt kulcsokkal (szilánkok 6)|Darabszám|Összes||
|usedmemory6|Használt memória (szilánkok 6)|Bájtok|Maximális||
|usedmemoryRss6|RSS (szilánkok 6) a használt memória|Bájtok|Maximális||
|serverLoad6|Kiszolgáló terhelését (szilánkok 6)|Százalék|Maximális||
|cacheWrite6|Gyorsítótár-írási (szilánkok 6)|BytesPerSecond|Maximális||
|cacheRead6|Gyorsítótár olvasható (szilánkok 6)|BytesPerSecond|Maximális||
|percentProcessorTime6|Processzor (szilánkok 6)|Százalék|Maximális||
|connectedclients7|Csatlakozott ügyfelek (szilánkok 7)|Darabszám|Maximális||
|totalcommandsprocessed7|Összes művelet (szilánkok 7)|Darabszám|Összes||
|cachehits7|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 7)|Darabszám|Összes||
|cachemisses7|A gyorsítótárbeli (szilánkok 7)|Darabszám|Összes||
|getcommands7|Lekérdezi a (szilánkok 7)|Darabszám|Összes||
|setcommands7|Beállítása (szilánkok 7)|Darabszám|Összes||
|evictedkeys7|Eltávolított kulcsok (szilánkok 7)|Darabszám|Összes||
|totalkeys7|Összes kulcs (szilánkok 7)|Darabszám|Maximális||
|expiredkeys7|Lejárt kulcsokkal (szilánkok 7)|Darabszám|Összes||
|usedmemory7|Használt memória (szilánkok 7)|Bájtok|Maximális||
|usedmemoryRss7|Használt memória RSS (szilánkok 7)|Bájtok|Maximális||
|serverLoad7|Kiszolgáló terhelését (szilánkok 7)|Százalék|Maximális||
|cacheWrite7|Gyorsítótár-írási (szilánkok 7)|BytesPerSecond|Maximális||
|cacheRead7|Gyorsítótár olvasható (szilánkok 7)|BytesPerSecond|Maximális||
|percentProcessorTime7|Processzor (szilánkok 7)|Százalék|Maximális||
|connectedclients8|Csatlakozott ügyfelek (szilánkok 8)|Darabszám|Maximális||
|totalcommandsprocessed8|Összes művelet (szilánkok 8)|Darabszám|Összes||
|cachehits8|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 8)|Darabszám|Összes||
|cachemisses8|A gyorsítótárbeli (szilánkok 8)|Darabszám|Összes||
|getcommands8|Lekérdezi a (szilánkok 8)|Darabszám|Összes||
|setcommands8|Beállítása (szilánkok 8)|Darabszám|Összes||
|evictedkeys8|Eltávolított kulcsok (szilánkok 8)|Darabszám|Összes||
|totalkeys8|Összes kulcs (szilánkok 8)|Darabszám|Maximális||
|expiredkeys8|Lejárt kulcsokkal (szilánkok 8)|Darabszám|Összes||
|usedmemory8|Használt memória (szilánkok 8)|Bájtok|Maximális||
|usedmemoryRss8|Használt memória RSS (szilánkok 8)|Bájtok|Maximális||
|serverLoad8|Kiszolgáló terhelését (szilánkok 8)|Százalék|Maximális||
|cacheWrite8|Gyorsítótár-írási (szilánkok 8)|BytesPerSecond|Maximális||
|cacheRead8|Gyorsítótár olvasható (szilánkok 8)|BytesPerSecond|Maximális||
|percentProcessorTime8|Processzor (szilánkok 8)|Százalék|Maximális||
|connectedclients9|Csatlakozott ügyfelek (szilánkok 9)|Darabszám|Maximális||
|totalcommandsprocessed9|Összes művelet (szilánkok 9)|Darabszám|Összes||
|cachehits9|Találatot eredményező gyorsítótárbeli kereséseinek (szilánkok 9)|Darabszám|Összes||
|cachemisses9|A gyorsítótárbeli (szilánkok 9)|Darabszám|Összes||
|getcommands9|Lekérdezi a (szilánkok 9)|Darabszám|Összes||
|setcommands9|Beállítása (szilánkok 9)|Darabszám|Összes||
|evictedkeys9|Eltávolított kulcsok (szilánkok 9)|Darabszám|Összes||
|totalkeys9|Összes kulcs (szilánkok 9)|Darabszám|Maximális||
|expiredkeys9|Lejárt kulcsokkal (szilánkok 9)|Darabszám|Összes||
|usedmemory9|Használt memória (szilánkok 9)|Bájtok|Maximális||
|usedmemoryRss9|Használt memória RSS (szilánkok 9)|Bájtok|Maximális||
|serverLoad9|Kiszolgáló terhelését (szilánkok 9)|Százalék|Maximális||
|cacheWrite9|Gyorsítótár-írási (szilánkok 9)|BytesPerSecond|Maximális||
|cacheRead9|Gyorsítótár olvasható (szilánkok 9)|BytesPerSecond|Maximális||
|percentProcessorTime9|Processzor (szilánkok 9)|Százalék|Maximális||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|TotalCalls|Az összes hívás|Darabszám|Összes|Hívások száma összesen.|
|SuccessfulCalls|Sikeres hívás|Darabszám|Összes|Sikeres hívások száma.|
|TotalErrors|Az összes hiba száma|Darabszám|Összes|Hibaüzenetet (HTTP-válasz kód 4xx vagy 5xx) végződő hívások száma.|
|BlockedCalls|Letiltott hívások|Darabszám|Összes|Adott túllépése sebessége vagy kvótakorlátot hívások száma.|
|ServerErrors|Kiszolgáló hibák|Darabszám|Összes|Belső szolgáltatáshiba (HTTP válasz kód 5xx) végződő hívások száma.|
|ClientErrors|Ügyfél-hibák|Darabszám|Összes|Ügyfél kiszolgálóoldali hiba (HTTP válasz kód 4xx) végződő hívások száma.|
|DataIn|Az adatok|Bájtok|Összes|A bejövő adatok (bájt) mérete.|
|DataOut|Kimenő adatforgalmat|Bájtok|Összes|Bájtban kimenő adatok mérete.|
|Késés|Késés|Ideje ezredmásodpercben|Átlagos|Késés ezredmásodpercben.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|Processzor-százalékos aránya|Processzor-százalékos aránya|Százalék|Átlagos|hello aránya, amelyek jelenleg a következő virtuális gépek hello használják lefoglalt számítási egység|
|A hálózati|A hálózati|Bájtok|Összes|hello minden hálózati interfészen hello (bejövő forgalom) virtuális gépek által fogadott bájtok száma|
|Kimenő hálózati|Kimenő hálózati|Bájtok|Összes|hello bájtban ki a virtuális gépek (kimenő forgalmat) hello által hálózati adaptereken|
|Lemez olvasott bájtok|Lemez olvasott bájtok|Bájtok|Összes|Az összes bájt beolvasása a lemezről figyelése időszak során|
|Lemez írt bájtok száma|Lemez írt bájtok száma|Bájtok|Összes|Toodisk időszak ellenőrzése során írt bájtok száma összesen|
|Lemezolvasási műveletek másodpercenkénti száma|Lemezolvasási műveletek másodpercenkénti száma|CountPerSecond|Átlagos|Lemez olvasási IOPS|
|Lemez írási művelet/mp|Lemez írási művelet/mp|CountPerSecond|Átlagos|Lemez írási IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|Processzor-százalékos aránya|Processzor-százalékos aránya|Százalék|Átlagos|hello aránya, amelyek jelenleg a következő virtuális gépek hello használják lefoglalt számítási egység|
|A hálózati|A hálózati|Bájtok|Összes|hello minden hálózati interfészen hello (bejövő forgalom) virtuális gépek által fogadott bájtok száma|
|Kimenő hálózati|Kimenő hálózati|Bájtok|Összes|hello bájtban ki a virtuális gépek (kimenő forgalmat) hello által hálózati adaptereken|
|Lemez olvasott bájtok|Lemez olvasott bájtok|Bájtok|Összes|Az összes bájt beolvasása a lemezről figyelése időszak során|
|Lemez írt bájtok száma|Lemez írt bájtok száma|Bájtok|Összes|Toodisk időszak ellenőrzése során írt bájtok száma összesen|
|Lemezolvasási műveletek másodpercenkénti száma|Lemezolvasási műveletek másodpercenkénti száma|CountPerSecond|Átlagos|Lemez olvasási IOPS|
|Lemez írási művelet/mp|Lemez írási művelet/mp|CountPerSecond|Átlagos|Lemez írási IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|Processzor-százalékos aránya|Processzor-százalékos aránya|Százalék|Átlagos|hello aránya, amelyek jelenleg a következő virtuális gépek hello használják lefoglalt számítási egység|
|A hálózati|A hálózati|Bájtok|Összes|hello minden hálózati interfészen hello (bejövő forgalom) virtuális gépek által fogadott bájtok száma|
|Kimenő hálózati|Kimenő hálózati|Bájtok|Összes|hello bájtban ki a virtuális gépek (kimenő forgalmat) hello által hálózati adaptereken|
|Lemez olvasott bájtok|Lemez olvasott bájtok|Bájtok|Összes|Az összes bájt beolvasása a lemezről figyelése időszak során|
|Lemez írt bájtok száma|Lemez írt bájtok száma|Bájtok|Összes|Toodisk időszak ellenőrzése során írt bájtok száma összesen|
|Lemezolvasási műveletek másodpercenkénti száma|Lemezolvasási műveletek másodpercenkénti száma|CountPerSecond|Átlagos|Lemez olvasási IOPS|
|Lemez írási művelet/mp|Lemez írási művelet/mp|CountPerSecond|Átlagos|Lemez írási IOPS|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|DCIApiCalls|Ügyfél Insights API-hívások|Darabszám|Összes||
|DCIMappingImportOperationSuccessfulLines|Leképezési importálási művelet sikeres sorok|Darabszám|Összes||
|DCIMappingImportOperationFailedLines|Sorok leképezése az importálási művelet sikertelen volt.|Darabszám|Összes||
|DCIMappingImportOperationTotalLines|Leképezési importálási művelet teljes sorok|Darabszám|Összes||
|DCIMappingImportOperationRuntimeInSeconds|Leképezési importálási művelet futásidejű másodpercben.|másodperc|Összes||
|DCIOutboundProfileExportSucceeded|Kimenő profil exportálása sikeres volt.|Darabszám|Összes||
|DCIOutboundProfileExportFailed|Nem sikerült kimenő profil exportálása|Darabszám|Összes||
|DCIOutboundProfileExportDuration|Kimenő profil exportálása időtartama|másodperc|Összes||
|DCIOutboundKpiExportSucceeded|Kimenő Kpi exportálása sikeres volt.|Darabszám|Összes||
|DCIOutboundKpiExportFailed|Kimenő Kpi az exportálás sikertelen.|Darabszám|Összes||
|DCIOutboundKpiExportDuration|Kimenő Kpi exportálási időtartama|másodperc|Összes||
|DCIOutboundKpiExportStarted|Kimenő Kpi Exportálás indítása|másodperc|Összes||
|DCIOutboundKpiRecordCount|Kimenő Kpi rekordszám|másodperc|Összes||
|DCIOutboundProfileExportCount|Kimenő profil exportálása száma|másodperc|Összes||
|DCIOutboundInitialProfileExportFailed|Nem sikerült kimenő kezdeti profil exportálása|másodperc|Összes||
|DCIOutboundInitialProfileExportSucceeded|Kimenő kezdeti profil exportálása sikeres volt.|másodperc|Összes||
|DCIOutboundInitialKpiExportFailed|Kimenő kezdeti Kpi az exportálás sikertelen.|másodperc|Összes||
|DCIOutboundInitialKpiExportSucceeded|Kimenő kezdeti Kpi exportálása sikerült|másodperc|Összes||
|DCIOutboundInitialProfileExportDurationInSeconds|Kimenő kezdeti profil exportálása időtartama másodpercben|másodperc|Összes||
|AdlaJobForStandardKpiFailed|Nem sikerült másodpercben szabványos Kpi Adla feladata|másodperc|Összes||
|AdlaJobForStandardKpiTimeOut|Adla feladat szabványos Kpi időkorlát másodpercben|másodperc|Összes||
|AdlaJobForStandardKpiCompleted|Standard Kpi másodperc alatt befejeződött Adla feladata|másodperc|Összes||
|ImportASAValuesFailed|Importálás ASA értékek száma nem sikerült.|Darabszám|Összes||
|ImportASAValuesSucceeded|Importálás ASA értékek száma sikeres volt.|Darabszám|Összes||

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|JobEndedSuccess|Sikeres feladatok|Darabszám|Összes|Sikertelen feladatok száma.|
|JobEndedFailure|A sikertelen feladatok|Darabszám|Összes|A sikertelen feladatok száma.|
|JobEndedCancelled|Megszakított feladatok|Darabszám|Összes|Megszakított feladatok száma.|
|JobAUEndedSuccess|Sikeres AU idő|másodperc|Összes|Sikertelen feladatok teljes AU ideje.|
|JobAUEndedFailure|Nem sikerült AU idő|másodperc|Összes|A sikertelen feladatok teljes AU ideje.|
|JobAUEndedCancelled|Megszakított AU idő|másodperc|Összes|Megszakított feladatok teljes AU ideje.|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|cpu_percent|Processzor százalékban|Százalék|Átlagos|Processzor százalékban|
|compute_limit|Számítási egység korlátot|Darabszám|Átlagos|Számítási egység korlátot|
|compute_consumption_percent|Számítási egység százalékos aránya|Százalék|Átlagos|Számítási egység százalékos aránya|
|memory_percent|Memória százaléka|Százalék|Átlagos|Memória százaléka|
|io_consumption_percent|IO-százaléka|Százalék|Átlagos|IO-százaléka|
|storage_percent|Tárolási százalékos aránya|Százalék|Átlagos|Tárolási százalékos aránya|
|storage_used|Felhasznált tárterület|Bájtok|Átlagos|Felhasznált tárterület|
|storage_limit|Tárolási kapacitása|Bájtok|Átlagos|Tárolási kapacitása|
|active_connections|Aktív kapcsolatok száma|Darabszám|Átlagos|Aktív kapcsolatok száma|
|connections_failed|Nem sikerült kapcsolatok száma|Darabszám|Átlagos|Nem sikerült kapcsolatok száma|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|cpu_percent|Processzor százalékban|Százalék|Átlagos|Processzor százalékban|
|compute_limit|Számítási egység korlátot|Darabszám|Átlagos|Számítási egység korlátot|
|compute_consumption_percent|Számítási egység százalékos aránya|Százalék|Átlagos|Számítási egység százalékos aránya|
|memory_percent|Memória százaléka|Százalék|Átlagos|Memória százaléka|
|io_consumption_percent|IO-százaléka|Százalék|Átlagos|IO-százaléka|
|storage_percent|Tárolási százalékos aránya|Százalék|Átlagos|Tárolási százalékos aránya|
|storage_used|Felhasznált tárterület|Bájtok|Átlagos|Felhasznált tárterület|
|storage_limit|Tárolási kapacitása|Bájtok|Átlagos|Tárolási kapacitása|
|active_connections|Aktív kapcsolatok száma|Darabszám|Átlagos|Aktív kapcsolatok száma|
|connections_failed|Nem sikerült kapcsolatok száma|Darabszám|Átlagos|Nem sikerült kapcsolatok száma|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetria üzenet küldési kísérlet|Darabszám|Összes|Elküldött tooyour IoT-központ eszközről a felhőbe telemetriai üzenetek megkísérelt toobe száma|
|d2c.telemetry.ingress.SUCCESS|Küldött telemetriai üzenetek|Darabszám|Összes|Tooyour IoT-központ sikeresen elküldve az eszközről a felhőbe telemetriai üzenetek száma|
|c2d.commands.egress.Complete.SUCCESS|A parancsok befejeződött|Darabszám|Összes|Hello eszköz által végrehajtott felhő eszközre parancsok száma|
|c2d.commands.egress.Abandon.SUCCESS|Elhagyott parancsok|Darabszám|Összes|Hello eszköz által félbe hagyott felhő eszközre parancsok száma|
|c2d.commands.egress.Reject.SUCCESS|Elutasított parancsok|Darabszám|Összes|Hello eszköz által elutasított felhő eszközre parancsok száma|
|devices.totalDevices|Eszközök teljes száma|Darabszám|Összes|A regisztrált eszközök száma tooyour IoT-központ|
|devices.connectedDevices.allProtocol|Csatlakoztatott eszközök|Darabszám|Összes|A csatlakoztatott eszközök száma tooyour IoT-központ|
|d2c.telemetry.egress.SUCCESS|Telemetria kézbesített üzenetek|Darabszám|Összes|Ennyiszer üzenetek sikeresen készültek tooendpoints (összesen)|
|d2c.telemetry.egress.dropped|Az eldobott üzenetek|Darabszám|Összes|Kihagyott, mert hello kézbesítési végpont elhalt üzenetek száma|
|d2c.telemetry.egress.Orphaned|Árva üzenetek|Darabszám|Összes|hello bármely útvonalakat, beleértve a hello tartalék útvonal nem megfelelő üzenetek száma|
|d2c.telemetry.egress.Invalid|Érvénytelen üzenet|Darabszám|Összes|hello üzenetek száma nem érkezik miatt hello végponttal tooincompatibility|
|d2c.telemetry.egress.fallback|Tartalék feltételnek megfelelő üzenetek|Darabszám|Összes|Tartalék végpont toohello üzenetek száma|
|d2c.endpoints.egress.eventHubs|Üzenetek kézbesítése történik tooEvent központok végpontjai|Darabszám|Összes|Üzenetek volt sikeresen írásbeli tooEvent Hub végpontok száma|
|d2c.endpoints.latency.eventHubs|Az Event Hubs végpontok üzenet késés|ideje ezredmásodpercben|Átlagos|hello átlagos közötti késés üzenet érkező toohello IoT-központ és az üzenet belépő az Event Hubs végpont, ezredmásodpercben|
|d2c.endpoints.egress.serviceBusQueues|Üzenetek kézbesítése történik tooService Bus-üzenetsorba végpontok|Darabszám|Összes|Üzenetek volt sikeresen írásbeli tooService Bus-üzenetsorba végpontok száma|
|d2c.endpoints.latency.serviceBusQueues|A Service Bus-üzenetsorba végpontokhoz üzenet késés|ideje ezredmásodpercben|Átlagos|hello átlagos közötti késés üzenet érkező toohello IoT-központ és az üzenet belépő be egy Service Bus-üzenetsorba végpont ezredmásodpercben|
|d2c.endpoints.egress.serviceBusTopics|Üzenetek kézbesítése történik tooService Bus-témakörbe végpontok|Darabszám|Összes|Üzenetek volt sikeresen írásbeli tooService Bus-témakörbe végpontok száma|
|d2c.endpoints.latency.serviceBusTopics|A Service Bus-témakörbe végpontokhoz üzenet késés|ideje ezredmásodpercben|Átlagos|hello átlagos közötti késés üzenet érkező toohello IoT-központ és az üzenet belépő be egy Service Bus-témakörbe végpont ezredmásodpercben|
|d2c.endpoints.egress.builtIn.events|Üzenetek kézbesítése történik toohello beépített végpont (üzenet/esemény)|Darabszám|Összes|Ennyiszer üzenetek volt sikeresen írásbeli toohello beépített végpont (üzenet/esemény)|
|d2c.endpoints.latency.builtIn.events|Üzenet késés hello beépített végpont (üzenet/esemény)|ideje ezredmásodpercben|Átlagos|hello átlagos várakozási ideje üzenet érkező toohello IoT-központ és az üzenet érkező között történő hello beépített végpont (üzenet/esemény), ezredmásodpercben |
|d2c.Twin.Read.SUCCESS|Sikeres iker olvassa be az eszközökön|Darabszám|Összes|olvassa be az összes sikeres eszköz által kezdeményezett iker hello száma.|
|d2c.Twin.Read.failure|Nem sikerült a kettős olvasások eszközökről|Darabszám|Összes|hello teljes számát a két eszköz által kezdeményezett olvasás sikertelen volt.|
|d2c.Twin.Read.size|A két válasz mérete olvassa be az eszközökön|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének minden sikeres eszköz által kezdeményezett iker olvasási műveletek.|
|d2c.Twin.Update.SUCCESS|Sikeres iker frissítések eszközökről|Darabszám|Összes|a két eszköz által kezdeményezett összes sikeres frissítések hello száma.|
|d2c.Twin.Update.failure|Nem sikerült a két frissítések eszközökről|Darabszám|Összes|hello teljes számát a két eszköz által kezdeményezett frissítések nem sikerült.|
|d2c.Twin.Update.size|A két frissítések eszközökről mérete|Bájtok|Átlagos|hello átlagos, minimális és maximális méretét az összes sikeres eszköz által kezdeményezett iker frissítések.|
|c2d.methods.SUCCESS|Sikeres közvetlen módszer meghívásához|Darabszám|Összes|az összes sikeres közvetlen metódushívások hello száma.|
|c2d.methods.failure|Nem sikerült a közvetlen módszer meghívásához|Darabszám|Összes|hello teljes számát a közvetlen metódushívások nem sikerült.|
|c2d.methods.requestSize|A közvetlen módszer meghívásához kérelem mérete|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének minden sikeres közvetlen metódusú kérelmeket.|
|c2d.methods.responseSize|Válasz mérete közvetlen módszer meghívásához|Bájtok|Átlagos|hello átlagos, a min és a max az összes sikeres közvetlen módszer válasz.|
|c2d.Twin.Read.SUCCESS|Sikeres iker háttérből olvassa be|Darabszám|Összes|olvassa be az összes sikeres back-end által kezdeményezett iker hello száma.|
|c2d.Twin.Read.failure|Sikertelen a két háttérből olvassa be|Darabszám|Összes|hello teljes számát a két back-end által kezdeményezett olvasások nem sikerült.|
|c2d.Twin.Read.size|A válasz méretének iker olvasások háttérből|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének minden sikeres back-end által kezdeményezett iker olvasási műveletek.|
|c2d.Twin.Update.SUCCESS|Sikeres iker frissítések háttérből|Darabszám|Összes|az összes sikeres back-end által kezdeményezett iker frissítések hello száma.|
|c2d.Twin.Update.failure|Sikertelen a két frissítések háttérből|Darabszám|Összes|hello teljes számát a két back-end által kezdeményezett frissítések nem sikerült.|
|c2d.Twin.Update.size|A két frissítések háttérből mérete|Bájtok|Átlagos|hello átlagos, minimális és maximális méretét az összes sikeres back-end által kezdeményezett iker frissítések.|
|twinQueries.success|Sikeres iker lekérdezések|Darabszám|Összes|az összes sikeres iker lekérdezés hello száma.|
|twinQueries.failure|Sikertelen a két lekérdezések|Darabszám|Összes|összes sikertelen iker lekérdezések hello száma.|
|twinQueries.resultSize|A két lekérdezések eredmény méretének|Bájtok|Átlagos|hello átlagos, minimális és maximális értékének hello eredmény méretének minden sikeres iker lekérdezések.|
|jobs.createTwinUpdateJob.success|A sikeres és a két frissítési feladatok|Darabszám|Összes|a két frissítési feladatok minden sikeres létrehozása hello száma.|
|jobs.createTwinUpdateJob.failure|Sikertelen és a két frissítési feladatok|Darabszám|Összes|a két frissítési feladatok során létrehozására tett sikertelen kísérlet hello száma.|
|jobs.createDirectMethodJob.success|A metódus meghívása feladatok sikeres létrehozása|Darabszám|Összes|közvetlen metódus meghívása feladatok minden sikeres létrehozása hello száma.|
|jobs.createDirectMethodJob.failure|A metódus meghívása feladatok sikertelen számát|Darabszám|Összes|közvetlen metódus meghívása feladatok során létrehozására tett sikertelen kísérlet hello száma.|
|jobs.listJobs.success|Sikeres hívás toolist feladatok|Darabszám|Összes|hello száma az összes sikeres hívás toolist feladat.|
|jobs.listJobs.failure|Sikertelen hívások toolist feladatok|Darabszám|Összes|összes sikertelen hívás toolist feladat hello száma.|
|jobs.cancelJob.success|Megszakított feladatok sikeres|Darabszám|Összes|az összes sikeres hello száma meghívja toocancel egy feladatot.|
|jobs.cancelJob.failure|Sikertelen feladat sikertelen|Darabszám|Összes|összes sikertelen hívás toocancel egy feladat hello száma.|
|jobs.queryJobs.success|Feladat sikeres lekérdezések|Darabszám|Összes|hello száma az összes sikeres hívás tooquery feladat.|
|jobs.queryJobs.failure|Sikertelen feladat-lekérdezések|Darabszám|Összes|összes sikertelen hívás tooquery feladat hello száma.|
|jobs.Completed|Befejezett feladatokhoz|Darabszám|Összes|az összes befejezett feladatokhoz hello számát.|
|jobs.Failed|A sikertelen feladatok|Darabszám|Összes|összes sikertelen feladatok száma hello.|
|d2c.telemetry.ingress.sendThrottle|Sávszélesség-szabályozási hibák száma|Darabszám|Összes|Toodevice átviteli szabályozások miatt szabályozás hibák száma|
|dailyMessageQuotaUsed|Használt üzenetek teljes száma|Darabszám|Átlagos|Jelenleg használt teljes üzenetek száma|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|INREQS|Bejövő küldési kérelmek|Darabszám|Összes|Teljes bejövő egy értesítési központot kérelem elküldése|
|SUCCREQ|Sikeres kérelmei|Darabszám|Összes|A sikeres kérelmek teljes névtér|
|FAILREQ|Sikertelen kérelmek|Darabszám|Összes|A sikertelen kérelmek teljes névtér|
|SVRBSY|Kiszolgáló elfoglalt hibák|Darabszám|Összes|Névtér a teljes kiszolgáló foglalt hibák|
|INTERR|Belső kiszolgálóhibákat|Darabszám|Összes|Teljes belső kiszolgálóhibákat névtér|
|MISCERR|Egyéb hibák|Darabszám|Összes|A sikertelen kérelmek teljes névtér|
|INMSGS|A bejövő üzenetek|Darabszám|Összes|Egy névtér teljes bejövő üzenete|
|OUTMSGS|Kimenő üzenetek|Darabszám|Összes|Teljes kimenő üzenetek névtér|
|EHINMBS|Bejövő bájtok|Bájtok|Összes|Event Hub bejövő üzenet átviteli névtér|
|EHOUTMBS|Kimenő bájtok|Bájtok|Összes|Teljes kimenő üzenetek névtér|
|EHABL|Archív várakozó üzenetek|Darabszám|Összes|Hub archív eseményüzeneteket a névtér várakozó fájlok|
|EHAMSGS|Üzenetek archiválása|Darabszám|Összes|Az Event Hubs archivált névtérben levő üzenetek|
|EHAMBS|Archív üzenet átviteli sebesség|Bájtok|Összes|Az Event Hubs archivált üzenet átviteli névtérben|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|RunsStarted|Futtatása megkezdődött|Darabszám|Összes|Az elindított munkafolyamat-futtatások száma.|
|RunsCompleted|Futtatása befejeződött|Darabszám|Összes|A befejezett munkafolyamat-futtatások száma.|
|RunsSucceeded|Futtatása sikeres volt.|Darabszám|Összes|A sikeres munkafolyamat-futtatások száma.|
|RunsFailed|Nem sikerült fut.|Darabszám|Összes|A sikertelen munkafolyamat-futtatások száma.|
|RunsCancelled|Megszakítva fut.|Darabszám|Összes|A megszakított munkafolyamat-futtatások száma.|
|RunLatency|Futtassa a késés|másodperc|Átlagos|A befejezett munkafolyamat-futtatások késése.|
|RunSuccessLatency|Futtassa a sikeres késés|másodperc|Átlagos|A sikeres munkafolyamat-futtatások késése.|
|RunThrottledEvents|Futtassa a szabályozottan halmozott események|Darabszám|Összes|A munkafolyamat-műveletek vagy -eseményindítók által elindított események száma.|
|RunFailurePercentage|Futtassa a hiba százalékos aránya|Százalék|Összes|Százalékos aránya a munkafolyamat futtatása sikertelen.|
|ActionsStarted|A műveletek indítása |Darabszám|Összes|Az elindított munkafolyamat-műveletek száma.|
|ActionsCompleted|A műveletek befejezése |Darabszám|Összes|A befejezett munkafolyamat-műveletek száma.|
|ActionsSucceeded|A művelet sikeres volt. |Darabszám|Összes|A sikeres munkafolyamat-műveletek száma.|
|ActionsFailed|Nem sikerült műveletek|Darabszám|Összes|A sikertelen munkafolyamat-műveletek száma.|
|ActionsSkipped|Kihagyott műveletek |Darabszám|Összes|A kihagyott munkafolyamat-műveletek száma.|
|ActionLatency|A művelet késés |másodperc|Átlagos|A befejezett munkafolyamat-műveletek késése.|
|ActionSuccessLatency|A művelet sikeres késés |másodperc|Átlagos|A sikeres munkafolyamat-műveletek késése.|
|ActionThrottledEvents|A művelet Halmozódni események|Darabszám|Összes|A munkafolyamat-műveletek által elindított események száma.|
|TriggersStarted|Eseményindítók indítása |Darabszám|Összes|Az elindított munkafolyamat-eseményindítók száma.|
|TriggersCompleted|Eseményindítók befejeződött |Darabszám|Összes|A befejezett munkafolyamat-eseményindítók száma.|
|TriggersSucceeded|Eseményindítók sikeres volt. |Darabszám|Összes|A sikeres munkafolyamat-eseményindítók száma.|
|TriggersFailed|Nem sikerült az eseményindítók |Darabszám|Összes|A sikertelen munkafolyamat-eseményindítók száma.|
|TriggersSkipped|Kihagyott eseményindítók|Darabszám|Összes|A kihagyott munkafolyamat-eseményindítók száma.|
|TriggersFired|Eseményindítók következik |Darabszám|Összes|Az aktivált munkafolyamat-eseményindítók száma.|
|TriggerLatency|Eseményindító késés |másodperc|Átlagos|A befejezett munkafolyamat-eseményindítók késése.|
|TriggerFireLatency|Eseményindító tűz késés |másodperc|Átlagos|Az aktivált munkafolyamat-eseményindítók késése.|
|TriggerSuccessLatency|Eseményindító sikeres késés |másodperc|Átlagos|A sikeres munkafolyamat-eseményindítók késése.|
|TriggerThrottledEvents|Eseményindító Halmozódni események|Darabszám|Összes|A munkafolyamat-eseményindítók által elindított események száma.|
|BillableActionExecutions|Számlázható művelet végrehajtások|Darabszám|Összes|Munkafolyamat-művelet végrehajtások első számlázva száma.|
|BillableTriggerExecutions|Számlázható eseményindító végrehajtások|Darabszám|Összes|Munkafolyamat eseményindító végrehajtások első számlázva száma.|
|TotalBillableExecutions|Teljes számlázható végrehajtások|Darabszám|Összes|Első számlázva munkafolyamat végrehajtások száma.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|Teljesítmény|Teljesítmény|BytesPerSecond|Átlagos||

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|BytesIn|BytesIn|Darabszám|Összes||
|BytesOut|BytesOut|Darabszám|Összes||

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|registration.all|Regisztrációs művelet|Darabszám|Összes|hello száma az összes sikeres regisztráció művelet (a létrehozott frissítések lekérdezések és a törlések). |
|registration.Create|Regisztrációs műveletek létrehozása|Darabszám|Összes|az összes sikeres regisztráció és hello számát.|
|registration.Update|Regisztráció a frissítési műveletek|Darabszám|Összes|minden sikeres regisztráció frissítés hello száma.|
|registration.Get|Regisztrációs olvasási műveletek|Darabszám|Összes|az összes sikeres regisztráció lekérdezés hello száma.|
|registration.delete|Regisztrációs Delete művelet|Darabszám|Összes|minden sikeres regisztráció törlési hello száma.|
|bejövő|A bejövő üzenetek|Darabszám|Összes|az összes sikeres hello száma az API-hívásokban küldése. |
|Incoming.Scheduled|Ütemezett leküldéses értesítések küldése|Darabszám|Összes|Ütemezett leküldéses értesítések megszakítva|
|Incoming.Scheduled.Cancel|Ütemezett leküldéses értesítések megszakítva|Darabszám|Összes|Ütemezett leküldéses értesítések megszakítva|
|Scheduled.Pending|Függőben lévő ütemezett értesítések|Darabszám|Összes|Függőben lévő ütemezett értesítések|
|Installation.all|Telepítési műveletek|Darabszám|Összes|Telepítési műveletek|
|Installation.Get|Telepítési műveletek beolvasása|Darabszám|Összes|Telepítési műveletek beolvasása|
|Installation.upsert|Telepítés-létrehozási és -frissítési műveletek|Darabszám|Összes|Telepítés-létrehozási és -frissítési műveletek|
|Installation.Patch|Javítás telepítési műveletek|Darabszám|Összes|Javítás telepítési műveletek|
|Installation.delete|Telepítési műveletek törlése|Darabszám|Összes|Telepítési műveletek törlése|
|outgoing.allpns.SUCCESS|Sikeres értesítések|Darabszám|Összes|minden sikeres értesítések hello száma.|
|outgoing.allpns.invalidpayload|Hasznos hibák|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello PNS hibát adott vissza a rossz hasznos hello száma.|
|outgoing.allpns.pnserror|Külső értesítési rendszerhibák|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert hiba történt (nem tartalmazza a hitelesítési problémákat az) PNS hello folytatott kommunikáció hello száma.|
|outgoing.allpns.channelerror|Csatorna által okozott hibák|Darabszám|Összes|hello száma leküldéses értesítések hello csatorna érvénytelensége miatt nem hozzárendelt alkalmazás szabályozva, vagy lejárt a megfelelő hello sikertelen.|
|outgoing.allpns.badorexpiredchannel|Rossz vagy lejárt csatorna által okozott hibák|Darabszám|Összes|hello száma leküldéses értesítések, amelyek nem sikerült, mert a hello regisztrációs csatorna/token/registrationId hello lejárt vagy érvénytelen.|
|outgoing.wns.SUCCESS|WNS sikeres értesítések|Darabszám|Összes|minden sikeres értesítések hello száma.|
|outgoing.wns.invalidcredentials|WNS engedélyezési hibái (érvénytelen hitelesítő adatok)|Darabszám|Összes|hello száma leküldéses értesítések, melyeknél nem sikerült, mert hello PNS nem fogadta el hello megadott hitelesítő adatok vagy hello hitelesítő adatok le vannak tiltva. (Windows Live nem ismeri fel hello hitelesítő adatok).|
|outgoing.wns.badchannel|WNS hibás csatorna hiba|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert a rendszer nem ismerte fel a hello ChannelURI hello regisztrációhoz száma hello (WNS állapota: 404 nem található).|
|outgoing.wns.expiredchannel|WNS lejárt csatorna hiba|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello ChannelURI lejárt száma hello (WNS állapota: 410 Gone).|
|outgoing.wns.throttled|WNS Halmozódni értesítések|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert ez az alkalmazás a WNS szabályozás száma hello (WNS állapota: 406 nem elfogadható).|
|outgoing.wns.tokenproviderunreachable|WNS engedélyezési hibái (nem érhető el)|Darabszám|Összes|Windows Live nem érhető el.|
|outgoing.wns.invalidtoken|WNS engedélyezési hibái (lexikális elem érvénytelen)|Darabszám|Összes|hello jogkivonat megadott tooWNS érvénytelen (WNS állapota: 401 nem engedélyezett).|
|outgoing.wns.wrongtoken|WNS engedélyezési hibái (helytelen Token)|Darabszám|Összes|token hello megadott tooWNS érvényes, de egy másik alkalmazással (WNS állapota: 403 Tiltott). Ez akkor fordulhat elő, ha hello ChannelURI hello regisztrációhoz társítva egy másik alkalmazás. Ellenőrizze, hogy hello ügyfélalkalmazás hello társítva azonos alkalmazás-hitelesítő adatokkal rendelkező hello értesítési központban.|
|outgoing.wns.invalidnotificationformat|WNS érvénytelen értesítés formátuma|Darabszám|Összes|hello hello értesítési formátuma érvénytelen (WNS állapota: 400). Vegye figyelembe, hogy a wns-ből nem utasíthatja el minden érvénytelen Payload van jelen.|
|outgoing.wns.invalidnotificationsize|WNS – érvénytelen értesítésméret által okozott hiba|Darabszám|Összes|hello értesítési tartalom mérete túl nagy (WNS állapota: 413).|
|outgoing.wns.channelthrottled|WNS csatorna Halmozódni|Darabszám|Összes|hello értesítési el lett dobva, mert folyamatban van a hello ChannelURI hello regisztrációhoz (WNS válaszfejlécet: X-WNS-NotificationStatus:channelThrottled).|
|outgoing.wns.channeldisconnected|WNS csatorna kapcsolata megszakadt|Darabszám|Összes|hello értesítési el lett dobva, mert folyamatban van a hello ChannelURI hello regisztrációhoz (WNS válaszfejlécet: X-WNS-DeviceConnectionStatus: leválasztva).|
|outgoing.wns.dropped|WNS eldobott értesítések|Darabszám|Összes|hello értesítési el lett dobva, mert folyamatban van a hello ChannelURI hello regisztrációhoz (X-WNS-NotificationStatus: az eldobott, de nem X-WNS-DeviceConnectionStatus: leválasztva).|
|outgoing.wns.pnserror|WNS hibák|Darabszám|Összes|Értesítés a WNS-sel való kommunikáció során hibák miatt nem érkezik.|
|outgoing.wns.authenticationerror|WNS hitelesítési hibák|Darabszám|Összes|Az értesítés kézbesítése nem kommunikál a Windows Live érvénytelen hitelesítő adatokat vagy nem megfelelő jogkivonatot hibák miatt.|
|outgoing.apns.SUCCESS|APNS sikeres értesítések|Darabszám|Összes|minden sikeres értesítések hello száma.|
|outgoing.apns.invalidcredentials|APNS engedélyezési hibái|Darabszám|Összes|hello száma leküldéses értesítések, melyeknél nem sikerült, mert hello PNS nem fogadta el hello megadott hitelesítő adatok vagy hello hitelesítő adatok le vannak tiltva.|
|outgoing.apns.badchannel|APNS hibás csatorna hiba|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello token érvénytelen száma hello (APN-állapotkód: 8).|
|outgoing.apns.expiredchannel|APNS lejárt csatorna hiba|Darabszám|Összes|hello APNS visszajelzés csatornánként érvénytelenítve lett jogkivonatban hello száma.|
|outgoing.apns.invalidnotificationsize|APNS – érvénytelen értesítésméret által okozott hiba|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello tartalom túl nagy volt száma hello (APN-állapotkód: 7).|
|outgoing.apns.pnserror|APNS-hibák|Darabszám|Összes|hello száma leküldéses értesítések, amelyek kommunikálni az APN szolgáltatás hibái miatt nem sikerült.|
|outgoing.gcm.SUCCESS|Sikeres GCM-értesítések|Darabszám|Összes|minden sikeres értesítések hello száma.|
|outgoing.gcm.invalidcredentials|GCM engedélyezési hibái (érvénytelen hitelesítő adatok)|Darabszám|Összes|hello száma leküldéses értesítések, melyeknél nem sikerült, mert hello PNS nem fogadta el hello megadott hitelesítő adatok vagy hello hitelesítő adatok le vannak tiltva.|
|outgoing.gcm.badchannel|GCM hibás csatorna hiba|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert a rendszer nem ismerte fel a hello regisztrációs hello registrationId száma hello (GCM eredmény: Érvénytelen regisztrációs).|
|outgoing.gcm.expiredchannel|GCM lejárt csatorna hiba|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert lejárt a hello registrationId hello regisztrációhoz száma hello (GCM eredmény: NotRegistered).|
|outgoing.gcm.throttled|GCM Halmozódni értesítések|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert a GCM szabályozva az alkalmazás száma hello (GCM-állapotkód: 501-599 vagy eredmény: nem érhető el).|
|outgoing.gcm.invalidnotificationformat|GCM érvénytelen értesítés formátuma|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello forgalma nem megfelelő formátumú száma hello (GCM eredmény: InvalidDataKey vagy InvalidTtl).|
|outgoing.gcm.invalidnotificationsize|GCM – érvénytelen értesítésméret által okozott hiba|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello tartalom túl nagy volt száma hello (GCM eredmény: MessageTooBig).|
|outgoing.gcm.wrongchannel|GCM rossz csatorna hiba|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert a hello regisztrációs hello registrationId nem hello száma társított toohello aktuális app (GCM eredmény: InvalidPackageName).|
|outgoing.gcm.pnserror|GCM hibák|Darabszám|Összes|hello száma leküldéses értesítések, amelyek a GCM-mel való kommunikációhoz hibák miatt nem sikerült.|
|outgoing.gcm.authenticationerror|GCM hitelesítési hibák|Darabszám|Összes|leküldéses értesítések, melyeknél nem sikerült, mert hello PNS nem fogadta el hello megadott hitelesítő adatok hello hitelesítő adatok le vannak tiltva vagy hello SenderId nincs megfelelően beállítva a hello app száma hello (GCM eredmény: MismatchedSenderId).|
|outgoing.mpns.SUCCESS|MPNS sikeres értesítések|Darabszám|Összes|minden sikeres értesítések hello száma.|
|outgoing.mpns.invalidcredentials|MPNS érvénytelen hitelesítő adatok|Darabszám|Összes|hello száma leküldéses értesítések, melyeknél nem sikerült, mert hello PNS nem fogadta el hello megadott hitelesítő adatok vagy hello hitelesítő adatok le vannak tiltva.|
|outgoing.mpns.badchannel|MPNS hibás csatorna hiba|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert a rendszer nem ismerte fel a hello ChannelURI hello regisztrációhoz száma hello (MPNS állapota: 404 nem található).|
|outgoing.mpns.throttled|MPNS Halmozódni értesítések|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert az alkalmazás a MPNS szabályozás száma hello (WNS MPNS: 406 nem elfogadható).|
|outgoing.mpns.invalidnotificationformat|MPNS érvénytelen értesítés formátuma|Darabszám|Összes|hello száma leküldéses értesítések, melyeknél nem sikerült, mert hello értesítés hello tartalom túl nagy volt.|
|outgoing.mpns.channeldisconnected|MPNS csatorna kapcsolata megszakadt|Darabszám|Összes|leküldéses értesítések, amelyek nem sikerült, mert meg lett szakítva, hello ChannelURI hello regisztrációhoz száma hello (MPNS állapot: nem található 412).|
|outgoing.mpns.dropped|MPNS eldobott értesítések|Darabszám|Összes|leküldéses értesítések MPNS által eldobott száma hello (MPNS válaszfejlécet: X-NotificationStatus: QueueFull vagy Suppressed).|
|outgoing.mpns.pnserror|MPNS-hibák|Darabszám|Összes|hello száma leküldéses értesítések, amelyek MPNS folytatott kommunikáció hibák miatt nem sikerült.|
|outgoing.mpns.authenticationerror|MPNS hitelesítési hibák|Darabszám|Összes|hello száma leküldéses értesítések, melyeknél nem sikerült, mert hello PNS nem fogadta el hello megadott hitelesítő adatok vagy hello hitelesítő adatok le vannak tiltva.|
|notificationhub.pushes|Minden kimenő értesítések|Darabszám|Összes|Minden kimenő értesítések hello értesítési központ|
|Incoming.all.Requests|Az összes bejövő kérelmek|Darabszám|Összes|Bejövő kérelmek teljes száma a értesítési központ|
|Incoming.all.failedrequests|Minden bejövő sikertelen kérelmek|Darabszám|Összes|Teljes bejövő sikertelen kérelmek egy értesítési központ|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|qpu_metric|QPU|Darabszám|Átlagos|QPU. Tartomány 0 – 100 s1, 0-200 S2 és 0 – 400 S4|
|memory_metric|Memory (Memória)|Bájtok|Átlagos|A memória. S1 0-25 GB-ot, S2 0-50 GB-ot és 0 – 100 GB-ot S4|
|TotalConnectionRequests|Teljes csatlakozási kérések|Darabszám|Átlagos|Teljes csatlakozási kérések. Ezek a érkezők.|
|SuccessfullConnectionsPerSec|Sikeres kapcsolatok másodpercenkénti száma|CountPerSecond|Átlagos|Sikeres csatlakozás befejezésekhez aránya.|
|TotalConnectionFailures|Teljes kapcsolódási hibák|Darabszám|Átlagos|Összes sikertelen csatlakozási kísérletek.|
|CurrentUserSessions|Aktuális felhasználói munkamenetek|Darabszám|Átlagos|Létrehozott felhasználói munkamenetek jelenlegi száma.|
|QueryPoolBusyThreads|Lekérdezés készlet foglalt szálak|Darabszám|Átlagos|Hello lekérdezés szálkészlet foglalt szálak számát.|
|CommandPoolJobQueueLength|A parancs készlet feladat várólistájának hossza|Darabszám|Átlagos|Hello parancs szálkészlet hello sorban lévő feladatok száma.|
|ProcessingPoolJobQueueLength|Feldolgozási készlet feladat várólistájának hossza|Darabszám|Átlagos|Hello várólistában lévő hello szálkészlet feldolgozása nem-I/O-feladatok száma.|
|CurrentConnections|Kapcsolat: A jelenlegi kapcsolatok száma|Darabszám|Átlagos|Ügyfél fennálló kapcsolatok aktuális száma.|
|CleanerCurrentPrice|Memória: Tisztító aktuális árlista|Darabszám|Átlagos|Memória, a $/ bájt/idő, a normalizált too1000 aktuális ára.|
|CleanerMemoryShrinkable|Memória: Zsugoríthatósági tisztító memória|Bájtok|Átlagos|Memória mérete, tulajdonos toopurging által hello háttér tisztító mennyisége.|
|CleanerMemoryNonshrinkable|Memória: Tisztító memória nonshrinkable|Bájtok|Átlagos|Memória mérete, nem tulajdonos toopurging által hello háttér tisztító mennyisége.|
|MemoryUsage|Memória: Memória használata|Bájtok|Átlagos|Memóriahasználat hello server folyamat, tisztító memória ár kiszámításakor. Egyenlő toocounter Process\PrivateBytes plus hello memórialeképezési adatok méretétől, figyelmen kívül bármely memória leképezve, vagy le van foglalva hello xVelocity memórián belüli analytics motor (VertiPaq) meghaladja a hello xVelocity motor memóriára vonatkozó korlátozást.|
|MemoryLimitHard|Memória: Memóriakorlátot rögzített|Bájtok|Átlagos|Rögzített memóriakorlátot, konfigurációs fájlból.|
|MemoryLimitHigh|Memória: Memória korlátozása magas|Bájtok|Átlagos|Magas memóriakorlátot, konfigurációs fájlból.|
|MemoryLimitLow|Memória: Memória korlátja alacsony|Bájtok|Átlagos|Kevés a memória a határt, a konfigurációs fájlból.|
|MemoryLimitVertiPaq|Memória: Memória korlátja VertiPaq|Bájtok|Átlagos|A memória maximális mennyiségét, a konfigurációs fájlból.|
|Kvóta|Memória: kvóta|Bájtok|Átlagos|Aktuális memóriakvótáját, bájtban. Memóriakvótáját memória grant vagy memória foglalás is nevezik.|
|QuotaBlocked|Memória: Kvóta letiltva|Darabszám|Átlagos|Le vannak tiltva, amíg más memória kvóták felszabadítását kvóta kérelmek jelenlegi száma.|
|VertiPaqNonpaged|Memória: a nem lapozható VertiPaq|Bájtok|Átlagos|Bájt memóriát hello memóriabeli motorjához zárolt hello munkakészlet való használatra.|
|VertiPaqPaged|Memória: A VertiPaq lapozható|Bájtok|Átlagos|A memóriában levő használt lapozható memória bájt.|
|RowsReadPerSec|Feldolgozási: Sor olvasása másodpercenkénti száma|CountPerSecond|Átlagos|Sorok aránya az összes relációs adatbázisok olvasni.|
|RowsConvertedPerSec|Feldolgozási: Sorok konvertálni másodpercenkénti száma|CountPerSecond|Átlagos|Sorok aránya alakítja a feldolgozás során.|
|RowsWrittenPerSec|Feldolgozási: Másodpercenként írt sorok|CountPerSecond|Átlagos|Feldolgozás során írt sorok száma.|
|CommandPoolBusyThreads|Szálak: Parancs készlet foglalt szálak|Darabszám|Átlagos|Hello parancs szálkészlet foglalt szálak számát.|
|CommandPoolIdleThreads|Szálak: Parancs készlet üresjárati szálak|Darabszám|Átlagos|Hello parancs szálkészlet üresjárati szálak számát.|
|LongParsingBusyThreads|Szálak: Hosszú foglalt szálak elemzése|Darabszám|Átlagos|Hosszú elemzése a szálkészlet hello foglalt szálak számát.|
|LongParsingIdleThreads|Szálak: Hosszú üresjárati szálak elemzése|Darabszám|Átlagos|Hosszú elemzése a szálkészlet hello üresjárati szálak számát.|
|LongParsingJobQueueLength|Szálak: Hosszú elemzése feladat várólistájának hossza|Darabszám|Átlagos|A hosszú elemzése a szálkészlet hello hello sorban lévő feladatok száma.|
|ProcessingPoolBusyIOJobThreads|Szálak: Készlet foglalt i/o-feladat szálak feldolgozása|Darabszám|Átlagos|I/o-feladatok feldolgozási szálkészletben hello futó szálak száma.|
|ProcessingPoolBusyNonIOThreads|Szálak: Készlet foglalt nem-I/O-szálak feldolgozása|Darabszám|Átlagos|Nem-I/O-feladatok feldolgozási szálkészletben hello futó szálak száma.|
|ProcessingPoolIOJobQueueLength|Szálak: Feldolgozása készlet i/o-feldolgozás várólistájának hossza|Darabszám|Átlagos|Hello várólistában hello feldolgozási szálkészletben lévő i/o-feladatok száma.|
|ProcessingPoolIdleIOJobThreads|Szálak: Készlet üresjárati i/o-feladat szálak feldolgozása|Darabszám|Átlagos|A feldolgozási szálkészletben hello i/o-feladatok üresjárati szálak száma.|
|ProcessingPoolIdleNonIOThreads|Szálak: Készlet üresjárati nem-I/O-szálak feldolgozása|Darabszám|Átlagos|A feldolgozási szálkészletben hello üresjárati szálak száma dedikált toonon-I/O-feladatokat.|
|QueryPoolIdleThreads|Szálak: Lekérdezés készlet üresjárati szálak|Darabszám|Átlagos|A feldolgozási szálkészletben hello i/o-feladatok üresjárati szálak száma.|
|QueryPoolJobQueueLength|Szálak: Lekérdezés készlet feladat várólista lengt|Darabszám|Átlagos|Hello lekérdezés szálkészlet hello sorban lévő feladatok száma.|
|ShortParsingBusyThreads|Szálak: Rövid foglalt szálak elemzése|Darabszám|Átlagos|Rövid szálkészlet elemzése hello foglalt szálak számát.|
|ShortParsingIdleThreads|Szálak: Rövid üresjárati szálak elemzése|Darabszám|Átlagos|Rövid szálkészlet elemzése hello üresjárati szálak számát.|
|ShortParsingJobQueueLength|Szálak: Rövid elemzése feladat várólistájának hossza|Darabszám|Átlagos|A rövid szálkészlet elemzése hello hello sorban lévő feladatok száma.|
|memory_thrashing_metric|Memória Akadozások|Százalék|Átlagos|Átlagos memória thrashing.|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|SearchLatency|Keresési késés|másodperc|Átlagos|Átlagos keresési késés hello keresési szolgáltatáshoz|
|SearchQueriesPerSecond|Keresési lekérdezések száma másodpercenként|CountPerSecond|Átlagos|Keresési lekérdezések másodpercenként hello keresési szolgáltatáshoz|
|ThrottledSearchQueriesPercentage|Szabályozottan halmozott keresési lekérdezések százalékos aránya|Százalék|Átlagos|Hello keresési szolgáltatás volt halmozódni keresési lekérdezések aránya|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|CPUXNS|Egy névtér CPU-használat|Százalék|Maximális|Service bus prémium névtér CPU kihasználtsága metrika|
|WSXNS|Felhasznál memória mérete névterenként|Százalék|Maximális|Service bus prémium névtér memória kihasználtsága metrika|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|cpu_percent|Processzorhasználat (%)|Százalék|Átlagos|Processzorhasználat (%)|
|physical_data_read_percent|Adat IO kihasználtsága (%)|Százalék|Átlagos|Adat IO kihasználtsága (%)|
|log_write_percent|Napló IO százalékos aránya|Százalék|Átlagos|Napló IO százalékos aránya|
|dtu_consumption_percent|DTU-kihasználtság (%)|Százalék|Átlagos|DTU-kihasználtság (%)|
|Tárolás|Adatbázis teljes mérete|Bájtok|Maximális|Adatbázis teljes mérete|
|connection_successful|Sikeres kapcsolatok|Darabszám|Összes|Sikeres kapcsolatok|
|connection_failed|Nem sikerült kapcsolatok|Darabszám|Összes|Nem sikerült kapcsolatok|
|blocked_by_firewall|Tiltsa le tűzfal|Darabszám|Összes|Tiltsa le tűzfal|
|Holtpont|Holtpont|Darabszám|Összes|Holtpont|
|storage_percent|Adatbázis méretének kihasználtsága|Százalék|Maximális|Adatbázis méretének kihasználtsága|
|xtp_storage_percent|A memórián belüli online Tranzakciófeldolgozási tárolási százaléka|Százalék|Átlagos|A memórián belüli online Tranzakciófeldolgozási tárolási százaléka|
|workers_percent|Feldolgozók százalékos aránya|Százalék|Átlagos|Feldolgozók százalékos aránya|
|sessions_percent|Munkamenetek százalékos aránya|Százalék|Átlagos|Munkamenetek százalékos aránya|
|dtu_limit|DTU korlátot|Darabszám|Átlagos|DTU korlátot|
|dtu_used|Felhasznált DTU|Darabszám|Átlagos|Felhasznált DTU|
|dwu_limit|DWU korlátot|Darabszám|Maximális|DWU korlátot|
|dwu_consumption_percent|DWU százalékos aránya|Százalék|Maximális|DWU százalékos aránya|
|dwu_used|A DWU használt|Darabszám|Maximális|A DWU használt|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|cpu_percent|Processzorhasználat (%)|Százalék|Átlagos|Processzorhasználat (%)|
|database_cpu_percent|Processzorhasználat (%)|Százalék|Átlagos|Processzorhasználat (%)|
|physical_data_read_percent|Adat IO kihasználtsága (%)|Százalék|Átlagos|Adat IO kihasználtsága (%)|
|database_physical_data_read_percent|Adat IO kihasználtsága (%)|Százalék|Átlagos|Adat IO kihasználtsága (%)|
|log_write_percent|Napló IO százalékos aránya|Százalék|Átlagos|Napló IO százalékos aránya|
|database_log_write_percent|Napló IO százalékos aránya|Százalék|Átlagos|Napló IO százalékos aránya|
|dtu_consumption_percent|DTU-kihasználtság (%)|Százalék|Átlagos|DTU-kihasználtság (%)|
|database_dtu_consumption_percent|DTU-kihasználtság (%)|Százalék|Átlagos|DTU-kihasználtság (%)|
|storage_percent|Tárolási százalékos aránya|Százalék|Átlagos|Tárolási százalékos aránya|
|workers_percent|Feldolgozók százalékos aránya|Százalék|Átlagos|Feldolgozók százalékos aránya|
|database_workers_percent|Feldolgozók százalékos aránya|Százalék|Átlagos|Feldolgozók százalékos aránya|
|sessions_percent|Munkamenetek százalékos aránya|Százalék|Átlagos|Munkamenetek százalékos aránya|
|database_sessions_percent|Munkamenetek százalékos aránya|Százalék|Átlagos|Munkamenetek százalékos aránya|
|eDTU_limit|eDTU korlátot|Darabszám|Átlagos|eDTU korlátot|
|storage_limit|Tárolási kapacitása|Bájtok|Átlagos|Tárolási kapacitása|
|eDTU_used|felhasznált edtu-ra|Darabszám|Átlagos|felhasznált edtu-ra|
|storage_used|Felhasznált tárterület|Bájtok|Átlagos|Felhasznált tárterület|
|database_storage_used|Felhasznált tárterület|Bájtok|Átlagos|Felhasznált tárterület|
|xtp_storage_percent|A memórián belüli online Tranzakciófeldolgozási tárolási százaléka|Százalék|Átlagos|A memórián belüli online Tranzakciófeldolgozási tárolási százaléka|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|dtu_consumption_percent|DTU-kihasználtság (%)|Százalék|Átlagos|DTU-kihasználtság (%)|
|database_dtu_consumption_percent|DTU-kihasználtság (%)|Százalék|Átlagos|DTU-kihasználtság (%)|
|storage_used|Felhasznált tárterület|Bájtok|Átlagos|Felhasznált tárterület|
|database_storage_used|Felhasznált tárterület|Bájtok|Átlagos|Felhasznált tárterület|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|ResourceUtilization|SU kihasználtsága (%)|Százalék|Maximális|SU kihasználtsága (%)|
|InputEvents|A bemeneti események|Darabszám|Összes|A bemeneti események|
|InputEventBytes|A bemeneti esemény bájt|Bájtok|Összes|A bemeneti esemény bájt|
|LateInputEvents|A későn érkező bemeneti események|Darabszám|Összes|A későn érkező bemeneti események|
|OutputEvents|A kimeneti eseményekben|Darabszám|Összes|A kimeneti eseményekben|
|ConversionErrors|Adatok konvertálási hibák|Darabszám|Összes|Adatok konvertálási hibák|
|Hibák|Futásidejű hibák|Darabszám|Összes|Futásidejű hibák|
|DroppedOrAdjustedEvents|Üzemen kívüli események|Darabszám|Összes|Üzemen kívüli események|
|AMLCalloutRequests|Függvény kérelmek|Darabszám|Összes|Függvény kérelmek|
|AMLCalloutFailedRequests|Sikertelen függvény kérelmek|Darabszám|Összes|Sikertelen függvény kérelmek|
|AMLCalloutInputEvents|Függvény események|Darabszám|Összes|Függvény események|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|CpuPercentage|Processzorhasználat (%)|Százalék|Átlagos|Processzorhasználat (%)|
|MemoryPercentage|Memóriahasználat (%)|Százalék|Átlagos|Memóriahasználat (%)|
|DiskQueueLength|Lemezvárólista hossza|Darabszám|Összes|Lemezvárólista hossza|
|HttpQueueLength|HTTP-várólista hossza|Darabszám|Összes|HTTP-várólista hossza|
|BytesReceived|Az adatok|Bájtok|Összes|Az adatok|
|BytesSent|Kimenő adatforgalmat|Bájtok|Összes|Kimenő adatforgalmat|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (funkciókat kivéve)

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|CpuTime|CPU-idő|másodperc|Összes|CPU-idő|
|Kérelmek|Kérelmek|Darabszám|Összes|Kérelmek|
|BytesReceived|Az adatok|Bájtok|Összes|Az adatok|
|BytesSent|Kimenő adatforgalmat|Bájtok|Összes|Kimenő adatforgalmat|
|Http101|HTTP 101|Darabszám|Összes|HTTP 101|
|Http2xx|HTTP 2xx|Darabszám|Összes|HTTP 2xx|
|Http3xx|HTTP 3xx|Darabszám|Összes|HTTP 3xx|
|Http401|HTTP 401-es|Darabszám|Összes|HTTP 401-es|
|Http403|A HTTP 403|Darabszám|Összes|A HTTP 403|
|Http404|HTTP 404-es|Darabszám|Összes|HTTP 404-es|
|Http406|406 http|Darabszám|Összes|406 http|
|Http4xx|HTTP 4xx|Darabszám|Összes|HTTP 4xx|
|Http5xx|HTTP-kiszolgáló hibák|Darabszám|Összes|HTTP-kiszolgáló hibák|
|MemoryWorkingSet|Memória munkakészlete|Bájtok|Átlagos|Memória munkakészlete|
|AverageMemoryWorkingSet|Munkakészlet átlagos memória|Bájtok|Átlagos|Munkakészlet átlagos memória|
|AverageResponseTime|Átlagos válaszidő|másodperc|Átlagos|Átlagos válaszidő|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkciók)

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|BytesReceived|Az adatok|Bájtok|Összes|Az adatok|
|BytesSent|Kimenő adatforgalmat|Bájtok|Összes|Kimenő adatforgalmat|
|Http5xx|HTTP-kiszolgáló hibák|Darabszám|Összes|HTTP-kiszolgáló hibák|
|MemoryWorkingSet|Memória munkakészlete|Bájtok|Átlagos|Memória munkakészlete|
|AverageMemoryWorkingSet|Munkakészlet átlagos memória|Bájtok|Átlagos|Munkakészlet átlagos memória|
|FunctionExecutionUnits|Függvény végrehajtása egység|Darabszám|Átlagos|Függvény végrehajtása egység|
|FunctionExecutionCount|Függvény végrehajtási száma|Darabszám|Átlagos|Függvény végrehajtási száma|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrika|Metrika megjelenített neve|Unit (Egység)|Aggregáció típusa|Leírás|
|---|---|---|---|---|
|CpuTime|CPU-idő|másodperc|Összes|CPU-idő|
|Kérelmek|Kérelmek|Darabszám|Összes|Kérelmek|
|BytesReceived|Az adatok|Bájtok|Összes|Az adatok|
|BytesSent|Kimenő adatforgalmat|Bájtok|Összes|Kimenő adatforgalmat|
|Http101|HTTP 101|Darabszám|Összes|HTTP 101|
|Http2xx|HTTP 2xx|Darabszám|Összes|HTTP 2xx|
|Http3xx|HTTP 3xx|Darabszám|Összes|HTTP 3xx|
|Http401|HTTP 401-es|Darabszám|Összes|HTTP 401-es|
|Http403|A HTTP 403|Darabszám|Összes|A HTTP 403|
|Http404|HTTP 404-es|Darabszám|Összes|HTTP 404-es|
|Http406|406 http|Darabszám|Összes|406 http|
|Http4xx|HTTP 4xx|Darabszám|Összes|HTTP 4xx|
|Http5xx|HTTP-kiszolgáló hibák|Darabszám|Összes|HTTP-kiszolgáló hibák|
|MemoryWorkingSet|Memória munkakészlete|Bájtok|Átlagos|Memória munkakészlete|
|AverageMemoryWorkingSet|Munkakészlet átlagos memória|Bájtok|Átlagos|Munkakészlet átlagos memória|
|AverageResponseTime|Átlagos válaszidő|másodperc|Átlagos|Átlagos válaszidő|
|FunctionExecutionUnits|Függvény végrehajtása egység|Darabszám|Átlagos|Függvény végrehajtása egység|
|FunctionExecutionCount|Függvény végrehajtási száma|Darabszám|Átlagos|Függvény végrehajtási száma|

## <a name="next-steps"></a>Következő lépések
* [Olvassa el a Azure figyelő metrikák](monitoring-overview-metrics.md)
* [Hozzon létre a riasztások mérőszámokat](insights-receive-alert-notifications.md)
* [Metrikák toostorage, az Event Hubs vagy Naplóelemzési exportálása](monitoring-overview-of-diagnostic-logs.md)
