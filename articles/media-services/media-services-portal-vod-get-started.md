---
title: "Igény szerinti videótartalom-továbbítás az Azure Portal használatával | Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a lépéseken, amelyek segítségével alapszintű igény szerinti videotartalom-továbbítási szolgáltatást hozhat létre az Azure Portal segítségével, az Azure Media Services (AMS) alkalmazással."
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
ms.openlocfilehash: a8eeeeff412837acba14b441a3c590edf7e3597a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a><span data-ttu-id="0e3c7-103">Igény szerinti tartalomtovábbítás az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="0e3c7-103">Get started with delivering content on demand using the Azure portal</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="0e3c7-104">Ez az oktatóanyag végigvezeti a lépéseken, amelyek segítségével alapszintű igény szerinti videotartalom-továbbítási szolgáltatást hozhat létre az Azure Portal segítségével, az Azure Media Services (AMS) alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-104">This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e3c7-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0e3c7-105">Prerequisites</span></span>
<span data-ttu-id="0e3c7-106">Az ismertetett eljárás végrehajtásához a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-106">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="0e3c7-107">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-107">An Azure account.</span></span> <span data-ttu-id="0e3c7-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0e3c7-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="0e3c7-109">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-109">A Media Services account.</span></span> <span data-ttu-id="0e3c7-110">A Media Services-fiók létrehozásáról a [Media Services-fiók létrehozása](media-services-portal-create-account.md) című cikk nyújt tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-110">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="0e3c7-111">Az oktatóanyag a következő feladatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-111">This tutorial includes the following tasks:</span></span>

1. <span data-ttu-id="0e3c7-112">Indítsa el a streamvégpontot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-112">Start streaming endpoint.</span></span>
2. <span data-ttu-id="0e3c7-113">Videofájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="0e3c7-113">Upload a video file.</span></span>
3. <span data-ttu-id="0e3c7-114">Forrásfájl kódolása adaptív sávszélességű MP4-fájlokká</span><span class="sxs-lookup"><span data-stu-id="0e3c7-114">Encode the source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="0e3c7-115">Az objektum közzététele, majd a streamelési és a progresszív letöltési URL-cím lekérése</span><span class="sxs-lookup"><span data-stu-id="0e3c7-115">Publish the asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="0e3c7-116">Tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="0e3c7-116">Play your content.</span></span>

## <a name="start-streaming-endpoints"></a><span data-ttu-id="0e3c7-117">Streamvégpontok elindítása</span><span class="sxs-lookup"><span data-stu-id="0e3c7-117">Start streaming endpoints</span></span> 

<span data-ttu-id="0e3c7-118">Az Azure Media Services egyik leggyakrabban használt funkciója a videók továbbítása az adaptív sávszélességű streamelés használatával.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-118">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="0e3c7-119">A Media Services dinamikus csomagolást biztosít, amelynek köszönhetően adaptív sávszélességű, MP4 formátumban kódolt tartalmait a Media Services által támogatott streamformátumok valamelyikében (MPEG DASH, HLS, Smooth Streaming) továbbíthatja igény szerint, mindezt anélkül, hogy az adott formátumban előcsomagolt verziót tárolna.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-119">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="0e3c7-120">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-120">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="0e3c7-121">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-121">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 

<span data-ttu-id="0e3c7-122">A streamvégpont elindításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-122">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="0e3c7-123">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0e3c7-123">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0e3c7-124">Kattintson a Settings (Beállítások) ablak Streaming endpoints (Streamvégpontok) elemére.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-124">In the Settings window, click Streaming endpoints.</span></span> 
3. <span data-ttu-id="0e3c7-125">Kattintson az alapértelmezett streamvégpontra.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-125">Click the default streaming endpoint.</span></span> 

    <span data-ttu-id="0e3c7-126">Megjelenik a DEFAULT STREAMING ENDPOINT DETAILS (Alapértelmezett streamvégpont adatai) ablak.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-126">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="0e3c7-127">Kattintson a Start ikonra.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-127">Click the Start icon.</span></span>
5. <span data-ttu-id="0e3c7-128">Mentse a módosításokat a Save (Mentés) gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-128">Click the Save button to save your changes.</span></span>

## <a name="upload-files"></a><span data-ttu-id="0e3c7-129">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="0e3c7-129">Upload files</span></span>
<span data-ttu-id="0e3c7-130">Ha az Azure Media Services használatával kíván videókat streamelni, fel kell töltenie a forrásvideókat, különböző bitsebességekre kell kódolnia azokat, majd közzé kell tennie az eredményt.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-130">To stream videos using Azure Media Services, you need to upload the source videos, encode them into multiple bitrates, and publish the result.</span></span> <span data-ttu-id="0e3c7-131">Ez a rész a folyamat első lépését írja le.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-131">The first step is covered in this section.</span></span> 

1. <span data-ttu-id="0e3c7-132">A **Settings** (Beállítások) ablakban kattintson az **Assets** (Objektumok) elemre.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-132">In the **Setting** window, click **Assets**.</span></span>
   
    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. <span data-ttu-id="0e3c7-134">Kattintson az **Upload** (Feltöltés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-134">Click the **Upload** button.</span></span>
   
    <span data-ttu-id="0e3c7-135">Megjelenik az **Upload a video asset** (Videóobjektum feltöltése) ablak.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-135">The **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0e3c7-136">Tetszőleges méretű fájlt választhat.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-136">There is no file size limitation.</span></span>
   > 
   > 
3. <span data-ttu-id="0e3c7-137">Keresse meg a kívánt videót a számítógépen, jelölje ki, majd kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-137">Browse to the desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="0e3c7-138">Megkezdődik a feltöltés. A folyamat előrehaladását a fájlnév alatt követheti nyomon.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-138">The upload starts and you can see the progress under the file name.</span></span>  

<span data-ttu-id="0e3c7-139">A feltöltés befejezését követően az új objektum bekerül az **Objektumok** ablakban található listába.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-139">Once the upload completes, you see the new asset listed in the **Assets** window.</span></span> 

## <a name="encode-assets"></a><span data-ttu-id="0e3c7-140">Objektumok kódolása</span><span class="sxs-lookup"><span data-stu-id="0e3c7-140">Encode assets</span></span>

<span data-ttu-id="0e3c7-141">Az Azure Media Services egyik legnépszerűbb funkciója, amikor a portál használatával adaptív sávszélességű streamelést biztosítunk az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-141">When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="0e3c7-142">A Media Services a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming és MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-142">Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="0e3c7-143">A videók adaptív sávszélességű streameléséhez többszörös sávszélességű fájlokká kell kódolnia a forrásvideókat.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-143">To prepare your videos for adaptive bitrate streaming, you need to encode your source video into multi-bitrate files.</span></span> <span data-ttu-id="0e3c7-144">Javasoljuk, hogy a videók kódolásához használja a **Media Encoder Standard** kódolót.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-144">You should use the **Media Encoder Standard** encoder to encode your videos.</span></span>  

<span data-ttu-id="0e3c7-145">A Media Services dinamikus csomagolást is biztosít, aminek köszönhetően anélkül lehet MPEG DASH, HLS és Smooth Streaming formátumban közvetíteni többszörös sávszélességű MP4-streameket, hogy át kellene őket csomagolni ezekbe a streamformátumokba.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-145">Media Services also provides dynamic packaging, which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having to repackage into these streaming formats.</span></span> <span data-ttu-id="0e3c7-146">A dinamikus csomagolás használatával csak egyféle formátumban kell tárolnia a fájlokat és fizetnie azok alapján, a Media Services pedig az ügyfelek igényeihez igazodva hozza létre és továbbítja számukra a megfelelő választ.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-146">With dynamic packaging, you only need to store and pay for the files in single storage format and Media Services builds and serves the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="0e3c7-147">Annak érdekében, hogy kihasználhassa a dinamikus csomagolást, kódolja többszörös sávszélességű MP4-fájlokká a forrásfájlt (a kódolás lépéseit egy későbbi részben találja meg).</span><span class="sxs-lookup"><span data-stu-id="0e3c7-147">To take advantage of dynamic packaging, you need to encode your source file into a set of multi-bitrate MP4 files (the encoding steps are demonstrated later in this section).</span></span>

### <a name="to-use-the-portal-to-encode"></a><span data-ttu-id="0e3c7-148">Kódolás a portál használatával</span><span class="sxs-lookup"><span data-stu-id="0e3c7-148">To use the portal to encode</span></span>
<span data-ttu-id="0e3c7-149">Ebben a részben leírjuk, milyen lépéseket kell elvégeznie a tartalmaknak a Media Encoder Standard segítségével történő kódolásához.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-149">This section describes the steps you can take to encode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="0e3c7-150">A **Settings** (Beállítások) ablakban válassza az **Assets** (Objektumok) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-150">In the **Settings** window, select **Assets**.</span></span>  
2. <span data-ttu-id="0e3c7-151">Az **Assets** (Objektumok) ablakban válassza ki a kódolni kívánt objektumot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-151">In the **Assets** window, select the asset that you would like to encode.</span></span>
3. <span data-ttu-id="0e3c7-152">Nyomja meg az **Encode** (Kódolás) gombot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-152">Press the **Encode** button.</span></span>
4. <span data-ttu-id="0e3c7-153">Az **Encode an asset** (Objektum kódolása) ablakban válassza a „Media Encoder Standard” feldolgozóeszközt, és egy beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-153">In the **Encode an asset** window, select the "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="0e3c7-154">A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-154">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="0e3c7-155">Ha szeretné szabályozni, hogy a rendszer mely kódolási beállításkészletet használja, ne feledje: rendkívül fontos, hogy a bemeneti videóhoz leginkább megfelelő készletet válassza.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-155">If you plan to control which encoding preset is used, keep this in mind: it is important to select the preset that is most appropriate for your input video.</span></span> <span data-ttu-id="0e3c7-156">Ha például tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont, akkor használja a „H264 Multiple Bitrate 1080p” beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-156">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use the "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="0e3c7-157">Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-157">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="0e3c7-158">Az egyszerűbb kezelés érdekében lehetősége van módosítani a kimeneti objektum nevét, illetve a feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-158">For easier management, you have an option of editing the name of the output asset, and the name of the job.</span></span>
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. <span data-ttu-id="0e3c7-160">Nyomja meg a **Create** (Létrehozás) gombot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-160">Press **Create**.</span></span>

### <a name="monitor-encoding-job-progress"></a><span data-ttu-id="0e3c7-161">Kódolási feladatok előrehaladásának figyelése</span><span class="sxs-lookup"><span data-stu-id="0e3c7-161">Monitor encoding job progress</span></span>
<span data-ttu-id="0e3c7-162">A kódolási feladat előrehaladásának figyeléséhez kattintson az oldal tetején található **Settings** (Beállítások), majd a **Jobs** (Feladatok) elemre.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-162">To monitor the progress of the encoding job, click **Settings** (at the top of the page) and then select **Jobs**.</span></span>

![Feladatok](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a><span data-ttu-id="0e3c7-164">Tartalom közzététele</span><span class="sxs-lookup"><span data-stu-id="0e3c7-164">Publish content</span></span>
<span data-ttu-id="0e3c7-165">Ahhoz, hogy átadhassa a tartalmak streamelésére vagy letöltésére használható URL-címet a felhasználónak, először „közzé kell tennie” az objektumot. Ehhez létre kell hoznia egy lokátort.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-165">To provide your user with a  URL that can be used to stream or download your content, you first need to "publish" your asset by creating a locator.</span></span> <span data-ttu-id="0e3c7-166">Az objektumban található fájlokhoz a lokátorok biztosítanak hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-166">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="0e3c7-167">A Media Services két lokátortípust támogat:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-167">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="0e3c7-168">Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például MPEG DASH, HLS vagy Smooth Streaming adatok streameléséhez) használhatók.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-168">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, to stream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="0e3c7-169">A streamelési lokátorok létrehozásához az objektumnak tartalmaznia kell egy .ism-fájlt.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-169">To create a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="0e3c7-170">Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-170">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="0e3c7-171">A streamelési URL-cím formátuma alább látható. Ez Smooth Streaming-objektumok lejátszására használható.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-171">A streaming URL has the following format and you can use it to play Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="0e3c7-172">HLS-streamelési URL-cím létrehozásához fűzze hozzá a következőt az URL-címhez: (format=m3u8-aapl).</span><span class="sxs-lookup"><span data-stu-id="0e3c7-172">To build an HLS streaming URL, append (format=m3u8-aapl) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="0e3c7-173">MPEG DASH-streamelési URL-cím létrehozásához fűzze hozzá a következőt az URL-címhez: (format=mpd-time-csf).</span><span class="sxs-lookup"><span data-stu-id="0e3c7-173">To build an  MPEG DASH streaming URL, append (format=mpd-time-csf) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="0e3c7-174">Az SAS URL-cím formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-174">A SAS URL has the following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> <span data-ttu-id="0e3c7-175">2015 márciusa előtt a portál kétéves lejárati idővel hozta létre a lokátorokat.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-175">If you used the portal to create locators before March 2015, locators with a two-year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="0e3c7-176">A lokátor lejárati idejének módosításához használjon [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-t.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-176">To update an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="0e3c7-177">Az SAS-keresők lejárati dátumának frissítésekor az URL-cím is módosul.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-177">When you update the expiration date of a SAS locator, the URL changes.</span></span>

### <a name="to-use-the-portal-to-publish-an-asset"></a><span data-ttu-id="0e3c7-178">Az objektum portál segítségével történő közzététele</span><span class="sxs-lookup"><span data-stu-id="0e3c7-178">To use the portal to publish an asset</span></span>
<span data-ttu-id="0e3c7-179">Az objektumnak a portál segítségével történő közzétételéhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-179">To use the portal to publish an asset, do the following:</span></span>

1. <span data-ttu-id="0e3c7-180">Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-180">Select **Settings** > **Assets**.</span></span>
2. <span data-ttu-id="0e3c7-181">Válassza ki a közzétenni kívánt objektumot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-181">Select the asset that you want to publish.</span></span>
3. <span data-ttu-id="0e3c7-182">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-182">Click the **Publish** button.</span></span>
4. <span data-ttu-id="0e3c7-183">Válassza ki a lokátor típusát.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-183">Select the locator type.</span></span>
5. <span data-ttu-id="0e3c7-184">Nyomja meg az **Add** (Hozzáadás) gombot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-184">Press **Add**.</span></span>
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="0e3c7-186">Az URL-cím bekerül a **Közzétett URL-címek** listájába.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-186">The URL is added to the list of **Published URLs**.</span></span>

## <a name="play-content-from-the-portal"></a><span data-ttu-id="0e3c7-187">Tartalom lejátszása a portálról</span><span class="sxs-lookup"><span data-stu-id="0e3c7-187">Play content from the portal</span></span>
<span data-ttu-id="0e3c7-188">Az Azure Portalon talál egy tartalomlejátszót, amellyel tesztelheti a videót.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-188">The Azure portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="0e3c7-189">Kattintson a kívánt videóra, majd a **Lejátszás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-189">Click the desired video and then click the **Play** button.</span></span>

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="0e3c7-191">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="0e3c7-191">Some considerations apply:</span></span>

* <span data-ttu-id="0e3c7-192">A streamelés megkezdéséhez futtassa az **alapértelmezett** streamvégpontot.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-192">To begin streaming, start running the **default** streaming endpoint.</span></span>
* <span data-ttu-id="0e3c7-193">Ellenőrizze, hogy közzétette-e a videót.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-193">Make sure the video has been published.</span></span>
* <span data-ttu-id="0e3c7-194">A **Media Player** az alapértelmezett streamvégpontból játssza le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-194">This **Media player** plays from the default streaming endpoint.</span></span> <span data-ttu-id="0e3c7-195">Ha egy nem alapértelmezett streamvégpontból szeretne lejátszani valamit, rákattintva másolja az URL-címet, és használjon másik lejátszót.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-195">If you want to play from a non-default streaming endpoint, click to copy the URL and use another player.</span></span> <span data-ttu-id="0e3c7-196">Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="0e3c7-196">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e3c7-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e3c7-197">Next steps</span></span>
<span data-ttu-id="0e3c7-198">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="0e3c7-198">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0e3c7-199">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0e3c7-199">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

