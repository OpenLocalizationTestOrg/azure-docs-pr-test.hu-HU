---
title: "Service Fabric fürt erőforrás-kezelő – elhelyezési házirendeket |} Microsoft Docs"
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
ms.openlocfilehash: 6c11d49d5fdb3148b0534c9448f815358fa8cab3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="placement-policies-for-service-fabric-services"></a><span data-ttu-id="f072c-103">Elhelyezési házirendeket a service fabric szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="f072c-103">Placement policies for service fabric services</span></span>
<span data-ttu-id="f072c-104">Elhelyezési házirendeket és a további szabályok, annak a szabályozására, szolgáltatáselhelyezés bizonyos meghatározott, kevésbé-közös esetekben használható.</span><span class="sxs-lookup"><span data-stu-id="f072c-104">Placement policies are additional rules that can be used to govern service placement in some specific, less-common scenarios.</span></span> <span data-ttu-id="f072c-105">Néhány példa a azokra a következők:</span><span class="sxs-lookup"><span data-stu-id="f072c-105">Some examples of those scenarios are:</span></span>

- <span data-ttu-id="f072c-106">A Service Fabric-fürt által felölelt földrajzi távolság, például több helyszíni adatközpontot vagy Azure-régiók között</span><span class="sxs-lookup"><span data-stu-id="f072c-106">Your Service Fabric cluster spans geographic distances, such as multiple on-premises datacenters or across Azure regions</span></span>
- <span data-ttu-id="f072c-107">A környezet geopolitikai vagy jogi vezérlő több területet, vagy valamilyen egyéb házirend esetében is ki kell kényszerítenie határok</span><span class="sxs-lookup"><span data-stu-id="f072c-107">Your environment spans multiple areas of geopolitical or legal control, or some other case where you have policy boundaries you need to enforce</span></span>
- <span data-ttu-id="f072c-108">Kommunikációs teljesítményt és késést szempontot nagy távolságra vagy a lassabb vagy kevésbé megbízható hálózati kapcsolat miatt</span><span class="sxs-lookup"><span data-stu-id="f072c-108">There are communication performance or latency considerations due to large distances or use of slower or less reliable network links</span></span>
- <span data-ttu-id="f072c-109">Szeretne rögzíteni munkaterhelések közös elhelyezésű, mint a lehető legjobb rendezését, vagy egyéb munkaterhelésekkel való vagy közelében, az ügyfél számára</span><span class="sxs-lookup"><span data-stu-id="f072c-109">You need to keep certain workloads collocated as a best effort, either with other workloads or in proximity to customers</span></span>

<span data-ttu-id="f072c-110">A jelentős része igazodnak a fizikai elrendezését a fürt a fürt a tartalék tartományok ábrázolva.</span><span class="sxs-lookup"><span data-stu-id="f072c-110">Most of these requirements align with the physical layout of the cluster, represented as the fault domains of the cluster.</span></span> 

<span data-ttu-id="f072c-111">A speciális elhelyezési házirendeket, amelyek segítenek a forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="f072c-111">The advanced placement policies that help address these scenarios are:</span></span>

1. <span data-ttu-id="f072c-112">Érvénytelen tartományok</span><span class="sxs-lookup"><span data-stu-id="f072c-112">Invalid domains</span></span>
2. <span data-ttu-id="f072c-113">Szükséges tartományok</span><span class="sxs-lookup"><span data-stu-id="f072c-113">Required domains</span></span>
3. <span data-ttu-id="f072c-114">Előnyben részesített tartományokat</span><span class="sxs-lookup"><span data-stu-id="f072c-114">Preferred domains</span></span>
4. <span data-ttu-id="f072c-115">A replika csomagolási letiltása</span><span class="sxs-lookup"><span data-stu-id="f072c-115">Disallowing replica packing</span></span>

<span data-ttu-id="f072c-116">Az alábbi funkciókat a legtöbb sikerült konfigurálni a csomópont-tulajdonságok és elhelyezési korlátozás keresztül, de néhány bonyolultabb.</span><span class="sxs-lookup"><span data-stu-id="f072c-116">Most of the following controls could be configured via node properties and placement constraints, but some are more complicated.</span></span> <span data-ttu-id="f072c-117">Ahhoz, hogy egyszerűbb dolog, a Service Fabric fürt erőforrás-kezelő biztosít a további elhelyezési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="f072c-117">To make things simpler, the Service Fabric Cluster Resource Manager provides these additional placement policies.</span></span> <span data-ttu-id="f072c-118">Elhelyezési házirendek / nevű szolgáltatás példány alapon.</span><span class="sxs-lookup"><span data-stu-id="f072c-118">Placement policies are configured on a per-named service instance basis.</span></span> <span data-ttu-id="f072c-119">Akkor is frissíthető dinamikusan.</span><span class="sxs-lookup"><span data-stu-id="f072c-119">They can also be updated dynamically.</span></span>

## <a name="specifying-invalid-domains"></a><span data-ttu-id="f072c-120">Érvénytelen tartományok megadása</span><span class="sxs-lookup"><span data-stu-id="f072c-120">Specifying invalid domains</span></span>
<span data-ttu-id="f072c-121">A **InvalidDomain** elhelyezési házirend lehetővé teszi, hogy egy adott tartalék tartomány érvénytelen egy adott szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f072c-121">The **InvalidDomain** placement policy allows you to specify that a particular Fault Domain is invalid for a specific service.</span></span> <span data-ttu-id="f072c-122">Ez a házirend biztosítja, hogy egy adott szolgáltatáshoz soha nem egy adott területen, például a geopolitikai vagy vállalati házirendek miatt.</span><span class="sxs-lookup"><span data-stu-id="f072c-122">This policy ensures that a particular service never runs in a particular area, for example for geopolitical or corporate policy reasons.</span></span> <span data-ttu-id="f072c-123">Érvénytelen többtartományos külön házirendek keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="f072c-123">Multiple invalid domains may be specified via separate policies.</span></span>

<span data-ttu-id="f072c-124"><center>
![Érvénytelen tartomány – példa][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="f072c-124"><center>
![Invalid Domain Example][Image1]
</center></span></span>

<span data-ttu-id="f072c-125">Kód:</span><span class="sxs-lookup"><span data-stu-id="f072c-125">Code:</span></span>

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="f072c-126">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="f072c-126">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a><span data-ttu-id="f072c-127">Adja meg a szükséges tartományok</span><span class="sxs-lookup"><span data-stu-id="f072c-127">Specifying required domains</span></span>
<span data-ttu-id="f072c-128">A szükséges tartományi elhelyezési házirend szükséges, hogy megtalálható-e a szolgáltatás csak a megadott tartományban.</span><span class="sxs-lookup"><span data-stu-id="f072c-128">The required domain placement policy requires that the service is present only in the specified domain.</span></span> <span data-ttu-id="f072c-129">Több szükséges tartomány külön házirendek keresztül adható meg.</span><span class="sxs-lookup"><span data-stu-id="f072c-129">Multiple required domains can be specified via separate policies.</span></span>

<span data-ttu-id="f072c-130"><center>
![Kötelező: Példa][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="f072c-130"><center>
![Required Domain Example][Image2]
</center></span></span>

<span data-ttu-id="f072c-131">Kód:</span><span class="sxs-lookup"><span data-stu-id="f072c-131">Code:</span></span>

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

<span data-ttu-id="f072c-132">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="f072c-132">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a><span data-ttu-id="f072c-133">Állapotalapú szolgáltatás elsődleges replika az elsődleges tartomány megadása</span><span class="sxs-lookup"><span data-stu-id="f072c-133">Specifying a preferred domain for the primary replicas of a stateful service</span></span>
<span data-ttu-id="f072c-134">Az előnyben részesített elsődleges tartomány az elsődleges helyezhető el a tartalék tartomány megadása</span><span class="sxs-lookup"><span data-stu-id="f072c-134">The Preferred Primary Domain specifies the fault domain to place the Primary in.</span></span> <span data-ttu-id="f072c-135">Az elsődleges fejeződik be a tartomány minden kifogástalan esetén.</span><span class="sxs-lookup"><span data-stu-id="f072c-135">The Primary ends up in this domain when everything is healthy.</span></span> <span data-ttu-id="f072c-136">Ha a tartomány vagy az elsődleges másodpéldány nem sikerül, vagy áll le, az elsődleges áthelyezi néhány más helyre, ideális ugyanabban a tartományban.</span><span class="sxs-lookup"><span data-stu-id="f072c-136">If the domain or the Primary replica fails or shuts down, the Primary moves to some other location, ideally in the same domain.</span></span> <span data-ttu-id="f072c-137">Ha az új hely nem előnyben részesített a tartományban, a fürt erőforrás-kezelő tér vissza az előnyben részesített tartományi lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="f072c-137">If this new location isn't in the preferred domain, the Cluster Resource Manager moves it back to the preferred domain as soon as possible.</span></span> <span data-ttu-id="f072c-138">Természetesen ez a beállítás csak értelme állapotalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f072c-138">Naturally this setting only makes sense for stateful services.</span></span> <span data-ttu-id="f072c-139">Ez a házirend olyan fürtökben, amelyek az Azure-régiók is átnyúlhatnak a leghasznosabb, vagy több adatközpontot azonban vannak olyan szolgáltatások, amelyek egy adott helyen elhelyezési inkább.</span><span class="sxs-lookup"><span data-stu-id="f072c-139">This policy is most useful in clusters that are spanned across Azure regions or multiple datacenters but have services that prefer placement in a certain location.</span></span> <span data-ttu-id="f072c-140">Elsődleges tartása megközelíti a felhasználók és más szolgáltatások segítségével adja meg kisebb késést biztosít, különösen az olvasások, amely alapértelmezés szerint elsődleges kezeli.</span><span class="sxs-lookup"><span data-stu-id="f072c-140">Keeping Primaries close to their users or other services helps provide lower latency, especially for reads, which are handled by Primaries by default.</span></span>

<span data-ttu-id="f072c-141"><center>
![Előnyben részesített elsődleges tartományok és a feladatátvétel][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="f072c-141"><center>
![Preferred Primary Domains and Failover][Image3]
</center></span></span>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="f072c-142">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="f072c-142">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a><span data-ttu-id="f072c-143">Replika terjesztési igénylő és csomagolási letiltása</span><span class="sxs-lookup"><span data-stu-id="f072c-143">Requiring replica distribution and disallowing packing</span></span>
<span data-ttu-id="f072c-144">Replika _általában_ elosztott hiba és a frissítési tartományok között, ha a fürt állapota kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="f072c-144">Replicas are _normally_ distributed across fault and upgrade domains when the cluster is healthy.</span></span> <span data-ttu-id="f072c-145">Azonban vannak esetek, ahol adott partíció egynél több replikát is szükségessé tehet ideiglenesen csomagolt egy tartományba.</span><span class="sxs-lookup"><span data-stu-id="f072c-145">However, there are cases where more than one replica for a given partition may end up temporarily packed into a single domain.</span></span> <span data-ttu-id="f072c-146">Például tegyük fel, hogy rendelkezik-e a fürt kilenc csomópontok a három tartalék tartományok, fd: / 0, fd: / 1 és fd: / 2.</span><span class="sxs-lookup"><span data-stu-id="f072c-146">For example, let's say that the cluster has nine nodes in three fault domains, fd:/0, fd:/1, and fd:/2.</span></span> <span data-ttu-id="f072c-147">Tételezzük is fel, hogy a szolgáltatás rendelkezik-e a három replikákat.</span><span class="sxs-lookup"><span data-stu-id="f072c-147">Let's also say that your service has three replicas.</span></span> <span data-ttu-id="f072c-148">Tegyük fel, hogy a csomópontok azokat fd található replikák volt használatban lévő: 1 és fd: / 2 csökkent.</span><span class="sxs-lookup"><span data-stu-id="f072c-148">Let's say that the nodes that were being used for those replicas in fd:/1 and fd:/2 went down.</span></span> <span data-ttu-id="f072c-149">A fürt erőforrás-kezelő általában inkább az azonos tartalék tartományokban többi csomópontjának.</span><span class="sxs-lookup"><span data-stu-id="f072c-149">Normally the Cluster Resource Manager would prefer other nodes in those same fault domains.</span></span> <span data-ttu-id="f072c-150">Ebben az esetben Tételezzük fel kapacitás problémái miatt az azokban a tartományokban a többi csomópont sem érvényes.</span><span class="sxs-lookup"><span data-stu-id="f072c-150">In this case, let's say due to capacity issues none of the other nodes in those domains were valid.</span></span> <span data-ttu-id="f072c-151">Ha a fürt erőforrás-kezelő létrehozta cserékhez említett-replikákhoz, azt kell választania a csomópontok a fd: / 0.</span><span class="sxs-lookup"><span data-stu-id="f072c-151">If the Cluster Resource Manager builds replacements for those replicas, it would have to choose nodes in fd:/0.</span></span> <span data-ttu-id="f072c-152">Azonban ez _, amely_ hoz létre olyan helyzet, ahol a tartalék tartomány korlátozás sérül.</span><span class="sxs-lookup"><span data-stu-id="f072c-152">However, doing _that_ creates a situation where the Fault Domain constraint is violated.</span></span> <span data-ttu-id="f072c-153">Replikák növeli annak esélyét, hogy a teljes replika beállítása sikerült leáll vagy elvesznek.</span><span class="sxs-lookup"><span data-stu-id="f072c-153">Packing replicas increases the chance that the whole replica set could go down or be lost.</span></span> 

> [!NOTE]
> <span data-ttu-id="f072c-154">További információ a korlátozások és a korlátozás prioritások általában, tekintse meg [ebben a témakörben](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span><span class="sxs-lookup"><span data-stu-id="f072c-154">For more information on constraints and constraint priorities generally, check out [this topic](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span></span>
>

<span data-ttu-id="f072c-155">Ha már legalább egyszer megtekintett állapotfigyelő üzenet például a "`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`", akkor ezt az állapotot, vagy azt hasonlót már elérte.</span><span class="sxs-lookup"><span data-stu-id="f072c-155">If you've ever seen a health message such as "`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`", then you've hit this condition or something like it.</span></span> <span data-ttu-id="f072c-156">Általában csak egy vagy két replikák vannak csomagolva együtt ideiglenesen.</span><span class="sxs-lookup"><span data-stu-id="f072c-156">Usually only one or two replicas are packed together temporarily.</span></span> <span data-ttu-id="f072c-157">Mindaddig, amíg nincsenek egy adott tartományban replikák kvórum nem lépi-e, tehát biztonságos.</span><span class="sxs-lookup"><span data-stu-id="f072c-157">So long as there are fewer than a quorum of replicas in a given domain, you're safe.</span></span> <span data-ttu-id="f072c-158">Csomagolási ritkán fordul elő, de akkor fordulhat elő, és általában ezekben a helyzetekben átmeneti mivel a csomópontok térjen vissza.</span><span class="sxs-lookup"><span data-stu-id="f072c-158">Packing is rare, but it can happen, and usually these situations are transient since the nodes come back.</span></span> <span data-ttu-id="f072c-159">Ha a csomópontok le marad, és a fürt erőforrás-kezelő cserékhez létrehozásához meg kell, általában nincsenek más csomópontok érhető el az ideális tartalék tartományok.</span><span class="sxs-lookup"><span data-stu-id="f072c-159">If the nodes do stay down and the Cluster Resource Manager needs to build replacements, usually there are other nodes available in the ideal fault domains.</span></span>

<span data-ttu-id="f072c-160">Bizonyos alkalmazások és szolgáltatások inkább, mindig használjon a replikákat, cél mennyiségű akkor is, ha azok csomagolt, kevesebb tartományokra.</span><span class="sxs-lookup"><span data-stu-id="f072c-160">Some workloads would prefer always having the target number of replicas, even if they are packed into fewer domains.</span></span> <span data-ttu-id="f072c-161">Az ilyen terhelések vannak fogadások teljes egyidejű állandó tartomány-hibákkal szemben, és általában helyre tudja állítani a helyi állapotát.</span><span class="sxs-lookup"><span data-stu-id="f072c-161">These workloads are betting against total simultaneous permanent domain failures and can usually recover local state.</span></span> <span data-ttu-id="f072c-162">Egyéb munkaterhelések ahelyett, hogy kellene az állásidő rendszernél kockázat nézetet, illetve az adatvesztés.</span><span class="sxs-lookup"><span data-stu-id="f072c-162">Other workloads would rather take the downtime earlier than risk correctness or loss of data.</span></span> <span data-ttu-id="f072c-163">A legtöbb termelési számítási feladatokhoz háromnál több replikákat, több mint három tartalék tartományok és tartalék tartomány sok érvényes csomópontok futtassa.</span><span class="sxs-lookup"><span data-stu-id="f072c-163">Most production workloads run with more than three replicas, more than three fault domains, and many valid nodes per fault domain.</span></span> <span data-ttu-id="f072c-164">Emiatt az alapértelmezett viselkedés lehetővé teszi, hogy tartományi csomagolási alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f072c-164">Because of this, the default behavior allows domain packing by default.</span></span> <span data-ttu-id="f072c-165">Az alapértelmezett viselkedés lehetővé teszi, hogy normál terheléselosztási és feladatátvételi szélsőséges esetben kezelni akkor is, ha ez azt jelenti, hogy ideiglenes tartomány csomagolási.</span><span class="sxs-lookup"><span data-stu-id="f072c-165">The default behavior allows normal balancing and failover to handle these extreme cases, even if that means temporary domain packing.</span></span>

<span data-ttu-id="f072c-166">Ha le szeretné tiltani egy adott munkaterhelés számára ilyen csomagolási, megadhatja a `RequireDomainDistribution` házirend a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f072c-166">If you want to disable such packing for a given workload, you can specify the `RequireDomainDistribution` policy on the service.</span></span> <span data-ttu-id="f072c-167">Ha ez a házirend be van állítva, a a fürt erőforrás-kezelő biztosítja, a partícióra két replika ugyanabban a tartományban hiba vagy frissítési futnak.</span><span class="sxs-lookup"><span data-stu-id="f072c-167">When this policy is set, the Cluster Resource Manager ensures no two replicas from the same partition run in the same fault or upgrade domain.</span></span>

<span data-ttu-id="f072c-168">Kód:</span><span class="sxs-lookup"><span data-stu-id="f072c-168">Code:</span></span>

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

<span data-ttu-id="f072c-169">PowerShell:</span><span class="sxs-lookup"><span data-stu-id="f072c-169">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

<span data-ttu-id="f072c-170">Most akkor lehet, amely földrajzilag nem ölel fürtben szolgáltatáshoz használja ezeket a beállításokat?</span><span class="sxs-lookup"><span data-stu-id="f072c-170">Now, would it be possible to use these configurations for services in a cluster that was not geographically spanned?</span></span> <span data-ttu-id="f072c-171">Meg lehetett, de nincs nagy OK túl.</span><span class="sxs-lookup"><span data-stu-id="f072c-171">You could, but there’s not a great reason too.</span></span> <span data-ttu-id="f072c-172">A szükséges, érvénytelen, mind az elsődleges tartomány konfigurációk kivéve, ha a forgatókönyvek használatához el kell kerülni.</span><span class="sxs-lookup"><span data-stu-id="f072c-172">The required, invalid, and preferred domain configurations should be avoided unless the scenarios require them.</span></span> <span data-ttu-id="f072c-173">Azt nem célszerű bármely a próbálja egy adott munkaterhelés egyetlen szekrényben futtatásához, vagy inkább a helyi fürt egyes szegmens másikkal kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="f072c-173">It doesn't make any sense to try to force a given workload to run in a single rack, or to prefer some segment of your local cluster over another.</span></span> <span data-ttu-id="f072c-174">Különböző hardverkonfigurációk legyen elosztva a tartalék tartományok és kezelt normál elhelyezési korlátozás és a csomópont tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f072c-174">Different hardware configurations should be spread across fault domains and handled via normal placement constraints and node properties.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f072c-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f072c-175">Next steps</span></span>
- <span data-ttu-id="f072c-176">A szolgáltatások konfigurálásáról [további információ a szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="f072c-176">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
