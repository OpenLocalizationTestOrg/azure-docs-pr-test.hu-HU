---
title: "aaaHandling esemény sorrendjét és az Azure Stream Analytics késedelmesség |} Microsoft Docs"
description: "Tudnivalók a Stream Analytics-soron vagy késői események adatfolyamban működéséről."
keywords: "nem megfelelő sorrendben, késői, események"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 87c028662fbafbf4f72f57f215d017f65bb649f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a><span data-ttu-id="07b56-104">Az Azure Stream Analytics rendelés eseménykezelésnek</span><span class="sxs-lookup"><span data-stu-id="07b56-104">Azure Stream Analytics event order handling</span></span>

<span data-ttu-id="07b56-105">A historikus adatfolyam események minden esemény rögzítése a hello idővel hello esemény érkezik.</span><span class="sxs-lookup"><span data-stu-id="07b56-105">In a temporal data stream of events, each event is recorded with hello time that hello event is received.</span></span> <span data-ttu-id="07b56-106">Egyes feltételek okozhat toooccasionally kap eseményfolyamokat egyes események egy másik ahhoz, mint ami lettek küldve.</span><span class="sxs-lookup"><span data-stu-id="07b56-106">Some conditions might cause event streams toooccasionally receive some events in a different order than which they were sent.</span></span> <span data-ttu-id="07b56-107">Egy egyszerű TCP ismét, vagy akár közötti eszköz- és fogadását az event hubs hello küld hello időbeállításainak okozhat a toooccur.</span><span class="sxs-lookup"><span data-stu-id="07b56-107">A simple TCP retransmit, or even a clock skew between hello sending device and hello receiving event hub might cause this toooccur.</span></span> <span data-ttu-id="07b56-108">"Írásjelek" események is kerülnek tooreceived eseményfolyamokat, tooadvance hello idő esemény érkezők hello hiányában.</span><span class="sxs-lookup"><span data-stu-id="07b56-108">“Punctuation” events also are added tooreceived event streams, tooadvance hello time in hello absence of event arrivals.</span></span> <span data-ttu-id="07b56-109">Ezek szükségesek a helyzetekben, például "Értesítés kérek, ha nincs bejelentkezések 3 perc következik be."</span><span class="sxs-lookup"><span data-stu-id="07b56-109">These are needed in scenarios like “Notify me when no logins occur for 3 minutes."</span></span>

<span data-ttu-id="07b56-110">Bemeneti adatfolyamot, amelyek nincsenek sorrendben vagy szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="07b56-110">Input streams that are not in order are either:</span></span>
* <span data-ttu-id="07b56-111">Rendezett (és ezért **késleltetett**).</span><span class="sxs-lookup"><span data-stu-id="07b56-111">Sorted (and therefore **delayed**).</span></span>
* <span data-ttu-id="07b56-112">Módosított hello rendszer tooa felhasználó által megadott házirend szerint.</span><span class="sxs-lookup"><span data-stu-id="07b56-112">Adjusted by hello system, according tooa user-specified policy.</span></span>


## <a name="lateness-tolerance"></a><span data-ttu-id="07b56-113">Késedelmesség tolerancia</span><span class="sxs-lookup"><span data-stu-id="07b56-113">Lateness tolerance</span></span>
<span data-ttu-id="07b56-114">A Stream Analytics eltűr ilyen típusú forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="07b56-114">Stream Analytics tolerates these types of scenarios.</span></span> <span data-ttu-id="07b56-115">A Stream Analytics "soron-" és "késői" események kezelésére van.</span><span class="sxs-lookup"><span data-stu-id="07b56-115">Stream Analytics has handling for "out-of-order" and "late" events.</span></span> <span data-ttu-id="07b56-116">Ezek az események a következő módokon hello kezelési:</span><span class="sxs-lookup"><span data-stu-id="07b56-116">It handles these events in hello following ways:</span></span>

* <span data-ttu-id="07b56-117">Az események sorrendje nem érkeznek, de belül hello beállítása tolerancia **által időbélyeg átrendezésekor**.</span><span class="sxs-lookup"><span data-stu-id="07b56-117">Events that arrive out of order but within hello set tolerance are **reordered by timestamp**.</span></span>
* <span data-ttu-id="07b56-118">Legkésőbb tolerancia érkező események **eldobni vagy módosítani**.</span><span class="sxs-lookup"><span data-stu-id="07b56-118">Events that arrive later than tolerance are **dropped or adjusted**.</span></span>
    * <span data-ttu-id="07b56-119">**Igazítva**: beállított tooappear toohave érkező hello legújabb elfogadható idő.</span><span class="sxs-lookup"><span data-stu-id="07b56-119">**Adjusted**: Adjusted tooappear toohave arrived at hello latest acceptable time.</span></span>
    * <span data-ttu-id="07b56-120">**Eldobott**: vetve.</span><span class="sxs-lookup"><span data-stu-id="07b56-120">**Dropped**: Discarded.</span></span>

![Stream Analytics eseménykezelésnek](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-hello-number-of-out-of-order-events"></a><span data-ttu-id="07b56-122">Csökkentse a hello soron események száma</span><span class="sxs-lookup"><span data-stu-id="07b56-122">Reduce hello number of out-of-order events</span></span>

<span data-ttu-id="07b56-123">Stream Analytics egy ideiglenes-átalakítás alkalmazása, akkor dolgozza fel a bejövő események (például az ablakos összesítéseket vagy időalapú illesztéseket), mert a Stream Analytics bejövő események időbélyeg sorrend szerint rendezi.</span><span class="sxs-lookup"><span data-stu-id="07b56-123">Because Stream Analytics applies a temporal transformation when it processes incoming events (for example, for windowed aggregates or temporal joins), Stream Analytics sorts incoming events by timestamp order.</span></span>

<span data-ttu-id="07b56-124">Ha hello "időbélyeg által" kulcsszóval van **nem** használt, hello Azure Event Hubs esemény sorba helyezni idő alapértelmezés szerint használt.</span><span class="sxs-lookup"><span data-stu-id="07b56-124">When hello “timestamp by” keyword is **not** used, hello Azure Event Hubs event enqueue time is used by default.</span></span> <span data-ttu-id="07b56-125">Az Event Hubs biztosítja, hogy az egyes partíciók hello az event hubs hello Timestamp monotonicity.</span><span class="sxs-lookup"><span data-stu-id="07b56-125">Event Hubs guarantees monotonicity of hello timestamp on each partition of hello event hub.</span></span> <span data-ttu-id="07b56-126">Emellett biztosítja azt, hogy az összes partíció események összevonva fogja tartalmazni időbélyeg sorrendben.</span><span class="sxs-lookup"><span data-stu-id="07b56-126">It also guarantees that events from all partitions will be merged in timestamp order.</span></span> <span data-ttu-id="07b56-127">Ezek két Event Hubs garanciák soron események nem biztosítása.</span><span class="sxs-lookup"><span data-stu-id="07b56-127">These two Event Hubs guarantees ensure no out-of-order events.</span></span>

<span data-ttu-id="07b56-128">Egyes esetekben fontos az Ön toouse hello feladó időbélyegzési.</span><span class="sxs-lookup"><span data-stu-id="07b56-128">Sometimes, it’s important for you toouse hello sender’s timestamp.</span></span> <span data-ttu-id="07b56-129">Ebben az esetben a hello eseménytartalom időbélyeg van kiválasztva, a "által időbélyegző."</span><span class="sxs-lookup"><span data-stu-id="07b56-129">In that case, a timestamp from hello event payload is chosen by using “timestamp by.”</span></span> <span data-ttu-id="07b56-130">Ezekben az esetekben egy vagy több forrás esemény misorder, előfordulhat, hogy vezette be:</span><span class="sxs-lookup"><span data-stu-id="07b56-130">In these scenarios, one or more sources of event misorder might be introduced:</span></span>

* <span data-ttu-id="07b56-131">Esemény gyártók óra dönt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="07b56-131">Event producers have clock skews.</span></span> <span data-ttu-id="07b56-132">Ez a esetén közös gyártók úgy, hogy különböző órák különböző számítógépeken vannak.</span><span class="sxs-lookup"><span data-stu-id="07b56-132">This is common when producers are from different computers, so they have different clocks.</span></span>
* <span data-ttu-id="07b56-133">Nincs hello események toohello cél az event hubs hello forrásból származó hálózati késést.</span><span class="sxs-lookup"><span data-stu-id="07b56-133">There's a network delay from hello source of hello events toohello destination event hub.</span></span>
* <span data-ttu-id="07b56-134">Óra dönt event hub partíciók között található.</span><span class="sxs-lookup"><span data-stu-id="07b56-134">Clock skews exist between event hub partitions.</span></span> <span data-ttu-id="07b56-135">A Stream Analytics először rendezése az összes event hub partícióról származó események esemény sorba helyezni időpontja.</span><span class="sxs-lookup"><span data-stu-id="07b56-135">Stream Analytics first sorts events from all event hub partitions by event enqueue time.</span></span> <span data-ttu-id="07b56-136">Ezt követően megvizsgálja a misordered események hello adatfolyamban.</span><span class="sxs-lookup"><span data-stu-id="07b56-136">Then, it examines hello data stream for misordered events.</span></span>

<span data-ttu-id="07b56-137">Hello konfigurációs lapján a következő alapértelmezett hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="07b56-137">On hello configuration tab, you see hello following defaults:</span></span>

![Stream Analytics soron kezelése](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

<span data-ttu-id="07b56-139">0 másodperc hello soron tűrési használja, ha vannak biztos, hogy az összes esemény sorrendben legyenek minden hello idő.</span><span class="sxs-lookup"><span data-stu-id="07b56-139">If you use 0 seconds as hello out-of-order tolerance window, you are asserting that all events are in order all hello time.</span></span> <span data-ttu-id="07b56-140">Hello három eseményforrást misordered megadott, nem valószínű, hogy ez érvényét veszti.</span><span class="sxs-lookup"><span data-stu-id="07b56-140">Given hello three sources of misordered events, it’s unlikely that this is true.</span></span> 

<span data-ttu-id="07b56-141">tooallow Stream Analytics toocorrect egy esemény misorder, egy nem nulla soron tűrési is megadhat.</span><span class="sxs-lookup"><span data-stu-id="07b56-141">tooallow Stream Analytics toocorrect an event misorder, you can specify a non-zero out-of-order tolerance window.</span></span> <span data-ttu-id="07b56-142">Stream Analytics pufferek események toothat ablakot, és úgy döntött, hogy timestamp hello használatával újrarendelések.</span><span class="sxs-lookup"><span data-stu-id="07b56-142">Stream Analytics buffers events up toothat window, and then reorders them by using hello timestamp you chose.</span></span> <span data-ttu-id="07b56-143">Ezt követően végrehajtja hello historikus átalakítása.</span><span class="sxs-lookup"><span data-stu-id="07b56-143">It then applies hello temporal transformation.</span></span> <span data-ttu-id="07b56-144">Egy 3-második ablak kezdődnie, és hangolja hello érték tooreduce hello események száma, amelyek idő igazodik.</span><span class="sxs-lookup"><span data-stu-id="07b56-144">You can start with a 3-second window, and tune hello value tooreduce hello number of events that are time-adjusted.</span></span> 

<span data-ttu-id="07b56-145">Egyik mellékhatása hello pufferelés az, hogy hello kimeneti **késleltetett által hello azonos idő**.</span><span class="sxs-lookup"><span data-stu-id="07b56-145">A side effect of hello buffering is that hello output is **delayed by hello same amount of time**.</span></span> <span data-ttu-id="07b56-146">Hangolja hello érték tooreduce hello soron események száma, és tartsa hello feladat késés alacsony.</span><span class="sxs-lookup"><span data-stu-id="07b56-146">You can tune hello value tooreduce hello number of out-of-order events, and keep hello job latency low.</span></span>

## <a name="get-help"></a><span data-ttu-id="07b56-147">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="07b56-147">Get help</span></span>
<span data-ttu-id="07b56-148">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="07b56-148">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="07b56-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="07b56-149">Next steps</span></span>
* [<span data-ttu-id="07b56-150">Bevezetés tooStream elemzés</span><span class="sxs-lookup"><span data-stu-id="07b56-150">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="07b56-151">A Stream Analytics használatába</span><span class="sxs-lookup"><span data-stu-id="07b56-151">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="07b56-152">Stream Analytics-feladatok méretezése</span><span class="sxs-lookup"><span data-stu-id="07b56-152">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="07b56-153">Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="07b56-153">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="07b56-154">A Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="07b56-154">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)