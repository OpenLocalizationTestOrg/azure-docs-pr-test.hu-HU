---
title: "aaaScheduler magas rendelkezésre állása és megbízhatósága"
description: "A Feladatütemező magas rendelkezésre állású és a megbízhatóság"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 5c9efb333eb42b393adc5deea657ca99206d425e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-high-availability-and-reliability"></a><span data-ttu-id="f2733-103">A Feladatütemező magas rendelkezésre állású és a megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="f2733-103">Scheduler High-Availability and Reliability</span></span>
## <a name="azure-scheduler-high-availability"></a><span data-ttu-id="f2733-104">Azure Schedulerrel magas rendelkezésre állású</span><span class="sxs-lookup"><span data-stu-id="f2733-104">Azure Scheduler High-Availability</span></span>
<span data-ttu-id="f2733-105">Alapszintű Azure platformon service Azure Scheduler magas rendelkezésre állású és georedundáns szolgáltatás központi telepítése és a földrajzi-területi feladat replikációs.</span><span class="sxs-lookup"><span data-stu-id="f2733-105">As a core Azure platform service, Azure Scheduler is highly available and features both geo-redundant service deployment and geo-regional job replication.</span></span>

### <a name="geo-redundant-service-deployment"></a><span data-ttu-id="f2733-106">Georedundáns szolgáltatás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="f2733-106">Geo-redundant service deployment</span></span>
<span data-ttu-id="f2733-107">Azure Schedulerrel hello szinte minden földrajzi régióban, amely az Azure-ban ma felhasználói felületén keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="f2733-107">Azure Scheduler is available via hello UI in almost every geo region that's in Azure today.</span></span> <span data-ttu-id="f2733-108">hello listája, amely Azure Scheduler elérhető régiók [az itt felsorolt](https://azure.microsoft.com/regions/#services).</span><span class="sxs-lookup"><span data-stu-id="f2733-108">hello list of regions that Azure Scheduler is available in is [listed here](https://azure.microsoft.com/regions/#services).</span></span> <span data-ttu-id="f2733-109">Egy adott adatközpont üzemeltetett régióban megjelenítése nem érhető el, ha Azure Scheduler hello feladatátvételi képességének vannak úgy, hogy egy másik adatközpontból hello szolgáltatás áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="f2733-109">If a data center in a hosted region is rendered unavailable, hello failover capabilities of Azure Scheduler are such that hello service is available from another data center.</span></span>

### <a name="geo-regional-job-replication"></a><span data-ttu-id="f2733-110">Földrajzi-területi feladat replikáció</span><span class="sxs-lookup"><span data-stu-id="f2733-110">Geo-regional job replication</span></span>
<span data-ttu-id="f2733-111">Nem csak az hello Azure Scheduler előtér-kérelmek, de a saját feladat elérhető egyben georeplikált.</span><span class="sxs-lookup"><span data-stu-id="f2733-111">Not only is hello Azure Scheduler front-end available for management requests, but your own job is also geo-replicated.</span></span> <span data-ttu-id="f2733-112">Ha kimaradás van egy régió tartozik, az Azure Scheduler átadja a feladatokat, és biztosítja a hello feladat fut egy másik adatközpont hello párosított földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="f2733-112">When there’s an outage in one region, Azure Scheduler fails over and ensures that hello job is run from another data center in hello paired geographic region.</span></span>

<span data-ttu-id="f2733-113">Például ha létrehozott egy feladatot a déli középső Régiójában, Azure Scheduler automatikusan replikálja az északi középső Régiójában feladatot.</span><span class="sxs-lookup"><span data-stu-id="f2733-113">For example, if you’ve created a job in South Central US, Azure Scheduler automatically replicates that job in North Central US.</span></span> <span data-ttu-id="f2733-114">Ha hiba történik a déli középső Régiójában, Azure Scheduler biztosítja a hello feladat fut északi középső Régiójában.</span><span class="sxs-lookup"><span data-stu-id="f2733-114">When there’s a failure in South Central US, Azure Scheduler ensures that hello job is run from North Central US.</span></span> 

![][1]

<span data-ttu-id="f2733-115">Ennek eredményeképpen Azure Scheduler biztosítja, hogy az adatok marad belül hello szélesebb körű azonos földrajzi régióban egy Azure hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="f2733-115">As a result, Azure Scheduler ensures that your data stays within hello same broader geographic region in case of an Azure failure.</span></span> <span data-ttu-id="f2733-116">Ennek eredményeképpen kell nem másolatot készít a feladat csak tooadd magas rendelkezésre állás – Azure Scheduler automatikusan a feladatok a magas rendelkezésre állású képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="f2733-116">As a result, you need not duplicate your job just tooadd high availability – Azure Scheduler automatically provides high-availability capabilities for your jobs.</span></span>

## <a name="azure-scheduler-reliability"></a><span data-ttu-id="f2733-117">Azure Schedulerrel megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="f2733-117">Azure Scheduler Reliability</span></span>
<span data-ttu-id="f2733-118">Azure Schedulerrel biztosítja, hogy a saját magas rendelkezésre állású, és időt vesz igénybe egy másik módszert toouser által létrehozott feladatok.</span><span class="sxs-lookup"><span data-stu-id="f2733-118">Azure Scheduler guarantees its own high-availability and takes a different approach toouser-created jobs.</span></span> <span data-ttu-id="f2733-119">Például a feladat alkalmazhatja a HTTP-végponttal, amely nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="f2733-119">For example, your job may invoke an HTTP endpoint that’s unavailable.</span></span> <span data-ttu-id="f2733-120">Azure Schedulerrel mindazonáltal megpróbál tooexecute a feladat sikeres legyen, mivel a alternatív beállítások toodeal hibával.</span><span class="sxs-lookup"><span data-stu-id="f2733-120">Azure Scheduler nonetheless tries tooexecute your job successfully, by giving you alternative options toodeal with failure.</span></span> <span data-ttu-id="f2733-121">Azure Schedulerrel hajtja végre ezt két módon:</span><span class="sxs-lookup"><span data-stu-id="f2733-121">Azure Scheduler does this in two ways:</span></span>

### <a name="configurable-retry-policy-via-retrypolicy"></a><span data-ttu-id="f2733-122">"RetryPolicy" via konfigurálható újra házirend</span><span class="sxs-lookup"><span data-stu-id="f2733-122">Configurable Retry Policy via “retryPolicy”</span></span>
<span data-ttu-id="f2733-123">Azure Schedulerrel tooconfigure újrapróbálkozási házirendje lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="f2733-123">Azure Scheduler allows you tooconfigure a retry policy.</span></span> <span data-ttu-id="f2733-124">Alapértelmezés szerint a feladat sikertelen lesz, ha a Feladatütemező megpróbál hello feladat újra négy alkalommal, 30 másodperces időközönként.</span><span class="sxs-lookup"><span data-stu-id="f2733-124">By default, if a job fails, Scheduler tries hello job again four more times, at 30-second intervals.</span></span> <span data-ttu-id="f2733-125">Újból konfigurálhatja az újrapróbálkozási házirend toobe szigorúbb (például tízszeresének, 30 másodperces időközönként) vagy lazább (például kétszer, naponta történik.)</span><span class="sxs-lookup"><span data-stu-id="f2733-125">You may re-configure this retry policy toobe more aggressive (for example, ten times, at 30-second intervals) or looser (for example, two times, at daily intervals.)</span></span>

<span data-ttu-id="f2733-126">Amikor ez segíthet az például létrehozhat egy feladatot, amely hetente egyszer fut, és elindítja a HTTP-végponttal.</span><span class="sxs-lookup"><span data-stu-id="f2733-126">As an example of when this may help, you may create a job that runs once a week and invokes an HTTP endpoint.</span></span> <span data-ttu-id="f2733-127">Ha a feladat futtatásakor néhány órán keresztül hello HTTP-végpont nem működik, kívánt toowait egy további hét hello feladat toorun újra óta sikertelen lesz, még akkor is hello alapértelmezett újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="f2733-127">If hello HTTP endpoint is down for a few hours when your job runs, you may not want toowait one more week for hello job toorun again since even hello default retry policy will fail.</span></span> <span data-ttu-id="f2733-128">Ezekben az esetekben előfordulhat, hogy újrakonfigurálta hello szokásos újrapróbálkozási házirend tooretry három óránként (például) 30 másodpercenként helyett.</span><span class="sxs-lookup"><span data-stu-id="f2733-128">In such cases, you may reconfigure hello standard retry policy tooretry every three hours (for example) instead of every 30 seconds.</span></span>

<span data-ttu-id="f2733-129">Hogyan tooconfigure újrapróbálkozási házirendje, tekintse meg a túl toolearn[retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span><span class="sxs-lookup"><span data-stu-id="f2733-129">toolearn how tooconfigure a retry policy, refer too[retryPolicy](scheduler-concepts-terms.md#retrypolicy).</span></span>

### <a name="alternate-endpoint-configurability-via-erroraction"></a><span data-ttu-id="f2733-130">Másik végpont Beállíthatóság "errorAction" keresztül</span><span class="sxs-lookup"><span data-stu-id="f2733-130">Alternate Endpoint Configurability via “errorAction”</span></span>
<span data-ttu-id="f2733-131">Az Azure Scheduler feladatot a cél-végponthoz hello marad nem érhető el, ha Azure Scheduler visszavált toohello alternatív hibakezelő végpont az újrapróbálkozási házirendje végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="f2733-131">If hello target endpoint for your Azure Scheduler job remains unreachable, Azure Scheduler falls back toohello alternate error-handling endpoint after following its retry policy.</span></span> <span data-ttu-id="f2733-132">Ha egy másik hibakezelés végpontot úgy van beállítva, az Azure Scheduler meghívja.</span><span class="sxs-lookup"><span data-stu-id="f2733-132">If an alternate error-handling endpoint is configured, Azure Scheduler invokes it.</span></span> <span data-ttu-id="f2733-133">Egy másik végponthoz a saját feladatok hello tapasztalt hibák a magas rendelkezésre állású.</span><span class="sxs-lookup"><span data-stu-id="f2733-133">With an alternate endpoint, your own jobs are highly available in hello face of failure.</span></span>

<span data-ttu-id="f2733-134">Tegyük fel, az alábbi, ábrán hello Azure Scheduler követi az újrapróbálkozási házirend toohit egy Győri webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="f2733-134">As an example, in hello diagram below, Azure Scheduler follows its retry policy toohit a New York web service.</span></span> <span data-ttu-id="f2733-135">Után hello újrapróbálkozik a sikertelen lesz, ha nincs alternatív ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="f2733-135">After hello retries fail, it checks if there's an alternate.</span></span> <span data-ttu-id="f2733-136">Majd előre kerül, és elindítja a kérést ugyanaz a hello alternatív toohello újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="f2733-136">It then goes ahead and starts making requests toohello alternate with hello same retry policy.</span></span>

![][2]

<span data-ttu-id="f2733-137">Vegye figyelembe, hogy ugyanazon újrapróbálkozási házirendje hello tooboth hello eredeti művelet és hello másik hibaműveletet vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="f2733-137">Note that hello same retry policy applies tooboth hello original action and hello alternate error action.</span></span> <span data-ttu-id="f2733-138">Az is lehetséges toohave hello alternatív hiba művelet művelettípus kell hello fő művelet művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="f2733-138">It’s also possible toohave hello alternate error action’s action type be different from hello main action’s action type.</span></span> <span data-ttu-id="f2733-139">Például hello fő műveletétől. Előfordulhat, hogy egy HTTP-végpont meghívása kell, amíg hello hiba művelet lehet egy tároló várólista, a service bus-üzenetsorba vagy a service bus témakör művelet, amelyet hiba-naplózás.</span><span class="sxs-lookup"><span data-stu-id="f2733-139">For example, while hello main action may be invoking an HTTP endpoint, hello error action may instead be a storage queue, service bus queue, or service bus topic action that does error-logging.</span></span>

<span data-ttu-id="f2733-140">Hogyan tooconfigure egy másik végpont, tekintse meg a túl toolearn[errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span><span class="sxs-lookup"><span data-stu-id="f2733-140">toolearn how tooconfigure an alternate endpoint, refer too[errorAction](scheduler-concepts-terms.md#action-and-erroraction).</span></span>

## <a name="see-also"></a><span data-ttu-id="f2733-141">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f2733-141">See Also</span></span>
 [<span data-ttu-id="f2733-142">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="f2733-142">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="f2733-143">Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="f2733-143">Azure Scheduler concepts, terminology, and entity hierarchy</span></span>](scheduler-concepts-terms.md)

 [<span data-ttu-id="f2733-144">Az ütemező hello Azure-portálon az első lépéseiben</span><span class="sxs-lookup"><span data-stu-id="f2733-144">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="f2733-145">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="f2733-145">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="f2733-146">Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező</span><span class="sxs-lookup"><span data-stu-id="f2733-146">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="f2733-147">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="f2733-147">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="f2733-148">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="f2733-148">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="f2733-149">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="f2733-149">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="f2733-150">Kimenő hitelesítés az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="f2733-150">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
