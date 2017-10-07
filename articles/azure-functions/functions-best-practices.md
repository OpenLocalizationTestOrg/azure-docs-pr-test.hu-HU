---
title: "az Azure Functions aaaBest eljárásai |} Microsoft Docs"
description: "Az Azure Functions ajánlott eljárásairól és mintáiról megismerése."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "az Azure functions mintázatok, ajánlott eljárás, Funkciók, Eseményfeldolgozási, webhookokkal, a dinamikus számítási, a kiszolgáló nélküli architektúrája"
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/13/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd3d826b75267a740e355b008943a48f71ebd339
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hello-performance-and-reliability-of-azure-functions"></a><span data-ttu-id="3cd2e-104">Hello teljesítmény-és az Azure Functions megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="3cd2e-104">Optimize hello performance and reliability of Azure Functions</span></span>

<span data-ttu-id="3cd2e-105">Ez a cikk útmutatást tooimprove hello teljesítményéről és megbízhatóságáról, a függvény alkalmazások tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-105">This article provides guidance tooimprove hello performance and reliability of your function apps.</span></span> 


## <a name="avoid-long-running-functions"></a><span data-ttu-id="3cd2e-106">Kerülje a hosszú ideig futó funkciók</span><span class="sxs-lookup"><span data-stu-id="3cd2e-106">Avoid long running functions</span></span>

<span data-ttu-id="3cd2e-107">Nagyméretű, hosszú futású funkciók váratlan időtúllépési hibákat is okozhat.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-107">Large, long-running functions can cause unexpected timeout issues.</span></span> <span data-ttu-id="3cd2e-108">Egy függvény nagy toomany Node.js függőségek miatt válhat.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-108">A function can become large due toomany Node.js dependencies.</span></span> <span data-ttu-id="3cd2e-109">Függőségek importálását is okozhat váratlan időtúllépések eredményező megnövekedett terhelést alkalommal.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-109">Importing dependencies can also cause increased load times that result in unexpected timeouts.</span></span> <span data-ttu-id="3cd2e-110">Függőségek töltődnek be mindkét explicit és implicit módon.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-110">Dependencies are loaded both explicitly and implicitly.</span></span> <span data-ttu-id="3cd2e-111">A kód által betöltött egyetlen modul töltődhet saját további modulok.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-111">A single module loaded by your code may load its own additional modules.</span></span>  

<span data-ttu-id="3cd2e-112">Amikor csak lehetséges, azonosítóterületen nagy funkciók kisebb függvénynek együtt beállítja, hogy a munka, és térjen vissza a gyors válaszok.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-112">Whenever possible, refactor large functions into smaller function sets that work together and return responses fast.</span></span> <span data-ttu-id="3cd2e-113">Például egy webhook vagy a HTTP-eseményindító függvény előfordulhat, hogy egy visszaigazoló választ belül bizonyos; gyakori, hogy a webhookok toorequire egy azonnali válasz is.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-113">For example, a webhook or HTTP trigger function might require an acknowledgment response within a certain time limit; it is common for webhooks toorequire an immediate response.</span></span> <span data-ttu-id="3cd2e-114">A várólista toobe dolgozza fel a várólista funkció be átadhatók hello HTTP eseményindító hasznos.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-114">You can pass hello HTTP trigger payload into a queue toobe processed by a queue trigger function.</span></span> <span data-ttu-id="3cd2e-115">Ez a megközelítés lehetővé teszi toodefer hello tényleges munka, és térjen vissza az azonnali válasz.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-115">This approach allows you toodefer hello actual work and return an immediate response.</span></span>


## <a name="cross-function-communication"></a><span data-ttu-id="3cd2e-116">Kereszt-függvény kommunikáció</span><span class="sxs-lookup"><span data-stu-id="3cd2e-116">Cross function communication</span></span>

<span data-ttu-id="3cd2e-117">Több funkciók integrálása, esetén általában a bevált gyakorlat toouse tárüzenetsort a közötti kommunikáció függvény.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-117">When integrating multiple functions, it is generally a best practice toouse storage queues for cross function communication.</span></span>  <span data-ttu-id="3cd2e-118">hello legfőbb oka tárüzenetsort olcsóbb és sokkal könnyebb tooprovision.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-118">hello main reason is storage queues are cheaper and much easier tooprovision.</span></span> 

<span data-ttu-id="3cd2e-119">A tárolási várólistában lévő egyes üzenetek méretének too64 korlátozott KB.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-119">Individual messages in a storage queue are limited in size too64 KB.</span></span> <span data-ttu-id="3cd2e-120">Toopass nagyobb üzeneteket funkciók között van szüksége, az Azure Service Bus-üzenetsorba használt toosupport üzenetek méretének mentése too256 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-120">If you need toopass larger messages between functions, an Azure Service Bus queue could be used toosupport message sizes up too256 KB.</span></span>

<span data-ttu-id="3cd2e-121">Service Bus-témakörök hasznosak, ha üzenet feldolgozás előtt szűrés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-121">Service Bus topics are useful if you require message filtering before processing.</span></span>

<span data-ttu-id="3cd2e-122">Az Event hubs hasznos toosupport nagy mennyiségű kommunikációs.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-122">Event hubs are useful toosupport high volume communications.</span></span>


## <a name="write-functions-toobe-stateless"></a><span data-ttu-id="3cd2e-123">Állapot nélküli funkciók toobe írása</span><span class="sxs-lookup"><span data-stu-id="3cd2e-123">Write functions toobe stateless</span></span> 

<span data-ttu-id="3cd2e-124">Funkciók kell lennie az állapot nélküli és idempotent Ha lehetséges.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-124">Functions should be stateless and idempotent if possible.</span></span> <span data-ttu-id="3cd2e-125">Az adatok minden szükséges állapotadatokat társítani.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-125">Associate any required state information with your data.</span></span> <span data-ttu-id="3cd2e-126">Például egy rendelés feldolgozása valószínűleg a hozzárendelt `state` tag.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-126">For example, an order being processed would likely have an associated `state` member.</span></span> <span data-ttu-id="3cd2e-127">A következő függvényt egy sorrendet, miközben maga hello függvény állapot nélküli marad, hogy állapotától függően sikerült feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-127">A function could process an order based on that state while hello function itself remains stateless.</span></span> 

<span data-ttu-id="3cd2e-128">Az Idempotent funkciók különösen az időzítő eseményindítók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-128">Idempotent functions are especially recommended with timer triggers.</span></span> <span data-ttu-id="3cd2e-129">Például ha rendelkezik, amelyet feltétlenül naponta egyszer kell futtatni, írják, hello nap alatt való futtatás hello ugyanazokat az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-129">For example, if you have something that absolutely must run once a day, write it so it can run any time during hello day with hello same results.</span></span> <span data-ttu-id="3cd2e-130">Ha nincs feladata egy adott napon a következő módokon léphet hello függvény.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-130">hello function can exit when there is no work for a particular day.</span></span> <span data-ttu-id="3cd2e-131">Is előző toocomplete sikertelen, ha következő futtatás hello kell onnan folytathatja az adatgyűjtést, ahol abbahagyta.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-131">Also if a previous run failed toocomplete, hello next run should pick up where it left off.</span></span>


## <a name="write-defensive-functions"></a><span data-ttu-id="3cd2e-132">Defenzív írást</span><span class="sxs-lookup"><span data-stu-id="3cd2e-132">Write defensive functions</span></span>

<span data-ttu-id="3cd2e-133">Tegyük fel, a függvény kivételt bármikor sikerült tapasztal.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-133">Assume your function could encounter an exception at any time.</span></span> <span data-ttu-id="3cd2e-134">Tervezze meg a függvények hello képességét toocontinue egy előző sikertelen pont hello következő végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-134">Design your functions with hello ability toocontinue from a previous fail point during hello next execution.</span></span> <span data-ttu-id="3cd2e-135">Vegye figyelembe a következő műveletek hello van szükség:</span><span class="sxs-lookup"><span data-stu-id="3cd2e-135">Consider a scenario that requires hello following actions:</span></span>

1. <span data-ttu-id="3cd2e-136">A lekérdezés egy db 10 000 soraihoz.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-136">Query for 10,000 rows in a db.</span></span>
2. <span data-ttu-id="3cd2e-137">Hozzon létre egy üzenetsor-üzenetet minden e sorok tooprocess további hello sor le.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-137">Create a queue message for each of those rows tooprocess further down hello line.</span></span>
 
<span data-ttu-id="3cd2e-138">Attól függően, hogy hogyan összetett a rendszer, előfordulhat, hogy: helytelen viselkedik érintett alárendelt szolgáltatásokkal hálózati kimaradások vagy a kvóta korlátozza elérte, stb. A függvény bármikor befolyásolhatja mindegyik.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-138">Depending on how complex your system is, you may have: involved downstream services behaving badly, networking outages, or quota limits reached, etc. All of these can affect your function at any time.</span></span> <span data-ttu-id="3cd2e-139">A funkciók toobe az előkészített kell toodesign.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-139">You need toodesign your functions toobe prepared for it.</span></span>

<span data-ttu-id="3cd2e-140">Hogyan reagál a kódját a után 5000 elem beszúrása a várólistára feldolgozásra hiba esetén?</span><span class="sxs-lookup"><span data-stu-id="3cd2e-140">How does your code react if a failure occurs after inserting 5,000 of those items into a queue for processing?</span></span> <span data-ttu-id="3cd2e-141">Nyomon követheti, hogy befejezte készletben.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-141">Track items in a set that you’ve completed.</span></span> <span data-ttu-id="3cd2e-142">Ellenkező esetben előfordulhat, hogy be őket újra a következő alkalommal.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-142">Otherwise, you might insert them again next time.</span></span> <span data-ttu-id="3cd2e-143">Ez a munkafolyamat egy súlyos hatása lehet.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-143">This can have a serious impact on your work flow.</span></span> 

<span data-ttu-id="3cd2e-144">Ha egy elem már megtörtént a feldolgozása, a függvény toobe lehetővé műveletvégzés.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-144">If a queue item was already processed, allow your function toobe a no-op.</span></span>

<span data-ttu-id="3cd2e-145">Használhatja a hello Azure Functions platform összetevők már foglalt védelmi intézkedések előnyeit.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-145">Take advantage of defensive measures already provided for components you use in hello Azure Functions platform.</span></span> <span data-ttu-id="3cd2e-146">Lásd például: **elhalt üzenetek kezelésének** hello dokumentációjában [Azure Storage Üzenetsorába elindítja a](functions-bindings-storage-queue.md#trigger).</span><span class="sxs-lookup"><span data-stu-id="3cd2e-146">For example, see **Handling poison queue messages** in hello documentation for [Azure Storage Queue triggers](functions-bindings-storage-queue.md#trigger).</span></span>
 

## <a name="dont-mix-test-and-production-code-in-hello-same-function-app"></a><span data-ttu-id="3cd2e-147">Nem vegyes teszt termelési kód és a hello azonos függvény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3cd2e-147">Don't mix test and production code in hello same function app</span></span>

<span data-ttu-id="3cd2e-148">A függvény alkalmazások funkciók osztozik az erőforrásokon.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-148">Functions within a function app share resources.</span></span> <span data-ttu-id="3cd2e-149">Például a memória van osztva.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-149">For example, memory is shared.</span></span> <span data-ttu-id="3cd2e-150">Egy függvény alkalmazást éles környezetben használata, ha nem adja hozzá, teszt kapcsolatos funkciók és erőforrások tooit.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-150">If you're using a function app in production, don't add test-related functions and resources tooit.</span></span> <span data-ttu-id="3cd2e-151">Éles kód végrehajtása során váratlan terhelést okozhat.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-151">It can cause unexpected overhead during production code execution.</span></span>

<span data-ttu-id="3cd2e-152">Ügyeljen az éles függvény alkalmazások betöltése.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-152">Be careful what you load in your production function apps.</span></span> <span data-ttu-id="3cd2e-153">Memória van átlagosan hello alkalmazásban minden függvény volt.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-153">Memory is averaged across each function in hello app.</span></span>

<span data-ttu-id="3cd2e-154">Ha egy megosztott hivatkozott több .NET-es függvényeket, elhelyezi egy közös megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-154">If you have a shared assembly referenced in multiple .Net functions, put it in a common shared folder.</span></span> <span data-ttu-id="3cd2e-155">A következő példa egy utasítás hasonló toohello hivatkozás hello szerelvény:</span><span class="sxs-lookup"><span data-stu-id="3cd2e-155">Reference hello assembly with a statement similar toohello following example:</span></span> 

    #r "..\Shared\MyAssembly.dll". 

<span data-ttu-id="3cd2e-156">Ellenkező esetben is könnyen tooaccidentally hello közötti másképp viselkednek bináris működik több teszt verzió telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-156">Otherwise, it is easy tooaccidentally deploy multiple test versions of hello same binary that behave differently between functions.</span></span>

<span data-ttu-id="3cd2e-157">Ne használja az éles kódban részletes naplózás.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-157">Don't use verbose logging in production code.</span></span> <span data-ttu-id="3cd2e-158">Rendelkezik egy negatívan befolyásolta a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-158">It has a negative performance impact.</span></span>



## <a name="use-async-code-but-avoid-blocking-calls"></a><span data-ttu-id="3cd2e-159">Aszinkron kódot használja, de elkerülni hívások</span><span class="sxs-lookup"><span data-stu-id="3cd2e-159">Use async code but avoid blocking calls</span></span>

<span data-ttu-id="3cd2e-160">Aszinkron programozás az ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-160">Asynchronous programming is a recommended best practice.</span></span> <span data-ttu-id="3cd2e-161">Azonban mindig elkerülése hello hivatkozó `Result` vagy tulajdonság hívása `Wait` metódusa egy `Task` példány.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-161">However, always avoid referencing hello `Result` property or calling `Wait` method on a `Task` instance.</span></span> <span data-ttu-id="3cd2e-162">Ez a megközelítés toothread Erőforrásfogyás vezethet.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-162">This approach can lead toothread exhaustion.</span></span>


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a><span data-ttu-id="3cd2e-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3cd2e-163">Next steps</span></span>
<span data-ttu-id="3cd2e-164">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="3cd2e-164">For more information, see hello following resources:</span></span>

<span data-ttu-id="3cd2e-165">Mivel az Azure Functions használ akkor is tisztában kell lennie az App Service-irányelvek Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="3cd2e-165">Because Azure Functions uses Azure App Service, you should also be aware of  App Service guidelines.</span></span>
* [<span data-ttu-id="3cd2e-166">Minták és gyakorlatok HTTP teljesítményoptimalizálás</span><span class="sxs-lookup"><span data-stu-id="3cd2e-166">Patterns and Practices HTTP Performance Optimizations</span></span>](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

