---
title: "Az Azure portálon keresztül Media Encoder Standard kódolása |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a kódolást és az Azure portál Media Encoder Standard keresztül."
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
ms.openlocfilehash: a299245e285c4caa68988b184799cd6f4d13e080
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a><span data-ttu-id="10740-103">Az Azure portálon keresztül Media Encoder Standard kódolása</span><span class="sxs-lookup"><span data-stu-id="10740-103">Encode an asset using Media Encoder Standard with the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="10740-104">Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="10740-104">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="10740-105">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="10740-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="10740-106">Az Azure Media Services egyik legnépszerűbb funkciója, amikor a portál használatával adaptív sávszélességű streamelést biztosítunk az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="10740-106">When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="10740-107">A Media Services a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming és MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="10740-107">Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="10740-108">A videók adaptív sávszélességű streameléséhez többszörös sávszélességű fájlokká kell kódolnia a forrásvideókat.</span><span class="sxs-lookup"><span data-stu-id="10740-108">To prepare your videos for adaptive bitrate streaming, you need to encode your source video into multi-bitrate files.</span></span> <span data-ttu-id="10740-109">Javasoljuk, hogy a videók kódolásához használja a **Media Encoder Standard** kódolót.</span><span class="sxs-lookup"><span data-stu-id="10740-109">You should use the **Media Encoder Standard** encoder to encode your videos.</span></span>  

<span data-ttu-id="10740-110">A Media Services is biztosít a dinamikus csomagolás, amely lehetővé teszi, hogy a következő formátumban közvetíteni többszörös sávszélességű MP4: MPEG DASH, HLS, Smooth Streaming, anélkül, hogy át kellene őket csomagolni ezekbe a streamformátumokba.</span><span class="sxs-lookup"><span data-stu-id="10740-110">Media Services also provides dynamic packaging which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having to re-package into these streaming formats.</span></span> <span data-ttu-id="10740-111">A dinamikus becsomagolás révén elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ.</span><span class="sxs-lookup"><span data-stu-id="10740-111">With dynamic packaging you only need to store and pay for the files in single storage format and Media Services will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="10740-112">Annak érdekében, hogy kihasználhassa a dinamikus csomagolást, kódolja többszörös sávszélességű MP4-fájlokká a forrásfájlt (a kódolás lépéseit egy későbbi részben találja meg).</span><span class="sxs-lookup"><span data-stu-id="10740-112">To take advantage of dynamic packaging, you need to encode your source file into a set of multi-bitrate MP4 files (the encoding steps are demonstrated later in this section).</span></span>

<span data-ttu-id="10740-113">Media feldolgozási méretezési, lásd: [ez](media-services-portal-scale-media-processing.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="10740-113">To scale media processing, see [this](media-services-portal-scale-media-processing.md) topic.</span></span>

## <a name="encode-with-the-azure-portal"></a><span data-ttu-id="10740-114">Az Azure portálon kódolása</span><span class="sxs-lookup"><span data-stu-id="10740-114">Encode with the Azure portal</span></span>
<span data-ttu-id="10740-115">Ebben a részben leírjuk, milyen lépéseket kell elvégeznie a tartalmaknak a Media Encoder Standard segítségével történő kódolásához.</span><span class="sxs-lookup"><span data-stu-id="10740-115">This section describes the steps you can take to encode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="10740-116">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="10740-116">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="10740-117">A **Settings** (Beállítások) ablakban válassza az **Assets** (Objektumok) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="10740-117">In the **Settings** window, select **Assets**.</span></span>  
3. <span data-ttu-id="10740-118">Az **Assets** (Objektumok) ablakban válassza ki a kódolni kívánt objektumot.</span><span class="sxs-lookup"><span data-stu-id="10740-118">In the **Assets** window, select the asset that you would like to encode.</span></span>
4. <span data-ttu-id="10740-119">Nyomja meg az **Encode** (Kódolás) gombot.</span><span class="sxs-lookup"><span data-stu-id="10740-119">Press the **Encode** button.</span></span>
5. <span data-ttu-id="10740-120">Az **Encode an asset** (Objektum kódolása) ablakban válassza a „Media Encoder Standard” feldolgozóeszközt, és egy beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="10740-120">In the **Encode an asset** window, select the "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="10740-121">A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket.</span><span class="sxs-lookup"><span data-stu-id="10740-121">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="10740-122">Ha szeretné szabályozni, hogy a rendszer mely kódolási beállításkészletet használja, ne feledje: rendkívül fontos, hogy a bemeneti videóhoz leginkább megfelelő készletet válassza.</span><span class="sxs-lookup"><span data-stu-id="10740-122">If you plan to control which encoding preset is used, keep this in mind: it is important to select the preset that is most appropriate for your input video.</span></span> <span data-ttu-id="10740-123">Ha például tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont, akkor használja a „H264 Multiple Bitrate 1080p” beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="10740-123">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use the "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="10740-124">Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.</span><span class="sxs-lookup"><span data-stu-id="10740-124">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="10740-125">Az egyszerűbb kezelés érdekében lehetősége van módosítani a kimeneti objektum nevét, illetve a feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="10740-125">For easier management, you have an option of editing the name of the output asset, and the name of the job.</span></span>
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. <span data-ttu-id="10740-127">Nyomja meg a **Create** (Létrehozás) gombot.</span><span class="sxs-lookup"><span data-stu-id="10740-127">Press **Create**.</span></span>

## <a name="next-step"></a><span data-ttu-id="10740-128">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="10740-128">Next step</span></span>
<span data-ttu-id="10740-129">Kódolási feladatok előrehaladásának és az Azure portál, a figyelheti [ez](media-services-portal-check-job-progress.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="10740-129">You can monitor encoding job progress with the Azure portal, as described in [this](media-services-portal-check-job-progress.md) article.</span></span>  

## <a name="media-services-learning-paths"></a><span data-ttu-id="10740-130">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="10740-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="10740-131">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="10740-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

