---
title: "aaaScheduler fogalmakat, feltételek és entitások |} Microsoft Docs"
description: "Az Azure Scheduler alapfogalmai, entitáshierarchiája és terminológiája, beleértve a feladatokat és a feladatgyűjteményeket.  Egy ütemezett feladat átfogó példáját mutatja be."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 73e7de7bfd2937e401aeab05e0e10fa292cf37b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a><span data-ttu-id="a14cb-104">A Scheduler alapfogalmai, entitáshierarchiája és terminológiája</span><span class="sxs-lookup"><span data-stu-id="a14cb-104">Scheduler concepts, terminology, + entity hierarchy</span></span>
## <a name="scheduler-entity-hierarchy"></a><span data-ttu-id="a14cb-105">A Scheduler entitáshierarchiája</span><span class="sxs-lookup"><span data-stu-id="a14cb-105">Scheduler entity hierarchy</span></span>
<span data-ttu-id="a14cb-106">hello következő táblázatban hello fő erőforrások elérhetővé tett vagy hello Scheduler API által használt.</span><span class="sxs-lookup"><span data-stu-id="a14cb-106">hello following table describes hello main resources exposed or used by hello Scheduler API:</span></span>

| <span data-ttu-id="a14cb-107">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="a14cb-107">Resource</span></span> | <span data-ttu-id="a14cb-108">Leírás</span><span class="sxs-lookup"><span data-stu-id="a14cb-108">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a14cb-109">**Feladatgyűjtemény**</span><span class="sxs-lookup"><span data-stu-id="a14cb-109">**Job collection**</span></span> |<span data-ttu-id="a14cb-110">A feladatgyűjtemény több feladatot tartalmaz, és tárolja a beállításokat, kvóták és szabályozások hello gyűjteményben feladatok által megosztott.</span><span class="sxs-lookup"><span data-stu-id="a14cb-110">A job collection contains a group of jobs and maintains settings, quotas, and throttles that are shared by jobs within hello collection.</span></span> <span data-ttu-id="a14cb-111">A feladatgyűjteményeket az előfizetés tulajdonosa hozza létre, ahol a feladatokat használat vagy alkalmazáshatárok alapján csoportosítja.</span><span class="sxs-lookup"><span data-stu-id="a14cb-111">A job collection is created by a subscription owner and groups jobs together based on usage or application boundaries.</span></span> <span data-ttu-id="a14cb-112">Korlátozott tooone régióban.</span><span class="sxs-lookup"><span data-stu-id="a14cb-112">It’s constrained tooone region.</span></span> <span data-ttu-id="a14cb-113">Lehetővé teszi a kvóták hello kényszerítési tooconstrain hello használata az összes feladat a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="a14cb-113">It also allows hello enforcement of quotas tooconstrain hello usage of all jobs in that collection.</span></span> <span data-ttu-id="a14cb-114">hello kvóták MaxJobs és maximális ismétlődésére tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a14cb-114">hello quotas include MaxJobs and MaxRecurrence.</span></span> |
| <span data-ttu-id="a14cb-115">**Feladat**</span><span class="sxs-lookup"><span data-stu-id="a14cb-115">**Job**</span></span> |<span data-ttu-id="a14cb-116">Egyedi, ismétlődő műveletet megadó feladat, egyszerű vagy összetett végrehajtási stratégiákkal.</span><span class="sxs-lookup"><span data-stu-id="a14cb-116">A job defines a single recurrent action, with simple or complex strategies for execution.</span></span> <span data-ttu-id="a14cb-117">A műveletek HTTP-, tárolásisor-, Service Bus üzenetsor- vagy Service Bus témakörkéréseket tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="a14cb-117">Actions may include HTTP, storage queue, service bus queue, or service bus topic requests.</span></span> |
| <span data-ttu-id="a14cb-118">**Feladatelőzmények**</span><span class="sxs-lookup"><span data-stu-id="a14cb-118">**Job history**</span></span> |<span data-ttu-id="a14cb-119">A feladatelőzmény egy feladat végrehajtásának részletes adatait jelenti.</span><span class="sxs-lookup"><span data-stu-id="a14cb-119">A job history represents details for an execution of a job.</span></span> <span data-ttu-id="a14cb-120">Megállapítható belőle a feladat végrehajtásának sikeressége vagy meghiúsulása, illetve bármely részletes válaszadat.</span><span class="sxs-lookup"><span data-stu-id="a14cb-120">It contains success vs. failure, as well as any response details.</span></span> |

## <a name="scheduler-entity-management"></a><span data-ttu-id="a14cb-121">A Scheduler entitáskezelése</span><span class="sxs-lookup"><span data-stu-id="a14cb-121">Scheduler entity management</span></span>
<span data-ttu-id="a14cb-122">Magas szinten hello Feladatütemező és hello service management API teszi közzé a következő hello erőforrásokon végrehajtott műveletek hello:</span><span class="sxs-lookup"><span data-stu-id="a14cb-122">At a high level, hello scheduler and hello service management API expose hello following operations on hello resources:</span></span>

| <span data-ttu-id="a14cb-123">Képesség</span><span class="sxs-lookup"><span data-stu-id="a14cb-123">Capability</span></span> | <span data-ttu-id="a14cb-124">Leírás és URI-cím</span><span class="sxs-lookup"><span data-stu-id="a14cb-124">Description and URI address</span></span> |
| --- | --- |
| <span data-ttu-id="a14cb-125">**A feladatgyűjtemény kezelése**</span><span class="sxs-lookup"><span data-stu-id="a14cb-125">**Job collection management**</span></span> |<span data-ttu-id="a14cb-126">PUT BESZERZÉSÉHEZ, és törölje a létrehozása és módosítása feladatgyűjteményei és a benne foglalt hello feladatok támogatása.</span><span class="sxs-lookup"><span data-stu-id="a14cb-126">GET, PUT, and DELETE support for creating and modifying job collections and hello jobs contained therein.</span></span> <span data-ttu-id="a14cb-127">A feladatgyűjtemény feladatok tárolója, és leképezi tooquotas és közös beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a14cb-127">A job collection is a container for jobs and maps tooquotas and shared settings.</span></span> <span data-ttu-id="a14cb-128">Példák a később részletezendő kvótákra: feladatok maximális száma és legkisebb ismétlődési időköz.</span><span class="sxs-lookup"><span data-stu-id="a14cb-128">Examples of quotas, described later, are maximum number of jobs and smallest recurrence interval.</span></span> <p><span data-ttu-id="a14cb-129">PUT és DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="a14cb-129">PUT and DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p><p><span data-ttu-id="a14cb-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span><span class="sxs-lookup"><span data-stu-id="a14cb-130">GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</span></span></p> |
| <span data-ttu-id="a14cb-131">**Feladatkezelés**</span><span class="sxs-lookup"><span data-stu-id="a14cb-131">**Job management**</span></span> |<span data-ttu-id="a14cb-132">GET, PUT, POST, PATCH és DELETE kérések támogatása a feladatok létrehozásához és módosításához.</span><span class="sxs-lookup"><span data-stu-id="a14cb-132">GET, PUT, POST, PATCH, and DELETE support for creating and modifying jobs.</span></span> <span data-ttu-id="a14cb-133">Minden feladat kell tartozniuk tooa feladatgyűjteményben már létező, így nem implicit létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a14cb-133">All jobs must belong tooa job collection that already exists, so there is no implicit creation.</span></span> <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| <span data-ttu-id="a14cb-134">**Feladatelőzmények kezelése**</span><span class="sxs-lookup"><span data-stu-id="a14cb-134">**Job history management**</span></span> |<span data-ttu-id="a14cb-135">GET parancs támogatása a 60 napos feladat-végrehajtási előzménytörténet lekéréséhez, ide értve a végrehajtás során eltelt időt és annak eredményeit is.</span><span class="sxs-lookup"><span data-stu-id="a14cb-135">GET support for fetching 60 days of job execution history, such as job elapsed time and job execution results.</span></span> <span data-ttu-id="a14cb-136">Az állapot szerinti szűrés érdekében támogatja a lekérdezési karakterláncok paramétereit.</span><span class="sxs-lookup"><span data-stu-id="a14cb-136">Adds query string parameter support for filtering based on state and status.</span></span> <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a><span data-ttu-id="a14cb-137">Feladattípusok</span><span class="sxs-lookup"><span data-stu-id="a14cb-137">Job types</span></span>
<span data-ttu-id="a14cb-138">Számos feladattípus létezik: HTTP-feladatok (beleértve az SSL-t támogató HTTPS-feladatokat), tárolásisor-feladatok, Service Bus üzenetsor-feladatok és Service Bus témakör-feladatok.</span><span class="sxs-lookup"><span data-stu-id="a14cb-138">There are multiple types of jobs: HTTP jobs (including HTTPS jobs that support SSL), storage queue jobs, service bus queue jobs, and service bus topic jobs.</span></span> <span data-ttu-id="a14cb-139">A HTTP-feladatok remekül használhatók, ha egy meglévő számítási feladat vagy szolgáltatás egy végponttal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a14cb-139">HTTP jobs are ideal if you have an endpoint of an existing workload or service.</span></span> <span data-ttu-id="a14cb-140">Várólista feladatok toopost üzenetek toostorage tárüzenetsort, is használ, így ezek a feladatok tárüzenetsort használó munkaterhelések ideális.</span><span class="sxs-lookup"><span data-stu-id="a14cb-140">You can use storage queue jobs toopost messages toostorage queues, so those jobs are ideal for workloads that use storage queues.</span></span> <span data-ttu-id="a14cb-141">Ehhez hasonlóan a Service Bus feladatok olyan számítási feladatok esetében alkalmazhatók előnyösen, amelyek Service Bus-üzenetsorokat és -témaköröket használnak.</span><span class="sxs-lookup"><span data-stu-id="a14cb-141">Similarly, service bus jobs are ideal for workloads that use service bus queues and topics.</span></span>

## <a name="hello-job-entity-in-detail"></a><span data-ttu-id="a14cb-142">hello "feladat" entitás részletesen</span><span class="sxs-lookup"><span data-stu-id="a14cb-142">hello "job" entity in detail</span></span>
<span data-ttu-id="a14cb-143">Alapszinten egy ütemezett feladat számos részből áll:</span><span class="sxs-lookup"><span data-stu-id="a14cb-143">At a basic level, a scheduled job has several parts:</span></span>

* <span data-ttu-id="a14cb-144">hello művelet tooperform hello feladat időzítő akkor következik be, amikor</span><span class="sxs-lookup"><span data-stu-id="a14cb-144">hello action tooperform when hello job timer fires</span></span>  
* <span data-ttu-id="a14cb-145">(Választható) hello idő toorun hello feladat</span><span class="sxs-lookup"><span data-stu-id="a14cb-145">(Optional) hello time toorun hello job</span></span>  
* <span data-ttu-id="a14cb-146">(Választható) Mikor és milyen gyakran toorepeat hello feladat</span><span class="sxs-lookup"><span data-stu-id="a14cb-146">(Optional) When and how often toorepeat hello job</span></span>  
* <span data-ttu-id="a14cb-147">(Választható) Egy művelet toofire Ha hello elsődleges művelet sikertelen</span><span class="sxs-lookup"><span data-stu-id="a14cb-147">(Optional) An action toofire if hello primary action fails</span></span>  

<span data-ttu-id="a14cb-148">Belsőleg egy ütemezett feladatot is rendszer által biztosított adatok, például a következő ütemezett végrehajtási idő hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a14cb-148">Internally, a scheduled job also contains system-provided data such as hello next scheduled execution time.</span></span>

<span data-ttu-id="a14cb-149">a következő kód hello példát egy átfogó egy ütemezett feladatot.</span><span class="sxs-lookup"><span data-stu-id="a14cb-149">hello following code provides a comprehensive example of a scheduled job.</span></span> <span data-ttu-id="a14cb-150">Részletes információkat az ezt követő szakaszokban talál.</span><span class="sxs-lookup"><span data-stu-id="a14cb-150">Details are provided in subsequent sections.</span></span>

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often toofire (default too1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default toorecur infinitely)
            "endTime": "2012-11-04",                     // optional (default toorecur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

<span data-ttu-id="a14cb-151">Hello minta ütemezett feladat fent látható, akkor olyan feladatdefinícióban több részből rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a14cb-151">As seen in hello sample scheduled job above, a job definition has several parts:</span></span>

* <span data-ttu-id="a14cb-152">Kezdési idő („startTime”)</span><span class="sxs-lookup"><span data-stu-id="a14cb-152">Start time (“startTime”)</span></span>  
* <span data-ttu-id="a14cb-153">Művelet („action”), amely hibaműveletet („errorAction”) is tartalmaz</span><span class="sxs-lookup"><span data-stu-id="a14cb-153">Action (“action”), which includes error action (“errorAction”)</span></span>
* <span data-ttu-id="a14cb-154">Ismétlődés („recurrence”)</span><span class="sxs-lookup"><span data-stu-id="a14cb-154">Recurrence (“recurrence”)</span></span>  
* <span data-ttu-id="a14cb-155">Folyamatállapot („state”)</span><span class="sxs-lookup"><span data-stu-id="a14cb-155">State (“state”)</span></span>  
* <span data-ttu-id="a14cb-156">Feladatállapot („status”)</span><span class="sxs-lookup"><span data-stu-id="a14cb-156">Status (“status”)</span></span>  
* <span data-ttu-id="a14cb-157">Újrapróbálkozási házirend („retryPolicy”)</span><span class="sxs-lookup"><span data-stu-id="a14cb-157">Retry policy (“retryPolicy”)</span></span>  

<span data-ttu-id="a14cb-158">Vizsgáljuk meg részletesebben ezeket:</span><span class="sxs-lookup"><span data-stu-id="a14cb-158">Let’s examine each of these in detail:</span></span>

## <a name="starttime"></a><span data-ttu-id="a14cb-159">startTime</span><span class="sxs-lookup"><span data-stu-id="a14cb-159">startTime</span></span>
<span data-ttu-id="a14cb-160">"startTime" Hello hello kezdési ideje, és lehetővé teszi, hogy a hívó toospecify hello eltolás hello keresztülhaladnak a hálózaton lévő időzóna [ISO-8601 formátumban](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="a14cb-160">hello "startTime” is hello start time and allows hello caller toospecify a time zone offset on hello wire in [ISO-8601 format](http://en.wikipedia.org/wiki/ISO_8601).</span></span>

## <a name="action-and-erroraction"></a><span data-ttu-id="a14cb-161">action és errorAction</span><span class="sxs-lookup"><span data-stu-id="a14cb-161">action and errorAction</span></span>
<span data-ttu-id="a14cb-162">hello "action" hello művelet meghívása a mindegyik előfordulásakor, és egy szolgáltatás hívása típusú ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a14cb-162">hello “action” is hello action invoked on each occurrence and describes a type of service invocation.</span></span> <span data-ttu-id="a14cb-163">hello művelete mi végrehajtja a megadott ütemezés hello.</span><span class="sxs-lookup"><span data-stu-id="a14cb-163">hello action is what will be executed on hello provided schedule.</span></span> <span data-ttu-id="a14cb-164">A Scheduler támogatja a HTTP-, a tárolásisor-, a Service Bus üzenetsor- és a Service Bus témakörműveleteket.</span><span class="sxs-lookup"><span data-stu-id="a14cb-164">Scheduler supports HTTP, storage queue, service bus topic, and service bus queue actions.</span></span>

<span data-ttu-id="a14cb-165">a fenti példában hello hello művelet egy HTTP-műveletet.</span><span class="sxs-lookup"><span data-stu-id="a14cb-165">hello action in hello example above is an HTTP action.</span></span> <span data-ttu-id="a14cb-166">Az alábbiakban egy példát láthat egy tárolásisor-műveletre:</span><span class="sxs-lookup"><span data-stu-id="a14cb-166">Below is an example of a storage queue action:</span></span>

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

<span data-ttu-id="a14cb-167">Az alábbiakban egy példát láthat egy a Service Bus témakör-műveletre:</span><span class="sxs-lookup"><span data-stu-id="a14cb-167">Below is an example of a service bus topic action.</span></span>

  <span data-ttu-id="a14cb-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span><span class="sxs-lookup"><span data-stu-id="a14cb-168">"action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",</span></span>  
      <span data-ttu-id="a14cb-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span><span class="sxs-lookup"><span data-stu-id="a14cb-169">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }</span></span>

<span data-ttu-id="a14cb-170">Az alábbiakban egy példát láthat egy Service Bus üzenetsor-műveletre:</span><span class="sxs-lookup"><span data-stu-id="a14cb-170">Below is an example of a service bus queue action:</span></span>

  <span data-ttu-id="a14cb-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span><span class="sxs-lookup"><span data-stu-id="a14cb-171">"action": { "serviceBusQueueMessage": { "queueName": "q1",</span></span>  
      <span data-ttu-id="a14cb-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span><span class="sxs-lookup"><span data-stu-id="a14cb-172">"namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {</span></span>  
        <span data-ttu-id="a14cb-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span><span class="sxs-lookup"><span data-stu-id="a14cb-173">"sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",</span></span>  
      <span data-ttu-id="a14cb-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span><span class="sxs-lookup"><span data-stu-id="a14cb-174">"brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }</span></span>

<span data-ttu-id="a14cb-175">hello "errorAction" hello hibakezelő, ha hello elsődleges művelet nem sikerült meghívni hello művelet.</span><span class="sxs-lookup"><span data-stu-id="a14cb-175">hello “errorAction” is hello error handler, hello action invoked when hello primary action fails.</span></span> <span data-ttu-id="a14cb-176">A változó toocall hibakezelés végpont használhatja, vagy a felhasználó értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="a14cb-176">You can use this variable toocall an error-handling endpoint or send a user notification.</span></span> <span data-ttu-id="a14cb-177">Ez használható egy másodlagos végponti hello esetben eléréséhez adott hello elsődleges nem érhető el (például az hello esetben egy olyan vészhelyzet esetén hello végpont helyen), vagy egy hibakezelési végpont értesítésére használható.</span><span class="sxs-lookup"><span data-stu-id="a14cb-177">This can be used for reaching a secondary endpoint in hello case that hello primary is not available (e.g., in hello case of a disaster at hello endpoint’s site) or can be used for notifying an error handling endpoint.</span></span> <span data-ttu-id="a14cb-178">Hello elsődleges művelet, mint például a hello hiba művelet lehet más műveletek alapján egyszerű vagy összetett logikát.</span><span class="sxs-lookup"><span data-stu-id="a14cb-178">Just like hello primary action, hello error action can be simple or composite logic based on other actions.</span></span> <span data-ttu-id="a14cb-179">Hogyan toocreate egy SAS-jogkivonatot, tekintse meg a túl toolearn[létrehozhat és használhat egy közös hozzáférésű Jogosultságkód](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span><span class="sxs-lookup"><span data-stu-id="a14cb-179">toolearn how toocreate a SAS token, refer too[Create and Use a Shared Access Signature](https://msdn.microsoft.com/library/azure/jj721951.aspx).</span></span>

## <a name="recurrence"></a><span data-ttu-id="a14cb-180">recurrence</span><span class="sxs-lookup"><span data-stu-id="a14cb-180">recurrence</span></span>
<span data-ttu-id="a14cb-181">Az ismétlődés több részből áll:</span><span class="sxs-lookup"><span data-stu-id="a14cb-181">Recurrence has several parts:</span></span>

* <span data-ttu-id="a14cb-182">Gyakoriság: percenként, óránként, naponta, hetente, havonta, évente</span><span class="sxs-lookup"><span data-stu-id="a14cb-182">Frequency: One of minute, hour, day, week, month, year</span></span>  
* <span data-ttu-id="a14cb-183">Időköz: Hello Ismétlődési gyakoriság megadott hello időközét</span><span class="sxs-lookup"><span data-stu-id="a14cb-183">Interval: Interval at hello given frequency for hello recurrence</span></span>  
* <span data-ttu-id="a14cb-184">Előírt ütemezés: Adja meg a perc, óra, létrehozását, hónap és hello ismételt monthdays</span><span class="sxs-lookup"><span data-stu-id="a14cb-184">Prescribed schedule: Specify minutes, hours, weekdays, months, and monthdays of hello recurrence</span></span>  
* <span data-ttu-id="a14cb-185">Darabszám: Az előfordulások darabszáma</span><span class="sxs-lookup"><span data-stu-id="a14cb-185">Count: Count of occurrences</span></span>  
* <span data-ttu-id="a14cb-186">A Befejezés időpontja: nincsenek feladatok hajtja végre, miután hello megadott befejezési időpontja</span><span class="sxs-lookup"><span data-stu-id="a14cb-186">End time: No jobs will execute after hello specified end time</span></span>  

<span data-ttu-id="a14cb-187">Ismétlődő feladatról akkor beszélünk, ha ismétlődő objektummal rendelkezik a JSON-definíciójában.</span><span class="sxs-lookup"><span data-stu-id="a14cb-187">A job is recurring if it has a recurring object specified in its JSON definition.</span></span> <span data-ttu-id="a14cb-188">Ha száma és az EndTime megadása meg van adva, akkor következik be előbb hello befejezési szabály figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="a14cb-188">If both count and endTime are specified, hello completion rule that occurs first is honored.</span></span>

## <a name="state"></a><span data-ttu-id="a14cb-189">state</span><span class="sxs-lookup"><span data-stu-id="a14cb-189">state</span></span>
<span data-ttu-id="a14cb-190">hello hello feladat állapota négy értékek egyikét: engedélyezve, befejeződött-e vagy hibát jelzett.</span><span class="sxs-lookup"><span data-stu-id="a14cb-190">hello state of hello job is one of four values: enabled, disabled, completed, or faulted.</span></span> <span data-ttu-id="a14cb-191">HELYEZHET el vagy javítás feladatokat, mint tooupdate őket toohello engedélyezett vagy letiltott állapotba.</span><span class="sxs-lookup"><span data-stu-id="a14cb-191">You can PUT or PATCH jobs so as tooupdate them toohello enabled or disabled state.</span></span> <span data-ttu-id="a14cb-192">Ha egy feladat már befejeződött vagy meghibásodott, vagyis a végső állapot nem frissíthető, (bár a hello feladat továbbra is TÖRÖLHETŐK).</span><span class="sxs-lookup"><span data-stu-id="a14cb-192">If a job has been completed or faulted, that is a final state that cannot be updated (though hello job can still be DELETED).</span></span> <span data-ttu-id="a14cb-193">Hello-state tulajdonsága például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="a14cb-193">An example of hello state property is as follows:</span></span>

        "state": "disabled", // enabled, disabled, completed, or faulted
<span data-ttu-id="a14cb-194">A befejeződött vagy meghiúsult feladatok 60 nap után törlődnek.</span><span class="sxs-lookup"><span data-stu-id="a14cb-194">Completed and faulted jobs are deleted after 60 days.</span></span>

## <a name="status"></a><span data-ttu-id="a14cb-195">status</span><span class="sxs-lookup"><span data-stu-id="a14cb-195">status</span></span>
<span data-ttu-id="a14cb-196">Miután egy ütemezési feladat elindult, információkat visszaadott kapcsolatos hello hello feladat jelenlegi állapota.</span><span class="sxs-lookup"><span data-stu-id="a14cb-196">Once a Scheduler job has started, information will be returned about hello current status of hello job.</span></span> <span data-ttu-id="a14cb-197">Ez az objektum nincs hello felhasználó állítható be – hello rendszer szerint van állítva.</span><span class="sxs-lookup"><span data-stu-id="a14cb-197">This object is not settable by hello user—it’s set by hello system.</span></span> <span data-ttu-id="a14cb-198">Azonban szerepel hello feladatobjektum (nem pedig egy külön csatolt erőforrás), hogy egy egyszerűen szerezhet hello feladat állapotának.</span><span class="sxs-lookup"><span data-stu-id="a14cb-198">However, it is included in hello job object (rather than a separate linked resource) so that one can obtain hello status of a job easily.</span></span>

<span data-ttu-id="a14cb-199">A feladatállapot tartalmaz hello idő hello előző végrehajtás (ha van ilyen), hello hello következő ütemezett végrehajtáskor (a folyamatban lévő feladatok), és hello végrehajtási száma hello feladat.</span><span class="sxs-lookup"><span data-stu-id="a14cb-199">Job status includes hello time of hello previous execution (if any), hello time of hello next scheduled execution (for in-progress jobs), and hello execution count of hello job.</span></span>

## <a name="retrypolicy"></a><span data-ttu-id="a14cb-200">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="a14cb-200">retryPolicy</span></span>
<span data-ttu-id="a14cb-201">A Feladatütemező feladat sikertelen lesz, esetén lehetséges toospecify egy újrapróbálkozási házirend toodetermine, hogyan hello műveletet a rendszer ismét megkísérli.</span><span class="sxs-lookup"><span data-stu-id="a14cb-201">If a Scheduler job fails, it is possible toospecify a retry policy toodetermine whether and how hello action is retried.</span></span> <span data-ttu-id="a14cb-202">Hello határozzák **retryType** objektum – túl van beállítva**nincs** Ha nincs újrapróbálkozási házirend, ahogy fent látható.</span><span class="sxs-lookup"><span data-stu-id="a14cb-202">This is determined by hello **retryType** object—it is set too**none** if there is no retry policy, as shown above.</span></span> <span data-ttu-id="a14cb-203">Állítsa be úgy túl**rögzített** újrapróbálkozási házirendje esetén.</span><span class="sxs-lookup"><span data-stu-id="a14cb-203">Set it too**fixed** if there is a retry policy.</span></span>

<span data-ttu-id="a14cb-204">tooset újrapróbálkozási házirendje, két további beállítás adható meg: újrapróbálkozási időközt (**retryInterval**) és az újrapróbálkozások számát hello (**retryCount**).</span><span class="sxs-lookup"><span data-stu-id="a14cb-204">tooset a retry policy, two additional settings may be specified: a retry interval (**retryInterval**) and hello number of retries (**retryCount**).</span></span>

<span data-ttu-id="a14cb-205">hello újrapróbálkozási időköz hello megadott **retryInterval** objektum, a hello időköz újrapróbálkozások között.</span><span class="sxs-lookup"><span data-stu-id="a14cb-205">hello retry interval, specified with hello **retryInterval** object, is hello interval between retries.</span></span> <span data-ttu-id="a14cb-206">Ennek alapértelmezett értéke 30 másodperc; minimum 15 másodperc, maximum 18 hónap állítható be.</span><span class="sxs-lookup"><span data-stu-id="a14cb-206">Its default value is 30 seconds, its minimum configurable value is 15 seconds, and its maximum value is 18 months.</span></span> <span data-ttu-id="a14cb-207">Az Ingyenes feladatok gyűjteményében szereplő feladatok minimális konfigurálható értéke 1 óra.</span><span class="sxs-lookup"><span data-stu-id="a14cb-207">Jobs in Free job collections have a minimum configurable value of 1 hour.</span></span>  <span data-ttu-id="a14cb-208">Hello ISO 8601 formátumban van definiálva.</span><span class="sxs-lookup"><span data-stu-id="a14cb-208">It is defined in hello ISO 8601 format.</span></span> <span data-ttu-id="a14cb-209">Hasonló módon van megadva az újrapróbálkozások számát hello hello értékének hello **retryCount** objektum; hello szám, ahányszor egy újrapróbálkozás.</span><span class="sxs-lookup"><span data-stu-id="a14cb-209">Similarly, hello value of hello number of retries is specified with hello **retryCount** object; it is hello number of times a retry is attempted.</span></span> <span data-ttu-id="a14cb-210">Az alapértelmezett érték 4, és maximális értéke pedig 20\.</span><span class="sxs-lookup"><span data-stu-id="a14cb-210">Its default value is 4, and its maximum value is 20\.</span></span> <span data-ttu-id="a14cb-211">Mindkét **retryInterval** és **retryCount** megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="a14cb-211">Both **retryInterval** and **retryCount** are optional.</span></span> <span data-ttu-id="a14cb-212">Az alapértelmezett értékükön akkor kapnak, ha **retryType** értéke túl**rögzített** és nincs kifejezetten megadva.</span><span class="sxs-lookup"><span data-stu-id="a14cb-212">They are given their default values if **retryType** is set too**fixed** and no values are specified explicitly.</span></span>

## <a name="see-also"></a><span data-ttu-id="a14cb-213">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a14cb-213">See also</span></span>
 [<span data-ttu-id="a14cb-214">A Scheduler ismertetése</span><span class="sxs-lookup"><span data-stu-id="a14cb-214">What is Scheduler?</span></span>](scheduler-intro.md)

 [<span data-ttu-id="a14cb-215">Az ütemező hello Azure-portálon az első lépéseiben</span><span class="sxs-lookup"><span data-stu-id="a14cb-215">Get started using Scheduler in hello Azure portal</span></span>](scheduler-get-started-portal.md)

 [<span data-ttu-id="a14cb-216">Csomagok és számlázás az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="a14cb-216">Plans and billing in Azure Scheduler</span></span>](scheduler-plans-billing.md)

 [<span data-ttu-id="a14cb-217">Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező</span><span class="sxs-lookup"><span data-stu-id="a14cb-217">How toobuild complex schedules and advanced recurrence with Azure Scheduler</span></span>](scheduler-advanced-complexity.md)

 [<span data-ttu-id="a14cb-218">Az Azure Scheduler REST API-jának leírása</span><span class="sxs-lookup"><span data-stu-id="a14cb-218">Azure Scheduler REST API reference</span></span>](https://msdn.microsoft.com/library/mt629143)

 [<span data-ttu-id="a14cb-219">Az Azure Scheduler PowerShell-parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="a14cb-219">Azure Scheduler PowerShell cmdlets reference</span></span>](scheduler-powershell-reference.md)

 [<span data-ttu-id="a14cb-220">Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság</span><span class="sxs-lookup"><span data-stu-id="a14cb-220">Azure Scheduler high-availability and reliability</span></span>](scheduler-high-availability-reliability.md)

 [<span data-ttu-id="a14cb-221">Azure Scheduler – korlátozások, alapértékek és hibakódok</span><span class="sxs-lookup"><span data-stu-id="a14cb-221">Azure Scheduler limits, defaults, and error codes</span></span>](scheduler-limits-defaults-errors.md)

 [<span data-ttu-id="a14cb-222">Kimenő hitelesítés az Azure Schedulerben</span><span class="sxs-lookup"><span data-stu-id="a14cb-222">Azure Scheduler outbound authentication</span></span>](scheduler-outbound-authentication.md)

