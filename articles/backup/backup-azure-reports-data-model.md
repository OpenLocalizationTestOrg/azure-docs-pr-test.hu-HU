---
title: az Azure Backup aaaData modell
description: "Ez a cikk a Power bi-ban az modell adatait az Azure Backup jelentések beszél."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 0767c330-690d-474d-85a6-aa8ddc410bb2
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/26/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e3e7ca13c7a3f007c206bd56b8753166a2c264b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-model-for-azure-backup-reports"></a>Adatmodell az Azure Backup-jelentésekhez
Ez a cikk ismerteti az Azure Backup-jelentések készítéséhez használt hello Power BI adatmodell. A adatok modell segítségével meglévő jelentéseket szűrheti a megfelelő mezőket alapján, és több táblákat és a mezőket hello modell használatával is fontosabb, létrehozhatja a saját jelentéseit. 

## <a name="creating-new-reports-in-power-bi"></a>Új jelentések létrehozását a Power bi-ban
A Power BI használatával, amely is testreszabási funkciókat is biztosítanak [hello adatmodell használatával jelentéseket készíthet](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Azure biztonsági mentési adatok modell segítségével
A következő megadott hello adatok egy részét a modell toocreate jelentések és a meglévő jelentések testreszabása mezők hello is használhatja.

### <a name="alert"></a>Riasztás
Ez a táblázat alapvető mezők és az aggregációhoz biztosítanak riasztási kapcsolódó mezőket.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Egész szám |Kiválasztott időszak létrehozott riasztások számát |
| % ActiveAlertsCreatedInPeriod |Százalékos aránya |Aktív riasztások a kiválasztott időszak aránya |
| % CriticalAlertsCreatedInPeriod |Százalékos aránya |A kijelölt időszakra vonatkozó kritikus riasztás százalékos aránya |
| AlertOccurenceDate |Dátum |Riasztás létrehozásának dátuma |
| AlertSeverity |Szöveg |Ha például kritikus hello riasztás súlyossága |
| AlertStatus |Szöveg |Hello riasztás például aktív állapota |
| AlertType |Szöveg |Hello típusú riasztást, például a biztonsági mentés |
| AlertUniqueId |Szöveg |Riasztást hello egyedi azonosítója |
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Egész szám |Kiválasztott időszak átlagos idő (percben) tooresolve riasztás |
| EntityState |Szöveg |Hello riasztási objektum aktív, a törölt például aktuális állapota |

### <a name="backup-item"></a>Biztonsági mentési elem
Ez a táblázat különböző biztonsági mentési elem kapcsolatos mezők alapvető mezők és az aggregációhoz biztosítanak.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #BackupItems |Egész szám |Biztonsági mentési elemek száma. |
| #UnprotectedBackupItems |Egész szám |A védelem leállt, vagy biztonsági mentések, de a biztonsági mentések nem indult el a beállított biztonsági mentési elemek száma.|
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| BackupItemFriendlyName |Szöveg |Biztonsági mentési elem rövid neve |
| BackupItemId |Szöveg |Biztonsági mentési elem azonosítója |
| BackupItemName |Szöveg |Biztonsági mentési elem neve |
| BackupItemType |Szöveg |Biztonsági mentési elem például VM FileFolder típusa |
| EntityState |Szöveg |Aktív, a törölt például hello biztonsági mentési elem objektum aktuális állapota |
| LastBackupDateTime |Dátum és idő |Utolsó biztonsági mentés idején a kijelölt elem biztonsági mentése |
| LastBackupState |Szöveg |A kijelölt elem biztonsági mentése sikeres, sikertelen például utolsó biztonsági mentés állapota |
| LastSuccessfulBackupDateTime |Dátum és idő |Utolsó sikeres biztonsági másolat készítésének idején a kijelölt elem biztonsági mentése |
| ProtectionState |Szöveg |Hello biztonsági mentési elem védett, ProtectionStopped például aktuális védelmi állapotának |

### <a name="calendar"></a>Naptár
Ez a táblázat ismerteti a naptár kapcsolatos mezők.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| Dátum |Dátum |Az adatok szűrése kiválasztott dátum |
| DateKey |Szöveg |Minden dátum elem egyedi kulcsa |
| DayDiff |Egész szám |Például a nap-szűrési adatok különbség, 0 azt jelzi, az aktuális napra vonatkozó adatokat, -1 érték azt jelzi, előző egy nap adatait, 0 és a -1 jelzik adatok jelenlegi és korábbi nap  |
| Hónap |Szöveg |Hónap kijelölt adatok szűréséhez hello év, hónap első napján kezdődik, és 31 napos ér véget |
| MonthDate | Dátum |Dátum hello hónap, amikor a hónap ér véget, a kiválasztott adatok szűrése |
| MonthDiff |Egész szám |Például a szűrési adatok hónap különbség, 0 jelzi az aktuális hónap adatait, -1 érték azt jelzi, az előző hónap adatok, 0 és a -1 jelzik adatok jelenlegi és előző hónap |
| Hét |Szöveg |A hét kijelölt adatok, szűrési hét kezdőnapja vasárnap és szombat vége |
| WeekDate |Dátum |Dátum hello hét amikor hét ér véget, a kiválasztott adatok szűrése |
| WeekDiff |Egész szám |Például a szűrési adatok hét különbség, 0 azt jelzi, aktuális hét adatok, -1 érték azt jelzi, előző hét adatok, 0 és a -1 jelzik adatok aktuális és az előző hét |
| Év |Szöveg |Kijelölt adatok szűréséhez naptári év |
| YearDate |Dátum |Dátum hello év, amikor év ér véget, a kiválasztott adatok szűrése |

### <a name="job"></a>Feladat
Ez a táblázat alapvető mezők és az aggregációhoz biztosítanak feladathoz kapcsolódó mezőket.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #JobsCreatedInPeriod |Egész szám |A kijelölt időszakban hello létrehozott feladatok száma |
| % FailuresForJobsCreatedInPeriod |Százalékos aránya |Százalék a teljes feladat-a kijelölt időszakban hello hibák |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Egész szám |MB az átvitt adatok 80 PERCENTILIS **biztonsági mentési** a kijelölt időszakban hello létrehozott feladatok |
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Egész szám |Átlagos idő (percben) a **befejezett biztonsági mentés** kijelölt időszakban létrehozott feladatok |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Egész szám |Átlagos idő (percben) a **visszaállítás befejeződött** kijelölt időszakban létrehozott feladatok |
| BackupStorageDestination |Szöveg |Biztonsági másolatok tárolásának például felhő, lemez és  |
| EntityState |Szöveg |Aktív, a törölt például hello feladat objektum aktuális állapota |
| JobFailureCode |Szöveg |Hiba a kód karakterlánc miatt, amelyet feladat hiba történt |
| JobOperation |Szöveg |A művelet, amelynek feladat futtatásakor például biztonsági mentése, visszaállítása, Backup konfigurálása |
| JobStartDate |Dátum |Feladat elindításának dátuma |
| JobStartTime |Time |Amikor a feladat elindítása fut |
| Feladat állapota |Szöveg |Hello állapotának befejeződött feladat például befejeződött, nem sikerült |
| JobUniqueId |Szöveg |Egyedi azonosító tooidentify hello feladat |

### <a name="policy"></a>Szabályzat
Ez a táblázat alapvető mezők és az aggregációhoz biztosítanak-házirendekkel kapcsolatos mezőket.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #Policies |Egész szám |Hello rendszerben létező biztonsági mentési házirendek száma |
| #PoliciesInUse |Egész szám |A biztonsági mentések beállítása jelenleg használt házirendek száma |
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| BackupDaysOfTheWeek |Szöveg |Ha biztonsági mentések ütemezett hello hét napjai |
| BackupFrequency |Szöveg |A gyakoriság, amellyel biztonsági mentések futnak, például, naponta, hetente |
| BackupTimes |Szöveg |Dátum és idő, amikor a biztonsági mentés van ütemezve |
| DailyRetentionDuration |Egész szám |Teljes megőrzés időtartama napokban megadva beállított biztonsági mentés |
| DailyRetentionTimes |Szöveg |Dátum és idő, amikor napi megőrzési konfigurálása |
| EntityState |Szöveg |Aktív, a törölt például hello csoportházirend-objektum aktuális állapota |
| MonthlyRetentionDaysOfTheMonth |Szöveg |Hello hónap havi megőrzési a kijelölt dátumok |
| MonthlyRetentionDaysOfTheWeek |Szöveg |A havi megőrzési kijelölt hello hét napjai |
| MonthlyRetentionDuration |Egész szám |A konfigurált biztonsági mentésekhez hónapban teljes megőrzési időtartama |
| MonthlyRetentionFormat |Szöveg |Írja be a havi megőrzési konfiguráció például naponta meghatározásával, hetente alapján hét nap |
| MonthlyRetentionTimes |Szöveg |Dátum és idő, amikor a havi megőrzési van konfigurálva |
| MonthlyRetentionWeeksOfTheMonth |Szöveg |Havi megőrzési esetén hello hónap hét konfigurált például First, Last stb. |
| Házirendnév |Szöveg |Meghatározott szabályzattal hello neve |
| PolicyUniqueId |Szöveg |Egyedi azonosító tooidentify hello házirend |
| RetentionType |Szöveg |Írja be a megőrzési házirend például, naponta, hetente, havonta, éves ütemezéshez |
| WeeklyRetentionDaysOfTheWeek |Szöveg |A heti megőrzési kijelölt hello hét napjai |
| WeeklyRetentionDuration |Egész szám |A konfigurált biztonsági mentésekhez hét teljes heti megőrzési időtartama |
| WeeklyRetentionTimes |Szöveg |Dátum és idő, ha a heti megőrzési van konfigurálva |
| YearlyRetentionDaysOfTheMonth |Szöveg |Hello hónap éves megőrzési a kijelölt dátumok |
| YearlyRetentionDaysOfTheWeek |Szöveg |Éves megőrzési kiválasztott hello hét napjai |
| YearlyRetentionDuration |Egész szám |A konfigurált biztonsági mentésekhez évben teljes megőrzési időtartama |
| YearlyRetentionFormat |Szöveg |Írja be az éves megőrzési konfiguráció például naponta meghatározásával, hetente alapján hét nap |
| YearlyRetentionMonthsOfTheYear |Szöveg |Éves megőrzési kiválasztott hello év hónapja |
| YearlyRetentionTimes |Szöveg |Dátum és idő, amikor éves megőrzési van konfigurálva |
| YearlyRetentionWeeksOfTheMonth |Szöveg |Éves megőrzési esetén hello hónap hét konfigurált például First, Last stb. |

### <a name="protected-server"></a>Védett kiszolgáló
Ez a táblázat különböző védett kiszolgálóval kapcsolatos mezők alapvető mezők és az aggregációhoz biztosítanak.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #ProtectedServers |Egész szám |Védett kiszolgálók száma |
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| AzureBackupAgentOSType |Szöveg |Az Azure Backup szolgáltatás ügynöke operációsrendszer-típus |
| AzureBackupAgentOSVersion |Szöveg |Az Azure Backup szolgáltatás ügynöke az operációs rendszer verziója |
| AzureBackupAgentUpdateDate |Szöveg |Ügynök Backup szolgáltatás ügynökének frissítésének dátuma |
| AzureBackupAgentVersion |Szöveg |Biztonsági másolat verzióját ügynök verziószáma |
| BackupManagementType |Szöveg |A biztonsági mentés például IaaSVM FileFolder szolgáltató típusa |
| EntityState |Szöveg |Aktív, a törölt például hello védett kiszolgáló objektum aktuális állapota |
| ProtectedServerFriendlyName |Szöveg |Védett kiszolgáló rövid neve |
| ProtectedServerName |Szöveg |Védett kiszolgáló neve |
| ProtectedServerType |Szöveg |A védett kiszolgáló típusa biztonsági mentése például IaaSVMContainer |
| ProtectedServerName |Szöveg |Védett kiszolgáló toowhich biztonsági mentési elem neve tartozik |
| RegisteredContainerId |Szöveg |A biztonsági mentéshez regisztrált tároló azonosító |

### <a name="storage"></a>Storage
Ez a táblázat alapvető mezők és az aggregációhoz biztosítanak tárolással kapcsolatos mezőket.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #ProtectedInstances |Egész szám |A kijelölt időszak előtér tárolási számlázási, számított alapján legújabb értékének kiszámítására használt védett példányok száma |
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| CloudStorageInMB |Egész szám |A kijelölt időszak legújabb érték alapján felhő biztonsági mentések, számított használt biztonsági mentési tároló |
| EntityState |Szöveg |Aktív, a törölt például hello objektum aktuális állapota |
| LastUpdatedDate |Dátum |Ha a kijelölt sor utolsó módosításának dátuma |

### <a name="time"></a>Time
Ez a táblázat idővel kapcsolatos mezők ismerteti.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| Óra |Time |Például 1:00:00 PM hello nap órája |
| HourNumber |Egész szám |Hello nap például 13,00 óra száma |
| Perc |Egész szám |Hello óra perc |
| PeriodOfTheDay |Szöveg |Period időszelet hello nap például a 12-3 óra |
| Time |Time |Időpontot hello például 12:00:01-kor |
| TimeKey |Szöveg |Kulcs értéke toorepresent idő |

### <a name="vault"></a>Tároló
Ez a táblázat különböző tárolóval kapcsolatos mezők alapvető mezők és az aggregációhoz biztosítanak.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| #Vaults |Egész szám |Tárolók száma |
| AsOnDateTime |Dátum és idő |Legutóbbi frissítés hello kijelölt sor ideje |
| AzureDataCenter |Szöveg |Az Adatközpont, ahol a tárolóban |
| EntityState |Szöveg |Aktív, a törölt például hello tároló objektum aktuális állapota |
| StorageReplicationType |Szöveg |Hello tároló például GeoRedundant tárolóreplikálást típusa |
| SubscriptionId |Szöveg |Hello ügyfél-jelentések létrehozása a kijelölt előfizetés-azonosító |
| VaultName |Szöveg |Hello tároló neve |
| VaultTags |Szöveg |Címkék társított toohello tároló |

## <a name="next-steps"></a>Következő lépések
Miután adatmodell hello Azure biztonsági mentés jelentések létrehozásához tekintse át, tekintse meg a cikkek létrehozásához és jelentések megtekintése a Power BI további információt a következő hello.

* [Jelentések létrehozása a Power bi-ban](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Szűrés a jelentéseket a Power bi-ban](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
