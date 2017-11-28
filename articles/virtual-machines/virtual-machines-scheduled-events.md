---
title: "aaaScheduled események Azure metaadatok szolgáltatással |} Microsoft Docs"
description: "Ahhoz, azok fordulhat elő, reagálni tooImpactful események a virtuális gépen."
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
ms.openlocfilehash: b5c0849958c3ab48fa9c2cbff7db62f2e9d7daf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service---scheduled-events-preview"></a><span data-ttu-id="18fbd-103">Az Azure metaadat-szolgáltatás - ütemezett események (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="18fbd-103">Azure Metadata Service - Scheduled Events (Preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="18fbd-104">Az előzetes verziójú funkciók elérhető tooyou hello feltétel, hogy elfogadja a használati feltételek toohello készülnek el.</span><span class="sxs-lookup"><span data-stu-id="18fbd-104">Previews are made available tooyou on hello condition that you agree toohello terms of use.</span></span> <span data-ttu-id="18fbd-105">További részletekért lásd: [Kiegészítő használati feltételek a Microsoft Azure előzetes verziójú termékeihez](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="18fbd-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="18fbd-106">Ütemezett események egyike hello subservices hello Azure metaadat-szolgáltatás alatt.</span><span class="sxs-lookup"><span data-stu-id="18fbd-106">Scheduled Events is one of hello subservices under hello Azure Metadata Service.</span></span> <span data-ttu-id="18fbd-107">Felelős felszínre hozza a jövőbeni események kapcsolatos információk (például újraindítás), az alkalmazás előkészítése őket, és korlátozza megszakítása.</span><span class="sxs-lookup"><span data-stu-id="18fbd-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="18fbd-108">Érhető el minden Azure virtuális gép esetében, beleértve a PaaS és IaaS.</span><span class="sxs-lookup"><span data-stu-id="18fbd-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="18fbd-109">Ütemezett események biztosít a virtuális gép idő tooperform megelőző feladatok toominimize hello hatásának egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="18fbd-109">Scheduled Events gives your Virtual Machine time tooperform preventive tasks toominimize hello effect of an event.</span></span> 

## <a name="introduction---why-scheduled-events"></a><span data-ttu-id="18fbd-110">Bevezetés - miért ütemezett eseményeket?</span><span class="sxs-lookup"><span data-stu-id="18fbd-110">Introduction - Why Scheduled Events?</span></span>

<span data-ttu-id="18fbd-111">Ütemezett események lépéseket toolimit hello hatása platform-intiated karbantartás vagy felhasználó által kezdeményezett műveletek a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="18fbd-111">With Scheduled Events, you can take steps toolimit hello impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="18fbd-112">Többpéldányos munkaterhelések esetén, amelyek replikációs technikák toomaintain állapota, lehet, hogy sebezhető toooutages több példánya között történik.</span><span class="sxs-lookup"><span data-stu-id="18fbd-112">Multi-instance workloads, which use replication techniques toomaintain state, may be vulnerable toooutages happening across multiple instances.</span></span> <span data-ttu-id="18fbd-113">Pl. kimaradás költséges feladatokat (például újjáépítése indexek) vagy replika adatvesztést is okozhat.</span><span class="sxs-lookup"><span data-stu-id="18fbd-113">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="18fbd-114">Sok más esetben hello a teljes szolgáltatás rendelkezésre állása növelhető a szabályos leállítást sorozat végrehajtásával épp (vagy megszakítás alatt álló) üzenetsoroktól tranzakciók, újbóli feladatok tooother virtuális gépek hello fürt (kézi feladatátvételre), vagy távolítsa el hello A virtuális gép hálózati load balancer készletből.</span><span class="sxs-lookup"><span data-stu-id="18fbd-114">In many other cases, hello overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks tooother VMs in hello cluster (manual failover), or removing hello Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="18fbd-115">Előfordulhatnak olyan esetek, ahol a rendszergazda egy jövőbeli eseményről, amely értesíti, vagy ilyen esemény naplózása érdekében hello szervizelhetőségét hello felhőben üzemeltetett alkalmazások fejlesztése.</span><span class="sxs-lookup"><span data-stu-id="18fbd-115">There are cases where notifying an administrator about an upcoming event or logging such an event help improving hello serviceability of applications hosted in hello cloud.</span></span>

<span data-ttu-id="18fbd-116">Az Azure metaadat-szolgáltatás felületek ütemezett események hello következő esetekben használja:</span><span class="sxs-lookup"><span data-stu-id="18fbd-116">Azure Metadata Service surfaces Scheduled Events in hello following use cases:</span></span>
-   <span data-ttu-id="18fbd-117">A platform által kezdeményezett karbantartási (például a gazda operációs Rendszerével Bevezetés)</span><span class="sxs-lookup"><span data-stu-id="18fbd-117">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="18fbd-118">A felhasználó által kezdeményezett hívások (például felhasználó újraindítja vagy redeploys a virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="18fbd-118">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="scheduled-events---hello-basics"></a><span data-ttu-id="18fbd-119">Ütemezett események - hello alapjai</span><span class="sxs-lookup"><span data-stu-id="18fbd-119">Scheduled Events - hello Basics</span></span>  

<span data-ttu-id="18fbd-120">Azure metaadat-szolgáltatás REST-végpont belülről érhetők el a virtuális gép hello használata virtuális gépek futtatásával kapcsolatos információkat tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="18fbd-120">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within hello VM.</span></span> <span data-ttu-id="18fbd-121">hello érhető el információ egy nem átirányítható IP-cím segítségével, hogy nincs felfedve hello VM kívül.</span><span class="sxs-lookup"><span data-stu-id="18fbd-121">hello information is available via a non-routable IP so that it is not exposed outside hello VM.</span></span>

### <a name="scope"></a><span data-ttu-id="18fbd-122">Hatókör</span><span class="sxs-lookup"><span data-stu-id="18fbd-122">Scope</span></span>
<span data-ttu-id="18fbd-123">Ütemezett események illesztve tooall felhőszolgáltatásban lévő virtuális gépek vagy virtuális gépek rendelkezésre állási csoport tooall.</span><span class="sxs-lookup"><span data-stu-id="18fbd-123">Scheduled events are surfaced tooall Virtual Machines in a cloud service or tooall Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="18fbd-124">Ennek eredményeképpen, jelölje be hello `Resources` hello esemény tooidentify, amely a virtuális gépek is érintett toobe mezőbe.</span><span class="sxs-lookup"><span data-stu-id="18fbd-124">As a result, you should check hello `Resources` field in hello event tooidentify which VMs are going toobe impacted.</span></span> 

### <a name="discovering-hello-endpoint"></a><span data-ttu-id="18fbd-125">Hello végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="18fbd-125">Discovering hello Endpoint</span></span>
<span data-ttu-id="18fbd-126">Hello esetében, ahol a rendszer létrehoz egy virtuális gép egy virtuális hálózatot (VNet), hello metaadat-szolgáltatás érhető el a statikus nem irányítható IP-címről `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="18fbd-126">In hello case where a Virtual Machine is created within a Virtual Network (VNet), hello metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="18fbd-127">Ha a virtuális gép hello hello alapértelmezett esetben felhőszolgáltatások és a klasszikus virtuális gépek, virtuális hálózaton belül nem jön létre további logikát szükséges toodiscover hello végpont toouse.</span><span class="sxs-lookup"><span data-stu-id="18fbd-127">If hello Virtual Machine is not created within a Virtual Network, hello default cases for cloud services and classic VMs, additional logic is required toodiscover hello endpoint toouse.</span></span> <span data-ttu-id="18fbd-128">Tekintse meg a toothis minta toolearn hogyan túl[hello gazdagép-végpont felderítése](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="18fbd-128">Refer toothis sample toolearn how too[discover hello host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="18fbd-129">Versioning</span><span class="sxs-lookup"><span data-stu-id="18fbd-129">Versioning</span></span> 
<span data-ttu-id="18fbd-130">hello példány metaadat-szolgáltatás rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="18fbd-130">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="18fbd-131">Verziók kötelező, és hello verziószámának `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="18fbd-131">Versions are mandatory and hello current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="18fbd-132">Előző előzetes kiadásaiban ütemezett események {legújabb} hello api-verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="18fbd-132">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="18fbd-133">Ez a formátum már nem támogatott, és a jövőbeli hello elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="18fbd-133">This format is no longer supported and will be deprecated in hello future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="18fbd-134">Fejlécek használata</span><span class="sxs-lookup"><span data-stu-id="18fbd-134">Using Headers</span></span>
<span data-ttu-id="18fbd-135">Ha hello metaadat-szolgáltatás, meg kell adnia hello fejléc `Metadata: true` tooensure hello kérést nem volt szándékos átirányítja.</span><span class="sxs-lookup"><span data-stu-id="18fbd-135">When you query hello Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="18fbd-136">Ütemezett események engedélyezése</span><span class="sxs-lookup"><span data-stu-id="18fbd-136">Enabling Scheduled Events</span></span>
<span data-ttu-id="18fbd-137">hello ütemezett események kérelmet használhat először Azure implicit módon engedélyezi hello szolgáltatást a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="18fbd-137">hello first time you make a request for scheduled events, Azure implicitly enables hello feature on your Virtual Machine.</span></span> <span data-ttu-id="18fbd-138">Ennek eredményeképpen kell késleltetett választ várt tootwo perc fel az első hívásakor.</span><span class="sxs-lookup"><span data-stu-id="18fbd-138">As a result, you should expect a delayed response in your first call of up tootwo minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="18fbd-139">Felhasználó által kezdeményezett karbantartás</span><span class="sxs-lookup"><span data-stu-id="18fbd-139">User Initiated Maintenance</span></span>
<span data-ttu-id="18fbd-140">Felhasználó kezdeményezte a virtuális gép karbantartási hello Azure-portálon API, a parancssori felületen keresztül, vagy PowerShell ütemezett események eredményez.</span><span class="sxs-lookup"><span data-stu-id="18fbd-140">User initiated virtual machine maintenance via hello Azure portal, API, CLI, or PowerShell will result in Scheduled Events.</span></span> <span data-ttu-id="18fbd-141">Ez lehetővé teszi az alkalmazás tootest hello karbantartási előkészítése programot, és lehetővé teszi, hogy a felhasználó által kezdeményezett karbantartás alkalmazás tooprepare.</span><span class="sxs-lookup"><span data-stu-id="18fbd-141">This allows you tootest hello maintenance preparation logic in your application and allows your application tooprepare for user initiated maintenance.</span></span>

<span data-ttu-id="18fbd-142">A virtuális gép újraindítása a művelet ütemezi típusú esemény `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="18fbd-142">Restarting a virtual machine will schedule an event with type `Reboot`.</span></span> <span data-ttu-id="18fbd-143">Újratelepíteni a virtuális gépet a művelet ütemezi típusú esemény `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="18fbd-143">Redeploying a virtual machine will schedule an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="18fbd-144">Jelenleg legfeljebb 10 felhasználó által kezdeményezett karbantartási műveletek egyidejűleg ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="18fbd-144">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="18fbd-145">Ezt a határt fog enyhíteni ütemezett események általánosan rendelkezésre álló előtt.</span><span class="sxs-lookup"><span data-stu-id="18fbd-145">This limit will be relaxed before Scheduled Events General Availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="18fbd-146">Felhasználó által kezdeményezett karbantartást ütemezett esemény jelenleg nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="18fbd-146">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="18fbd-147">Beállíthatóság tervezünk-e egy későbbi kiadásban.</span><span class="sxs-lookup"><span data-stu-id="18fbd-147">Configurability is planned for a future release.</span></span>

## <a name="using-hello-api"></a><span data-ttu-id="18fbd-148">Hello API használatával</span><span class="sxs-lookup"><span data-stu-id="18fbd-148">Using hello API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="18fbd-149">Lekérdezést eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="18fbd-149">Query for events</span></span>
<span data-ttu-id="18fbd-150">Ütemezett események egyszerűen azáltal, hogy hello következő hívás lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="18fbd-150">You can query for Scheduled Events simply by making hello following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="18fbd-151">A válasz ütemezett események tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="18fbd-151">A response contains an array of scheduled events.</span></span> <span data-ttu-id="18fbd-152">Üres tömb azt jelenti, hogy nincsenek-e jelenleg ütemezett események.</span><span class="sxs-lookup"><span data-stu-id="18fbd-152">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="18fbd-153">Hello esetben ahol ütemezett események, hello válasz események tömbjét tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="18fbd-153">In hello case where there are scheduled events, hello response contains an array of events:</span></span> 
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

### <a name="event-properties"></a><span data-ttu-id="18fbd-154">Esemény tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="18fbd-154">Event Properties</span></span>
|<span data-ttu-id="18fbd-155">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="18fbd-155">Property</span></span>  |  <span data-ttu-id="18fbd-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="18fbd-156">Description</span></span> |
| - | - |
| <span data-ttu-id="18fbd-157">Eseményazonosító</span><span class="sxs-lookup"><span data-stu-id="18fbd-157">EventId</span></span> | <span data-ttu-id="18fbd-158">Ez az esemény globálisan egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="18fbd-158">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="18fbd-159">Példa:</span><span class="sxs-lookup"><span data-stu-id="18fbd-159">Example:</span></span> <br><ul><li><span data-ttu-id="18fbd-160">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="18fbd-160">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="18fbd-161">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="18fbd-161">EventType</span></span> | <span data-ttu-id="18fbd-162">Ez az esemény hatására hatása.</span><span class="sxs-lookup"><span data-stu-id="18fbd-162">Impact this event causes.</span></span> <br><br> <span data-ttu-id="18fbd-163">Értékek:</span><span class="sxs-lookup"><span data-stu-id="18fbd-163">Values:</span></span> <br><ul><li> <span data-ttu-id="18fbd-164">`Freeze`: hello virtuális gép ütemezett toopause néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="18fbd-164">`Freeze`: hello Virtual Machine is scheduled toopause for few seconds.</span></span> <span data-ttu-id="18fbd-165">hello CPU fel lesz függesztve, de nincs hatással a memória, a megnyitott fájlokat vagy a hálózati kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="18fbd-165">hello CPU will be suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="18fbd-166">`Reboot`: virtuális gép az ütemezett újraindítás hello (a nem állandó memória elvész).</span><span class="sxs-lookup"><span data-stu-id="18fbd-166">`Reboot`: hello Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="18fbd-167">`Redeploy`: hello virtuális gép ütemezett toomove tooanother csomópont (a rövid élettartamú lemezek elvesznek).</span><span class="sxs-lookup"><span data-stu-id="18fbd-167">`Redeploy`: hello Virtual Machine is scheduled toomove tooanother node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="18fbd-168">ResourceType</span><span class="sxs-lookup"><span data-stu-id="18fbd-168">ResourceType</span></span> | <span data-ttu-id="18fbd-169">Ez az esemény hatással van erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="18fbd-169">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="18fbd-170">Értékek:</span><span class="sxs-lookup"><span data-stu-id="18fbd-170">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="18fbd-171">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="18fbd-171">Resources</span></span>| <span data-ttu-id="18fbd-172">Ez az esemény hatással van erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="18fbd-172">List of resources this event impacts.</span></span> <span data-ttu-id="18fbd-173">Legfeljebb egy ez garantáltan toocontain gépek [frissítési tartomány](windows/manage-availability.md), de nem tartalmazhat a hello UD lévő összes gépen.</span><span class="sxs-lookup"><span data-stu-id="18fbd-173">This is guaranteed toocontain machines from at most one [Update Domain](windows/manage-availability.md), but may not contain all machines in hello UD.</span></span> <br><br> <span data-ttu-id="18fbd-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="18fbd-174">Example:</span></span> <br><ul><li> <span data-ttu-id="18fbd-175">["FrontEnd_IN_0", "BackEnd_IN_0"]</span><span class="sxs-lookup"><span data-stu-id="18fbd-175">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="18fbd-176">Esemény állapota</span><span class="sxs-lookup"><span data-stu-id="18fbd-176">Event Status</span></span> | <span data-ttu-id="18fbd-177">Ez az esemény állapotát.</span><span class="sxs-lookup"><span data-stu-id="18fbd-177">Status of this event.</span></span> <br><br> <span data-ttu-id="18fbd-178">Értékek:</span><span class="sxs-lookup"><span data-stu-id="18fbd-178">Values:</span></span> <ul><li><span data-ttu-id="18fbd-179">`Scheduled`: Ez az esemény az ütemezett toostart hello megadott hello idő múlva `NotBefore` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="18fbd-179">`Scheduled`: This event is scheduled toostart after hello time specified in hello `NotBefore` property.</span></span><li><span data-ttu-id="18fbd-180">`Started`: Ez az esemény elindult.</span><span class="sxs-lookup"><span data-stu-id="18fbd-180">`Started`: This event has started.</span></span></ul> <span data-ttu-id="18fbd-181">Nem `Completed` vagy hasonló állapot valaha is biztosítja; hello esemény már nem adható vissza, ha hello esemény befejeződött.</span><span class="sxs-lookup"><span data-stu-id="18fbd-181">No `Completed` or similar status is ever provided; hello event will no longer be returned when hello event is completed.</span></span>
| <span data-ttu-id="18fbd-182">NotBefore</span><span class="sxs-lookup"><span data-stu-id="18fbd-182">NotBefore</span></span>| <span data-ttu-id="18fbd-183">Az idő elteltével kezdheti el ezt az eseményt.</span><span class="sxs-lookup"><span data-stu-id="18fbd-183">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="18fbd-184">Példa:</span><span class="sxs-lookup"><span data-stu-id="18fbd-184">Example:</span></span> <br><ul><li> <span data-ttu-id="18fbd-185">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="18fbd-185">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="18fbd-186">Esemény ütemezése</span><span class="sxs-lookup"><span data-stu-id="18fbd-186">Event Scheduling</span></span>
<span data-ttu-id="18fbd-187">Minden esemény van ütemezve egy minimális időtartama a jövőbeli hello eseménytípus alapján.</span><span class="sxs-lookup"><span data-stu-id="18fbd-187">Each event is scheduled a minimum amount of time in hello future based on event type.</span></span> <span data-ttu-id="18fbd-188">Most megjelenik egy esemény `NotBefore` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="18fbd-188">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="18fbd-189">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="18fbd-189">EventType</span></span>  | <span data-ttu-id="18fbd-190">Minimális értesítés</span><span class="sxs-lookup"><span data-stu-id="18fbd-190">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="18fbd-191">Rögzítése</span><span class="sxs-lookup"><span data-stu-id="18fbd-191">Freeze</span></span>| <span data-ttu-id="18fbd-192">15 perc</span><span class="sxs-lookup"><span data-stu-id="18fbd-192">15 minutes</span></span> |
| <span data-ttu-id="18fbd-193">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="18fbd-193">Reboot</span></span> | <span data-ttu-id="18fbd-194">15 perc</span><span class="sxs-lookup"><span data-stu-id="18fbd-194">15 minutes</span></span> |
| <span data-ttu-id="18fbd-195">Ismételt üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="18fbd-195">Redeploy</span></span> | <span data-ttu-id="18fbd-196">10 perc</span><span class="sxs-lookup"><span data-stu-id="18fbd-196">10 minutes</span></span> |

### <a name="starting-an-event-expedite"></a><span data-ttu-id="18fbd-197">Az eseményt (gyorsítása)</span><span class="sxs-lookup"><span data-stu-id="18fbd-197">Starting an event (expedite)</span></span>

<span data-ttu-id="18fbd-198">Miután egy jövőbeli esemény megtanulta, és a szabályos leállítást logikát befejeződött, függőben lévő esemény hello jóváhagyhatja azáltal, hogy egy `POST` toohello metaadat-bA hello hívás `EventId`.</span><span class="sxs-lookup"><span data-stu-id="18fbd-198">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve hello outstanding event by making a `POST` call toohello metadata service with hello `EventId`.</span></span> <span data-ttu-id="18fbd-199">Ez azt jelzi, hogy azt lerövidíthető hello minimális értesítési tooAzure (amikor csak lehetséges).</span><span class="sxs-lookup"><span data-stu-id="18fbd-199">This indicates tooAzure that it can shorten hello minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="18fbd-200">Egy esemény igazolása lehetővé teszi hello esemény tooproceed összes `Resources` hello esemény, nem csak hello virtuális gépen, amely elismeri hello esemény.</span><span class="sxs-lookup"><span data-stu-id="18fbd-200">Acknowledging a event will allow hello event tooproceed for all `Resources` in hello event, not just hello virtual machine that acknowledges hello event.</span></span> <span data-ttu-id="18fbd-201">Ezért választhatja, hogy tooelect egy vezető toocoordinate hello nyugtázása, akkor egyszerűen hello első gép hello `Resources` mező.</span><span class="sxs-lookup"><span data-stu-id="18fbd-201">You may therefore choose tooelect a leader toocoordinate hello acknowledgement, which may be as simple as hello first machine in hello `Resources` field.</span></span>

## <a name="samples"></a><span data-ttu-id="18fbd-202">Példák</span><span class="sxs-lookup"><span data-stu-id="18fbd-202">Samples</span></span>

### <a name="powershell-sample"></a><span data-ttu-id="18fbd-203">PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="18fbd-203">PowerShell Sample</span></span> 

<span data-ttu-id="18fbd-204">hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.</span><span class="sxs-lookup"><span data-stu-id="18fbd-204">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

```PowerShell
# How tooget scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How tooapprove a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create hello Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert tooJSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with hello following: `n" $approvalString

    # Post approval string tooscheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up hello scheduled events URI for a VNET-enabled VM
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


### <a name="c-sample"></a><span data-ttu-id="18fbd-205">C\# minta</span><span class="sxs-lookup"><span data-stu-id="18fbd-205">C\# Sample</span></span> 

<span data-ttu-id="18fbd-206">hello következő minta nem egyszerű ügyfél, amely hello metaadatok szolgáltatással kommunikál.</span><span class="sxs-lookup"><span data-stu-id="18fbd-206">hello following sample is of a simple client that communicates with hello metadata service.</span></span>

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up hello scheduled events URI for a VNET-enabled VM
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

<span data-ttu-id="18fbd-207">Ütemezett események jelölhető ki, a következő adatstruktúrák hello használata:</span><span class="sxs-lookup"><span data-stu-id="18fbd-207">Scheduled Events can be represented using hello following data structures:</span></span>

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

<span data-ttu-id="18fbd-208">hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.</span><span class="sxs-lookup"><span data-stu-id="18fbd-208">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

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
            Console.WriteLine("Press Enter tooapprove executing events\n");
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

            Console.WriteLine("Complete. Press enter toorepeat\n\n");
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

### <a name="python-sample"></a><span data-ttu-id="18fbd-209">Python-minta</span><span class="sxs-lookup"><span data-stu-id="18fbd-209">Python Sample</span></span> 

<span data-ttu-id="18fbd-210">hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.</span><span class="sxs-lookup"><span data-stu-id="18fbd-210">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="18fbd-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18fbd-211">Next Steps</span></span> 

- <span data-ttu-id="18fbd-212">További információk a hello hello elérhető API-k [metaadat-szolgáltatás példány](virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="18fbd-212">Read more about hello APIs available in hello [instance metadata service](virtual-machines-instancemetadataservice-overview.md).</span></span>
- <span data-ttu-id="18fbd-213">További tudnivalók [a tervezett karbantartások Windows virtuális gépek Azure-ban](windows/planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="18fbd-213">Learn about [planned maintenance for Windows virtual machines in Azure](windows/planned-maintenance.md).</span></span>
- <span data-ttu-id="18fbd-214">További tudnivalók [a tervezett karbantartások Linux virtuális gépek Azure-ban](linux/planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="18fbd-214">Learn about [planned maintenance for Linux virtual machines in Azure](linux/planned-maintenance.md).</span></span>
