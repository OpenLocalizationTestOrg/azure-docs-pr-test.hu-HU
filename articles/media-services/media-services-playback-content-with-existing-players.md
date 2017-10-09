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
# <a name="playing-your-content-with-existing-players"></a>A meglévő játékosokkal tartalom lejátszása
Az Azure Media Services számos népszerű formátumban, például a Smooth Streaming, HTTP Live Streaming és MPEG-Dash támogatja. Ez a témakör mutató használható tootest fogadhassák tooexisting játékosok.

### <a name="hello-azure-portal-media-services-content-player"></a>hello Azure-portálon a Media Services content player
Hello **Azure** portálon talál egy tartalomlejátszót használható tootest a videót.

Kattintson a hello szükséges videó (Győződjön meg arról, hogy volt [közzétett](media-services-portal-publish.md)) hello kattintson **lejátszása** hello portal hello alján gombra.

Vegye figyelembe a következőket:

* Hello **MEDIA SERVICES CONTENT PLAYER** hello alapértelmezett streamvégpontból játssza le. Ha azt szeretné, hogy egy nem alapértelmezett streamvégpontból tooplay, használjon másik lejátszót. Például [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player
Használjon [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tooplayback a tartalom hello a következő formátumok valamelyikében (egyszerű vagy védett):

* Smooth Streaming
* MPEG DASH
* HLS
* Fokozatos MP4

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>AES által titkosított jogkivonatok
[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>A Silverlight-lejátszó
#### <a name="monitoring"></a>Figyelés
[http://smf.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a>PlayReady-tokenhez
[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>KÖTŐJEL lejátszó
[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Egyéb
tootest HLS URL-címeket is használhatja:

* **Safari** iOS-eszközön vagy
* **3ivx HLS Player** Windows rendszeren.

## <a name="developing-video-players"></a>Videó játékosok fejlesztése
Hogyan toodevelop saját játékosok: információ [videó játékosok fejlesztése](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
