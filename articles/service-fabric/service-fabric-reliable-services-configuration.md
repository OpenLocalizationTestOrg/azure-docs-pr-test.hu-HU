---
title: "megbízható Azure mikroszolgáltatások aaaConfigure |} Microsoft Docs"
description: "További tudnivalók az Azure Service Fabric állapotalapú Reliable Services konfigurálása."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: 1e9c2890b62890a777561f25c04dc0fd11db9f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-stateful-reliable-services"></a>Állapot-nyilvántartó megbízható szolgáltatások konfigurálása
Megbízható szolgáltatások konfigurációs beállításainak két csoportjára van. Egy globális hello fürtben lévő összes megbízható szolgáltatás addig, amíg hello többi adott tooa adott megbízható szolgáltatáshoz.

## <a name="global-configuration"></a>A globális konfiguráció
hello globális megbízható szolgáltatáskonfiguráció hello fürtjegyzékben hello fürt hello KtlLogger szakasz alatt van megadva. Lehetővé teszi a megosztott hello napló helye és mérete plusz hello globális memóriakorlátokat hello naplózó által használt konfigurációját. hello fürtjegyzékben, amely tárolja a beállításokat és konfigurációkat, amelyek érvényesek a tooall csomópontok és a szolgáltatások hello fürt egyetlen XML-fájl. hello fájl neve általában ClusterManifest.xml. Megtekintheti a Get-ServiceFabricClusterManifest hello powershell-paranccsal fürt hello a fürtjegyzékben.

### <a name="configuration-names"></a>Konfigurációs nevek
| Név | Unit (Egység) | Alapértelmezett érték | Megjegyzések |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |Kilobájt |8388608 |Minimális száma a rendszermag módú hello naplózó a KB tooallocate írható memória pufferkészlet. Memóriakészlet használható gyorsítótárazás állapotadatokat toodisk írása előtt. |
| WriteBufferMemoryPoolMaximumInKB |Kilobájt |Korlátlan |Maximális méret toowhich hello naplózó írási puffer memóriakészletben növelhető. |
| SharedLogId |GUID |"" |Adja meg egy egyedi GUID toouse hello alapértelmezett megosztott naplófájl hello fürt összes csomópontján, amely a szolgáltatás adott konfigurációban hello SharedLogId nem adja meg az összes megbízható szolgáltatás által használt azonosító. Ha SharedLogId meg van adva, majd SharedLogPath is kötelező. |
| SharedLogPath |Teljes elérési útja |"" |Megadja a hello ahol hello megosztott hello fürt összes csomópontján, amely a szolgáltatás adott konfigurációban hello SharedLogPath nem adja meg az összes megbízható szolgáltatás által használt naplófájl teljes elérési útja. Azonban ha SharedLogPath meg van adva, majd SharedLogId is kötelező. |
| SharedLogSizeInMB |Mérete (MB) |8192 |Hello számát adja meg MB szabad lemezterületre toostatically hello megosztott napló lefoglalni. hello érték 2048 vagy nagyobb lehet. |

Azure ARM vagy a helyszíni JSON-sablon hello az alábbi példa bemutatja, hogyan toochange hello hello megosztott tranzakciós napló, amely lekérdezi hozza létre a tooback a megbízható gyűjtemények állapotalapú szolgáltatások.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>A minta helyi fejlesztői fürt manifest szakasz
Ha azt szeretné toochange Ez az a helyi fejlesztőkörnyezetet, tooedit hello helyi clustermanifest.xml fájl szükséges.

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>Megjegyzések
hello naplózó rendelkezik számára lefoglalt nem lapozható kernelmemória, amely elérhető tooall megbízható szolgáltatások-csomóponton lévő adatok gyorsítótárazása toohello hello szolgáltatás replika társított dedikált napló írása előtt a globális készlet. hello készletméretet hello WriteBufferMemoryPoolMinimumInKB és WriteBufferMemoryPoolMaximumInKB beállításait vezérli. WriteBufferMemoryPoolMinimumInKB határozza meg, mindkét hello memóriakészlet kezdeti méretét és hello legkisebb mérete toowhich hello memóriakészletben csökkentheti. A rendszer WriteBufferMemoryPoolMaximumInKB hello legnagyobb mérete toowhich hello memóriakészletben is növekszik. Minden megbízható szolgáltatás-replikával, amely meg van nyitva mentése tooWriteBufferMemoryPoolMaximumInKB megállapítása szerint a rendszer összeggel növelheti hello memóriakészletben hello méretét. Ha további igény szerinti hello memóriakészletben érhető el, mint a memória, memória kérelmek program elhalasztja mindaddig, amíg elérhető memória. Ezért ha hello írási puffer memóriakészletben kicsi egy speciális konfigurációja, majd a teljesítmény romolhat.

hello SharedLogId és SharedLogPath beállítások mindig használt együtt toodefine hello GUID és hello alapértelmezett helye megosztott napló hello fürt összes csomópontján. hello alapértelmezett megosztott napló szolgál, amelyek nem adnak meg hello beállítások hello settings.xml hello adott szolgáltatás az összes megbízható szolgáltatás. A legjobb teljesítmény érdekében megosztott hello napló fájl tooreduce versengés kizárólag a használt lemezeket megosztott naplófájlok kell elhelyezni.

SharedLogSizeInMB ennyi hello szabad terület toopreallocate hello alapértelmezett megosztott napló minden csomóponton.  SharedLogId és SharedLogPath nem kell ahhoz, hogy a megadott SharedLogSizeInMB toobe megadott toobe.

## <a name="service-specific-configuration"></a>Adott konfigurációs szolgáltatás
Állapotalapú Reliable Services alapértelmezett konfigurációk módosítása hello konfigurációs csomagot (konfiguráció) használatával, vagy a szolgáltatás megvalósítása (kód) hello.

* **Config** -konfigurációs hello a konfigurációs csomag keresztül valósul hello Settings.xml fájlban hello Microsoft Visual Studio gyökér package objektum minden egyes szolgáltatás hello alkalmazásban hello Config mappában létrehozott módosításával.
* **Kód** -konfigurációs kódot keresztül valósul hozzon létre egy ReliableStateManager egy ReliableStateManagerConfiguration objektum hello megfelelő beállítások használatával.

Alapértelmezés szerint a hello Azure Service Fabric-futtatókörnyezet megkeresi az előre definiált szakaszneveket hello Settings.xml fájlban, és hello konfigurációs értékeket fel az alapul szolgáló futásidejű összetevők hello létrehozása során.

> [!NOTE]
> Tegye **nem** hello szakaszneveket, a következő konfigurációk hello Settings.xml fájlban, kivéve, ha azt tervezi, tooconfigure kód a szolgáltatás hello Visual Studio megoldás létrehozott hello törlése.
> Hello konfigurációs csomag szakasz név vagy átnevezése szükséges kódváltoztatást hello ReliableStateManager konfigurálásakor.
> 
> 

### <a name="replicator-security-configuration"></a>A replikáló biztonsági konfiguráció
A replikáló biztonsági beállításokkal használt toosecure hello kommunikációs csatornát replikáció során használt. Ez azt jelenti, hogy a szolgáltatás nem lesz képes toosee biztonságos egymás replikációs forgalmat, amely biztosítja, hogy magas rendelkezésre állási hello adatokat is. Alapértelmezés szerint egy üres biztonsági konfigurációs szakasz megakadályozza, hogy a replikációs biztonságot.

### <a name="default-section-name"></a>Alapértelmezett szakasz neve
ReplicatorSecurityConfig

> [!NOTE]
> toochange Ez a szakasz a név felülbírálás hello replicatorSecuritySectionName toohello ReliableStateManagerConfiguration konstruktort ReliableStateManager hello szolgáltatás létrehozásakor.
> 
> 

### <a name="replicator-configuration"></a>Replikációs konfiguráció
Replikációs konfiguráció, amely feladata, hogy nagymértékben megbízható hello állapot-nyilvántartó megbízható szolgáltatás állapota replikálásához és hello állapot helyi megőrzése hello replikátor konfigurálása.
hello alapértelmezett konfiguráció hello Visual Studio-sablon által generált és elegendőnek kell lennie. Ez a szakasz olyan beállításokat, amelyeket a rendszer rendelkezésre álló tootune hello replikátor beszél.

### <a name="default-section-name"></a>Alapértelmezett szakasz neve
ReplicatorConfig

> [!NOTE]
> toochange Ez a szakasz a név felülbírálás hello replicatorSettingsSectionName toohello ReliableStateManagerConfiguration konstruktort ReliableStateManager hello szolgáltatás létrehozásakor.
> 
> 

### <a name="configuration-names"></a>Konfigurációs nevek
| Név | Unit (Egység) | Alapértelmezett érték | Megjegyzések |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |másodperc |0.015 |Időszakát mely hello replikátor: hello másodlagos vár a művelet előtt küld vissza egy nyugtázási toohello elsődleges fogadása után. Más nyugták toobe intervallum feldolgozott műveletek küldött küldése a egy választ. |
| ReplicatorEndpoint |N/A |Nincs alapértelmezett érték – a kötelező paraméter: |IP-címet és portot, amelyen az elsődleges és másodlagos replikátor hello más gyártóitól hello replika toocommunicate fog használni. Ez hivatkoznia kell a TCP-erőforrás végpont hello szolgáltatás jegyzékben. Tekintse meg a túl[Service manifest erőforrások](service-fabric-service-manifest-resources.md) tooread végpont erőforrások meghatározása a szolgáltatás jegyzékben többet. |
| MaxPrimaryReplicationQueueSize |Műveletek száma |8192 |Hello elsődleges várólistában lévő műveletek maximális száma. Egy műveletet a fel nem szabadul, miután hello elsődleges replikátor nyugtázást fogad az összes hello másodlagos gyártóitól. Ez az érték 64 és 2 valamelyik hatványa nagyobbnak kell lennie. |
| MaxSecondaryReplicationQueueSize |Műveletek száma |16384 |A másodlagos várólista hello műveletek maximális száma. Egy művelet fel nem szabadul állapotában adatmegőrzési keresztül magas rendelkezésre állású elvégzése után. Ez az érték 64 és 2 valamelyik hatványa nagyobbnak kell lennie. |
| CheckpointThresholdInMB |MB |50 |Hello állapot alkulcsaihoz fájl naplóterület mennyisége. |
| MaxRecordSizeInKB |KB |1024 |Legnagyobb rekord mérete, amely a replikátor hello írhat hello naplóban. Ez az érték nagyobb, mint 16 és 4 többszörösének kell lennie. |
| MinLogSizeInMB |MB |0 (megállapítása szerint a rendszer) |Hello tranzakciós napló legkisebb méretét. hello napló nem engedélyezett tootruncate tooa méret alatt ez a beállítás. 0 azt jelenti, hogy hello replikátor hello minimális naplóméret határozza meg. Az érték növelésével növeli a részleges példányszám és a növekményes biztonsági mentések során óta veszélyét annak, hogy a megfelelő naplófájlok rekordok csonkolva lesz arányában hello lehetőségét. |
| TruncationThresholdFactor |Tényező |2 |Meghatározza, hogy milyen méretben hello napló csonkolási indul. Csonkolási küszöbérték TruncationThresholdFactor megszorozza MinLogSizeInMB határozza meg. TruncationThresholdFactor 1-nél nagyobbnak kell lennie. MinLogSizeInMB * TruncationThresholdFactor MaxStreamSizeInMB kisebbnek kell lennie. |
| ThrottlingThresholdFactor |Tényező |4 |Meghatározza, hogy milyen hello napló mérete, hello replika kezdi szabályozva. Sávszélesség-szabályozási küszöbértéke (MB) határozza meg a maximális ((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)). Sávszélesség-szabályozási küszöbértéke (MB) csonkolása küszöbértéket (megabájtban) nagyobbnak kell lennie. Csonkolási küszöbérték (MB) a MaxStreamSizeInMB kisebbnek kell lennie. |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |Maximális mérete (MB) a megadott biztonságimásolat-naplólánccal rendelkeznek a biztonsági mentési naplók halmozott. Egy növekményes biztonsági mentési kérelem sikertelen lesz, ha hello növekményes biztonsági mentés hoz létre egy biztonsági mentési napló, amelyek halmozott hello biztonsági mentési naplók óta hello kapcsolódó teljes biztonsági mentés toobe nagyobb, mint a mérete. Ilyen esetben a felhasználó az szükséges tootake teljes biztonsági mentés. |
| SharedLogId |GUID |"" |Adja meg egy egyedi GUID toouse hello megosztott naplófájlt a replika használt azonosító. Szolgáltatások általában, ne használja ezt a beállítást. Azonban ha SharedLogId meg van adva, majd SharedLogPath is kötelező. |
| SharedLogPath |Teljes elérési útja |"" |Megadja a hello teljes elérési útja, ahol létrejön hello megosztott naplófájl a replikára vonatkozóan. Szolgáltatások általában, ne használja ezt a beállítást. Azonban ha SharedLogPath meg van adva, majd SharedLogId is kötelező. |
| SlowApiMonitoringDuration |másodperc |300 |Figyelési időköz felügyelt API-hívásokban hello beállítása. Példa: felhasználó által megadott biztonsági mentési visszahívási függvény. Hello lejárta, után egy figyelmeztetés állapotjelentése küld állapotfigyelési toohello. |

### <a name="sample-configuration-via-code"></a>Mintakonfiguráció kód
```csharp
class Program
{
    /// <summary>
    /// This is hello entry point of hello service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Minta konfigurációs fájlt
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
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


### <a name="remarks"></a>Megjegyzések
BatchAcknowledgementInterval replikációs késésétől szabályozza. "0" értéket eredményez hello lehető legkisebb késleltetést, átviteli hello költségekkel (mivel több fel nyugtázási üzeneteket kell küldött és feldolgozásra, kevesebb nyugták tartalmazó).
hello BatchAcknowledgementInterval, nagyobb hello értéket hello magasabb hello teljes replikáció átviteli hello költség magasabb művelet késés mellett. Ez közvetlenül több tranzakciók véglegesítése toohello késését.

CheckpointThresholdInMB vezérlők hello memóriamennyiség, amely a replikátor hello hello értékét használhatja toostore állapotadatokat hello replika dedikált naplófájlban. Az tooa magasabb érték, mint amikor egy új replika szerepel-e toohello set gyorsabb újrakonfigurálás hello alapértelmezett okozhat növelése. Ez azért van, amely akkor történik meg miatt további előzmények hello naplóban műveletek toohello rendelkezésre állását toohello részleges állapot átviteli miatt. A potenciálisan növelheti az hello helyreállítási időt a replikák rendszerösszeomlás után.

hello MaxRecordSizeInKB beállítás határozza meg, hogy egy rekordot, amely hello naplófájl hello replikátor által írható hello maximális méretét. A legtöbb esetben hello alapértelmezett 1024 KB-os rekord mérete optimális. Előfordulhat azonban, ha hello szolgáltatás okozza nagyobb adatok elemek toobe része hello állapotát, majd ezt az értéket kell toobe nőtt. Nincs sok előnye létrehozása során MaxRecordSizeInKB kisebb, mint 1024, mint kisebb rekordok csak hello helyigényt hello kisebb rekordhoz. Elvárjuk, hogy ez az érték csak ritkán módosított toobe lenne szükség.

hello SharedLogId és SharedLogPath beállításokat a rendszer mindig használt együtt toomake szolgáltatás használata egy különálló megosztott napló hello alapértelmezett megosztott naplóból hello csomópont. A hatékonyság növelése érdekében a lehető legtöbb szolgáltatások hello adja meg ugyanazt a közös napló. Megosztott fájlok megosztott hello napló fájl tooreduce központi adatátviteli versengés kizárólag a használt lemezen kell elhelyezni. Elvárjuk, hogy ez az érték csak ritkán módosított toobe lenne szükség.

## <a name="next-steps"></a>Következő lépések
* [A Visual Studio a Service Fabric-alkalmazás hibakeresése](service-fabric-debugging-your-application.md)
* [Fejlesztői útmutató a Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

