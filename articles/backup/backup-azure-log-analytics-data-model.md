---
title: "aaaLog Analytics adatmodell Azure biztonsági mentés"
description: "Ez a cikk beszél Naplóelemzési az modell adatait az Azure biztonságimásolat-adatok."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: dfd5c73d-0d34-4d48-959e-1936986f9fc0
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 04ac16e38b896851f60b1c4ffbea4343347ae32c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Napló Analytics adatmodell Azure biztonságimásolat-adatok
Ez a cikk ismerteti a tárházat a jelentési adatok tooLog Analytics használt hello adatmodell. A adatok modell segítségével hozhat létre egyéni lekérdezéseket, irányítópultok, és az OMS felhasználható az. 

## <a name="using-azure-backup-data-model"></a>Azure biztonsági mentési adatok modell segítségével
A következő mezők hello adatok modell toocreate látványelemek, egyéni lekérdezések és a követelményeknek irányítópult részeként hello is használhatja.

### <a name="alert"></a>Riasztás
Ez a táblázat ismerteti a riasztási kapcsolódó mezők.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| AlertUniqueId_s |Szöveg |Riasztást hello egyedi azonosítója |
| AlertType_s |Szöveg |Hello típusú riasztást, például biztonsági mentése |
| AlertStatus_s |Szöveg |Hello riasztást, például aktív állapota |
| AlertOccurenceDateTime_s |Dátum és idő |Dátum és a riasztás létrehozásának időpontja |
| AlertSeverity_s |Szöveg |Hello riasztás súlyossága, például kritikus |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| BackupItemUniqueId_s |Szöveg |Ez a riasztás túl tartozik hello biztonsági mentési elem toowhich egyedi azonosítója|
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello riasztási objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM, ez a riasztás túl tartozik FileFolder toowhich szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - riasztás neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| ProtectedServerUniqueId_s |Szöveg |Hello egyedi azonosítóját védett toowhich a riasztás túl tartozik|
| VaultUniqueId_s |Szöveg |Hello egyedi azonosítóját védett toowhich a riasztás túl tartozik|
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="backupitem"></a>BackupItem
Ez a táblázat ismerteti a biztonsági mentési elem kapcsolatos mezők.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |  
| BackupItemUniqueId_s |Szöveg |Hello biztonsági mentési elem egyedi azonosítója |
| BackupItemId_s |Szöveg |Biztonsági mentési elem azonosítója |
| BackupItemName_s |Szöveg |Biztonsági mentési elem neve |
| BackupItemFriendlyName_s |Szöveg |Biztonsági mentési elem rövid neve |
| BackupItemType_s |Szöveg |Biztonsági mentési elem, például a virtuális gép, FileFolder típusa |
| ProtectedServerName_s |Szöveg |Védett kiszolgáló toowhich biztonsági mentési elem neve túl tartozik|
| ProtectionState_s |Szöveg |Hello biztonsági mentési elem, például a védett, a ProtectionStopped aktuális védelmi állapot |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello biztonsági mentési elem objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM, a biztonsági mentési cikkhez tartozó túl FileFolder toowhich szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - BackupItem neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="backupitemassociation"></a>BackupItemAssociation
A táblázat ismerteti a különböző entitásokkal biztonsági mentési elem társításokat.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |  
| BackupItemUniqueId_s |Szöveg |Hello biztonsági mentési elem egyedi azonosítója |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello biztonsági mentési elem társítás objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM, a biztonsági mentési cikkhez tartozó túl FileFolder toowhich szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - BackupItemAssociation neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| PolicyUniqueId_g |Szöveg |Egyedi azonosító tooidentify hello házirend, mely biztonsági mentési elem túl társítva|
| ProtectedServerUniqueId_s |Szöveg |Hello egyedi azonosítóját védett kiszolgáló toowhich a biztonsági mentési cikkhez tartozó túl|
| VaultUniqueId_s |Szöveg |A biztonsági mentési cikkhez tartozó túl hello tároló toowhich egyedi azonosítója|
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="job"></a>Feladat
Ez a táblázat ismerteti a feladathoz kapcsolódó mezőket.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| BackupItemUniqueId_s |Szöveg |Ez a feladat tartozik túl hello biztonsági mentési elem toowhich egyedi azonosítója|
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello feladat objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM FileFolder toowhich túl tartozik, ez a feladat végrehajtásához szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - név feladat |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| ProtectedServerUniqueId_s |Szöveg |Hello egyedi azonosítóját védett toowhich túl tartozik, ez a feladat|
| VaultUniqueId_s |Szöveg |Hello egyedi azonosítóját védett toowhich túl tartozik, ez a feladat|
| JobOperation_s |Szöveg |A művelet, amelynek feladat futtatásakor például biztonsági mentése, visszaállítása, Backup konfigurálása |
| JobStatus_s |Szöveg |Hello állapotának befejeződött feladatok, például befejezve, sikertelen |
| JobFailureCode_s |Szöveg |Hiba a kód karakterlánc miatt, amelyet feladat hiba történt |
| JobStartDateTime_s |Dátum és idő |Dátum és idő elindított futó feladat |
| BackupStorageDestination_s |Szöveg |Biztonsági másolatok tárolásának, például felhő, lemez és  |
| JobDurationInSecs_s | Szám |Feladatok teljes időtartama másodpercben |
| DataTransferredInMB_s | Szám |A feladat MB-ban átvitt adatok|
| JobUniqueId_g |Szöveg |Egyedi azonosító tooidentify hello feladat |
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="policy"></a>Szabályzat
Ez a táblázat-házirendekkel kapcsolatos mezők ismerteti.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello házirend objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM FileFolder toowhich túl tartozik, ezzel a házirend-szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - házirend neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| PolicyUniqueId_g |Szöveg |Egyedi azonosító tooidentify hello házirend |
| PolicyName_s |Szöveg |Meghatározott szabályzattal hello neve |
| BackupFrequency_s |Szöveg |A gyakoriság, amellyel biztonsági mentések futnak, például, naponta, hetente |
| BackupTimes_s |Szöveg |Dátum és idő, amikor a biztonsági mentés van ütemezve |
| BackupDaysOfTheWeek_s |Szöveg |Ha biztonsági mentések ütemezett hello hét napjai |
| RetentionDuration_s |Egész szám |Konfigurált biztonsági mentések megőrzési időtartama |
| DailyRetentionDuration_s |Egész szám |Teljes megőrzés időtartama napokban megadva beállított biztonsági mentés |
| DailyRetentionTimes_s |Szöveg |Dátum és idő, amikor napi megőrzési konfigurálása |
| WeeklyRetentionDuration_s |Egész szám |A konfigurált biztonsági mentésekhez hét teljes heti megőrzési időtartama |
| WeeklyRetentionTimes_s |Szöveg |Dátum és idő, ha a heti megőrzési van konfigurálva |
| WeeklyRetentionDaysOfTheWeek_s |Szöveg |A heti megőrzési kijelölt hello hét napjai |
| MonthlyRetentionDuration_s |Egész szám |A konfigurált biztonsági mentésekhez hónapban teljes megőrzési időtartama |
| MonthlyRetentionTimes_s |Szöveg |Dátum és idő, amikor a havi megőrzési van konfigurálva |
| MonthlyRetentionFormat_s |Szöveg |Havi megőrzési, például meghatározásával, hetente alapján hét nap napi konfigurációjának típusa |
| MonthlyRetentionDaysOfTheWeek_s |Szöveg |A havi megőrzési kijelölt hello hét napjai |
| MonthlyRetentionWeeksOfTheMonth_s |Szöveg |Ha havi megőrzési van konfigurálva, hello hónap hét például First, Last stb. |
| YearlyRetentionDuration_s |Egész szám |A konfigurált biztonsági mentésekhez évben teljes megőrzési időtartama |
| YearlyRetentionTimes_s |Szöveg |Dátum és idő, amikor éves megőrzési van konfigurálva |
| YearlyRetentionMonthsOfTheYear_s |Szöveg |Éves megőrzési kiválasztott hello év hónapja |
| YearlyRetentionFormat_s |Szöveg |A konfigurációban az éves megőrzési, például napi meghatározásával, hetente alapján hét nap |
| YearlyRetentionDaysOfTheMonth_s |Szöveg |Hello hónap éves megőrzési a kijelölt dátumok |
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="policyassociation"></a>PolicyAssociation
Ez a táblázat ismerteti a különböző entitásokkal házirendtársításait részleteit.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello házirend objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés például IaaSVM, ez a házirend tartozik túl FileFolder toowhich szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - PolicyAssociation neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| PolicyUniqueId_g |Szöveg |Egyedi azonosító tooidentify hello házirend |
| VaultUniqueId_s |Szöveg |Ezzel a házirend tartozik túl hello tároló toowhich egyedi azonosítója|
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="protectedserver"></a>ProtectedServer
Ez a táblázat ismerteti a védett kiszolgálóval kapcsolatos mezők.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| ProtectedServerName_s |Szöveg |Védett kiszolgáló neve |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello aktuális állapotának védett server objektum, például az aktív, a törölt |
| BackupManagementType_s |Szöveg |A biztonsági mentés például IaaSVM, a védett kiszolgáló tartozik túl FileFolder toowhich szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - ProtectedServer neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| ProtectedServerUniqueId_s |Szöveg |A védett kiszolgáló hello egyedi azonosítója |
| RegisteredContainerId_s |Szöveg |A biztonsági mentéshez regisztrált tároló azonosító |
| ProtectedServerType_s |Szöveg |Például Windows mentésbe a védett kiszolgáló típusa |
| ProtectedServerFriendlyName_s |Szöveg |Védett kiszolgáló rövid neve |
| AzureBackupAgentVersion_s |Szöveg |Biztonsági másolat verzióját ügynök verziószáma |
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
A táblázat tartalmazza a védett kiszolgáló társítások más entitásokkal adatait.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello aktuális állapotának védett kiszolgáló társítás objektum, például az aktív, a törölt |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM, a védett kiszolgáló tartozik túl FileFolder toowhich szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - ProtectedServerAssociation neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| ProtectedServerUniqueId_s |Szöveg |A védett kiszolgáló hello egyedi azonosítója |
| VaultUniqueId_s |Szöveg |A védett kiszolgáló tartozik túl hello tároló toowhich egyedi azonosítója|
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

### <a name="storage"></a>Storage
Ez a táblázat ismerteti a tárolással kapcsolatos mezők.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| CloudStorageInBytes_s |Egész szám |Biztonsági mentések, számított által használt biztonsági mentési felhőtárhelyére legújabb érték alapján. |
| ProtectedInstances_s |Egész szám |Előtérbeli tárolási számlázási, számított alapján legújabb értékének kiszámítására használt védett példányok száma |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello tárolási objektum, például az aktív, a törölt aktuális állapota |
| BackupManagementType_s |Szöveg |A biztonsági mentés, például IaaSVM FileFolder toowhich túl tartozik a tárolási szolgáltató típusa|
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - tároló neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| ProtectedServerUniqueId_s |Szöveg |Hello egyedi azonosítóját védett kiszolgáló, amelynek tároló kiszámítása |
| VaultUniqueId_s |Szöveg |Egyedi azonosító hello tároló tárolási kiszámítása |
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező representse hello erőforrás, amelynek folyik adatgyűjtés - tárolók típusa |

### <a name="vault"></a>Tároló
Ez a táblázat ismerteti a tárolóval kapcsolatos mezők.

| Mező | Adattípus | Leírás |
| --- | --- | --- |
| EventName_s |Szöveg |Ez a mező képviseli az esemény nevét, a rendszer mindig AzureBackupCentralReport |
| SchemaVersion_s |Szöveg |Ebben a mezőben hello séma jelenlegi verziója azt jelzi, hogy az **1-es verzió** |
| State_s |Szöveg |Hello tároló objektum, például az aktív, a törölt aktuális állapota |
| OperationName |Szöveg |Ez a mező képviseli az aktuális művelet hello - tároló neve |
| Kategória |Szöveg |Ez a mező képviseli kategória tooLog Analytics leküldött diagnosztikai adatok, a AzureBackupReport |
| Erőforrás |Szöveg |Hello erőforrás, amelynek gyűjtenek adatokat, azt mutatja, hogy Recovery Services-tároló neve |
| VaultUniqueId_s |Szöveg |Hello tároló egyedi azonosítója |
| VaultName_s |Szöveg |Hello tároló neve |
| AzureDataCenter_s |Szöveg |Az Adatközpont, ahol a tárolóban |
| StorageReplicationType_s |Szöveg |Hello tároló, például GeoRedundant tárolóreplikálást típusa |
| SourceSystem |Szöveg |A forrásrendszerben hello aktuális adatok – Azure |
| ResourceId |Szöveg |Ez a mező jelöli erőforrás-azonosító, amelynek gyűjtenek adatokat, azt illusztrálja a Recovery Services-tároló erőforrás-azonosító |
| SubscriptionId |Szöveg |Ez a mező képviseli az előfizetés-azonosító hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceGroup |Szöveg |Ez a mező képviseli az erőforráscsoport hello erőforrás (RS tároló), amelynek gyűjtenek adatokat |
| ResourceProvider |Szöveg |Ez a mező képviseli hello erőforrás-szolgáltató, amelynek folyik adatgyűjtés - Microsoft.RecoveryServices |
| ResourceType |Szöveg |Ez a mező képviseli az hello erőforrástípust, amelynek folyik adatgyűjtés - tárolók |

## <a name="next-steps"></a>Következő lépések
Miután adatmodell hello Azure biztonsági mentés jelentések létrehozásához tekintse át, elindíthatja [irányítópult létrehozása](../log-analytics/log-analytics-dashboards.md) Naplóelemzés és az OMS Szolgáltatáshoz.
