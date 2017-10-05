---
title: "Gyakorlati tanácsok az Azure Functions |} Microsoft Docs"
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
ms.openlocfilehash: 645a5dd16e72619e7c2470ab8f03098f0fa6c7f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a><span data-ttu-id="c5b23-104">A teljesítmény-és az Azure Functions megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="c5b23-104">Optimize the performance and reliability of Azure Functions</span></span>

<span data-ttu-id="c5b23-105">Ez a cikk nyújt útmutatást a jobb teljesítmény és megbízhatóság a függvény alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="c5b23-105">This article provides guidance to improve the performance and reliability of your function apps.</span></span> 


## <a name="avoid-long-running-functions"></a><span data-ttu-id="c5b23-106">Kerülje a hosszú ideig futó funkciók</span><span class="sxs-lookup"><span data-stu-id="c5b23-106">Avoid long running functions</span></span>

<span data-ttu-id="c5b23-107">Nagyméretű, hosszú futású funkciók váratlan időtúllépési hibákat is okozhat.</span><span class="sxs-lookup"><span data-stu-id="c5b23-107">Large, long-running functions can cause unexpected timeout issues.</span></span> <span data-ttu-id="c5b23-108">Egy függvény nagyméretű, sok Node.js függőségek miatt válhat.</span><span class="sxs-lookup"><span data-stu-id="c5b23-108">A function can become large due to many Node.js dependencies.</span></span> <span data-ttu-id="c5b23-109">Függőségek importálását is okozhat váratlan időtúllépések eredményező megnövekedett terhelést alkalommal.</span><span class="sxs-lookup"><span data-stu-id="c5b23-109">Importing dependencies can also cause increased load times that result in unexpected timeouts.</span></span> <span data-ttu-id="c5b23-110">Függőségek töltődnek be mindkét explicit és implicit módon.</span><span class="sxs-lookup"><span data-stu-id="c5b23-110">Dependencies are loaded both explicitly and implicitly.</span></span> <span data-ttu-id="c5b23-111">A kód által betöltött egyetlen modul töltődhet saját további modulok.</span><span class="sxs-lookup"><span data-stu-id="c5b23-111">A single module loaded by your code may load its own additional modules.</span></span>  

<span data-ttu-id="c5b23-112">Amikor csak lehetséges, azonosítóterületen nagy funkciók kisebb függvénynek együtt beállítja, hogy a munka, és térjen vissza a gyors válaszok.</span><span class="sxs-lookup"><span data-stu-id="c5b23-112">Whenever possible, refactor large functions into smaller function sets that work together and return responses fast.</span></span> <span data-ttu-id="c5b23-113">Például egy webhook vagy a HTTP-eseményindító függvény előfordulhat, hogy egy visszaigazoló választ belül bizonyos; esetében gyakori, a webhookok való egy azonnali válasz igényelnek.</span><span class="sxs-lookup"><span data-stu-id="c5b23-113">For example, a webhook or HTTP trigger function might require an acknowledgment response within a certain time limit; it is common for webhooks to require an immediate response.</span></span> <span data-ttu-id="c5b23-114">A HTTP-eseményindító forgalma átadhatók egy várólista eseményindító függvény által feldolgozandó egy várólistára.</span><span class="sxs-lookup"><span data-stu-id="c5b23-114">You can pass the HTTP trigger payload into a queue to be processed by a queue trigger function.</span></span> <span data-ttu-id="c5b23-115">Ez a megközelítés lehetővé teszi a tényleges munkát késleltető, és térjen vissza az azonnali válasz.</span><span class="sxs-lookup"><span data-stu-id="c5b23-115">This approach allows you to defer the actual work and return an immediate response.</span></span>


## <a name="cross-function-communication"></a><span data-ttu-id="c5b23-116">Kereszt-függvény kommunikáció</span><span class="sxs-lookup"><span data-stu-id="c5b23-116">Cross function communication</span></span>

<span data-ttu-id="c5b23-117">Több funkciók integrálása, esetén általában tárüzenetsort közötti kommunikáció függvény használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="c5b23-117">When integrating multiple functions, it is generally a best practice to use storage queues for cross function communication.</span></span>  <span data-ttu-id="c5b23-118">A legfőbb oka tárüzenetsort olcsóbb és sokkal könnyebben kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5b23-118">The main reason is storage queues are cheaper and much easier to provision.</span></span> 

<span data-ttu-id="c5b23-119">A tárolási várólistában lévő egyes üzenetek 64 KB méretű korlátozottak.</span><span class="sxs-lookup"><span data-stu-id="c5b23-119">Individual messages in a storage queue are limited in size to 64 KB.</span></span> <span data-ttu-id="c5b23-120">Ha nagyobb üzenetek továbbítani funkciók között van szüksége, az Azure Service Bus várólista üzenet használható méretének legfeljebb 256 KB.</span><span class="sxs-lookup"><span data-stu-id="c5b23-120">If you need to pass larger messages between functions, an Azure Service Bus queue could be used to support message sizes up to 256 KB.</span></span>

<span data-ttu-id="c5b23-121">Service Bus-témakörök hasznosak, ha üzenet feldolgozás előtt szűrés van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c5b23-121">Service Bus topics are useful if you require message filtering before processing.</span></span>

<span data-ttu-id="c5b23-122">Az Event hubs hasznosak nagy mennyiségű kommunikáció támogatásához.</span><span class="sxs-lookup"><span data-stu-id="c5b23-122">Event hubs are useful to support high volume communications.</span></span>


## <a name="write-functions-to-be-stateless"></a><span data-ttu-id="c5b23-123">Írást kell lennie az állapot nélküli</span><span class="sxs-lookup"><span data-stu-id="c5b23-123">Write functions to be stateless</span></span> 

<span data-ttu-id="c5b23-124">Funkciók kell lennie az állapot nélküli és idempotent Ha lehetséges.</span><span class="sxs-lookup"><span data-stu-id="c5b23-124">Functions should be stateless and idempotent if possible.</span></span> <span data-ttu-id="c5b23-125">Az adatok minden szükséges állapotadatokat társítani.</span><span class="sxs-lookup"><span data-stu-id="c5b23-125">Associate any required state information with your data.</span></span> <span data-ttu-id="c5b23-126">Például egy rendelés feldolgozása valószínűleg a hozzárendelt `state` tag.</span><span class="sxs-lookup"><span data-stu-id="c5b23-126">For example, an order being processed would likely have an associated `state` member.</span></span> <span data-ttu-id="c5b23-127">Egy függvény alapján az állapotban, miközben maga a funkció továbbra is az állapot nélküli rendelés feldolgozása sikerült.</span><span class="sxs-lookup"><span data-stu-id="c5b23-127">A function could process an order based on that state while the function itself remains stateless.</span></span> 

<span data-ttu-id="c5b23-128">Az Idempotent funkciók különösen az időzítő eseményindítók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="c5b23-128">Idempotent functions are especially recommended with timer triggers.</span></span> <span data-ttu-id="c5b23-129">Például ha rendelkezik, amelyet feltétlenül naponta egyszer kell futtatni, írják, a nap folyamán bármikor ugyanaz az eredmény való futtatás.</span><span class="sxs-lookup"><span data-stu-id="c5b23-129">For example, if you have something that absolutely must run once a day, write it so it can run any time during the day with the same results.</span></span> <span data-ttu-id="c5b23-130">A függvény a következő módokon léphet nincs feladata egy adott napon esetén.</span><span class="sxs-lookup"><span data-stu-id="c5b23-130">The function can exit when there is no work for a particular day.</span></span> <span data-ttu-id="c5b23-131">Emellett egy korábbi futtatás nem sikerült végrehajtani, ha a következő futtatáskor kell átvételéhez, ahol abbahagyta.</span><span class="sxs-lookup"><span data-stu-id="c5b23-131">Also if a previous run failed to complete, the next run should pick up where it left off.</span></span>


## <a name="write-defensive-functions"></a><span data-ttu-id="c5b23-132">Defenzív írást</span><span class="sxs-lookup"><span data-stu-id="c5b23-132">Write defensive functions</span></span>

<span data-ttu-id="c5b23-133">Tegyük fel, a függvény kivételt bármikor sikerült tapasztal.</span><span class="sxs-lookup"><span data-stu-id="c5b23-133">Assume your function could encounter an exception at any time.</span></span> <span data-ttu-id="c5b23-134">A funkciók tervezése folytathatja a korábbi sikertelen pontokról a következő végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="c5b23-134">Design your functions with the ability to continue from a previous fail point during the next execution.</span></span> <span data-ttu-id="c5b23-135">Vegye figyelembe a következő műveletek van szükség:</span><span class="sxs-lookup"><span data-stu-id="c5b23-135">Consider a scenario that requires the following actions:</span></span>

1. <span data-ttu-id="c5b23-136">A lekérdezés egy db 10 000 soraihoz.</span><span class="sxs-lookup"><span data-stu-id="c5b23-136">Query for 10,000 rows in a db.</span></span>
2. <span data-ttu-id="c5b23-137">Hozzon létre egy üzenetsor-üzenetet az összes további feldolgozható sorok le a sort.</span><span class="sxs-lookup"><span data-stu-id="c5b23-137">Create a queue message for each of those rows to process further down the line.</span></span>
 
<span data-ttu-id="c5b23-138">Attól függően, hogy hogyan összetett a rendszer, előfordulhat, hogy: helytelen viselkedik érintett alárendelt szolgáltatásokkal hálózati kimaradások vagy a kvóta korlátozza elérte, stb. A függvény bármikor befolyásolhatja mindegyik.</span><span class="sxs-lookup"><span data-stu-id="c5b23-138">Depending on how complex your system is, you may have: involved downstream services behaving badly, networking outages, or quota limits reached, etc. All of these can affect your function at any time.</span></span> <span data-ttu-id="c5b23-139">Kell terveznie a Funkciók, elő kell készíteni a azt.</span><span class="sxs-lookup"><span data-stu-id="c5b23-139">You need to design your functions to be prepared for it.</span></span>

<span data-ttu-id="c5b23-140">Hogyan reagál a kódját a után 5000 elem beszúrása a várólistára feldolgozásra hiba esetén?</span><span class="sxs-lookup"><span data-stu-id="c5b23-140">How does your code react if a failure occurs after inserting 5,000 of those items into a queue for processing?</span></span> <span data-ttu-id="c5b23-141">Nyomon követheti, hogy befejezte készletben.</span><span class="sxs-lookup"><span data-stu-id="c5b23-141">Track items in a set that you’ve completed.</span></span> <span data-ttu-id="c5b23-142">Ellenkező esetben előfordulhat, hogy be őket újra a következő alkalommal.</span><span class="sxs-lookup"><span data-stu-id="c5b23-142">Otherwise, you might insert them again next time.</span></span> <span data-ttu-id="c5b23-143">Ez a munkafolyamat egy súlyos hatása lehet.</span><span class="sxs-lookup"><span data-stu-id="c5b23-143">This can have a serious impact on your work flow.</span></span> 

<span data-ttu-id="c5b23-144">Egy elem már megtörtént a feldolgozása, ha engedélyezi a műveletvégzés a funkciót.</span><span class="sxs-lookup"><span data-stu-id="c5b23-144">If a queue item was already processed, allow your function to be a no-op.</span></span>

<span data-ttu-id="c5b23-145">Használhatja az Azure Functions platform összetevők már foglalt védelmi intézkedések előnyeit.</span><span class="sxs-lookup"><span data-stu-id="c5b23-145">Take advantage of defensive measures already provided for components you use in the Azure Functions platform.</span></span> <span data-ttu-id="c5b23-146">Lásd például: **elhalt üzenetek kezelésének** dokumentációját [Azure Storage Üzenetsorába elindítja a](functions-bindings-storage-queue.md#trigger).</span><span class="sxs-lookup"><span data-stu-id="c5b23-146">For example, see **Handling poison queue messages** in the documentation for [Azure Storage Queue triggers](functions-bindings-storage-queue.md#trigger).</span></span>
 

## <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a><span data-ttu-id="c5b23-147">Angol szövegben ne keverje függvény ugyanazt az alkalmazást a teszt- és éles kódot</span><span class="sxs-lookup"><span data-stu-id="c5b23-147">Don't mix test and production code in the same function app</span></span>

<span data-ttu-id="c5b23-148">A függvény alkalmazások funkciók osztozik az erőforrásokon.</span><span class="sxs-lookup"><span data-stu-id="c5b23-148">Functions within a function app share resources.</span></span> <span data-ttu-id="c5b23-149">Például a memória van osztva.</span><span class="sxs-lookup"><span data-stu-id="c5b23-149">For example, memory is shared.</span></span> <span data-ttu-id="c5b23-150">Egy függvény alkalmazást éles környezetben használata, nem felvétele teszt kapcsolatos funkciók, és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c5b23-150">If you're using a function app in production, don't add test-related functions and resources to it.</span></span> <span data-ttu-id="c5b23-151">Éles kód végrehajtása során váratlan terhelést okozhat.</span><span class="sxs-lookup"><span data-stu-id="c5b23-151">It can cause unexpected overhead during production code execution.</span></span>

<span data-ttu-id="c5b23-152">Ügyeljen az éles függvény alkalmazások betöltése.</span><span class="sxs-lookup"><span data-stu-id="c5b23-152">Be careful what you load in your production function apps.</span></span> <span data-ttu-id="c5b23-153">Memória van átlagosan volt az alkalmazás minden függvényt.</span><span class="sxs-lookup"><span data-stu-id="c5b23-153">Memory is averaged across each function in the app.</span></span>

<span data-ttu-id="c5b23-154">Ha egy megosztott hivatkozott több .NET-es függvényeket, elhelyezi egy közös megosztott mappába.</span><span class="sxs-lookup"><span data-stu-id="c5b23-154">If you have a shared assembly referenced in multiple .Net functions, put it in a common shared folder.</span></span> <span data-ttu-id="c5b23-155">Az alábbi példához hasonló utasítással szerelvény hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="c5b23-155">Reference the assembly with a statement similar to the following example:</span></span> 

    #r "..\Shared\MyAssembly.dll". 

<span data-ttu-id="c5b23-156">Ellenkező esetben egyszerűen véletlenül telepíthető több teszt verziója megegyezik a bináris parancsmagoktól eltérően működő funkciók között.</span><span class="sxs-lookup"><span data-stu-id="c5b23-156">Otherwise, it is easy to accidentally deploy multiple test versions of the same binary that behave differently between functions.</span></span>

<span data-ttu-id="c5b23-157">Ne használja az éles kódban részletes naplózás.</span><span class="sxs-lookup"><span data-stu-id="c5b23-157">Don't use verbose logging in production code.</span></span> <span data-ttu-id="c5b23-158">Rendelkezik egy negatívan befolyásolta a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="c5b23-158">It has a negative performance impact.</span></span>



## <a name="use-async-code-but-avoid-blocking-calls"></a><span data-ttu-id="c5b23-159">Aszinkron kódot használja, de elkerülni hívások</span><span class="sxs-lookup"><span data-stu-id="c5b23-159">Use async code but avoid blocking calls</span></span>

<span data-ttu-id="c5b23-160">Aszinkron programozás az ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="c5b23-160">Asynchronous programming is a recommended best practice.</span></span> <span data-ttu-id="c5b23-161">Azonban mindig elkerülése hivatkozik a `Result` vagy tulajdonság hívása `Wait` metódusa egy `Task` példány.</span><span class="sxs-lookup"><span data-stu-id="c5b23-161">However, always avoid referencing the `Result` property or calling `Wait` method on a `Task` instance.</span></span> <span data-ttu-id="c5b23-162">Ez a megközelítés szál Erőforrásfogyás vezethet.</span><span class="sxs-lookup"><span data-stu-id="c5b23-162">This approach can lead to thread exhaustion.</span></span>


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a><span data-ttu-id="c5b23-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5b23-163">Next steps</span></span>
<span data-ttu-id="c5b23-164">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="c5b23-164">For more information, see the following resources:</span></span>

<span data-ttu-id="c5b23-165">Mivel az Azure Functions használ akkor is tisztában kell lennie az App Service-irányelvek Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c5b23-165">Because Azure Functions uses Azure App Service, you should also be aware of  App Service guidelines.</span></span>
* [<span data-ttu-id="c5b23-166">Minták és gyakorlatok HTTP teljesítményoptimalizálás</span><span class="sxs-lookup"><span data-stu-id="c5b23-166">Patterns and Practices HTTP Performance Optimizations</span></span>](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)

