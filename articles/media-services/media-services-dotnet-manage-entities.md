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
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a><span data-ttu-id="9cef9-103">Eszközök és a kapcsolódó entitásokból Media Services .NET SDK kezelése</span><span class="sxs-lookup"><span data-stu-id="9cef9-103">Managing Assets and Related Entities with Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9cef9-104">.NET</span><span class="sxs-lookup"><span data-stu-id="9cef9-104">.NET</span></span>](media-services-dotnet-manage-entities.md)
> * [<span data-ttu-id="9cef9-105">REST</span><span class="sxs-lookup"><span data-stu-id="9cef9-105">REST</span></span>](media-services-rest-manage-entities.md)
> 
> 

<span data-ttu-id="9cef9-106">Ez a témakör bemutatja, hogyan toomanage Azure Media Services .NET rendelkező entitások.</span><span class="sxs-lookup"><span data-stu-id="9cef9-106">This topic shows how toomanage Azure Media Services entities with .NET.</span></span> 

>[!NOTE]
> <span data-ttu-id="9cef9-107">2017. április 1., kezdési bármely feladat rekord 90 napnál régebbi fiókja automatikusan törlődik, a társított feladat bejegyzéseket, valamint akkor is, ha hello rekordok teljes száma nem éri el hello kvóta felső határát.</span><span class="sxs-lookup"><span data-stu-id="9cef9-107">Starting April 1, 2017, any Job record in your account older than 90 days will be automatically deleted, along with its associated Task records, even if hello total number of records is below hello maximum quota.</span></span> <span data-ttu-id="9cef9-108">Például a 2017. április 1. a régebbi, mint a 2016. December 31-én fiókjában feladat rekordot automatikusan törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="9cef9-108">For example, on April 1, 2017, any Job record in your account older than December 31, 2016, will be automatically deleted.</span></span> <span data-ttu-id="9cef9-109">Ha tooarchive hello/feladat tájékoztatásra van szüksége, használhatja a jelen témakörben ismertetett hello kódot.</span><span class="sxs-lookup"><span data-stu-id="9cef9-109">If you need tooarchive hello job/task information, you can use hello code described in this topic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cef9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9cef9-110">Prerequisites</span></span>

<span data-ttu-id="9cef9-111">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="9cef9-111">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="get-an-asset-reference"></a><span data-ttu-id="9cef9-112">Egy eszköz mutató hivatkozás beszerzése</span><span class="sxs-lookup"><span data-stu-id="9cef9-112">Get an Asset Reference</span></span>
<span data-ttu-id="9cef9-113">A gyakori feladatok a hivatkozás tooan meglévő eszköz a Media Services tooget.</span><span class="sxs-lookup"><span data-stu-id="9cef9-113">A frequent task is tooget a reference tooan existing asset in Media Services.</span></span> <span data-ttu-id="9cef9-114">a következő példakód hello jeleníti meg, hogyan letölthető egy eszköz hivatkozási hello eszközök gyűjtemény hello kiszolgálón környezeti objektumot, egy eszköz-azonosítóját. a következő kódot a példában hello alapján Linq lekérdezési tooget egy hivatkozási tooan meglévő IAsset objektumot.</span><span class="sxs-lookup"><span data-stu-id="9cef9-114">hello following code example shows how you can get an asset reference from hello Assets collection on hello server context object, based on an asset Id. hello following code example uses a Linq query tooget a reference tooan existing IAsset object.</span></span>

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

## <a name="list-all-assets"></a><span data-ttu-id="9cef9-115">Az összes eszköz felsorolása</span><span class="sxs-lookup"><span data-stu-id="9cef9-115">List All Assets</span></span>
<span data-ttu-id="9cef9-116">Tároló található eszközök száma hello növekedésével hasznos toolist az eszközök.</span><span class="sxs-lookup"><span data-stu-id="9cef9-116">As hello number of assets you have in storage grows, it is helpful toolist your assets.</span></span> <span data-ttu-id="9cef9-117">a következő példakód hello jeleníti meg, hogyan tooiterate keresztül hello eszközök gyűjtemény hello server környezeti objektumon.</span><span class="sxs-lookup"><span data-stu-id="9cef9-117">hello following code example shows how tooiterate through hello Assets collection on hello server context object.</span></span> <span data-ttu-id="9cef9-118">Az egyes eszközök hello példakód a tulajdonság értékek toohello konzol némelyike ír.</span><span class="sxs-lookup"><span data-stu-id="9cef9-118">With each asset, hello code example also writes some of its property values toohello console.</span></span> <span data-ttu-id="9cef9-119">Például minden eszköz sok media fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9cef9-119">For example, each asset can contain many media files.</span></span> <span data-ttu-id="9cef9-120">hello Kódpélda írja ki minden eszközhöz társított összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="9cef9-120">hello code example writes out all files associated with each asset.</span></span>

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

## <a name="get-a-job-reference"></a><span data-ttu-id="9cef9-121">Feladat hivatkozás</span><span class="sxs-lookup"><span data-stu-id="9cef9-121">Get a Job Reference</span></span>

<span data-ttu-id="9cef9-122">Használatakor a Media Services kódban feladatok feldolgozása, gyakran kell egy hivatkozás tooan meglévő feladatot az alábbi kódpéldát azonosító hello alapján jeleníti meg, hogyan tooget egy hivatkozás tooan IJob hello feladatgyűjteménynek objektum tooget.</span><span class="sxs-lookup"><span data-stu-id="9cef9-122">When you work with processing tasks in Media Services code, you often need tooget a reference tooan existing job based on an Id. hello following code example shows how tooget a reference tooan IJob object from hello Jobs collection.</span></span>

<span data-ttu-id="9cef9-123">Előfordulhat, hogy a tooget feladat hivatkozni kell a hosszan futó kódolási feladat indításakor, és toocheck hello feladat állapota olyan szálon kell.</span><span class="sxs-lookup"><span data-stu-id="9cef9-123">You may need tooget a job reference when starting a long-running encoding job, and need toocheck hello job status on a thread.</span></span> <span data-ttu-id="9cef9-124">Ilyen esetben hello metódus ad vissza egy olyan szálból, úgy kell tooretrieve egy frissített hivatkozás tooa feladat.</span><span class="sxs-lookup"><span data-stu-id="9cef9-124">In cases like this, when hello method returns from a thread, you need tooretrieve a refreshed reference tooa job.</span></span>

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

## <a name="list-jobs-and-assets"></a><span data-ttu-id="9cef9-125">Lista feladatok és eszközök</span><span class="sxs-lookup"><span data-stu-id="9cef9-125">List Jobs and Assets</span></span>
<span data-ttu-id="9cef9-126">Fontos kapcsolódó feladat toolist eszközök és az ahhoz tartozó feladatot a Media Services.</span><span class="sxs-lookup"><span data-stu-id="9cef9-126">An important related task is toolist assets with their associated job in Media Services.</span></span> <span data-ttu-id="9cef9-127">hello következő kódrészlet példa bemutatja, hogyan toolist minden IJob objektumot, majd az egyes feladatokhoz megjeleníti hello feladat, az összes kapcsolódó feladatok, az összes bemeneti eszközök tulajdonságait, és az összes kimeneti eszközök.</span><span class="sxs-lookup"><span data-stu-id="9cef9-127">hello following code example shows you how toolist each IJob object, and then for each job, it displays properties about hello job, all related tasks, all input assets, and all output assets.</span></span> <span data-ttu-id="9cef9-128">Ebben a példában hello kód számos más feladatok hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="9cef9-128">hello code in this example can be useful for numerous other tasks.</span></span> <span data-ttu-id="9cef9-129">Például, ha azt szeretné, hogy toolist hello kimeneti eszközök egy vagy több kódolási feladatokból, korábban már futott, a kód bemutatja, hogyan tooaccess hello kimeneti eszközök.</span><span class="sxs-lookup"><span data-stu-id="9cef9-129">For example, if you want toolist hello output assets from one or more encoding jobs that you ran previously, this code shows how tooaccess hello output assets.</span></span> <span data-ttu-id="9cef9-130">Ha egy hivatkozás tooan kimeneti eszközt, majd biztosíthat hello tartalom tooother felhasználók vagy alkalmazások által letöltheti, vagy URL-címek megadása.</span><span class="sxs-lookup"><span data-stu-id="9cef9-130">When you have a reference tooan output asset, you can then deliver hello content tooother users or applications by downloading it, or providing URLs.</span></span> 

<span data-ttu-id="9cef9-131">Eszközök kézbesítéséhez lehetőségekről további információkért lásd: [eszközök biztosítanak a Media Services SDK for .NET hello](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="9cef9-131">For more information on options for delivering assets, see [Deliver Assets with hello Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

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

## <a name="list-all-access-policies"></a><span data-ttu-id="9cef9-132">Minden hozzáférési házirendek felsorolása</span><span class="sxs-lookup"><span data-stu-id="9cef9-132">List all Access Policies</span></span>
<span data-ttu-id="9cef9-133">A Media Services szolgáltatásban a hozzáférési házirendek meg egy eszköz vagy annak fájljait.</span><span class="sxs-lookup"><span data-stu-id="9cef9-133">In Media Services, you can define an access policy on an asset or its files.</span></span> <span data-ttu-id="9cef9-134">A hozzáférési házirendek meghatározása hello engedélyeit egy fájlra vagy egy eszköz (milyen típusú hozzáférést, és hello időtartama).</span><span class="sxs-lookup"><span data-stu-id="9cef9-134">An access policy defines hello permissions for a file or an asset (what type of access, and hello duration).</span></span> <span data-ttu-id="9cef9-135">A Media Services kódban általában definiált hozzáférési házirend egy IAccessPolicy objektum létrehozása és társítása egy meglévő eszközt.</span><span class="sxs-lookup"><span data-stu-id="9cef9-135">In your Media Services code, you typically define an access policy by creating an IAccessPolicy object and then associating it with an existing asset.</span></span> <span data-ttu-id="9cef9-136">Ezután hozzon létre egy ILocator objektum, amely lehetővé teszi a Media Services közvetlen hozzáférést tooassets adja meg.</span><span class="sxs-lookup"><span data-stu-id="9cef9-136">Then you create a ILocator object, which lets you provide direct access tooassets in Media Services.</span></span> <span data-ttu-id="9cef9-137">hello Visual Studio-projekt, amely a dokumentáció sorozat társul több kód példák hogyan toocreate és hozzárendelése hozzáférési házirendek és a lokátorokat tooassets tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9cef9-137">hello Visual Studio project that accompanies this documentation series contains several code examples that show how toocreate and assign access policies and locators tooassets.</span></span>

<span data-ttu-id="9cef9-138">a következő kód példa azt mutatja meg hogyan hello hello kiszolgálón, és látható hello típusú engedélyek minden hozzáférési házirendek társított minden egyes toolist.</span><span class="sxs-lookup"><span data-stu-id="9cef9-138">hello following code example shows how toolist all access policies on hello server, and shows hello type of permissions associated with each.</span></span> <span data-ttu-id="9cef9-139">Egy másik hasznos tooview hozzáférési házirendek toolist összes ILocator objektum hello kiszolgálón, és majd az egyes lokátor listázhatja a társított hozzáférési házirendet a AccessPolicy tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="9cef9-139">Another useful way tooview access policies is toolist all ILocator objects on hello server, and then for each locator, you can list its associated access policy by using its AccessPolicy property.</span></span>

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
    
## <a name="limit-access-policies"></a><span data-ttu-id="9cef9-140">Korlát hozzáférési házirendek</span><span class="sxs-lookup"><span data-stu-id="9cef9-140">Limit Access Policies</span></span> 

>[!NOTE]
> <span data-ttu-id="9cef9-141">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="9cef9-141">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="9cef9-142">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="9cef9-142">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> 

<span data-ttu-id="9cef9-143">Például a kódot, amely csak akkor futtassa egyszer az alkalmazásban a következő hello egy általános házirendcsoport is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="9cef9-143">For example, you can create a generic set of policies with hello following code that would only run one time in your application.</span></span> <span data-ttu-id="9cef9-144">Bejelentkezhet azonosítók tooa naplófájl későbbi használatra:</span><span class="sxs-lookup"><span data-stu-id="9cef9-144">You can log IDs tooa log file for later use:</span></span>

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

<span data-ttu-id="9cef9-145">Ezt követően a kódban ilyen azonosítók meglévő hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="9cef9-145">Then, you can use hello existing IDs in your code like this:</span></span>

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

## <a name="list-all-locators"></a><span data-ttu-id="9cef9-146">Minden keresők felsorolása</span><span class="sxs-lookup"><span data-stu-id="9cef9-146">List All Locators</span></span>
<span data-ttu-id="9cef9-147">Egy kereső egy biztosító URL-címet egy közvetlen elérési tooaccess együtt engedélyek toohello eszköz referenciakonfigurációjához hello lokátor társított hozzáférési házirend által definiált konfigurációjának kialakításához.</span><span class="sxs-lookup"><span data-stu-id="9cef9-147">A locator is a URL that provides a direct path tooaccess an asset, along with permissions toohello asset as defined by hello locator's associated access policy.</span></span> <span data-ttu-id="9cef9-148">Minden eszköz lehet a Lokátorokat tulajdonsága kapcsolódó ILocator objektumok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="9cef9-148">Each asset can have a collection of ILocator objects associated with it on its Locators property.</span></span> <span data-ttu-id="9cef9-149">hello kiszolgálói környezetbe is, amely tartalmazza az összes keresők keresők gyűjteményével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9cef9-149">hello server context also has a Locators collection that contains all locators.</span></span>

<span data-ttu-id="9cef9-150">hello alábbi példakód sorolja fel az összes keresők hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9cef9-150">hello following code example lists all locators on hello server.</span></span> <span data-ttu-id="9cef9-151">Az egyes lokátor hello azonosító hello kapcsolódó eszköz és a hozzáférési házirend mutatja.</span><span class="sxs-lookup"><span data-stu-id="9cef9-151">For each locator, it shows hello Id for hello related asset and access policy.</span></span> <span data-ttu-id="9cef9-152">Hello típusát és az engedélyek, hello lejárati dátumot, hello teljes elérési útja toohello eszköz is megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="9cef9-152">It also displays hello type of permissions, hello expiration date, and hello full path toohello asset.</span></span>

<span data-ttu-id="9cef9-153">Vegye figyelembe, hogy a lokátor elérési tooan eszköz csak egy alap URL-cím toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="9cef9-153">Note that a locator path tooan asset is only a base URL toohello asset.</span></span> <span data-ttu-id="9cef9-154">a közvetlen elérési tooindividual, hogy egy felhasználó vagy alkalmazás sikerült keresse meg a fájlok toocreate, a kódot fel kell vennie hello megadott elérési út toohello lokátor elérési útja.</span><span class="sxs-lookup"><span data-stu-id="9cef9-154">toocreate a direct path tooindividual files that a user or application could browse to, your code must add hello specific file path toohello locator path.</span></span> <span data-ttu-id="9cef9-155">További információt a toodo a, lásd: hello témakör [eszközök biztosítanak a Media Services SDK for .NET hello](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="9cef9-155">For more information on how toodo this, see hello topic [Deliver Assets with hello Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

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

## <a name="enumerating-through-large-collections-of-entities"></a><span data-ttu-id="9cef9-156">Az entitások nagy gyűjteményekre számbavétele</span><span class="sxs-lookup"><span data-stu-id="9cef9-156">Enumerating through large collections of entities</span></span>
<span data-ttu-id="9cef9-157">Entitások lekérdezésekor korlátozás van adja vissza egy időben, mert a nyilvános REST v2 korlátozza a lekérdezési eredmények too1000 eredmények 1000 entitások.</span><span class="sxs-lookup"><span data-stu-id="9cef9-157">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results too1000 results.</span></span> <span data-ttu-id="9cef9-158">Ha nagy gyűjteményekre entitások számbavétele toouse Skip, és hajtsa végre a megfelelő szüksége.</span><span class="sxs-lookup"><span data-stu-id="9cef9-158">You need toouse Skip and Take when enumerating through large collections of entities.</span></span> 

<span data-ttu-id="9cef9-159">hello következő függvény hurkok minden hello feladatok hello keresztül megadott Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="9cef9-159">hello following function loops through all hello jobs in hello provided Media Services Account.</span></span> <span data-ttu-id="9cef9-160">A Media Services 1000 feladatok Feladatgyűjteménynek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9cef9-160">Media Services returns 1000 jobs in Jobs Collection.</span></span> <span data-ttu-id="9cef9-161">hello funkció teszi használja a Skip, és tegye meg arról, hogy a toomake, amely az összes feladat enumerálása (Ha a fiók rendelkezik 1000-nél több feladat).</span><span class="sxs-lookup"><span data-stu-id="9cef9-161">hello function makes use of Skip and Take toomake sure that all jobs are enumerated (in case you have more than 1000 jobs in your account).</span></span>

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

## <a name="delete-an-asset"></a><span data-ttu-id="9cef9-162">Egy eszköz törlése</span><span class="sxs-lookup"><span data-stu-id="9cef9-162">Delete an Asset</span></span>
<span data-ttu-id="9cef9-163">a következő példa hello egy eszköz törlése.</span><span class="sxs-lookup"><span data-stu-id="9cef9-163">hello following example deletes an asset.</span></span>

    static void DeleteAsset( IAsset asset)
    {
        // delete hello asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted hello Asset");

    }

## <a name="delete-a-job"></a><span data-ttu-id="9cef9-164">Egy feladat törlése</span><span class="sxs-lookup"><span data-stu-id="9cef9-164">Delete a Job</span></span>
<span data-ttu-id="9cef9-165">egy feladat toodelete, ellenőrizni kell a hello feladat hello-State tulajdonsága a hello állapota.</span><span class="sxs-lookup"><span data-stu-id="9cef9-165">toodelete a job, you must check hello state of hello job as indicated in hello State property.</span></span> <span data-ttu-id="9cef9-166">Feladatok befejeződött vagy érvénytelenített törölhetők, feladatok, amelyek az egyes állapotok például aszinkron, ütemezett vagy feldolgozásra, vissza kell vonni, és ezután törölje őket.</span><span class="sxs-lookup"><span data-stu-id="9cef9-166">Jobs that are finished or canceled can be deleted, while jobs that are in certain other states, such as queued, scheduled, or processing, must be canceled first, and then they can be deleted.</span></span>

<span data-ttu-id="9cef9-167">hello alábbi példakód bemutatja egy módszer, amellyel egy feladat törlése feladatállapotok ellenőrzése és majd törlésével a hello állapota nem fejeződött be, vagy megszakítva.</span><span class="sxs-lookup"><span data-stu-id="9cef9-167">hello following code example shows a method for deleting a job by checking job states and then deleting when hello state is finished or canceled.</span></span> <span data-ttu-id="9cef9-168">Ez a kód hello előző szakasz ebben a témakörben az első hivatkozás tooa feladat függ: feladat hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="9cef9-168">This code depends on hello previous section in this topic for getting a reference tooa job: Get a job reference.</span></span>

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


## <a name="delete-an-access-policy"></a><span data-ttu-id="9cef9-169">Hozzáférési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="9cef9-169">Delete an Access Policy</span></span>
<span data-ttu-id="9cef9-170">hello következő kódrészlet példa bemutatja, hogyan tooget egy hivatkozás tooan hozzáférési házirend alapján a házirend azonosítója, és toodelete hello házirend.</span><span class="sxs-lookup"><span data-stu-id="9cef9-170">hello following code example shows how tooget a reference tooan access policy based on a policy Id, and then toodelete hello policy.</span></span>

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



## <a name="media-services-learning-paths"></a><span data-ttu-id="9cef9-171">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="9cef9-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9cef9-172">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="9cef9-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

