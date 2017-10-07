---
title: "az Azure mikroszolgáltatások aaaChange ReliableDictionaryActorStateProvider beállítások |} Microsoft Docs"
description: "Azure Service Fabric állapot-nyilvántartó szereplője ReliableDictionaryActorStateProvider típusú beállításának ismertetése."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: 79b48ffa-2474-4f1c-a857-3471f9590ded
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: 44c85a41c90a17669ba874401d7921c94e7be9ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Reliable Actors--ReliableDictionaryActorStateProvider konfigurálása
Hello alapértelmezett konfigurációja ReliableDictionaryActorStateProvider hello settings.xml fájlban hello Visual Studio gyökér package objektum hello Config mappában a hello megadott szereplő hozott létre módosításával módosíthatja.

hello Azure Service Fabric-futtatókörnyezet megkeresi az előre definiált szakaszneveket hello settings.xml fájlban, és hello konfigurációs értékeket fel az alapul szolgáló futásidejű összetevők hello létrehozása során.

> [!NOTE]
> Tegye **nem** törölni vagy módosítani a hello szakaszneveket, a következő konfigurációk hello settings.xml fájlban, a Visual Studio megoldás hello generált hello.
> 
> 

A ReliableDictionaryActorStateProvider hello konfigurációjára hatással levő globális beállítások is vannak.

## <a name="global-configuration"></a>A globális konfiguráció
hello globális konfiguráció van megadva a fürtjegyzékben hello hello fürt hello KtlLogger szakasz alatt. Lehetővé teszi a megosztott hello napló helye és mérete plusz hello globális memóriakorlátokat hello naplózó által használt konfigurációját. Vegye figyelembe, hogy hello fürtjegyzékben hatással vannak ReliableDictionaryActorStateProvider használó összes szolgáltatást és megbízható állapotalapú szolgáltatások.

hello fürtjegyzékben, amely tárolja a beállításokat és konfigurációkat, amelyek érvényesek a tooall csomópontok és a szolgáltatások hello fürt egyetlen XML-fájl. hello fájl neve általában ClusterManifest.xml. Megtekintheti a Get-ServiceFabricClusterManifest hello powershell-paranccsal fürt hello a fürtjegyzékben.

### <a name="configuration-names"></a>Konfigurációs nevek
| Név | Unit (Egység) | Alapértelmezett érték | Megjegyzések |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobájt |8388608 |Minimális száma a rendszermag módú hello naplózó a KB tooallocate írható memória pufferkészlet. Memóriakészlet használható gyorsítótárazás állapotadatokat toodisk írása előtt. |
| WriteBufferMemoryPoolMaximumInKB |Kilobájt |Korlátlan |Maximális méret toowhich hello naplózó írási puffer memóriakészletben növelhető. |
| SharedLogId |GUID |"" |Adja meg egy egyedi GUID toouse hello alapértelmezett megosztott naplófájl hello fürt összes csomópontján, amely a szolgáltatás adott konfigurációban hello SharedLogId nem adja meg az összes megbízható szolgáltatás által használt azonosító. Ha SharedLogId meg van adva, majd SharedLogPath is kötelező. |
| SharedLogPath |Teljes elérési útja |"" |Megadja a hello ahol hello megosztott hello fürt összes csomópontján, amely a szolgáltatás adott konfigurációban hello SharedLogPath nem adja meg az összes megbízható szolgáltatás által használt naplófájl teljes elérési útja. Azonban ha SharedLogPath meg van adva, majd SharedLogId is kötelező. |
| SharedLogSizeInMB |Mérete (MB) |8192 |Hello számát adja meg MB szabad lemezterületre toostatically hello megosztott napló lefoglalni. hello érték 2048 vagy nagyobb lehet. |

### <a name="sample-cluster-manifest-section"></a>A minta fürtöt manifest szakasz
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Megjegyzések
hello naplózó rendelkezik számára lefoglalt nem lapozható kernelmemória, amely elérhető tooall megbízható szolgáltatások-csomóponton lévő adatok gyorsítótárazása toohello hello szolgáltatás replika társított dedikált napló írása előtt a globális készlet. hello készletméretet hello WriteBufferMemoryPoolMinimumInKB és WriteBufferMemoryPoolMaximumInKB beállításait vezérli. WriteBufferMemoryPoolMinimumInKB határozza meg, mindkét hello memóriakészlet kezdeti méretét és hello legkisebb mérete toowhich hello memóriakészletben csökkentheti. A rendszer WriteBufferMemoryPoolMaximumInKB hello legnagyobb mérete toowhich hello memóriakészletben is növekszik. Minden megbízható szolgáltatás-replikával, amely meg van nyitva mentése tooWriteBufferMemoryPoolMaximumInKB megállapítása szerint a rendszer összeggel növelheti hello memóriakészletben hello méretét. Ha további igény szerinti hello memóriakészletben érhető el, mint a memória, memória kérelmek program elhalasztja mindaddig, amíg elérhető memória. Ezért ha hello írási puffer memóriakészletben kicsi egy speciális konfigurációja, majd a teljesítmény romolhat.

hello SharedLogId és SharedLogPath beállítások mindig használt együtt toodefine hello GUID és hello alapértelmezett helye megosztott napló hello fürt összes csomópontján. hello alapértelmezett megosztott napló szolgál, amelyek nem adnak meg hello beállítások hello settings.xml hello adott szolgáltatás az összes megbízható szolgáltatás. A legjobb teljesítmény érdekében megosztott hello napló fájl tooreduce versengés kizárólag a használt lemezeket megosztott naplófájlok kell elhelyezni.

SharedLogSizeInMB ennyi hello szabad terület toopreallocate hello alapértelmezett megosztott napló minden csomóponton.  SharedLogId és SharedLogPath nem kell ahhoz, hogy a megadott SharedLogSizeInMB toobe megadott toobe.

## <a name="replicator-security-configuration"></a>A replikáló biztonsági konfiguráció
A replikáló biztonsági beállításokkal használt toosecure hello kommunikációs csatornát replikáció során használt. Ez azt jelenti, hogy a szolgáltatások egymás replikációs forgalom nem látható, magas rendelkezésre állási biztosítja hello adatok egyben biztonságos.
Alapértelmezés szerint egy üres biztonsági konfigurációs szakasz megakadályozza, hogy a replikációs biztonságot.

### <a name="section-name"></a>A szakasz neve
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikációs konfiguráció
Replikációs beállítások használt tooconfigure hello replikátor, amely feladata, hogy nagymértékben megbízható hello szereplő Állapotszolgáltató állapot replikálásához és hello állapot helyi megőrzése.
hello alapértelmezett konfiguráció hello Visual Studio-sablon által generált és elegendőnek kell lennie. Ez a szakasz olyan beállításokat, amelyeket a rendszer rendelkezésre álló tootune hello replikátor beszél.

### <a name="section-name"></a>A szakasz neve
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurációs nevek
| Név | Unit (Egység) | Alapértelmezett érték | Megjegyzések |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |másodperc |0.015 |Időszakát mely hello replikátor: hello másodlagos vár a művelet előtt küld vissza egy nyugtázási toohello elsődleges fogadása után. Más nyugták toobe intervallum feldolgozott műveletek küldött küldése a egy választ. |
| ReplicatorEndpoint |N/A |Nincs alapértelmezett érték – a kötelező paraméter: |IP-címet és portot, amelyen az elsődleges és másodlagos replikátor hello más gyártóitól hello replika toocommunicate fog használni. Ez hivatkoznia kell a TCP-erőforrás végpont hello szolgáltatás jegyzékben. Tekintse meg a túl[Service manifest erőforrások](service-fabric-service-manifest-resources.md) tooread több végpont erőforrások definiálása szolgáltatás jegyzékben. |
| (Maxreplicationmessagesize). |Bájtok |50 MB |Replikációs adatok egyetlen üzenetben továbbítható maximális mérete. |
| MaxPrimaryReplicationQueueSize |Műveletek száma |8192 |Hello elsődleges várólistában lévő műveletek maximális száma. Egy műveletet a fel nem szabadul, miután hello elsődleges replikátor nyugtázást fogad az összes hello másodlagos gyártóitól. Ez az érték 64 és 2 valamelyik hatványa nagyobbnak kell lennie. |
| MaxSecondaryReplicationQueueSize |Műveletek száma |16384 |A másodlagos várólista hello műveletek maximális száma. Egy művelet fel nem szabadul állapotában adatmegőrzési keresztül magas rendelkezésre állású elvégzése után. Ez az érték 64 és 2 valamelyik hatványa nagyobbnak kell lennie. |
| CheckpointThresholdInMB |MB |200 |Hello állapot alkulcsaihoz fájl naplóterület mennyisége. |
| MaxRecordSizeInKB |KB |1024 |Legnagyobb rekord mérete, amely a replikátor hello írhat hello naplóban. Ez az érték nagyobb, mint 16 és 4 többszörösének kell lennie. |
| OptimizeLogForLowerDiskUsage |Logikai érték |Igaz |Amikor igaz értékű, hello napló van konfigurálva, hogy hello replika meg dedikált naplófájlt hoz létre egy NTFS-ritka fájl használatával. Ez csökkenti a hello tényleges lemezterület-használat hello fájl. Hamis érték esetén hello fájl rögzített foglalásokat, hello legjobb írási teljesítmény elérése érdekében hozza létre. |
| SharedLogId |GUID |"" |Adja meg egy egyedi guid toouse hello megosztott naplófájlt a replika használt azonosító. Szolgáltatások általában, ne használja ezt a beállítást. Azonban ha SharedLogId meg van adva, majd SharedLogPath is kötelező. |
| SharedLogPath |Teljes elérési útja |"" |Megadja a hello teljes elérési útja, ahol létrejön hello megosztott naplófájl a replikára vonatkozóan. Szolgáltatások általában, ne használja ezt a beállítást. Azonban ha SharedLogPath meg van adva, majd SharedLogId is kötelező. |

## <a name="sample-configuration-file"></a>Minta konfigurációs fájlt
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```

## <a name="remarks"></a>Megjegyzések
hello BatchAcknowledgementInterval paraméterrel állítható be replikációs késés. "0" értéket eredményez hello lehető legkisebb késleltetést, átviteli hello költségekkel (mivel több fel nyugtázási üzeneteket kell küldött és feldolgozásra, kevesebb nyugták tartalmazó).
hello BatchAcknowledgementInterval, nagyobb hello értéket hello magasabb hello teljes replikáció átviteli hello költség magasabb művelet késés mellett. Ez közvetlenül több tranzakciók véglegesítése toohello késését.

hello CheckpointThresholdInMB paraméter vezérlők hello lemezterület nagyságát, amely a replikátor hello használható toostore állapotadatokat hello replika dedikált naplófájlban. Az tooa magasabb érték, mint amikor egy új replika szerepel-e toohello set gyorsabb újrakonfigurálás hello alapértelmezett okozhat növelése. Ez azért van, amely akkor történik meg miatt további előzmények hello naplóban műveletek toohello rendelkezésre állását toohello részleges állapot átviteli miatt. A potenciálisan növelheti az hello helyreállítási időt a replikák rendszerösszeomlás után.

Ha OptimizeForLowerDiskUsage tootrue, napló fájl terület lesz túlzott kiosztott, hogy aktív replikákat tárolhat további állapotadatokat a naplófájlokat, amíg inaktív replikák kevesebb lemezterületet fogja használni. Ez teszi lehetővé toohost további replikák csomóponton. Ha OptimizeForLowerDiskUsage toofalse, hello állapotadatokat gyorsabban toohello naplófájlok van írva.

hello MaxRecordSizeInKB beállítás határozza meg, hogy egy rekordot, amely hello naplófájl hello replikátor által írható hello maximális méretét. A legtöbb esetben hello alapértelmezett 1024 KB-os rekord mérete optimális. Előfordulhat azonban, ha hello szolgáltatás okozza nagyobb adatok elemek toobe része hello állapotát, majd ezt az értéket kell toobe nőtt. Nincs sok előnye létrehozása során MaxRecordSizeInKB kisebb, mint 1024, mint kisebb rekordok csak hello helyigényt hello kisebb rekordhoz. Elvárjuk, hogy ez az érték csak ritkán módosított toobe lenne szükség.

hello SharedLogId és SharedLogPath beállításokat a rendszer mindig használt együtt toomake szolgáltatás használata egy különálló megosztott napló hello alapértelmezett megosztott naplóból hello csomópont. A hatékonyság növelése érdekében a lehető legtöbb szolgáltatások hello adja meg ugyanazt a közös napló. Megosztott naplófájlok hello megosztott naplófájl, tooreduce központi adatátviteli versengés kizárólag a használt lemezen kell elhelyezni. Elvárjuk, hogy ezek az értékek csak ritkán módosított toobe lenne szükség.

