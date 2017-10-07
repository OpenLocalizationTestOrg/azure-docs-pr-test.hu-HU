---
title: "aaaGet használatába kézbesítéséhez VoD hello Azure-portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello egy alapszintű Video-on-Demand (VoD) tartalomtovábbító service végrehajtási hello Azure-portál használatával Azure Media Services (AMS) alkalmazással."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 5c1c1b1f74ec1f1301120fe8e5a5ae183fe0338f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-hello-azure-portal"></a>Ismerkedés az Azure-portálon hello segítségével igény szerinti tartalomtovábbítás
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Ez az oktatóanyag végigvezeti hello egy alapszintű Video-on-Demand (VoD) tartalomtovábbító service végrehajtási hello Azure-portál használatával Azure Media Services (AMS) alkalmazással.

## <a name="prerequisites"></a>Előfeltételek
Az alábbiakban hello szükséges toocomplete hello oktatóanyag:

* Egy Azure-fiók. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
* Egy Media Services-fiók. egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).

Ez az oktatóanyag hello a következő feladatokat tartalmazza:

1. Indítsa el a streamvégpontot.
2. Videofájl feltöltése
3. Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlokat alakítja.
4. Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-cím.  
5. Tartalom lejátszása

## <a name="start-streaming-endpoints"></a>Streamvégpontok elindítása 

Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett használatával adatfolyam adaptív sávszélességű streamelést működik. A Media Services dinamikus becsomagolást biztosít, amely lehetővé teszi toodeliver az adaptív sávszélességű MP4-kódolású tartalmak anélkül, hogy előre csomagolt toostore (MPEG DASH, HLS, Smooth Streaming), a Media Services just-in-time, által támogatott streamformátumok Ezekbe a formátumokba egyes verzióit.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

toostart hello streamvégpontra, a következő hello:

1. Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).
2. Hello-beállítások ablakában kattintson a Streaming végpontok. 
3. Kattintson a hello alapértelmezett streamvégpontra. 

    hello alapértelmezett STREAMING ENDPOINT részletek ablak.

4. Kattintson a hello Start ikonra.
5. Kattintson a hello Mentés gombra toosave a módosításokat.

## <a name="upload-files"></a>Fájlok feltöltése
toostream tooupload hello forrás videók, kell a videók Azure Media Services használatával, kódolás több bitrates be, és tegye közzé a hello eredménye. Ebben a szakaszban foglalt hello első lépése. 

1. A hello **beállítás** ablak, kattintson a **eszközök**.
   
    ![Fájlok feltöltése](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Kattintson a hello **feltöltése** gombra.
   
    Hello **töltse fel a video asset** ablak jelenik meg.
   
   > [!NOTE]
   > Tetszőleges méretű fájlt választhat.
   > 
   > 
3. Keresse meg a szükséges toohello videót a számítógépen, válassza ki azt, és kattintson az OK gombra.  
   
    hello feltöltés elindul, és megtekintheti a hello fájlnév hello folyamatban.  

Hello feltöltés befejezését követően megjelenik-e hello új objektum bekerül hello **eszközök** ablak. 

## <a name="encode-assets"></a>Objektumok kódolása

Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett az adaptív sávszélességű streamelési tooyour ügyfelek működik. Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH. tooprepare videók adaptív sávszélességű streamelés esetén meg kell tooencode a forrás videó többszörös sávszélességű fájlokba. Használjon hello **Media Encoder Standard** kódoló tooencode videók.  

Media Services dinamikus becsomagolást is biztosít, amely lehetővé teszi toodeliver közvetíteni többszörös sávszélességű MP4 a hello a következő formátumban: MPEG DASH, HLS, Smooth Streaming, anélkül, hogy az ezekbe a formátumokba toorepackage. A dinamikus csomagolás csak akkor kell toostore és hello fájlokat egyetlen tárolási formátumban és a Media Services fizetési alapszik, és betölti az ügyféltől érkező kérésnek megfelelő választ hello.

tootake előny dinamikus becsomagolás kell tooencode a forrásfájlt (hello kódolás lépéseit egy későbbi részében) többszörös sávszélességű MP4-fájlokat alakítja.

### <a name="toouse-hello-portal-tooencode"></a>toouse hello portál tooencode
Ez a szakasz ismerteti a hello lépéseket, amelyek tooencode a tartalmaknak a Media Encoder Standard.

1. A hello **beállítások** ablakban válassza ki **eszközök**.  
2. A hello **eszközök** ablakot, hogy szeretné-e tooencode válassza hello eszköz.
3. Nyomja le az hello **Encode** gombra.
4. A hello **kódolása egy eszköz** ablak, jelölje be hello "Media Encoder Standard" feldolgozóeszközt, és egy beállításkészletet. A beállításkészletekkel kapcsolatos információkért lásd [a sávszélességi skála automatikus létrehozását](media-services-autogen-bitrate-ladder-with-mes.md) és a [MES előre beállított feladatait](media-services-mes-presets-overview.md) ismertető részeket. Ha azt tervezi, hogy mely kódolási beállításkészlet használt toocontrol, ezt tartsa szem előtt: fontos tooselect hello készlet, amely a legjobban megfelelő a bemeneti videó. Például ha tudja, hogy a bemeneti videó felbontása 1920 x 1080 képpont rendelkezik, akkor használja hello "H264 Multiple Bitrate 1080p" beállításkészletet. Ha például alacsony felbontású (640 x 360) videót szeretne kódolni, ne válassza a „H264 Multiple Bitrate 1080p” beállításkészletet.
   
   Az egyszerűbb kezelés érdekében lehetősége van a szerkesztési hello hello kimeneti eszköz, és hello nevét hello feladat.
   
   ![Objektumok kódolása](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Nyomja meg a **Create** (Létrehozás) gombot.

### <a name="monitor-encoding-job-progress"></a>Kódolási feladatok előrehaladásának figyelése
feladat, kódolás hello toomonitor hello előrehaladását kattintson **beállítások** (hello hello oldal tetején), és válassza **feladatok**.

![Feladatok](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Tartalom közzététele
tooprovide a felhasználó egy URL-cím használt toostream vagy letölteni a tartalmat, akkor először kell túl "közzététele" az eszköz egy kereső létrehozásával. Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel. A Media Services két lokátortípust támogat: 

* Streamelési (OnDemandOrigin) lokátorokat, amelyek adaptív streameléshez (például toostream MPEG DASH, HLS vagy Smooth Streaming) használt. toocreate a streamelési lokátorok létrehozásához az eszköz tartalmaznia kell egy .ism-fájlt. 
* Progresszív (SAS-) lokátorokat, amelyek a videó progresszív letöltésen keresztül történő továbbítására használatosak.

A streamelési URL-cím formátuma a következő hello rendelkezik, és használata tooplay Smooth Streaming-objektumok.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

toobuild HLS-streamelési URL-cím, egy hozzáfűző (formátum = m3u8-aapl) toohello URL-CÍMÉT.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

egy streamelési URL-cím, MPEG DASH toobuild hozzáfűzése (formátum mpd-idő-csf =) toohello URL-CÍMÉT.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Egy SAS URL-címnek a következő formátumban hello.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Ha korábban használt hello portál toocreate keresők 2015. márciusi, keresők kétéves lejárati dátummal jöttek létre.  
> 
> 

a lokátor, használja a lejárati dátum tooupdate [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-k. Amikor frissíti egy SAS-kereső hello lejárati dátuma, hello URL-cím módosítja.

### <a name="toouse-hello-portal-toopublish-an-asset"></a>toouse hello portál toopublish egy eszköz
toouse hello portál toopublish egy eszköz hello a következő:

1. Válassza a **Settgings (Beállítások)** > **Assets (Objektumok)** lehetőséget.
2. Válassza ki a megjeleníteni kívánt toopublish hello eszközt.
3. Kattintson a hello **közzététel** gombra.
4. Válassza ki a hello lokátor típusát.
5. Nyomja meg az **Add** (Hozzáadás) gombot.
   
    ![Közzététel](./media/media-services-portal-vod-get-started/media-services-publish1.png)

hello URL-cím kerül toohello listája **közzétett URL-címek**.

## <a name="play-content-from-hello-portal"></a>Hello portálról tartalom lejátszása
hello Azure-portálon talál egy tartalomlejátszót használható tootest a videót.

Kattintson a kívánt hello videó, majd hello **lejátszása** gombra.

![Közzététel](./media/media-services-portal-vod-get-started/media-services-play.png)

Vegye figyelembe a következőket:

* adatfolyam-, toobegin start futó hello **alapértelmezett** streamvégpontra.
* Ellenőrizze, hogy a videó hello is közzé lett téve.
* Ez **Media player** hello alapértelmezett streamvégpontból játssza le. Ha azt szeretné, hogy a nem alapértelmezett tooplay az adatfolyam-továbbítási végpontra, kattintson a toocopy hello URL-címet, és használjon másik lejátszót. Például az [Azure Media Services lejátszót](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

