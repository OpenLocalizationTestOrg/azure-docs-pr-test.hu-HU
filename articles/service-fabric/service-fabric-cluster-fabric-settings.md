---
title: "Azure Service Fabric-fürt beállítások aaaChange |} Microsoft Docs"
description: "Ez a cikk ismerteti a hello hálóbeállításokat hello háló frissítési házirendek és testre szabhatja."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: chackdan
ms.openlocfilehash: a8b125f7b4a02547cf4bcf2c936d0c7f1686c1a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>A Service Fabric-fürt beállításait és a háló házirend testreszabása
Ez a dokumentum azt ismerteti, hogyan toocustomize hello különböző hálóbeállításokat és hello háló házirend a Service Fabric-fürt számára. Testre szabhatja őket a hello portálon vagy az Azure Resource Manager-sablonnal.

> [!NOTE]
> Nem minden beállítás hello portálon keresztül érhetők el. Amennyiben a lenti beállítás nem érhető el hello portálon testreszabása az Azure Resource Manager-sablonnal.
> 

## <a name="customizing-service-fabric-cluster-settings-using-azure-resource-manager-templates"></a>Service Fabric-fürt beállításainak Azure Resource Manager-sablonok használatával
az alábbi hello lépések bemutatják, hogyan tooadd egy új beállítás *MaxDiskQuotaInMB* toohello *diagnosztika* szakasz.

1. Nyissa meg toohttps://resources.azure.com
2. Keresse meg a tooyour előfizetés kibontásával előfizetések csoportok -> erőforrás -> Microsoft.ServiceFabric -> saját fürt neve
3. A hello jobb felső sarokban válassza ki a "Olvasási/írási"
4. Szerkesztés kiválasztására és frissítésére hello `fabricSettings` JSON elem és az új elem hozzáadása

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

## <a name="fabric-settings-that-you-can-customize"></a>Testre szabható hálóbeállításokat
Az alábbiakban a testre szabható hálóbeállításokat hello:

### <a name="section-name-diagnostics"></a>Szakasz Name: diagnosztika
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| ConsumerInstances |Karakterlánc |hello DCA fogyasztói példányainak listáját. |
| ProducerInstances |Karakterlánc |hello DCA készítő példányainak listáját. |
| AppEtwTraceDeletionAgeInDays |Int, alapértelmezett érték 3 |Nap elteltével nem törölni alkalmazás ETW-nyomkövetési adatokat tartalmazó régi ETL-fájlok száma. |
| AppDiagnosticStoreAccessRequiresImpersonation |Logikai érték, az alapértelmezett érték true |E megszemélyesítési, ha nevében hello alkalmazás elérése diagnosztikai tárolja. |
| MaxDiskQuotaInMB |Int, alapértelmezett érték 65536 |Lemezkvóta Windows Fabric MB a rendszernapló fájljaiban. |
| DiskFullSafetySpaceInMB |Int, alapértelmezett érték: 1024 |MB tooprotect a DCA által használható fennmaradó szabad lemezterület. |
| ApplicationLogsFormatVersion |Int, alapértelmezett érték a 0 |Alkalmazás verziója formátum naplózza. Támogatott értékek: 0 és 1. 1-es verziójú tartalmaz hello ETW-esemény rekord verzió 0-nál több mezőket. |
| ClusterId |Karakterlánc |hello hello fürt egyedi azonosítója. Ez jön létre, amikor hello fürt létrehozását. |
| EnableTelemetry |Logikai érték, az alapértelmezett érték true |Ez tooenable lesz, vagy tiltsa le a telemetriai adatokat. |
| EnableCircularTraceSession |Logikai érték, alapértelmezett értéke "false" |A jelző azt jelzi, hogy használják-e körkörös nyomkövetési munkamenet. |

### <a name="section-name-traceetw"></a>Szakasz nevét: Nyomkövetési/Etw
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Szint |Int, alapértelmezett érték 4 |Nyomkövetési etw szint is igénybe vehet az érték 1, 2, 3, 4. támogatott toobe 4 kell őrizniük hello nyomkövetési szint |

### <a name="section-name-performancecounterlocalstore"></a>Szakasz Name: PerformanceCounterLocalStore
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| IsEnabled |Logikai érték, az alapértelmezett érték true |A jelző azt jelzi, hogy engedélyezve van-e a teljesítményszámlálók gyűjteményét a helyi csomóponton. |
| SamplingIntervalInSeconds |Int, alapértelmezett érték 60 |Mintavételi időköz gyűjtött teljesítményszámlálók. |
| Számlálók |Karakterlánc |Teljesítmény számlálók toocollect vesszővel tagolt listája. |
| MaxCounterBinaryFileSizeInMB |Int, alapértelmezett érték 1 |Maximális mérete (MB) a teljesítmény számláló bináris fájl. |
| NewCounterBinaryFileCreationIntervalInMinutes |Int, alapértelmezett érték 10 |Maximális korlát (másodperc) után, amely egy új teljesítmény számláló bináris fájl jön létre. |

### <a name="section-name-setup"></a>Szakasz Name: a telepítő
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| FabricDataRoot |Karakterlánc |A Service Fabric-adatok gyökérkönyvtár. Alapértelmezett Azure d:\svcfab: |
| Fabriclogroot mappában |Karakterlánc |Service fabric napló gyökérkönyvtár. Ez azért, ahol kerülnek ú naplók és a nyomkövetési adatokat. |
| ServiceRunAsAccountName |Karakterlánc |a fióknév hello mely toorun a fabric host szolgáltatás alatt. |
| ServiceStartupType |Karakterlánc |hello hello a fabric host szolgáltatás indítási típusa. |
| SkipFirewallConfiguration |Logikai érték, alapértelmezett értéke "false" |Itt adhatja meg, ha a tűzfal beállításainak toobe kell-e hello rendszer állítja be. Ez vonatkozik, csak akkor, ha a windows tűzfalat használja. Ha külső gyártótól származó tűzfalak használ, majd meg kell nyitnia hello rendszer és alkalmazások toouse hello portok |

### <a name="section-name-transactionalreplicator"></a>Szakasz Name: TransactionalReplicator
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| MaxCopyQueueSize |Uint, alapértelmezett érték 16384 |Ez az hello maximális érték hello kezdeti replikálási műveletek fenntartó hello várólista határozza meg. Vegye figyelembe, hogy 2 szintűnek kell lennie. Ha futásidőben hello várólista növekedésének toothis mérete műveletek közötti hello elsődleges és másodlagos gyártóitól halmozódni fog. |
| BatchAcknowledgementInterval | Idő (másodpercben), az alapértelmezett érték 0.015 | Adja meg az időtartam másodpercben. Meghatározza, hogy hello, hogy mennyi ideig hello replikátor vár a művelet előtt küld vissza nyugtázást fogadása után. Más műveletek kapott ebben az időszakban a nyugtázásokhoz egyetlen üzenetet küld csökkentése hálózati forgalmat, de a potenciálisan csökkentése hello átviteli sebességének hello replikátor -> fog rendelkezni. |
| (Maxreplicationmessagesize). |Uint, alapértelmezett érték 52428800 | Replikációs műveletek maximális méretét. Az alapértelmezett érték 50MB. |
| ReplicatorAddress |karakterlánc, alapértelmezett értéke "localhost:0" | hello végpont egy karakterlánc-"IP:Port" hello Windows Fabric replikátor tooestablish-kapcsolatok a rendelés toosend/fogadási műveletek más replikák által használt formában. |
| InitialPrimaryReplicationQueueSize |Uint, alapértelmezett érték 64 | Ez az érték hello várólista hello replikációs műveletek hello elsődleges fenntartó hello kezdeti mérete határozza meg. Vegye figyelembe, hogy 2 szintűnek kell lennie.|
| MaxPrimaryReplicationQueueSize |Uint, alapértelmezett érték 8192 |Ez az hello sikerült létező hello elsődleges replikációs várólistában lévő műveletek maximális számát. Vegye figyelembe, hogy 2 szintűnek kell lennie. |
| MaxPrimaryReplicationQueueMemorySize |Uint, alapértelmezett érték a 0 |Ez az maximális érték hello hello elsődleges replikációs sor bájtokban. |
| InitialSecondaryReplicationQueueSize |Uint, alapértelmezett érték 64 |Ez az érték hello várólista hello replikációs műveletek hello másodlagos fenntartó hello kezdeti mérete határozza meg. Vegye figyelembe, hogy 2 szintűnek kell lennie. |
| MaxSecondaryReplicationQueueSize |Uint, alapértelmezett érték 16384 |Ez az hello sikerült létező hello másodlagos replikációs várólistában lévő műveletek maximális számát. Vegye figyelembe, hogy 2 szintűnek kell lennie. |
| MaxSecondaryReplicationQueueMemorySize |Uint, alapértelmezett érték a 0 |Ez az maximális érték hello hello másodlagos replikációs sor bájtokban. |
| SecondaryClearAcknowledgedOperations |Logikai érték, alapértelmezett értéke "false" |Logikai érték, amely szabályozza, hogy hello másodlagos replikátor hello műveletek törlődik, miután azok ismernek toohello elsődleges (kiürített toohello lemez). A beállítások a tooTRUE során egy feladatátvétel után be replikák elfogja hello új elsődleges, a további lemezolvasások eredményezhet. |
| MaxMetadataSizeInKB |Int, alapértelmezett érték 4 |Hello napló adatfolyam metaadatai maximális méretét. |
| MaxRecordSizeInKB |Uint, alapértelmezett érték: 1024 | A naplóbejegyzés adatfolyam maximális mérete. |
| CheckpointThresholdInMB |Int, alapértelmezett érték 50 |Ellenőrzőpont indul, amikor hello naplóhasználatra meghaladja ezt az értéket. |
| MaxAccumulatedBackupLogSizeInMB |Int, alapértelmezett érték a 800 |Maximális mérete (MB) a megadott biztonságimásolat-naplólánccal rendelkeznek a biztonsági mentési naplók halmozott. Egy növekményes biztonsági mentési kérelem sikertelen lesz, ha hello növekményes biztonsági mentés hoz létre egy biztonsági mentési napló, amelyek halmozott hello biztonsági mentési naplók óta hello kapcsolódó teljes biztonsági mentés toobe nagyobb, mint a mérete. Ilyen esetben a felhasználó az szükséges tootake teljes biztonsági mentés. |
| MaxWriteQueueDepthInKB |Int, alapértelmezett érték a 0 | Maximális int írási várólistamélység adott hello core naplózó hello napló ennek a replikának társított kilobájtban megadott is használhat. Ez az érték hello core naplózó frissítései közben lehet függőben lévő bájtok maximális száma. 0 a hello alapvető naplózó toocompute egy megfelelő értéket, vagy a 4 többszöröse lehet. |
| SharedLogId |Karakterlánc |Megosztott napló azonosítóját. Ez egy GUID azonosítót, és minden megosztott napló egyedinek kell lennie. |
| SharedLogPath |Karakterlánc |Toohello megosztott naplófájl elérési útja. Ha ez az érték üres hello alapértelmezett megosztott napló szolgál. |
| SlowApiMonitoringDuration |Idő (másodpercben), az alapértelmezett érték 300 | Adja meg a duration API figyelmeztetés állapotát az esemény előtt.|
| MinLogSizeInMB |Int, alapértelmezett érték a 0 |Hello tranzakciós napló legkisebb méretét. hello napló nem engedélyezett tootruncate tooa méret alatt ez a beállítás. 0 azt jelenti, hogy hello replikátor hello minimális naplóméret tooother beállítások alapján határozza meg. Az érték növelésével növeli a részleges példányszám és a növekményes biztonsági mentések során óta veszélyét annak, hogy a megfelelő naplófájlok rekordok csonkolva lesz arányában hello lehetőségét. |

### <a name="section-name-fabricclient"></a>Szakasz Name: FabricClient
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| NodeAddresses |karakterlánc, alapértelmezett érték a "" |Címek (kapcsolati karakterláncok), amely a Naming Service hello hello használt toocommunicate különböző csomópontokon gyűjteménye. Kezdetben hello ügyfél kapcsolódik véletlenszerűen hello címek egyikének kiválasztásával. Ha egynél több kapcsolati karakterlánc van megadva, és egy kapcsolat egy kommunikációs vagy időtúllépési hiba; miatt sikertelen hello ügyfél vált toouse hello következő cím egymás után. Újrapróbálkozások szemantikáját a lásd az hello szolgáltatáscím elnevezési újrapróbálkozási szakaszát. |
| ConnectionInitializationTimeout |Idő (másodpercben), az alapértelmezett érték 2 |Adja meg az időtartam másodpercben. Kapcsolat időkorlátja minden alkalommal ügyfél megpróbál tooopen kapcsolat toohello átjáró. |
| PartitionLocationCacheLimit |Int, alapértelmezett érték 100000 |A szolgáltatás feloldásához gyorsítótárazott partíciók száma (too0 nincs korlát beállítása). |
| ServiceChangePollInterval |Idő (másodpercben), alapértelmezett érték a 120 |Adja meg az időtartam másodpercben. hello időköz szolgáltatás egymást követő lekérdezések között hello ügyfél toohello átjáró regisztrált szolgáltatás módosítási értesítést visszahívások módosítja. |
| KeepAliveIntervalInSeconds |Int, alapértelmezett érték 20 |hello időköz mely hello FabricClient átviteli életben tartási üzenetek toohello átjáró küldje el. A 0; életben tartási le van tiltva. Pozitív értéknek kell lennie. |
| HealthOperationTimeout |Idő (másodpercben), alapértelmezett érték a 120 |Adja meg az időtartam másodpercben. hello időtúllépés jelentés üzenetet küldött tooHealth Manager. |
| HealthReportSendInterval |Idő (másodpercben), az alapértelmezett érték 30 |Adja meg az időtartam másodpercben. mely jelentéskészítési összetevő küld halmozott állapotfigyelő jelentések tooHealth Manager hello időközét. |
| HealthReportRetrySendInterval |Idő (másodpercben), az alapértelmezett érték 30 |Adja meg az időtartam másodpercben. mely jelentéskészítési összetevő újra elküldi a halmozott állapotfigyelő jelentések tooHealth Manager hello időközét. |
| RetryBackoffInterval |Idő (másodpercben), az alapértelmezett érték 3 |Adja meg az időtartam másodpercben. hello vissza az indító időköz hello művelet megkísérlése előtt. |
| MaxFileSenderThreads |Uint, alapértelmezett érték 10 |hello maximális száma párhuzamos átvitt fájlok. |

### <a name="section-name-common"></a>Szakasz Name: közös
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| PerfMonitorInterval |Idő (másodpercben), az alapértelmezett érték 1 |Adja meg az időtartam másodpercben. Teljesítmény figyelési időköz. Beállítás too0 vagy negatív érték letiltja a figyelést. |

### <a name="section-name-healthmanager"></a>Szakasz Name: HealthManager
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |Logikai érték, alapértelmezett értéke "false" |Fürt állapotházirend kiértékelése: egy alkalmazás típusa állapotának kiértékelését engedélyezése. |

### <a name="section-name-fabricnode"></a>Szakasz Name: FabricNode
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| StateTraceInterval |Idő (másodpercben), az alapértelmezett érték 300 |Adja meg az időtartam másodpercben. csomópont állapota minden egyes csomóponton, illetve a hierarchiában felfelé a FM/FMM csomópontjai nyomkövetés hello intervallumát. |

### <a name="section-name-nodedomainids"></a>Szakasz Name: NodeDomainIds
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| UpgradeDomainId |karakterlánc, alapértelmezett érték a "" |Egy csomópont tartozik hello frissítési tartomány ismerteti. |
| PropertyGroup |NodeFaultDomainIdCollection |Egy csomópont tartozik hello tartalék tartományok ismerteti. hello tartalék tartomány URI, amely hello csomópont hello adatközpontban hello helyét ismerteti keresztül van definiálva.  Tartalék tartomány URI sem hello formázása, fd: / fd/URI elérésiút-szegmens követ.|

### <a name="section-name-nodeproperties"></a>Szakasz Name: NodeProperties
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| PropertyGroup |NodePropertyCollectionMap |Csomópont tulajdonságai kulcs-érték pár karakterlánc gyűjteménye. |

### <a name="section-name-nodecapacities"></a>Szakasz Name: NodeCapacities
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| PropertyGroup |NodeCapacityCollectionMap |Csomópont-kapacitás különböző metrikáihoz gyűjteménye. |

### <a name="section-name-fabricnode"></a>Szakasz Name: FabricNode
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| StartApplicationPortRange |Int, alapértelmezett érték a 0 |Kezdő hello alkalmazás portok alrendszer üzemeltető kezeli. Szükséges, ha EndpointFilteringEnabled üzemeltetési igaz. |
| EndApplicationPortRange |Int, alapértelmezett érték a 0 |A záró (nem a két szélsőértéket beleértve) hello alkalmazás portok alrendszer üzemeltető kezeli. Szükséges, ha EndpointFilteringEnabled üzemeltetési igaz. |
| ClusterX509StoreName |karakterlánc, alapértelmezett érték "A" |Fürtön belüli kommunikáció biztonságához fürt tanúsítványt tartalmazó X.509 tanúsítvány tároló neve. |
| ClusterX509FindType |karakterlánc, alapértelmezett értéke "FindByThumbprint" |Azt jelzi, hogyan toosearch fürt tanúsítvány hello tárolóban megadott ClusterX509StoreName támogatott értékek: "FindByThumbprint"; "FindBySubjectName" a "FindBySubjectName"; Ha több egyező; hello hello legtávolabbi jelszólejárat szolgál. |
| ClusterX509FindValue |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét toolocate fürt tanúsítványt használja. |
| ClusterX509FindValueSecondary |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét toolocate fürt tanúsítványt használja. |
| ServerAuthX509StoreName |karakterlánc, alapértelmezett érték "A" |X.509 tanúsítvány tároló, amely tartalmazza a kiszolgálói tanúsítvány entrée szolgáltatás neve. |
| ServerAuthX509FindType |karakterlánc, alapértelmezett értéke "FindByThumbprint" |Azt jelzi, hogyan kiszolgálótanúsítvány hello tárolójában toosearch ServerAuthX509StoreName támogatott értéke által meghatározott: FindByThumbprint; FindBySubjectName. |
| ServerAuthX509FindValue |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét toolocate kiszolgálói tanúsítványt használja. |
| ServerAuthX509FindValueSecondary |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét toolocate kiszolgálói tanúsítványt használja. |
| ClientAuthX509StoreName |karakterlánc, alapértelmezett érték "A" |Hello X.509 tanúsítvány tároló, a tanúsítványt az alapértelmezett rendszergazdai szerepkörhöz FabricClient neve. |
| ClientAuthX509FindType |karakterlánc, alapértelmezett értéke "FindByThumbprint" |Azt jelzi, hogyan hello tárolójában tanúsítvány toosearch ClientAuthX509StoreName támogatott értéke által meghatározott: FindByThumbprint; FindBySubjectName. |
| ClientAuthX509FindValue |karakterlánc, alapértelmezett érték a "" | Keresési szűrő értékét alapértelmezett rendszergazdai szerepkör FabricClient toolocate tanúsítványt használja. |
| ClientAuthX509FindValueSecondary |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét alapértelmezett rendszergazdai szerepkör FabricClient toolocate tanúsítványt használja. |
| UserRoleClientX509StoreName |karakterlánc, alapértelmezett érték "A" |Hello X.509 tanúsítvány tároló, a tanúsítványt az alapértelmezett felhasználói szerepkörhöz FabricClient neve. |
| UserRoleClientX509FindType |karakterlánc, alapértelmezett értéke "FindByThumbprint" |Azt jelzi, hogyan hello tárolójában tanúsítvány toosearch UserRoleClientX509StoreName támogatott értéke által meghatározott: FindByThumbprint; FindBySubjectName. |
| UserRoleClientX509FindValue |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét alapértelmezett felhasználói szerepkör FabricClient toolocate tanúsítványt használja. |
| UserRoleClientX509FindValueSecondary |karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét alapértelmezett felhasználói szerepkör FabricClient toolocate tanúsítványt használja. |

### <a name="section-name-paas"></a>Szakasz Name: Paas
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| ClusterId |karakterlánc, alapértelmezett érték a "" |A konfiguráció védelmét háló által használt X509 tanúsítványtárolóból. |

### <a name="section-name-fabrichost"></a>Szakasz név: {a FabricHost
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| StopTimeout |Idő (másodpercben), az alapértelmezett érték 300 |Adja meg az időtartam másodpercben. az üzemeltetett szolgáltatás aktiváláshoz; hello időtúllépés az inaktiválást és frissítését. |
| StartTimeout |Idő (másodpercben), az alapértelmezett érték 300 |Adja meg az időtartam másodpercben. Fabricactivationmanager indítási időkorlátját. |
| ActivationRetryBackoffInterval |Idő (másodpercben), az alapértelmezett érték 5 |Adja meg az időtartam másodpercben. Visszalépési időköz minden aktiválási hibájánál; minden folyamatos működése hiba hello rendszer megpróbálja újból be toohello MaxActivationFailureCount hello aktiválása. hello újrapróbálkozási időköz minden kísérlet rendszerhez nevű szoftver a folyamatos aktiválási hiba és hello aktiválási vissza az indító időköz. |
| ActivationMaxRetryInterval |Idő (másodpercben), az alapértelmezett érték 300 |Adja meg az időtartam másodpercben. Maximális újrapróbálkozási időköz az aktiváláshoz. Minden folyamatos hiba hello újrapróbálkozáskor időköz számítjuk Min (ActivationMaxRetryInterval; Folyamatos hibaszámláló * ActivationRetryBackoffInterval). |
| ActivationMaxFailureCount |Int, alapértelmezett érték 10 |Ez azért, amelynek a rendszer a sikertelen aktiválást, mielőtt megpróbálja hello maximális száma. |
| EnableServiceFabricAutomaticUpdates |Logikai érték, alapértelmezett értéke "false" |Ez a tooenable háló automatikus frissítés a Windows Update segítségével. |
| EnableServiceFabricBaseUpgrade |Logikai érték, alapértelmezett értéke "false" |Ez a tooenable alap frissítés a kiszolgálón. |
| EnableRestartManagement |Logikai érték, alapértelmezett értéke "false" |Ez a tooenable kiszolgáló újraindítására. |


### <a name="section-name-failovermanager"></a>Szakasz Name: FailoverManager
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| UserReplicaRestartWaitDuration |Idő (másodpercben), az alapértelmezett érték 60.0 * 30 |Adja meg az időtartam másodpercben. Ha egy megőrzött replikát leáll; Windows Fabric megvárja-e a ennek az időtartamnak a hello replika toocome készítsen biztonsági mentést (amelynek a hello állapot másolatát) új helyettesítő-replikák létrehozása előtt. |
| QuorumLossWaitDuration |Idő (másodpercben), alapértelmezett érték a MaxValue |Adja meg az időtartam másodpercben. Ez a hello maximális időtartama, amelyeknek a kvórum elvesztése állapotban egy partíció toobe azt engedélyezi. Ha hello partíció továbbra is a kvórum elvesztése után az ezen időtartam; hello partíció helyre lett állítva a kvórum elvesztése által hello annak eldöntéséhez, hogy elveszett replikákat le. Vegye figyelembe, hogy ez potenciálisan járnak a adatvesztés. |
| UserStandByReplicaKeepDuration |Idő (másodpercben), az alapértelmezett érték 3600.0 * 24 * 7 |Adja meg az időtartam másodpercben. Ha egy megőrzött replikát térjen vissza az alsó állapota; akkor lehet, hogy már helyett. Ez az időzítő meghatározza, hogy mennyi ideig hello FM hello készenléti replika közli a törlésük előtt. |
| UserMaxStandByReplicaCount |Int, alapértelmezett érték 1 |hello alapértelmezett maximális található StandBy replikák száma hello rendszer szolgáltatás a felhasználó számára. |

### <a name="section-name-namingservice"></a>Szakasz Name: NamingService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, alapértelmezett érték 7 |Mindegyik partíció hello szolgáltatás tárának beállítása replika hello száma. Replika készletek növekszik hello megbízhatósági szint hello információt a Naming Service tároló; hello hello száma növelése információk hello hello módosítást csökkentése elvesznek miatt csomóponthibák; a Windows Fabric és hello ennyi idő alatt megnövekedett terhelés költségekkel tart tooperform frissítések toohello elnevezési adatokat.|
|MinReplicaSetSize | Int, alapértelmezett érték 3 | hello szolgáltatás replikák szükséges toowrite toocomplete történő frissítést. Ha kevesebb, mint a replikákat a hello rendszer hello megbízhatóság rendszer aktív frissítések toohello elnevezési szolgáltatás tárolójának megtagadja a replikák visszaállításáig. Ez az érték soha nem lehet több, mint a hello TargetReplicaSetSize. |
|ReplicaRestartWaitDuration | Idő (másodpercben), az alapértelmezett érték (60.0 * 30)| Adja meg az időtartam másodpercben. Ha a szolgáltatás replika leáll; Ez az időzítő elindul.  Amikor lejár hello FM akkor kezdődik, tooreplace hello replikákat, amely nem működnek (hogy még tartja őket elveszett). |
|QuorumLossWaitDuration | Idő (másodpercben), alapértelmezett érték a MaxValue | Adja meg az időtartam másodpercben. Ha a szolgáltatás lekérdezi a kvórum elvesztése; Ez az időzítő elindul.  Amikor lejár hello FM figyelembe veszi hello le replikákat elveszett; és kísérlet toorecover kvórum. Nem, hogy emiatt előfordulhat adatvesztés. |
|StandByReplicaKeepDuration | Idő (másodpercben), az alapértelmezett érték 3600.0 * 2 | Adja meg az időtartam másodpercben. Ha egy szolgáltatás replikák térjen vissza az alsó állapota; akkor lehet, hogy már helyett.  Ez az időzítő meghatározza, hogy mennyi ideig hello FM hello készenléti replika közli a törlésük előtt. |
|PlacementConstraints | karakterlánc, alapértelmezett érték a "" | Az elhelyezési korlátozás hello szolgáltatás. |
|ServiceDescriptionCacheLimit | Int, alapértelmezett érték a 0 | hello őrzi meg a hello LRU szolgáltatás leírása gyorsítótár következő bejegyzések maximális számában hello tároló szolgáltatás (too0 nincs korlát beállítása). |
|RepairInterval | Idő (másodpercben), az alapértelmezett érték 5 | Adja meg az időtartam másodpercben. Mely hello elnevezési inkonzisztenciát javítási közötti hello hatóság tulajdonos és a tulajdonos neve lesz a kiindulási pont időköz. |
|MaxNamingServiceHealthReports | Int, alapértelmezett érték 10 | egy időben hello névhasználati tároló lassú műveletek maximális száma a szolgáltatás jelentések nem kifogástalan. Ha 0; minden lassú műveletek kerülnek. |
| MaxMessageSize |Int, alapértelmezett érték 4*1024*1024 |hello üzenetek maximális méretét, az ügyfél-kommunikációhoz csomópont elnevezési használatakor. Szolgáltatásmegtagadási támadás enyhítése; alapértelmezett érték: 4MB. |
| MaxFileOperationTimeout |Idő (másodpercben), az alapértelmezett érték 30 |Adja meg az időtartam másodpercben. hello maximális időtúllépés fájl tárolási szolgáltatási művelet engedélyezett. Kérések megadó nagyobb időtúllépést a program elutasítja. |
| Konfigurált MaxOperationTimeout |Idő (másodpercben), az alapértelmezett érték 600 |Adja meg az időtartam másodpercben. hello maximális időkorlátot az Ügyfélműveletek engedélyezett. Kérések megadó nagyobb időtúllépést a program elutasítja. |
| MaxClientConnections |Int, alapértelmezett érték 1000 |hello maximálisan megengedett számú ügyfél kapcsolódik-átjárón. |
| ServiceNotificationTimeout |Idő (másodpercben), az alapértelmezett érték 30 |Adja meg az időtartam másodpercben. értesítések toohello szolgáltatásügyfél során használt hello időtúllépés. |
| MaxOutstandingNotificationsPerClient |Int, alapértelmezett érték 1000 |függőben lévő értesítések előtt egy ügyfél-regisztrációk maximális száma hello kényszerített hello az átjáró le van zárva. |
| MaxIndexedEmptyPartitions |Int, alapértelmezett érték 1000 |hello üres partíciók száma, amely a marad indexelt hello értesítési gyorsítótár újracsatlakozására ügyfelek szinkronizálásához. Ez a szám fent üres partíciókat hello index növekvő keresési verzió sorrendben törlődik. Újracsatlakozás ügyfelek továbbra is szinkronizálja és kihagyott üres partíció frissítését; de hello szinkronizálási protokoll drágább válik. |
| GatewayServiceDescriptionCacheLimit |Int, alapértelmezett érték a 0 |hello őrzi meg a hello LRU szolgáltatás leírása gyorsítótár következő bejegyzések maximális számában hello Naming Gateway (too0 nincs korlát beállítása). |
| PartitionCount |Int, alapértelmezett érték 3 |a hello szolgáltatás tároló toobe létrehozott partíciók hello száma. Mindegyik partíció tulajdonában van egy partíciókulcsot, amely megfelel a tooits index; Igen partíciókulcsok [0; PartitionCount) létezik. Állítsa be a növekvő számú hello szolgáltatás partíciók növekszik hello szolgáltatás hello skála hello átlagos bármilyen biztonsági a replika által tárolt adatok mennyisége csökkentésével végre; a nagyobb erőforrás-felhasználás költségekkel (óta PartitionCount * ReplicaSetSize szolgáltatás replikák fenn kell tartani).|

### <a name="section-name-runas"></a>Szakasz Name: RunAs
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Ezért |karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello Futtatás mint fiók nevét. Ez csak a "DomainUser" vagy "ManagedServiceAccount" fiókot a szükséges típus. Érvényes értékek: "tartomány\felhasználónév" vagy "user@domain". |
|RunAsAccountType|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók típusa. Ez szükséges minden RunAs szakasz érvényes értékek: "DomainUser/NetworkService/ManagedServiceAccount/LocalSystem".|
|Futtatási_jelszó|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók jelszavát. Ez csak a "DomainUser" fióktípus szükséges. |

### <a name="section-name-runasfabric"></a>Szakasz Name: RunAs_Fabric
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Ezért |karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello Futtatás mint fiók nevét. Ez csak a "DomainUser" vagy "ManagedServiceAccount" fiókot a szükséges típus. Érvényes értékek: "tartomány\felhasználónév" vagy "user@domain". |
|RunAsAccountType|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók típusa. Ez szükséges minden RunAs szakasz érvényes értékek: "LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem". |
|Futtatási_jelszó|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók jelszavát. Ez csak a "DomainUser" fióktípus szükséges. |

### <a name="section-name-runashttpgateway"></a>Szakasz Name: RunAs_HttpGateway
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Ezért |karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello Futtatás mint fiók nevét. Ez csak a "DomainUser" vagy "ManagedServiceAccount" fiókot a szükséges típus. Érvényes értékek: "tartomány\felhasználónév" vagy "user@domain". |
|RunAsAccountType|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók típusa. Ez szükséges minden RunAs szakasz érvényes értékek: "LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem". |
|Futtatási_jelszó|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók jelszavát. Ez csak a "DomainUser" fióktípus szükséges. |

### <a name="section-name-runasdca"></a>Szakasz Name: RunAs_DCA
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Ezért |karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello Futtatás mint fiók nevét. Ez csak a "DomainUser" vagy "ManagedServiceAccount" fiókot a szükséges típus. Érvényes értékek: "tartomány\felhasználónév" vagy "user@domain". |
|RunAsAccountType|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók típusa. Ez szükséges minden RunAs szakasz érvényes értékek: "LocalUser/DomainUser/NetworkService/ManagedServiceAccount/LocalSystem". |
|Futtatási_jelszó|karakterlánc, alapértelmezett érték a "" |Azt jelzi, hogy hello futtató fiók jelszavát. Ez csak a "DomainUser" fióktípus szükséges. |

### <a name="section-name-httpgateway"></a>Szakasz Name: HttpGateway
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
|IsEnabled|Logikai érték, alapértelmezett értéke "false" | Engedélyezi vagy letiltja a hello httpgateway. Httpgateway alapértelmezés szerint le van tiltva, és ez a konfiguráció toobe set tooenable kell azt. |
|ActiveListeners |Uint, alapértelmezett érték 50 | Olvasási toopost toohello http kiszolgáló várólista száma. Ezen lehetőség hello is teljesíteni hello HttpGateway egyidejű kérelmek száma. |
|MaxEntityBodySize |Uint, alapértelmezett érték 4194304 |  Hello maximális méretének hello, amelyek a http-kérelem lehet számítani biztosít. Alapértelmezett érték: 4MB. Httpgateway meghiúsul kérelmet, ha a törzs mérete rendelkezik > ezt az értéket. Minimális olvasási adatrészlet mérete 4096 bájt. Így ez toobe > = 4096. |
|HttpGatewayHealthReportSendInterval |Idő (másodpercben), az alapértelmezett érték 30 | Adja meg az időtartam másodpercben. hello időköz mely hello Http átjáró küldje el az egészségügyi jelentések tooHealth Manager halmozott. |

### <a name="section-name-ktllogger"></a>Szakasz Name: KtlLogger
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
|AutomaticMemoryConfiguration |Int, alapértelmezett érték 1 | Az jelzőt, amely azt jelzi, ha hello memória beállításait konfigurálni kell, hogy dinamikusan és automatikusan. Ha nulla majd hello memória konfigurációs beállítások közvetlenül használatosak, és ne változtassa meg a rendszer feltételek alapján. Ha egy hello memória beállítások automatikusan konfigurálva, és módosíthatja az rendszer feltételek alapján. |
|WriteBufferMemoryPoolMinimumInKB |Int, alapértelmezett érték 8388608 |KB tooinitially hello száma hello írási pufferkészlet memóriát lefoglalni. Használja a 0 tooindicate nincs alapértelmezett korlát meg kell felelniük az alábbi SharedLogSizeInMB. |
|WriteBufferMemoryPoolMaximumInKB | Int, alapértelmezett érték a 0 |az írási puffer memória készlet toogrow legfeljebb KB tooallow hello hello száma. Használja a 0 tooindicate nincs korlát. |
|MaximumDestagingWriteOutstandingInKB | Int, alapértelmezett érték a 0 | hello száma KB tooallow hello napló tooadvance hello dedikált napló előre megosztott. Használja a 0 tooindicate nincs korlát.
|SharedLogPath |karakterlánc, alapértelmezett érték a "" | Elérési út és fájlnév neve toolocation tooplace megosztott naplózási tároló. Használjon "" az alapértelmezett elérési út a fabric adatgyökerében használatával. |
|SharedLogId |karakterlánc, alapértelmezett érték a "" |Megosztott naplózási tároló egyedi GUID azonosítója. Használjon "" Ha a fabric adatgyökerében alapértelmezett elérési úttal. |
|SharedLogSizeInMB |Int, alapértelmezett érték 8192 | a hello megosztott naplózási tároló MB tooallocate hello száma. |

### <a name="section-name-applicationgatewayhttp"></a>Szakasz nevét: Alapú/Http
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
|IsEnabled |Logikai érték, alapértelmezett értéke "false" | Engedélyezi vagy letiltja a hello HttpApplicationGateway. HttpApplicationGateway alapértelmezés szerint le van tiltva, és ez a konfiguráció toobe set tooenable kell azt. |
|NumberOfParallelOperations | Uint, alapértelmezett érték 5000 | Olvasási toopost toohello http kiszolgáló várólista száma. Ezen lehetőség hello is teljesíteni hello HttpGateway egyidejű kérelmek száma. |
|DefaultHttpRequestTimeout |Idő másodpercben. alapértelmezett érték a 120 |Adja meg az időtartam másodpercben.  Hello alapértelmezett kérelem időtúllépése ad az hello http app átjáró feldolgozott hello http-kérelmekre. |
|ResolveServiceBackoffInterval |Idő (másodpercben), az alapértelmezett érték 5 |Adja meg az időtartam másodpercben.  Hello alapértelmezett biztonsági ki időköz biztosít egy sikertelen feloldás szolgáltatás művelet megkísérlése előtt. |
|BodyChunkSize |Uint, alapértelmezett érték 16384 |  Által biztosított hello mérete bájtban hello adatrészlet használt tooread hello törzsében. |
|GatewayAuthCredentialType |karakterlánc, alapértelmezett értéke "None" | Meghatározza, hogy hello típusú biztonsági hitelesítő adatok toouse hello http app átjáró végpont az érvényes értékek: "Nincs / X 509. |
|GatewayX509CertificateStoreName |karakterlánc, alapértelmezett érték "A" | X.509 tanúsítvány tároló http app átjáró tanúsítványt tartalmazó neve. |
|GatewayX509CertificateFindType |karakterlánc, alapértelmezett értéke "FindByThumbprint" | Azt jelzi, hogyan hello tárolójában tanúsítvány toosearch GatewayX509CertificateStoreName támogatott értéke által meghatározott: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | karakterlánc, alapértelmezett érték a "" | Keresési szűrő értékét toolocate hello http app átjáró tanúsítványt használja. Ez a tanúsítvány hello https-végponton van beállítva, és akkor is használt tooverify hello identitás hello alkalmazás hello szolgáltatások által szükség esetén. Első; findvalue – keresése Ha, amely nem létezik; FindValueSecondary keresése. |
|GatewayX509CertificateFindValueSecondary | karakterlánc, alapértelmezett érték a "" |Keresési szűrő értékét toolocate hello http app átjáró tanúsítványt használja. Ez a tanúsítvány hello https-végponton van beállítva, és akkor is használt tooverify hello identitás hello alkalmazás hello szolgáltatások által szükség esetén. Első; findvalue – keresése Ha, amely nem létezik; FindValueSecondary keresése.|

### <a name="section-name-management"></a>Szakasz Name: kezelése
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Előtaggal |SecureString | Kapcsolati karakterlánc toohello Lemezképtárolóba gyökere. |
| ImageStoreMinimumTransferBPS | Int, alapértelmezett érték: 1024 |hello minimális átviteli sebesség hello fürt és a Lemezképtárolóba között. Ez az érték használt toodetermine hello időtúllépés esetén elérése hello külső Lemezképtárolóba. Módosítsa ezt az értéket, ha a fürt hello és Lemezképtárolóba közötti hello késés magas tooallow hello fürt toodownload a több idejébe hello külső Lemezképtárolóba csak. |
|AzureStorageMaxWorkerThreads | Int, alapértelmezett érték 25 | hello maximális száma párhuzamos munkaszál. |
|AzureStorageMaxConnections | Int, alapértelmezett érték 5000 | hello a tooazure tárolási létesített egyidejű kapcsolatok maximális számát. |
|AzureStorageOperationTimeout | Idő (másodpercben), az alapértelmezett érték 6000 | Adja meg az időtartam másodpercben. Xstore művelet toocomplete időkorlátját. |
|ImageCachingEnabled | Logikai érték, az alapértelmezett érték true | Ez a konfiguráció lehetővé teszi tooenable vagy a gyorsítótárazás letiltása. |
|DisableChecksumValidation | Logikai érték, alapértelmezett értéke "false" | Ez a konfiguráció tooenable lehetővé teszi, vagy tiltsa le az ellenőrzőösszeg-érvényesítés alkalmazás kiépítése során. |
|DisableServerSideCopy | Logikai érték, alapértelmezett értéke "false" | Ez a konfiguráció lehetővé teszi, vagy letilthatja a kiszolgáló ügyféloldali másolása a Lemezképtárolóba hello alkalmazáscsomag alkalmazás kiépítése során. |

### <a name="section-name-healthmanagerclusterhealthpolicy"></a>Szakasz nevét: HealthManager/ClusterHealthPolicy
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| A ConsiderWarningAsError |Logikai érték, alapértelmezett értéke "false" |Fürt állapotházirend kiértékelése: figyelmeztetések hibák tekintendők. |
| MaxPercentUnhealthyNodes | Int, alapértelmezett érték a 0 |Fürt állapotházirend kiértékelése: nem kifogástalan csomópontokat maximális százalékát engedélyezett hello fürt toobe kifogástalan. |
| MaxPercentUnhealthyApplications | Int, alapértelmezett érték a 0 |Állapotházirend értékelési fürt: a nem megfelelő alkalmazások maximális százalékát engedélyezett hello fürt toobe kifogástalan. |
|A MaxPercentDeltaUnhealthyNodes | Int, alapértelmezett érték 10 |A fürt frissítési állapotházirend értékelése: különbözeti nem kifogástalan csomópontokat maximális százalékát engedélyezett hello fürt toobe kifogástalan. |
|Maxpercentupgradedomaindeltaunhealthynodes paraméterek hiányzó értékei | Int, alapértelmezett érték 15 |A fürt frissítési állapotházirend értékelése: a frissítési tartományok sérült csomópontok különbözeti maximális százalék engedélyezett hello fürt toobe kifogástalan.|

### <a name="section-name-faultanalysisservice"></a>Szakasz Name: FaultAnalysisService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, alapértelmezett érték a 0 |NOT_PLATFORM_UNIX_START hello FaultAnalysisService a TargetReplicaSetSize. |
| MinReplicaSetSize |Int, alapértelmezett érték a 0 |hello FaultAnalysisService a MinReplicaSetSize. |
| ReplicaRestartWaitDuration |Idő (másodpercben), az alapértelmezett érték 60 perc|Adja meg az időtartam másodpercben. hello ReplicaRestartWaitDuration FaultAnalysisService számára. |
| QuorumLossWaitDuration | Idő (másodpercben), alapértelmezett érték a MaxValue |Adja meg az időtartam másodpercben. hello QuorumLossWaitDuration FaultAnalysisService számára. |
| StandByReplicaKeepDuration| Idő (másodpercben), az alapértelmezett érték (60*24*7) perc |Adja meg az időtartam másodpercben. hello StandByReplicaKeepDuration FaultAnalysisService számára. |
| PlacementConstraints | karakterlánc, alapértelmezett érték a ""| hello PlacementConstraints FaultAnalysisService számára. |
| StoredActionCleanupIntervalInSeconds | Int, alapértelmezett érték 3600 |Ez az, hogy milyen gyakran hello tároló törlődnek.  Csak azokat a műveleteket, terminál állapotban; és az elvégzett legalább ezelőtt CompletedActionKeepDurationInSeconds lesz eltávolítva. |
| CompletedActionKeepDurationInSeconds | Int, alapértelmezett érték 604800 | Ez a körülbelül mennyi ideig tookeep műveleteket terminál állapotban van.  Ez is attól függ, StoredActionCleanupIntervalInSeconds; mivel a hello munkahelyi toocleanup csak adott időközönként történik. 604800 érték 7 nap. |
| StoredChaosEventCleanupIntervalInSeconds | Int, alapértelmezett érték 3600 |Azt, hogy milyen gyakran hello tároló naplózza a tisztítás; Ha hello események száma legfeljebb 30000; hello karbantartása rendszer indítsa. |

### <a name="section-name-filestoreservice"></a>Szakasz Name: FileStoreService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| NamingOperationTimeout |Idő (másodpercben), az alapértelmezett érték 60 |Adja meg az időtartam másodpercben. hello időkorlátjának elnevezési művelet végrehajtása. |
| QueryOperationTimeout | Idő (másodpercben), az alapértelmezett érték 60 |Adja meg az időtartam másodpercben. hello időkorlátjának lekérdezési művelet végrehajtása. |
| MaxCopyOperationThreads | Uint, alapértelmezett érték a 0 | hello párhuzamos fájlok maximális számát, hogy a másodlagos elsődleges másolhatja. "0" == magok száma. |
| MaxFileOperationThreads | Uint, alapértelmezett érték 100 | hello maximális száma párhuzamos engedélyezett tooperform FileOperations (Copy/Move) az elsődleges hello. "0" == magok száma. |
| MaxStoreOperations | Uint, alapértelmezett érték 4096 |hello maximális száma párhuzamos tároló transcation műveletek elsődleges engedélyezett. "0" == magok száma. |
| MaxRequestProcessingThreads | Uint, alapértelmezett érték 200 |hello maximális száma párhuzamos engedélyezett tooprocess kérelmek hello elsődleges. "0" == magok száma. |
| MaxSecondaryFileCopyFailureThreshold | Uint, alapértelmezett érték 25| Mielőtt másodlagos hello fájl másolása újbóli próbálkozások számát maximális hello. |
| AnonymousAccessEnabled | Logikai érték, az alapértelmezett érték true |Engedélyezi/letiltja a névtelen hozzáférés toohello FileStoreService megosztja. |
| PrimaryAccountType | karakterlánc, alapértelmezett érték a "" |hello elsődleges hello egyszerű tooACL hello FileStoreService az AccountType megosztja. |
| PrimaryAccountUserName | karakterlánc, alapértelmezett érték a "" |hello elsődleges fiók felhasználóneve hello egyszerű tooACL hello FileStoreService megosztások. |
| PrimaryAccountUserPassword | SecureString, az alapértelmezett érték üres |megosztja a FileStoreService hello egyszerű tooACL hello hello elsődleges fiók jelszavát. |
| FileStoreService | PrimaryAccountNTLMPasswordSecret | SecureString, az alapértelmezett érték üres | hello jelszó titkos adatot, amely kezdőérték toogenerated használja ugyanazt a jelszót az NTLM-hitelesítés használata esetén. |
| PrimaryAccountNTLMX509StoreLocation | karakterlánc, alapértelmezett értéke "LocalMachine"| hello tároló helye hello X509 használt tanúsítvány toogenerate HMAC hello PrimaryAccountNTLMPasswordSecret NTLM-hitelesítés használata esetén. |
| PrimaryAccountNTLMX509StoreName | karakterlánc, alapértelmezett értéke "MY"| hello hello X509 használt tanúsítvány toogenerate HMAC hello PrimaryAccountNTLMPasswordSecret NTLM-hitelesítés használata esetén a tároló neve. |
| PrimaryAccountNTLMX509Thumbprint | karakterlánc, alapértelmezett érték a ""|hello ujjlenyomata hello X509 használt tanúsítvány toogenerate HMAC hello PrimaryAccountNTLMPasswordSecret NTLM-hitelesítés használata esetén. |
| SecondaryAccountType | karakterlánc, alapértelmezett érték a ""| hello másodlagos hello egyszerű tooACL hello FileStoreService az AccountType megosztja. |
| SecondaryAccountUserName | karakterlánc, alapértelmezett érték a ""| hello másodlagos fiók felhasználóneve hello egyszerű tooACL hello FileStoreService megosztások. |
| SecondaryAccountUserPassword | SecureString, az alapértelmezett érték üres |megosztja a FileStoreService hello egyszerű tooACL hello hello másodlagos fiók jelszavát.  |
| SecondaryAccountNTLMPasswordSecret | SecureString, az alapértelmezett érték üres | hello jelszó titkos adatot, amely kezdőérték toogenerated használja ugyanazt a jelszót az NTLM-hitelesítés használata esetén. |
| SecondaryAccountNTLMX509StoreLocation | karakterlánc, alapértelmezett értéke "LocalMachine" |hello tároló helye hello X509 használt tanúsítvány toogenerate HMAC hello SecondaryAccountNTLMPasswordSecret NTLM-hitelesítés használata esetén. |
| SecondaryAccountNTLMX509StoreName | karakterlánc, alapértelmezett értéke "MY" |hello hello X509 használt tanúsítvány toogenerate HMAC hello SecondaryAccountNTLMPasswordSecret NTLM-hitelesítés használata esetén a tároló neve. |
| SecondaryAccountNTLMX509Thumbprint | karakterlánc, alapértelmezett érték a ""| hello ujjlenyomata hello X509 használt tanúsítvány toogenerate HMAC hello SecondaryAccountNTLMPasswordSecret NTLM-hitelesítés használata esetén. |

### <a name="section-name-imagestoreservice"></a>Szakasz Name: ImageStoreService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Engedélyezve |Logikai érték, alapértelmezett értéke "false" |hello ImageStoreService Enabled jelzőt. |
| TargetReplicaSetSize | Int, alapértelmezett érték 7 |hello ImageStoreService a TargetReplicaSetSize. |
| MinReplicaSetSize | Int, alapértelmezett érték 3 |hello ImageStoreService a MinReplicaSetSize. |
| ReplicaRestartWaitDuration | Idő (másodpercben), az alapértelmezett érték 60.0 * 30 | Adja meg az időtartam másodpercben. hello ReplicaRestartWaitDuration ImageStoreService számára. |
| QuorumLossWaitDuration | Idő (másodpercben), alapértelmezett érték a MaxValue | Adja meg az időtartam másodpercben. hello QuorumLossWaitDuration ImageStoreService számára. |
| StandByReplicaKeepDuration | Idő (másodpercben), az alapértelmezett érték 3600.0 * 2 | Adja meg az időtartam másodpercben. hello StandByReplicaKeepDuration ImageStoreService számára. |
| PlacementConstraints | karakterlánc, alapértelmezett érték a "" | hello PlacementConstraints ImageStoreService számára. |
| ClientUploadTimeout | Idő (másodpercben), az alapértelmezett érték 1800 |Adja meg az időtartam másodpercben. Legfelső szintű feltöltés időtúllépési értékének tooImage Store szolgáltatás igénylése. |
| ClientCopyTimeout | Idő (másodpercben), az alapértelmezett érték 1800 | Adja meg az időtartam másodpercben. Legfelső szintű másolási időtúllépési értékének tooImage Store szolgáltatás igénylése. |
| ClientDownloadTimeout | Idő (másodpercben), az alapértelmezett érték 1800 | Adja meg az időtartam másodpercben. Legfelső szintű letöltése időtúllépési értékének tooImage Store szolgáltatás igénylése |
| ClientListTimeout | Idő (másodpercben), az alapértelmezett érték 600 | Adja meg az időtartam másodpercben. Legfelső szintű lista időtúllépési értékének tooImage Store szolgáltatás igénylése. |
| ClientDefaultTimeout | Idő (másodpercben), alapértelmezett értéke 180 | Adja meg az időtartam másodpercben. Minden nem feltöltés/nem letöltési kérelmek időtúllépési értéke (pl. létezik; törlése) tooImage Store szolgáltatást. |

### <a name="section-name-imagestoreclient"></a>Szakasz Name: ImageStoreClient
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| ClientUploadTimeout |Idő (másodpercben), az alapértelmezett érték 1800 | Adja meg az időtartam másodpercben. Legfelső szintű feltöltés időtúllépési értékének tooImage Store szolgáltatás igénylése. |
| ClientCopyTimeout | Idő (másodpercben), az alapértelmezett érték 1800 | Adja meg az időtartam másodpercben. Legfelső szintű másolási időtúllépési értékének tooImage Store szolgáltatás igénylése. |
|ClientDownloadTimeout | Idő (másodpercben), az alapértelmezett érték 1800 | Adja meg az időtartam másodpercben. Legfelső szintű letöltése időtúllépési értékének tooImage Store szolgáltatás igénylése. |
|ClientListTimeout | Idő (másodpercben), az alapértelmezett érték 600 |Adja meg az időtartam másodpercben. Legfelső szintű lista időtúllépési értékének tooImage Store szolgáltatás igénylése. |
|ClientDefaultTimeout | Idő (másodpercben), alapértelmezett értéke 180 | Adja meg az időtartam másodpercben. Minden nem feltöltés/nem letöltési kérelmek időtúllépési értéke (pl. létezik; törlése) tooImage Store szolgáltatást. |

### <a name="section-name-tokenvalidationservice"></a>Szakasz Name: TokenValidationService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| Szolgáltatók |karakterlánc, alapértelmezett értéke "DSTS" |Jogkivonatok érvényesség-ellenőrzése szolgáltatók tooenable vesszővel elválasztott listáját (érvényes szolgáltatók a következők: DSTS; AAD-BEN). Jelenleg csak egyetlen szolgáltató bármikor engedélyezhető. |

### <a name="section-name-upgradeorchestrationservice"></a>Szakasz Name: UpgradeOrchestrationService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| TargetReplicaSetSize |Int, alapértelmezett érték a 0 |hello UpgradeOrchestrationService a TargetReplicaSetSize. |
| MinReplicaSetSize |Int, alapértelmezett érték a 0 | hello UpgradeOrchestrationService a MinReplicaSetSize.
| ReplicaRestartWaitDuration | Idő (másodpercben), az alapértelmezett érték 60 perc| Adja meg az időtartam másodpercben. hello ReplicaRestartWaitDuration UpgradeOrchestrationService számára. |
| QuorumLossWaitDuration | Idő (másodpercben), alapértelmezett érték a MaxValue | Adja meg az időtartam másodpercben. hello QuorumLossWaitDuration UpgradeOrchestrationService számára. |
| StandByReplicaKeepDuration | Idő (másodpercben), az alapértelmezett érték 60*24*7 perc | Adja meg az időtartam másodpercben. hello StandByReplicaKeepDuration UpgradeOrchestrationService számára. |
| PlacementConstraints | karakterlánc, alapértelmezett érték a "" | hello PlacementConstraints UpgradeOrchestrationService számára. |
| AutoupgradeEnabled | Logikai érték, az alapértelmezett érték true | Automatikus lekérdezési és frissítési művelet a cél-állapot fájl alapján. |
| UpgradeApprovalRequired | Logikai érték, alapértelmezett értéke "false" | A beállítás toomake kód frissítése a folytatás előtt rendszergazdai jóváhagyás megkövetelése. |

### <a name="section-name-upgradeservice"></a>Szakasz Name: UpgradeService
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| PlacementConstraints |karakterlánc, alapértelmezett érték a "" |hello PlacementConstraints frissítési szolgáltatás számára. |
| TargetReplicaSetSize | Int, alapértelmezett érték 3 | hello UpgradeService a TargetReplicaSetSize. |
| MinReplicaSetSize | Int, alapértelmezett érték a 2. régiója | hello UpgradeService a MinReplicaSetSize. |
| CoordinatorType | karakterlánc, alapértelmezett értéke "WUTest"| hello CoordinatorType UpgradeService számára. |
| BaseUrl | karakterlánc, alapértelmezett érték a "" |A UpgradeService BaseUrl. |
| ClusterId | karakterlánc, alapértelmezett érték a "" | A UpgradeService ClusterId. |
| X509StoreName | karakterlánc, alapértelmezett érték "A"| A UpgradeService X509StoreName. |
| X509StoreLocation | karakterlánc, alapértelmezett érték a "" | A UpgradeService X509StoreLocation. |
| X509FindType | karakterlánc, alapértelmezett érték a ""| A UpgradeService X509FindType. |
| X509FindValue | karakterlánc, alapértelmezett érték a "" | A UpgradeService X509FindValue. |
| X509SecondaryFindValue | karakterlánc, alapértelmezett érték a "" | A UpgradeService X509SecondaryFindValue. |
| OnlyBaseUpgrade | Logikai érték, alapértelmezett értéke "false" | A UpgradeService OnlyBaseUpgrade. |
| TestCabFolder | karakterlánc, alapértelmezett érték a "" | A UpgradeService TestCabFolder. |

### <a name="section-name-securityclientaccess"></a>Szakasz nevét: Biztonsági/ClientAccess
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| CreateName |karakterlánc, alapértelmezett az "Admin" |Biztonsági beállítások a elnevezési URI létrehozásához. |
| DeleteName |karakterlánc, alapértelmezett az "Admin" |Biztonsági beállítások elnevezési URI törlésre. |
| PropertyWriteBatch |karakterlánc, alapértelmezett az "Admin" |Biztonsági beállítások elnevezési tulajdonság írási műveleteket. |
| CreateService |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások a szolgáltatások létrehozásához. |
| CreateServiceFromTemplate |karakterlánc, alapértelmezett az "Admin" |Szolgáltatás létrehozása sablonból biztonsági beállításainak konfigurálása. |
| Updateservice függvényhez |karakterlánc, alapértelmezett az "Admin" |Biztonsági beállítások a szolgáltatásfrissítéseket. |
| DeleteService  |karakterlánc, alapértelmezett az "Admin" |Biztonsági beállítások a szolgáltatás törlésre. |
| ProvisionApplicationType |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások alkalmazás típusa üzembe helyezéséhez. |
| CreateApplication |karakterlánc, alapértelmezett az "Admin" | Alkalmazás létrehozása biztonsági beállításainak konfigurálása. |
| Deleteapplication függvényhez |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások az alkalmazás törléséhez. |
| UpgradeApplication |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások elindításáról és alkalmazásfrissítések megszakítása. |
| RollbackApplicationUpgrade |karakterlánc, alapértelmezett az "Admin" | Alkalmazásfrissítések visszaállítása biztonsági konfigurációját. |
| UnprovisionApplicationType |karakterlánc, alapértelmezett az "Admin" | Alkalmazás típus leépítése biztonsági konfigurációját. |
| MoveNextUpgradeDomain |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások az alkalmazásfrissítések explicit frissítési tartománnyal folytatása. |
| ReportUpgradeHealth |karakterlánc, alapértelmezett az "Admin" | Az aktuális frissítés állapota hello alkalmazásfrissítések folytatása biztonsági konfigurációját. |
| ReportHealth |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállításainak konfigurálása állapotát. |
| ProvisionFabric |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások MSI-fájl és/vagy a fürt jegyzékének történő üzembe helyezéséhez. |
| UpgradeFabric |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások fürt frissítések elindító. |
| RollbackFabricUpgrade |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások a fürt frissítési visszaállítása. |
| UnprovisionFabric |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások a MSI-fájl és/vagy a fürt jegyzékének kiépítés megszüntetésére. |
| MoveNextFabricUpgradeDomain |karakterlánc, alapértelmezett az "Admin" | A fürt frissítési Folytatás egy külön frissítési tartomány a biztonsági beállítások. |
| ReportFabricUpgradeHealth |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások a fürt frissítések hello jelenlegi frissítési folyamat folytatása. |
| StartInfrastructureTask |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások az infrastruktúra-feladatok elindítása. |
| FinishInfrastructureTask |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások a befejezési infrastruktúra feladatokhoz. |
| ActivateNode |karakterlánc, alapértelmezett az "Admin" | A csomópont aktiválási biztonsági beállításainak konfigurálása. |
| DeactivateNode |karakterlánc, alapértelmezett az "Admin" | A csomópont inaktiválása biztonsági konfigurációját. |
| DeactivateNodesBatch |karakterlánc, alapértelmezett az "Admin" | Több csomópont inaktiválása biztonsági konfigurációját. |
| RemoveNodeDeactivations |karakterlánc, alapértelmezett az "Admin" | Több csomóponton visszaállítási inaktiválási biztonsági beállításainak konfigurálása. |
| GetNodeDeactivationStatus |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások az inaktiválást állapotának ellenőrzése. |
| NodeStateRemoved |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállításainak konfigurálása csomópont állapota eltávolítva. |
| RecoverPartition |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások partíció helyreállításához. |
| RecoverPartitions |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások partíciók helyreállításához. |
| RecoverServicePartitions |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások szolgáltatáspartíciók helyreállításához. |
| RecoverSystemPartitions |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások rendszer szolgáltatáspartíciók helyreállításához. |
| ReportFault |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállításainak konfigurálása hiba. |
| InvokeInfrastructureCommand |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások az infrastruktúra feladat parancsok. |
| FileContent |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások lemezkép ügyfél fájlátvitel (külső toocluster) tárolja. |
| FileDownload |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások a lemezképet tároló ügyfél fájl letöltése kezdeményezés (külső toocluster). |
| InternalList |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások lemezkép ügyfél fájl list művelet (belső) tárolja. |
| Törlés |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások lemezkép ügyfél törlési művelet tárolja. |
| Feltöltés |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások lemezkép tárolásához ügyfél feltöltési művelete. |
| GetStagingLocation |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások lemezkép átmeneti helyre lekérés ügyfél tárolja. |
| GetStoreLocation |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások lemezkép ügyfél tároló helye lekérése tárolja. |
| NodeControl |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások indítása; leállítása; és a csomópontok újraindítását. |
| CodePackageControl |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások kód csomagok újraindításához. |
| UnreliableTransportControl |karakterlánc, alapértelmezett az "Admin" | Nem megbízható átviteli hozzáadása és eltávolítása a viselkedés a. |
| MoveReplicaControl |karakterlánc, alapértelmezett az "Admin" | Helyezze át a replikát. |
| PredeployPackageToNode |karakterlánc, alapértelmezett az "Admin" | Telepítés előtti api. |
| StartPartitionDataLoss |karakterlánc, alapértelmezett az "Admin" | Partíciók adatvesztés kapott. |
| StartPartitionQuorumLoss |karakterlánc, alapértelmezett az "Admin" | Partíciók a kvórum elvesztése kapott. |
| StartPartitionRestart |karakterlánc, alapértelmezett az "Admin" | Néhány vagy az összes partíció hello replikák egyidejűleg újraindul. |
| CancelTestCommand |karakterlánc, alapértelmezett az "Admin" | Egy adott TestCommand - megszakítja a felhőszolgáltató közötti átviteléhez esetén. |
| StartChaos |karakterlánc, alapértelmezett az "Admin" | Chaos - elindul, ha nincs elindítva. |
| StopChaos |karakterlánc, alapértelmezett az "Admin" | Leállítja Chaos - elindítása. |
| StartNodeTransition |karakterlánc, alapértelmezett az "Admin" | Biztonsági beállítások csomópont átmenet indításához. |
| StartClusterConfigurationUpgrade |karakterlánc, alapértelmezett az "Admin" | Kapott StartClusterConfigurationUpgrade partícióra. |
| GetUpgradesPendingApproval |karakterlánc, alapértelmezett az "Admin" | Kapott GetUpgradesPendingApproval partícióra. |
| StartApprovedUpgrades |karakterlánc, alapértelmezett az "Admin" | Kapott StartApprovedUpgrades partícióra. |
| Ping |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Ügyfél ping-üzenetek biztonsági beállításainak konfigurálása. |
| Lekérdezés |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások a lekérdezések. |
| NameExists |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | A biztonsági konfiguráció elnevezési URI meglétét ellenőrzi. |
| EnumerateSubnames |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások elnevezési URI számbavétel. |
| EnumerateProperties |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások elnevezési tulajdonság enumerálása. |
| PropertyReadBatch |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások elnevezési tulajdonság olvasási műveleteket. |
| GetServiceDescription |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások szolgáltatás hosszú-lekérdezési értesítéseket és a szolgáltatások ismertetése olvasásakor. |
| ResolveService |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások a megfelelő szolgáltatásra feloldásához. |
| ResolveNameOwner |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások kapcsolatos elnevezési URI tulajdonosa. |
| ResolvePartition |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások rendszerszolgáltatások megoldása. |
| ServiceNotifications |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások eseményalapú szolgáltatási értesítésekhez. |
| PrefixResolveService |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások a megfelelő szolgáltatásra előtag feloldásához. |
| GetUpgradeStatus |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások a lekérdezés alkalmazás frissítési állapot. |
| GetFabricUpgradeStatus |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások a lekérdezés fürt frissítési állapot. |
| InvokeInfrastructureQuery |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások infrastruktúrakezelési feladatok lekérdezése. |
| Lista |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Biztonsági beállítások lemezkép ügyfél fájl list művelet tárolja. |
| Resetpartitionload függvényhez |karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Egy failoverUnit alaphelyzetbe állítása betöltésének biztonsági beállításainak konfigurálása. |
| ToggleVerboseServicePlacementHealthReporting | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Részletes ServicePlacement HealthReporting való átváltással biztonsági beállításainak konfigurálása. |
| GetPartitionDataLossProgress | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Lekéri az adatok elvesztését invoke api-hívások hello folyamatban. |
| GetPartitionQuorumLossProgress | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Lekéri a kvórum elvesztése invoke api-hívások hello folyamatban. |
| GetPartitionRestartProgress | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Lekéri a hello folyamatban van egy újraindítás partíció api-hívások. |
| GetChaosReport | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Lekéri egy adott időtartományban hello Chaos állapotát. |
| GetNodeTransitionProgress | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Folyamatban van a csomópont átmenet parancs helyezésének biztonsági konfigurációját. |
| GetClusterConfigurationUpgradeStatus | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Kapott GetClusterConfigurationUpgradeStatus partícióra. |
| GetClusterConfiguration | karakterlánc, alapértelmezett értéke "Admin\|\|Felhasználó" | Kapott GetClusterConfiguration partícióra. |

### <a name="section-name-reconfigurationagent"></a>Szakasz Name: ReconfigurationAgent
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| ApplicationUpgradeMaxReplicaCloseDuration | Idő (másodpercben), alapértelmezett érték a 900 |Adja meg az időtartam másodpercben. hello időtartama, mely hello a rendszer vár, mielőtt leállítja az üzemeltetett szolgáltatások azzal, amelyek rendelkeznek a replikák mappában a Bezárás gombra. |
| ServiceApiHealthDuration | Idő (másodpercben), alapértelmezett értéke 30 perc | Adja meg az időtartam másodpercben. ServiceApiHealthDuration határozza meg, hogy mennyi ideig tegye azt várni egy service API toorun azt jelentse be a nem megfelelő. |
| ServiceReconfigurationApiHealthDuration | Idő (másodpercben), az alapértelmezett érték 30 | Adja meg az időtartam másodpercben. ServiceReconfigurationApiHealthDuration határozza meg, hogy mennyi ideig hello előtt egy szolgáltatás újrakonfigurálása a nem megfelelő állapotúként jelentette. |
| PeriodicApiSlowTraceInterval | Idő (másodpercben), az alapértelmezett érték 5 perc | Adja meg az időtartam másodpercben. PeriodicApiSlowTraceInterval hello időköz, amelyben lassú API-hívásokat fog szerepelnie hello API figyelő által határozza meg. |
| NodeDeactivationMaxReplicaCloseDuration | Idő (másodpercben), alapértelmezett érték a 900 | Adja meg az időtartam másodpercben. hello maximális idő toowait, amely blokkolja a csomópont inaktiválása szolgáltatásgazda megszakítása előtt. |
| FabricUpgradeMaxReplicaCloseDuration | Idő (másodpercben), alapértelmezett érték a 900 | Adja meg az időtartam másodpercben. hello maximális időtartam RA vár, mielőtt leállítja az adatszolgáltatás gazdájának, amely nem bezárja a replika. |
| IsDeactivationInfoEnabled | Logikai érték, az alapértelmezett érték true | Határozza meg e RA használja majd az inaktiválást adatai, elsődleges újbóli választás az új fürtök ebben a konfigurációban kell beállítani, amelyek frissítik a meglévő fürtök tootrue végrehajtásához adja meg, hogyan hello kibocsátási megjegyzések tooenable ez. |

### <a name="section-name-placementandloadbalancing"></a>Szakasz Name: PlacementAndLoadBalancing
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| TraceCRMReasons |Logikai érték, az alapértelmezett érték true |Meghatározza, hogy CRM okát tootrace kiadott típusú áthelyezések toohello működési eseményeit csatorna. |
| ValidatePlacementConstraint | Logikai érték, az alapértelmezett érték true | Itt adhatja meg, függetlenül attól, hello szolgáltatás PlacementConstraint kifejezése van hitelesítve, amikor frissül egy szolgáltatás ServiceDescription leírásban. |
| PlacementConstraintValidationCacheSize | Int, az alapértelmezett beállítás 10000 | Korlátok hello hello tábla gyors érvényesítésének és elhelyezési korlátozás kifejezések gyorsítótár méretét. |
|VerboseHealthReportLimit | Int, alapértelmezett érték 20 | Meghatározza a hello ennyiszer replika helyezett toogo rendelkezik, mielőtt egy állapotfigyelési figyelmeztetése azt jelentette, (Ha részletes állapotfigyelő reporting engedélyezve van). |
|ConstraintViolationHealthReportLimit | Int, alapértelmezett érték 50 | Hello ennyiszer korlátozás megsértése replika rendelkezik toobe tartósan megoldatlan diagnosztika végzik, és a rendszerállapot-jelentések kibocsátott előtt határozza meg. |
|DetailedConstraintViolationHealthReportLimit | Int, alapértelmezett érték 200 | Hello ennyiszer korlátozás megsértése replika rendelkezik toobe tartósan megoldatlan diagnosztika végzik, és részletes állapotjelentések kibocsátott előtt határozza meg. |
|DetailedVerboseHealthReportLimit | Int, alapértelmezett érték 200 | Hello szám, ahányszor egy helyezett replika rendelkezik olyan toobe tartósan helyezett előtt jelentések kibocsátott részletes állapota határozza meg. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, alapértelmezett érték 20 | Egymást követő alkalommal előtt végzik a diagnosztika és állapotfigyelő figyelmeztetések kibocsátott eldobott áthelyezések ResourceBalancer által kiállított hello számát határozza meg. Negatív: Nincs figyelmeztetések kibocsátott azzal a feltétellel. |
|DetailedNodeListLimit | Int, alapértelmezett érték 15 | Definiálja a korlátozás tooinclude előtt csonkolása csomópontok száma hello hello helyezett replika jelenti. |
|DetailedPartitionListLimit | Int, alapértelmezett érték 15 | Definiálja a diagnosztikai bejegyzésenként egy korlátozás tooinclude csonkolása előtt a partíciók száma hello diagnosztika. |
|DetailedDiagnosticsInfoListLimit | Int, alapértelmezett érték 15 | Határozza meg a hello számú diagnosztikai bejegyzést (részletes adatokkal) / megkötés tooinclude csonkolása lévő diagnosztika előtt.|
|PLBRefreshGap | Idő (másodpercben), az alapértelmezett érték 1 | Adja meg az időtartam másodpercben. Meghatározza a hello minimális, hogy mennyi ideig kell telnie PLB újra frissíti az állapotot. |
|MinPlacementInterval | Idő (másodpercben), az alapértelmezett érték 1 | Adja meg az időtartam másodpercben. Hello két egymást követő elhelyezési kerekítés előtt eltelt minimális mérete határozza meg. |
|MinConstraintCheckInterval | Idő (másodpercben), az alapértelmezett érték 1 | Adja meg az időtartam másodpercben. Hello minimális, hogy mennyi ideig kell telnie két egymást követő megkötés ellenőrizze kerekítés határozza meg. |
|MinLoadBalancingInterval | Idő (másodpercben), az alapértelmezett érték 5 | Adja meg az időtartam másodpercben. Hello két egymást követő terheléselosztási kerekítés előtt eltelt minimális mérete határozza meg. |
|BalancingDelayAfterNodeDown | Idő (másodpercben), alapértelmezett érték a 120 |Adja meg az időtartam másodpercben. Nem indulnak el tevékenységek terheléselosztás ez idő alatt a csomópont esemény után. |
|BalancingDelayAfterNewNode | Idő (másodpercben), alapértelmezett érték a 120 |Adja meg az időtartam másodpercben. Nem indulnak el tevékenységeket végző új csomópont hozzáadása után ez idő alatt. |
|ConstraintFixPartialDelayAfterNodeDown | Idő (másodpercben), alapértelmezett érték a 120 | Adja meg az időtartam másodpercben. Hajtsa végre a nem Fix FaultDomain és UpgradeDomain korlátozások megsértésének ez idő alatt a csomópont esemény után. |
|ConstraintFixPartialDelayAfterNewNode | Idő (másodpercben), alapértelmezett érték a 120 | Adja meg az időtartam másodpercben. DDo FaultDomain nem javítsa ki és UpgradeDomain korlátozások megsértésének új csomópont hozzáadása után ez idő alatt. |
|GlobalMovementThrottleThreshold | Uint, alapértelmezett érték 1000 | Típusú áthelyezések engedélyezett maximális száma hello terheléselosztás fázis a múltbeli hello időköz GlobalMovementThrottleCountingInterval jelölik. |
|GlobalMovementThrottleThresholdForPlacement | Uint, alapértelmezett érték a 0 | Típusú áthelyezések maximális száma engedélyezett elhelyezési fázis a múltbeli hello GlobalMovementThrottleCountingInterval.0 által jelzett időköz korlátozás nélküli állapotot jelzi.|
|GlobalMovementThrottleThresholdForBalancing | Uint, alapértelmezett érték a 0 | Típusú áthelyezések múltbeli hello fázisban terheléselosztás engedélyezett maximális száma időköz GlobalMovementThrottleCountingInterval jelölik. 0 a korlátozás nélküli állapotot jelzi. |
|GlobalMovementThrottleCountingInterval | Idő (másodpercben), az alapértelmezett érték 600 | Adja meg az időtartam másodpercben. Hello hello hosszát jelzik túli mely tootrack (GlobalMovementThrottleThreshold párhuzamosan használva) tartomány replika áthelyezések száma intervallumát. Beállítható too0 tooignore globális szabályozás regisztrálását. |
|MovementPerPartitionThrottleThreshold | Uint, alapértelmezett érték 50 | Nincs terheléselosztás kapcsolódó mozgás egy partíció fog létrejönni, ha a kapcsolódó mozgásának replikákra vonatkozó adott partíció terheléselosztási hello száma elérte vagy meghaladta MovementPerFailoverUnitThrottleThreshold az elmúlt hello által jelzett időköz MovementPerPartitionThrottleCountingInterval. |
|MovementPerPartitionThrottleCountingInterval | Idő (másodpercben), az alapértelmezett érték 600 | Adja meg az időtartam másodpercben. Hello hello hosszát jelzik túli mely tootrack replika típusú áthelyezések (MovementPerPartitionThrottleThreshold párhuzamosan használva) minden partíció intervallumát. |
|PlacementSearchTimeout | Idő (másodpercben), az alapértelmezett érték 0,5 | Adja meg az időtartam másodpercben. Amikor az szolgáltatások; Keresse meg a hosszú, legfeljebb eredményt visszatérése előtt. |
|UseMoveCostReports | Logikai érték, alapértelmezett értéke "false" | Arra utasítja a hello LB tooignore hello költség eleme hello pontozó függvény; jobb kiegyensúlyozott elhelyezésre helyezi át a létrejövő potenciálisan nagy száma. |
|PreventTransientOvercommit | Logikai érték, alapértelmezett értéke "false" | Meghatározza, hogy PLB azonnal számolja olyan erőforrásokon használható, szabadul fel által kezdeményezett hello helyezi át. Alapértelmezett; PLB kilép a kezdeményezése, és helyezze át, amely ugyanahhoz a csomóponthoz, amelyek átmeneti hozhat létre teszi hello. A beállítás megakadályozza, hogy ez a paraméter tootrue azok milyen overcommits és igény szerinti töredezettségmentesítés (más néven placementWithMove) le lesz tiltva. |
|InBuildThrottlingEnabled | Logikai érték, alapértelmezett értéke "false" | Határozza meg, hogy engedélyezve legyen-e a hello beépített sávszélesség-szabályozás. |
|InBuildThrottlingAssociatedMetric | karakterlánc, alapértelmezett érték a "" | hello tartozó metrika neve a szabályozás. |
|InBuildThrottlingGlobalMaxValue | Int, alapértelmezett érték a 0 |hello található beépített replikák globálisan engedélyezett maximális száma. |
|SwapPrimaryThrottlingEnabled | Logikai érték, alapértelmezett értéke "false"| Határozza meg, hogy engedélyezve legyen-e a hello swap elsődleges sávszélesség-szabályozás. |
|SwapPrimaryThrottlingAssociatedMetric | karakterlánc, alapértelmezett érték a ""| hello tartozó metrika neve a szabályozás. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, alapértelmezett érték a 0 | hello található swap elsődleges replikák globálisan engedélyezett maximális száma. |
|PlacementConstraintPriority | Int, alapértelmezett érték a 0 | Meghatározza, hogy hello prioritású virtuális gép elhelyezési korlátozás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|PreferredLocationConstraintPriority | Int, alapértelmezett érték a 2. régiója| Meghatározza az előnyben részesített földrajzi megszorítás hello prioritását: 0: rögzített; 1: enyhe; 2: optimalizálási; negatív: figyelmen kívül hagyása |
|CapacityConstraintPriority | Int, alapértelmezett érték a 0 | Meghatározza, hogy a kapacitás megkötés hello prioritás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|AffinityConstraintPriority | Int, alapértelmezett érték a 0 | Meghatározza, hogy a kapcsolat megkötés hello prioritás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|FaultDomainConstraintPriority | Int, alapértelmezett érték a 0 | Meghatározza, hogy a tartalék tartomány korlátozás hello prioritás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|UpgradeDomainConstraintPriority | Int, alapértelmezett érték 1| Meghatározza, hogy a frissítési tartomány megkötés hello prioritás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|ScaleoutCountConstraintPriority | Int, alapértelmezett érték a 0 | Meghatározza, hogy scaleout száma megkötés hello prioritás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|ApplicationCapacityConstraintPriority | Int, alapértelmezett érték a 0 | Meghatározza, hogy a kapacitás megkötés hello prioritás: 0: rögzített; 1: enyhe; negatív: figyelmen kívül hagyja. |
|MoveParentToFixAffinityViolation | Logikai érték, alapértelmezett értéke "false" | Toofix affinitás korlátozások beállítást, amely meghatározza, hogy ha a szülő replikák lehet áthelyezni.|
|MoveExistingReplicaForPlacement | Logikai érték, az alapértelmezett érték true |Beállítást, amely meghatározza, hogy meglévő replika toomove az Elhelyezés során. |
|UseSeparateSecondaryLoad | Logikai érték, az alapértelmezett érték true | Beállítás, amely meghatározza, hogy ha eltérő másodlagos terhelési használja. |
|PlaceChildWithoutParent | Logikai érték, az alapértelmezett érték true | Beállítás, amely meghatározza, hogy az alárendelt szolgáltatás replika helyezhető be nincs szülő replika esetén. |
|PartiallyPlaceServices | Logikai érték, az alapértelmezett érték true | Meghatározza, hogy ha a fürt összes szolgáltatás replika "mindent vagy semmit" kerülnek-e a megadott korlátozott megfelelő csomópontok számukra.|
|InterruptBalancingForAllFailoverUnitUpdates | Logikai érték, alapértelmezett értéke "false" | Azt határozza meg, ha feladatátvételi egységek frissítés bármely típusú megszakítási gyors vagy lassú terheléselosztás futtatásához. A megadott, "false" terheléselosztás futtatása megszakad, ha FailoverUnit: van létrehozni vagy törölni; hiányoznak a replikák; módosította elsődleges replika helyére vagy replikák száma megváltozott. Terheléselosztás futtatása nem szakad más esetben – ha FailoverUnit: extra replikák; bármely replika jelző; megváltozott csak a partíció-es vagy bármely egyéb megváltozott. |

### <a name="section-name-security"></a>Szakasz Name: biztonsági
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| ClusterProtectionLevel |Nincs vagy EncryptAndSign |Nincs (alapértelmezett) nem biztonságos fürtök, biztonságos fürtök EncryptAndSign. |

### <a name="section-name-hosting"></a>Szakasz Name: üzemeltetéséhez
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| ServiceTypeRegistrationTimeout |Idő (másodpercben), az alapértelmezett érték 300 |Hello ServiceType toobe háló regisztrált megengedett maximális időtartamot |
| ServiceTypeDisableFailureThreshold |Egész szám, alapértelmezett értéke 1 |Ez a hello küszöbértéket, a hello hiba száma, amely után FailoverManager (FM) van az értesítés toodisable hello szolgáltatás típusa, hogy csomópontot, majd próbálkozzon egy másik csomópont elhelyezésre. |
| ActivationRetryBackoffInterval |Idő (másodpercben), az alapértelmezett érték 5 |Visszalépési időköz minden aktiválási hiba esetén; Minden folyamatos aktiválási hiba esetén hello rendszer újrapróbálkozások hello toohello MaxActivationFailureCount be az aktiválási. hello újrapróbálkozási időköz minden kísérlet rendszerhez nevű szoftver a folyamatos aktiválási hiba és hello aktiválási vissza az indító időköz. |
| ActivationMaxRetryInterval |Idő (másodpercben), az alapértelmezett érték 300 |Minden folyamatos aktiválási hiba esetén hello rendszer újrapróbálkozások hello tooActivationMaxFailureCount fel az aktiválást. ActivationMaxRetryInterval Megadja azt a várakozási időközt újrapróbálkozás előtti, minden aktiválási hiba után |
| ActivationMaxFailureCount |Egész szám, alapértelmezett érték 10 |Ennyiszer rendszer újrapróbálkozások sikertelenek, mielőtt aktiválás |

### <a name="section-name-failovermanager"></a>Szakasz Name: FailoverManager
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| PeriodicLoadPersistInterval |Idő (másodpercben), az alapértelmezett érték 10 |Ez határozza meg, milyen gyakran hello új terheléselosztási jelentések FM ellenőrzése |

### <a name="section-name-federation"></a>Szakasz Name: összevonási
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| LeaseDuration |Idő (másodpercben), az alapértelmezett érték 30 |A címbérlet tart egy csomópont és a szomszédjaihoz csatlakoztatni közötti időtartamot. |
| LeaseDurationAcrossFaultDomain |Idő (másodpercben), az alapértelmezett érték 30 |Egy csomópont és a szomszédjaihoz csatlakoztatni között tartalék tartományokban tovább tartó a címbérlet időtartama. |

### <a name="section-name-clustermanager"></a>Szakasz Name: ClusterManager
| **A paraméter** | **Megengedett értékek** | **Útmutatás vagy rövid leírása** |
| --- | --- | --- |
| UpgradeStatusPollInterval |Idő (másodpercben), az alapértelmezett érték 60 |a frissítési alkalmazás állapotának lekérdezése hello gyakorisága. Ez az érték határozza meg a frissítés bármely GetApplicationUpgradeProgress hívás hello aránya |
| UpgradeHealthCheckInterval |Idő (másodpercben), az alapértelmezett érték 60 |állapot hello gyakorisága ellenőrzi a figyelt alkalmazás frissítéskor |
| FabricUpgradeStatusPollInterval |Idő (másodpercben), az alapértelmezett érték 60 |a Fabric frissítésének állapota lekérdezése hello gyakorisága. Ez az érték határozza meg a frissítés bármely GetFabricUpgradeProgress hívás hello aránya |
| FabricUpgradeHealthCheckInterval |Idő (másodpercben), az alapértelmezett érték 60 |állapot-ellenőrzést egy figyelt Fabric frissítés során hello gyakorisága |
|InfrastructureTaskProcessingInterval | Idő (másodpercben), az alapértelmezett érték 10 |Adja meg az időtartam másodpercben. hello feldolgozási időköze hello infrastruktúra feladat feldolgozása állapotjelző gép használják. |
|TargetReplicaSetSize |Int, alapértelmezett érték 7 |hello ClusterManager a TargetReplicaSetSize. |
|MinReplicaSetSize |Int, alapértelmezett érték 3 |hello ClusterManager a MinReplicaSetSize. |
|ReplicaRestartWaitDuration |Idő (másodpercben), az alapértelmezett érték (60.0 * 30)|Adja meg az időtartam másodpercben. hello ReplicaRestartWaitDuration ClusterManager számára. |
|QuorumLossWaitDuration |Idő (másodpercben), alapértelmezett érték a MaxValue | Adja meg az időtartam másodpercben. hello QuorumLossWaitDuration ClusterManager számára. |
|StandByReplicaKeepDuration | Idő (másodpercben), az alapértelmezett érték (3600.0 * 2)|Adja meg az időtartam másodpercben. hello StandByReplicaKeepDuration ClusterManager számára. |
|PlacementConstraints | karakterlánc, alapértelmezett érték a "" |hello PlacementConstraints ClusterManager számára. |
|SkipRollbackUpdateDefaultService | Logikai érték, alapértelmezett értéke "false" |hello CM kihagyja a visszaállítási frissített alapértelmezett szolgáltatások alkalmazás frissítési-visszavonás során. |
|EnableDefaultServicesUpgrade | Logikai érték, alapértelmezett értéke "false" |Frissíti az alapértelmezett szolgáltatások engedélyezése az alkalmazásfrissítés során. Alapértelmezett szolgáltatások leírásai volna felülírja a frissítés után. |
|InfrastructureTaskHealthCheckWaitDuration |Idő (másodpercben), az alapértelmezett érték 0| Adja meg az időtartam másodpercben. hello a mennyisége idő toowait állapotellenőrzést után utófeldolgozási egy infrastruktúra-feladat indítása előtt. |
|InfrastructureTaskHealthCheckStableDuration | Idő (másodpercben), az alapértelmezett érték 0| Adja meg az időtartam másodpercben. hello mennyisége idő tooobserve egymást követő átadott állapotát ellenőrzi, mielőtt utófeldolgozás egy infrastruktúra-feladat futtatása sikeresen befejeződött. Megfigyelő állapotellenőrzése nem sikerült Ez az időzítő alaphelyzetbe állnak. |
|InfrastructureTaskHealthCheckRetryTimeout | Idő (másodpercben), az alapértelmezett érték 60 |Adja meg az időtartam másodpercben. hello mennyi idő toospend tett sikertelen állapotellenőrzést infrastruktúra feladatról utáni feldolgozása közben. Átadott állapotellenőrzése betartásával Ez az időzítő alaphelyzetbe állnak. |
|ImageBuilderTimeoutBuffer |Idő (másodpercben), az alapértelmezett érték 3 |Adja meg az időtartam másodpercben. Az Image Builder meghatározott időtúllépési hibák tooreturn toohello ügyfél idő tooallow hello mennyisége. Ha a puffer túl kicsi. majd hello ügyfél előtt hello kiszolgálói időtúllépés, és lekérdezi egy általános időtúllépési hiba. |
|MinOperationTimeout | Idő (másodpercben), az alapértelmezett érték 60 |Adja meg az időtartam másodpercben. hello minimális globális időkorlátjának belső feldolgozási ClusterManager műveleteket. |
|Konfigurált MaxOperationTimeout |Idő (másodpercben), alapértelmezett érték a MaxValue | Adja meg az időtartam másodpercben. hello maximális globális időkorlátjának belső feldolgozási ClusterManager műveleteket. |
|MaxTimeoutRetryBuffer | Idő (másodpercben), az alapértelmezett érték 600 |Adja meg az időtartam másodpercben. maximális művelet időtúllépése hello megkísérlésekor belső esedékes tootimeouts van <Original Timeout>  +  <MaxTimeoutRetryBuffer>. További időtúllépés MinOperationTimeout lépésekben kerül. |
|MaxCommunicationTimeout |Idő (másodpercben), az alapértelmezett érték 600 |Adja meg az időtartam másodpercben. belső kommunikációs ClusterManager és egyéb rendszerszolgáltatások közötti maximális időkorlátjának hello (azaz; A Naming Service; Feladatátvevőfürt-kezelő és stb). Ez az időkorlát kisebb, mint a globális konfigurált MaxOperationTimeout (mivel előfordulhat, hogy minden ügyfél művelethez rendszerösszetevők közötti több kommunikáció) kell lennie. |
|MaxDataMigrationTimeout |Idő (másodpercben), az alapértelmezett érték 600 |Adja meg az időtartam másodpercben. hello maximális időtúllépés áttelepítési helyreállítási műveletek után a Fabric frissítése megtörtént. |
|MaxOperationRetryDelay |Idő (másodpercben), az alapértelmezett érték 5| Adja meg az időtartam másodpercben. hibák vannak fellépő belső újrapróbálkozások késleltetési hello. |
|ReplicaSetCheckTimeoutRollbackOverride |Idő (másodpercben), az alapértelmezett érték 1200 | Adja meg az időtartam másodpercben. Ha ReplicaSetCheckTimeout toohello DWORD; maximális értéke Ezután azt felülbírálja a visszaállítási hello alkalmazásában config hello értékű. soha nem használt előregörgetésre hello érték felülbírálja. |
|ImageBuilderJobQueueThrottle |Int, alapértelmezett érték 10 |Az Image Builder proxy feladat-várólistán alkalmazás kérelmek száma késleltetési szál. |

## <a name="next-steps"></a>Következő lépések
Ezek a cikkek további információt a kiszolgálófürt-felügyelet olvasható:

[Adja hozzá, a át, a tanúsítványok eltávolítása az Azure-fürttel](service-fabric-cluster-security-update-certs-azure.md) 

