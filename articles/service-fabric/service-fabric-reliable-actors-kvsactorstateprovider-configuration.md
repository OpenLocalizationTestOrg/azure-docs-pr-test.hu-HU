---
title: "az Azure mikroszolgáltatások aaaChange KVSActorStateProvider beállítások |} Microsoft Docs"
description: "Azure Service Fabric állapot-nyilvántartó szereplője KVSActorStateProvider típusú beállításának ismertetése."
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: timlt
editor: 
ms.assetid: dbed72f4-dda5-4287-bd56-da492710cd96
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: sumukhs
ms.openlocfilehash: e003512678556e68a8926b1b9c6c28d9ae3979d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Reliable Actors--KVSActorStateProvider konfigurálása
Hello alapértelmezett konfigurációja KVSActorStateProvider hello settings.xml fájlban hello Microsoft Visual Studio gyökér package objektum a megadott szereplő hello hello Config mappában létrehozott módosításával módosíthatja.

hello Azure Service Fabric-futtatókörnyezet megkeresi az előre definiált szakaszneveket hello settings.xml fájlban, és hello konfigurációs értékeket fel az alapul szolgáló futásidejű összetevők hello létrehozása során.

> [!NOTE]
> Tegye **nem** törölni vagy módosítani a hello szakaszneveket, a következő konfigurációk hello settings.xml fájlban, a Visual Studio megoldás hello generált hello.
> 
> 

## <a name="replicator-security-configuration"></a>A replikáló biztonsági konfiguráció
A replikáló biztonsági beállításokkal használt toosecure hello kommunikációs csatornát replikáció során használt. Ez azt jelenti, hogy a szolgáltatások nem látható egymás replikációs forgalmat, amely biztosítja, hogy magas rendelkezésre állási hello adatokat is biztonságos.
Alapértelmezés szerint egy üres biztonsági konfigurációs szakasz megakadályozza, hogy a replikációs biztonságot.

### <a name="section-name"></a>A szakasz neve
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replikációs konfiguráció
Replikátor konfigurációk hello replikátor, amely feladata, hogy nagymértékben megbízható hello szereplő Állapotszolgáltató állapotot konfigurálja.
hello alapértelmezett konfiguráció hello Visual Studio-sablon által generált és elegendőnek kell lennie. Ez a szakasz olyan beállításokat, amelyeket a rendszer rendelkezésre álló tootune hello replikátor beszél.

### <a name="section-name"></a>A szakasz neve
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurációs nevek
| Név | Unit (Egység) | Alapértelmezett érték | Megjegyzések |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |másodperc |0.015 |Időszakát mely hello replikátor: hello másodlagos vár a művelet előtt küld vissza egy nyugtázási toohello elsődleges fogadása után. Más nyugták toobe intervallum feldolgozott műveletek küldött küldése a egy választ. |
| ReplicatorEndpoint |N/A |Nincs alapértelmezett érték – a kötelező paraméter: |IP-címet és portot, amelyen az elsődleges és másodlagos replikátor hello más gyártóitól hello replika toocommunicate fog használni. Ez hivatkoznia kell a TCP-erőforrás végpont hello szolgáltatás jegyzékben. Tekintse meg a túl[Service manifest erőforrások](service-fabric-service-manifest-resources.md) tooread több végpont erőforrások definiálása hello szolgáltatás jegyzékben. |
| RetryInterval |másodperc |5 |Időszak után mely hello replikátor újra továbbítja a Ha az egy művelet nyugtázása nem kapott üzenetet. |
| (Maxreplicationmessagesize). |Bájtok |50 MB |Replikációs adatok egyetlen üzenetben továbbítható maximális mérete. |
| MaxPrimaryReplicationQueueSize |Műveletek száma |1024 |Hello elsődleges várólistában lévő műveletek maximális száma. Egy műveletet a fel nem szabadul, miután hello elsődleges replikátor nyugtázást fogad az összes hello másodlagos gyártóitól. Ez az érték 64 és 2 valamelyik hatványa nagyobbnak kell lennie. |
| MaxSecondaryReplicationQueueSize |Műveletek száma |2048 |A másodlagos várólista hello műveletek maximális száma. Egy művelet fel nem szabadul állapotában adatmegőrzési keresztül magas rendelkezésre állású elvégzése után. Ez az érték 64 és 2 valamelyik hatványa nagyobbnak kell lennie. |

## <a name="store-configuration"></a>Adattár konfigurálása
Tárolási konfigurációk használt tooconfigure hello helyi tárolóban, amely a replikált használt toopersist hello állapotban.
hello alapértelmezett konfiguráció hello Visual Studio-sablon által generált és elegendőnek kell lennie. Ez a szakasz beszél további között vannak olyanok, rendelkezésre álló tootune hello helyi tárolóból.

### <a name="section-name"></a>A szakasz neve
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfigurációs nevek
| Név | Unit (Egység) | Alapértelmezett érték | Megjegyzések |
| --- | --- | --- | --- |
| MaxAsyncCommitDelayInMilliseconds |ideje ezredmásodpercben |200 |Hello maximálisan engedélyezett tartós helyi tárolási véglegesítések intervallumát kötegelés állítja. |
| MaxVerPages |Lapok száma |16384 |a helyi hello verzió lapok maximális számát hello adatbázis tárolja. Meghatározza, hogy hello nyitott tranzakciók maximális száma. |

## <a name="sample-configuration-file"></a>Minta konfigurációs fájlt
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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

