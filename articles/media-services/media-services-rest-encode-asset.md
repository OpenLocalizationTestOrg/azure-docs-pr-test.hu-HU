---
title: "egy Azure-Media Encoder Standard használatával objektum aaaHow tooencode |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Media Encoder Standard tooencode media Azure Media Services a tartalmat. Kódminták REST API-t használja."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b766bafded7ee98eda3e6ef149c31d5d8fe406fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencode-an-asset-by-using-media-encoder-standard"></a><span data-ttu-id="306cf-104">Hogyan tooencode egy eszköz Media Encoder Standard használatával</span><span class="sxs-lookup"><span data-stu-id="306cf-104">How tooencode an asset by using Media Encoder Standard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="306cf-105">.NET</span><span class="sxs-lookup"><span data-stu-id="306cf-105">.NET</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [<span data-ttu-id="306cf-106">REST</span><span class="sxs-lookup"><span data-stu-id="306cf-106">REST</span></span>](media-services-rest-encode-asset.md)
> * [<span data-ttu-id="306cf-107">Portál</span><span class="sxs-lookup"><span data-stu-id="306cf-107">Portal</span></span>](media-services-portal-encode.md)
>
>

## <a name="overview"></a><span data-ttu-id="306cf-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="306cf-108">Overview</span></span>
<span data-ttu-id="306cf-109">toodeliver digitális videót hello interneten keresztül, kell tömöríteni hello adathordozót.</span><span class="sxs-lookup"><span data-stu-id="306cf-109">toodeliver digital video over hello Internet, you must compress hello media.</span></span> <span data-ttu-id="306cf-110">Digitális videofájlok nagyok, és lehet, hogy túl nagy toodeliver hello interneten keresztül, vagy az ügyfelek eszközök toodisplay megfelelően.</span><span class="sxs-lookup"><span data-stu-id="306cf-110">Digital video files are large and may be too big toodeliver over hello Internet, or for your customers’ devices toodisplay properly.</span></span> <span data-ttu-id="306cf-111">Kódolás az tömörítés videó és hang, így az ügyfelek megtekintheti a media hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="306cf-111">Encoding is hello process of compressing video and audio so your customers can view your media.</span></span>

<span data-ttu-id="306cf-112">Kódolási feladatok olyan hello leggyakoribb feldolgozási műveletek Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="306cf-112">Encoding jobs are one of hello most common processing operations in Azure Media Services.</span></span> <span data-ttu-id="306cf-113">Kódolási feladatok tooconvert médiafájlok hoz létre egy kódolási tooanother.</span><span class="sxs-lookup"><span data-stu-id="306cf-113">You create encoding jobs tooconvert media files from one encoding tooanother.</span></span> <span data-ttu-id="306cf-114">Amikor kódolására, hello Media Services beépített kódoló (Media Encoder Standard) is használhatja.</span><span class="sxs-lookup"><span data-stu-id="306cf-114">When you encode, you can use hello Media Services built-in encoder (Media Encoder Standard).</span></span> <span data-ttu-id="306cf-115">Egy Media Services partner által biztosított kódoló is használható.</span><span class="sxs-lookup"><span data-stu-id="306cf-115">You can also use an encoder provided by a Media Services partner.</span></span> <span data-ttu-id="306cf-116">Külső kódolók hello Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="306cf-116">Third-party encoders are available through hello Azure Marketplace.</span></span> <span data-ttu-id="306cf-117">Hello részleteit kódolási feladatokhoz a kódoló-készlet karakterláncokat használatával, vagy előre definiált konfigurációs fájlok használatával adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="306cf-117">You can specify hello details of encoding tasks by using preset strings defined for your encoder, or by using preset configuration files.</span></span> <span data-ttu-id="306cf-118">a rendelkezésre álló készletek toosee hello típusú lásd [feladat készletek a Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="306cf-118">toosee hello types of presets that are available, see [Task Presets for Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="306cf-119">Minden egyes feladat egy vagy, hogy a feldolgozási hello típusától függően további feladatok tooaccomplish szeretné.</span><span class="sxs-lookup"><span data-stu-id="306cf-119">Each job can have one or more tasks depending on hello type of processing that you want tooaccomplish.</span></span> <span data-ttu-id="306cf-120">Hello REST API-t, keresztül hozhat létre feladatokat és azok kapcsolódó feladatok az alábbi két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="306cf-120">Through hello REST API, you can create jobs and their related tasks in one of two ways:</span></span>

* <span data-ttu-id="306cf-121">Feladatok lehet meghatározott beágyazott hello feladatok navigációs tulajdonság a feladat entitások keresztül.</span><span class="sxs-lookup"><span data-stu-id="306cf-121">Tasks can be defined inline through hello Tasks navigation property on Job entities.</span></span>
* <span data-ttu-id="306cf-122">OData kötegfeldolgozási használja.</span><span class="sxs-lookup"><span data-stu-id="306cf-122">Use OData batch processing.</span></span>

<span data-ttu-id="306cf-123">Javasoljuk, hogy mindig kódolása egy adaptív sávszélességű MP4 állítsa be a forrásfájlokat, és alakítsa át hello beállítása toohello kívánt formátum használatával [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-123">We recommend that you always encode your source files into an adaptive bitrate MP4 set, and then convert hello set toohello desired format by using [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="306cf-124">Ha az kimeneti adategységen tárolótitkosítást alkalmaz, konfigurálnia kell a hello adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="306cf-124">If your output asset is storage encrypted, you must configure hello asset delivery policy.</span></span> <span data-ttu-id="306cf-125">További információkért lásd: [objektumtovábbítási szabályzat konfigurálása](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-125">For more information, see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="306cf-126">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="306cf-126">Considerations</span></span>

<span data-ttu-id="306cf-127">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="306cf-127">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="306cf-128">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-128">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

<span data-ttu-id="306cf-129">Mielőtt media processzorok hivatkozik, győződjön meg arról, hogy rendelkezik-e hello helyes adathordozók processzor azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="306cf-129">Before you start referencing media processors, verify that you have hello correct media processor ID.</span></span> <span data-ttu-id="306cf-130">További információkért lásd: [media processzorok beolvasása](media-services-rest-get-media-processor.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-130">For more information, see [Get media processors](media-services-rest-get-media-processor.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="306cf-131">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="306cf-131">Connect tooMedia Services</span></span>

<span data-ttu-id="306cf-132">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-132">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="306cf-133">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="306cf-133">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="306cf-134">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="306cf-134">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-a-job-with-a-single-encoding-task"></a><span data-ttu-id="306cf-135">Hozzon létre egy feladatot egyetlen kódolási feladat</span><span class="sxs-lookup"><span data-stu-id="306cf-135">Create a job with a single encoding task</span></span>
> [!NOTE]
> <span data-ttu-id="306cf-136">Dolgozunk a hello Media Services REST API-t, ha hello következők érvényesek:</span><span class="sxs-lookup"><span data-stu-id="306cf-136">When you're working with hello Media Services REST API, hello following considerations apply:</span></span>
>
> <span data-ttu-id="306cf-137">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="306cf-137">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="306cf-138">További információkért lásd: [Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-138">For more information, see [Setup for Media Services REST API development](media-services-rest-how-to-use.md).</span></span>
>
> <span data-ttu-id="306cf-139">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="306cf-139">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="306cf-140">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="306cf-140">You must make subsequent calls toohello new URI.</span></span> <span data-ttu-id="306cf-141">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-141">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
>
> <span data-ttu-id="306cf-142">Ha JSON használatával és toouse hello megadásával **__metadata** hello kérelem (már például tooreferences csatolt objektum van hátra), meg kell adni a hello kulcsszó **elfogadás** fejléc túl[részletes JSON formátumban ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Fogadja el: az application/json; odata = részletes.</span><span class="sxs-lookup"><span data-stu-id="306cf-142">When using JSON and specifying toouse hello **__metadata** keyword in hello request (for example, tooreferences a linked object), you must set hello **Accept** header too[JSON Verbose format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accept: application/json;odata=verbose.</span></span>
>
>

<span data-ttu-id="306cf-143">hello a következő példa bemutatja, hogyan toocreate és a feladás egy vagy több feladat is egy feladatot állíthatja tooencode videó adott feloldási és minőségi.</span><span class="sxs-lookup"><span data-stu-id="306cf-143">hello following example shows you how toocreate and post a job with one task set tooencode a video at a specific resolution and quality.</span></span> <span data-ttu-id="306cf-144">A Media Encoder Standard kódol, használhatja a megadott feladat konfigurációs készletek [Itt](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="306cf-144">When you encode with Media Encoder Standard, you can use task configuration presets specified [here](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="306cf-145">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="306cf-145">Request:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

<span data-ttu-id="306cf-146">Válasz:</span><span class="sxs-lookup"><span data-stu-id="306cf-146">Response:</span></span>

    HTTP/1.1 201 Created

    . . .

### <a name="set-hello-output-assets-name"></a><span data-ttu-id="306cf-147">– Hello kimeneti eszköz neve</span><span class="sxs-lookup"><span data-stu-id="306cf-147">Set hello output asset's name</span></span>
<span data-ttu-id="306cf-148">hello a következő példa bemutatja, hogyan tooset hello assetName attribútum:</span><span class="sxs-lookup"><span data-stu-id="306cf-148">hello following example shows how tooset hello assetName attribute:</span></span>

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a><span data-ttu-id="306cf-149">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="306cf-149">Considerations</span></span>
* <span data-ttu-id="306cf-150">TaskBody tulajdonságok literális XML toodefine hello száma bemeneti vagy kimeneti eszközök hello tevékenység által használt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="306cf-150">TaskBody properties must use literal XML toodefine hello number of input, or output assets that are used by hello task.</span></span> <span data-ttu-id="306cf-151">hello feladat témakör hello XML Schema definíció hello XML számára.</span><span class="sxs-lookup"><span data-stu-id="306cf-151">hello task topic contains hello XML Schema Definition for hello XML.</span></span>
* <span data-ttu-id="306cf-152">Hello TaskBody definícióját, az egyes belső értékének <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="306cf-152">In hello TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="306cf-153">Kimeneti adategységek feladatonként.</span><span class="sxs-lookup"><span data-stu-id="306cf-153">A task can have multiple output assets.</span></span> <span data-ttu-id="306cf-154">Egy JobOutputAsset(x) csak egyszer használható egy feladatot a feladatok kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="306cf-154">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="306cf-155">A tevékenység bemeneti eszközként JobInputAsset vagy JobOutputAsset adhat meg.</span><span class="sxs-lookup"><span data-stu-id="306cf-155">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="306cf-156">Feladatok kell nem alkotnak hurkot.</span><span class="sxs-lookup"><span data-stu-id="306cf-156">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="306cf-157">hello érték paraméter átadására tooJobInputAsset vagy JobOutputAsset hello index értékét az adott eszköz számára jelöli.</span><span class="sxs-lookup"><span data-stu-id="306cf-157">hello value parameter that you pass tooJobInputAsset or JobOutputAsset represents hello index value for an asset.</span></span> <span data-ttu-id="306cf-158">hello tényleges eszközök hello InputMediaAssets és hello entitás feladatdefiníció OutputMediaAssets navigációs tulajdonságainak vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="306cf-158">hello actual assets are defined in hello InputMediaAssets and OutputMediaAssets navigation properties on hello job entity definition.</span></span>
* <span data-ttu-id="306cf-159">A Media Services OData v3 épül, mert egyes eszközöket a hello InputMediaAssets hello és OutputMediaAssets navigációs tulajdonság gyűjtemények hivatkozott keresztül a "__metadata: uri" név-érték pár.</span><span class="sxs-lookup"><span data-stu-id="306cf-159">Because Media Services is built on OData v3, hello individual assets in hello InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
* <span data-ttu-id="306cf-160">InputMediaAssets tooone vagy további eszközök, a Media Services továbbítja.</span><span class="sxs-lookup"><span data-stu-id="306cf-160">InputMediaAssets maps tooone or more assets that you created in Media Services.</span></span> <span data-ttu-id="306cf-161">OutputMediaAssets hello rendszer hozza létre.</span><span class="sxs-lookup"><span data-stu-id="306cf-161">OutputMediaAssets are created by hello system.</span></span> <span data-ttu-id="306cf-162">Egy meglévő eszköz nem hivatkozó.</span><span class="sxs-lookup"><span data-stu-id="306cf-162">They don't reference an existing asset.</span></span>
* <span data-ttu-id="306cf-163">OutputMediaAssets elnevezheti hello assetName attribútum használatával.</span><span class="sxs-lookup"><span data-stu-id="306cf-163">OutputMediaAssets can be named by using hello assetName attribute.</span></span> <span data-ttu-id="306cf-164">Ha ez az attribútum nincs jelen, akkor hello OutputMediaAsset hello nevét bármilyen hello belső szöveges értékét hello <outputAsset> elem utótaggal hello feladat nevét, és közül hello feladatazonosító érték (hello esetet, ahol hello Name tulajdonság nincs megadva).</span><span class="sxs-lookup"><span data-stu-id="306cf-164">If this attribute is not present, then hello name of hello OutputMediaAsset is whatever hello inner text value of hello <outputAsset> element is with a suffix of either hello Job Name value, or hello Job Id value (in hello case where hello Name property isn't defined).</span></span> <span data-ttu-id="306cf-165">Például ha assetName értékének beállítása túl "Sample", majd hello OutputMediaAsset Name tulajdonsága túl "Sample."</span><span class="sxs-lookup"><span data-stu-id="306cf-165">For example, if you set a value for assetName too"Sample," then hello OutputMediaAsset Name property is set too"Sample."</span></span> <span data-ttu-id="306cf-166">Azonban ha assetName értékének beállítása nem, de adta meg az hello feladat neve túl "NewJob", majd hello OutputMediaAsset neve lenne "_NewJob JobOutputAsset (érték)."</span><span class="sxs-lookup"><span data-stu-id="306cf-166">However, if you didn't set a value for assetName, but did set hello job name too"NewJob," then hello OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob."</span></span>

## <a name="create-a-job-with-chained-tasks"></a><span data-ttu-id="306cf-167">Hozzon létre egy feladatot láncolt feladatok</span><span class="sxs-lookup"><span data-stu-id="306cf-167">Create a job with chained tasks</span></span>
<span data-ttu-id="306cf-168">Számos alkalmazás forgatókönyvben fejlesztők szeretné toocreate feldolgozási feladatok sorozata.</span><span class="sxs-lookup"><span data-stu-id="306cf-168">In many application scenarios, developers want toocreate a series of processing tasks.</span></span> <span data-ttu-id="306cf-169">A Media Services szolgáltatásban láncolt feladatok sorozatát hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="306cf-169">In Media Services, you can create a series of chained tasks.</span></span> <span data-ttu-id="306cf-170">Minden tevékenység különböző feldolgozó lépéseket hajtja végre, és különböző media processzorok használhatja.</span><span class="sxs-lookup"><span data-stu-id="306cf-170">Each task performs different processing steps and can use different media processors.</span></span> <span data-ttu-id="306cf-171">hello kapcsolt feladatok is egy eszköz kiosztják a feladat tooanother hello eszköz lineáris sorozatát feladatok végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="306cf-171">hello chained tasks can hand off an asset from one task tooanother, performing a linear sequence of tasks on hello asset.</span></span> <span data-ttu-id="306cf-172">Azonban végrehajtott feladatokban hello feladatokat is nem szükséges toobe sorrendje egy sorozatban.</span><span class="sxs-lookup"><span data-stu-id="306cf-172">However, hello tasks performed in a job are not required toobe in a sequence.</span></span> <span data-ttu-id="306cf-173">Láncolt feladatot hoz létre, amikor hello kapcsolt **ITask** objektumok létrehozásakor egyetlen **IJob** objektum.</span><span class="sxs-lookup"><span data-stu-id="306cf-173">When you create a chained task, hello chained **ITask** objects are created in a single **IJob** object.</span></span>

> [!NOTE]
> <span data-ttu-id="306cf-174">Jelenleg legfeljebb 30 tevékenységek maximális száma feladat.</span><span class="sxs-lookup"><span data-stu-id="306cf-174">There is currently a limit of 30 tasks per job.</span></span> <span data-ttu-id="306cf-175">Ha több mint 30 feladatok kell toochain, hozzon létre több feladat toocontain hello feladatokat.</span><span class="sxs-lookup"><span data-stu-id="306cf-175">If you need toochain more than 30 tasks, create more than one job toocontain hello tasks.</span></span>
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a><span data-ttu-id="306cf-176">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="306cf-176">Considerations</span></span>
<span data-ttu-id="306cf-177">tooenable feladat láncolás:</span><span class="sxs-lookup"><span data-stu-id="306cf-177">tooenable task chaining:</span></span>

* <span data-ttu-id="306cf-178">Egy feladat legalább két tevékenységet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="306cf-178">A job must have at least two tasks.</span></span>
* <span data-ttu-id="306cf-179">Legalább egy tevékenységet, amelynek a bemeneti érték egy másik feladat hello feladat kimenetének hello kell lennie.</span><span class="sxs-lookup"><span data-stu-id="306cf-179">There must be at least one task whose input is hello output of another task in hello job.</span></span>

## <a name="use-odata-batch-processing"></a><span data-ttu-id="306cf-180">Használhatja a kötegelt feldolgozást OData</span><span class="sxs-lookup"><span data-stu-id="306cf-180">Use OData batch processing</span></span>
<span data-ttu-id="306cf-181">hello a következő példa bemutatja, hogyan toouse OData a batch-feldolgozási toocreate egy feladatot, és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="306cf-181">hello following example shows how toouse OData batch processing toocreate a job and tasks.</span></span> <span data-ttu-id="306cf-182">Kötegfeldolgozási információkért lásd: [nyitott Data (OData) protokollnak kötegelt feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="306cf-182">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a><span data-ttu-id="306cf-183">Hozzon létre egy feladatot a JobTemplate használatával</span><span class="sxs-lookup"><span data-stu-id="306cf-183">Create a job by using a JobTemplate</span></span>
<span data-ttu-id="306cf-184">Ha feldolgozni a több eszköz használatával gyakori feladatokat, a JobTemplate toospecify hello alapértelmezett feladat készletek használata vagy a feladatok tooset hello sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="306cf-184">When you process multiple assets by using a common set of tasks, use a JobTemplate toospecify hello default task presets, or tooset hello order of tasks.</span></span>

<span data-ttu-id="306cf-185">hello a következő példa bemutatja, hogyan toocreate egy JobTemplate egy TaskTemplate, amely a soron belül definiált.</span><span class="sxs-lookup"><span data-stu-id="306cf-185">hello following example shows how toocreate a JobTemplate with a TaskTemplate that is defined inline.</span></span> <span data-ttu-id="306cf-186">hello TaskTemplate Media Encoder Standard hello hello MediaProcessor tooencode hello eszköz fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="306cf-186">hello TaskTemplate uses hello Media Encoder Standard as hello MediaProcessor tooencode hello asset file.</span></span> <span data-ttu-id="306cf-187">Azonban más MediaProcessors használható is.</span><span class="sxs-lookup"><span data-stu-id="306cf-187">However, other MediaProcessors can be used as well.</span></span>

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> <span data-ttu-id="306cf-188">Egyéb Media Services entitások, ellentétben minden TaskTemplate egy új GUID azonosító megadása és elhelyezheti a kérelem törzsében szereplő hello taskTemplateId és azonosító tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="306cf-188">Unlike other Media Services entities, you must define a new GUID identifier for each TaskTemplate and place it in hello taskTemplateId and Id property in your request body.</span></span> <span data-ttu-id="306cf-189">hello tartalom azonosítási séma hajtsa végre az Azure Media Services entitások azonosítása ismertetett hello séma.</span><span class="sxs-lookup"><span data-stu-id="306cf-189">hello content identification scheme must follow hello scheme described in Identify Azure Media Services Entities.</span></span> <span data-ttu-id="306cf-190">JobTemplates is, nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="306cf-190">Also, JobTemplates cannot be updated.</span></span> <span data-ttu-id="306cf-191">Ehelyett hozzon létre egy újat a frissített módosításokat.</span><span class="sxs-lookup"><span data-stu-id="306cf-191">Instead, you must create a new one with your updated changes.</span></span>
>
>

<span data-ttu-id="306cf-192">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="306cf-192">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .


<span data-ttu-id="306cf-193">a következő példa azt mutatja meg hogyan hello toocreate egy feladatot, amely JobTemplate azonosítóra hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="306cf-193">hello following example shows how toocreate a job that references a JobTemplate Id:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


<span data-ttu-id="306cf-194">Ha sikeres, a következő válasz hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="306cf-194">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a><span data-ttu-id="306cf-195">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="306cf-195">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="306cf-196">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="306cf-196">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="306cf-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="306cf-197">Next steps</span></span>
<span data-ttu-id="306cf-198">Most, hogy tudja, hogyan toocreate egy eszköz, a feladat tooencode: [hogyan toocheck feladat folyamatban van a Media Services](media-services-rest-check-job-progress.md).</span><span class="sxs-lookup"><span data-stu-id="306cf-198">Now that you know how toocreate a job tooencode an asset, see [How toocheck job progress with Media Services](media-services-rest-check-job-progress.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="306cf-199">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="306cf-199">See also</span></span>
[<span data-ttu-id="306cf-200">Media processzorok beolvasása</span><span class="sxs-lookup"><span data-stu-id="306cf-200">Get Media Processors</span></span>](media-services-rest-get-media-processor.md)
