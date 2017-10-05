---
title: "Az App Service API app eseményindítók |} Microsoft Docs"
description: "Az Azure App Service API-alkalmazás az eseményindítók implementálása"
services: logic-apps
documentationcenter: .net
author: guangyang
manager: erikre
editor: jimbe
ms.assetid: 493c3703-786d-4434-9dca-8f77744b2f5d
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: rachelap
ms.openlocfilehash: 3ddfb142e7f1a47e2a8564387da785acf36fa61f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-api-app-triggers"></a><span data-ttu-id="a14d0-103">Azure App Service API app triggers (Azure App Service API-alkalmazások eseményindítói)</span><span class="sxs-lookup"><span data-stu-id="a14d0-103">Azure App Service API app triggers</span></span>
> [!NOTE]
> <span data-ttu-id="a14d0-104">A cikk e verziója API apps 2014 12-01. dátumú előnézeti sémaverziójára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a14d0-104">This version of the article applies to API apps 2014-12-01-preview schema version.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="a14d0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a14d0-105">Overview</span></span>
<span data-ttu-id="a14d0-106">Ez a cikk azt ismerteti, hogyan API app eseményindítók és a logikai alkalmazás felhasználni azokat.</span><span class="sxs-lookup"><span data-stu-id="a14d0-106">This article explains how to implement API app triggers and consume them from a Logic app.</span></span>

<span data-ttu-id="a14d0-107">Minden ebben a témakörben a kódrészleteket másolja át a [FileWatcher API-alkalmazás kódminta](http://go.microsoft.com/fwlink/?LinkId=534802).</span><span class="sxs-lookup"><span data-stu-id="a14d0-107">All of the code snippets in this topic are copied from the [FileWatcher API App code sample](http://go.microsoft.com/fwlink/?LinkId=534802).</span></span>

<span data-ttu-id="a14d0-108">Vegye figyelembe, hogy töltse le a kód a cikk létrehozása és futtatása a következő nuget-csomagot kell: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span><span class="sxs-lookup"><span data-stu-id="a14d0-108">Note that you'll need to download the following nuget package for the code in this article to build and run: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span></span>

## <a name="what-are-api-app-triggers"></a><span data-ttu-id="a14d0-109">Mik azok a API app eseményindítók?</span><span class="sxs-lookup"><span data-stu-id="a14d0-109">What are API app triggers?</span></span>
<span data-ttu-id="a14d0-110">Egy általános forgatókönyv a API-alkalmazás egy eseményt, hogy az ügyfelek az API-alkalmazás az esemény bekövetkeztekor is elvégezheti a szükséges műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a14d0-110">It's a common scenario for an API app to fire an event so that clients of the API app can take the appropriate action in response to the event.</span></span> <span data-ttu-id="a14d0-111">A REST API-alapú mechanizmus, amely támogatja az ebben a forgatókönyvben egy API-alkalmazás eseményindító neve.</span><span class="sxs-lookup"><span data-stu-id="a14d0-111">The REST API based mechanism that supports this scenario is called an API app trigger.</span></span>

<span data-ttu-id="a14d0-112">Például tételezzük fel az Ügyfélkód használ a [Twitter-összekötő API-alkalmazás](../connectors/connectors-create-api-twitter.md) és a kódot kell egy műveletet, új Twitter-üzeneteket, amelyek szavakat tartalmaz alapján.</span><span class="sxs-lookup"><span data-stu-id="a14d0-112">For example, let's say your client code is using the [Twitter Connector API app](../connectors/connectors-create-api-twitter.md) and your code needs to perform an action based on new tweets that contain specific words.</span></span> <span data-ttu-id="a14d0-113">Ebben az esetben előfordulhat, hogy beállította egy lekérdezési vagy leküldéses eseményindító igénynek megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="a14d0-113">In this case, you might set up a poll or push trigger to facilitate this need.</span></span>

## <a name="poll-trigger-versus-push-trigger"></a><span data-ttu-id="a14d0-114">Leküldéses eseményindítók és a lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="a14d0-114">Poll trigger versus push trigger</span></span>
<span data-ttu-id="a14d0-115">Jelenleg kétféle típusú eseményindítók támogatottak:</span><span class="sxs-lookup"><span data-stu-id="a14d0-115">Currently, two types of triggers are supported:</span></span>

* <span data-ttu-id="a14d0-116">A lekérdezési eseményindító - ügyfél kérdezze le az értesítéshez egy esemény, amelynek lejárt az API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a14d0-116">Poll trigger - Client polls the API app for notification of an event having been fired</span></span>
* <span data-ttu-id="a14d0-117">Leküldéses eseményindító - ügyfél értesítést kaphat az API-alkalmazás által az esemény akkor következik be</span><span class="sxs-lookup"><span data-stu-id="a14d0-117">Push trigger - Client is notified by the API app when an event fires</span></span>

### <a name="poll-trigger"></a><span data-ttu-id="a14d0-118">A lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="a14d0-118">Poll trigger</span></span>
<span data-ttu-id="a14d0-119">Lekérdezési eseményindító valósul meg rendszeres REST API-t, és az ügyfelek (például a logikai alkalmazás) vár el, és kérdezze le az értesítési eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="a14d0-119">A poll trigger is implemented as a regular REST API and expects its clients (such as a Logic app) to poll it in order to get notification.</span></span> <span data-ttu-id="a14d0-120">Amíg az ügyfél esetleg-állapot karbantartásához, a lekérdezési eseményindító maga állapotmentes.</span><span class="sxs-lookup"><span data-stu-id="a14d0-120">While the client may maintain state, the poll trigger itself is stateless.</span></span>

<span data-ttu-id="a14d0-121">A következő adatokat a kérelem-válasz csomagok kapcsolatban néhány kulcsfontosságú elemeit annak a lekérdezési eseményindító szerződés mutatja be:</span><span class="sxs-lookup"><span data-stu-id="a14d0-121">The following information regarding the request and response packets illustrate some key aspects of the poll trigger contract:</span></span>

* <span data-ttu-id="a14d0-122">Kérés</span><span class="sxs-lookup"><span data-stu-id="a14d0-122">Request</span></span>
  * <span data-ttu-id="a14d0-123">HTTP-metódus: beolvasása</span><span class="sxs-lookup"><span data-stu-id="a14d0-123">HTTP method: GET</span></span>
  * <span data-ttu-id="a14d0-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a14d0-124">Parameters</span></span>
    * <span data-ttu-id="a14d0-125">triggerState - a nem kötelező paraméter lehetővé teszi az ügyfelek állapotukra, hogy a lekérdezési eseményindító is megfelelően be kell-e értesítés ad vissza, vagy nem a megadott állapotán alapuló megadását.</span><span class="sxs-lookup"><span data-stu-id="a14d0-125">triggerState - This optional parameter allows clients to specify their state so that the poll trigger can properly decide whether to return notification or not based on the specified state.</span></span>
    * <span data-ttu-id="a14d0-126">API-specifikus paramétereket</span><span class="sxs-lookup"><span data-stu-id="a14d0-126">API-specific parameters</span></span>
* <span data-ttu-id="a14d0-127">Válasz</span><span class="sxs-lookup"><span data-stu-id="a14d0-127">Response</span></span>
  * <span data-ttu-id="a14d0-128">Állapotkód **200** - kérelem érvényes, és a eseményindítóval értesítést.</span><span class="sxs-lookup"><span data-stu-id="a14d0-128">Status code **200** - Request is valid and there is a notification from the trigger.</span></span> <span data-ttu-id="a14d0-129">Az értesítési tartalom az adott válasz törzsének lesz.</span><span class="sxs-lookup"><span data-stu-id="a14d0-129">The content of the notification will be the response body.</span></span> <span data-ttu-id="a14d0-130">A válaszban szereplő "Újrapróbálkozási-után" fejléc azt jelzi, hogy további értesítési adatokat kell beolvasni későbbi kérés hívással.</span><span class="sxs-lookup"><span data-stu-id="a14d0-130">A "Retry-After" header in the response indicates that additional notification data must be retrieved with a subsequent request call.</span></span>
  * <span data-ttu-id="a14d0-131">Állapotkód **202** - kérelem érvényes, de nincs új értesítés a eseményindítóval van.</span><span class="sxs-lookup"><span data-stu-id="a14d0-131">Status code **202** - Request is valid, but there is no new notification from the trigger.</span></span>
  * <span data-ttu-id="a14d0-132">Állapotkód **4xx** -kérelem érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="a14d0-132">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="a14d0-133">Az ügyfél nem kell újra a kéréssel.</span><span class="sxs-lookup"><span data-stu-id="a14d0-133">The client should not retry the request.</span></span>
  * <span data-ttu-id="a14d0-134">Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett.</span><span class="sxs-lookup"><span data-stu-id="a14d0-134">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="a14d0-135">Az ügyfél kell újra a kéréssel.</span><span class="sxs-lookup"><span data-stu-id="a14d0-135">The client should retry the request.</span></span>

<span data-ttu-id="a14d0-136">A következő kódrészlet példa bemutatja, hogyan lekérdezési eseményindító végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a14d0-136">The following code snippet is an example of how to implement a poll trigger.</span></span>

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

<span data-ttu-id="a14d0-137">A lekérdezési eseményindító teszteléséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a14d0-137">To test this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="a14d0-138">Egy hitelesítés használatára az API-alkalmazás telepítése **névtelen nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-138">Deploy the API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="a14d0-139">Hívja a **touch** fájl touch művelet.</span><span class="sxs-lookup"><span data-stu-id="a14d0-139">Call the **touch** operation to touch a file.</span></span> <span data-ttu-id="a14d0-140">Az alábbi ábrán egy minta kérelem Postman keresztül.</span><span class="sxs-lookup"><span data-stu-id="a14d0-140">The following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="a14d0-141">![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="a14d0-141">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
3. <span data-ttu-id="a14d0-142">A lekérdezési eseményindító hívja a **triggerState** paraméter értéke egy időbélyegző #2. lépés előtt.</span><span class="sxs-lookup"><span data-stu-id="a14d0-142">Call the poll trigger with the **triggerState** parameter set to a time stamp prior to Step #2.</span></span> <span data-ttu-id="a14d0-143">A következő kép bemutatja a kérelemmintát Postman keresztül.</span><span class="sxs-lookup"><span data-stu-id="a14d0-143">The following image shows the sample request via Postman.</span></span>
   <span data-ttu-id="a14d0-144">![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="a14d0-144">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span></span>

### <a name="push-trigger"></a><span data-ttu-id="a14d0-145">Leküldéses eseményindítók</span><span class="sxs-lookup"><span data-stu-id="a14d0-145">Push trigger</span></span>
<span data-ttu-id="a14d0-146">A leküldéses eseményindítót, egy rendszeres REST API-t, amely a leküldéses értesítések értesítések az ügyfelek számára, ha adott eseményeket érvényesítést értesítést szeretne kapni regisztrált lett megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="a14d0-146">A push trigger is implemented as a regular REST API that pushes notifications to clients who have registered to be notified when specific events fire.</span></span>

<span data-ttu-id="a14d0-147">A következő adatokat a kérelem-válasz csomagok kapcsolatban a leküldéses eseményindító szerződés néhány kulcsfontosságú elemeit mutatják be.</span><span class="sxs-lookup"><span data-stu-id="a14d0-147">The following information regarding the request and response packets illustrate some key aspects of the push trigger contract.</span></span>

* <span data-ttu-id="a14d0-148">Kérés</span><span class="sxs-lookup"><span data-stu-id="a14d0-148">Request</span></span>
  * <span data-ttu-id="a14d0-149">HTTP-metódus: PUT</span><span class="sxs-lookup"><span data-stu-id="a14d0-149">HTTP method: PUT</span></span>
  * <span data-ttu-id="a14d0-150">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a14d0-150">Parameters</span></span>
    * <span data-ttu-id="a14d0-151">eseményindító azonosítója: szükséges – nem átlátszó karakterlánc (például egy GUID), amely a regisztráció leküldéses eseményindító jelöli.</span><span class="sxs-lookup"><span data-stu-id="a14d0-151">triggerId: required - Opaque string (such as a GUID) that represents the registration of a push trigger.</span></span>
    * <span data-ttu-id="a14d0-152">callbackUrl: szükséges – a visszahívás URL-cím lehet meghívni, ha az esemény akkor következik be.</span><span class="sxs-lookup"><span data-stu-id="a14d0-152">callbackUrl: required - URL of the callback to invoke when the event fires.</span></span> <span data-ttu-id="a14d0-153">A meghívási egy egyszerű POST HTTP-hívás, amely.</span><span class="sxs-lookup"><span data-stu-id="a14d0-153">The invocation is a simple POST HTTP call.</span></span>
    * <span data-ttu-id="a14d0-154">API-specifikus paramétereket</span><span class="sxs-lookup"><span data-stu-id="a14d0-154">API-specific parameters</span></span>
* <span data-ttu-id="a14d0-155">Válasz</span><span class="sxs-lookup"><span data-stu-id="a14d0-155">Response</span></span>
  * <span data-ttu-id="a14d0-156">Állapotkód **200** -kérelem regisztrálása sikeres ügyfél.</span><span class="sxs-lookup"><span data-stu-id="a14d0-156">Status code **200** - Request to register client successful.</span></span>
  * <span data-ttu-id="a14d0-157">Állapotkód **4xx** -kérelem érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="a14d0-157">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="a14d0-158">Az ügyfél nem kell újra a kéréssel.</span><span class="sxs-lookup"><span data-stu-id="a14d0-158">The client should not retry the request.</span></span>
  * <span data-ttu-id="a14d0-159">Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett.</span><span class="sxs-lookup"><span data-stu-id="a14d0-159">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="a14d0-160">Az ügyfél kell újra a kéréssel.</span><span class="sxs-lookup"><span data-stu-id="a14d0-160">The client should retry the request.</span></span>
* <span data-ttu-id="a14d0-161">A visszahívási</span><span class="sxs-lookup"><span data-stu-id="a14d0-161">Callback</span></span>
  * <span data-ttu-id="a14d0-162">HTTP-metódus: POST</span><span class="sxs-lookup"><span data-stu-id="a14d0-162">HTTP method: POST</span></span>
  * <span data-ttu-id="a14d0-163">A kérelem törzse: értesítési tartalom.</span><span class="sxs-lookup"><span data-stu-id="a14d0-163">Request body: Notification content.</span></span>

<span data-ttu-id="a14d0-164">A következő kódrészletet a következő példa bemutatja, hogyan leküldéses eseményindító végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="a14d0-164">The following code snippet is an example of how to implement a push trigger:</span></span>

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

<span data-ttu-id="a14d0-165">A lekérdezési eseményindító teszteléséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a14d0-165">To test this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="a14d0-166">Egy hitelesítés használatára az API-alkalmazás telepítése **névtelen nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-166">Deploy the API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="a14d0-167">Keresse meg a [http://requestb.in/](http://requestb.in/) egy RequestBin, amelyek erre a célra a visszahívási URL-cím létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a14d0-167">Browse to [http://requestb.in/](http://requestb.in/) to create a RequestBin which will serve as your callback URL.</span></span>
3. <span data-ttu-id="a14d0-168">A leküldéses eseményindító, GUID-nak hívja **eseményindító azonosítója** és a RequestBin az URL-címet **callbackUrl**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-168">Call the push trigger with a GUID as **triggerId** and the RequestBin URL as **callbackUrl**.</span></span>
   <span data-ttu-id="a14d0-169">![Hívást leküldéses eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="a14d0-169">![Call Push Trigger via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span></span>
4. <span data-ttu-id="a14d0-170">Hívja a **touch** fájl touch művelet.</span><span class="sxs-lookup"><span data-stu-id="a14d0-170">Call the **touch** operation to touch a file.</span></span> <span data-ttu-id="a14d0-171">Az alábbi ábrán egy minta kérelem Postman keresztül.</span><span class="sxs-lookup"><span data-stu-id="a14d0-171">The following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="a14d0-172">![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="a14d0-172">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
5. <span data-ttu-id="a14d0-173">Annak ellenőrzéséhez, hogy a leküldéses eseményindító visszahívás hívása tulajdonság kimeneti RequestBin ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a14d0-173">Check the RequestBin to confirm that the push trigger callback is invoked with property output.</span></span>
   <span data-ttu-id="a14d0-174">![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span><span class="sxs-lookup"><span data-stu-id="a14d0-174">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span></span>

### <a name="describe-triggers-in-api-definition"></a><span data-ttu-id="a14d0-175">Az API-definíció eseményindítók leírása</span><span class="sxs-lookup"><span data-stu-id="a14d0-175">Describe triggers in API definition</span></span>
<span data-ttu-id="a14d0-176">Utáni végrehajtási az eseményindítókat és az API-alkalmazás telepítése az Azure-ba, navigáljon a **API-definíció** panel az Azure betekintő portálon, és jelenik meg, hogy az eseményindítók a rendszer automatikusan felismeri a felhasználói felületen, amelyek célja a Swagger 2.0 API-definíció az API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a14d0-176">After implementing the triggers and deploying your API app to Azure, navigate to the **API Definition** blade in the Azure preview portal and you'll see that triggers are automatically recognized in the UI, which is driven by the Swagger 2.0 API definition of the API app.</span></span>

![API Definition panel](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

<span data-ttu-id="a14d0-178">Ha a **letöltése Swagger** gombra, és nyissa meg a JSON-fájlt, az alábbihoz hasonló eredményeket talál:</span><span class="sxs-lookup"><span data-stu-id="a14d0-178">If you click the **Download Swagger** button and open the JSON file, you'll see results similar to the following:</span></span>

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

<span data-ttu-id="a14d0-179">A bővített tulajdonság **x-ms-schedular-eseményindító** hogyan eseményindítók részelemcímkék ismertetését API-definíció, és automatikusan kerül az API-alkalmazás átjáró által az API-definíció az átjárón keresztül kérése, ha a kérelem a következő feltételek egyike.</span><span class="sxs-lookup"><span data-stu-id="a14d0-179">The extension property **x-ms-schedular-trigger** is how triggers are described in API definition, and is automatically added by the API app gateway when you request the API definition via the gateway if the request to one of the following criteria.</span></span> <span data-ttu-id="a14d0-180">(Azt is megteheti ezt a tulajdonságot manuálisan.)</span><span class="sxs-lookup"><span data-stu-id="a14d0-180">(You can also add this property manually.)</span></span>

* <span data-ttu-id="a14d0-181">A lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="a14d0-181">Poll trigger</span></span>
  * <span data-ttu-id="a14d0-182">Ha a HTTP-metódus **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-182">If the HTTP method is **GET**.</span></span>
  * <span data-ttu-id="a14d0-183">Ha a **OperationID azonosítójú** tulajdonság tartalmazza a karakterláncot **eseményindító**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-183">If the **operationId** property contains the string **trigger**.</span></span>
  * <span data-ttu-id="a14d0-184">Ha a **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága **triggerState**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-184">If the **parameters** property includes a parameter with a **name** property set to **triggerState**.</span></span>
* <span data-ttu-id="a14d0-185">Leküldéses eseményindítók</span><span class="sxs-lookup"><span data-stu-id="a14d0-185">Push trigger</span></span>
  * <span data-ttu-id="a14d0-186">Ha a HTTP-metódus **PUT**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-186">If the HTTP method is **PUT**.</span></span>
  * <span data-ttu-id="a14d0-187">Ha a **OperationID azonosítójú** tulajdonság tartalmazza a karakterláncot **eseményindító**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-187">If the **operationId** property contains the string **trigger**.</span></span>
  * <span data-ttu-id="a14d0-188">Ha a **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága **eseményindító azonosítója**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-188">If the **parameters** property includes a parameter with a **name** property set to **triggerId**.</span></span>

## <a name="use-api-app-triggers-in-logic-apps"></a><span data-ttu-id="a14d0-189">A Logic apps API app eseményindítók használata</span><span class="sxs-lookup"><span data-stu-id="a14d0-189">Use API app triggers in Logic apps</span></span>
### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a><span data-ttu-id="a14d0-190">Listázhatja és konfigurálhatja az API app eseményindítók a Logic apps-tervezőben</span><span class="sxs-lookup"><span data-stu-id="a14d0-190">List and configure API app triggers in the Logic apps designer</span></span>
<span data-ttu-id="a14d0-191">Logikai alkalmazás ugyanabban az erőforráscsoportban, az API-alkalmazást hoz létre, ha lesz, egyszerűen kattintással veheti fel a tervezői vásznon.</span><span class="sxs-lookup"><span data-stu-id="a14d0-191">If you create a Logic app in the same resource group as the API app, you will be able to add it to the designer canvas simply by clicking it.</span></span> <span data-ttu-id="a14d0-192">A következő lemezképek ezt mutatják be:</span><span class="sxs-lookup"><span data-stu-id="a14d0-192">The following images illustrate this:</span></span>

![A Logic App Designer eseményindítók](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![A lekérdezési eseményindító Logic App Designer konfigurálása](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![A Logic App Designer leküldéses eseményindító konfigurálása](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a><span data-ttu-id="a14d0-196">A Logic apps az API app eseményindítók optimalizálása</span><span class="sxs-lookup"><span data-stu-id="a14d0-196">Optimize API app triggers for Logic apps</span></span>
<span data-ttu-id="a14d0-197">Miután hozzáadta a eseményindítók API-alkalmazásba, dolgot néhány látna szívesen, ha az API-alkalmazás a logikai alkalmazás is van.</span><span class="sxs-lookup"><span data-stu-id="a14d0-197">After you add triggers to an API app, there are a few things you can do to improve the experience when using the API app in a Logic app.</span></span>

<span data-ttu-id="a14d0-198">Például a **triggerState** lekérdezéses eseményindítók paramétert kell beállítani a logikai alkalmazást a következő kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="a14d0-198">For instance, the **triggerState** parameter for poll triggers should be set to the following expression in the Logic app.</span></span> <span data-ttu-id="a14d0-199">Ebben a kifejezésben kell értékelje ki a logikai alkalmazást az eseményindító utolsó meghívását, és ezt az értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a14d0-199">This expression should evaluate the last invocation of the trigger from the Logic app, and return that value.</span></span>  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

<span data-ttu-id="a14d0-200">Megjegyzés: A kifejezésben használt függvény annak magyarázatát, tekintse meg a dokumentációt a [Logic App Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span><span class="sxs-lookup"><span data-stu-id="a14d0-200">NOTE: For an explanation of the functions used in the expression above, refer to the documentation on [Logic App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span></span>

<span data-ttu-id="a14d0-201">Logic app felhasználók van viszont a kifejezést fent a **triggerState** paraméter használatakor az eseményindító.</span><span class="sxs-lookup"><span data-stu-id="a14d0-201">Logic app users would need to provide the expression above for the **triggerState** parameter while using the trigger.</span></span> <span data-ttu-id="a14d0-202">Ez a Logic app Designer keresztül a bővített tulajdonság beállított értéke lehet **x-ms-ütemező – ajánlás**.</span><span class="sxs-lookup"><span data-stu-id="a14d0-202">It is possible to have this value preset by the Logic app designer through the extension property **x-ms-scheduler-recommendation**.</span></span>  <span data-ttu-id="a14d0-203">A **x-ms-láthatósági** bővített tulajdonság értéke állítható be *belső* , hogy a paraméter maga nem jelenik meg a tervező.</span><span class="sxs-lookup"><span data-stu-id="a14d0-203">The **x-ms-visibility** extension property can be set to a value of *internal* so that the parameter itself is not shown on the designer.</span></span>  <span data-ttu-id="a14d0-204">A következő kódrészletet, amely mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a14d0-204">The following snippet illustrates that.</span></span>

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

<span data-ttu-id="a14d0-205">Leküldéses eseményindítók a **eseményindító azonosítója** paraméter egyedi módon kell azonosítania a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a14d0-205">For push triggers, the **triggerId** parameter must uniquely identify the Logic app.</span></span> <span data-ttu-id="a14d0-206">Ajánlott eljárás is, hogy ez a tulajdonság a munkafolyamat nevét a következő kifejezés használatával:</span><span class="sxs-lookup"><span data-stu-id="a14d0-206">A recommended best practice is to set this property to the name of the workflow by using the following expression:</span></span>

    @workflow().name

<span data-ttu-id="a14d0-207">Használja a **x-ms-ütemező – javaslat** és **x-ms-láthatósági** bővítmény tulajdonságai a saját API definiton, az API-alkalmazás is továbbítja a Logic app designer automatikusan beállítja az ebben a kifejezésben, a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="a14d0-207">Using the **x-ms-scheduler-recommendation** and **x-ms-visibility** extension properties in its API definiton, the API app can convey to the Logic app designer to automatically set this expression for the user.</span></span>

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a><span data-ttu-id="a14d0-208">Az API attribútumdefiníciós bővítmény tulajdonságai hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a14d0-208">Add extension properties in API defintion</span></span>
<span data-ttu-id="a14d0-209">További metaadat-információkat – például a bővítmény tulajdonságai **x-ms-ütemező – ajánlás** és **x-ms-láthatósági** -lehet hozzáadni a két módszer egyikével API attribútumdefiníciós: statikus vagy dinamikus.</span><span class="sxs-lookup"><span data-stu-id="a14d0-209">Additional metadata information - such as the extension properties **x-ms-scheduler-recommendation** and **x-ms-visibility** - can be added in the API defintion in one of two ways: static or dynamic.</span></span>

<span data-ttu-id="a14d0-210">A statikus metaadatok közvetlenül szerkesztheti a */metadata/apiDefinition.swagger.json* a projekt fájlt, és adja hozzá manuálisan a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="a14d0-210">For static metadata, you can directly edit the */metadata/apiDefinition.swagger.json* file in your project and add the properties manually.</span></span>

<span data-ttu-id="a14d0-211">API-alkalmazások dinamikus metaadatok segítségével szerkesztheti a SwaggerConfig.cs fájlt egy művelet szűrő, amely adhat hozzá a következő kiterjesztések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="a14d0-211">For API apps using dynamic metadata, you can edit the SwaggerConfig.cs file to add an operation filter which can add these extensions.</span></span>

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


<span data-ttu-id="a14d0-212">Hogyan Ez az osztály lehetővé teszi a dinamikus metaadatok forgatókönyv implementálhatók példát a következő:</span><span class="sxs-lookup"><span data-stu-id="a14d0-212">The following is an example of how this class can be implemented to facilitate the dynamic metadata scenario.</span></span>

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
