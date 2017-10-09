---
title: "aaaService Fabric-fürt erőforráskezelő - felügyelet integrálását |} Microsoft Docs"
description: "Hello integrációs pontjainak közötti hello fürt erőforrás-kezelő és a Service Fabric Management áttekintése."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9a24c9de121fbe2e8e5e8e4d117e64686918936a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Fürt resource manager integrálása a Service Fabric-fürt felügyeleti
Service Fabric fürt erőforrás-kezelő hello frissítések nem meghajtója Service Fabric azonban részt vesz. hello első módon hello fürt erőforrás-kezelő segítségével felügyeleti követési hello által keresett hello fürt és a benne lévő hello szolgáltatások állapotát. hello fürt erőforrás-kezelő akkor küld állapotjelentések ki, ha azt nem hello fürt üzembe hello szükséges konfiguráció. Például ha nincs elegendő kapacitása hello fürt erőforrás-kezelő küld állapotfigyelő figyelmeztetések és hibák hello kapcsolatos problémára utaló ki. Integráció más adat toodo a frissítések működése rendelkezik. hello erőforrás-kezelő fürt működése némileg megváltoztatja frissítéskor.  

## <a name="health-integration"></a>Rendszerállapot-integráció
Fürt erőforrás-kezelő hello folyamatosan nyomon követi a szolgáltatások elhelyezéséhez megadott hello szabályok. Is egész mindegyik metrikát hello csomópontján és hello fürt és hello fürt kapacitása fennmaradó hello követi nyomon. Ha azt nem elégíti ki ezeket a szabályokat, vagy ha nincs elég kapacitás, az egészségügyi figyelmeztetések és hibák kibocsátott. Például ha egy csomópont kapacitás és hello fürt erőforrás-kezelő megpróbál toofix hello helyzet szolgáltatások áthelyezésével. Ha nem tud javítani hello helyzet állapotfigyelő figyelmeztetés azt jelzi, hogy melyik csomópont kapacitás, és melyik metrikáihoz bocsát ki.

Egy másik hello Resource Manager egészségügyi figyelmeztetések: egy elhelyezési korlátozás megsértése. Például, ha meg van adva egy elhelyezési korlátozás (például `“NodeColor == Blue”`) és erőforrás-kezelő hello észleli, hogy a korlátozás megsértését, azt egy állapotfigyelési figyelmeztetése bocsát ki. Ez az egyéni korlátozások és hello alapértelmezett korlátozásokban (például hello tartalék tartomány és a frissítési tartomány korlátozások).

Íme egy példa egy ilyen állapotjelentése. Ebben az esetben hello állapotjelentése az egyik hello rendszer szolgáltatás partíciók van. hello állapotát az üzenet azt jelzi, hogy az adott partíció replikák átmenetileg túl kevés frissítés tartományokra vannak csomagolt hello.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : hello Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating hello Constraint: UpgradeDomain Details: UpgradeDomain ID -- 4, Replica on NodeName -- Node.8 Currently Upgrading -- false Distribution Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Mi az egészségügyi üzenet ezzel azt jelzi, az itt található:

1. Összes hello replika maguk kifogástalan: mindegyik rendelkezik-e AggregatedHealthState: Ok
2. hello frissítési tartomány terjesztési megkötés jelenleg éppen sérül. Ez azt jelenti, hogy egy adott frissítés tartomány rendelkezik további replikák ehhez a partícióhoz a kelleténél.
3. Melyik csomópontján hello replika, ezzel hello megsértése tartalmazza. Ebben az esetben hello csomópont "Node.8" hello nevű
4. Frissítés e jelenleg folyik a ez partícióazonosító ("jelenleg frissítése – false")
5. Ezt a szolgáltatást a terjesztési házirend hello: "Terjesztési házirend--csomagolási". Ez vonatkozik hello `RequireDomainDistribution` [elhelyezési házirend](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). "Csomagolási" azt jelzi, hogy ebben az esetben DomainDistribution _nem_ szükséges, hogy tudjuk, hogy elhelyezési házirend nincs megadva ehhez a szolgáltatáshoz. 
6. Ha hello jelentés történt – 8/10/2015 7 13:02 du.:

Adatok, például az, hogy valami nem megfelelő állapotba került, és ezzel egyúttal tudja éles toolet tűz használt toodetect powers riasztások, és halt rossz frissítések. Ebben az esetben akkor szeretnénk toosee Ha azt kitalálja, hogy miért hello erőforrás-kezelő kellett toopack hello replikák hello frissítési tartomány be. Általában csomagolási nem átmeneti, mert hello hello csomópontja más frissítési tartományok lettek, például.

Tegyük fel, erőforrás-kezelő fürt hello próbál tooplace néhány szolgáltatást, de nincs bármely megoldások, amelyek működnek. Szolgáltatások nem helyezhető el, esetén általában hello a következő okok valamelyike miatt:

1. Néhány átmeneti állapotról tette lehetetlen tooplace a szolgáltatáspéldány, vagy a replika megfelelően
2. hello szolgáltatás elhelyezésének vonatkozó követelmények nincsenek unsatisfiable.

Ezekben az esetekben a hello fürt erőforrás-kezelő a rendszerállapot-jelentések segítségével meghatározhatja, miért hello szolgáltatás nem helyezhető el. A folyamat hello megkötés eltávolítási feladatütemezési nevezzük. Során, hello rendszer végigvezeti a azok kiküszöbölik hello szolgáltatást és a rekordok érintő konfigurált hello megkötések. Így ha a szolgáltatás nem tudja elhelyezni, toobe látható csomópontok szüntetni volt, és miért.

## <a name="constraint-types"></a>Korlátozás típusok
Most szolgáltatással kapcsolatban egyes hello különböző korlátozásokat, a rendszerállapot-jelentések. Állapotfigyelő üzenetek kapcsolódó toothese megkötések látni fogja, amikor a replikák nem helyezhető el.

* **ReplicaExclusionStatic** és **ReplicaExclusionDynamic**: ezek a megkötések azt jelzi, hogy a megoldás vissza lett utasítva, mert a két szolgáltatás objektumokat hello partíción kellene toobe helyezett hello azonos csomópont. Ez nem engedélyezett, mert, majd az adott csomópont hibát túlságosan befolyásolná a adott partíció. ReplicaExclusionStatic és ReplicaExclusionDynamic szinte hello ugyanaz a szabály és hello különbségek valóban nem számít. Ha Ön egy korlátozás eltávolítási sorozatot tartalmazó vagy hello ReplicaExclusionStatic vagy ReplicaExclusionDynamic megkötést, erőforrás-kezelő fürt hello úgy értelmezi, hogy nincs-e elég csomópont. Ehhez a fennmaradó megoldások toouse ezek érvénytelen adatközpontokon, amelyek nem engedélyezettek. hello egyéb megkötések hello sorrendben lesznek általában adja meg, miért szüntetni hello első helyen a csomópontok.
* **PlacementConstraint**: Ha ez az üzenet jelenik meg, az azt jelenti, hogy egyes csomópontok azt szüntetni, mivel azok nem egyeztek hello szolgáltatás placement Constraints korlátozásokat. Azt nyomkövetési jelenleg konfigurált hello placement Constraints korlátozásokat ki, ez az üzenet részeként. Ez nem rendellenes, ha egy elhelyezési korlátozás definiálva van. Azonban ha elhelyezési korlátozás helytelenül hatására túl sok csomópontot toobe szüntetni azt, hogyan szeretné észlel.
* **NodeCapacity**: ennél a határértéknél azt jelenti, hogy az adott fürt erőforrás-kezelő nem tudta helyezze hello replikák hello hello csomópontok jelzett, mert, célszerű keresztül kapacitás helyezni.
* **Kapcsolat**: Ez a korlát, hogy azt nem sikerült helyezze hello replika hatással hello csomópontok óta hello affinitás korlátozás megsértését okozna. További információk a kapcsolat [Ez a cikk](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** és **UpgradeDomain**: ennél a határértéknél megszünteti a csomópont, ha forgalomba hozatalára hello replika hello a csomópontok egy adott hiba vagy a frissítési tartomány csomagolási miatt. Néhány példa ennél a határértéknél megvitatása hello témakör jelennek meg [hiba és a frissítés tartomány típusmegkötéseket és az eredményül kapott viselkedéstől](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: nem szabad általában látja csomópontok eltávolítása hello megoldás, mert alapértelmezés szerint az optimalizálás fut ennél a határértéknél. hello az előnyben részesített földrajzi megszorítás is megtalálható frissítéskor. A frissítés során használt toomove szolgáltatások hátsó toowhere voltak hello frissítés indítása, ha.

## <a name="blocklisting-nodes"></a>Blocklisting csomópontok
Egy másik üzenet hello fürt erőforrás-kezelő állapotjelentések akkor, ha csomópontok blocklisted. Az eltolásokat tekintheti blocklisting meg automatikusan alkalmazott ideiglenes korlátozásként. Csomópontok blocklisted jelenik meg, ha az adott szolgáltatás típusú példányok elindításakor az ismétlődő sikertelen ügyfeleken. Csomópontok blocklisted /-szolgáltatás típusa alapján. Egy csomópont lehet egy service Type blocklisted, de nem egy másik. 

Látni fogja, fejlesztés során gyakran indítsa blocklisting: néhány programhibája miatt a szolgáltatás állomás toocrash indításakor. A Service Fabric megpróbál toocreate hello szolgáltatásgazda néhány alkalommal, és hello probléma tartja lépett fel. Több próbálkozás után hello csomópont lekérdezi blocklisted, és hello fürt erőforrás-kezelő megpróbálja máshol toocreate hello szolgáltatást. A hiba több csomóponton tartja történik, akkor lehetséges, hogy minden érvényes csomópont hello lévő hello fürt mentése tiltsa le. Blocklisting is, hogy nincs elegendő sikeresen elindítani a hello szolgáltatás toomeet szükséges hello méretezési sok csomópontok mindegyikének eltávolítását. Látni fogja, általában további hibákat vagy figyelmeztetéseket hello fürt erőforrás-kezelő azt jelzi, hogy hello szolgáltatás alatt hello kívánt replika vagy a példányok száma, valamint milyen hello sikertelenséget jelző rendszerállapot-üzenetek toohello vezet blocklisting hello első helyen.

Blocklisting nincs állandó feltétel. Néhány perc elteltével hello csomópontot a rendszer eltávolítja a hello blocklist, és a Service Fabric újra aktiválhatja hello szolgáltatások ezen a csomóponton. Szolgáltatás folytatáshoz toofail hello csomópont esetén az adott szolgáltatás blocklisted újra. 

### <a name="constraint-priorities"></a>Korlátozás prioritások

> [!WARNING]
> Korlátozás prioritások módosítása nem ajánlott, és jelentős hátrányosan befolyásolhatja a fürtön. hello alábbi információkat referenciaként hello alapértelmezett megkötés prioritások és azok viselkedését valósul meg. 
>

Az összes megkötés akkor előfordulhat, hogy rendelkezik lett végezni "Hey – úgy vélem, hogy a tartalék tartomány korlátozások hello legfontosabb dolog, a rendszer. A sorrend tooensure hello tartalék tartomány korlátozás nem felel meg, vagyok hajlandó tooviolate egyéb megkötések. "

Megkötések konfigurálható különböző prioritási szintet. Ezek a következők:

   - "rögzített" (0)
   - "soft" (1)
   - "optimalizálási" (2)
   - "off" (-1). 
   
A legtöbb hello megkötések alapértelmezés szerint rögzített korlátozások van konfigurálva.

Ritka megkötések hello prioritásának módosítása. Nem történt alkalommal ahol megkötés prioritások szükséges toochange, általában toowork néhány egyéb hiba vagy befolyásolta-hello környezet működésére. Hello megkötés prioritás infrastruktúra hello rugalmasságot általában a jól működött, de gyakran nem szükséges. A legtöbb hello idő minden, az alapértelmezett prioritások helyezkedik el. 

hello prioritási szintek nem jelenti azt, hogy egy adott korlátozás _fog_ sikeres, és nem lesz mindig kell megfelelnie. Korlátozás prioritások megadása egy sorrendet, amelyben megkötések lépnek érvénybe. Prioritások meghatározása hello mellékhatásokkal lehetetlen toosatisfy esetén minden megkötések. Általában minden hello korlátozó kielégíthetők, kivéve, ha valami más továbblép a hello környezetben. Néhány példa a forgatókönyvek, amelyek tooconstraint megsértésével olyan ütköző tartoznak, illetve nagyszámú egyidejű hibák.

A speciális esetben hello megkötés prioritások módosíthatja. Például, hogy a keresett tooensure, hogy affinitás volna mindig sérti-amikor csomópont-kapacitás szükséges toosolve ad ki. tooachieve, képes hello prioritást hello affinitás megkötés túl "soft" (1), és hagyja hello kapacitás megkötés beállítása túl "rögzített" (0).

a következő konfigurációs hello megadott hello alapértelmezett prioritási értékek hello különböző korlátozásokat:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Tartalék tartomány és a frissítési tartomány megkötések
hello fürt erőforrás-kezelő hiba és a frissítési tartományok között felülbírálásokkal tookeep szolgáltatások szeretne. Az belül hello fürt erőforrás-kezelő motor korlátozásként modellek el. További információt talál azok használata és az adott viselkedést, tekintse meg a cikk hello a [fürtkonfiguráció](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

Erőforrás-kezelő fürt hello esetleg toopack néhány replikák azokat a frissítéseket, vagy a más korlátozások megsértésének rendelés toodeal a frissítési tartományok. Csak akkor, ha nincsenek több hibákat vagy olyan más forgalom hello rendszerrel, amely megakadályozza a megfelelő elhelyezési csomagolási hiba vagy a frissítési tartományok a szokásos módon történik. Ha ezek közül valamelyik helyzet esetén csomagolási tooprevent, használhatja hello `RequireDomainDistribution` [elhelyezési házirend](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Vegye figyelembe, hogy ez lehetséges, hogy hatással a szolgáltatás rendelkezésre állása és megbízhatósága, egyik mellékhatása, ezért fontolja meg alaposan.

Hello környezet megfelelően van konfigurálva, ha minden korlátozó teljesen betartását, még a frissítés során. hello kulcs dolog, hogy az a megkötések az erőforrás-kezelő fürt van figyelése hello. Ha úgy észleli, hogy megsértése azonnal azt jelenti, és megpróbál toocorrect hello probléma.

## <a name="hello-preferred-location-constraint"></a>előnyben részesített hello földrajzi megszorítás
hello PreferredLocation megkötés kissé eltér, mert két különböző használ. Egy ennél a határértéknél használata alkalmazás frissítéskor. hello fürt erőforrás-kezelő automatikusan kezeli ennél a határértéknél frissítéskor. Használt tooensure, hogy ha frissíti a be nem fejeződik, hogy replikák vissza tootheir kezdeti helyét. hello más hello PreferredLocation megkötés használatos hello [ `PreferredPrimaryDomain` elhelyezési házirend](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Mindkét esetben optimalizálás, és ezért hello PreferredLocation megkötés hello egyetlen megkötés túl beállítása "Optimalizálási" alapértelmezés szerint.

## <a name="upgrades"></a>Frissítések
Erőforrás-kezelő fürt hello is segít alkalmazás és a fürt frissítése során, amely rendelkezik a két feladat során:

* Győződjön meg arról, hogy hello szabályok hello fürt ne legyenek veszélyben
* Próbálja meg zökkenőmentesen toohelp hello frissítési Ugrás

### <a name="keep-enforcing-hello-rules"></a>Tartsa hello szabályok érvényesítése
hello fő dolog toobe tisztában, hogy hello szabályok – hello vonatkozó szigorú korlátozásokat például egy elhelyezési korlátozás és a kapacitások - továbbra is érvényesek frissítéskor. Egy elhelyezési korlátozás győződjön meg arról, hogy a munkaterhelések csak futni, ahol azok csatlakozhatnának a, akár a frissítések során. Ha a Szolgáltatások erősen korlátozott, frissítések több időt vesz igénybe. Előfordulhat, hogy néhány lehetőség, ahol folytathatja a frissítési hello szolgáltatást vagy hello csomópont futtatása esetén állapotba.

### <a name="smart-replacements"></a>Intelligens cserékhez
Ha a frissítés elindul, hello erőforrás-kezelő pillanatképet készít a hello hello fürt aktuális elrendezése. Minden frissítési tartomány befejeződött, akkor megpróbálja tooreturn hello szolgáltatásokat, hogy a frissítési tartomány tootheir eredeti megállapodás voltak. Ezzel a módszerrel nincsenek legfeljebb két átmenetek szolgáltatás hello frissítés során. Egy kilép a hatással hello csomópont, és egy helyezze vissza. Hello frissítés biztosítja azt is hello frissítés előtti hello fürt vagy a szolgáltatás toohow adatszolgáltató nem befolyásolja a hello fürt hello elrendezését. 

### <a name="reduced-churn"></a>Csökkentett forgalom
Egy másik olyan dolog, ami a frissítések során történik az adott fürt erőforrás-kezelő kikapcsolja a terheléselosztás hello. Megakadályozza a terheléselosztás megakadályozza, hogy a szükségtelen reakciók toohello frissítés, például a szolgáltatások helyezi át a csomópontokra, amelyeket hello frissítés is szerepelnek. Ha a szóban forgó hello frissítése a fürtök frissítése, majd hello fürtözési nem kiegyensúlyozott hello frissítés során. Korlátozás ellenőrzések addig maradnak aktívak, csak a mozgás hello proaktív terheléselosztási a mérőszámok alapján le van tiltva.

### <a name="buffered-capacity--upgrade"></a>A pufferelt kapacitás & frissítése
Általában érdemes hello frissítési toocomplete akkor is, ha hello fürt vagy a korlátozott toofull. Hello kapacitás hello fürt kezelése fontos még több frissítéskor megy végbe. Attól függően, hogy hello hány frissítési tartományt, 5 és 20 százalékát kapacitás között kell áttelepíteni, hello frissítés áthalad hello fürt. Munka toogo valahol rendelkezik. Ha ez az hello fogalmát [kapacitások pufferelt](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) hasznos. A pufferelt kapacitás tiszteletben tartják normál működés során. hello fürt erőforrás-kezelő frissítéskor szükség lehet, hogy töltse ki csomópontja fel tootheir teljes kapacitás (hello puffer fel).

## <a name="next-steps"></a>Következő lépések
* Indítsa el a hello elejétől és [egy bevezető toohello Service Fabric fürt erőforrás-kezelő beolvasása](service-fabric-cluster-resource-manager-introduction.md)
