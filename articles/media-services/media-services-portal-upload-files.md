---
title: "AAA\"hello Azure-portál használatával Media Services-fiók fájlokat feltöltés |} Microsoft dokumentumok\""
description: "Ez az oktatóanyag végigvezeti hello fájlok feltöltése a Media Services-fiók hello Azure-portál használatával"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 4ce1e133c72854532735ba7c72a43c92a75bc240
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-azure-portal"></a><span data-ttu-id="0bd92-103">Fájlok feltöltése a Media Services-fiók hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="0bd92-103">Upload files into a Media Services account using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0bd92-104">Portál</span><span class="sxs-lookup"><span data-stu-id="0bd92-104">Portal</span></span>](media-services-portal-upload-files.md)
> * [<span data-ttu-id="0bd92-105">.NET</span><span class="sxs-lookup"><span data-stu-id="0bd92-105">.NET</span></span>](media-services-dotnet-upload-files.md)
> * [<span data-ttu-id="0bd92-106">REST</span><span class="sxs-lookup"><span data-stu-id="0bd92-106">REST</span></span>](media-services-rest-upload-files.md)
> 
> [!NOTE]
> <span data-ttu-id="0bd92-107">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0bd92-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="0bd92-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0bd92-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 


<span data-ttu-id="0bd92-109">A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik.</span><span class="sxs-lookup"><span data-stu-id="0bd92-109">In Media Services, you upload your digital files into an asset.</span></span> <span data-ttu-id="0bd92-110">hello eszköz tartalmazhat videó, hang, képeket, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.) Miután hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.</span><span class="sxs-lookup"><span data-stu-id="0bd92-110">hello Asset  can contain video, audio, images, thumbnail collections, text tracks and closed caption files (and hello metadata about these files.) Once hello files are uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>


## <a name="upload-files"></a><span data-ttu-id="0bd92-111">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="0bd92-111">Upload files</span></span>

>[!NOTE]
><span data-ttu-id="0bd92-112">Nincs a Media Services feldolgozás támogatott korlátot toohello fájl maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="0bd92-112">There is a limit toohello maximum file size supported for processing in Media Services.</span></span> <span data-ttu-id="0bd92-113">Ellenőrizze a [ez](media-services-quotas-and-limitations.md) témakör hello méretű fájlt választhat vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="0bd92-113">Please see [this](media-services-quotas-and-limitations.md) topic for details about hello file size limitation.</span></span>
>

1. <span data-ttu-id="0bd92-114">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="0bd92-114">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="0bd92-115">A hello **beállítások** panelen kattintson a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="0bd92-115">On hello **Settings** blade, click **Assets**.</span></span>
   
    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. <span data-ttu-id="0bd92-117">Kattintson a hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0bd92-117">Click hello **Upload** button.</span></span>
   
    <span data-ttu-id="0bd92-118">Hello **töltse fel a video asset** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0bd92-118">hello **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0bd92-119">Tetszőleges méretű fájlt választhat.</span><span class="sxs-lookup"><span data-stu-id="0bd92-119">There is no file size limitation.</span></span>
   > 
   > 
4. <span data-ttu-id="0bd92-120">Keresse meg a szükséges toohello videót a számítógépen, válassza ki azt, és kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="0bd92-120">Browse toohello desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="0bd92-121">hello feltöltés elindul, és megtekintheti a hello fájlnév hello folyamatban.</span><span class="sxs-lookup"><span data-stu-id="0bd92-121">hello upload starts and you can see hello progress under hello file name.</span></span>  

<span data-ttu-id="0bd92-122">Hello feltöltése után látni fogja a hello új objektum bekerül hello **eszközök** ablak.</span><span class="sxs-lookup"><span data-stu-id="0bd92-122">Once hello upload completes, you will see hello new asset listed in hello **Assets** window.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0bd92-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0bd92-123">Next steps</span></span>
<span data-ttu-id="0bd92-124">Most már kódolhatja a feltöltött adategységeket.</span><span class="sxs-lookup"><span data-stu-id="0bd92-124">You can now encode your uploaded assets.</span></span> <span data-ttu-id="0bd92-125">További információ: [Encode Assets](media-services-portal-encode.md) (Adategységek kódolása).</span><span class="sxs-lookup"><span data-stu-id="0bd92-125">For more information, see [Encode assets](media-services-portal-encode.md).</span></span>

<span data-ttu-id="0bd92-126">Használhatja az Azure Functions tootrigger egy kódolási feladat, a konfigurált hello tároló érkező fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="0bd92-126">You can also use Azure Functions tootrigger an encoding job based on a file arriving in hello configured container.</span></span> <span data-ttu-id="0bd92-127">További információkért tekintse meg [ezt a mintát](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span><span class="sxs-lookup"><span data-stu-id="0bd92-127">For more information, see [this sample](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0bd92-128">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0bd92-128">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0bd92-129">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0bd92-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

