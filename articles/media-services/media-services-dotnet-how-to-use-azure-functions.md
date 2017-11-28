---
title: az Azure Functions a Media Services aaaDevelop
description: "Ez a témakör bemutatja, hogyan fejlesztése az Azure Functions a Media Services használatával toostart hello Azure-portálon."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2017
ms.author: juliako
ms.openlocfilehash: 3b2c2fb498fea399c862dfbdb63033d06cabf6d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#<a name="develop-azure-functions-with-media-services"></a><span data-ttu-id="c8026-103">A Media Services az Azure Functions fejlesztése</span><span class="sxs-lookup"><span data-stu-id="c8026-103">Develop Azure Functions with Media Services</span></span>

<span data-ttu-id="c8026-104">Ez a témakör bemutatja, hogyan tooget létrehozása az Azure Functions, használja a Media Services használatába.</span><span class="sxs-lookup"><span data-stu-id="c8026-104">This topic shows you how tooget started with creating Azure Functions that use Media Services.</span></span> <span data-ttu-id="c8026-105">Ebben a témakörben meghatározott Azure-függvény hello figyeli nevű fiók tárolót **bemeneti** új MP4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="c8026-105">hello Azure Function defined in this topic monitors a storage account container named **input** for new MP4 files.</span></span> <span data-ttu-id="c8026-106">Ha egy fájl hello storage tárolóba megszakad, hello blob eseményindító hello függvény fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="c8026-106">Once a file is dropped into hello storage container, hello blob trigger will execute hello function.</span></span>

<span data-ttu-id="c8026-107">Ha szeretné, hogy tooexplore és központi telepítése a meglévő Azure Functions, amely az Azure Media Services, tekintse meg [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="c8026-107">If you want tooexplore and deploy existing Azure Functions that use Azure Media Services, check out [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span> <span data-ttu-id="c8026-108">Ebben a tárházban található példák bemutatják, a Media Services tooshow munkafolyamatok kapcsolódó tooingesting tartalom közvetlenül a blob-tároló, kódolási és tartalmat ír biztonsági tooblob tárolási használó.</span><span class="sxs-lookup"><span data-stu-id="c8026-108">This repository contains examples that use Media Services tooshow workflows related tooingesting content directly from blob storage, encoding, and writing content back tooblob storage.</span></span> <span data-ttu-id="c8026-109">Hogyan toomonitor feladat Webhookok és Azure várólisták értesítések példákat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c8026-109">It also includes examples of how toomonitor job notifications via WebHooks and Azure Queues.</span></span> <span data-ttu-id="c8026-110">A funkciók hello hello példák alapján is készíthet [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) tárházba.</span><span class="sxs-lookup"><span data-stu-id="c8026-110">You can also develop your Functions based on hello examples in hello [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) repository.</span></span> <span data-ttu-id="c8026-111">toodeploy hello funkciók, majd nyomja le az hello **tooAzure telepítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8026-111">toodeploy hello functions, press hello **Deploy tooAzure** button.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8026-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8026-112">Prerequisites</span></span>

- <span data-ttu-id="c8026-113">Mielőtt létrehozhatná az első függvényét, egy aktív Azure-fiók toohave kell.</span><span class="sxs-lookup"><span data-stu-id="c8026-113">Before you can create your first function, you need toohave an active Azure account.</span></span> <span data-ttu-id="c8026-114">Ha még nem rendelkezik Azure-fiókkal, [létrehozhat egy ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="c8026-114">If you don't already have an Azure account, [free accounts are available](https://azure.microsoft.com/free/).</span></span>
- <span data-ttu-id="c8026-115">Ha toocreate Azure Functions műveleteket az Azure Media Services (AMS) fiók vagy a Media Services által küldött tooevents figyelni kívánja, akkor hozzon létre az AMS-fiók leírtak [Itt](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="c8026-115">If you are going toocreate Azure Functions that perform actions on your Azure Media Services (AMS) account or listen tooevents sent by Media Services, you should create an AMS account, as described [here](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="c8026-116">Megértését [hogyan toouse Azure functions](../azure-functions/functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c8026-116">Understanding of [how toouse Azure functions](../azure-functions/functions-overview.md).</span></span> <span data-ttu-id="c8026-117">Emellett tekintse át:</span><span class="sxs-lookup"><span data-stu-id="c8026-117">Also, review:</span></span>
    - [<span data-ttu-id="c8026-118">Az Azure functions HTTP és a webhook kötések</span><span class="sxs-lookup"><span data-stu-id="c8026-118">Azure functions HTTP and webhook bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
    - [<span data-ttu-id="c8026-119">Hogyan tooconfigure Azure-függvény Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="c8026-119">How tooconfigure Azure Function app settings</span></span>](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a><span data-ttu-id="c8026-120">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="c8026-120">Considerations</span></span>

-  <span data-ttu-id="c8026-121">Az Azure Functions hello fogyasztás terv alatt futó rendelkezik 5 perces időkorlát korlátozza.</span><span class="sxs-lookup"><span data-stu-id="c8026-121">Azure Functions running under hello Consumption plan have 5 minutes timeout limit.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="c8026-122">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8026-122">Create a function app</span></span>

1. <span data-ttu-id="c8026-123">Nyissa meg toohello [Azure-portálon](http://portal.azure.com) , és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c8026-123">Go toohello [Azure portal](http://portal.azure.com) and sign-in with your Azure account.</span></span>
2. <span data-ttu-id="c8026-124">Függvény-alkalmazás létrehozása leírtak [Itt](../azure-functions/functions-create-function-app-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c8026-124">Create a function app as described [here](../azure-functions/functions-create-function-app-portal.md).</span></span>

>[!NOTE]
> <span data-ttu-id="c8026-125">Egy tárfiókot meg a hello **StorageConnection** környezeti változó (lásd a következő lépés hello) kell lennie a hello és az alkalmazás ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="c8026-125">A storage account that you specify in hello **StorageConnection** environment variable (see hello next step) should be in hello same region as your app.</span></span>

## <a name="configure-function-app-settings"></a><span data-ttu-id="c8026-126">Függvény-Alkalmazásbeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8026-126">Configure function app settings</span></span>

<span data-ttu-id="c8026-127">A Media Services funkciók fejlesztésekor fogja használni a teljes a funkciók hasznos tooadd környezeti változókat is.</span><span class="sxs-lookup"><span data-stu-id="c8026-127">When developing Media Services functions, it is handy tooadd environment variables that will be used throughout your functions.</span></span> <span data-ttu-id="c8026-128">Alkalmazásbeállítások tooconfigure, hivatkozásra hello App beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c8026-128">tooconfigure app settings, click hello Configure App Settings link.</span></span> <span data-ttu-id="c8026-129">További információkért lásd: [hogyan tooconfigure Azure-függvény Alkalmazásbeállítások](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span><span class="sxs-lookup"><span data-stu-id="c8026-129">For more information, see  [How tooconfigure Azure Function app settings](../azure-functions/functions-how-to-use-azure-function-app-settings.md).</span></span> 

<span data-ttu-id="c8026-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="c8026-130">For example:</span></span>

![Beállítások](./media/media-services-azure-functions/media-services-azure-functions001.png)

<span data-ttu-id="c8026-132">hello függvényt, ez a cikk feltételezi, hogy rendelkezik-e a környezeti változók a beállítások a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c8026-132">hello function, defined in this article, assumes you have hello following environment variables in your app settings:</span></span>

<span data-ttu-id="c8026-133">**AMSAccount** : *AMS-fiók neve* (pl. testams)</span><span class="sxs-lookup"><span data-stu-id="c8026-133">**AMSAccount** : *AMS account name* (e.g. testams)</span></span>

<span data-ttu-id="c8026-134">**AMSKey** : *AMS fiókkulcs* (pl. IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)</span><span class="sxs-lookup"><span data-stu-id="c8026-134">**AMSKey** : *AMS account key* (e.g. IHOySnH+XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM=)</span></span>

<span data-ttu-id="c8026-135">**MediaServicesStorageAccountName** : *tárfióknév* (pl. testamsstorage)</span><span class="sxs-lookup"><span data-stu-id="c8026-135">**MediaServicesStorageAccountName** : *storage account name* (e.g., testamsstorage)</span></span>

<span data-ttu-id="c8026-136">**MediaServicesStorageAccountKey** : *tárfiók kulcsa* (pl. xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="c8026-136">**MediaServicesStorageAccountKey** : *storage account key* (e.g., xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX==)</span></span>

<span data-ttu-id="c8026-137">**StorageConnection** : *tárolókapcsolat* (pl. megadni: DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)</span><span class="sxs-lookup"><span data-stu-id="c8026-137">**StorageConnection** : *storage connection* (e.g., DefaultEndpointsProtocol=https;AccountName=testamsstorage;AccountKey=xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX==)</span></span>

## <a name="create-a-function"></a><span data-ttu-id="c8026-138">Függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8026-138">Create a function</span></span>

<span data-ttu-id="c8026-139">A függvény az alkalmazást telepíti, ha megtalálja között **alkalmazásszolgáltatások** Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c8026-139">Once your function app is deployed, you can find it among **App Services** Azure Functions.</span></span>

1. <span data-ttu-id="c8026-140">Válassza ki a függvény alkalmazást, majd kattintson a **új függvény**.</span><span class="sxs-lookup"><span data-stu-id="c8026-140">Select your function app and click **New Function**.</span></span>
2. <span data-ttu-id="c8026-141">Válassza ki a hello **C#** nyelvi és **adatfeldolgozási** forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="c8026-141">Choose hello **C#** language and **Data Processing** scenario.</span></span>
3. <span data-ttu-id="c8026-142">Válasszon **BlobTrigger** sablont.</span><span class="sxs-lookup"><span data-stu-id="c8026-142">Choose **BlobTrigger** template.</span></span> <span data-ttu-id="c8026-143">Ez a funkció akkor is kiváltódik, amikor blob hello van töltve **bemeneti** tároló.</span><span class="sxs-lookup"><span data-stu-id="c8026-143">This function will be triggered whenever a blob is uploaded into hello **input** container.</span></span> <span data-ttu-id="c8026-144">Hello **bemeneti** neve van megadva az hello **elérési**, hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="c8026-144">hello **input** name is specified in hello **Path**, in hello next step.</span></span>

    ![Fájlok](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. <span data-ttu-id="c8026-146">Miután **BlobTrigger**, néhány további vezérlők hello lapon fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="c8026-146">Once you select **BlobTrigger**, some more controls will appear on hello page.</span></span>

    ![Fájlok](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. <span data-ttu-id="c8026-148">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8026-148">Click **Create**.</span></span> 


## <a name="files"></a><span data-ttu-id="c8026-149">Fájlok</span><span class="sxs-lookup"><span data-stu-id="c8026-149">Files</span></span>

<span data-ttu-id="c8026-150">Az Azure-függvény kódot és egyéb fájlokat ebben a szakaszban ismertetett társítva.</span><span class="sxs-lookup"><span data-stu-id="c8026-150">Your Azure function is associated with code files and other files that are described in this section.</span></span> <span data-ttu-id="c8026-151">Alapértelmezés szerint a következő függvényt társított **function.json** és **run.csx** (C#) fájlok.</span><span class="sxs-lookup"><span data-stu-id="c8026-151">By default, a function is associated with **function.json** and **run.csx** (C#) files.</span></span> <span data-ttu-id="c8026-152">Tooadd szüksége lesz egy **project.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8026-152">You will need tooadd a **project.json** file.</span></span> <span data-ttu-id="c8026-153">Ez a szakasz többi hello hello definícióit ezeket a fájlokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c8026-153">hello rest of this section shows hello definitions for these files.</span></span>

![Fájlok](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a><span data-ttu-id="c8026-155">Function.JSON</span><span class="sxs-lookup"><span data-stu-id="c8026-155">function.json</span></span>

<span data-ttu-id="c8026-156">hello function.json fájl határozza meg, hello függvénykötés és egyéb konfigurációs beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="c8026-156">hello function.json file defines hello function bindings and other configuration settings.</span></span> <span data-ttu-id="c8026-157">hello futásidejű használ, a fájl toodetermine hello események toomonitor és hogyan toopass adatok és a visszatérési adatainak függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c8026-157">hello runtime uses this file toodetermine hello events toomonitor and how toopass data into and return data from function execution.</span></span> <span data-ttu-id="c8026-158">További információkért lásd: [Azure functions HTTP és a webhook kötések](../azure-functions/functions-reference.md#function-code).</span><span class="sxs-lookup"><span data-stu-id="c8026-158">For more information, see [Azure functions HTTP and webhook bindings](../azure-functions/functions-reference.md#function-code).</span></span>

>[!NOTE]
><span data-ttu-id="c8026-159">Set hello **le van tiltva** tulajdonság túl**igaz** tooprevent hello függvény végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="c8026-159">Set hello **disabled** property too**true** tooprevent hello function from being executed.</span></span> 


<span data-ttu-id="c8026-160">Íme egy példa **function.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8026-160">Here is an example of **function.json** file.</span></span>

    {
    "bindings": [
      {
        "name": "myBlob",
        "type": "blobTrigger",
        "direction": "in",
        "path": "input/{fileName}.mp4",
        "connection": "StorageConnection"
      }
    ],
    "disabled": false
    }

### <a name="projectjson"></a><span data-ttu-id="c8026-161">Project.JSON</span><span class="sxs-lookup"><span data-stu-id="c8026-161">project.json</span></span>

<span data-ttu-id="c8026-162">hello project.json fájl függőségeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c8026-162">hello project.json file contains dependencies.</span></span> <span data-ttu-id="c8026-163">Íme egy példa **project.json** fájlt, amely tartalmazza a szükséges hello .NET Azure Media Services a Nuget-csomagok.</span><span class="sxs-lookup"><span data-stu-id="c8026-163">Here is an example of **project.json** file that includes hello required .NET Azure Media Services packages from Nuget.</span></span> <span data-ttu-id="c8026-164">Megjegyezzük, hogy hello verziószámok fogja módosítani legújabb frissítéseit toohello csomagok, így ellenőrizze hello legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="c8026-164">Note that hello version numbers will change with latest updates toohello packages, so you should confirm hello most recent versions.</span></span> 

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
    
### <a name="runcsx"></a><span data-ttu-id="c8026-165">Run.csx</span><span class="sxs-lookup"><span data-stu-id="c8026-165">run.csx</span></span>

<span data-ttu-id="c8026-166">Ez a hello C#-kódban a függvénynél.</span><span class="sxs-lookup"><span data-stu-id="c8026-166">This is hello C# code for your function.</span></span>  <span data-ttu-id="c8026-167">figyelők az alábbiakban meghatározott hello függvény nevű fiók tárolót **bemeneti** (Ez mi volt megadva a hello elérési utat) az új MP4-fájlok.</span><span class="sxs-lookup"><span data-stu-id="c8026-167">hello function defined below monitors a storage account container named **input** (that is what was specified in hello path) for new MP4 files.</span></span> <span data-ttu-id="c8026-168">Ha egy fájl hello storage tárolóba megszakad, hello blob eseményindító hello függvény fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="c8026-168">Once a file is dropped into hello storage container, hello blob trigger will execute hello function.</span></span>
    
<span data-ttu-id="c8026-169">Ebben a szakaszban meghatározott hello példa bemutatja, hogyan</span><span class="sxs-lookup"><span data-stu-id="c8026-169">hello example defined in this section demonstrates</span></span> 

1. <span data-ttu-id="c8026-170">Hogyan tooingest egy eszköz, a Media Services a fiók (által másolás blob egy AMS adategységbe) és</span><span class="sxs-lookup"><span data-stu-id="c8026-170">how tooingest an asset into a Media Services account (by coping a blob into an AMS asset) and</span></span> 
2. <span data-ttu-id="c8026-171">Hogyan toosubmit egy kódolási feladat által használt Media Encoder Standard tartozó "adaptív Streameléshez" előre.</span><span class="sxs-lookup"><span data-stu-id="c8026-171">how toosubmit an encoding job that uses Media Encoder Standard's "Adaptive Streaming" preset .</span></span>

<span data-ttu-id="c8026-172">Hello valós esetben valószínűleg szeretné, hogy tootrack feladatok előrehaladásának és tegye közzé a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="c8026-172">In hello real life scenario, you most likely want tootrack job progress and then publish your encoded asset.</span></span> <span data-ttu-id="c8026-173">További információkért lásd: [használata Azure Webhookok toomonitor Media Services feladat értesítések](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="c8026-173">For more information, see [Use Azure WebHooks toomonitor Media Services job notifications](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> <span data-ttu-id="c8026-174">További példákért lásd [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span><span class="sxs-lookup"><span data-stu-id="c8026-174">For more examples, see [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).</span></span>  

<span data-ttu-id="c8026-175">Ha elkészült a függvényt definiáló kattintson **mentéséhez, és futtassa**.</span><span class="sxs-lookup"><span data-stu-id="c8026-175">Once you are done defining your function click **Save and Run**.</span></span>

    #r "Microsoft.WindowsAzure.Storage"
    #r "Newtonsoft.Json"
    #r "System.Web"

    using System;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Net;
    using System.Threading;
    using System.Threading.Tasks;
    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Auth;

    private static readonly string _mediaServicesAccountName = Environment.GetEnvironmentVariable("AMSAccount");
    private static readonly string _mediaServicesAccountKey = Environment.GetEnvironmentVariable("AMSKey");

    static string _storageAccountName = Environment.GetEnvironmentVariable("MediaServicesStorageAccountName");
    static string _storageAccountKey = Environment.GetEnvironmentVariable("MediaServicesStorageAccountKey");

    private static CloudStorageAccount _destinationStorageAccount = null;

    // Field for service context.
    private static CloudMediaContext _context = null;
    private static MediaServicesCredentials _cachedCredentials = null;

    public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
    {
        // NOTE that hello variables {fileName} here come from hello path setting in function.json
        // and are passed into hello  Run method signature above. We can use this toomake decisions on what type of file
        // was dropped into hello input container for hello function. 

        // No need toodo any Retry strategy in this function, By default, hello SDK calls a function up too5 times for a 
        // given blob. If hello fifth try fails, hello SDK adds a message tooa queue named webjobs-blobtrigger-poison.

        log.Info($"C# Blob trigger function processed: {fileName}.mp4");
        log.Info($"Using Azure Media Services account : {_mediaServicesAccountName}");


        try
        {
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(
                _mediaServicesAccountName,
                _mediaServicesAccountKey);

        // Used hello chached credentials toocreate CloudMediaContext.
        _context = new CloudMediaContext(_cachedCredentials);

        // Step 1:  Copy hello Blob into a new Input Asset for hello Job
        // ***NOTE: Ideally we would have a method tooingest a Blob directly here somehow. 
        // using code from this sample - https://azure.microsoft.com/en-us/documentation/articles/media-services-copying-existing-blob/

        StorageCredentials mediaServicesStorageCredentials =
            new StorageCredentials(_storageAccountName, _storageAccountKey);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with hello Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with hello encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify hello input asset toobe encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset toocontain hello results of hello job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means hello output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

        }
        catch (Exception ex)
        {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
        }
    }

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


    public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
        IAsset newAsset = null;

        try{
            Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
            newAsset = await copyAssetTask;
            log.Info($"Asset Copied : {newAsset.Id}");
        }
        catch(Exception ex){
            log.Info("Copy Failed");
            log.Info($"ERROR : {ex.Message}");
            throw ex;
        }

        return newAsset;
    }

    /// <summary>
    /// Creates a new asset and copies blobs from hello specifed storage account.
    /// </summary>
    /// <param name="blob">hello specified blob.</param>
    /// <returns>hello new asset.</returns>
    public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
    {
         //Get a reference toohello storage account that is associated with hello Media Services account. 
        StorageCredentials mediaServicesStorageCredentials =
        new StorageCredentials(_storageAccountName, _storageAccountKey);
        _destinationStorageAccount = new CloudStorageAccount(mediaServicesStorageCredentials, false);

        // Create a new asset. 
        var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
        log.Info($"Created new asset {asset.Name}");

        IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
        TimeSpan.FromHours(4), AccessPermissions.Write);
        ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
        CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

        // Get hello destination asset container reference
        string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
        CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

        try{
        assetContainer.CreateIfNotExists();
        }
        catch (Exception ex)
        {
        log.Error ("ERROR:" + ex.Message);
        }

        log.Info("Created asset.");

        // Get hold of hello destination blob
        CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

        // Copy Blob
        try
        {
        using (var stream = await blob.OpenReadAsync()) 
        {            
            await destinationBlob.UploadFromStreamAsync(stream);          
        }

        log.Info("Copy Complete.");

        var assetFile = asset.AssetFiles.Create(blob.Name);
        assetFile.ContentFileSize = blob.Properties.Length;
        assetFile.IsPrimary = true;
        assetFile.Update();
        asset.Update();
        }
        catch (Exception ex)
        {
        log.Error(ex.Message);
        log.Info (ex.StackTrace);
        log.Info ("Copy Failed.");
        throw;
        }

        destinationLocator.Delete();
        writePolicy.Delete();

        return asset;
    }
##<a name="test-your-function"></a><span data-ttu-id="c8026-176">A függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="c8026-176">Test your function</span></span>

<span data-ttu-id="c8026-177">tootest a függvény van szüksége egy MP4 fájlt hello tooupload **bemeneti** hello tárfiók hello kapcsolati karakterláncban megadott tároló.</span><span class="sxs-lookup"><span data-stu-id="c8026-177">tootest your function, you need tooupload an MP4 file into hello **input** container of hello storage account that you specified in hello connection string.</span></span>  

## <a name="next-step"></a><span data-ttu-id="c8026-178">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="c8026-178">Next step</span></span>

<span data-ttu-id="c8026-179">Ekkor készen áll a Media Services-alkalmazások fejlesztése toostart áll.</span><span class="sxs-lookup"><span data-stu-id="c8026-179">At this point, you are ready toostart developing a Media Services application.</span></span> 
 
<span data-ttu-id="c8026-180">További részletek és a teljes mintát/megoldások Azure Functions és a Logic Apps használatának az Azure Media Services toocreate egyéni létrehozási munkafolyamatok: hello [Media Services .NET funkciók Integraiton mintát a Githubon](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span><span class="sxs-lookup"><span data-stu-id="c8026-180">For more details and complete samples/solutions of using Azure Functions and Logic Apps with Azure Media Services toocreate custom content creation workflows, see hello [Media Services .NET Functions Integraiton Sample on GitHub](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)</span></span>

<span data-ttu-id="c8026-181">Lásd még [használata Azure Webhookok toomonitor Media Services .NET értesítések feladat](media-services-dotnet-check-job-progress-with-webhooks.md).</span><span class="sxs-lookup"><span data-stu-id="c8026-181">Also, see [Use Azure WebHooks toomonitor Media Services job notifications with .NET](media-services-dotnet-check-job-progress-with-webhooks.md).</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="c8026-182">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="c8026-182">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c8026-183">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c8026-183">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

