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
#<a name="develop-azure-functions-with-media-services"></a>A Media Services az Azure Functions fejlesztése

Ez a témakör bemutatja, hogyan tooget létrehozása az Azure Functions, használja a Media Services használatába. Ebben a témakörben meghatározott Azure-függvény hello figyeli nevű fiók tárolót **bemeneti** új MP4-fájlokat. Ha egy fájl hello storage tárolóba megszakad, hello blob eseményindító hello függvény fogja végrehajtani.

Ha szeretné, hogy tooexplore és központi telepítése a meglévő Azure Functions, amely az Azure Media Services, tekintse meg [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Ebben a tárházban található példák bemutatják, a Media Services tooshow munkafolyamatok kapcsolódó tooingesting tartalom közvetlenül a blob-tároló, kódolási és tartalmat ír biztonsági tooblob tárolási használó. Hogyan toomonitor feladat Webhookok és Azure várólisták értesítések példákat is tartalmaz. A funkciók hello hello példák alapján is készíthet [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) tárházba. toodeploy hello funkciók, majd nyomja le az hello **tooAzure telepítése** gombra.

## <a name="prerequisites"></a>Előfeltételek

- Mielőtt létrehozhatná az első függvényét, egy aktív Azure-fiók toohave kell. Ha még nem rendelkezik Azure-fiókkal, [létrehozhat egy ingyenes fiókot](https://azure.microsoft.com/free/).
- Ha toocreate Azure Functions műveleteket az Azure Media Services (AMS) fiók vagy a Media Services által küldött tooevents figyelni kívánja, akkor hozzon létre az AMS-fiók leírtak [Itt](media-services-portal-create-account.md).
- Megértését [hogyan toouse Azure functions](../azure-functions/functions-overview.md). Emellett tekintse át:
    - [Az Azure functions HTTP és a webhook kötések](../azure-functions/functions-triggers-bindings.md)
    - [Hogyan tooconfigure Azure-függvény Alkalmazásbeállítások](../azure-functions/functions-how-to-use-azure-function-app-settings.md)
    
## <a name="considerations"></a>Megfontolandó szempontok

-  Az Azure Functions hello fogyasztás terv alatt futó rendelkezik 5 perces időkorlát korlátozza.

## <a name="create-a-function-app"></a>Függvényalkalmazás létrehozása

1. Nyissa meg toohello [Azure-portálon](http://portal.azure.com) , és jelentkezzen be az Azure-fiókjával.
2. Függvény-alkalmazás létrehozása leírtak [Itt](../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Egy tárfiókot meg a hello **StorageConnection** környezeti változó (lásd a következő lépés hello) kell lennie a hello és az alkalmazás ugyanabban a régióban.

## <a name="configure-function-app-settings"></a>Függvény-Alkalmazásbeállítások konfigurálása

A Media Services funkciók fejlesztésekor fogja használni a teljes a funkciók hasznos tooadd környezeti változókat is. Alkalmazásbeállítások tooconfigure, hivatkozásra hello App beállításainak konfigurálása. További információkért lásd: [hogyan tooconfigure Azure-függvény Alkalmazásbeállítások](../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Példa:

![Beállítások](./media/media-services-azure-functions/media-services-azure-functions001.png)

hello függvényt, ez a cikk feltételezi, hogy rendelkezik-e a környezeti változók a beállítások a következő hello:

**AMSAccount** : *AMS-fiók neve* (pl. testams)

**AMSKey** : *AMS fiókkulcs* (pl. IHOySnH + XX3LGPfraE5fKPl0EnzvEPKkOPKCr59aiMM =)

**MediaServicesStorageAccountName** : *tárfióknév* (pl. testamsstorage)

**MediaServicesStorageAccountKey** : *tárfiók kulcsa* (pl. xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXXX ==)

**StorageConnection** : *tárolókapcsolat* (pl. megadni: DefaultEndpointsProtocol = https; AccountName = testamsstorage; AccountKey = xx7RN7mvpcipkuXvn5g7jwxnKh5MwYQ/awZAzkSIxQA8tmCtn93rqobjgjt41Wb0zwTZWeWQHY5kSZF0XXXXX ==)

## <a name="create-a-function"></a>Függvény létrehozása

A függvény az alkalmazást telepíti, ha megtalálja között **alkalmazásszolgáltatások** Azure Functions.

1. Válassza ki a függvény alkalmazást, majd kattintson a **új függvény**.
2. Válassza ki a hello **C#** nyelvi és **adatfeldolgozási** forgatókönyv.
3. Válasszon **BlobTrigger** sablont. Ez a funkció akkor is kiváltódik, amikor blob hello van töltve **bemeneti** tároló. Hello **bemeneti** neve van megadva az hello **elérési**, hello következő lépésben.

    ![Fájlok](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Miután **BlobTrigger**, néhány további vezérlők hello lapon fog megjelenni.

    ![Fájlok](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Kattintson a **Create** (Létrehozás) gombra. 


## <a name="files"></a>Fájlok

Az Azure-függvény kódot és egyéb fájlokat ebben a szakaszban ismertetett társítva. Alapértelmezés szerint a következő függvényt társított **function.json** és **run.csx** (C#) fájlok. Tooadd szüksége lesz egy **project.json** fájlt. Ez a szakasz többi hello hello definícióit ezeket a fájlokat jeleníti meg.

![Fájlok](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>Function.JSON

hello function.json fájl határozza meg, hello függvénykötés és egyéb konfigurációs beállításokkal. hello futásidejű használ, a fájl toodetermine hello események toomonitor és hogyan toopass adatok és a visszatérési adatainak függvény végrehajtása. További információkért lásd: [Azure functions HTTP és a webhook kötések](../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Set hello **le van tiltva** tulajdonság túl**igaz** tooprevent hello függvény végrehajtását. 


Íme egy példa **function.json** fájlt.

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

### <a name="projectjson"></a>Project.JSON

hello project.json fájl függőségeket tartalmaz. Íme egy példa **project.json** fájlt, amely tartalmazza a szükséges hello .NET Azure Media Services a Nuget-csomagok. Megjegyezzük, hogy hello verziószámok fogja módosítani legújabb frissítéseit toohello csomagok, így ellenőrizze hello legújabb verzióját. 

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
    
### <a name="runcsx"></a>Run.csx

Ez a hello C#-kódban a függvénynél.  figyelők az alábbiakban meghatározott hello függvény nevű fiók tárolót **bemeneti** (Ez mi volt megadva a hello elérési utat) az új MP4-fájlok. Ha egy fájl hello storage tárolóba megszakad, hello blob eseményindító hello függvény fogja végrehajtani.
    
Ebben a szakaszban meghatározott hello példa bemutatja, hogyan 

1. Hogyan tooingest egy eszköz, a Media Services a fiók (által másolás blob egy AMS adategységbe) és 
2. Hogyan toosubmit egy kódolási feladat által használt Media Encoder Standard tartozó "adaptív Streameléshez" előre.

Hello valós esetben valószínűleg szeretné, hogy tootrack feladatok előrehaladásának és tegye közzé a kódolt objektumhoz. További információkért lásd: [használata Azure Webhookok toomonitor Media Services feladat értesítések](media-services-dotnet-check-job-progress-with-webhooks.md). További példákért lásd [Media Services Azure Functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

Ha elkészült a függvényt definiáló kattintson **mentéséhez, és futtassa**.

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
##<a name="test-your-function"></a>A függvény tesztelése

tootest a függvény van szüksége egy MP4 fájlt hello tooupload **bemeneti** hello tárfiók hello kapcsolati karakterláncban megadott tároló.  

## <a name="next-step"></a>Következő lépés

Ekkor készen áll a Media Services-alkalmazások fejlesztése toostart áll. 
 
További részletek és a teljes mintát/megoldások Azure Functions és a Logic Apps használatának az Azure Media Services toocreate egyéni létrehozási munkafolyamatok: hello [Media Services .NET funkciók Integraiton mintát a Githubon](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Lásd még [használata Azure Webhookok toomonitor Media Services .NET értesítések feladat](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

