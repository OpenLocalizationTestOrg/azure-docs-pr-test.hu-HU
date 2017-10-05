---
title: "Ütemezett események Azure metaadatok szolgáltatással |} Microsoft Docs"
description: "Reagálnak Impactful események a virtuális gépen ahhoz, azok fordulhat elő."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/10/2016
ms.author: zivr
ms.openlocfilehash: 793803bfc12059a68ec881da9de37116f7a0eb8c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-metadata-service---scheduled-events-preview"></a><span data-ttu-id="1b995-103">Az Azure metaadat-szolgáltatás - ütemezett események (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="1b995-103">Azure Metadata Service - Scheduled Events (Preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="1b995-104">Az előzetes verziójú funkciók rendelkezésre álló, feltéve, hogy elfogadja a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="1b995-104">Previews are made available to you on the condition that you agree to the terms of use.</span></span> <span data-ttu-id="1b995-105">További részletekért lásd: [Kiegészítő használati feltételek a Microsoft Azure előzetes verziójú termékeihez](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="1b995-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="1b995-106">Ütemezett események egyike a subservices alatt az Azure metaadat-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b995-106">Scheduled Events is one of the subservices under the Azure Metadata Service.</span></span> <span data-ttu-id="1b995-107">Felelős felszínre hozza a jövőbeni események kapcsolatos információk (például újraindítás), az alkalmazás előkészítése őket, és korlátozza megszakítása.</span><span class="sxs-lookup"><span data-stu-id="1b995-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="1b995-108">Érhető el minden Azure virtuális gép esetében, beleértve a PaaS és IaaS.</span><span class="sxs-lookup"><span data-stu-id="1b995-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="1b995-109">Ütemezett események időpontot a virtuális gép olyan esemény hatás minimalizálása érdekében megelőző feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b995-109">Scheduled Events gives your Virtual Machine time to perform preventive tasks to minimize the effect of an event.</span></span> 

## <a name="introduction---why-scheduled-events"></a><span data-ttu-id="1b995-110">Bevezetés - miért ütemezett eseményeket?</span><span class="sxs-lookup"><span data-stu-id="1b995-110">Introduction - Why Scheduled Events?</span></span>

<span data-ttu-id="1b995-111">Ütemezett események korlátozhatják a platform-intiated karbantartás vagy felhasználó által kezdeményezett műveletek a szolgáltatás lépéseket is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="1b995-111">With Scheduled Events, you can take steps to limit the impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="1b995-112">Többpéldányos munkaterhelések esetén, amelyek replikációs technikák-állapot karbantartásához, védtelen maradhat a kimaradások esetén több példánya között történik.</span><span class="sxs-lookup"><span data-stu-id="1b995-112">Multi-instance workloads, which use replication techniques to maintain state, may be vulnerable to outages happening across multiple instances.</span></span> <span data-ttu-id="1b995-113">Pl. kimaradás költséges feladatokat (például újjáépítése indexek) vagy replika adatvesztést is okozhat.</span><span class="sxs-lookup"><span data-stu-id="1b995-113">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="1b995-114">Sok egyéb esetben a teljes szolgáltatás rendelkezésre állása növelhető a szabályos leállítást sorozat végrehajtásával épp (vagy megszakítás alatt álló) üzenetsoroktól tranzakciók, a többi virtuális gép a fürtben (kézi feladatátvételre) feladatok újbóli, vagy a virtuális gép eltávolítása egy hálózati terheléselosztó-készlet betölteni.</span><span class="sxs-lookup"><span data-stu-id="1b995-114">In many other cases, the overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks to other VMs in the cluster (manual failover), or removing the Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="1b995-115">Előfordulhatnak olyan esetek, ahol egy jövőbeli eseményről rendszergazda értesítése vagy egy ilyen esemény naplózása érdekében a felhőalapú alkalmazások szervizelhetőségét javítása.</span><span class="sxs-lookup"><span data-stu-id="1b995-115">There are cases where notifying an administrator about an upcoming event or logging such an event help improving the serviceability of applications hosted in the cloud.</span></span>

<span data-ttu-id="1b995-116">Az Azure metaadat-szolgáltatás ütemezett események Felfed, a következő esetekben használja:</span><span class="sxs-lookup"><span data-stu-id="1b995-116">Azure Metadata Service surfaces Scheduled Events in the following use cases:</span></span>
-   <span data-ttu-id="1b995-117">A platform által kezdeményezett karbantartási (például a gazda operációs Rendszerével Bevezetés)</span><span class="sxs-lookup"><span data-stu-id="1b995-117">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="1b995-118">A felhasználó által kezdeményezett hívások (például felhasználó újraindítja vagy redeploys a virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="1b995-118">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="scheduled-events---the-basics"></a><span data-ttu-id="1b995-119">Ütemezett események - az alapok</span><span class="sxs-lookup"><span data-stu-id="1b995-119">Scheduled Events - The Basics</span></span>  

<span data-ttu-id="1b995-120">Azure metaadat-szolgáltatás elérhetővé teszi a virtuális gépek használatával érhető el a virtuális Gépen belül, a REST-végpont futtatásával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="1b995-120">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within the VM.</span></span> <span data-ttu-id="1b995-121">A érhető el információ egy nem átirányítható IP-cím segítségével, hogy nincs felfedve, a virtuális gép kívül.</span><span class="sxs-lookup"><span data-stu-id="1b995-121">The information is available via a non-routable IP so that it is not exposed outside the VM.</span></span>

### <a name="scope"></a><span data-ttu-id="1b995-122">Hatókör</span><span class="sxs-lookup"><span data-stu-id="1b995-122">Scope</span></span>
<span data-ttu-id="1b995-123">Ütemezett események illesztett összes virtuális gépet egy felhőalapú szolgáltatás, vagy egy rendelkezésre állási csoportban lévő összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="1b995-123">Scheduled events are surfaced to all Virtual Machines in a cloud service or to all Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="1b995-124">Ennek eredményeképpen, jelölje be a `Resources` mező mellett az esemény azonosításához, amely a virtuális gépek is érinthet szeretné.</span><span class="sxs-lookup"><span data-stu-id="1b995-124">As a result, you should check the `Resources` field in the event to identify which VMs are going to be impacted.</span></span> 

### <a name="discovering-the-endpoint"></a><span data-ttu-id="1b995-125">A végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="1b995-125">Discovering the Endpoint</span></span>
<span data-ttu-id="1b995-126">Abban az esetben, ahol a rendszer létrehoz egy virtuális gép egy virtuális hálózatot (VNet), a metaadatok szolgáltatás nem érhető el egy statikus nem irányítható IP-cím, `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="1b995-126">In the case where a Virtual Machine is created within a Virtual Network (VNet), the metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="1b995-127">Ha a virtuális gép nem hozza létre a virtuális hálózatról, az alapértelmezett eset felhőalapú szolgáltatásokhoz és a klasszikus virtuális gépeket, további logikát kell használandó végpont felderítése.</span><span class="sxs-lookup"><span data-stu-id="1b995-127">If the Virtual Machine is not created within a Virtual Network, the default cases for cloud services and classic VMs, additional logic is required to discover the endpoint to use.</span></span> <span data-ttu-id="1b995-128">Tekintse meg ezt a mintát megtudhatja, hogyan [a gazdagép-végpont felderítése](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="1b995-128">Refer to this sample to learn how to [discover the host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="1b995-129">Versioning</span><span class="sxs-lookup"><span data-stu-id="1b995-129">Versioning</span></span> 
<span data-ttu-id="1b995-130">A példány metaadat-szolgáltatás nem rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="1b995-130">The Instance Metadata Service is versioned.</span></span> <span data-ttu-id="1b995-131">Verziók kötelező, és a jelenlegi verzió: `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="1b995-131">Versions are mandatory and the current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="1b995-132">Előző előzetes kiadásaiban ütemezett események {legújabb} támogatott api-verzióval.</span><span class="sxs-lookup"><span data-stu-id="1b995-132">Previous preview releases of scheduled events supported {latest} as the api-version.</span></span> <span data-ttu-id="1b995-133">Ez a formátum már nem támogatott, és a jövőben elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="1b995-133">This format is no longer supported and will be deprecated in the future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="1b995-134">Fejlécek használata</span><span class="sxs-lookup"><span data-stu-id="1b995-134">Using Headers</span></span>
<span data-ttu-id="1b995-135">Ha a metaadat-szolgáltatás, meg kell adnia a fejléc `Metadata: true` annak érdekében, hogy nem szándékos átirányítja a kérést.</span><span class="sxs-lookup"><span data-stu-id="1b995-135">When you query the Metadata Service, you must provide the header `Metadata: true` to ensure the request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="1b995-136">Ütemezett események engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1b995-136">Enabling Scheduled Events</span></span>
<span data-ttu-id="1b995-137">Az első alkalommal ütemezett események kérelmet használhat Azure implicit módon lehetővé teszi, hogy a szolgáltatás a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="1b995-137">The first time you make a request for scheduled events, Azure implicitly enables the feature on your Virtual Machine.</span></span> <span data-ttu-id="1b995-138">Ennek eredményeképpen kell késleltetett választ várt akár két perc alatt az első hívásakor.</span><span class="sxs-lookup"><span data-stu-id="1b995-138">As a result, you should expect a delayed response in your first call of up to two minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="1b995-139">Felhasználó által kezdeményezett karbantartás</span><span class="sxs-lookup"><span data-stu-id="1b995-139">User Initiated Maintenance</span></span>
<span data-ttu-id="1b995-140">Felhasználó kezdeményezte a virtuális gép karbantartási API, CLI-t, az Azure portálon keresztül, vagy PowerShell ütemezett események eredményez.</span><span class="sxs-lookup"><span data-stu-id="1b995-140">User initiated virtual machine maintenance via the Azure portal, API, CLI, or PowerShell will result in Scheduled Events.</span></span> <span data-ttu-id="1b995-141">Ez lehetővé teszi, hogy a karbantartási előkészítése logika tesztelni az alkalmazás, és lehetővé teszi, hogy a felhasználó által kezdeményezett karbantartási előkészítése az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1b995-141">This allows you to test the maintenance preparation logic in your application and allows your application to prepare for user initiated maintenance.</span></span>

<span data-ttu-id="1b995-142">A virtuális gép újraindítása a művelet ütemezi típusú esemény `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="1b995-142">Restarting a virtual machine will schedule an event with type `Reboot`.</span></span> <span data-ttu-id="1b995-143">Újratelepíteni a virtuális gépet a művelet ütemezi típusú esemény `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="1b995-143">Redeploying a virtual machine will schedule an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="1b995-144">Jelenleg legfeljebb 10 felhasználó által kezdeményezett karbantartási műveletek egyidejűleg ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="1b995-144">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="1b995-145">Ezt a határt fog enyhíteni ütemezett események általánosan rendelkezésre álló előtt.</span><span class="sxs-lookup"><span data-stu-id="1b995-145">This limit will be relaxed before Scheduled Events General Availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="1b995-146">Felhasználó által kezdeményezett karbantartást ütemezett esemény jelenleg nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="1b995-146">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="1b995-147">Beállíthatóság tervezünk-e egy későbbi kiadásban.</span><span class="sxs-lookup"><span data-stu-id="1b995-147">Configurability is planned for a future release.</span></span>

## <a name="using-the-api"></a><span data-ttu-id="1b995-148">Az API-val</span><span class="sxs-lookup"><span data-stu-id="1b995-148">Using the API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="1b995-149">Lekérdezést eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="1b995-149">Query for events</span></span>
<span data-ttu-id="1b995-150">Ütemezett események alapján is kereshet, egyszerűen azáltal, hogy a következő hívást:</span><span class="sxs-lookup"><span data-stu-id="1b995-150">You can query for Scheduled Events simply by making the following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="1b995-151">A válasz ütemezett események tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1b995-151">A response contains an array of scheduled events.</span></span> <span data-ttu-id="1b995-152">Üres tömb azt jelenti, hogy nincsenek-e jelenleg ütemezett események.</span><span class="sxs-lookup"><span data-stu-id="1b995-152">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="1b995-153">Abban az esetben, ahol ütemezett események, a válasz események tömbjét tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1b995-153">In the case where there are scheduled events, the response contains an array of events:</span></span> 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a><span data-ttu-id="1b995-154">Esemény tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="1b995-154">Event Properties</span></span>
|<span data-ttu-id="1b995-155">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1b995-155">Property</span></span>  |  <span data-ttu-id="1b995-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b995-156">Description</span></span> |
| - | - |
| <span data-ttu-id="1b995-157">Eseményazonosító</span><span class="sxs-lookup"><span data-stu-id="1b995-157">EventId</span></span> | <span data-ttu-id="1b995-158">Ez az esemény globálisan egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="1b995-158">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="1b995-159">Példa:</span><span class="sxs-lookup"><span data-stu-id="1b995-159">Example:</span></span> <br><ul><li><span data-ttu-id="1b995-160">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="1b995-160">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="1b995-161">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="1b995-161">EventType</span></span> | <span data-ttu-id="1b995-162">Ez az esemény hatására hatása.</span><span class="sxs-lookup"><span data-stu-id="1b995-162">Impact this event causes.</span></span> <br><br> <span data-ttu-id="1b995-163">Értékek:</span><span class="sxs-lookup"><span data-stu-id="1b995-163">Values:</span></span> <br><ul><li> <span data-ttu-id="1b995-164">`Freeze`: A virtuális gép felfüggesztése néhány másodpercig van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="1b995-164">`Freeze`: The Virtual Machine is scheduled to pause for few seconds.</span></span> <span data-ttu-id="1b995-165">A Processzor fel lesz függesztve, de nincs hatással a memória, a megnyitott fájlokat vagy a hálózati kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="1b995-165">The CPU will be suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="1b995-166">`Reboot`: Újraindítás van ütemezve a virtuális gép (nem állandó memória nem vesztek el).</span><span class="sxs-lookup"><span data-stu-id="1b995-166">`Reboot`: The Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="1b995-167">`Redeploy`: A virtuális gép áthelyezése egy másik csomópontra van ütemezve (a rövid élettartamú lemezek elvesznek).</span><span class="sxs-lookup"><span data-stu-id="1b995-167">`Redeploy`: The Virtual Machine is scheduled to move to another node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="1b995-168">ResourceType</span><span class="sxs-lookup"><span data-stu-id="1b995-168">ResourceType</span></span> | <span data-ttu-id="1b995-169">Ez az esemény hatással van erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="1b995-169">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="1b995-170">Értékek:</span><span class="sxs-lookup"><span data-stu-id="1b995-170">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="1b995-171">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="1b995-171">Resources</span></span>| <span data-ttu-id="1b995-172">Ez az esemény hatással van erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="1b995-172">List of resources this event impacts.</span></span> <span data-ttu-id="1b995-173">Ez magában foglalja a gépeket legalább egy garantáltan [frissítési tartomány](windows/manage-availability.md), azonban nem tartalmazhat a UD összes gép.</span><span class="sxs-lookup"><span data-stu-id="1b995-173">This is guaranteed to contain machines from at most one [Update Domain](windows/manage-availability.md), but may not contain all machines in the UD.</span></span> <br><br> <span data-ttu-id="1b995-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="1b995-174">Example:</span></span> <br><ul><li> <span data-ttu-id="1b995-175">["FrontEnd_IN_0", "BackEnd_IN_0"]</span><span class="sxs-lookup"><span data-stu-id="1b995-175">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="1b995-176">Esemény állapota</span><span class="sxs-lookup"><span data-stu-id="1b995-176">Event Status</span></span> | <span data-ttu-id="1b995-177">Ez az esemény állapotát.</span><span class="sxs-lookup"><span data-stu-id="1b995-177">Status of this event.</span></span> <br><br> <span data-ttu-id="1b995-178">Értékek:</span><span class="sxs-lookup"><span data-stu-id="1b995-178">Values:</span></span> <ul><li><span data-ttu-id="1b995-179">`Scheduled`: Ez az esemény után a megadott ideig történő futásra van ütemezve a `NotBefore` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1b995-179">`Scheduled`: This event is scheduled to start after the time specified in the `NotBefore` property.</span></span><li><span data-ttu-id="1b995-180">`Started`: Ez az esemény elindult.</span><span class="sxs-lookup"><span data-stu-id="1b995-180">`Started`: This event has started.</span></span></ul> <span data-ttu-id="1b995-181">Nem `Completed` vagy hasonló állapot valaha is biztosítja; az esemény már nem adható vissza, ha az esemény befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1b995-181">No `Completed` or similar status is ever provided; the event will no longer be returned when the event is completed.</span></span>
| <span data-ttu-id="1b995-182">NotBefore</span><span class="sxs-lookup"><span data-stu-id="1b995-182">NotBefore</span></span>| <span data-ttu-id="1b995-183">Az idő elteltével kezdheti el ezt az eseményt.</span><span class="sxs-lookup"><span data-stu-id="1b995-183">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="1b995-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="1b995-184">Example:</span></span> <br><ul><li> <span data-ttu-id="1b995-185">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="1b995-185">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="1b995-186">Esemény ütemezése</span><span class="sxs-lookup"><span data-stu-id="1b995-186">Event Scheduling</span></span>
<span data-ttu-id="1b995-187">Minden esemény van ütemezve egy jövőbeli időpontot minimális mennyiségű esemény típusa alapján.</span><span class="sxs-lookup"><span data-stu-id="1b995-187">Each event is scheduled a minimum amount of time in the future based on event type.</span></span> <span data-ttu-id="1b995-188">Most megjelenik egy esemény `NotBefore` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1b995-188">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="1b995-189">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="1b995-189">EventType</span></span>  | <span data-ttu-id="1b995-190">Minimális értesítés</span><span class="sxs-lookup"><span data-stu-id="1b995-190">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="1b995-191">Rögzítése</span><span class="sxs-lookup"><span data-stu-id="1b995-191">Freeze</span></span>| <span data-ttu-id="1b995-192">15 perc</span><span class="sxs-lookup"><span data-stu-id="1b995-192">15 minutes</span></span> |
| <span data-ttu-id="1b995-193">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="1b995-193">Reboot</span></span> | <span data-ttu-id="1b995-194">15 perc</span><span class="sxs-lookup"><span data-stu-id="1b995-194">15 minutes</span></span> |
| <span data-ttu-id="1b995-195">Ismételt üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="1b995-195">Redeploy</span></span> | <span data-ttu-id="1b995-196">10 perc</span><span class="sxs-lookup"><span data-stu-id="1b995-196">10 minutes</span></span> |

### <a name="starting-an-event-expedite"></a><span data-ttu-id="1b995-197">Az eseményt (gyorsítása)</span><span class="sxs-lookup"><span data-stu-id="1b995-197">Starting an event (expedite)</span></span>

<span data-ttu-id="1b995-198">Miután egy jövőbeli esemény megtanulta, és a szabályos leállítást logikát befejeződött, a függőben lévő esemény jóváhagyhatja azáltal, hogy egy `POST` az metaadat-ba irányuló hívás a `EventId`.</span><span class="sxs-lookup"><span data-stu-id="1b995-198">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve the outstanding event by making a `POST` call to the metadata service with the `EventId`.</span></span> <span data-ttu-id="1b995-199">Ez azt jelzi, az Azure-ba, hogy azt a minimális értesítési lerövidíthető (amikor csak lehetséges).</span><span class="sxs-lookup"><span data-stu-id="1b995-199">This indicates to Azure that it can shorten the minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="1b995-200">Egy esemény igazolása lehetővé teszi az esemény folytatható összes `Resources` abban az esetben, nem csak a virtuális gépet, amely elismeri az esemény.</span><span class="sxs-lookup"><span data-stu-id="1b995-200">Acknowledging a event will allow the event to proceed for all `Resources` in the event, not just the virtual machine that acknowledges the event.</span></span> <span data-ttu-id="1b995-201">Ezért választhatja, hogy egy vezető összehangolni a nyugtázási, akkor más dolga, mint az első machine kiválasztják a `Resources` mező.</span><span class="sxs-lookup"><span data-stu-id="1b995-201">You may therefore choose to elect a leader to coordinate the acknowledgement, which may be as simple as the first machine in the `Resources` field.</span></span>

## <a name="samples"></a><span data-ttu-id="1b995-202">Példák</span><span class="sxs-lookup"><span data-stu-id="1b995-202">Samples</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="1b995-203">PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="1b995-203">PowerShell Sample</span></span> 

<span data-ttu-id="1b995-204">Az alábbi minta ütemezett események a metaadatok szolgáltatást, és hagyja jóvá a függőben lévő eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="1b995-204">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


### <a name="c-sample"></a><span data-ttu-id="1b995-205">C\# minta</span><span class="sxs-lookup"><span data-stu-id="1b995-205">C\# Sample</span></span> 

<span data-ttu-id="1b995-206">Az alábbi minta nem egyszerű ügyfél, amely a metaadatokat szolgáltatással kommunikál.</span><span class="sxs-lookup"><span data-stu-id="1b995-206">The following sample is of a simple client that communicates with the metadata service.</span></span>

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up the scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

<span data-ttu-id="1b995-207">Ütemezett események jelölhető ki, a következő adatstruktúrák használatával:</span><span class="sxs-lookup"><span data-stu-id="1b995-207">Scheduled Events can be represented using the following data structures:</span></span>

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

<span data-ttu-id="1b995-208">Az alábbi minta ütemezett események a metaadatok szolgáltatást, és hagyja jóvá a függőben lévő eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="1b995-208">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

### <a name="python-sample"></a><span data-ttu-id="1b995-209">Python-minta</span><span class="sxs-lookup"><span data-stu-id="1b995-209">Python Sample</span></span> 

<span data-ttu-id="1b995-210">Az alábbi minta ütemezett események a metaadatok szolgáltatást, és hagyja jóvá a függőben lévő eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="1b995-210">The following sample queries the metadata service for scheduled events and approves each outstanding event.</span></span>

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a><span data-ttu-id="1b995-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b995-211">Next Steps</span></span> 

- <span data-ttu-id="1b995-212">További információk az elérhető API-kat a [metaadat-szolgáltatás példány](virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b995-212">Read more about the APIs available in the [instance metadata service](virtual-machines-instancemetadataservice-overview.md).</span></span>
- <span data-ttu-id="1b995-213">További tudnivalók [a tervezett karbantartások Windows virtuális gépek Azure-ban](windows/planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="1b995-213">Learn about [planned maintenance for Windows virtual machines in Azure](windows/planned-maintenance.md).</span></span>
- <span data-ttu-id="1b995-214">További tudnivalók [a tervezett karbantartások Linux virtuális gépek Azure-ban](linux/planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="1b995-214">Learn about [planned maintenance for Linux virtual machines in Azure](linux/planned-maintenance.md).</span></span>
