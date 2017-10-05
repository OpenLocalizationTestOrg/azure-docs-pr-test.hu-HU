---
title: "Azure Webhookok segítségével figyelheti a feladat értesítések Media Services .NET |} Microsoft Docs"
description: "Útmutató: Azure Webhookok a figyelheti a Media Services feladat értesítések. A példakód C# nyelven írt, és a .NET-keretrendszerhez készült Media Services SDK használja."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a61fe157-81b1-45c1-89f2-224b7ef55869
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/06/2017
ms.author: juliako
ms.openlocfilehash: eaa875a7c78de0b69c81514ea023f9b8bceb2656
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-webhooks-to-monitor-media-services-job-notifications-with-net"></a><span data-ttu-id="535ee-104">Használata Azure Webhookok feladat értesítések Media Services .NET figyelése</span><span class="sxs-lookup"><span data-stu-id="535ee-104">Use Azure WebHooks to monitor Media Services job notifications with .NET</span></span>
<span data-ttu-id="535ee-105">Feladatok futtatása, ha gyakran kell segítségével nyomon követheti a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="535ee-105">When you run jobs, you often require a way to track job progress.</span></span> <span data-ttu-id="535ee-106">Azure Webhookok használatával figyelheti a Media Services feladat értesítések vagy [Azure Queue storage](media-services-dotnet-check-job-progress-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="535ee-106">You can monitor Media Services job notifications by using Azure Webhooks or [Azure Queue storage](media-services-dotnet-check-job-progress-with-queues.md).</span></span> <span data-ttu-id="535ee-107">Ez a témakör bemutatja, hogyan Webhookok együttműködni.</span><span class="sxs-lookup"><span data-stu-id="535ee-107">This topic shows how to work with Webhooks.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="535ee-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="535ee-108">Prerequisites</span></span>

<span data-ttu-id="535ee-109">Az ismertetett eljárás végrehajtásához a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="535ee-109">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="535ee-110">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="535ee-110">An Azure account.</span></span> <span data-ttu-id="535ee-111">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="535ee-111">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="535ee-112">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="535ee-112">A Media Services account.</span></span> <span data-ttu-id="535ee-113">A Media Services-fiók létrehozásáról a [Media Services-fiók létrehozása](media-services-portal-create-account.md) című cikk nyújt tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="535ee-113">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="535ee-114">Megértését [használata az Azure functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="535ee-114">Understanding of [how to use Azure functions](../azure-functions/functions-overview.md).</span></span> <span data-ttu-id="535ee-115">Ezenkívül tekintse át a [Azure functions HTTP és a webhook kötések](../azure-functions/functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="535ee-115">Also, review [Azure functions HTTP and webhook bindings](../azure-functions/functions-bindings-http-webhook.md).</span></span>

<span data-ttu-id="535ee-116">Ez a témakör bemutatja, hogyan</span><span class="sxs-lookup"><span data-stu-id="535ee-116">This topic shows how to</span></span>

*  <span data-ttu-id="535ee-117">Adja meg az Azure-függvény, testre szabott webhookok válaszolni.</span><span class="sxs-lookup"><span data-stu-id="535ee-117">Define an Azure Function that is customized to respond to webhooks.</span></span> 
    
    <span data-ttu-id="535ee-118">Ebben az esetben a webhook váltja ki, a Media Services a kódolási feladat állapotának megváltozásakor.</span><span class="sxs-lookup"><span data-stu-id="535ee-118">In this case, the webhook is triggered by Media Services when your encoding job changes status.</span></span> <span data-ttu-id="535ee-119">A függvény a webhook hívás vissza a Media Services értesítések figyeli, és a kimeneti adategységen tesz közzé, ha a feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="535ee-119">The function listens for the webhook call back from Media Services notifications and publishes the output asset once the job finishes.</span></span> 
    
    >[!NOTE]
    ><span data-ttu-id="535ee-120">A folytatás előtt győződjön meg arról, hogy hogyan [Azure Functions HTTP és a webhook kötések](../azure-functions/functions-bindings-http-webhook.md) működik.</span><span class="sxs-lookup"><span data-stu-id="535ee-120">Before continuing, make sure you understand how [Azure Functions HTTP and webhook bindings](../azure-functions/functions-bindings-http-webhook.md) work.</span></span>
    >
    
* <span data-ttu-id="535ee-121">A webhook hozzáadása a kódolási feladat, és adja meg a webhook URL-CÍMÉT és a titkos kulcsot, amely válaszol a webhook.</span><span class="sxs-lookup"><span data-stu-id="535ee-121">Add a webhook to your encoding task and specify the webhook URL and secret key that this webhook responds to.</span></span> <span data-ttu-id="535ee-122">Az itt bemutatott példában a kódot, amely a kódolási feladat létrehoz egy konzolalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="535ee-122">In the example shown here, the code that creates the encoding task is a console app.</span></span>

## <a name="setting-up-webhook-notification-azure-functions"></a><span data-ttu-id="535ee-123">Az Azure functions "webhook értesítési" beállítása</span><span class="sxs-lookup"><span data-stu-id="535ee-123">Setting up "webhook notification" Azure functions</span></span>

<span data-ttu-id="535ee-124">Ebben a szakaszban a kódot, amely egy webhook Azure funkcióinak megvalósítását mutatja.</span><span class="sxs-lookup"><span data-stu-id="535ee-124">The code in this section shows an implementation of an Azure function that is a webhook.</span></span> <span data-ttu-id="535ee-125">Ez a példa a függvény a webhook hívásához vissza a Media Services értesítések figyeli, és a kimeneti adategységen tesz közzé, ha a feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="535ee-125">In this sample, the function listens for the webhook call back from Media Services notifications and publishes the output asset once the job finishes.</span></span>

<span data-ttu-id="535ee-126">A webhook egy aláírási kulcs (hitelesítő adatok) adja meg az értesítési végpont konfigurálásakor egyeznie vár.</span><span class="sxs-lookup"><span data-stu-id="535ee-126">The webhook expects a signing key (credential) to match the one you pass when you configure the notification endpoint.</span></span> <span data-ttu-id="535ee-127">Az aláírási kulcs értéke 64 bájtos Base64-kódolású védelemmel való ellátásához és az Azure Media Services a Webhookok visszahívások biztonságos használt.</span><span class="sxs-lookup"><span data-stu-id="535ee-127">The signing key is the 64-byte Base64 encoded value that is used to protect and secure your WebHooks callbacks from Azure Media Services.</span></span> 

<span data-ttu-id="535ee-128">A következő kódrészlet a **VerifyWebHookRequestSignature** metódus végzi az ellenőrzés az értesítési üzenet.</span><span class="sxs-lookup"><span data-stu-id="535ee-128">In the following code, the **VerifyWebHookRequestSignature** method does the verification on the notification message.</span></span> <span data-ttu-id="535ee-129">Az ellenőrzés célja annak biztosítására, hogy az üzenet Azure Media Services által küldött, és nem módosítják.</span><span class="sxs-lookup"><span data-stu-id="535ee-129">The purpose of this validation is to ensure that the message was sent by Azure Media Services and hasn’t been tampered with.</span></span> <span data-ttu-id="535ee-130">Az aláírás nem kötelező megadni az Azure functions, mert már van a **kód** érték, mint egy lekérdezési paraméter Transport Layer Security (TLS) keresztül.</span><span class="sxs-lookup"><span data-stu-id="535ee-130">The signature is optional for Azure functions as it has the **Code** value as a query parameter over Transport Layer Security (TLS).</span></span> 

<span data-ttu-id="535ee-131">Különböző Media Services .NET Azure funkcióinak (beleértve az ebben a témakörben látható) meghatározása található [Itt](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="535ee-131">You can find the definition of various Media Services .NET Azure functions (including the one shown in this topic) [here](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span>

<span data-ttu-id="535ee-132">Az alábbi kód átjáróra a listában látható Azure függvényparamétereket és a társított Azure függvény három fájlok meghatározásai: function.json project.json és run.csx.</span><span class="sxs-lookup"><span data-stu-id="535ee-132">The following code listing shows the definitions of Azure function parameters and three files that are associated with the Azure function: function.json, project.json, and run.csx.</span></span>

### <a name="application-settings"></a><span data-ttu-id="535ee-133">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="535ee-133">Application settings</span></span> 

<span data-ttu-id="535ee-134">Az alábbi táblázat az ebben a szakaszban az Azure függvényt által használt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="535ee-134">The following table shows the parameters that are used by the Azure function defined in this section.</span></span> 

|<span data-ttu-id="535ee-135">Név</span><span class="sxs-lookup"><span data-stu-id="535ee-135">Name</span></span>|<span data-ttu-id="535ee-136">Meghatározás</span><span class="sxs-lookup"><span data-stu-id="535ee-136">Definition</span></span>|<span data-ttu-id="535ee-137">Példa</span><span class="sxs-lookup"><span data-stu-id="535ee-137">Example</span></span>| 
|---|---|---|
|<span data-ttu-id="535ee-138">AMSAccount</span><span class="sxs-lookup"><span data-stu-id="535ee-138">AMSAccount</span></span>|<span data-ttu-id="535ee-139">Az AMS-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="535ee-139">Your AMS account name.</span></span> |<span data-ttu-id="535ee-140">juliakomediaservices</span><span class="sxs-lookup"><span data-stu-id="535ee-140">juliakomediaservices</span></span>|
|<span data-ttu-id="535ee-141">AMSKey</span><span class="sxs-lookup"><span data-stu-id="535ee-141">AMSKey</span></span> |<span data-ttu-id="535ee-142">Az AMS-fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="535ee-142">Your AMS account key.</span></span> | <span data-ttu-id="535ee-143">JUWJdDaOHQQqsZeiXZuE76eDt2SO + YMJk25Lghgy2nY =</span><span class="sxs-lookup"><span data-stu-id="535ee-143">JUWJdDaOHQQqsZeiXZuE76eDt2SO+YMJk25Lghgy2nY=</span></span>|
|<span data-ttu-id="535ee-144">MediaServicesStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="535ee-144">MediaServicesStorageAccountName</span></span> |<span data-ttu-id="535ee-145">A tárfiók társított az AMS-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="535ee-145">A name of the storage account that is associated with your AMS account.</span></span>| <span data-ttu-id="535ee-146">storagepkeewmg5c3peq</span><span class="sxs-lookup"><span data-stu-id="535ee-146">storagepkeewmg5c3peq</span></span>|
|<span data-ttu-id="535ee-147">MediaServicesStorageAccountKey</span><span class="sxs-lookup"><span data-stu-id="535ee-147">MediaServicesStorageAccountKey</span></span> |<span data-ttu-id="535ee-148">A tárfiók az AMS-fiók társított kulcs.</span><span class="sxs-lookup"><span data-stu-id="535ee-148">A key of the storage account that is associated with your AMS account.</span></span>|
|<span data-ttu-id="535ee-149">SigningKey</span><span class="sxs-lookup"><span data-stu-id="535ee-149">SigningKey</span></span> |<span data-ttu-id="535ee-150">Az aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="535ee-150">A signing key.</span></span>| <span data-ttu-id="535ee-151">j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt</span><span class="sxs-lookup"><span data-stu-id="535ee-151">j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt</span></span>|
|<span data-ttu-id="535ee-152">WebHookEndpoint</span><span class="sxs-lookup"><span data-stu-id="535ee-152">WebHookEndpoint</span></span> | <span data-ttu-id="535ee-153">A webhook végpont címe.</span><span class="sxs-lookup"><span data-stu-id="535ee-153">A webhook endpoint address.</span></span> | <span data-ttu-id="535ee-154">https://juliakofuncapp.azurewebsites.NET/API/Notification_Webhook_Function?Code=iN2phdrTnCxmvaKExFWOTulfnm4C71mMLIy8tzLr7Zvf6Z22HHIK5g==.</span><span class="sxs-lookup"><span data-stu-id="535ee-154">https://juliakofuncapp.azurewebsites.net/api/Notification_Webhook_Function?code=iN2phdrTnCxmvaKExFWOTulfnm4C71mMLIy8tzLr7Zvf6Z22HHIK5g==.</span></span>|

### <a name="functionjson"></a><span data-ttu-id="535ee-155">Function.JSON</span><span class="sxs-lookup"><span data-stu-id="535ee-155">function.json</span></span>

<span data-ttu-id="535ee-156">A function.json fájl határozza meg, a függvénykötés és egyéb konfigurációs beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="535ee-156">The function.json file defines the function bindings and other configuration settings.</span></span> <span data-ttu-id="535ee-157">A futtatókörnyezet ezt a fájlt határozza meg a figyelendő események és adja át az adatokat, és vissza adatokat, a függvény végrehajtása használja.</span><span class="sxs-lookup"><span data-stu-id="535ee-157">The runtime uses this file to determine the events to monitor and how to pass data into and return data from function execution.</span></span> 

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [
        "post",
        "get",
        "put",
        "update",
        "patch"
          ]
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }
    
### <a name="projectjson"></a><span data-ttu-id="535ee-158">Project.JSON</span><span class="sxs-lookup"><span data-stu-id="535ee-158">project.json</span></span>

<span data-ttu-id="535ee-159">A project.json függőségeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="535ee-159">The project.json file contains dependencies.</span></span> 

    {
      "frameworks": {
        "net46":{
          "dependencies": {
        "windowsazure.mediaservices": "3.8.0.5",
        "windowsazure.mediaservices.extensions": "3.8.0.3"
          }
        }
       }
    }
    
### <a name="runcsx"></a><span data-ttu-id="535ee-160">Run.csx</span><span class="sxs-lookup"><span data-stu-id="535ee-160">run.csx</span></span>

<span data-ttu-id="535ee-161">A következő C# kód, amely egy webhook Azure funkcióinak definícióját szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="535ee-161">The following C# code shows a definition of an Azure function that is a webhook.</span></span> <span data-ttu-id="535ee-162">A függvény a webhook hívás vissza a Media Services értesítések figyeli, és a kimeneti adategységen tesz közzé, ha a feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="535ee-162">The function listens for the webhook call back from Media Services notifications and publishes the output asset once the job finishes.</span></span> 


>[!NOTE]
><span data-ttu-id="535ee-163">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="535ee-163">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="535ee-164">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="535ee-164">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="535ee-165">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="535ee-165">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

    ///////////////////////////////////////////////////
    #r "Newtonsoft.Json"

    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using System.Globalization;
    using Newtonsoft.Json;
    using Microsoft.Azure;
    using System.Net;
    using System.Security.Cryptography;

    internal const string SignatureHeaderKey = "sha256";
    internal const string SignatureHeaderValueTemplate = SignatureHeaderKey + "={0}";
    static string _webHookEndpoint = Environment.GetEnvironmentVariable("WebHookEndpoint");
    static string _signingKey = Environment.GetEnvironmentVariable("SigningKey");
    static string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    static string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

    static CloudMediaContext _context = null;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

        Task<byte[]> taskForRequestBody = req.Content.ReadAsByteArrayAsync();
        byte[] requestBody = await taskForRequestBody;

        string jsonContent = await req.Content.ReadAsStringAsync();
        log.Info($"Request Body = {jsonContent}");

        IEnumerable<string> values = null;
        if (req.Headers.TryGetValues("ms-signature", out values))
        {
        byte[] signingKey = Convert.FromBase64String(_signingKey);
        string signatureFromHeader = values.FirstOrDefault();

        if (VerifyWebHookRequestSignature(requestBody, signatureFromHeader, signingKey))
        {
            string requestMessageContents = Encoding.UTF8.GetString(requestBody);

            NotificationMessage msg = JsonConvert.DeserializeObject<NotificationMessage>(requestMessageContents);

            if (VerifyHeaders(req, msg, log))
            { 
            string newJobStateStr = (string)msg.Properties.Where(j => j.Key == "NewState").FirstOrDefault().Value;
            if (newJobStateStr == "Finished")
            {
                _context = new CloudMediaContext(new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey));

                if(_context!=null)   
                {                        
                string urlForClientStreaming = PublishAndBuildStreamingURLs(msg.Properties["JobId"]);
                log.Info($"URL to the manifest for client streaming using HLS protocol: {urlForClientStreaming}");
                }
            }

            return req.CreateResponse(HttpStatusCode.OK, string.Empty);
            }
            else
            {
            log.Info($"VerifyHeaders failed.");
            return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyHeaders failed.");
            }
        }
        else
        {
            log.Info($"VerifyWebHookRequestSignature failed.");
            return req.CreateResponse(HttpStatusCode.BadRequest, "VerifyWebHookRequestSignature failed.");
        }
        }

        return req.CreateResponse(HttpStatusCode.BadRequest, "Generic Error.");
    }

    private static string PublishAndBuildStreamingURLs(String jobID)
    {
        IJob job = _context.Jobs.Where(j => j.Id == jobID).FirstOrDefault();
        IAsset asset = job.OutputMediaAssets.FirstOrDefault();

        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
        TimeSpan.FromDays(30),
        AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy,
        DateTime.UtcNow.AddMinutes(-5));


        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                    EndsWith(".ism")).
                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest" +  "(format=m3u8-aapl)";
        return urlForClientStreaming;

    }

    private static bool VerifyWebHookRequestSignature(byte[] data, string actualValue, byte[] verificationKey)
    {
        using (var hasher = new HMACSHA256(verificationKey))
        {
        byte[] sha256 = hasher.ComputeHash(data);
        string expectedValue = string.Format(CultureInfo.InvariantCulture, SignatureHeaderValueTemplate, ToHex(sha256));

        return (0 == String.Compare(actualValue, expectedValue, System.StringComparison.Ordinal));
        }
    }

    private static bool VerifyHeaders(HttpRequestMessage req, NotificationMessage msg, TraceWriter log)
    {
        bool headersVerified = false;

        try
        {
        IEnumerable<string> values = null;
        if (req.Headers.TryGetValues("ms-mediaservices-accountid", out values))
        {
            string accountIdHeader = values.FirstOrDefault();
            string accountIdFromMessage = msg.Properties["AccountId"];

            if (0 == string.Compare(accountIdHeader, accountIdFromMessage, StringComparison.OrdinalIgnoreCase))
            {
            headersVerified = true;
            }
            else
            {
            log.Info($"accountIdHeader={accountIdHeader} does not match accountIdFromMessage={accountIdFromMessage}");
            }
        }
        else
        {
            log.Info($"Header ms-mediaservices-accountid not found.");
        }
        }
        catch (Exception e)
        {
        log.Info($"VerifyHeaders hit exception {e}");
        headersVerified = false;
        }

        return headersVerified;
    }

    private static readonly char[] HexLookup = new char[] { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F' };

    /// <summary>
    /// Converts a <see cref="T:byte[]"/> to a hex-encoded string.
    /// </summary>
    private static string ToHex(byte[] data)
    {
        if (data == null)
        {
        return string.Empty;
        }

        char[] content = new char[data.Length * 2];
        int output = 0;
        byte d;
        for (int input = 0; input < data.Length; input++)
        {
        d = data[input];
        content[output++] = HexLookup[d / 0x10];
        content[output++] = HexLookup[d % 0x10];
        }
        return new string(content);
    }

    internal enum NotificationEventType
    {
        None = 0,
        JobStateChange = 1,
        NotificationEndPointRegistration = 2,
        NotificationEndPointUnregistration = 3,
        TaskStateChange = 4,
        TaskProgress = 5
    }
    
    internal sealed class NotificationMessage
    {
        public string MessageVersion { get; set; }
        public string ETag { get; set; }
        public NotificationEventType EventType { get; set; }
        public DateTime TimeStamp { get; set; }
        public IDictionary<string, string> Properties { get; set; }
    }

### <a name="function-output"></a><span data-ttu-id="535ee-166">Függvény kimeneti</span><span class="sxs-lookup"><span data-stu-id="535ee-166">Function output</span></span>

<span data-ttu-id="535ee-167">A fenti példában a következő kimeneti, ami az értékek eltérőek.</span><span class="sxs-lookup"><span data-stu-id="535ee-167">The example above produced the following output, your values will vary.</span></span>

    C# HTTP trigger function processed a request. RequestUri=https://juliako001-functions.azurewebsites.net/api/Notification_Webhook_Function?code=9376d69kygoy49oft81nel8frty5cme8hb9xsjslxjhalwhfrqd79awz8ic4ieku74dvkdfgvi
    Request Body = {
      "MessageVersion": "1.1",
      "ETag": "b8977308f48858a8f224708bc963e1a09ff917ce730316b4e7ae9137f78f3b20",
      "EventType": 4,
      "TimeStamp": "2017-02-16T03:59:53.3041122Z",
      "Properties": {
        "JobId": "nb:jid:UUID:badd996c-8d7c-4ae0-9bc1-bd7f1902dbdd",
        "TaskId": "nb:tid:UUID:80e26fb9-ee04-4739-abd8-2555dc24639f",
        "NewState": "Finished",
        "OldState": "Processing",
        "AccountName": "mediapkeewmg5c3peq",
        "AccountId": "301912b0-659e-47e0-9bc4-6973f2be3424",
        "NotificationEndPointId": "nb:nepid:UUID:cb5d707b-4db8-45fe-a558-19f8d3306093"
      }
    }
    
    URL to the manifest for client streaming using HLS protocol: http://mediapkeewmg5c3peq.streaming.mediaservices.windows.net/0ac98077-2b58-4db7-a8da-789a13ac6167/BigBuckBunny.ism/manifest(format=m3u8-aapl)

## <a name="adding-webhook-to-your-encoding-task"></a><span data-ttu-id="535ee-168">A kódolási feladat Webhook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="535ee-168">Adding Webhook to your encoding task</span></span>

<span data-ttu-id="535ee-169">Ebben a szakaszban a kódot, amely egy feladatot ad hozzá egy webhook értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="535ee-169">In this section, the code that adds a webhook notification to a Task is shown.</span></span> <span data-ttu-id="535ee-170">Egy feladat szintű értesítések, amelyek több számára hasznos lehet egy feladat láncolt feladatok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="535ee-170">You can also add a job level notification, which would be more useful for a job with chained tasks.</span></span>  

1. <span data-ttu-id="535ee-171">A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="535ee-171">Create a new C# Console Application in Visual Studio.</span></span> <span data-ttu-id="535ee-172">Adja meg a nevét, helyét és a megoldás nevét, és kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="535ee-172">Enter the Name, Location, and Solution name, and then click OK.</span></span>
2. <span data-ttu-id="535ee-173">Használjon [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) Azure Media Services telepítése.</span><span class="sxs-lookup"><span data-stu-id="535ee-173">Use [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) to install Azure Media Services.</span></span>
3. <span data-ttu-id="535ee-174">Frissítse az App.config fájl megfelelő értékekkel:</span><span class="sxs-lookup"><span data-stu-id="535ee-174">Update App.config file with appropriate values:</span></span> 
    
    * <span data-ttu-id="535ee-175">Az Azure Media Services nevét és a kulcsot, értesítések, küldi</span><span class="sxs-lookup"><span data-stu-id="535ee-175">Azure Media Services name and key that will be sending notifications,</span></span> 
    * <span data-ttu-id="535ee-176">webhook URL-CÍMÉT, amely a értesítésekre vár</span><span class="sxs-lookup"><span data-stu-id="535ee-176">webhook URL that expects to get the notifications,</span></span> 
    * <span data-ttu-id="535ee-177">az aláírási kulcs, amely megfelel a kulcsot, a webhook vár.</span><span class="sxs-lookup"><span data-stu-id="535ee-177">the signing key that matches the key that your webhook expects.</span></span> <span data-ttu-id="535ee-178">Az aláírási kulcs értéke 64 bájtos Base64-kódolású védelemmel való ellátásához és az Azure Media Services a Webhookok visszahívások biztonságos használt.</span><span class="sxs-lookup"><span data-stu-id="535ee-178">The signing key is the 64-byte Base64 encoded value that is used to protect and secure your WebHooks callbacks from Azure Media Services.</span></span> 

            <appSettings>
              <add key="MediaServicesAccountName" value="AMSAcctName" />
              <add key="MediaServicesAccountKey" value="AMSAcctKey" />
              <add key="WebhookURL" value="https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>" />
              <add key="WebhookSigningKey" value="j0txf1f8msjytzvpe40nxbpxdcxtqcgxy0nt" />
            </appSettings>
            
4. <span data-ttu-id="535ee-179">A Program.cs fájlban frissítse az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="535ee-179">Update your Program.cs file with the following code:</span></span>

        using System;
        using System.Configuration;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace NotificationWebHook
        {
            class Program
            {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
            private static readonly string _webHookEndpoint =
                ConfigurationManager.AppSettings["WebhookURL"];
            private static readonly string _signingKey =
                 ConfigurationManager.AppSettings["WebhookSigningKey"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {

                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(new MediaServicesCredentials(
                        _mediaServicesAccountName,
                        _mediaServicesAccountKey));

                byte[] keyBytes = Convert.FromBase64String(_signingKey);

                IAsset newAsset = _context.Assets.FirstOrDefault();

                // Check for existing Notification Endpoint with the name "FunctionWebHook"

                var existingEndpoint = _context.NotificationEndPoints.Where(e => e.Name == "FunctionWebHook").FirstOrDefault();
                INotificationEndPoint endpoint = null;

                if (existingEndpoint != null)
                {
                Console.WriteLine("webhook endpoint already exists");
                endpoint = (INotificationEndPoint)existingEndpoint;
                }
                else
                {
                endpoint = _context.NotificationEndPoints.Create("FunctionWebHook",
                    NotificationEndPointType.WebHook, _webHookEndpoint, keyBytes);
                Console.WriteLine("Notification Endpoint Created with Key : {0}", keyBytes.ToString());
                }

                // Declare a new encoding job with the Standard encoder
                IJob job = _context.Jobs.Create("MES Job");

                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "Adaptive Streaming",
                TaskOptions.None);

                // Specify the input asset to be encoded.
                task.InputAssets.Add(newAsset);

                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew(newAsset.Name, AssetCreationOptions.None);

                // Add the WebHook notification to this Task and request all notification state changes.
                // Note that you can also add a job level notification
                // which would be more useful for a job with chained tasks.  
                if (endpoint != null)
                {
                task.TaskNotificationSubscriptions.AddNew(NotificationJobState.All, endpoint, true);
                Console.WriteLine("Created Notification Subscription for endpoint: {0}", _webHookEndpoint);
                }
                else
                {
                Console.WriteLine("No Notification Endpoint is being used");
                }

                job.Submit();

                Console.WriteLine("Expect WebHook to be triggered for the Job ID: {0}", job.Id);
                Console.WriteLine("Expect WebHook to be triggered for the Task ID: {0}", task.Id);

                Console.WriteLine("Job Submitted");

            }
            private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

                if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

                return processor;
            }

            }
        }

## <a name="next-step"></a><span data-ttu-id="535ee-180">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="535ee-180">Next step</span></span>
<span data-ttu-id="535ee-181">Tekintse át a Media Services tanulási útvonalai</span><span class="sxs-lookup"><span data-stu-id="535ee-181">Review Media Services learning paths</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="535ee-182">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="535ee-182">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
