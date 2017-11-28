---
title: "AAA\"hello Azure-portálon a tartalom közzététele |} Microsoft dokumentumok\""
description: "Ez az oktatóanyag végigvezeti hello közzétételi hello Azure-portálon a tartalmat."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a7a3867a6939b4b9da883176c6cc20c99d6c54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-content-with-hello-azure-portal"></a><span data-ttu-id="a11cf-103">Az Azure-portálon hello tartalom közzététele</span><span class="sxs-lookup"><span data-stu-id="a11cf-103">Publish content with hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a11cf-104">Portál</span><span class="sxs-lookup"><span data-stu-id="a11cf-104">Portal</span></span>](media-services-portal-publish.md)
> * [<span data-ttu-id="a11cf-105">.NET</span><span class="sxs-lookup"><span data-stu-id="a11cf-105">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="a11cf-106">REST</span><span class="sxs-lookup"><span data-stu-id="a11cf-106">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="a11cf-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a11cf-107">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="a11cf-108">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a11cf-108">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="a11cf-109">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a11cf-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="a11cf-110">tooprovide a felhasználó egy URL-cím használt toostream vagy letölteni a tartalmat, akkor először kell túl "közzététele" az eszköz egy kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="a11cf-110">tooprovide your user with a  URL that can be used toostream or download your content, you first need too"publish" your asset by creating a locator.</span></span> <span data-ttu-id="a11cf-111">Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel.</span><span class="sxs-lookup"><span data-stu-id="a11cf-111">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="a11cf-112">A Media Services két lokátortípust támogat:</span><span class="sxs-lookup"><span data-stu-id="a11cf-112">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="a11cf-113">Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például toostream MPEG DASH, HLS vagy Smooth Streaming) használt.</span><span class="sxs-lookup"><span data-stu-id="a11cf-113">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, toostream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="a11cf-114">toocreate a streamelési lokátorok létrehozásához az eszköz tartalmaznia kell egy .ism-fájlt.</span><span class="sxs-lookup"><span data-stu-id="a11cf-114">toocreate a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="a11cf-115">Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.</span><span class="sxs-lookup"><span data-stu-id="a11cf-115">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="a11cf-116">A streamelési URL-cím formátuma a következő hello rendelkezik, és használata tooplay Smooth Streaming-objektumok.</span><span class="sxs-lookup"><span data-stu-id="a11cf-116">A streaming URL has hello following format and you can use it tooplay Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="a11cf-117">toobuild HLS-streamelési URL-cím, egy hozzáfűző (formátum = m3u8-aapl) toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a11cf-117">toobuild an HLS streaming URL, append (format=m3u8-aapl) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="a11cf-118">egy streamelési URL-cím, MPEG DASH toobuild hozzáfűzése (formátum mpd-idő-csf =) toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a11cf-118">toobuild an  MPEG DASH streaming URL, append (format=mpd-time-csf) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

<span data-ttu-id="a11cf-119">Egy SAS URL-címnek a következő formátumban hello.</span><span class="sxs-lookup"><span data-stu-id="a11cf-119">A SAS URL has hello following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="a11cf-120">További információkért lásd: [tartalom áttekintése kézbesítéséhez](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a11cf-120">For more information, see [Delivering content overview](media-services-deliver-content-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a11cf-121">Ha korábban használt hello portál toocreate keresők 2015. márciusi, keresők kétéves lejárati dátummal jöttek létre.</span><span class="sxs-lookup"><span data-stu-id="a11cf-121">If you used hello portal toocreate locators before March 2015, locators with a two year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="a11cf-122">a lokátor, használja a lejárati dátum tooupdate [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-k.</span><span class="sxs-lookup"><span data-stu-id="a11cf-122">tooupdate an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="a11cf-123">Vegye figyelembe, hogy egy SAS-lokátor lejárati dátuma hello frissítésekor módosítja hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a11cf-123">Note that when you update hello expiration date of a SAS locator, hello URL changes.</span></span>

### <a name="toouse-hello-portal-toopublish-an-asset"></a><span data-ttu-id="a11cf-124">toouse hello portál toopublish egy eszköz</span><span class="sxs-lookup"><span data-stu-id="a11cf-124">toouse hello portal toopublish an asset</span></span>
<span data-ttu-id="a11cf-125">toouse hello portál toopublish egy eszköz hello a következő:</span><span class="sxs-lookup"><span data-stu-id="a11cf-125">toouse hello portal toopublish an asset, do hello following:</span></span>

1. <span data-ttu-id="a11cf-126">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="a11cf-126">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="a11cf-127">Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a11cf-127">Select **Settings** > **Assets**.</span></span>
3. <span data-ttu-id="a11cf-128">Válassza ki a megjeleníteni kívánt toopublish hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="a11cf-128">Select hello asset that you want toopublish.</span></span>
4. <span data-ttu-id="a11cf-129">Kattintson a hello **közzététel** gombra.</span><span class="sxs-lookup"><span data-stu-id="a11cf-129">Click hello **Publish** button.</span></span>
5. <span data-ttu-id="a11cf-130">Válassza ki a hello lokátor típusát.</span><span class="sxs-lookup"><span data-stu-id="a11cf-130">Select hello locator type.</span></span>
6. <span data-ttu-id="a11cf-131">Nyomja meg az **Add** (Hozzáadás) gombot.</span><span class="sxs-lookup"><span data-stu-id="a11cf-131">Press **Add**.</span></span>
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="a11cf-133">hello URL-cím hozzáadódik toohello listája **közzétett URL-címek**.</span><span class="sxs-lookup"><span data-stu-id="a11cf-133">hello URL will be added toohello list of **Published URLs**.</span></span>

## <a name="play-content-from-hello-portal"></a><span data-ttu-id="a11cf-134">Hello portálról tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="a11cf-134">Play content from hello portal</span></span>
<span data-ttu-id="a11cf-135">hello Azure-portálon talál egy tartalomlejátszót használható tootest a videót.</span><span class="sxs-lookup"><span data-stu-id="a11cf-135">hello Azure portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="a11cf-136">Kattintson a kívánt hello videó, majd hello **lejátszása** gombra.</span><span class="sxs-lookup"><span data-stu-id="a11cf-136">Click hello desired video and then click hello **Play** button.</span></span>

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="a11cf-138">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="a11cf-138">Some considerations apply:</span></span>

* <span data-ttu-id="a11cf-139">Ellenőrizze, hogy a videó hello is közzé lett téve.</span><span class="sxs-lookup"><span data-stu-id="a11cf-139">Make sure hello video has been published.</span></span>
* <span data-ttu-id="a11cf-140">Ez **Media player** hello alapértelmezett streamvégpontból játssza le.</span><span class="sxs-lookup"><span data-stu-id="a11cf-140">This **Media player** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="a11cf-141">Ha azt szeretné, hogy a nem alapértelmezett tooplay az adatfolyam-továbbítási végpontra, kattintson a toocopy hello URL-címet, és használjon másik lejátszót.</span><span class="sxs-lookup"><span data-stu-id="a11cf-141">If you want tooplay from a non-default streaming endpoint, click toocopy hello URL and use another player.</span></span> <span data-ttu-id="a11cf-142">Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="a11cf-142">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
* <span data-ttu-id="a11cf-143">adatfolyam-továbbítási végpontra, amelyből streaming hello rendszernek kell futnia.</span><span class="sxs-lookup"><span data-stu-id="a11cf-143">hello streaming endpoint from which you are streaming must be running.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="a11cf-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a11cf-144">Next steps</span></span>
<span data-ttu-id="a11cf-145">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="a11cf-145">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a11cf-146">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="a11cf-146">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

