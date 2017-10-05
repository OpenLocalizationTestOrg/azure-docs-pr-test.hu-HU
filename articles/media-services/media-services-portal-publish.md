---
title: "  Az Azure portálon tartalom közzététele |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a közzétételi és az Azure portál tartalmaihoz."
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
ms.openlocfilehash: 68a2fbdda0996cf4ba5ea3b09816bf845af756f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="publish-content-with-the-azure-portal"></a><span data-ttu-id="9782a-103">Az Azure portálon tartalom közzététele</span><span class="sxs-lookup"><span data-stu-id="9782a-103">Publish content with the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9782a-104">Portal</span><span class="sxs-lookup"><span data-stu-id="9782a-104">Portal</span></span>](media-services-portal-publish.md)
> * [<span data-ttu-id="9782a-105">.NET</span><span class="sxs-lookup"><span data-stu-id="9782a-105">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="9782a-106">REST</span><span class="sxs-lookup"><span data-stu-id="9782a-106">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="9782a-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9782a-107">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="9782a-108">Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9782a-108">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="9782a-109">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9782a-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="9782a-110">Ahhoz, hogy átadhassa a tartalmak streamelésére vagy letöltésére használható URL-címet a felhasználónak, először „közzé kell tennie” az objektumot. Ehhez létre kell hoznia egy lokátort.</span><span class="sxs-lookup"><span data-stu-id="9782a-110">To provide your user with a  URL that can be used to stream or download your content, you first need to "publish" your asset by creating a locator.</span></span> <span data-ttu-id="9782a-111">Az objektumban található fájlokhoz a lokátorok biztosítanak hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="9782a-111">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="9782a-112">A Media Services két lokátortípust támogat:</span><span class="sxs-lookup"><span data-stu-id="9782a-112">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="9782a-113">Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például MPEG DASH, HLS vagy Smooth Streaming adatok streameléséhez) használhatók.</span><span class="sxs-lookup"><span data-stu-id="9782a-113">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, to stream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="9782a-114">A streamelési lokátorok létrehozásához az objektumnak tartalmaznia kell egy .ism-fájlt.</span><span class="sxs-lookup"><span data-stu-id="9782a-114">To create a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="9782a-115">Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.</span><span class="sxs-lookup"><span data-stu-id="9782a-115">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="9782a-116">A streamelési URL-cím formátuma alább látható. Ez Smooth Streaming-objektumok lejátszására használható.</span><span class="sxs-lookup"><span data-stu-id="9782a-116">A streaming URL has the following format and you can use it to play Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="9782a-117">HLS-streamelési URL-cím létrehozásához fűzze hozzá a következőt az URL-címhez: (format=m3u8-aapl).</span><span class="sxs-lookup"><span data-stu-id="9782a-117">To build an HLS streaming URL, append (format=m3u8-aapl) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="9782a-118">MPEG DASH-streamelési URL-cím létrehozásához fűzze hozzá a következőt az URL-címhez: (format=mpd-time-csf).</span><span class="sxs-lookup"><span data-stu-id="9782a-118">To build an  MPEG DASH streaming URL, append (format=mpd-time-csf) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

<span data-ttu-id="9782a-119">Az SAS URL-cím formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="9782a-119">A SAS URL has the following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="9782a-120">További információkért lásd: [tartalom áttekintése kézbesítéséhez](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9782a-120">For more information, see [Delivering content overview](media-services-deliver-content-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9782a-121">2015 márciusa előtt a portál kétéves lejárati idővel hozta létre a lokátorokat.</span><span class="sxs-lookup"><span data-stu-id="9782a-121">If you used the portal to create locators before March 2015, locators with a two year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="9782a-122">A lokátor lejárati idejének módosításához használjon [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-t.</span><span class="sxs-lookup"><span data-stu-id="9782a-122">To update an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="9782a-123">Ne feledje, hogy a SAS-lokátor lejárati idejének módosításával az URL-cím is megváltozik.</span><span class="sxs-lookup"><span data-stu-id="9782a-123">Note that when you update the expiration date of a SAS locator, the URL changes.</span></span>

### <a name="to-use-the-portal-to-publish-an-asset"></a><span data-ttu-id="9782a-124">Az objektum portál segítségével történő közzététele</span><span class="sxs-lookup"><span data-stu-id="9782a-124">To use the portal to publish an asset</span></span>
<span data-ttu-id="9782a-125">Az objektumnak a portál segítségével történő közzétételéhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9782a-125">To use the portal to publish an asset, do the following:</span></span>

1. <span data-ttu-id="9782a-126">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="9782a-126">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="9782a-127">Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9782a-127">Select **Settings** > **Assets**.</span></span>
3. <span data-ttu-id="9782a-128">Válassza ki a közzétenni kívánt objektumot.</span><span class="sxs-lookup"><span data-stu-id="9782a-128">Select the asset that you want to publish.</span></span>
4. <span data-ttu-id="9782a-129">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="9782a-129">Click the **Publish** button.</span></span>
5. <span data-ttu-id="9782a-130">Válassza ki a lokátor típusát.</span><span class="sxs-lookup"><span data-stu-id="9782a-130">Select the locator type.</span></span>
6. <span data-ttu-id="9782a-131">Nyomja meg az **Add** (Hozzáadás) gombot.</span><span class="sxs-lookup"><span data-stu-id="9782a-131">Press **Add**.</span></span>
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="9782a-133">A rendszer hozzáadja az URL-címet a **Published URLs** (Közzétett URL-címek) listához.</span><span class="sxs-lookup"><span data-stu-id="9782a-133">The URL will be added to the list of **Published URLs**.</span></span>

## <a name="play-content-from-the-portal"></a><span data-ttu-id="9782a-134">Tartalom lejátszása a portálról</span><span class="sxs-lookup"><span data-stu-id="9782a-134">Play content from the portal</span></span>
<span data-ttu-id="9782a-135">Az Azure Portalon talál egy tartalomlejátszót, amellyel tesztelheti a videót.</span><span class="sxs-lookup"><span data-stu-id="9782a-135">The Azure portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="9782a-136">Kattintson a kívánt videóra, majd a **Lejátszás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9782a-136">Click the desired video and then click the **Play** button.</span></span>

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="9782a-138">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="9782a-138">Some considerations apply:</span></span>

* <span data-ttu-id="9782a-139">Ellenőrizze, hogy közzétette-e a videót.</span><span class="sxs-lookup"><span data-stu-id="9782a-139">Make sure the video has been published.</span></span>
* <span data-ttu-id="9782a-140">A **Media Player** az alapértelmezett streamvégpontból játssza le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="9782a-140">This **Media player** plays from the default streaming endpoint.</span></span> <span data-ttu-id="9782a-141">Ha egy nem alapértelmezett streamvégpontból szeretne lejátszani valamit, rákattintva másolja az URL-címet, és használjon másik lejátszót.</span><span class="sxs-lookup"><span data-stu-id="9782a-141">If you want to play from a non-default streaming endpoint, click to copy the URL and use another player.</span></span> <span data-ttu-id="9782a-142">Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="9782a-142">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
* <span data-ttu-id="9782a-143">A streamvégpontján, amelyről streaming rendszernek kell futnia.</span><span class="sxs-lookup"><span data-stu-id="9782a-143">The streaming endpoint from which you are streaming must be running.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="9782a-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9782a-144">Next steps</span></span>
<span data-ttu-id="9782a-145">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="9782a-145">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9782a-146">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="9782a-146">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

