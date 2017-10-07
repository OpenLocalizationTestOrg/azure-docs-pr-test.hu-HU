---
title: "a Fabric fürt Resource Manager - aaaService elhelyezési házirendeket |} Microsoft Docs"
description: "További elhelyezési házirendeket és a Service Fabric szolgáltatások szabályainak áttekintése"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bb58642520085ab3000f3929cf9aea7a8f6e3070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="placement-policies-for-service-fabric-services"></a>Elhelyezési házirendeket a service fabric szolgáltatásokhoz
Elhelyezési házirendeket lehet néhány meghatározott, kevésbé-közös forgatókönyvekben használt toogovern szolgáltatáselhelyezés további szabályok is. Néhány példa a azokra a következők:

- A Service Fabric-fürt által felölelt földrajzi távolság, például több helyszíni adatközpontot vagy Azure-régiók között
- A környezet geopolitikai vagy jogi vezérlő több területet, vagy valamilyen egyéb házirend határok esetében is tooenforce van szüksége
- Kommunikációs teljesítményt és késést szempontot toolarge távolság vagy a lassabb vagy kevésbé megbízható hálózati kapcsolat miatt
- Bizonyos munkaterhelések közös elhelyezésű, mint a lehető legjobb rendezését, vagy egyéb munkaterhelésekkel való, vagy a közelségi kapcsolat toocustomers tookeep van szüksége

A jelentős része igazodnak a fizikai elrendezését hello hello fürt, a tartalék tartományok hello fürt hello szerepel. 

hello speciális elhelyezési házirendeket, amelyek segítenek a forgatókönyvek a következők:

1. Érvénytelen tartományok
2. Szükséges tartományok
3. Előnyben részesített tartományokat
4. A replika csomagolási letiltása

A következő vezérlők hello többsége sikerült konfigurálni a csomópont-tulajdonságok és elhelyezési korlátozás keresztül, de néhány bonyolultabb. egyszerűbb toomake dolog, hello Service Fabric fürt erőforrás-kezelő elhelyezésének további szabályzatokról biztosít. Elhelyezési házirendek / nevű szolgáltatás példány alapon. Akkor is frissíthető dinamikusan.

## <a name="specifying-invalid-domains"></a>Érvénytelen tartományok megadása
Hello **InvalidDomain** elhelyezési házirend lehetővé teszi, hogy egy adott tartalék tartomány érvénytelen egy adott szolgáltatáshoz toospecify. Ez a házirend biztosítja, hogy egy adott szolgáltatáshoz soha nem egy adott területen, például a geopolitikai vagy vállalati házirendek miatt. Érvénytelen többtartományos külön házirendek keresztül adható meg.

<center>
![Érvénytelen tartomány – példa][Image1]
</center>

Kód:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Adja meg a szükséges tartományok
hello szükséges tartományi elhelyezési házirend szükséges, hogy csak az hello megadott tartomány legyen-e hello szolgáltatást. Több szükséges tartomány külön házirendek keresztül adható meg.

<center>
![Kötelező: Példa][Image2]
</center>

Kód:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-hello-primary-replicas-of-a-stateful-service"></a>Hello elsődleges replikára változott egy állapotalapú szolgáltatás az elsődleges tartomány megadása
hello elsődleges tartomány elsődleges megadja hello tartalék tartomány tooplace hello az elsődleges. hello elsődleges fejeződik be a tartomány minden kifogástalan esetén. Ha hello tartomány vagy hello elsődleges replika nem sikerül, vagy leállítja, hello elsődleges áthelyezi toosome más helyre, ideális esetben a hello ugyanabban a tartományban. Ha az új hely nem előnyben részesített hello tartományban, hello fürt erőforrás-kezelő helyezi át a lehető leghamarabb vissza toohello elsődleges tartományt. Természetesen ez a beállítás csak értelme állapotalapú szolgáltatások. Ez a házirend olyan fürtökben, amelyek az Azure-régiók is átnyúlhatnak a leghasznosabb, vagy több adatközpontot azonban vannak olyan szolgáltatások, amelyek egy adott helyen elhelyezési inkább. Való tartása elsődleges zárja be a tootheir felhasználók és más szolgáltatások segít kisebb késést biztosít, különösen az olvasások, amely alapértelmezés szerint elsődleges kezeli.

<center>
![Előnyben részesített elsődleges tartományok és a feladatátvétel][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Replika terjesztési igénylő és csomagolási letiltása
Replika _általában_ elosztott hiba és a frissítési tartományok között, ha hello fürt állapota kifogástalan. Azonban vannak esetek, ahol adott partíció egynél több replikát is szükségessé tehet ideiglenesen csomagolt egy tartományba. Például tételezzük fel hello fürthöz tartoznak kilenc csomópontok három tartalék tartományokban, fd: / 0, fd: / 1 és fd: / 2. Tételezzük is fel, hogy a szolgáltatás rendelkezik-e a három replikákat. Tegyük fel, hogy hello e fd található replikák volt használatban lévő csomópontok: 1 és fd: / 2 csökkent. Általában a hello erőforrás-kezelő fürt többi csomópontjának azonos tartalék tartományokban inkább. Ebben az esetben Tételezzük fel toocapacity problémák miatt nincs hello a többi csomópont azokban a tartományokban volt érvényes. Ha hello fürt erőforrás-kezelő létrehozta cserékhez említett-replikákhoz, toochoose csomópontok kellene a fd: / 0. Azonban ez _, amely_ hoz létre olyan helyzet, ahol hello tartalék tartomány korlátozás sérül. Replikák növekszik hello esélye annak, hogy a teljes replika hello sikerült leáll vagy elvesznek. 

> [!NOTE]
> További információ a korlátozások és a korlátozás prioritások általában, tekintse meg [ebben a témakörben](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Ha már legalább egyszer megtekintett állapotfigyelő üzenet például a "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", akkor ezt az állapotot, vagy azt hasonlót már elérte. Általában csak egy vagy két replikák vannak csomagolva együtt ideiglenesen. Mindaddig, amíg nincsenek egy adott tartományban replikák kvórum nem lépi-e, tehát biztonságos. Csomagolási ritkán fordul elő, de akkor fordulhat elő, és általában ezekben a helyzetekben átmeneti óta hello csomópontok térjen vissza. Ha hello csomópontok le marad, és erőforrás-kezelő fürt hello toobuild cserékhez kell, általában nincsenek más csomópontok elérhető hello ideális tartalék tartományokban.

Bizonyos alkalmazások és szolgáltatások inkább, mindig rendelkező hello cél száma replikákat, még akkor is, ha azok csomagolt, kevesebb tartományokra. Az ilyen terhelések vannak fogadások teljes egyidejű állandó tartomány-hibákkal szemben, és általában helyre tudja állítani a helyi állapotát. Egyéb munkaterhelések ahelyett, hogy igényelne hello állásidő rendszernél kockázat nézetet, illetve az adatvesztés. A legtöbb termelési számítási feladatokhoz háromnál több replikákat, több mint három tartalék tartományok és tartalék tartomány sok érvényes csomópontok futtassa. Ebből kifolyólag hello alapértelmezett viselkedés lehetővé teszi, hogy tartományi csomagolási alapértelmezés szerint. hello alapértelmezett viselkedés lehetővé teszi a normál terheléselosztási és feladatátvételi toohandle szélsőséges esetben akkor is, ha ez azt jelenti, hogy ideiglenes tartomány csomagolási.

Ha azt szeretné, toodisable ilyen csomagolóládába egy adott munkaterhelés számára, megadhatja a hello `RequireDomainDistribution` házirend hello szolgáltatásban. Ha ez a házirend be van állítva, hello fürt erőforrás-kezelő biztosítja a partícióra futtatása ugyanazon fault vagy frissítési tartomány hello hello két replika.

Kód:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Most, akkor lehetséges toouse kell ezeket a konfigurációkat, szolgáltatások, amelyek nem földrajzilag ölel fürtben? Meg lehetett, de nincs nagy OK túl. hello konfigurációk kerülendő, kivéve, ha hello forgatókönyvek használatához szükséges, érvénytelen és előnyben részesített tartományát. Nem létrehozni, akkor semmilyen értelemben tootry tooforce egy adott munkaterhelés toorun egyetlen állvány vagy tooprefer néhány szegmens a helyi fürt másikkal. Különböző hardverkonfigurációk legyen elosztva a tartalék tartományok és kezelt normál elhelyezési korlátozás és a csomópont tulajdonságait.

## <a name="next-steps"></a>Következő lépések
- A szolgáltatások konfigurálásáról [további információ a szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
