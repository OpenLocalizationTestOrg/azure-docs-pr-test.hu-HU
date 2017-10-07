---
title: "aaaManaging eszközök és a kapcsolódó entitásokból Media Services .NET SDK-val"
description: "Ismerje meg, hogyan toomanage eszközök és a kapcsolódó entitásokból hello Media Services SDK for .NET."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1bd8fd42-7306-463d-bfe5-f642802f1906
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 59a8543ffc6f7f30da2c67a6fcae09bc46da7a52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Eszközök és a kapcsolódó entitásokból Media Services .NET SDK kezelése
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-manage-entities.md)
> * [REST](media-services-rest-manage-entities.md)
> 
> 

Ez a témakör bemutatja, hogyan toomanage Azure Media Services .NET rendelkező entitások. 

>[!NOTE]
> 2017. április 1., kezdési bármely feladat rekord 90 napnál régebbi fiókja automatikusan törlődik, a társított feladat bejegyzéseket, valamint akkor is, ha hello rekordok teljes száma nem éri el hello kvóta felső határát. Például a 2017. április 1. a régebbi, mint a 2016. December 31-én fiókjában feladat rekordot automatikusan törlődni fog. Ha tooarchive hello/feladat tájékoztatásra van szüksége, használhatja a jelen témakörben ismertetett hello kódot.

## <a name="prerequisites"></a>Előfeltételek

A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 

## <a name="get-an-asset-reference"></a>Egy eszköz mutató hivatkozás beszerzése
A gyakori feladatok a hivatkozás tooan meglévő eszköz a Media Services tooget. a következő példakód hello jeleníti meg, hogyan letölthető egy eszköz hivatkozási hello eszközök gyűjtemény hello kiszolgálón környezeti objektumot, egy eszköz-azonosítóját. a következő kódot a példában hello alapján Linq lekérdezési tooget egy hivatkozási tooan meglévő IAsset objektumot.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query tooget an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference hello asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }

## <a name="list-all-assets"></a>Az összes eszköz felsorolása
Tároló található eszközök száma hello növekedésével hasznos toolist az eszközök. a következő példakód hello jeleníti meg, hogyan tooiterate keresztül hello eszközök gyűjtemény hello server környezeti objektumon. Az egyes eszközök hello példakód a tulajdonság értékek toohello konzol némelyike ír. Például minden eszköz sok media fájlokat tartalmazza. hello Kódpélda írja ki minden eszközhöz társított összes fájlt.

    static void ListAssets()
    {
        string waitMessage = "Building hello list. This may take a few "
            + "seconds tooa few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder toostore hello list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display hello collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display hello files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }

        // Display output in console.
        Console.Write(builder.ToString());
    }

## <a name="get-a-job-reference"></a>Feladat hivatkozás

Használatakor a Media Services kódban feladatok feldolgozása, gyakran kell egy hivatkozás tooan meglévő feladatot az alábbi kódpéldát azonosító hello alapján jeleníti meg, hogyan tooget egy hivatkozás tooan IJob hello feladatgyűjteménynek objektum tooget.

Előfordulhat, hogy a tooget feladat hivatkozni kell a hosszan futó kódolási feladat indításakor, és toocheck hello feladat állapota olyan szálon kell. Ilyen esetben hello metódus ad vissza egy olyan szálból, úgy kell tooretrieve egy frissített hivatkozás tooa feladat.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query tooget an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return hello job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }

## <a name="list-jobs-and-assets"></a>Lista feladatok és eszközök
Fontos kapcsolódó feladat toolist eszközök és az ahhoz tartozó feladatot a Media Services. hello következő kódrészlet példa bemutatja, hogyan toolist minden IJob objektumot, majd az egyes feladatokhoz megjeleníti hello feladat, az összes kapcsolódó feladatok, az összes bemeneti eszközök tulajdonságait, és az összes kimeneti eszközök. Ebben a példában hello kód számos más feladatok hasznos lehet. Például, ha azt szeretné, hogy toolist hello kimeneti eszközök egy vagy több kódolási feladatokból, korábban már futott, a kód bemutatja, hogyan tooaccess hello kimeneti eszközök. Ha egy hivatkozás tooan kimeneti eszközt, majd biztosíthat hello tartalom tooother felhasználók vagy alkalmazások által letöltheti, vagy URL-címek megadása. 

Eszközök kézbesítéséhez lehetőségekről további információkért lásd: [eszközök biztosítanak a Media Services SDK for .NET hello](media-services-deliver-streaming-content.md).

    // List all jobs on hello server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building hello list. This may take a few "
            + "seconds tooa few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder toostore hello list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display hello collection of jobs on hello server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display hello associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }

            // For each job, display hello list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {

                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }

            // For each job, display hello list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }

        }

        // Display output in console.
        Console.Write(builder.ToString());
    }

## <a name="list-all-access-policies"></a>Minden hozzáférési házirendek felsorolása
A Media Services szolgáltatásban a hozzáférési házirendek meg egy eszköz vagy annak fájljait. A hozzáférési házirendek meghatározása hello engedélyeit egy fájlra vagy egy eszköz (milyen típusú hozzáférést, és hello időtartama). A Media Services kódban általában definiált hozzáférési házirend egy IAccessPolicy objektum létrehozása és társítása egy meglévő eszközt. Ezután hozzon létre egy ILocator objektum, amely lehetővé teszi a Media Services közvetlen hozzáférést tooassets adja meg. hello Visual Studio-projekt, amely a dokumentáció sorozat társul több kód példák hogyan toocreate és hozzárendelése hozzáférési házirendek és a lokátorokat tooassets tartalmazza.

a következő kód példa azt mutatja meg hogyan hello hello kiszolgálón, és látható hello típusú engedélyek minden hozzáférési házirendek társított minden egyes toolist. Egy másik hasznos tooview hozzáférési házirendek toolist összes ILocator objektum hello kiszolgálón, és majd az egyes lokátor listázhatja a társított hozzáférési házirendet a AccessPolicy tulajdonság használatával.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");

        }
    }
    
## <a name="limit-access-policies"></a>Korlát hozzáférési házirendek 

>[!NOTE]
> A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. 

Például a kódot, amely csak akkor futtassa egyszer az alkalmazásban a következő hello egy általános házirendcsoport is létrehozhat. Bejelentkezhet azonosítók tooa naplófájl későbbi használatra:

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

Ezt követően a kódban ilyen azonosítók meglévő hello is használhatja:

    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get hello standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get hello existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("hello locator base path is " + originLocator.BaseUri.ToString());

## <a name="list-all-locators"></a>Minden keresők felsorolása
Egy kereső egy biztosító URL-címet egy közvetlen elérési tooaccess együtt engedélyek toohello eszköz referenciakonfigurációjához hello lokátor társított hozzáférési házirend által definiált konfigurációjának kialakításához. Minden eszköz lehet a Lokátorokat tulajdonsága kapcsolódó ILocator objektumok gyűjteménye. hello kiszolgálói környezetbe is, amely tartalmazza az összes keresők keresők gyűjteményével rendelkezik.

hello alábbi példakód sorolja fel az összes keresők hello kiszolgálón. Az egyes lokátor hello azonosító hello kapcsolódó eszköz és a hozzáférési házirend mutatja. Hello típusát és az engedélyek, hello lejárati dátumot, hello teljes elérési útja toohello eszköz is megjeleníti.

Vegye figyelembe, hogy a lokátor elérési tooan eszköz csak egy alap URL-cím toohello eszköz. a közvetlen elérési tooindividual, hogy egy felhasználó vagy alkalmazás sikerült keresse meg a fájlok toocreate, a kódot fel kell vennie hello megadott elérési út toohello lokátor elérési útja. További információt a toodo a, lásd: hello témakör [eszközök biztosítanak a Media Services SDK for .NET hello](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // hello locator path is hello base or parent path (with included permissions) tooaccess  
            // hello media content of an asset. toocreate a full URL tooa specific media file, take 
            // hello locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Az entitások nagy gyűjteményekre számbavétele
Entitások lekérdezésekor korlátozás van adja vissza egy időben, mert a nyilvános REST v2 korlátozza a lekérdezési eredmények too1000 eredmények 1000 entitások. Ha nagy gyűjteményekre entitások számbavétele toouse Skip, és hajtsa végre a megfelelő szüksége. 

hello következő függvény hurkok minden hello feladatok hello keresztül megadott Media Services-fiók. A Media Services 1000 feladatok Feladatgyűjteménynek adja vissza. hello funkció teszi használja a Skip, és tegye meg arról, hogy a toomake, amely az összes feladat enumerálása (Ha a fiók rendelkezik 1000-nél több feladat).

    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in hello Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }

                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

## <a name="delete-an-asset"></a>Egy eszköz törlése
a következő példa hello egy eszköz törlése.

    static void DeleteAsset( IAsset asset)
    {
        // delete hello asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted hello Asset");

    }

## <a name="delete-a-job"></a>Egy feladat törlése
egy feladat toodelete, ellenőrizni kell a hello feladat hello-State tulajdonsága a hello állapota. Feladatok befejeződött vagy érvénytelenített törölhetők, feladatok, amelyek az egyes állapotok például aszinkron, ütemezett vagy feldolgozásra, vissza kell vonni, és ezután törölje őket.

hello alábbi példakód bemutatja egy módszer, amellyel egy feladat törlése feladatállapotok ellenőrzése és majd törlésével a hello állapota nem fejeződött be, vagy megszakítva. Ez a kód hello előző szakasz ebben a témakörben az első hivatkozás tooa feladat függ: feladat hivatkozás.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;

        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);

            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync toodo async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }

        }
    }


## <a name="delete-an-access-policy"></a>Hozzáférési házirend törlése
hello következő kódrészlet példa bemutatja, hogyan tooget egy hivatkozás tooan hozzáférési házirend alapján a házirend azonosítója, és toodelete hello házirend.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // toodelete a specific access policy, get a reference toohello policy.  
        // based on hello policy Id passed toohello method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

