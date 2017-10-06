---
title: "aaaApp Service API app eseményindítók |} Microsoft Docs"
description: "Hogyan tooimplement elindítja az API-alkalmazás az Azure App Service-ben"
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
ms.openlocfilehash: 2d6b6a942a23c0a93987e9c48b69ecc739bfd814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-api-app-triggers"></a><span data-ttu-id="dfb11-103">Azure App Service API app triggers (Azure App Service API-alkalmazások eseményindítói)</span><span class="sxs-lookup"><span data-stu-id="dfb11-103">Azure App Service API app triggers</span></span>
> [!NOTE]
> <span data-ttu-id="dfb11-104">Hello cikk e verziója tooAPI apps 2014-12-01-előnézeti sémaverziójára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="dfb11-104">This version of hello article applies tooAPI apps 2014-12-01-preview schema version.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="dfb11-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dfb11-105">Overview</span></span>
<span data-ttu-id="dfb11-106">Ez a cikk azt ismerteti, hogyan tooimplement API app váltja ki, és felhasználni azokat a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dfb11-106">This article explains how tooimplement API app triggers and consume them from a Logic app.</span></span>

<span data-ttu-id="dfb11-107">Minden ebben a témakörben hello kódtöredékek hello átmásolva [FileWatcher API-alkalmazás kódminta](http://go.microsoft.com/fwlink/?LinkId=534802).</span><span class="sxs-lookup"><span data-stu-id="dfb11-107">All of hello code snippets in this topic are copied from hello [FileWatcher API App code sample](http://go.microsoft.com/fwlink/?LinkId=534802).</span></span>

<span data-ttu-id="dfb11-108">Vegye figyelembe, hogy szüksége lesz a következő hello kód Ez a cikk toobuild a nuget-csomagot, és futtassa toodownload hello: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span><span class="sxs-lookup"><span data-stu-id="dfb11-108">Note that you'll need toodownload hello following nuget package for hello code in this article toobuild and run: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span></span>

## <a name="what-are-api-app-triggers"></a><span data-ttu-id="dfb11-109">Mik azok a API app eseményindítók?</span><span class="sxs-lookup"><span data-stu-id="dfb11-109">What are API app triggers?</span></span>
<span data-ttu-id="dfb11-110">Egy általános forgatókönyv egy API-alkalmazás toofire egy eseményt az, hogy az ügyfelek hello API-alkalmazás is hello megfelelő intézkedéseket válasz toohello eseményben.</span><span class="sxs-lookup"><span data-stu-id="dfb11-110">It's a common scenario for an API app toofire an event so that clients of hello API app can take hello appropriate action in response toohello event.</span></span> <span data-ttu-id="dfb11-111">hello REST API-alapú mechanizmus, amely támogatja az ebben a forgatókönyvben egy API-alkalmazás eseményindító neve.</span><span class="sxs-lookup"><span data-stu-id="dfb11-111">hello REST API based mechanism that supports this scenario is called an API app trigger.</span></span>

<span data-ttu-id="dfb11-112">Például tételezzük fel az Ügyfélkód használ hello [Twitter-összekötő API-alkalmazás](../connectors/connectors-create-api-twitter.md) , ezért a kód tooperform művelet alapuló új Twitter-üzeneteket, amelyek szavakat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="dfb11-112">For example, let's say your client code is using hello [Twitter Connector API app](../connectors/connectors-create-api-twitter.md) and your code needs tooperform an action based on new tweets that contain specific words.</span></span> <span data-ttu-id="dfb11-113">Ebben az esetben előfordulhat, hogy beállította egy lekérdezési vagy leküldéses eseményindító toofacilitate igénynek.</span><span class="sxs-lookup"><span data-stu-id="dfb11-113">In this case, you might set up a poll or push trigger toofacilitate this need.</span></span>

## <a name="poll-trigger-versus-push-trigger"></a><span data-ttu-id="dfb11-114">Leküldéses eseményindítók és a lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="dfb11-114">Poll trigger versus push trigger</span></span>
<span data-ttu-id="dfb11-115">Jelenleg kétféle típusú eseményindítók támogatottak:</span><span class="sxs-lookup"><span data-stu-id="dfb11-115">Currently, two types of triggers are supported:</span></span>

* <span data-ttu-id="dfb11-116">A lekérdezési eseményindító - ügyfél kérdezze le az értesítés egy eseményt, hogy lejárt a hello API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="dfb11-116">Poll trigger - Client polls hello API app for notification of an event having been fired</span></span>
* <span data-ttu-id="dfb11-117">Leküldéses eseményindító - ügyfél értesítést hello API-alkalmazás által az esemény akkor következik be</span><span class="sxs-lookup"><span data-stu-id="dfb11-117">Push trigger - Client is notified by hello API app when an event fires</span></span>

### <a name="poll-trigger"></a><span data-ttu-id="dfb11-118">A lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="dfb11-118">Poll trigger</span></span>
<span data-ttu-id="dfb11-119">A lekérdezési eseményindító valósul meg rendszeres REST API-t, és az ügyfelek (például a logikai alkalmazás) toopoll vár az order tooget értesítés.</span><span class="sxs-lookup"><span data-stu-id="dfb11-119">A poll trigger is implemented as a regular REST API and expects its clients (such as a Logic app) toopoll it in order tooget notification.</span></span> <span data-ttu-id="dfb11-120">Hello ügyfél fenntarthatja állapotba, amíg hello lekérdezési eseményindító magát az állapot nélküli.</span><span class="sxs-lookup"><span data-stu-id="dfb11-120">While hello client may maintain state, hello poll trigger itself is stateless.</span></span>

<span data-ttu-id="dfb11-121">hello hello kérés- és csomagok vonatkozó információkat a következő néhány kulcsfontosságú elemeit annak hello lekérdezési eseményindító szerződés mutatja be:</span><span class="sxs-lookup"><span data-stu-id="dfb11-121">hello following information regarding hello request and response packets illustrate some key aspects of hello poll trigger contract:</span></span>

* <span data-ttu-id="dfb11-122">Kérés</span><span class="sxs-lookup"><span data-stu-id="dfb11-122">Request</span></span>
  * <span data-ttu-id="dfb11-123">HTTP-metódus: beolvasása</span><span class="sxs-lookup"><span data-stu-id="dfb11-123">HTTP method: GET</span></span>
  * <span data-ttu-id="dfb11-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="dfb11-124">Parameters</span></span>
    * <span data-ttu-id="dfb11-125">triggerState - a nem kötelező paraméter lehetővé teszi az ügyfelek úgy, hogy a lekérdezési eseményindító hello állapotukra is megfelelően toospecify mellett dönt, hogy tooreturn értesítési vagy nem alapján hello megadott állapot.</span><span class="sxs-lookup"><span data-stu-id="dfb11-125">triggerState - This optional parameter allows clients toospecify their state so that hello poll trigger can properly decide whether tooreturn notification or not based on hello specified state.</span></span>
    * <span data-ttu-id="dfb11-126">API-specifikus paramétereket</span><span class="sxs-lookup"><span data-stu-id="dfb11-126">API-specific parameters</span></span>
* <span data-ttu-id="dfb11-127">Válasz</span><span class="sxs-lookup"><span data-stu-id="dfb11-127">Response</span></span>
  * <span data-ttu-id="dfb11-128">Állapotkód **200** - kérelem érvényes, és értesítést hello eseményindítóval.</span><span class="sxs-lookup"><span data-stu-id="dfb11-128">Status code **200** - Request is valid and there is a notification from hello trigger.</span></span> <span data-ttu-id="dfb11-129">hello tartalom hello értesítés hello adott válasz törzsének lesz.</span><span class="sxs-lookup"><span data-stu-id="dfb11-129">hello content of hello notification will be hello response body.</span></span> <span data-ttu-id="dfb11-130">Hello válaszul "Újrapróbálkozási-után" fejléc azt jelzi, hogy további értesítési adatokat kell beolvasni későbbi kérés hívással.</span><span class="sxs-lookup"><span data-stu-id="dfb11-130">A "Retry-After" header in hello response indicates that additional notification data must be retrieved with a subsequent request call.</span></span>
  * <span data-ttu-id="dfb11-131">Állapotkód **202** - kérelem érvényes, de nincs új értesítés hello eseményindítóval van.</span><span class="sxs-lookup"><span data-stu-id="dfb11-131">Status code **202** - Request is valid, but there is no new notification from hello trigger.</span></span>
  * <span data-ttu-id="dfb11-132">Állapotkód **4xx** -kérelem érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="dfb11-132">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="dfb11-133">hello ügyfél hello kérelem nem újra kell próbálkoznia.</span><span class="sxs-lookup"><span data-stu-id="dfb11-133">hello client should not retry hello request.</span></span>
  * <span data-ttu-id="dfb11-134">Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett.</span><span class="sxs-lookup"><span data-stu-id="dfb11-134">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="dfb11-135">hello ügyfél hello kérelem újra kell próbálkoznia.</span><span class="sxs-lookup"><span data-stu-id="dfb11-135">hello client should retry hello request.</span></span>

<span data-ttu-id="dfb11-136">a következő kódrészletet hello példája hogyan indítható el, a lekérdezési tooimplement.</span><span class="sxs-lookup"><span data-stu-id="dfb11-136">hello following code snippet is an example of how tooimplement a poll trigger.</span></span>

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check toosee whether there is any file touched after hello timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after hello timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after hello timestamp, tell hello caller toopoll again after 1 mintue.
        else
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

<span data-ttu-id="dfb11-137">indítás, a lekérdezési tootest, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dfb11-137">tootest this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="dfb11-138">Hello API App hitelesítési-beállítással a telepítése **névtelen nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-138">Deploy hello API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="dfb11-139">Hello hívás **touch** művelet tootouch egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="dfb11-139">Call hello **touch** operation tootouch a file.</span></span> <span data-ttu-id="dfb11-140">hello kép a következő egy minta kérelem keresztül Postman jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="dfb11-140">hello following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="dfb11-141">![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="dfb11-141">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
3. <span data-ttu-id="dfb11-142">Hello lekérdezési eseményindító hello hívás **triggerState** paraméterkészlet tooa idő stamp előzetes tooStep #2.</span><span class="sxs-lookup"><span data-stu-id="dfb11-142">Call hello poll trigger with hello **triggerState** parameter set tooa time stamp prior tooStep #2.</span></span> <span data-ttu-id="dfb11-143">hello következő kép bemutatja hello kérelemmintát Postman keresztül.</span><span class="sxs-lookup"><span data-stu-id="dfb11-143">hello following image shows hello sample request via Postman.</span></span>
   <span data-ttu-id="dfb11-144">![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="dfb11-144">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span></span>

### <a name="push-trigger"></a><span data-ttu-id="dfb11-145">Leküldéses eseményindítók</span><span class="sxs-lookup"><span data-stu-id="dfb11-145">Push trigger</span></span>
<span data-ttu-id="dfb11-146">A leküldéses eseményindítót, egy rendszeres REST API-t, amely értesíti, ha a meghatározott események érvényesítést toobe regisztrált értesítések tooclients leküldéses értesítések lett megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="dfb11-146">A push trigger is implemented as a regular REST API that pushes notifications tooclients who have registered toobe notified when specific events fire.</span></span>

<span data-ttu-id="dfb11-147">hello kérés- és csomagok vonatkozó információkat a következő hello hello leküldéses eseményindító szerződés néhány kulcsfontosságú elemeit mutatják be.</span><span class="sxs-lookup"><span data-stu-id="dfb11-147">hello following information regarding hello request and response packets illustrate some key aspects of hello push trigger contract.</span></span>

* <span data-ttu-id="dfb11-148">Kérés</span><span class="sxs-lookup"><span data-stu-id="dfb11-148">Request</span></span>
  * <span data-ttu-id="dfb11-149">HTTP-metódus: PUT</span><span class="sxs-lookup"><span data-stu-id="dfb11-149">HTTP method: PUT</span></span>
  * <span data-ttu-id="dfb11-150">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="dfb11-150">Parameters</span></span>
    * <span data-ttu-id="dfb11-151">eseményindító azonosítója: szükséges – átlátszatlan karakterlánc (például egy GUID), hogy jelöli hello regisztráció leküldéses eseményindító.</span><span class="sxs-lookup"><span data-stu-id="dfb11-151">triggerId: required - Opaque string (such as a GUID) that represents hello registration of a push trigger.</span></span>
    * <span data-ttu-id="dfb11-152">callbackUrl: szükséges – hello visszahívási tooinvoke hello az esemény akkor következik be, amikor URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="dfb11-152">callbackUrl: required - URL of hello callback tooinvoke when hello event fires.</span></span> <span data-ttu-id="dfb11-153">hello meghívása egy egyszerű POST HTTP-hívás, amely.</span><span class="sxs-lookup"><span data-stu-id="dfb11-153">hello invocation is a simple POST HTTP call.</span></span>
    * <span data-ttu-id="dfb11-154">API-specifikus paramétereket</span><span class="sxs-lookup"><span data-stu-id="dfb11-154">API-specific parameters</span></span>
* <span data-ttu-id="dfb11-155">Válasz</span><span class="sxs-lookup"><span data-stu-id="dfb11-155">Response</span></span>
  * <span data-ttu-id="dfb11-156">Állapotkód **200** -kérelem tooregister ügyfél sikeres.</span><span class="sxs-lookup"><span data-stu-id="dfb11-156">Status code **200** - Request tooregister client successful.</span></span>
  * <span data-ttu-id="dfb11-157">Állapotkód **4xx** -kérelem érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="dfb11-157">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="dfb11-158">hello ügyfél hello kérelem nem újra kell próbálkoznia.</span><span class="sxs-lookup"><span data-stu-id="dfb11-158">hello client should not retry hello request.</span></span>
  * <span data-ttu-id="dfb11-159">Állapotkód **5xx** -kérelmet egy belső kiszolgálóhiba és/vagy az átmeneti hibát eredményezett.</span><span class="sxs-lookup"><span data-stu-id="dfb11-159">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="dfb11-160">hello ügyfél hello kérelem újra kell próbálkoznia.</span><span class="sxs-lookup"><span data-stu-id="dfb11-160">hello client should retry hello request.</span></span>
* <span data-ttu-id="dfb11-161">A visszahívási</span><span class="sxs-lookup"><span data-stu-id="dfb11-161">Callback</span></span>
  * <span data-ttu-id="dfb11-162">HTTP-metódus: POST</span><span class="sxs-lookup"><span data-stu-id="dfb11-162">HTTP method: POST</span></span>
  * <span data-ttu-id="dfb11-163">A kérelem törzse: értesítési tartalom.</span><span class="sxs-lookup"><span data-stu-id="dfb11-163">Request body: Notification content.</span></span>

<span data-ttu-id="dfb11-164">a következő kódrészletet hello példája hogyan indítható el, egy leküldéses tooimplement:</span><span class="sxs-lookup"><span data-stu-id="dfb11-164">hello following code snippet is an example of how tooimplement a push trigger:</span></span>

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by hello AppService service SDK.
        // Here it defines hello input of hello push trigger is a string and hello output toohello callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register hello trigger toosome trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by hello AppService service SDK indicating hello registration is completed.
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
            // Use FileSystemWatcher toolisten toofile change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire hello push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate hello FileSystemWatcher object with hello triggerId.
            _store[triggerId] = watcher;

        }

        // Fire hello assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed tooinvoke hello callback.
            Runtime runtime,
            // hello callback tooinvoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK tooinvoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

<span data-ttu-id="dfb11-165">indítás, a lekérdezési tootest, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dfb11-165">tootest this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="dfb11-166">Hello API App hitelesítési-beállítással a telepítése **névtelen nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-166">Deploy hello API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="dfb11-167">Keresse meg a túl[http://requestb.in/](http://requestb.in/) toocreate egy RequestBin, amelyek erre a célra a visszahívási URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="dfb11-167">Browse too[http://requestb.in/](http://requestb.in/) toocreate a RequestBin which will serve as your callback URL.</span></span>
3. <span data-ttu-id="dfb11-168">Hello leküldéses eseményindító, GUID-nak hívja **eseményindító azonosítója** és RequestBin az URL-címet hello **callbackUrl**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-168">Call hello push trigger with a GUID as **triggerId** and hello RequestBin URL as **callbackUrl**.</span></span>
   <span data-ttu-id="dfb11-169">![Hívást leküldéses eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="dfb11-169">![Call Push Trigger via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span></span>
4. <span data-ttu-id="dfb11-170">Hello hívás **touch** művelet tootouch egy fájlt.</span><span class="sxs-lookup"><span data-stu-id="dfb11-170">Call hello **touch** operation tootouch a file.</span></span> <span data-ttu-id="dfb11-171">hello kép a következő egy minta kérelem keresztül Postman jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="dfb11-171">hello following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="dfb11-172">![Postman keresztül Touch művelet hívása](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="dfb11-172">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
5. <span data-ttu-id="dfb11-173">Jelölőnégyzet hello RequestBin tooconfirm leküldéses eseményindító visszahívási hello tulajdonság kimeneti hívják meg.</span><span class="sxs-lookup"><span data-stu-id="dfb11-173">Check hello RequestBin tooconfirm that hello push trigger callback is invoked with property output.</span></span>
   <span data-ttu-id="dfb11-174">![Hívás lekérdezési eseményindító Postman keresztül](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span><span class="sxs-lookup"><span data-stu-id="dfb11-174">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span></span>

### <a name="describe-triggers-in-api-definition"></a><span data-ttu-id="dfb11-175">Az API-definíció eseményindítók leírása</span><span class="sxs-lookup"><span data-stu-id="dfb11-175">Describe triggers in API definition</span></span>
<span data-ttu-id="dfb11-176">Utáni végrehajtási hello eseményindítók és központi telepítése az API-alkalmazás tooAzure, keresse meg a toohello **API-definíció** hello Azure betekintő portálon, és panel jelenik meg, hogy az eseményindítók a rendszer automatikusan felismeri a felhasználói felület, amelyek célja a hello hello Swagger 2.0 API definition hello API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="dfb11-176">After implementing hello triggers and deploying your API app tooAzure, navigate toohello **API Definition** blade in hello Azure preview portal and you'll see that triggers are automatically recognized in hello UI, which is driven by hello Swagger 2.0 API definition of hello API app.</span></span>

![API Definition panel](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

<span data-ttu-id="dfb11-178">Ha hello **letöltése Swagger** gombra, nyissa meg hello JSON-fájl, látni fogja a eredmények hasonló toohello a következő:</span><span class="sxs-lookup"><span data-stu-id="dfb11-178">If you click hello **Download Swagger** button and open hello JSON file, you'll see results similar toohello following:</span></span>

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

<span data-ttu-id="dfb11-179">bővített tulajdonság hello **x-ms-schedular-eseményindító** hogyan eseményindítók részelemcímkék ismertetését API-definíció, és ha hello kérik a tooone hello API definition hello átjárón keresztül igénylésekor automatikusan hello API app átjáró által hozzáadott a következő feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="dfb11-179">hello extension property **x-ms-schedular-trigger** is how triggers are described in API definition, and is automatically added by hello API app gateway when you request hello API definition via hello gateway if hello request tooone of hello following criteria.</span></span> <span data-ttu-id="dfb11-180">(Azt is megteheti ezt a tulajdonságot manuálisan.)</span><span class="sxs-lookup"><span data-stu-id="dfb11-180">(You can also add this property manually.)</span></span>

* <span data-ttu-id="dfb11-181">A lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="dfb11-181">Poll trigger</span></span>
  * <span data-ttu-id="dfb11-182">Ha a HTTP-metódus hello **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-182">If hello HTTP method is **GET**.</span></span>
  * <span data-ttu-id="dfb11-183">Ha hello **OperationID azonosítójú** tulajdonság hello karakterláncot tartalmaz **eseményindító**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-183">If hello **operationId** property contains hello string **trigger**.</span></span>
  * <span data-ttu-id="dfb11-184">Ha hello **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága túl**triggerState**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-184">If hello **parameters** property includes a parameter with a **name** property set too**triggerState**.</span></span>
* <span data-ttu-id="dfb11-185">Leküldéses eseményindítók</span><span class="sxs-lookup"><span data-stu-id="dfb11-185">Push trigger</span></span>
  * <span data-ttu-id="dfb11-186">Ha a HTTP-metódus hello **PUT**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-186">If hello HTTP method is **PUT**.</span></span>
  * <span data-ttu-id="dfb11-187">Ha hello **OperationID azonosítójú** tulajdonság hello karakterláncot tartalmaz **eseményindító**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-187">If hello **operationId** property contains hello string **trigger**.</span></span>
  * <span data-ttu-id="dfb11-188">Ha hello **paraméterek** tulajdonságánál paraméterrel egy **neve** tulajdonsága túl**eseményindító azonosítója**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-188">If hello **parameters** property includes a parameter with a **name** property set too**triggerId**.</span></span>

## <a name="use-api-app-triggers-in-logic-apps"></a><span data-ttu-id="dfb11-189">A Logic apps API app eseményindítók használata</span><span class="sxs-lookup"><span data-stu-id="dfb11-189">Use API app triggers in Logic apps</span></span>
### <a name="list-and-configure-api-app-triggers-in-hello-logic-apps-designer"></a><span data-ttu-id="dfb11-190">Listázhatja és konfigurálhatja az API app eseményindítók hello Logic apps-tervezőben</span><span class="sxs-lookup"><span data-stu-id="dfb11-190">List and configure API app triggers in hello Logic apps designer</span></span>
<span data-ttu-id="dfb11-191">Ha létrehoz egy logikai alkalmazás hello erőforráscsoportjában hello API-alkalmazás, képes tooadd lehet azt toohello tervezői vászonra ehhez egyszerűen kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="dfb11-191">If you create a Logic app in hello same resource group as hello API app, you will be able tooadd it toohello designer canvas simply by clicking it.</span></span> <span data-ttu-id="dfb11-192">a következő lemezképek hello ezt mutatják be:</span><span class="sxs-lookup"><span data-stu-id="dfb11-192">hello following images illustrate this:</span></span>

![A Logic App Designer eseményindítók](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![A lekérdezési eseményindító Logic App Designer konfigurálása](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![A Logic App Designer leküldéses eseményindító konfigurálása](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a><span data-ttu-id="dfb11-196">A Logic apps az API app eseményindítók optimalizálása</span><span class="sxs-lookup"><span data-stu-id="dfb11-196">Optimize API app triggers for Logic apps</span></span>
<span data-ttu-id="dfb11-197">Eseményindítók tooan API app hozzáadta, van néhány módszert megismerhet tooimprove hello élmény hello API-alkalmazás a logikai alkalmazás használatakor.</span><span class="sxs-lookup"><span data-stu-id="dfb11-197">After you add triggers tooan API app, there are a few things you can do tooimprove hello experience when using hello API app in a Logic app.</span></span>

<span data-ttu-id="dfb11-198">Például hello **triggerState** hello logikai alkalmazás kifejezés a következő toohello lekérdezéses eseményindítók paramétert kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="dfb11-198">For instance, hello **triggerState** parameter for poll triggers should be set toohello following expression in hello Logic app.</span></span> <span data-ttu-id="dfb11-199">Ebben a kifejezésben kell kiértékelni hello utolsó meghívása hello eseményindító hello Logic App, és ezt az értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="dfb11-199">This expression should evaluate hello last invocation of hello trigger from hello Logic app, and return that value.</span></span>  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

<span data-ttu-id="dfb11-200">Megjegyzés: A fenti hello kifejezésben használt hello funkció magyarázat, tekintse meg a toohello dokumentációját a [Logic App Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span><span class="sxs-lookup"><span data-stu-id="dfb11-200">NOTE: For an explanation of hello functions used in hello expression above, refer toohello documentation on [Logic App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span></span>

<span data-ttu-id="dfb11-201">Logic app felhasználók kellene tooprovide hello kifejezés fent a hello **triggerState** paraméter hello eseményindító használata során.</span><span class="sxs-lookup"><span data-stu-id="dfb11-201">Logic app users would need tooprovide hello expression above for hello **triggerState** parameter while using hello trigger.</span></span> <span data-ttu-id="dfb11-202">Már lehetséges toohave ezt az értéket az adott néven beállítás hello Logic app Designer hello bővítmény tulajdonságon keresztül **x-ms-ütemező – ajánlás**.</span><span class="sxs-lookup"><span data-stu-id="dfb11-202">It is possible toohave this value preset by hello Logic app designer through hello extension property **x-ms-scheduler-recommendation**.</span></span>  <span data-ttu-id="dfb11-203">Hello **x-ms-láthatósági** bővített tulajdonság értéke tooa állítható be *belső* , hogy maga hello paraméter nem jelenik meg a hello Tervező.</span><span class="sxs-lookup"><span data-stu-id="dfb11-203">hello **x-ms-visibility** extension property can be set tooa value of *internal* so that hello parameter itself is not shown on hello designer.</span></span>  <span data-ttu-id="dfb11-204">a következő kódrészletet hello mutatja be, amely.</span><span class="sxs-lookup"><span data-stu-id="dfb11-204">hello following snippet illustrates that.</span></span>

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

<span data-ttu-id="dfb11-205">Leküldéses eseményindítók hello **eseményindító azonosítója** paraméter egyedi módon kell azonosítania hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dfb11-205">For push triggers, hello **triggerId** parameter must uniquely identify hello Logic app.</span></span> <span data-ttu-id="dfb11-206">Ajánlott eljárás a tulajdonságnév toohello hello munkafolyamat használatával hello kifejezés a következő tooset:</span><span class="sxs-lookup"><span data-stu-id="dfb11-206">A recommended best practice is tooset this property toohello name of hello workflow by using hello following expression:</span></span>

    @workflow().name

<span data-ttu-id="dfb11-207">Hello segítségével **x-ms-ütemező – ajánlás** és **x-ms-láthatósági** bővítmény tulajdonságai a saját API definiton hello API-alkalmazás is átadja toohello Logic app Tervező tooautomatically állítsa hello felhasználói értékeit meghatározó kifejezés.</span><span class="sxs-lookup"><span data-stu-id="dfb11-207">Using hello **x-ms-scheduler-recommendation** and **x-ms-visibility** extension properties in its API definiton, hello API app can convey toohello Logic app designer tooautomatically set this expression for hello user.</span></span>

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a><span data-ttu-id="dfb11-208">Az API attribútumdefiníciós bővítmény tulajdonságai hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfb11-208">Add extension properties in API defintion</span></span>
<span data-ttu-id="dfb11-209">További metaadat-információkat – például hello bővítmény tulajdonságai **x-ms-ütemező – ajánlás** és **x-ms-láthatósági** -hello API attribútumdefiníciós az alábbi két módszer egyikével lehet hozzáadni: statikus vagy dinamikus.</span><span class="sxs-lookup"><span data-stu-id="dfb11-209">Additional metadata information - such as hello extension properties **x-ms-scheduler-recommendation** and **x-ms-visibility** - can be added in hello API defintion in one of two ways: static or dynamic.</span></span>

<span data-ttu-id="dfb11-210">A statikus metaadatok közvetlenül szerkesztheti hello */metadata/apiDefinition.swagger.json* a projekt fájlt, és adja hozzá manuálisan hello tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="dfb11-210">For static metadata, you can directly edit hello */metadata/apiDefinition.swagger.json* file in your project and add hello properties manually.</span></span>

<span data-ttu-id="dfb11-211">Dinamikus metaadatok segítségével API-alkalmazások esetén szerkesztheti hello SwaggerConfig.cs fájl tooadd egy művelet szűrőt, amely ezen bővítmények adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="dfb11-211">For API apps using dynamic metadata, you can edit hello SwaggerConfig.cs file tooadd an operation filter which can add these extensions.</span></span>

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


<span data-ttu-id="dfb11-212">hello Ez az osztály hogyan lehet megvalósított toofacilitate hello dinamikus metaadatok forgatókönyv egy példa látható.</span><span class="sxs-lookup"><span data-stu-id="dfb11-212">hello following is an example of how this class can be implemented toofacilitate hello dynamic metadata scenario.</span></span>

    // Add extension properties on hello triggerState parameter
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
                    // x-ms-visibility: set too'internal' toosignify this is an internal field
                    // x-ms-scheduler-recommendation: set tooa value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
