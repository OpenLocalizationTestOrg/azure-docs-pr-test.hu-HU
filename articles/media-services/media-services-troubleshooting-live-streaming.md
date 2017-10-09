---
title: "az élő adatfolyamként történő aaaTroubleshooting útmutató |} Microsoft Docs"
description: "Ez a témakör minden hogyan tootroubleshoot élő adatfolyam-továbbítási problémák a javaslatokat."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 8549bae947ff3b225ce624220d1e48b63f90208c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a><span data-ttu-id="f2d61-103">Hibaelhárítási útmutató az élő streameléshez</span><span class="sxs-lookup"><span data-stu-id="f2d61-103">Troubleshooting guide for live streaming</span></span>
<span data-ttu-id="f2d61-104">Ez a témakör a kapcsolatos javaslatok minden egyes élő adatfolyam problémák tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="f2d61-104">This topic gives suggestions on how tootroubleshoot some live streaming problems.</span></span>

## <a name="issues-related-tooon-premises-encoders"></a><span data-ttu-id="f2d61-105">Tooon helyszíni kódolókkal kapcsolódó problémák</span><span class="sxs-lookup"><span data-stu-id="f2d61-105">Issues related tooon-premises encoders</span></span>
<span data-ttu-id="f2d61-106">Ez a szakasz ad javaslatokat a hogyan tootroubleshoot problémák kapcsolódó tooon helyszíni kódolókkal, amelyek konfigurálva toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák élő kódolásra.</span><span class="sxs-lookup"><span data-stu-id="f2d61-106">This section gives suggestions on how tootroubleshoot problems related tooon-premises encoders that are configured toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>

### <a name="problem-would-like-toosee-logs"></a><span data-ttu-id="f2d61-107">Probléma: Szeretné toosee naplók</span><span class="sxs-lookup"><span data-stu-id="f2d61-107">Problem: Would like toosee logs</span></span>
* <span data-ttu-id="f2d61-108">**Lehetséges probléma**: nem található kódoló naplózza, hogy elősegítheti hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="f2d61-108">**Potential issue**: Can't find encoder logs that might help in debugging issues.</span></span>
  
  * <span data-ttu-id="f2d61-109">**Telestream Wirecast**: általában található naplók alapján C:\Users\{felhasználónév} \AppData\Roaming\Wirecast\\</span><span class="sxs-lookup"><span data-stu-id="f2d61-109">**Telestream Wirecast**: You can usually find logs under C:\Users\{username}\AppData\Roaming\Wirecast\\</span></span> 
  * <span data-ttu-id="f2d61-110">**Elemi Live**: hivatkozások toologs rendelkezzen hello felügyeleti portálon található.</span><span class="sxs-lookup"><span data-stu-id="f2d61-110">**Elemental Live**: You can find has links toologs on hello management portal.</span></span> <span data-ttu-id="f2d61-111">Kattintson a **statisztikák**, majd **naplók**.</span><span class="sxs-lookup"><span data-stu-id="f2d61-111">Click on **Stats**, then **Logs**.</span></span> <span data-ttu-id="f2d61-112">A hello **naplófájlok** lapon megjelenik egy lista összes naplók LiveEvent elemek hello; válasszon egy megfelelő az aktuális munkamenet hello.</span><span class="sxs-lookup"><span data-stu-id="f2d61-112">On hello **Log Files** page, you will see a list of logs for all hello LiveEvent items; select hello one matching your current session.</span></span> 
  * <span data-ttu-id="f2d61-113">**Élő kódoló adathordozó Flash**: hello található **naplókönyvtár...**  toohello navigálva **kódolás napló** fülre.</span><span class="sxs-lookup"><span data-stu-id="f2d61-113">**Flash Media Live Encoder**: You can find hello **Log Directory...** by navigating toohello **Encoding Log** tab.</span></span>

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a><span data-ttu-id="f2d61-114">Hiba: Nincs lehetőség a progresszív adatfolyam írása</span><span class="sxs-lookup"><span data-stu-id="f2d61-114">Problem: There is no option for outputting a progressive stream</span></span>
* <span data-ttu-id="f2d61-115">**Lehetséges probléma**: hello kódoló használt automatikusan félképek nem összefésülése.</span><span class="sxs-lookup"><span data-stu-id="f2d61-115">**Potential issue**: hello encoder being used doesn't automatically deinterlace.</span></span> 
  
    <span data-ttu-id="f2d61-116">**Hibaelhárítási lépések**: keresse meg a deszerializálni kötésre beállítás hello kódoló felületen belül.</span><span class="sxs-lookup"><span data-stu-id="f2d61-116">**Troubleshooting steps**: Look for a de-interlacing option within hello encoder interface.</span></span> <span data-ttu-id="f2d61-117">Vonja rövidebbnek engedélyezése után újból fokozatos kimeneti beállításokat ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="f2d61-117">Once de-interlacing is enabled, check again for progressive output settings.</span></span> 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-tooconnect"></a><span data-ttu-id="f2d61-118">Probléma: Kísérlet történt több kódoló kimeneti beállításait, és továbbra sem tudja tooconnect.</span><span class="sxs-lookup"><span data-stu-id="f2d61-118">Problem: Tried several encoder output settings and still unable tooconnect.</span></span>
* <span data-ttu-id="f2d61-119">**Lehetséges probléma**: Azure kódolási csatorna nem megfelelően alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="f2d61-119">**Potential issue**: Azure encoding channel was not properly reset.</span></span> 
  
    <span data-ttu-id="f2d61-120">**Hibaelhárítási lépések**: Győződjön meg arról, hogy hello kódoló már nem küldését tooAMS, állítsa le és hello csatorna alaphelyzetbe állítására.</span><span class="sxs-lookup"><span data-stu-id="f2d61-120">**Troubleshooting steps**: Make sure hello encoder is no longer pushing tooAMS, stop and reset hello channel.</span></span> <span data-ttu-id="f2d61-121">Ha újra futtatni, próbáljon meg a kódoló hello új beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="f2d61-121">Once running again, try connecting your encoder with hello new settings.</span></span> <span data-ttu-id="f2d61-122">Ha még nem oldható hello problémát, próbálja meg létrehozni teljesen új csatorna, néha csatornák megsérül több meghiúsult próbálkozást követően.</span><span class="sxs-lookup"><span data-stu-id="f2d61-122">If this still does not correct hello issue, try creating a new channel entirely, sometimes channels can become corrupt after several failed attempts.</span></span>  
* <span data-ttu-id="f2d61-123">**Lehetséges probléma**: hello GOP méret és kulcskocka beállítások nincsenek optimális.</span><span class="sxs-lookup"><span data-stu-id="f2d61-123">**Potential issue**: hello GOP size or key frame settings are not optimal.</span></span> 
  
    <span data-ttu-id="f2d61-124">**Hibaelhárítási lépések**: GOP ajánlott méret vagy keyframe időköz érték 2 másodperc.</span><span class="sxs-lookup"><span data-stu-id="f2d61-124">**Troubleshooting steps**: Recommended GOP size or keyframe interval is 2 seconds.</span></span> <span data-ttu-id="f2d61-125">Néhány kódolók kiszámításához képkockaszámát, ennek a beállításnak, míg mások használnak másodperc.</span><span class="sxs-lookup"><span data-stu-id="f2d61-125">Some encoders calculate this setting in number of frames, while others use seconds.</span></span> <span data-ttu-id="f2d61-126">Például: 30fps exportálásakor hello GOP mérete lenne 60 keretek, amely egyenértékű too2 másodperc.</span><span class="sxs-lookup"><span data-stu-id="f2d61-126">For example: When outputting 30fps, hello GOP size would be 60 frames, which is equivalent too2 seconds.</span></span>  
* <span data-ttu-id="f2d61-127">**Lehetséges probléma**: lezárt portok blokkolják a hello adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="f2d61-127">**Potential issue**: Closed ports are blocking hello stream.</span></span> 
  
    <span data-ttu-id="f2d61-128">**Hibaelhárítási lépések**: streaming RTMP keresztül, hogy nem tűzfal, illetve hogy 1935 és 1936 kimenő portjai nyitva-e a proxy beállításainak tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="f2d61-128">**Troubleshooting steps**: When streaming via RTMP, check firewall and/or proxy settings tooconfirm that outbound ports 1935 and 1936 are open.</span></span> <span data-ttu-id="f2d61-129">RTP streaming használatakor győződjön meg arról, hogy nyitva-e kimenő port: 2010.</span><span class="sxs-lookup"><span data-stu-id="f2d61-129">When using RTP streaming, confirm that outbound port 2010 is open.</span></span> 

### <a name="problem-when-configuring-hello-encoder-toostream-with-hello-rtp-protocol-there-is-no-place-tooenter-a-host-name"></a><span data-ttu-id="f2d61-130">Probléma: Hello kódoló toostream a hello RTP protokoll konfigurálásakor nincs nem tooenter egy állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="f2d61-130">Problem: When configuring hello encoder toostream with hello RTP protocol, there is no place tooenter a host name.</span></span>
* <span data-ttu-id="f2d61-131">**Lehetséges probléma**: sok RTP kódolók állomásnevek nem engedélyezi, és egy IP-címet szerzett toobe lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="f2d61-131">**Potential issue**: Many RTP encoders do not allow for host names, and an IP address will need toobe acquired.</span></span>  
  
    <span data-ttu-id="f2d61-132">**Hibaelhárítási lépések**: toofind hello IP-cím, nyisson meg egy parancssort, minden olyan számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f2d61-132">**Troubleshooting steps**: toofind hello IP address, open a command prompt on any computer.</span></span> <span data-ttu-id="f2d61-133">toodo ezt a Windows, nyissa meg a hello Futtatás indító (WIN + K), és írja be a "cmd" tooopen.</span><span class="sxs-lookup"><span data-stu-id="f2d61-133">toodo this in Windows, open hello Run launcher (WIN + R) and type “cmd” tooopen.</span></span>  
  
    <span data-ttu-id="f2d61-134">Ha meg nyitva a parancssor hello, írja be a "Ping [AMS állomásnév]".</span><span class="sxs-lookup"><span data-stu-id="f2d61-134">Once hello command prompt is open, type "Ping [AMS Host Name]".</span></span> 
  
    <span data-ttu-id="f2d61-135">hello állomásnév származtathatók hello Azure betöltési URL-cím, az alábbi példa hello példának hello port számát, kihagyva:</span><span class="sxs-lookup"><span data-stu-id="f2d61-135">hello host name can be derived by omitting hello port number from hello Azure Ingest URL, as highlighted in hello following example:</span></span> 
  
    <span data-ttu-id="f2d61-136">RTP://test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 /</span><span class="sxs-lookup"><span data-stu-id="f2d61-136">rtp://test2-amstest009.rtp.channel.mediaservices.windows.net:2010/</span></span> 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> <span data-ttu-id="f2d61-138">Ha követte hello hibaelhárítási lépéseket még nem sikerült továbbíthat, küldje el a támogatási jegy hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="f2d61-138">If after following hello troubleshooting steps you still cannot successfully stream, submit a support ticket using hello Azure portal.</span></span>
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="f2d61-139">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="f2d61-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f2d61-140">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f2d61-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

