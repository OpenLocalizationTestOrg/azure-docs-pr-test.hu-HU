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
# <a name="troubleshooting-guide-for-live-streaming"></a>Hibaelhárítási útmutató az élő streameléshez
Ez a témakör a kapcsolatos javaslatok minden egyes élő adatfolyam problémák tootroubleshoot.

## <a name="issues-related-tooon-premises-encoders"></a>Tooon helyszíni kódolókkal kapcsolódó problémák
Ez a szakasz ad javaslatokat a hogyan tootroubleshoot problémák kapcsolódó tooon helyszíni kódolókkal, amelyek konfigurálva toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák élő kódolásra.

### <a name="problem-would-like-toosee-logs"></a>Probléma: Szeretné toosee naplók
* **Lehetséges probléma**: nem található kódoló naplózza, hogy elősegítheti hibáinak feltárására.
  
  * **Telestream Wirecast**: általában található naplók alapján C:\Users\{felhasználónév} \AppData\Roaming\Wirecast\ 
  * **Elemi Live**: hivatkozások toologs rendelkezzen hello felügyeleti portálon található. Kattintson a **statisztikák**, majd **naplók**. A hello **naplófájlok** lapon megjelenik egy lista összes naplók LiveEvent elemek hello; válasszon egy megfelelő az aktuális munkamenet hello. 
  * **Élő kódoló adathordozó Flash**: hello található **naplókönyvtár...**  toohello navigálva **kódolás napló** fülre.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Hiba: Nincs lehetőség a progresszív adatfolyam írása
* **Lehetséges probléma**: hello kódoló használt automatikusan félképek nem összefésülése. 
  
    **Hibaelhárítási lépések**: keresse meg a deszerializálni kötésre beállítás hello kódoló felületen belül. Vonja rövidebbnek engedélyezése után újból fokozatos kimeneti beállításokat ellenőrzi. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-tooconnect"></a>Probléma: Kísérlet történt több kódoló kimeneti beállításait, és továbbra sem tudja tooconnect.
* **Lehetséges probléma**: Azure kódolási csatorna nem megfelelően alaphelyzetbe állítása. 
  
    **Hibaelhárítási lépések**: Győződjön meg arról, hogy hello kódoló már nem küldését tooAMS, állítsa le és hello csatorna alaphelyzetbe állítására. Ha újra futtatni, próbáljon meg a kódoló hello új beállításokkal. Ha még nem oldható hello problémát, próbálja meg létrehozni teljesen új csatorna, néha csatornák megsérül több meghiúsult próbálkozást követően.  
* **Lehetséges probléma**: hello GOP méret és kulcskocka beállítások nincsenek optimális. 
  
    **Hibaelhárítási lépések**: GOP ajánlott méret vagy keyframe időköz érték 2 másodperc. Néhány kódolók kiszámításához képkockaszámát, ennek a beállításnak, míg mások használnak másodperc. Például: 30fps exportálásakor hello GOP mérete lenne 60 keretek, amely egyenértékű too2 másodperc.  
* **Lehetséges probléma**: lezárt portok blokkolják a hello adatfolyam. 
  
    **Hibaelhárítási lépések**: streaming RTMP keresztül, hogy nem tűzfal, illetve hogy 1935 és 1936 kimenő portjai nyitva-e a proxy beállításainak tooconfirm. RTP streaming használatakor győződjön meg arról, hogy nyitva-e kimenő port: 2010. 

### <a name="problem-when-configuring-hello-encoder-toostream-with-hello-rtp-protocol-there-is-no-place-tooenter-a-host-name"></a>Probléma: Hello kódoló toostream a hello RTP protokoll konfigurálásakor nincs nem tooenter egy állomásnevet.
* **Lehetséges probléma**: sok RTP kódolók állomásnevek nem engedélyezi, és egy IP-címet szerzett toobe lesz szüksége.  
  
    **Hibaelhárítási lépések**: toofind hello IP-cím, nyisson meg egy parancssort, minden olyan számítógépen. toodo ezt a Windows, nyissa meg a hello Futtatás indító (WIN + K), és írja be a "cmd" tooopen.  
  
    Ha meg nyitva a parancssor hello, írja be a "Ping [AMS állomásnév]". 
  
    hello állomásnév származtathatók hello Azure betöltési URL-cím, az alábbi példa hello példának hello port számát, kihagyva: 
  
    RTP://test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> Ha követte hello hibaelhárítási lépéseket még nem sikerült továbbíthat, küldje el a támogatási jegy hello Azure-portál használatával.
> 
> 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

