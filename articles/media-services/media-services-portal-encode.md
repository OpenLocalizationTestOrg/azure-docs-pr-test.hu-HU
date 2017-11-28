---
title: "a hello Azure-portálon keresztül Media Encoder Standard aaaEncode |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello kódolási hello Azure-portálon a Media Encoder Standard keresztül."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 0d118bbbe1fa9f4ba0bfa3ea3b10fb541d1d6379
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-hello-azure-portal"></a><span data-ttu-id="0530e-103">Egy eszköz használata a Media Encoder Standard hello Azure-portálon kódolása</span><span class="sxs-lookup"><span data-stu-id="0530e-103">Encode an asset using Media Encoder Standard with hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="0530e-104">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0530e-104">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="0530e-105">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0530e-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="0530e-106">Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett az adaptív sávszélességű streamelési tooyour ügyfelek működik.</span><span class="sxs-lookup"><span data-stu-id="0530e-106">When working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="0530e-107">Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="0530e-107">Media Services supports hello following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="0530e-108">tooprepare videók adaptív sávszélességű streamelés esetén meg kell tooencode a forrás videó többszörös sávszélességű fájlokba.</span><span class="sxs-lookup"><span data-stu-id="0530e-108">tooprepare your videos for adaptive bitrate streaming, you need tooencode your source video into multi-bitrate files.</span></span> <span data-ttu-id="0530e-109">Használjon hello **Media Encoder Standard** kódoló tooencode videók.</span><span class="sxs-lookup"><span data-stu-id="0530e-109">You should use hello **Media Encoder Standard** encoder tooencode your videos.</span></span>  

<span data-ttu-id="0530e-110">A Media Services dinamikus becsomagolást is biztosít, amely lehetővé teszi a toodeliver közvetíteni többszörös sávszélességű MP4 a hello a következő formátumban: MPEG DASH, HLS, Smooth Streaming, anélkül, hogy toore-csomagját ezekbe a formátumokba.</span><span class="sxs-lookup"><span data-stu-id="0530e-110">Media Services also provides dynamic packaging which allows you toodeliver your multi-bitrate MP4s in hello following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having toore-package into these streaming formats.</span></span> <span data-ttu-id="0530e-111">A dinamikus csomagolás csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.</span><span class="sxs-lookup"><span data-stu-id="0530e-111">With dynamic packaging you only need toostore and pay for hello files in single storage format and Media Services will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="0530e-112">tootake előny dinamikus becsomagolás kell tooencode a forrásfájlt (hello kódolás lépéseit egy későbbi részében) többszörös sávszélességű MP4-fájlokat alakítja.</span><span class="sxs-lookup"><span data-stu-id="0530e-112">tootake advantage of dynamic packaging, you need tooencode your source file into a set of multi-bitrate MP4 files (hello encoding steps are demonstrated later in this section).</span></span>

<span data-ttu-id="0530e-113">tooscale media feldolgozása, lásd: [ez](media-services-portal-scale-media-processing.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="0530e-113">tooscale media processing, see [this](media-services-portal-scale-media-processing.md) topic.</span></span>

## <a name="encode-with-hello-azure-portal"></a><span data-ttu-id="0530e-114">Az Azure-portálon hello kódolása</span><span class="sxs-lookup"><span data-stu-id="0530e-114">Encode with hello Azure portal</span></span>
<span data-ttu-id="0530e-115">Ez a szakasz ismerteti a hello lépéseket, amelyek tooencode a tartalmaknak a Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="0530e-115">This section describes hello steps you can take tooencode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="0530e-116">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="0530e-116">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="0530e-117">A hello **beállítások** ablakban válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="0530e-117">In hello **Settings** window, select **Assets**.</span></span>  
3. <span data-ttu-id="0530e-118">A hello **eszközök** ablakot, hogy szeretné-e tooencode válassza hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="0530e-118">In hello **Assets** window, select hello asset that you would like tooencode.</span></span>
4. <span data-ttu-id="0530e-119">Nyomja le az hello **Encode** gombra.</span><span class="sxs-lookup"><span data-stu-id="0530e-119">Press hello **Encode** button.</span></span>
5. <span data-ttu-id="0530e-120">A hello **kódolása egy eszköz** ablak, jelölje be hello "Media Encoder Standard" feldolgozóeszközt, és egy beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="0530e-120">In hello **Encode an asset** window, select hello "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="0530e-121">A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket.</span><span class="sxs-lookup"><span data-stu-id="0530e-121">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="0530e-122">Ha azt tervezi, hogy mely kódolási beállításkészlet használt toocontrol, ezt tartsa szem előtt: fontos tooselect hello készlet, amely a legjobban megfelelő a bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="0530e-122">If you plan toocontrol which encoding preset is used, keep this in mind: it is important tooselect hello preset that is most appropriate for your input video.</span></span> <span data-ttu-id="0530e-123">Például ha tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont rendelkezik, akkor használja hello "H264 Multiple Bitrate 1080p" beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="0530e-123">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use hello "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="0530e-124">Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="0530e-124">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="0530e-125">Az egyszerűbb kezelés érdekében lehetősége van a szerkesztési hello hello kimeneti eszköz, és hello nevét hello feladat.</span><span class="sxs-lookup"><span data-stu-id="0530e-125">For easier management, you have an option of editing hello name of hello output asset, and hello name of hello job.</span></span>
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. <span data-ttu-id="0530e-127">Nyomja meg a **Create** (Létrehozás) gombot.</span><span class="sxs-lookup"><span data-stu-id="0530e-127">Press **Create**.</span></span>

## <a name="next-step"></a><span data-ttu-id="0530e-128">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="0530e-128">Next step</span></span>
<span data-ttu-id="0530e-129">Kódolási feladatok előrehaladásának hello Azure-portálon a figyelheti a [ez](media-services-portal-check-job-progress.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="0530e-129">You can monitor encoding job progress with hello Azure portal, as described in [this](media-services-portal-check-job-progress.md) article.</span></span>  

## <a name="media-services-learning-paths"></a><span data-ttu-id="0530e-130">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0530e-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0530e-131">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0530e-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

