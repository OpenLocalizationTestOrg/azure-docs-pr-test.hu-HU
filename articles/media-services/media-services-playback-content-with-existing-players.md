---
title: "meglévő játékosok tooplayback a tartalom - Azure aaaUse |} Microsoft Docs"
description: "Ez a témakör a meglévő játékosok használható tooplayback a tartalmat."
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
ms.openlocfilehash: 54817345a19a9d3b18f1e7b352c3342043a569b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="playing-your-content-with-existing-players"></a><span data-ttu-id="f4ccf-103">A meglévő játékosokkal tartalom lejátszása</span><span class="sxs-lookup"><span data-stu-id="f4ccf-103">Playing your content with existing players</span></span>
<span data-ttu-id="f4ccf-104">Az Azure Media Services számos népszerű formátumban, például a Smooth Streaming, HTTP Live Streaming és MPEG-Dash támogatja.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-104">Azure Media Services supports many popular streaming formats, such as Smooth Streaming, HTTP Live Streaming, and MPEG-Dash.</span></span> <span data-ttu-id="f4ccf-105">Ez a témakör mutató használható tootest fogadhassák tooexisting játékosok.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-105">This topic points you tooexisting players that you can use tootest your streams.</span></span>

### <a name="hello-azure-portal-media-services-content-player"></a><span data-ttu-id="f4ccf-106">hello Azure-portálon a Media Services content player</span><span class="sxs-lookup"><span data-stu-id="f4ccf-106">hello Azure portal Media Services content player</span></span>
<span data-ttu-id="f4ccf-107">Hello **Azure** portálon talál egy tartalomlejátszót használható tootest a videót.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-107">hello **Azure** portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="f4ccf-108">Kattintson a hello szükséges videó (Győződjön meg arról, hogy volt [közzétett](media-services-portal-publish.md)) hello kattintson **lejátszása** hello portal hello alján gombra.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-108">Click on hello desired video (make sure it was [published](media-services-portal-publish.md)) and click hello **Play** button at hello bottom of hello portal.</span></span>

<span data-ttu-id="f4ccf-109">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="f4ccf-109">Some considerations apply:</span></span>

* <span data-ttu-id="f4ccf-110">Hello **MEDIA SERVICES CONTENT PLAYER** hello alapértelmezett streamvégpontból játssza le.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-110">hello **MEDIA SERVICES CONTENT PLAYER** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="f4ccf-111">Ha azt szeretné, hogy egy nem alapértelmezett streamvégpontból tooplay, használjon másik lejátszót.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-111">If you want tooplay from a non-default streaming endpoint, use another player.</span></span> <span data-ttu-id="f4ccf-112">Például [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="f4ccf-112">For example, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a><span data-ttu-id="f4ccf-114">Azure Media Player</span><span class="sxs-lookup"><span data-stu-id="f4ccf-114">Azure Media Player</span></span>
<span data-ttu-id="f4ccf-115">Használjon [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tooplayback a tartalom hello a következő formátumok valamelyikében (egyszerű vagy védett):</span><span class="sxs-lookup"><span data-stu-id="f4ccf-115">Use [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tooplayback your content (clear or protected) in any of hello following formats:</span></span>

* <span data-ttu-id="f4ccf-116">Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="f4ccf-116">Smooth Streaming</span></span>
* <span data-ttu-id="f4ccf-117">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="f4ccf-117">MPEG DASH</span></span>
* <span data-ttu-id="f4ccf-118">HLS</span><span class="sxs-lookup"><span data-stu-id="f4ccf-118">HLS</span></span>
* <span data-ttu-id="f4ccf-119">Fokozatos MP4</span><span class="sxs-lookup"><span data-stu-id="f4ccf-119">Progressive MP4</span></span>

### <a name="flash-player"></a><span data-ttu-id="f4ccf-120">Flash Player</span><span class="sxs-lookup"><span data-stu-id="f4ccf-120">Flash Player</span></span>
#### <a name="aes-encrypted-with-token"></a><span data-ttu-id="f4ccf-121">AES által titkosított jogkivonatok</span><span class="sxs-lookup"><span data-stu-id="f4ccf-121">AES-encrypted with Token</span></span>
[<span data-ttu-id="f4ccf-122">http://aestoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="f4ccf-122">http://aestoken.azurewebsites.net</span></span>](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a><span data-ttu-id="f4ccf-123">A Silverlight-lejátszó</span><span class="sxs-lookup"><span data-stu-id="f4ccf-123">Silverlight Players</span></span>
#### <a name="monitoring"></a><span data-ttu-id="f4ccf-124">Figyelés</span><span class="sxs-lookup"><span data-stu-id="f4ccf-124">Monitoring</span></span>
[<span data-ttu-id="f4ccf-125">http://smf.cloudapp.NET/healthmonitor</span><span class="sxs-lookup"><span data-stu-id="f4ccf-125">http://smf.cloudapp.net/healthmonitor</span></span>](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a><span data-ttu-id="f4ccf-126">PlayReady-tokenhez</span><span class="sxs-lookup"><span data-stu-id="f4ccf-126">PlayReady with Token</span></span>
[<span data-ttu-id="f4ccf-127">http://sltoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="f4ccf-127">http://sltoken.azurewebsites.net</span></span>](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a><span data-ttu-id="f4ccf-128">KÖTŐJEL lejátszó</span><span class="sxs-lookup"><span data-stu-id="f4ccf-128">DASH Players</span></span>
[<span data-ttu-id="f4ccf-129">http://dashplayer.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="f4ccf-129">http://dashplayer.azurewebsites.net</span></span>](http://dashplayer.azurewebsites.net)

[<span data-ttu-id="f4ccf-130">http://dashif.org</span><span class="sxs-lookup"><span data-stu-id="f4ccf-130">http://dashif.org</span></span>](http://dashif.org)

### <a name="other"></a><span data-ttu-id="f4ccf-131">Egyéb</span><span class="sxs-lookup"><span data-stu-id="f4ccf-131">Other</span></span>
<span data-ttu-id="f4ccf-132">tootest HLS URL-címeket is használhatja:</span><span class="sxs-lookup"><span data-stu-id="f4ccf-132">tootest HLS URLs you can also use:</span></span>

* <span data-ttu-id="f4ccf-133">**Safari** iOS-eszközön vagy</span><span class="sxs-lookup"><span data-stu-id="f4ccf-133">**Safari** on an iOS device or</span></span>
* <span data-ttu-id="f4ccf-134">**3ivx HLS Player** Windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="f4ccf-134">**3ivx HLS Player** on Windows.</span></span>

## <a name="developing-video-players"></a><span data-ttu-id="f4ccf-135">Videó játékosok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="f4ccf-135">Developing video players</span></span>
<span data-ttu-id="f4ccf-136">Hogyan toodevelop saját játékosok: információ [videó játékosok fejlesztése](media-services-develop-video-players.md)</span><span class="sxs-lookup"><span data-stu-id="f4ccf-136">For information about how toodevelop your own players, see [Developing video players](media-services-develop-video-players.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="f4ccf-137">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="f4ccf-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f4ccf-138">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f4ccf-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
