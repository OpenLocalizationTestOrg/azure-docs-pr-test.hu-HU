---
title: "aaaLive adatfolyam helyszíni kódolókkal hello Azure-portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello létre egy csatornát, amely átmenő közvetítésre van konfigurálva."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 1fb341e022f66f33903e13e07d3e84c0216cad77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-hello-azure-portal"></a>Hogyan tooperform élő Stream továbbítása helyszíni kódolókkal hello Azure-portál használatával
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-live-passthrough-get-started.md)
> * [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Ez az oktatóanyag végigvezeti hello hello Azure portál toocreate használatával egy **csatorna** , amely átmenő közvetítésre van konfigurálva. 

## <a name="prerequisites"></a>Előfeltételek
Az alábbiakban hello szükséges toocomplete hello oktatóanyag:

* Egy Azure-fiók. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
* Egy Media Services-fiók. egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).
* Egy webkamera. Például a [Telestream Wirecast kódoló](http://www.telestream.net/wirecast/overview.htm).

A következő cikkek tooreview hello javasoljuk:

* [Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/) (Az Azure Media Services RTMP-támogatása és az élő kódolók)
* [Overview of Live Streaming using Azure Media Services](media-services-manage-channels-overview.md) (Az Azure Media Services segítségével történő élő streamelés áttekintése)
* [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) (Élő stream továbbítása többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal)

## <a id="scenario"></a>Az élő adatfolyamok egy gyakori alkalmazási helyzete
hello lépései streamelő alkalmazásokat, amelyek átmenő közvetítésre konfigurált csatornák létrehozni. Ez az oktatóanyag bemutatja, hogyan toocreate átmenő csatornát és élő eseményeket és kezelése.

>[!NOTE]
>Győződjön meg arról, amelyből el kívánja toostream tartalom streamvégpontra hello hello **futtató** állapotát. 
    
1. Csatlakozás egy videokamerát tooa számítógép. Indítson el és konfiguráljon egy élő helyszíni kódolót, amely többszörös sávszélességű RTMP- vagy fragmentált MP4-streamet állít elő. További tájékoztatást az [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824) (Az Azure Media Services RTMP-támogatása és az élő kódolók) című cikk nyújt.
   
    Ezt a lépést a csatorna létrehozása után is el lehet végezni.
2. Hozzon létre és indítson el egy átmenő csatornát.
3. Kérje le hello csatorna feldolgozó URL-CÍMÉT. 
   
    hello betöltési URL-cím hello élő kódoló toosend hello adatfolyam toohello csatorna által használt.
4. Hello csatorna előnézeti URL-cím beolvasása. 
   
    Az URL-cím tooverify, hogy a csatornája megfelelően fogadja-hello élő adatfolyam használata.
5. Hozzon létre egy élő eseményt/programot. 
   
    Az Azure portál használatával hello, amikor élő esemény létrehozása egy objektumot is létrehoz. 

6. Amikor készen áll a toostart streamelésre és az archiválásra, indítsa el hello eseményt/programot.
7. Másik lehetőségként hello élő kódoló jelzett toostart hirdetést is lehet. hello hirdetés bekerül hello kimeneti adatfolyam.
8. Amikor azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény, állítsa le hello eseményt/programot.
9. Törölje a hello eseményt/programot (és választhatóan a hello eszköz törlése).     

> [!IMPORTANT]
> Tekintse át [élő Stream továbbítása helyszíni kódolókkal, amely többféle sávszélességű adatfolyamok létrehozása](media-services-live-streaming-with-onprem-encoders.md) kapcsolatos fogalmakat és szempontokat toolearn kapcsolódó toolive Stream továbbítása helyszíni kódolókkal és átmenő csatornákkal.
> 
> 

## <a name="tooview-notifications-and-errors"></a>tooview értesítéseket és hibaüzeneteket
Ha azt szeretné, hogy tooview értesítések és hibák hello Azure-portálon, kattintson a hello értesítés ikonra.

![Értesítések](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a>Átmenő csatornák és események létrehozása és elindítása.
A csatorna egy események/programok, amelyek lehetővé teszik a toocontrol hello közzétételét és tárolását élő stream szegmenseinek társítva. Az eseményeket a csatornák kezelik. 

Megadhatja a hello óraszámon belül hello program hello beállítása kívánt tooretain rögzített hello tartalom **archiválási időtartammal** hossza. Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be. Az archiválási időtartam is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól. Események hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál. Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.

Minden esemény egy objektumhoz van társítva. toopublish hello esemény, létre kell hoznia egy OnDemand-kereső hello társított eszköz. Ez a lokátor lehetővé teszi, hogy a toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.

Egy csatorna legfeljebb események egyidejűleg fut, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből. Ez lehetővé teszi toopublish kapcsolatos és archiválási különböző részei egy eseményt, igény szerint. Például az üzleti igény szerint tooarchive program, de toobroadcast 6 óra csak az elmúlt 10 perc. tooaccomplish, toocreate két egyidejűleg zajló program van szüksége. Egy program van beállítva a tooarchive hello esemény 6 óra, de hello program nem lesz közzétéve. hello másik program beállítva tooarchive 10 percig, és a program közzé van téve.

A meglévő élő eseményeket nem szabad újra felhasználni. Ehelyett hozzon létre egy új eseményt minden eseményhez, és indítsa el.

Amikor készen áll a toostart streamelésre és az archiválásra hello esemény indítása. Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény. 

toodelete archivált tartalmat, állítsa le és hello esemény törlése, és törölje a hozzá társított objektumot hello. Egy eszköz nem törölhető, ha egy esemény; használják hello esemény először törölni kell. 

Állítsa le és a hello esemény törlése után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz.

Ha szeretné tooretain hello archivált tartalom, de nem rendelkezik az adatfolyamként történő elérhetővé, törölje a streamelési locator hello.

### <a name="toouse-hello-portal-toocreate-a-channel"></a>toouse hello portál toocreate csatorna
Ez a szakasz bemutatja, hogyan toouse hello **Gyorslétrehozás** beállítás toocreate átmenő csatornát.

Az átmenő csatornákról az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikk nyújt részletes tájékoztatást.

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** ablak, kattintson a **Live streaming**. 
   
    ![Bevezetés](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    Hello **élő adatfolyam-** ablak jelenik meg.
3. Kattintson a **Gyorslétrehozás** toocreate átmenő csatornát a hello RTMP betöltési protokollt.
   
    Hello **új csatorna létrehozása** ablak jelenik meg.
4. Nevezze el hello új csatornát, és kattintson a **létrehozása**. 
   
    Ez egy átmenő csatornát hello hoz létre RTMP betöltési protokollt.

## <a name="create-events"></a>Események létrehozása
1. Válassza ki a csatorna toowhich kívánt tooadd egy eseményt.
2. Kattintson a **Live Event** (Élő esemény) gombra.

![Esemény](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a>A betöltési URL-címek beolvasása
Hello csatorna létrehozása után kaphat a betöltési URL-címeket adja meg az élő kódoló toohello. a kódoló hello ezen URL-címek tooinput élő adatfolyam.

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-hello-event"></a>A figyelési hello esemény
toowatch hello esemény, kattintson a **figyelési** a streamelési URL-cím az Azure-portál vagy a példány hello hello és a Windows Media Player az Ön által választott. 

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Élő esemény automatikusan megkapja az átalakított tooon igény szerinti tartalomterjesztésről leállításakor.

## <a name="clean-up"></a>A fölöslegessé vált elemek eltávolítása
Az átmenő csatornákról az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikk nyújt részletes tájékoztatást.

* Egy csatornát csak akkor, ha le lett állítva az összes események/programok hello csatornán állítható le.  Hello csatorna leállítását követően nem számítunk fel díjakat. Ha toostart kell azt újra, hogy fog rendelkezni hello azonos feldolgozó URL-CÍMÉT, akkor nem kell tooreconfigure a kódoló.
* Egy csatorna törölhetők, csak akkor, ha hello csatornán összes élő eseményt törölték.

## <a name="view-archived-content"></a>Archivált tartalom megtekintése
Állítsa le és a hello esemény törlése után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz. Egy eszköz nem törölhető, ha egy esemény; használják hello esemény először törölni kell. 

toomanage az objektumok kiválasztása **beállítás** kattintson **eszközök**.

![Objektumok](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

