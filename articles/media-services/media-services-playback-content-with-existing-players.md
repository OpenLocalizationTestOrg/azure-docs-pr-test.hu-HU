---
title: "Meglévő játékosok lejátszásához használjon a tartalom - Azure |} Microsoft Docs"
description: "Ez a témakör felsorolja a meglévő játékosok használható lejátszásához a tartalmat."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako
ms.openlocfilehash: 48f373b013b1192c353352b801876d706d91dd28
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="playing-your-content-with-existing-players"></a><span data-ttu-id="d4967-103">A meglévő játékosokkal tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="d4967-103">Playing your content with existing players</span></span>
<span data-ttu-id="d4967-104">Az Azure Media Services számos népszerű formátumban, például a Smooth Streaming, HTTP Live Streaming és MPEG-Dash támogatja.</span><span class="sxs-lookup"><span data-stu-id="d4967-104">Azure Media Services supports many popular streaming formats, such as Smooth Streaming, HTTP Live Streaming, and MPEG-Dash.</span></span> <span data-ttu-id="d4967-105">Ez a témakör mutat meglévő játékosok fogadhassák teszteléséhez használható.</span><span class="sxs-lookup"><span data-stu-id="d4967-105">This topic points you to existing players that you can use to test your streams.</span></span>

### <a name="the-azure-portal-media-services-content-player"></a><span data-ttu-id="d4967-106">Az Azure portál Media Services content player</span><span class="sxs-lookup"><span data-stu-id="d4967-106">The Azure portal Media Services content player</span></span>
<span data-ttu-id="d4967-107">A **Azure** portálon talál egy tartalomlejátszót, amelyek segítségével tesztelheti a videót.</span><span class="sxs-lookup"><span data-stu-id="d4967-107">The **Azure** portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="d4967-108">Kattintson a kívánt videóra (Győződjön meg arról, hogy volt [közzétett](media-services-portal-publish.md)), és kattintson a **lejátszása** gomb a portál alján.</span><span class="sxs-lookup"><span data-stu-id="d4967-108">Click on the desired video (make sure it was [published](media-services-portal-publish.md)) and click the **Play** button at the bottom of the portal.</span></span>

<span data-ttu-id="d4967-109">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="d4967-109">Some considerations apply:</span></span>

* <span data-ttu-id="d4967-110">A **MEDIA SERVICES CONTENT PLAYER** (Media Services tartalomlejátszó) az alapértelmezett streamvégpontból játssza le a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="d4967-110">The **MEDIA SERVICES CONTENT PLAYER** plays from the default streaming endpoint.</span></span> <span data-ttu-id="d4967-111">Ha egy nem alapértelmezett streamvégpontból szeretne lejátszani valamit, használjon másik lejátszót.</span><span class="sxs-lookup"><span data-stu-id="d4967-111">If you want to play from a non-default streaming endpoint, use another player.</span></span> <span data-ttu-id="d4967-112">Például [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="d4967-112">For example, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a><span data-ttu-id="d4967-114">Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="d4967-114">Azure Media Player</span></span>
<span data-ttu-id="d4967-115">Használjon [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) lejátszásához a tartalom (egyszerű vagy védett) a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="d4967-115">Use [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) to playback your content (clear or protected) in any of the following formats:</span></span>

* <span data-ttu-id="d4967-116">Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="d4967-116">Smooth Streaming</span></span>
* <span data-ttu-id="d4967-117">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="d4967-117">MPEG DASH</span></span>
* <span data-ttu-id="d4967-118">HLS</span><span class="sxs-lookup"><span data-stu-id="d4967-118">HLS</span></span>
* <span data-ttu-id="d4967-119">Fokozatos MP4</span><span class="sxs-lookup"><span data-stu-id="d4967-119">Progressive MP4</span></span>

### <a name="flash-player"></a><span data-ttu-id="d4967-120">Flash Player</span><span class="sxs-lookup"><span data-stu-id="d4967-120">Flash Player</span></span>
#### <a name="aes-encrypted-with-token"></a><span data-ttu-id="d4967-121">AES által titkosított jogkivonatok</span><span class="sxs-lookup"><span data-stu-id="d4967-121">AES-encrypted with Token</span></span>
[<span data-ttu-id="d4967-122">http://aestoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="d4967-122">http://aestoken.azurewebsites.net</span></span>](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a><span data-ttu-id="d4967-123">A Silverlight-lejátszó</span><span class="sxs-lookup"><span data-stu-id="d4967-123">Silverlight Players</span></span>
#### <a name="monitoring"></a><span data-ttu-id="d4967-124">Figyelés</span><span class="sxs-lookup"><span data-stu-id="d4967-124">Monitoring</span></span>
[<span data-ttu-id="d4967-125">http://smf.cloudapp.NET/healthmonitor</span><span class="sxs-lookup"><span data-stu-id="d4967-125">http://smf.cloudapp.net/healthmonitor</span></span>](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a><span data-ttu-id="d4967-126">PlayReady-tokenhez</span><span class="sxs-lookup"><span data-stu-id="d4967-126">PlayReady with Token</span></span>
[<span data-ttu-id="d4967-127">http://sltoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="d4967-127">http://sltoken.azurewebsites.net</span></span>](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a><span data-ttu-id="d4967-128">KÖTŐJEL lejátszó</span><span class="sxs-lookup"><span data-stu-id="d4967-128">DASH Players</span></span>
[<span data-ttu-id="d4967-129">http://dashplayer.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="d4967-129">http://dashplayer.azurewebsites.net</span></span>](http://dashplayer.azurewebsites.net)

[<span data-ttu-id="d4967-130">http://dashif.org</span><span class="sxs-lookup"><span data-stu-id="d4967-130">http://dashif.org</span></span>](http://dashif.org)

### <a name="other"></a><span data-ttu-id="d4967-131">Egyéb</span><span class="sxs-lookup"><span data-stu-id="d4967-131">Other</span></span>
<span data-ttu-id="d4967-132">Tesztelése HLS URL-címeket is használhatja:</span><span class="sxs-lookup"><span data-stu-id="d4967-132">To test HLS URLs you can also use:</span></span>

* <span data-ttu-id="d4967-133">**Safari** iOS-eszközön vagy</span><span class="sxs-lookup"><span data-stu-id="d4967-133">**Safari** on an iOS device or</span></span>
* <span data-ttu-id="d4967-134">**3ivx HLS Player** Windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="d4967-134">**3ivx HLS Player** on Windows.</span></span>

## <a name="developing-video-players"></a><span data-ttu-id="d4967-135">Videó játékosok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="d4967-135">Developing video players</span></span>
<span data-ttu-id="d4967-136">A saját játékosok fejlesztésével kapcsolatos további információkért lásd: [videó játékosok fejlesztése](media-services-develop-video-players.md)</span><span class="sxs-lookup"><span data-stu-id="d4967-136">For information about how to develop your own players, see [Developing video players](media-services-develop-video-players.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="d4967-137">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="d4967-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d4967-138">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="d4967-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
