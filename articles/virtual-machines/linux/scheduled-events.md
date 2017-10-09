---
title: "Linux virtuális gépek Azure-ban események aaaScheduled |} Microsoft Docs"
description: "Ütemezett események hello Azure metaadatok szolgáltatás használata a Linux virtuális gépeken."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: e153c8854725330acefd67d0ef542f6149170a7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a><span data-ttu-id="6d703-103">Az Azure metaadat-szolgáltatás: Ütemezett események (előzetes verzió) Linux virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="6d703-103">Azure Metadata Service: Scheduled Events (Preview) for Linux VMs</span></span>

> [!NOTE] 
> <span data-ttu-id="6d703-104">Az előzetes verziójú funkciók elérhető tooyou hello feltétel, hogy elfogadja a használati feltételek toohello készülnek el.</span><span class="sxs-lookup"><span data-stu-id="6d703-104">Previews are made available tooyou on hello condition that you agree toohello terms of use.</span></span> <span data-ttu-id="6d703-105">További részletekért lásd: [Kiegészítő használati feltételek a Microsoft Azure előzetes verziójú termékeihez](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span><span class="sxs-lookup"><span data-stu-id="6d703-105">For more information, see [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).</span></span>
>

<span data-ttu-id="6d703-106">Ütemezett események egyike hello subservices hello Azure metaadat-szolgáltatás alatt.</span><span class="sxs-lookup"><span data-stu-id="6d703-106">Scheduled Events is one of hello subservices under hello Azure Metadata Service.</span></span> <span data-ttu-id="6d703-107">Felelős felszínre hozza a jövőbeni események kapcsolatos információk (például újraindítás), az alkalmazás előkészítése őket, és korlátozza megszakítása.</span><span class="sxs-lookup"><span data-stu-id="6d703-107">It is responsible for surfacing information regarding upcoming events (for example, reboot) so your application can prepare for them and limit disruption.</span></span> <span data-ttu-id="6d703-108">Érhető el minden Azure virtuális gép esetében, beleértve a PaaS és IaaS.</span><span class="sxs-lookup"><span data-stu-id="6d703-108">It is available for all Azure Virtual Machine types including PaaS and IaaS.</span></span> <span data-ttu-id="6d703-109">Ütemezett események biztosít a virtuális gép idő tooperform megelőző feladatok toominimize hello hatásának egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="6d703-109">Scheduled Events gives your Virtual Machine time tooperform preventive tasks toominimize hello effect of an event.</span></span> 

<span data-ttu-id="6d703-110">A Windows és a Linux virtuális gépek ütemezett események érhető el.</span><span class="sxs-lookup"><span data-stu-id="6d703-110">Scheduled Events is available for both Windows and Linux VMs.</span></span> <span data-ttu-id="6d703-111">További információk a Windows az ütemezett eseményekről: [ütemezett eseményeket a Windows virtuális gépek](../windows/scheduled-events.md).</span><span class="sxs-lookup"><span data-stu-id="6d703-111">For information about Scheduled Events on Windows, see [Scheduled Events for Windows VMs](../windows/scheduled-events.md).</span></span>

## <a name="why-scheduled-events"></a><span data-ttu-id="6d703-112">Miért ütemezett eseményeket?</span><span class="sxs-lookup"><span data-stu-id="6d703-112">Why Scheduled Events?</span></span>

<span data-ttu-id="6d703-113">Ütemezett események lépéseket toolimit hello hatása platform-intiated karbantartás vagy felhasználó által kezdeményezett műveletek a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6d703-113">With Scheduled Events, you can take steps toolimit hello impact of platform-intiated maintenance or user-initiated actions on your service.</span></span> 

<span data-ttu-id="6d703-114">Többpéldányos munkaterhelések esetén, amelyek replikációs technikák toomaintain állapota, lehet, hogy sebezhető toooutages több példánya között történik.</span><span class="sxs-lookup"><span data-stu-id="6d703-114">Multi-instance workloads, which use replication techniques toomaintain state, may be vulnerable toooutages happening across multiple instances.</span></span> <span data-ttu-id="6d703-115">Pl. kimaradás költséges feladatokat (például újjáépítése indexek) vagy replika adatvesztést is okozhat.</span><span class="sxs-lookup"><span data-stu-id="6d703-115">Such outages may result in expensive tasks (for example, rebuilding indexes) or even a replica loss.</span></span> 

<span data-ttu-id="6d703-116">Sok más esetben hello a teljes szolgáltatás rendelkezésre állása növelhető a szabályos leállítást sorozat végrehajtásával épp (vagy megszakítás alatt álló) üzenetsoroktól tranzakciók, újbóli feladatok tooother virtuális gépek hello fürt (kézi feladatátvételre), vagy távolítsa el hello A virtuális gép hálózati load balancer készletből.</span><span class="sxs-lookup"><span data-stu-id="6d703-116">In many other cases, hello overall service availability may be improved by performing a graceful shutdown sequence such as completing (or canceling) in-flight transactions, reassigning tasks tooother VMs in hello cluster (manual failover), or removing hello Virtual Machine from a network load balancer pool.</span></span> 

<span data-ttu-id="6d703-117">Előfordulhatnak olyan esetek, ahol a rendszergazda egy jövőbeli eseményről, amely értesíti, vagy ilyen esemény naplózása érdekében hello szervizelhetőségét hello felhőben üzemeltetett alkalmazások fejlesztése.</span><span class="sxs-lookup"><span data-stu-id="6d703-117">There are cases where notifying an administrator about an upcoming event or logging such an event help improving hello serviceability of applications hosted in hello cloud.</span></span>

<span data-ttu-id="6d703-118">Az Azure metaadat-szolgáltatás felületek ütemezett események hello következő esetekben használja:</span><span class="sxs-lookup"><span data-stu-id="6d703-118">Azure Metadata Service surfaces Scheduled Events in hello following use cases:</span></span>
-   <span data-ttu-id="6d703-119">A platform által kezdeményezett karbantartási (például a gazda operációs Rendszerével Bevezetés)</span><span class="sxs-lookup"><span data-stu-id="6d703-119">Platform initiated maintenance (for example, Host OS rollout)</span></span>
-   <span data-ttu-id="6d703-120">A felhasználó által kezdeményezett hívások (például felhasználó újraindítja vagy redeploys a virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="6d703-120">User-initiated calls (for example, user restarts or redeploys a VM)</span></span>


## <a name="hello-basics"></a><span data-ttu-id="6d703-121">hello alapjai</span><span class="sxs-lookup"><span data-stu-id="6d703-121">hello basics</span></span>  

<span data-ttu-id="6d703-122">Azure metaadat-szolgáltatás REST-végpont belülről érhetők el a virtuális gép hello használata virtuális gépek futtatásával kapcsolatos információkat tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="6d703-122">Azure Metadata service exposes information about running Virtual Machines using a REST Endpoint accessible from within hello VM.</span></span> <span data-ttu-id="6d703-123">hello érhető el információ egy nem átirányítható IP-cím segítségével, hogy nincs felfedve hello VM kívül.</span><span class="sxs-lookup"><span data-stu-id="6d703-123">hello information is available via a non-routable IP so that it is not exposed outside hello VM.</span></span>

### <a name="scope"></a><span data-ttu-id="6d703-124">Hatókör</span><span class="sxs-lookup"><span data-stu-id="6d703-124">Scope</span></span>
<span data-ttu-id="6d703-125">Ütemezett események illesztve tooall felhőszolgáltatásban lévő virtuális gépek vagy virtuális gépek rendelkezésre állási csoport tooall.</span><span class="sxs-lookup"><span data-stu-id="6d703-125">Scheduled events are surfaced tooall Virtual Machines in a cloud service or tooall Virtual Machines in an Availability Set.</span></span> <span data-ttu-id="6d703-126">Ennek eredményeképpen, jelölje be hello `Resources` hello esemény tooidentify, amely a virtuális gépek is érintett toobe mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6d703-126">As a result, you should check hello `Resources` field in hello event tooidentify which VMs are going toobe impacted.</span></span> 

### <a name="discovering-hello-endpoint"></a><span data-ttu-id="6d703-127">Hello végpont felderítése</span><span class="sxs-lookup"><span data-stu-id="6d703-127">Discovering hello endpoint</span></span>
<span data-ttu-id="6d703-128">Hello esetében, ahol a rendszer létrehoz egy virtuális gép egy virtuális hálózatot (VNet), hello metaadat-szolgáltatás érhető el a statikus nem irányítható IP-címről `169.254.169.254`.</span><span class="sxs-lookup"><span data-stu-id="6d703-128">In hello case where a Virtual Machine is created within a Virtual Network (VNet), hello metadata service is available from a static non-routable IP, `169.254.169.254`.</span></span>
<span data-ttu-id="6d703-129">Ha a virtuális gép hello hello alapértelmezett esetben felhőszolgáltatások és a klasszikus virtuális gépek, virtuális hálózaton belül nem jön létre további logikát szükséges toodiscover hello végpont toouse.</span><span class="sxs-lookup"><span data-stu-id="6d703-129">If hello Virtual Machine is not created within a Virtual Network, hello default cases for cloud services and classic VMs, additional logic is required toodiscover hello endpoint toouse.</span></span> <span data-ttu-id="6d703-130">Tekintse meg a toothis minta toolearn hogyan túl[hello gazdagép-végpont felderítése](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span><span class="sxs-lookup"><span data-stu-id="6d703-130">Refer toothis sample toolearn how too[discover hello host endpoint](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).</span></span>

### <a name="versioning"></a><span data-ttu-id="6d703-131">Versioning</span><span class="sxs-lookup"><span data-stu-id="6d703-131">Versioning</span></span> 
<span data-ttu-id="6d703-132">hello példány metaadat-szolgáltatás rendszerverzióval ellátott.</span><span class="sxs-lookup"><span data-stu-id="6d703-132">hello Instance Metadata Service is versioned.</span></span> <span data-ttu-id="6d703-133">Verziók kötelező, és hello verziószámának `2017-03-01`.</span><span class="sxs-lookup"><span data-stu-id="6d703-133">Versions are mandatory and hello current version is `2017-03-01`.</span></span>

> [!NOTE] 
> <span data-ttu-id="6d703-134">Előző előzetes kiadásaiban ütemezett események {legújabb} hello api-verziója támogatott.</span><span class="sxs-lookup"><span data-stu-id="6d703-134">Previous preview releases of scheduled events supported {latest} as hello api-version.</span></span> <span data-ttu-id="6d703-135">Ez a formátum már nem támogatott, és a jövőbeli hello elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="6d703-135">This format is no longer supported and will be deprecated in hello future.</span></span>

### <a name="using-headers"></a><span data-ttu-id="6d703-136">Fejlécek használata</span><span class="sxs-lookup"><span data-stu-id="6d703-136">Using headers</span></span>
<span data-ttu-id="6d703-137">Ha hello metaadat-szolgáltatás, meg kell adnia hello fejléc `Metadata: true` tooensure hello kérést nem volt szándékos átirányítja.</span><span class="sxs-lookup"><span data-stu-id="6d703-137">When you query hello Metadata Service, you must provide hello header `Metadata: true` tooensure hello request was not unintentionally redirected.</span></span>

### <a name="enabling-scheduled-events"></a><span data-ttu-id="6d703-138">Ütemezett események engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6d703-138">Enabling Scheduled Events</span></span>
<span data-ttu-id="6d703-139">hello ütemezett események kérelmet használhat először Azure implicit módon engedélyezi hello szolgáltatást a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="6d703-139">hello first time you make a request for scheduled events, Azure implicitly enables hello feature on your Virtual Machine.</span></span> <span data-ttu-id="6d703-140">Ennek eredményeképpen kell késleltetett választ várt tootwo perc fel az első hívásakor.</span><span class="sxs-lookup"><span data-stu-id="6d703-140">As a result, you should expect a delayed response in your first call of up tootwo minutes.</span></span>

### <a name="user-initiated-maintenance"></a><span data-ttu-id="6d703-141">Felhasználó által kezdeményezett karbantartás</span><span class="sxs-lookup"><span data-stu-id="6d703-141">User initiated maintenance</span></span>
<span data-ttu-id="6d703-142">Felhasználó kezdeményezte a virtuális gép karbantartási hello Azure-portálon API, a parancssori felületen keresztül, vagy PowerShell ütemezett esemény eredményez.</span><span class="sxs-lookup"><span data-stu-id="6d703-142">User initiated virtual machine maintenance via hello Azure portal, API, CLI, or PowerShell results in a scheduled event.</span></span> <span data-ttu-id="6d703-143">Ez lehetővé teszi az alkalmazás tootest hello karbantartási előkészítése programot, és lehetővé teszi, hogy a felhasználó által kezdeményezett karbantartás alkalmazás tooprepare.</span><span class="sxs-lookup"><span data-stu-id="6d703-143">This allows you tootest hello maintenance preparation logic in your application and allows your application tooprepare for user initiated maintenance.</span></span>

<span data-ttu-id="6d703-144">A virtuális gép újraindítása ütemezi típusú esemény `Reboot`.</span><span class="sxs-lookup"><span data-stu-id="6d703-144">Restarting a virtual machine schedules an event with type `Reboot`.</span></span> <span data-ttu-id="6d703-145">Újratelepíteni a virtuális gépet ütemezi a következő típusú eseményre `Redeploy`.</span><span class="sxs-lookup"><span data-stu-id="6d703-145">Redeploying a virtual machine schedules an event with type `Redeploy`.</span></span>

> [!NOTE] 
> <span data-ttu-id="6d703-146">Jelenleg legfeljebb 10 felhasználó által kezdeményezett karbantartási műveletek egyidejűleg ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="6d703-146">Currently a maximum of 10 user initiated maintenance operations can be simultaneously scheduled.</span></span> <span data-ttu-id="6d703-147">Ezt a határt fog enyhíteni ütemezett események általánosan rendelkezésre álló előtt.</span><span class="sxs-lookup"><span data-stu-id="6d703-147">This limit will be relaxed before Scheduled Events general availability.</span></span>

> [!NOTE] 
> <span data-ttu-id="6d703-148">Felhasználó által kezdeményezett karbantartást ütemezett esemény jelenleg nem konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="6d703-148">Currently user initiated maintenance resulting in Scheduled Events is not configurable.</span></span> <span data-ttu-id="6d703-149">Beállíthatóság tervezünk-e egy későbbi kiadásban.</span><span class="sxs-lookup"><span data-stu-id="6d703-149">Configurability is planned for a future release.</span></span>

## <a name="using-hello-api"></a><span data-ttu-id="6d703-150">Hello API használatával</span><span class="sxs-lookup"><span data-stu-id="6d703-150">Using hello API</span></span>

### <a name="query-for-events"></a><span data-ttu-id="6d703-151">Lekérdezést eseményekhez.</span><span class="sxs-lookup"><span data-stu-id="6d703-151">Query for events</span></span>
<span data-ttu-id="6d703-152">Ütemezett események egyszerűen azáltal, hogy hello következő hívás lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="6d703-152">You can query for Scheduled Events simply by making hello following call:</span></span>

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

<span data-ttu-id="6d703-153">A válasz ütemezett események tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6d703-153">A response contains an array of scheduled events.</span></span> <span data-ttu-id="6d703-154">Üres tömb azt jelenti, hogy nincsenek-e jelenleg ütemezett események.</span><span class="sxs-lookup"><span data-stu-id="6d703-154">An empty array means that there are currently no events scheduled.</span></span>
<span data-ttu-id="6d703-155">Hello esetben ahol ütemezett események, hello válasz események tömbjét tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6d703-155">In hello case where there are scheduled events, hello response contains an array of events:</span></span> 
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

### <a name="event-properties"></a><span data-ttu-id="6d703-156">Esemény tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6d703-156">Event properties</span></span>
|<span data-ttu-id="6d703-157">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6d703-157">Property</span></span>  |  <span data-ttu-id="6d703-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="6d703-158">Description</span></span> |
| - | - |
| <span data-ttu-id="6d703-159">Eseményazonosító</span><span class="sxs-lookup"><span data-stu-id="6d703-159">EventId</span></span> | <span data-ttu-id="6d703-160">Ez az esemény globálisan egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="6d703-160">Globally unique identifier for this event.</span></span> <br><br> <span data-ttu-id="6d703-161">Példa:</span><span class="sxs-lookup"><span data-stu-id="6d703-161">Example:</span></span> <br><ul><li><span data-ttu-id="6d703-162">602d9444-d2cd-49c7-8624-8643e7171297</span><span class="sxs-lookup"><span data-stu-id="6d703-162">602d9444-d2cd-49c7-8624-8643e7171297</span></span>  |
| <span data-ttu-id="6d703-163">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="6d703-163">EventType</span></span> | <span data-ttu-id="6d703-164">Ez az esemény hatására hatása.</span><span class="sxs-lookup"><span data-stu-id="6d703-164">Impact this event causes.</span></span> <br><br> <span data-ttu-id="6d703-165">Értékek:</span><span class="sxs-lookup"><span data-stu-id="6d703-165">Values:</span></span> <br><ul><li> <span data-ttu-id="6d703-166">`Freeze`: hello virtuális gép ütemezett toopause néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="6d703-166">`Freeze`: hello Virtual Machine is scheduled toopause for few seconds.</span></span> <span data-ttu-id="6d703-167">hello CPU fel van függesztve, de nincs hatással a memória, a megnyitott fájlokat vagy a hálózati kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="6d703-167">hello CPU is suspended, but there is no impact on memory, open files, or network connections.</span></span> <li><span data-ttu-id="6d703-168">`Reboot`: virtuális gép az ütemezett újraindítás hello (a nem állandó memória elvész).</span><span class="sxs-lookup"><span data-stu-id="6d703-168">`Reboot`: hello Virtual Machine is scheduled for reboot (non-persistent memory is lost).</span></span> <li><span data-ttu-id="6d703-169">`Redeploy`: hello virtuális gép ütemezett toomove tooanother csomópont (a rövid élettartamú lemezek elvesznek).</span><span class="sxs-lookup"><span data-stu-id="6d703-169">`Redeploy`: hello Virtual Machine is scheduled toomove tooanother node (ephemeral disks are lost).</span></span> |
| <span data-ttu-id="6d703-170">ResourceType</span><span class="sxs-lookup"><span data-stu-id="6d703-170">ResourceType</span></span> | <span data-ttu-id="6d703-171">Ez az esemény hatással van erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="6d703-171">Type of resource this event impacts.</span></span> <br><br> <span data-ttu-id="6d703-172">Értékek:</span><span class="sxs-lookup"><span data-stu-id="6d703-172">Values:</span></span> <ul><li>`VirtualMachine`|
| <span data-ttu-id="6d703-173">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="6d703-173">Resources</span></span>| <span data-ttu-id="6d703-174">Ez az esemény hatással van erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="6d703-174">List of resources this event impacts.</span></span> <span data-ttu-id="6d703-175">Legfeljebb egy ez garantáltan toocontain gépek [frissítési tartomány](manage-availability.md), de nem tartalmazhat a hello UD lévő összes gépen.</span><span class="sxs-lookup"><span data-stu-id="6d703-175">This is guaranteed toocontain machines from at most one [Update Domain](manage-availability.md), but may not contain all machines in hello UD.</span></span> <br><br> <span data-ttu-id="6d703-176">Példa:</span><span class="sxs-lookup"><span data-stu-id="6d703-176">Example:</span></span> <br><ul><li> <span data-ttu-id="6d703-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span><span class="sxs-lookup"><span data-stu-id="6d703-177">["FrontEnd_IN_0", "BackEnd_IN_0"]</span></span> |
| <span data-ttu-id="6d703-178">Esemény állapota</span><span class="sxs-lookup"><span data-stu-id="6d703-178">Event Status</span></span> | <span data-ttu-id="6d703-179">Ez az esemény állapotát.</span><span class="sxs-lookup"><span data-stu-id="6d703-179">Status of this event.</span></span> <br><br> <span data-ttu-id="6d703-180">Értékek:</span><span class="sxs-lookup"><span data-stu-id="6d703-180">Values:</span></span> <ul><li><span data-ttu-id="6d703-181">`Scheduled`: Ez az esemény az ütemezett toostart hello megadott hello idő múlva `NotBefore` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="6d703-181">`Scheduled`: This event is scheduled toostart after hello time specified in hello `NotBefore` property.</span></span><li><span data-ttu-id="6d703-182">`Started`: Ez az esemény elindult.</span><span class="sxs-lookup"><span data-stu-id="6d703-182">`Started`: This event has started.</span></span></ul> <span data-ttu-id="6d703-183">Nem `Completed` vagy hasonló állapot valaha is biztosítja; hello esemény már nem adható vissza, ha hello esemény befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6d703-183">No `Completed` or similar status is ever provided; hello event will no longer be returned when hello event is completed.</span></span>
| <span data-ttu-id="6d703-184">NotBefore</span><span class="sxs-lookup"><span data-stu-id="6d703-184">NotBefore</span></span>| <span data-ttu-id="6d703-185">Az idő elteltével kezdheti el ezt az eseményt.</span><span class="sxs-lookup"><span data-stu-id="6d703-185">Time after which this event may start.</span></span> <br><br> <span data-ttu-id="6d703-186">Példa:</span><span class="sxs-lookup"><span data-stu-id="6d703-186">Example:</span></span> <br><ul><li> <span data-ttu-id="6d703-187">2016-09-19T18:29:47Z</span><span class="sxs-lookup"><span data-stu-id="6d703-187">2016-09-19T18:29:47Z</span></span>  |

### <a name="event-scheduling"></a><span data-ttu-id="6d703-188">Esemény ütemezése</span><span class="sxs-lookup"><span data-stu-id="6d703-188">Event scheduling</span></span>
<span data-ttu-id="6d703-189">Minden esemény van ütemezve egy minimális időtartama a jövőbeli hello eseménytípus alapján.</span><span class="sxs-lookup"><span data-stu-id="6d703-189">Each event is scheduled a minimum amount of time in hello future based on event type.</span></span> <span data-ttu-id="6d703-190">Most megjelenik egy esemény `NotBefore` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="6d703-190">This time is reflected in an event's `NotBefore` property.</span></span> 

|<span data-ttu-id="6d703-191">Esemény típusa</span><span class="sxs-lookup"><span data-stu-id="6d703-191">EventType</span></span>  | <span data-ttu-id="6d703-192">Minimális értesítés</span><span class="sxs-lookup"><span data-stu-id="6d703-192">Minimum Notice</span></span> |
| - | - |
| <span data-ttu-id="6d703-193">Rögzítése</span><span class="sxs-lookup"><span data-stu-id="6d703-193">Freeze</span></span>| <span data-ttu-id="6d703-194">15 perc</span><span class="sxs-lookup"><span data-stu-id="6d703-194">15 minutes</span></span> |
| <span data-ttu-id="6d703-195">Újraindítás</span><span class="sxs-lookup"><span data-stu-id="6d703-195">Reboot</span></span> | <span data-ttu-id="6d703-196">15 perc</span><span class="sxs-lookup"><span data-stu-id="6d703-196">15 minutes</span></span> |
| <span data-ttu-id="6d703-197">Ismételt üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="6d703-197">Redeploy</span></span> | <span data-ttu-id="6d703-198">10 perc</span><span class="sxs-lookup"><span data-stu-id="6d703-198">10 minutes</span></span> |

### <a name="starting-an-event"></a><span data-ttu-id="6d703-199">Egy esemény indítása</span><span class="sxs-lookup"><span data-stu-id="6d703-199">Starting an event</span></span> 

<span data-ttu-id="6d703-200">Miután egy jövőbeli esemény megtanulta, és a szabályos leállítást logikát befejeződött, függőben lévő esemény hello jóváhagyhatja azáltal, hogy egy `POST` toohello metaadat-bA hello hívás `EventId`.</span><span class="sxs-lookup"><span data-stu-id="6d703-200">Once you have learned of an upcoming event and completed your logic for graceful shutdown, you can approve hello outstanding event by making a `POST` call toohello metadata service with hello `EventId`.</span></span> <span data-ttu-id="6d703-201">Ez azt jelzi, hogy azt lerövidíthető hello minimális értesítési tooAzure (amikor csak lehetséges).</span><span class="sxs-lookup"><span data-stu-id="6d703-201">This indicates tooAzure that it can shorten hello minimum notification time (when possible).</span></span> 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> <span data-ttu-id="6d703-202">Egy esemény igazolása lehetővé teszi, hogy hello esemény tooproceed összes `Resources` hello esemény, nem csak hello virtuális gépen, amely elismeri hello esemény.</span><span class="sxs-lookup"><span data-stu-id="6d703-202">Acknowledging an event allows hello event tooproceed for all `Resources` in hello event, not just hello virtual machine that acknowledges hello event.</span></span> <span data-ttu-id="6d703-203">Ezért választhatja, hogy tooelect egy vezető toocoordinate hello nyugtázása, akkor egyszerűen hello első gép hello `Resources` mező.</span><span class="sxs-lookup"><span data-stu-id="6d703-203">You may therefore choose tooelect a leader toocoordinate hello acknowledgement, which may be as simple as hello first machine in hello `Resources` field.</span></span>




## <a name="python-sample"></a><span data-ttu-id="6d703-204">Python-minta</span><span class="sxs-lookup"><span data-stu-id="6d703-204">Python sample</span></span> 

<span data-ttu-id="6d703-205">hello alábbi mintalekérdezések hello ütemezett események metaadat-szolgáltatás, és hagyja jóvá a függőben lévő események.</span><span class="sxs-lookup"><span data-stu-id="6d703-205">hello following sample queries hello metadata service for scheduled events and approves each outstanding event.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6d703-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d703-206">Next steps</span></span> 

- <span data-ttu-id="6d703-207">További információk a hello hello elérhető API-k [példány metaadat-szolgáltatás](instance-metadata-service.md).</span><span class="sxs-lookup"><span data-stu-id="6d703-207">Read more about hello APIs available in hello [Instance Metadata service](instance-metadata-service.md).</span></span>
- <span data-ttu-id="6d703-208">További tudnivalók [a tervezett karbantartások Linux virtuális gépek Azure-ban](planned-maintenance.md).</span><span class="sxs-lookup"><span data-stu-id="6d703-208">Learn about [planned maintenance for Linux virtual machines in Azure](planned-maintenance.md).</span></span>
