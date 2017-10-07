---
title: "aaaGet használatába kézbesítéséhez VoD hello Azure-portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello egy alapszintű Video-on-Demand (VoD) tartalomtovábbító service végrehajtási hello Azure-portál használatával Azure Media Services (AMS) alkalmazással."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 5c1c1b1f74ec1f1301120fe8e5a5ae183fe0338f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-hello-azure-portal"></a><span data-ttu-id="079b7-103">Ismerkedés az Azure-portálon hello segítségével igény szerinti tartalomtovábbítás</span><span class="sxs-lookup"><span data-stu-id="079b7-103">Get started with delivering content on demand using hello Azure portal</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="079b7-104">Ez az oktatóanyag végigvezeti hello egy alapszintű Video-on-Demand (VoD) tartalomtovábbító service végrehajtási hello Azure-portál használatával Azure Media Services (AMS) alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="079b7-104">This tutorial walks you through hello steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="079b7-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="079b7-105">Prerequisites</span></span>
<span data-ttu-id="079b7-106">Az alábbiakban hello szükséges toocomplete hello oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="079b7-106">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="079b7-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="079b7-107">An Azure account.</span></span> <span data-ttu-id="079b7-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="079b7-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="079b7-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="079b7-109">A Media Services account.</span></span> <span data-ttu-id="079b7-110">egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="079b7-110">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="079b7-111">Ez az oktatóanyag hello a következő feladatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="079b7-111">This tutorial includes hello following tasks:</span></span>

1. <span data-ttu-id="079b7-112">Indítsa el a streamvégpontot.</span><span class="sxs-lookup"><span data-stu-id="079b7-112">Start streaming endpoint.</span></span>
2. <span data-ttu-id="079b7-113">Videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="079b7-113">Upload a video file.</span></span>
3. <span data-ttu-id="079b7-114">Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlokat alakítja.</span><span class="sxs-lookup"><span data-stu-id="079b7-114">Encode hello source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="079b7-115">Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="079b7-115">Publish hello asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="079b7-116">Tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="079b7-116">Play your content.</span></span>

## <a name="start-streaming-endpoints"></a><span data-ttu-id="079b7-117">Streamvégpontok elindítása</span><span class="sxs-lookup"><span data-stu-id="079b7-117">Start streaming endpoints</span></span> 

<span data-ttu-id="079b7-118">Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett használatával adatfolyam adaptív sávszélességű streamelést működik.</span><span class="sxs-lookup"><span data-stu-id="079b7-118">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="079b7-119">A Media Services dinamikus becsomagolást biztosít, amely lehetővé teszi toodeliver az adaptív sávszélességű MP4-kódolású tartalmak anélkül, hogy előre csomagolt toostore (MPEG DASH, HLS, Smooth Streaming), a Media Services just-in-time, által támogatott streamformátumok Ezekbe a formátumokba egyes verzióit.</span><span class="sxs-lookup"><span data-stu-id="079b7-119">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="079b7-120">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="079b7-120">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="079b7-121">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="079b7-121">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

<span data-ttu-id="079b7-122">toostart hello streamvégpontra, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="079b7-122">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="079b7-123">Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="079b7-123">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="079b7-124">Hello-beállítások ablakában kattintson a Streaming végpontok.</span><span class="sxs-lookup"><span data-stu-id="079b7-124">In hello Settings window, click Streaming endpoints.</span></span> 
3. <span data-ttu-id="079b7-125">Kattintson a hello alapértelmezett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="079b7-125">Click hello default streaming endpoint.</span></span> 

    <span data-ttu-id="079b7-126">hello alapértelmezett STREAMING ENDPOINT részletek ablak.</span><span class="sxs-lookup"><span data-stu-id="079b7-126">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="079b7-127">Kattintson a hello Start ikonra.</span><span class="sxs-lookup"><span data-stu-id="079b7-127">Click hello Start icon.</span></span>
5. <span data-ttu-id="079b7-128">Kattintson a hello Mentés gombra toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="079b7-128">Click hello Save button toosave your changes.</span></span>

## <a name="upload-files"></a><span data-ttu-id="079b7-129">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="079b7-129">Upload files</span></span>
<span data-ttu-id="079b7-130">toostream tooupload hello forrás videók, kell a videók Azure Media Services használatával, kódolás több bitrates be, és tegye közzé a hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="079b7-130">toostream videos using Azure Media Services, you need tooupload hello source videos, encode them into multiple bitrates, and publish hello result.</span></span> <span data-ttu-id="079b7-131">Ebben a szakaszban foglalt hello első lépése.</span><span class="sxs-lookup"><span data-stu-id="079b7-131">hello first step is covered in this section.</span></span> 

1. <span data-ttu-id="079b7-132">A hello **beállítás** ablak, kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="079b7-132">In hello **Setting** window, click **Assets**.</span></span>
   
    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. <span data-ttu-id="079b7-134">Kattintson a hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="079b7-134">Click hello **Upload** button.</span></span>
   
    <span data-ttu-id="079b7-135">Hello **töltse fel a video asset** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="079b7-135">hello **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="079b7-136">Tetszőleges méretű fájlt választhat.</span><span class="sxs-lookup"><span data-stu-id="079b7-136">There is no file size limitation.</span></span>
   > 
   > 
3. <span data-ttu-id="079b7-137">Keresse meg a szükséges toohello videót a számítógépen, válassza ki azt, és kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="079b7-137">Browse toohello desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="079b7-138">hello feltöltés elindul, és megtekintheti a hello fájlnév hello folyamatban.</span><span class="sxs-lookup"><span data-stu-id="079b7-138">hello upload starts and you can see hello progress under hello file name.</span></span>  

<span data-ttu-id="079b7-139">Hello feltöltés befejezését követően megjelenik-e hello új objektum bekerül hello **eszközök** ablak.</span><span class="sxs-lookup"><span data-stu-id="079b7-139">Once hello upload completes, you see hello new asset listed in hello **Assets** window.</span></span> 

## <a name="encode-assets"></a><span data-ttu-id="079b7-140">Objektumok kódolása</span><span class="sxs-lookup"><span data-stu-id="079b7-140">Encode assets</span></span>

<span data-ttu-id="079b7-141">Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett az adaptív sávszélességű streamelési tooyour ügyfelek működik.</span><span class="sxs-lookup"><span data-stu-id="079b7-141">When working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="079b7-142">Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="079b7-142">Media Services supports hello following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="079b7-143">tooprepare videók adaptív sávszélességű streamelés esetén meg kell tooencode a forrás videó többszörös sávszélességű fájlokba.</span><span class="sxs-lookup"><span data-stu-id="079b7-143">tooprepare your videos for adaptive bitrate streaming, you need tooencode your source video into multi-bitrate files.</span></span> <span data-ttu-id="079b7-144">Használjon hello **Media Encoder Standard** kódoló tooencode videók.</span><span class="sxs-lookup"><span data-stu-id="079b7-144">You should use hello **Media Encoder Standard** encoder tooencode your videos.</span></span>  

<span data-ttu-id="079b7-145">Media Services dinamikus becsomagolást is biztosít, amely lehetővé teszi toodeliver közvetíteni többszörös sávszélességű MP4 a hello a következő formátumban: MPEG DASH, HLS, Smooth Streaming, anélkül, hogy az ezekbe a formátumokba toorepackage.</span><span class="sxs-lookup"><span data-stu-id="079b7-145">Media Services also provides dynamic packaging, which allows you toodeliver your multi-bitrate MP4s in hello following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having toorepackage into these streaming formats.</span></span> <span data-ttu-id="079b7-146">A dinamikus csomagolás csak akkor kell toostore és hello fájlokat egyetlen tárolási formátumban és a Media Services fizetési alapszik, és betölti az ügyféltől érkező kérésnek megfelelő választ hello.</span><span class="sxs-lookup"><span data-stu-id="079b7-146">With dynamic packaging, you only need toostore and pay for hello files in single storage format and Media Services builds and serves hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="079b7-147">tootake előny dinamikus becsomagolás kell tooencode a forrásfájlt (hello kódolás lépéseit egy későbbi részében) többszörös sávszélességű MP4-fájlokat alakítja.</span><span class="sxs-lookup"><span data-stu-id="079b7-147">tootake advantage of dynamic packaging, you need tooencode your source file into a set of multi-bitrate MP4 files (hello encoding steps are demonstrated later in this section).</span></span>

### <a name="toouse-hello-portal-tooencode"></a><span data-ttu-id="079b7-148">toouse hello portál tooencode</span><span class="sxs-lookup"><span data-stu-id="079b7-148">toouse hello portal tooencode</span></span>
<span data-ttu-id="079b7-149">Ez a szakasz ismerteti a hello lépéseket, amelyek tooencode a tartalmaknak a Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="079b7-149">This section describes hello steps you can take tooencode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="079b7-150">A hello **beállítások** ablakban válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="079b7-150">In hello **Settings** window, select **Assets**.</span></span>  
2. <span data-ttu-id="079b7-151">A hello **eszközök** ablakot, hogy szeretné-e tooencode válassza hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="079b7-151">In hello **Assets** window, select hello asset that you would like tooencode.</span></span>
3. <span data-ttu-id="079b7-152">Nyomja le az hello **Encode** gombra.</span><span class="sxs-lookup"><span data-stu-id="079b7-152">Press hello **Encode** button.</span></span>
4. <span data-ttu-id="079b7-153">A hello **kódolása egy eszköz** ablak, jelölje be hello "Media Encoder Standard" feldolgozóeszközt, és egy beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="079b7-153">In hello **Encode an asset** window, select hello "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="079b7-154">A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket.</span><span class="sxs-lookup"><span data-stu-id="079b7-154">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="079b7-155">Ha azt tervezi, hogy mely kódolási beállításkészlet használt toocontrol, ezt tartsa szem előtt: fontos tooselect hello készlet, amely a legjobban megfelelő a bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="079b7-155">If you plan toocontrol which encoding preset is used, keep this in mind: it is important tooselect hello preset that is most appropriate for your input video.</span></span> <span data-ttu-id="079b7-156">Például ha tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont rendelkezik, akkor használja hello "H264 Multiple Bitrate 1080p" beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="079b7-156">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use hello "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="079b7-157">Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="079b7-157">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="079b7-158">Az egyszerűbb kezelés érdekében lehetősége van a szerkesztési hello hello kimeneti eszköz, és hello nevét hello feladat.</span><span class="sxs-lookup"><span data-stu-id="079b7-158">For easier management, you have an option of editing hello name of hello output asset, and hello name of hello job.</span></span>
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. <span data-ttu-id="079b7-160">Nyomja meg a **Create** (Létrehozás) gombot.</span><span class="sxs-lookup"><span data-stu-id="079b7-160">Press **Create**.</span></span>

### <a name="monitor-encoding-job-progress"></a><span data-ttu-id="079b7-161">Kódolási feladatok előrehaladásának figyelése</span><span class="sxs-lookup"><span data-stu-id="079b7-161">Monitor encoding job progress</span></span>
<span data-ttu-id="079b7-162">feladat, kódolás hello toomonitor hello előrehaladását kattintson **beállítások** (hello hello oldal tetején), és válassza **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="079b7-162">toomonitor hello progress of hello encoding job, click **Settings** (at hello top of hello page) and then select **Jobs**.</span></span>

![Feladatok](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a><span data-ttu-id="079b7-164">Tartalom közzététele</span><span class="sxs-lookup"><span data-stu-id="079b7-164">Publish content</span></span>
<span data-ttu-id="079b7-165">tooprovide a felhasználó egy URL-cím használt toostream vagy letölteni a tartalmat, akkor először kell túl "közzététele" az eszköz egy kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="079b7-165">tooprovide your user with a  URL that can be used toostream or download your content, you first need too"publish" your asset by creating a locator.</span></span> <span data-ttu-id="079b7-166">Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel.</span><span class="sxs-lookup"><span data-stu-id="079b7-166">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="079b7-167">A Media Services két lokátortípust támogat:</span><span class="sxs-lookup"><span data-stu-id="079b7-167">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="079b7-168">Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például toostream MPEG DASH, HLS vagy Smooth Streaming) használt.</span><span class="sxs-lookup"><span data-stu-id="079b7-168">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, toostream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="079b7-169">toocreate a streamelési lokátorok létrehozásához az eszköz tartalmaznia kell egy .ism-fájlt.</span><span class="sxs-lookup"><span data-stu-id="079b7-169">toocreate a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="079b7-170">Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.</span><span class="sxs-lookup"><span data-stu-id="079b7-170">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="079b7-171">A streamelési URL-cím formátuma a következő hello rendelkezik, és használata tooplay Smooth Streaming-objektumok.</span><span class="sxs-lookup"><span data-stu-id="079b7-171">A streaming URL has hello following format and you can use it tooplay Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="079b7-172">toobuild HLS-streamelési URL-cím, egy hozzáfűző (formátum = m3u8-aapl) toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="079b7-172">toobuild an HLS streaming URL, append (format=m3u8-aapl) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="079b7-173">egy streamelési URL-cím, MPEG DASH toobuild hozzáfűzése (formátum mpd-idő-csf =) toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="079b7-173">toobuild an  MPEG DASH streaming URL, append (format=mpd-time-csf) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="079b7-174">Egy SAS URL-címnek a következő formátumban hello.</span><span class="sxs-lookup"><span data-stu-id="079b7-174">A SAS URL has hello following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> <span data-ttu-id="079b7-175">Ha korábban használt hello portál toocreate keresők 2015. márciusi, keresők kétéves lejárati dátummal jöttek létre.</span><span class="sxs-lookup"><span data-stu-id="079b7-175">If you used hello portal toocreate locators before March 2015, locators with a two-year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="079b7-176">a lokátor, használja a lejárati dátum tooupdate [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-k.</span><span class="sxs-lookup"><span data-stu-id="079b7-176">tooupdate an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="079b7-177">Amikor frissíti egy SAS-kereső hello lejárati dátuma, hello URL-cím módosítja.</span><span class="sxs-lookup"><span data-stu-id="079b7-177">When you update hello expiration date of a SAS locator, hello URL changes.</span></span>

### <a name="toouse-hello-portal-toopublish-an-asset"></a><span data-ttu-id="079b7-178">toouse hello portál toopublish egy eszköz</span><span class="sxs-lookup"><span data-stu-id="079b7-178">toouse hello portal toopublish an asset</span></span>
<span data-ttu-id="079b7-179">toouse hello portál toopublish egy eszköz hello a következő:</span><span class="sxs-lookup"><span data-stu-id="079b7-179">toouse hello portal toopublish an asset, do hello following:</span></span>

1. <span data-ttu-id="079b7-180">Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="079b7-180">Select **Settings** > **Assets**.</span></span>
2. <span data-ttu-id="079b7-181">Válassza ki a megjeleníteni kívánt toopublish hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="079b7-181">Select hello asset that you want toopublish.</span></span>
3. <span data-ttu-id="079b7-182">Kattintson a hello **közzététel** gombra.</span><span class="sxs-lookup"><span data-stu-id="079b7-182">Click hello **Publish** button.</span></span>
4. <span data-ttu-id="079b7-183">Válassza ki a hello lokátor típusát.</span><span class="sxs-lookup"><span data-stu-id="079b7-183">Select hello locator type.</span></span>
5. <span data-ttu-id="079b7-184">Nyomja meg az **Add** (Hozzáadás) gombot.</span><span class="sxs-lookup"><span data-stu-id="079b7-184">Press **Add**.</span></span>
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="079b7-186">hello URL-cím kerül toohello listája **közzétett URL-címek**.</span><span class="sxs-lookup"><span data-stu-id="079b7-186">hello URL is added toohello list of **Published URLs**.</span></span>

## <a name="play-content-from-hello-portal"></a><span data-ttu-id="079b7-187">Hello portálról tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="079b7-187">Play content from hello portal</span></span>
<span data-ttu-id="079b7-188">hello Azure-portálon talál egy tartalomlejátszót használható tootest a videót.</span><span class="sxs-lookup"><span data-stu-id="079b7-188">hello Azure portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="079b7-189">Kattintson a kívánt hello videó, majd hello **lejátszása** gombra.</span><span class="sxs-lookup"><span data-stu-id="079b7-189">Click hello desired video and then click hello **Play** button.</span></span>

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="079b7-191">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="079b7-191">Some considerations apply:</span></span>

* <span data-ttu-id="079b7-192">adatfolyam-, toobegin start futó hello **alapértelmezett** streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="079b7-192">toobegin streaming, start running hello **default** streaming endpoint.</span></span>
* <span data-ttu-id="079b7-193">Ellenőrizze, hogy a videó hello is közzé lett téve.</span><span class="sxs-lookup"><span data-stu-id="079b7-193">Make sure hello video has been published.</span></span>
* <span data-ttu-id="079b7-194">Ez **Media player** hello alapértelmezett streamvégpontból játssza le.</span><span class="sxs-lookup"><span data-stu-id="079b7-194">This **Media player** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="079b7-195">Ha azt szeretné, hogy a nem alapértelmezett tooplay az adatfolyam-továbbítási végpontra, kattintson a toocopy hello URL-címet, és használjon másik lejátszót.</span><span class="sxs-lookup"><span data-stu-id="079b7-195">If you want tooplay from a non-default streaming endpoint, click toocopy hello URL and use another player.</span></span> <span data-ttu-id="079b7-196">Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="079b7-196">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="079b7-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="079b7-197">Next steps</span></span>
<span data-ttu-id="079b7-198">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="079b7-198">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="079b7-199">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="079b7-199">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

