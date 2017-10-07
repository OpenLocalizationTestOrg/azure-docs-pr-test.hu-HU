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
# <a name="placement-policies-for-service-fabric-services"></a><span data-ttu-id="870f7-103">Elhelyezési házirendeket a service fabric szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="870f7-103">Placement policies for service fabric services</span></span>
<span data-ttu-id="870f7-104">Elhelyezési házirendeket lehet néhány meghatározott, kevésbé-közös forgatókönyvekben használt toogovern szolgáltatáselhelyezés további szabályok is.</span><span class="sxs-lookup"><span data-stu-id="870f7-104">Placement policies are additional rules that can be used toogovern service placement in some specific, less-common scenarios.</span></span> <span data-ttu-id="870f7-105">Néhány példa a azokra a következők:</span><span class="sxs-lookup"><span data-stu-id="870f7-105">Some examples of those scenarios are:</span></span>

- <span data-ttu-id="870f7-106">A Service Fabric-fürt által felölelt földrajzi távolság, például több helyszíni adatközpontot vagy Azure-régiók között</span><span class="sxs-lookup"><span data-stu-id="870f7-106">Your Service Fabric cluster spans geographic distances, such as multiple on-premises datacenters or across Azure regions</span></span>
- <span data-ttu-id="870f7-107">A környezet geopolitikai vagy jogi vezérlő több területet, vagy valamilyen egyéb házirend határok esetében is tooenforce van szüksége</span><span class="sxs-lookup"><span data-stu-id="870f7-107">Your environment spans multiple areas of geopolitical or legal control, or some other case where you have policy boundaries you need tooenforce</span></span>
- <span data-ttu-id="870f7-108">Kommunikációs teljesítményt és késést szempontot toolarge távolság vagy a lassabb vagy kevésbé megbízható hálózati kapcsolat miatt</span><span class="sxs-lookup"><span data-stu-id="870f7-108">There are communication performance or latency considerations due toolarge distances or use of slower or less reliable network links</span></span>
- <span data-ttu-id="870f7-109">Bizonyos munkaterhelések közös elhelyezésű, mint a lehető legjobb rendezését, vagy egyéb munkaterhelésekkel való, vagy a közelségi kapcsolat toocustomers tookeep van szüksége</span><span class="sxs-lookup"><span data-stu-id="870f7-109">You need tookeep certain workloads collocated as a best effort, either with other workloads or in proximity toocustomers</span></span>

<span data-ttu-id="870f7-110">A jelentős része igazodnak a fizikai elrendezését hello hello fürt, a tartalék tartományok hello fürt hello szerepel.</span><span class="sxs-lookup"><span data-stu-id="870f7-110">Most of these requirements align with hello physical layout of hello cluster, represented as hello fault domains of hello cluster.</span></span> 

<span data-ttu-id="870f7-111">hello speciális elhelyezési házirendeket, amelyek segítenek a forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="870f7-111">hello advanced placement policies that help address these scenarios are:</span></span>

1. <span data-ttu-id="870f7-112">Érvénytelen tartományok</span><span class="sxs-lookup"><span data-stu-id="870f7-112">Invalid domains</span></span>
2. <span data-ttu-id="870f7-113">Szükséges tartományok</span><span class="sxs-lookup"><span data-stu-id="870f7-113">Required domains</span></span>
3. <span data-ttu-id="870f7-114">Előnyben részesített tartományokat</span><span class="sxs-lookup"><span data-stu-id="870f7-114">Preferred domains</span></span>
4. <span data-ttu-id="870f7-115">A replika csomagolási letiltása</span><span class="sxs-lookup"><span data-stu-id="870f7-115">Disallowing replica packing</span></span>

<span data-ttu-id="870f7-116">A következő vezérlők hello többsége sikerült konfigurálni a csomópont-tulajdonságok és elhelyezési korlátozás keresztül, de néhány bonyolultabb.</span><span class="sxs-lookup"><span data-stu-id="870f7-116">Most of hello following controls could be configured via node properties and placement constraints, but some are more complicated.</span></span> <span data-ttu-id="870f7-117">egyszerűbb toomake dolog, hello Service Fabric fürt erőforrás-kezelő elhelyezésének további szabályzatokról biztosít.</span><span class="sxs-lookup"><span data-stu-id="870f7-117">toomake things simpler, hello Service Fabric Cluster Resource Manager provides these additional placement policies.</span></span> <span data-ttu-id="870f7-118">Elhelyezési házirendek / nevű szolgáltatás példány alapon.</span><span class="sxs-lookup"><span data-stu-id="870f7-118">Placement policies are configured on a per-named service instance basis.</span></span> <span data-ttu-id="870f7-119">Akkor is frissíthető dinamikusan.</span><span class="sxs-lookup"><span data-stu-id="870f7-119">They can also be updated dynamically.</span></span>

## <a name="specifying-invalid-domains"></a><span data-ttu-id="870f7-120">Érvénytelen tartományok megadása</span><span class="sxs-lookup"><span data-stu-id="870f7-120">Specifying invalid domains</span></span>
<span data-ttu-id="870f7-121">Hello **InvalidDomain** elhelyezési házirend lehetővé teszi, hogy egy adott tartalék tartomány érvénytelen egy adott szolgáltatáshoz toospecify.</span><span class="sxs-lookup"><span data-stu-id="870f7-121">hello **InvalidDomain** placement policy allows you toospecify that a particular Fault Domain is invalid for a specific service.</span></span> <span data-ttu-id="870f7-122">Ez a házirend biztosítja, hogy egy adott szolgáltatáshoz soha nem egy adott területen, például a geopolitikai vagy vállalati házirendek miatt.</span><span class="sxs-lookup"><span data-stu-id="870f7-122">This policy ensures that a particular service never runs in a particular area, for example for geopolitical or corporate policy reasons.</span></span> <span data-ttu-id="870f7-123">Érvénytelen többtartományos külön házirendek keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="870f7-123">Multiple invalid domains may be specified via separate policies.</span></span>

<span data-ttu-id="870f7-124"><center>
![Érvénytelen tartomány – példa][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="870f7-124"><center>
![Invalid Domain Example][Image1]
</center></span></span>

<span data-ttu-id="870f7-125">Kód:</span><span class="sxs-lookup"><span data-stu-id="870f7-125">Code:</span></span>

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="870f7-126">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="870f7-126">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a><span data-ttu-id="870f7-127">Adja meg a szükséges tartományok</span><span class="sxs-lookup"><span data-stu-id="870f7-127">Specifying required domains</span></span>
<span data-ttu-id="870f7-128">hello szükséges tartományi elhelyezési házirend szükséges, hogy csak az hello megadott tartomány legyen-e hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="870f7-128">hello required domain placement policy requires that hello service is present only in hello specified domain.</span></span> <span data-ttu-id="870f7-129">Több szükséges tartomány külön házirendek keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="870f7-129">Multiple required domains can be specified via separate policies.</span></span>

<span data-ttu-id="870f7-130"><center>
![Kötelező: Példa][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="870f7-130"><center>
![Required Domain Example][Image2]
</center></span></span>

<span data-ttu-id="870f7-131">Kód:</span><span class="sxs-lookup"><span data-stu-id="870f7-131">Code:</span></span>

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

<span data-ttu-id="870f7-132">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="870f7-132">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-hello-primary-replicas-of-a-stateful-service"></a><span data-ttu-id="870f7-133">Hello elsődleges replikára változott egy állapotalapú szolgáltatás az elsődleges tartomány megadása</span><span class="sxs-lookup"><span data-stu-id="870f7-133">Specifying a preferred domain for hello primary replicas of a stateful service</span></span>
<span data-ttu-id="870f7-134">hello elsődleges tartomány elsődleges megadja hello tartalék tartomány tooplace hello az elsődleges.</span><span class="sxs-lookup"><span data-stu-id="870f7-134">hello Preferred Primary Domain specifies hello fault domain tooplace hello Primary in.</span></span> <span data-ttu-id="870f7-135">hello elsődleges fejeződik be a tartomány minden kifogástalan esetén.</span><span class="sxs-lookup"><span data-stu-id="870f7-135">hello Primary ends up in this domain when everything is healthy.</span></span> <span data-ttu-id="870f7-136">Ha hello tartomány vagy hello elsődleges replika nem sikerül, vagy leállítja, hello elsődleges áthelyezi toosome más helyre, ideális esetben a hello ugyanabban a tartományban.</span><span class="sxs-lookup"><span data-stu-id="870f7-136">If hello domain or hello Primary replica fails or shuts down, hello Primary moves toosome other location, ideally in hello same domain.</span></span> <span data-ttu-id="870f7-137">Ha az új hely nem előnyben részesített hello tartományban, hello fürt erőforrás-kezelő helyezi át a lehető leghamarabb vissza toohello elsődleges tartományt.</span><span class="sxs-lookup"><span data-stu-id="870f7-137">If this new location isn't in hello preferred domain, hello Cluster Resource Manager moves it back toohello preferred domain as soon as possible.</span></span> <span data-ttu-id="870f7-138">Természetesen ez a beállítás csak értelme állapotalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="870f7-138">Naturally this setting only makes sense for stateful services.</span></span> <span data-ttu-id="870f7-139">Ez a házirend olyan fürtökben, amelyek az Azure-régiók is átnyúlhatnak a leghasznosabb, vagy több adatközpontot azonban vannak olyan szolgáltatások, amelyek egy adott helyen elhelyezési inkább.</span><span class="sxs-lookup"><span data-stu-id="870f7-139">This policy is most useful in clusters that are spanned across Azure regions or multiple datacenters but have services that prefer placement in a certain location.</span></span> <span data-ttu-id="870f7-140">Való tartása elsődleges zárja be a tootheir felhasználók és más szolgáltatások segít kisebb késést biztosít, különösen az olvasások, amely alapértelmezés szerint elsődleges kezeli.</span><span class="sxs-lookup"><span data-stu-id="870f7-140">Keeping Primaries close tootheir users or other services helps provide lower latency, especially for reads, which are handled by Primaries by default.</span></span>

<span data-ttu-id="870f7-141"><center>
![Előnyben részesített elsődleges tartományok és a feladatátvétel][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="870f7-141"><center>
![Preferred Primary Domains and Failover][Image3]
</center></span></span>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="870f7-142">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="870f7-142">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a><span data-ttu-id="870f7-143">Replika terjesztési igénylő és csomagolási letiltása</span><span class="sxs-lookup"><span data-stu-id="870f7-143">Requiring replica distribution and disallowing packing</span></span>
<span data-ttu-id="870f7-144">Replika _általában_ elosztott hiba és a frissítési tartományok között, ha hello fürt állapota kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="870f7-144">Replicas are _normally_ distributed across fault and upgrade domains when hello cluster is healthy.</span></span> <span data-ttu-id="870f7-145">Azonban vannak esetek, ahol adott partíció egynél több replikát is szükségessé tehet ideiglenesen csomagolt egy tartományba.</span><span class="sxs-lookup"><span data-stu-id="870f7-145">However, there are cases where more than one replica for a given partition may end up temporarily packed into a single domain.</span></span> <span data-ttu-id="870f7-146">Például tételezzük fel hello fürthöz tartoznak kilenc csomópontok három tartalék tartományokban, fd: / 0, fd: / 1 és fd: / 2.</span><span class="sxs-lookup"><span data-stu-id="870f7-146">For example, let's say that hello cluster has nine nodes in three fault domains, fd:/0, fd:/1, and fd:/2.</span></span> <span data-ttu-id="870f7-147">Tételezzük is fel, hogy a szolgáltatás rendelkezik-e a három replikákat.</span><span class="sxs-lookup"><span data-stu-id="870f7-147">Let's also say that your service has three replicas.</span></span> <span data-ttu-id="870f7-148">Tegyük fel, hogy hello e fd található replikák volt használatban lévő csomópontok: 1 és fd: / 2 csökkent.</span><span class="sxs-lookup"><span data-stu-id="870f7-148">Let's say that hello nodes that were being used for those replicas in fd:/1 and fd:/2 went down.</span></span> <span data-ttu-id="870f7-149">Általában a hello erőforrás-kezelő fürt többi csomópontjának azonos tartalék tartományokban inkább.</span><span class="sxs-lookup"><span data-stu-id="870f7-149">Normally hello Cluster Resource Manager would prefer other nodes in those same fault domains.</span></span> <span data-ttu-id="870f7-150">Ebben az esetben Tételezzük fel toocapacity problémák miatt nincs hello a többi csomópont azokban a tartományokban volt érvényes.</span><span class="sxs-lookup"><span data-stu-id="870f7-150">In this case, let's say due toocapacity issues none of hello other nodes in those domains were valid.</span></span> <span data-ttu-id="870f7-151">Ha hello fürt erőforrás-kezelő létrehozta cserékhez említett-replikákhoz, toochoose csomópontok kellene a fd: / 0.</span><span class="sxs-lookup"><span data-stu-id="870f7-151">If hello Cluster Resource Manager builds replacements for those replicas, it would have toochoose nodes in fd:/0.</span></span> <span data-ttu-id="870f7-152">Azonban ez _, amely_ hoz létre olyan helyzet, ahol hello tartalék tartomány korlátozás sérül.</span><span class="sxs-lookup"><span data-stu-id="870f7-152">However, doing _that_ creates a situation where hello Fault Domain constraint is violated.</span></span> <span data-ttu-id="870f7-153">Replikák növekszik hello esélye annak, hogy a teljes replika hello sikerült leáll vagy elvesznek.</span><span class="sxs-lookup"><span data-stu-id="870f7-153">Packing replicas increases hello chance that hello whole replica set could go down or be lost.</span></span> 

> [!NOTE]
> <span data-ttu-id="870f7-154">További információ a korlátozások és a korlátozás prioritások általában, tekintse meg [ebben a témakörben](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span><span class="sxs-lookup"><span data-stu-id="870f7-154">For more information on constraints and constraint priorities generally, check out [this topic](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span></span>
>

<span data-ttu-id="870f7-155">Ha már legalább egyszer megtekintett állapotfigyelő üzenet például a "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", akkor ezt az állapotot, vagy azt hasonlót már elérte.</span><span class="sxs-lookup"><span data-stu-id="870f7-155">If you've ever seen a health message such as "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", then you've hit this condition or something like it.</span></span> <span data-ttu-id="870f7-156">Általában csak egy vagy két replikák vannak csomagolva együtt ideiglenesen.</span><span class="sxs-lookup"><span data-stu-id="870f7-156">Usually only one or two replicas are packed together temporarily.</span></span> <span data-ttu-id="870f7-157">Mindaddig, amíg nincsenek egy adott tartományban replikák kvórum nem lépi-e, tehát biztonságos.</span><span class="sxs-lookup"><span data-stu-id="870f7-157">So long as there are fewer than a quorum of replicas in a given domain, you're safe.</span></span> <span data-ttu-id="870f7-158">Csomagolási ritkán fordul elő, de akkor fordulhat elő, és általában ezekben a helyzetekben átmeneti óta hello csomópontok térjen vissza.</span><span class="sxs-lookup"><span data-stu-id="870f7-158">Packing is rare, but it can happen, and usually these situations are transient since hello nodes come back.</span></span> <span data-ttu-id="870f7-159">Ha hello csomópontok le marad, és erőforrás-kezelő fürt hello toobuild cserékhez kell, általában nincsenek más csomópontok elérhető hello ideális tartalék tartományokban.</span><span class="sxs-lookup"><span data-stu-id="870f7-159">If hello nodes do stay down and hello Cluster Resource Manager needs toobuild replacements, usually there are other nodes available in hello ideal fault domains.</span></span>

<span data-ttu-id="870f7-160">Bizonyos alkalmazások és szolgáltatások inkább, mindig rendelkező hello cél száma replikákat, még akkor is, ha azok csomagolt, kevesebb tartományokra.</span><span class="sxs-lookup"><span data-stu-id="870f7-160">Some workloads would prefer always having hello target number of replicas, even if they are packed into fewer domains.</span></span> <span data-ttu-id="870f7-161">Az ilyen terhelések vannak fogadások teljes egyidejű állandó tartomány-hibákkal szemben, és általában helyre tudja állítani a helyi állapotát.</span><span class="sxs-lookup"><span data-stu-id="870f7-161">These workloads are betting against total simultaneous permanent domain failures and can usually recover local state.</span></span> <span data-ttu-id="870f7-162">Egyéb munkaterhelések ahelyett, hogy igényelne hello állásidő rendszernél kockázat nézetet, illetve az adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="870f7-162">Other workloads would rather take hello downtime earlier than risk correctness or loss of data.</span></span> <span data-ttu-id="870f7-163">A legtöbb termelési számítási feladatokhoz háromnál több replikákat, több mint három tartalék tartományok és tartalék tartomány sok érvényes csomópontok futtassa.</span><span class="sxs-lookup"><span data-stu-id="870f7-163">Most production workloads run with more than three replicas, more than three fault domains, and many valid nodes per fault domain.</span></span> <span data-ttu-id="870f7-164">Ebből kifolyólag hello alapértelmezett viselkedés lehetővé teszi, hogy tartományi csomagolási alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="870f7-164">Because of this, hello default behavior allows domain packing by default.</span></span> <span data-ttu-id="870f7-165">hello alapértelmezett viselkedés lehetővé teszi a normál terheléselosztási és feladatátvételi toohandle szélsőséges esetben akkor is, ha ez azt jelenti, hogy ideiglenes tartomány csomagolási.</span><span class="sxs-lookup"><span data-stu-id="870f7-165">hello default behavior allows normal balancing and failover toohandle these extreme cases, even if that means temporary domain packing.</span></span>

<span data-ttu-id="870f7-166">Ha azt szeretné, toodisable ilyen csomagolóládába egy adott munkaterhelés számára, megadhatja a hello `RequireDomainDistribution` házirend hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="870f7-166">If you want toodisable such packing for a given workload, you can specify hello `RequireDomainDistribution` policy on hello service.</span></span> <span data-ttu-id="870f7-167">Ha ez a házirend be van állítva, hello fürt erőforrás-kezelő biztosítja a partícióra futtatása ugyanazon fault vagy frissítési tartomány hello hello két replika.</span><span class="sxs-lookup"><span data-stu-id="870f7-167">When this policy is set, hello Cluster Resource Manager ensures no two replicas from hello same partition run in hello same fault or upgrade domain.</span></span>

<span data-ttu-id="870f7-168">Kód:</span><span class="sxs-lookup"><span data-stu-id="870f7-168">Code:</span></span>

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

<span data-ttu-id="870f7-169">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="870f7-169">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

<span data-ttu-id="870f7-170">Most, akkor lehetséges toouse kell ezeket a konfigurációkat, szolgáltatások, amelyek nem földrajzilag ölel fürtben?</span><span class="sxs-lookup"><span data-stu-id="870f7-170">Now, would it be possible toouse these configurations for services in a cluster that was not geographically spanned?</span></span> <span data-ttu-id="870f7-171">Meg lehetett, de nincs nagy OK túl.</span><span class="sxs-lookup"><span data-stu-id="870f7-171">You could, but there’s not a great reason too.</span></span> <span data-ttu-id="870f7-172">hello konfigurációk kerülendő, kivéve, ha hello forgatókönyvek használatához szükséges, érvénytelen és előnyben részesített tartományát.</span><span class="sxs-lookup"><span data-stu-id="870f7-172">hello required, invalid, and preferred domain configurations should be avoided unless hello scenarios require them.</span></span> <span data-ttu-id="870f7-173">Nem létrehozni, akkor semmilyen értelemben tootry tooforce egy adott munkaterhelés toorun egyetlen állvány vagy tooprefer néhány szegmens a helyi fürt másikkal.</span><span class="sxs-lookup"><span data-stu-id="870f7-173">It doesn't make any sense tootry tooforce a given workload toorun in a single rack, or tooprefer some segment of your local cluster over another.</span></span> <span data-ttu-id="870f7-174">Különböző hardverkonfigurációk legyen elosztva a tartalék tartományok és kezelt normál elhelyezési korlátozás és a csomópont tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="870f7-174">Different hardware configurations should be spread across fault domains and handled via normal placement constraints and node properties.</span></span>

## <a name="next-steps"></a><span data-ttu-id="870f7-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="870f7-175">Next steps</span></span>
- <span data-ttu-id="870f7-176">A szolgáltatások konfigurálásáról [további információ a szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="870f7-176">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
