---
title: "Egy Azure-objektum kódolása a Media Encoder Standard használatával hogyan |} Microsoft Docs"
description: "Útmutató: Media Encoder Standard segítségével az Azure Media Services media tartalom kódolása. Kódminták REST API-t használja."
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
ms.openlocfilehash: 796f3b5a4dd56a0160986600cbbcf38faf8add56
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a><span data-ttu-id="03c6b-104">Egy eszköz kódolással Media Encoder Standard használatával</span><span class="sxs-lookup"><span data-stu-id="03c6b-104">How to encode an asset by using Media Encoder Standard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="03c6b-105">.NET</span><span class="sxs-lookup"><span data-stu-id="03c6b-105">.NET</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [<span data-ttu-id="03c6b-106">REST</span><span class="sxs-lookup"><span data-stu-id="03c6b-106">REST</span></span>](media-services-rest-encode-asset.md)
> * [<span data-ttu-id="03c6b-107">Portal</span><span class="sxs-lookup"><span data-stu-id="03c6b-107">Portal</span></span>](media-services-portal-encode.md)
>
>

## <a name="overview"></a><span data-ttu-id="03c6b-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="03c6b-108">Overview</span></span>
<span data-ttu-id="03c6b-109">Digitális videót továbbíthasson az interneten, meg kell végezni a tartalom tömörítését.</span><span class="sxs-lookup"><span data-stu-id="03c6b-109">To deliver digital video over the Internet, you must compress the media.</span></span> <span data-ttu-id="03c6b-110">Digitális videofájlok nagyok, és lehet, hogy az interneten keresztül, vagy a felhasználók eszközökhöz megfelelően megjeleníthető túl nagy.</span><span class="sxs-lookup"><span data-stu-id="03c6b-110">Digital video files are large and may be too big to deliver over the Internet, or for your customers’ devices to display properly.</span></span> <span data-ttu-id="03c6b-111">Kódolás tömörítés videó és hang, így az ügyfelek megtekintheti a media során a rendszer.</span><span class="sxs-lookup"><span data-stu-id="03c6b-111">Encoding is the process of compressing video and audio so your customers can view your media.</span></span>

<span data-ttu-id="03c6b-112">Kódolási feladatok olyan Azure Media Services a leggyakrabban használt feldolgozási műveletek.</span><span class="sxs-lookup"><span data-stu-id="03c6b-112">Encoding jobs are one of the most common processing operations in Azure Media Services.</span></span> <span data-ttu-id="03c6b-113">A kódolási feladat a médiafájlokat alakítja át egy meghatározott kódolásból egy másikra.</span><span class="sxs-lookup"><span data-stu-id="03c6b-113">You create encoding jobs to convert media files from one encoding to another.</span></span> <span data-ttu-id="03c6b-114">Amikor kódolásához, a Media Services beépített kódoló (Media Encoder Standard) is használhatja.</span><span class="sxs-lookup"><span data-stu-id="03c6b-114">When you encode, you can use the Media Services built-in encoder (Media Encoder Standard).</span></span> <span data-ttu-id="03c6b-115">Egy Media Services partner által biztosított kódoló is használható.</span><span class="sxs-lookup"><span data-stu-id="03c6b-115">You can also use an encoder provided by a Media Services partner.</span></span> <span data-ttu-id="03c6b-116">Külső kódolókkal az Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="03c6b-116">Third-party encoders are available through the Azure Marketplace.</span></span> <span data-ttu-id="03c6b-117">Megadhatja a kódolási feladatokhoz a kódoló-készlet karakterláncokat használatával, vagy előre definiált konfigurációs fájlok részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="03c6b-117">You can specify the details of encoding tasks by using preset strings defined for your encoder, or by using preset configuration files.</span></span> <span data-ttu-id="03c6b-118">A készletek rendelkezésre álló típusú, olvassa el [feladat készletek a Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="03c6b-118">To see the types of presets that are available, see [Task Presets for Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="03c6b-119">Minden feladat elvégzéséhez kívánt feldolgozási típusától függően egy vagy több feladat is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="03c6b-119">Each job can have one or more tasks depending on the type of processing that you want to accomplish.</span></span> <span data-ttu-id="03c6b-120">A REST API használatával hozhat létre feladatokat és azok kapcsolódó feladatok az alábbi két módszer egyikével:</span><span class="sxs-lookup"><span data-stu-id="03c6b-120">Through the REST API, you can create jobs and their related tasks in one of two ways:</span></span>

* <span data-ttu-id="03c6b-121">Feladatok lehet meghatározott beágyazott a feladatok navigációs tulajdonság a feladat entitások keresztül.</span><span class="sxs-lookup"><span data-stu-id="03c6b-121">Tasks can be defined inline through the Tasks navigation property on Job entities.</span></span>
* <span data-ttu-id="03c6b-122">OData kötegfeldolgozási használja.</span><span class="sxs-lookup"><span data-stu-id="03c6b-122">Use OData batch processing.</span></span>

<span data-ttu-id="03c6b-123">Javasoljuk, hogy mindig kódolása egy adaptív sávszélességű MP4 állítsa be a forrásfájlokat, és majd a készlet át kell alakítania a kívánt formátum használatával [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-123">We recommend that you always encode your source files into an adaptive bitrate MP4 set, and then convert the set to the desired format by using [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="03c6b-124">Ha az kimeneti adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="03c6b-124">If your output asset is storage encrypted, you must configure the asset delivery policy.</span></span> <span data-ttu-id="03c6b-125">További információkért lásd: [objektumtovábbítási szabályzat konfigurálása](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-125">For more information, see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="03c6b-126">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="03c6b-126">Considerations</span></span>

<span data-ttu-id="03c6b-127">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="03c6b-127">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="03c6b-128">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-128">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

<span data-ttu-id="03c6b-129">Mielőtt media processzorok hivatkozik, győződjön meg arról, hogy rendelkezik-e a megfelelő adathordozót processzor azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="03c6b-129">Before you start referencing media processors, verify that you have the correct media processor ID.</span></span> <span data-ttu-id="03c6b-130">További információkért lásd: [media processzorok beolvasása](media-services-rest-get-media-processor.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-130">For more information, see [Get media processors](media-services-rest-get-media-processor.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="03c6b-131">Kapcsolódás a Media Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="03c6b-131">Connect to Media Services</span></span>

<span data-ttu-id="03c6b-132">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-132">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="03c6b-133">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="03c6b-133">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="03c6b-134">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="03c6b-134">You must make subsequent calls to the new URI.</span></span>

## <a name="create-a-job-with-a-single-encoding-task"></a><span data-ttu-id="03c6b-135">Hozzon létre egy feladatot egyetlen kódolási feladat</span><span class="sxs-lookup"><span data-stu-id="03c6b-135">Create a job with a single encoding task</span></span>
> [!NOTE]
> <span data-ttu-id="03c6b-136">Dolgozunk a Media Services REST API-t, ha a következők érvényesek:</span><span class="sxs-lookup"><span data-stu-id="03c6b-136">When you're working with the Media Services REST API, the following considerations apply:</span></span>
>
> <span data-ttu-id="03c6b-137">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="03c6b-137">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="03c6b-138">További információkért lásd: [Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-138">For more information, see [Setup for Media Services REST API development](media-services-rest-how-to-use.md).</span></span>
>
> <span data-ttu-id="03c6b-139">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="03c6b-139">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="03c6b-140">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="03c6b-140">You must make subsequent calls to the new URI.</span></span> <span data-ttu-id="03c6b-141">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-141">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
>
> <span data-ttu-id="03c6b-142">Ha JSON használatával és használandó megadásával a **__metadata** kulcsszó a kérés (például, hogy objektumra hivatkozik, csatolt), meg kell adni a **elfogadás** fejlécének [részletes JSON formátumban](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Fogadja el: az application/json; odata = részletes.</span><span class="sxs-lookup"><span data-stu-id="03c6b-142">When using JSON and specifying to use the **__metadata** keyword in the request (for example, to references a linked object), you must set the **Accept** header to [JSON Verbose format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accept: application/json;odata=verbose.</span></span>
>
>

<span data-ttu-id="03c6b-143">A következő példa bemutatja, hogyan hozhat létre, és indítsa el egy feladat az adott feloldási és minőségi videó kódolására beállítani egy feladat.</span><span class="sxs-lookup"><span data-stu-id="03c6b-143">The following example shows you how to create and post a job with one task set to encode a video at a specific resolution and quality.</span></span> <span data-ttu-id="03c6b-144">A Media Encoder Standard kódol, használhatja a megadott feladat konfigurációs készletek [Itt](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="03c6b-144">When you encode with Media Encoder Standard, you can use task configuration presets specified [here](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="03c6b-145">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="03c6b-145">Request:</span></span>

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

<span data-ttu-id="03c6b-146">Válasz:</span><span class="sxs-lookup"><span data-stu-id="03c6b-146">Response:</span></span>

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a><span data-ttu-id="03c6b-147">A kimeneti adategységen nevének megadása</span><span class="sxs-lookup"><span data-stu-id="03c6b-147">Set the output asset's name</span></span>
<span data-ttu-id="03c6b-148">A következő példa bemutatja, hogyan assetName attribútum:</span><span class="sxs-lookup"><span data-stu-id="03c6b-148">The following example shows how to set the assetName attribute:</span></span>

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a><span data-ttu-id="03c6b-149">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="03c6b-149">Considerations</span></span>
* <span data-ttu-id="03c6b-150">TaskBody tulajdonságok literális XML segítségével határozza meg a bemeneti számát, vagy a tevékenység által használt eszközök kimeneti.</span><span class="sxs-lookup"><span data-stu-id="03c6b-150">TaskBody properties must use literal XML to define the number of input, or output assets that are used by the task.</span></span> <span data-ttu-id="03c6b-151">A feladat a témakör az XML-Schema definíció az XML-fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="03c6b-151">The task topic contains the XML Schema Definition for the XML.</span></span>
* <span data-ttu-id="03c6b-152">A TaskBody definícióban minden belső értékének <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="03c6b-152">In the TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="03c6b-153">Kimeneti adategységek feladatonként.</span><span class="sxs-lookup"><span data-stu-id="03c6b-153">A task can have multiple output assets.</span></span> <span data-ttu-id="03c6b-154">Egy JobOutputAsset(x) csak egyszer használható egy feladatot a feladatok kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="03c6b-154">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="03c6b-155">A tevékenység bemeneti eszközként JobInputAsset vagy JobOutputAsset adhat meg.</span><span class="sxs-lookup"><span data-stu-id="03c6b-155">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="03c6b-156">Feladatok kell nem alkotnak hurkot.</span><span class="sxs-lookup"><span data-stu-id="03c6b-156">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="03c6b-157">Az érték paraméter jelzi, hogy átadta JobInputAsset vagy JobOutputAsset az eszközhöz tartozó index értékét jelöli.</span><span class="sxs-lookup"><span data-stu-id="03c6b-157">The value parameter that you pass to JobInputAsset or JobOutputAsset represents the index value for an asset.</span></span> <span data-ttu-id="03c6b-158">A tényleges eszközök entitás feladatdefinícióban InputMediaAssets és OutputMediaAssets navigációs tulajdonságok vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="03c6b-158">The actual assets are defined in the InputMediaAssets and OutputMediaAssets navigation properties on the job entity definition.</span></span>
* <span data-ttu-id="03c6b-159">A Media Services OData v3 épül, mert az egyes eszközök a InputMediaAssets és OutputMediaAssets navigációs tulajdonság gyűjteményeket hivatkozott keresztül a "__metadata: uri" név-érték pár.</span><span class="sxs-lookup"><span data-stu-id="03c6b-159">Because Media Services is built on OData v3, the individual assets in the InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
* <span data-ttu-id="03c6b-160">Egy vagy több olyan eszközt, a Media Services InputMediaAssets rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="03c6b-160">InputMediaAssets maps to one or more assets that you created in Media Services.</span></span> <span data-ttu-id="03c6b-161">A rendszer OutputMediaAssets hozza létre.</span><span class="sxs-lookup"><span data-stu-id="03c6b-161">OutputMediaAssets are created by the system.</span></span> <span data-ttu-id="03c6b-162">Egy meglévő eszköz nem hivatkozó.</span><span class="sxs-lookup"><span data-stu-id="03c6b-162">They don't reference an existing asset.</span></span>
* <span data-ttu-id="03c6b-163">OutputMediaAssets elnevezheti a assetName attribútum használatával.</span><span class="sxs-lookup"><span data-stu-id="03c6b-163">OutputMediaAssets can be named by using the assetName attribute.</span></span> <span data-ttu-id="03c6b-164">Ha ez az attribútum nincs jelen, akkor a OutputMediaAsset nevét bármilyen belső szöveges értékét a <outputAsset> eleme az előtagja pedig a feladat nevének értékét, vagy a feladat-azonosító értéke (abban az esetben, ha a Name tulajdonság nincs megadva).</span><span class="sxs-lookup"><span data-stu-id="03c6b-164">If this attribute is not present, then the name of the OutputMediaAsset is whatever the inner text value of the <outputAsset> element is with a suffix of either the Job Name value, or the Job Id value (in the case where the Name property isn't defined).</span></span> <span data-ttu-id="03c6b-165">Például ha assetName "Mintájának" értéket állítsa be, majd a OutputMediaAsset neve tulajdonság értéke "Minta."</span><span class="sxs-lookup"><span data-stu-id="03c6b-165">For example, if you set a value for assetName to "Sample," then the OutputMediaAsset Name property is set to "Sample."</span></span> <span data-ttu-id="03c6b-166">Azonban ha assetName értékének beállítása nem, de adta meg az "NewJob" a feladat nevét, majd a OutputMediaAsset név lesz "_NewJob JobOutputAsset (érték)."</span><span class="sxs-lookup"><span data-stu-id="03c6b-166">However, if you didn't set a value for assetName, but did set the job name to "NewJob," then the OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob."</span></span>

## <a name="create-a-job-with-chained-tasks"></a><span data-ttu-id="03c6b-167">Hozzon létre egy feladatot láncolt feladatok</span><span class="sxs-lookup"><span data-stu-id="03c6b-167">Create a job with chained tasks</span></span>
<span data-ttu-id="03c6b-168">Számos alkalmazás forgatókönyvben az fejlesztők kíván létrehozni a feldolgozási feladatok sorozata.</span><span class="sxs-lookup"><span data-stu-id="03c6b-168">In many application scenarios, developers want to create a series of processing tasks.</span></span> <span data-ttu-id="03c6b-169">A Media Services szolgáltatásban láncolt feladatok sorozatát hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="03c6b-169">In Media Services, you can create a series of chained tasks.</span></span> <span data-ttu-id="03c6b-170">Minden tevékenység különböző feldolgozó lépéseket hajtja végre, és különböző media processzorok használhatja.</span><span class="sxs-lookup"><span data-stu-id="03c6b-170">Each task performs different processing steps and can use different media processors.</span></span> <span data-ttu-id="03c6b-171">A láncolt feladatokat is kiosztják egy eszköz egy feladat a másikra, az eszköz lineáris sorozatát feladatok végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="03c6b-171">The chained tasks can hand off an asset from one task to another, performing a linear sequence of tasks on the asset.</span></span> <span data-ttu-id="03c6b-172">A feladatok a feladatokat azonban nem szükségesek sorrendben kell.</span><span class="sxs-lookup"><span data-stu-id="03c6b-172">However, the tasks performed in a job are not required to be in a sequence.</span></span> <span data-ttu-id="03c6b-173">Amikor a láncolt láncolt feladat létrehozásához **ITask** objektumok létrehozásakor egyetlen **IJob** objektum.</span><span class="sxs-lookup"><span data-stu-id="03c6b-173">When you create a chained task, the chained **ITask** objects are created in a single **IJob** object.</span></span>

> [!NOTE]
> <span data-ttu-id="03c6b-174">Jelenleg legfeljebb 30 tevékenységek maximális száma feladat.</span><span class="sxs-lookup"><span data-stu-id="03c6b-174">There is currently a limit of 30 tasks per job.</span></span> <span data-ttu-id="03c6b-175">Ha több mint 30 feladatok láncolt van szüksége, hozzon létre több feladatot a feladatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="03c6b-175">If you need to chain more than 30 tasks, create more than one job to contain the tasks.</span></span>
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


### <a name="considerations"></a><span data-ttu-id="03c6b-176">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="03c6b-176">Considerations</span></span>
<span data-ttu-id="03c6b-177">Ahhoz, hogy a feladat-láncolás:</span><span class="sxs-lookup"><span data-stu-id="03c6b-177">To enable task chaining:</span></span>

* <span data-ttu-id="03c6b-178">Egy feladat legalább két tevékenységet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="03c6b-178">A job must have at least two tasks.</span></span>
* <span data-ttu-id="03c6b-179">Legalább egy tevékenységet, amelynek a bemeneti érték a feladat egy másik feladat kimenetének kell lennie.</span><span class="sxs-lookup"><span data-stu-id="03c6b-179">There must be at least one task whose input is the output of another task in the job.</span></span>

## <a name="use-odata-batch-processing"></a><span data-ttu-id="03c6b-180">Használhatja a kötegelt feldolgozást OData</span><span class="sxs-lookup"><span data-stu-id="03c6b-180">Use OData batch processing</span></span>
<span data-ttu-id="03c6b-181">A következő példa bemutatja, hogyan OData kötegfeldolgozási használata a feladatok és a feladatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="03c6b-181">The following example shows how to use OData batch processing to create a job and tasks.</span></span> <span data-ttu-id="03c6b-182">Kötegfeldolgozási információkért lásd: [nyitott Data (OData) protokollnak kötegelt feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="03c6b-182">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

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



## <a name="create-a-job-by-using-a-jobtemplate"></a><span data-ttu-id="03c6b-183">Hozzon létre egy feladatot a JobTemplate használatával</span><span class="sxs-lookup"><span data-stu-id="03c6b-183">Create a job by using a JobTemplate</span></span>
<span data-ttu-id="03c6b-184">Több eszköz feladatok közös beállításkészletre segítségével dolgozza fel, ha a egy JobTemplate adhatja meg az alapértelmezett feladat készletek vagy feladatok sorrendjének beállításához.</span><span class="sxs-lookup"><span data-stu-id="03c6b-184">When you process multiple assets by using a common set of tasks, use a JobTemplate to specify the default task presets, or to set the order of tasks.</span></span>

<span data-ttu-id="03c6b-185">A következő példa bemutatja, hogyan hozzon létre egy JobTemplate egy TaskTemplate, amely meghatározott beágyazott.</span><span class="sxs-lookup"><span data-stu-id="03c6b-185">The following example shows how to create a JobTemplate with a TaskTemplate that is defined inline.</span></span> <span data-ttu-id="03c6b-186">A TaskTemplate használja a Media Encoder Standard a MediaProcessor kódolására az adategységfájlon.</span><span class="sxs-lookup"><span data-stu-id="03c6b-186">The TaskTemplate uses the Media Encoder Standard as the MediaProcessor to encode the asset file.</span></span> <span data-ttu-id="03c6b-187">Azonban más MediaProcessors használható is.</span><span class="sxs-lookup"><span data-stu-id="03c6b-187">However, other MediaProcessors can be used as well.</span></span>

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
> <span data-ttu-id="03c6b-188">Eltérően egyéb Media Services entitások egy új GUID azonosító megadása minden TaskTemplate és naplózza azt a taskTemplateId és az Id tulajdonság a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="03c6b-188">Unlike other Media Services entities, you must define a new GUID identifier for each TaskTemplate and place it in the taskTemplateId and Id property in your request body.</span></span> <span data-ttu-id="03c6b-189">A tartalom azonosítási séma hajtsa végre a rendszer az Azure Media Services entitások azonosítása leírt.</span><span class="sxs-lookup"><span data-stu-id="03c6b-189">The content identification scheme must follow the scheme described in Identify Azure Media Services Entities.</span></span> <span data-ttu-id="03c6b-190">JobTemplates is, nem lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="03c6b-190">Also, JobTemplates cannot be updated.</span></span> <span data-ttu-id="03c6b-191">Ehelyett hozzon létre egy újat a frissített módosításokat.</span><span class="sxs-lookup"><span data-stu-id="03c6b-191">Instead, you must create a new one with your updated changes.</span></span>
>
>

<span data-ttu-id="03c6b-192">Ha sikeres, a következő választ ad vissza:</span><span class="sxs-lookup"><span data-stu-id="03c6b-192">If successful, the following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .


<span data-ttu-id="03c6b-193">A következő példa bemutatja, hogyan hozzon létre egy feladatot, amely JobTemplate azonosítóra hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="03c6b-193">The following example shows how to create a job that references a JobTemplate Id:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


<span data-ttu-id="03c6b-194">Ha sikeres, a következő választ ad vissza:</span><span class="sxs-lookup"><span data-stu-id="03c6b-194">If successful, the following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a><span data-ttu-id="03c6b-195">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="03c6b-195">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="03c6b-196">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="03c6b-196">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="03c6b-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03c6b-197">Next steps</span></span>
<span data-ttu-id="03c6b-198">Most, hogy tudja, hogyan hozzon létre egy feladatot, amely egy eszköz kódolására, lásd: [ellenőrzése a feladat előrehaladását a Media Services](media-services-rest-check-job-progress.md).</span><span class="sxs-lookup"><span data-stu-id="03c6b-198">Now that you know how to create a job to encode an asset, see [How to check job progress with Media Services](media-services-rest-check-job-progress.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="03c6b-199">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="03c6b-199">See also</span></span>
[<span data-ttu-id="03c6b-200">Media processzorok beolvasása</span><span class="sxs-lookup"><span data-stu-id="03c6b-200">Get Media Processors</span></span>](media-services-rest-get-media-processor.md)
