---
title: "aaaConfigure hello Telestream Wirecast kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello Wirecast élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e373f6c08232c652e65db584ded409c405d8cffe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-wirecast-encoder-toosend-a-single-bitrate-live-stream"></a>Hello Wirecast kódoló toosend egyféle sávszélességű élő adatfolyamot használata
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Élő elemi](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Ez a témakör bemutatja, hogyan tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás.  További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel. Ez az eszköz csak a Windows rendszerű Számítógépeken fut. Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Előfeltételek
* [Azure Media Services-fiók létrehozása](media-services-portal-create-account.md)
* Gondoskodjon arról, hogy fut egy adatfolyam-végpontot. További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)
* Hello hello legújabb verziójának telepítéséhez [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.
* Hello eszköz indítása, és csatlakozzon a tooyour AMS-fiók.

## <a name="tips"></a>Tippek
* Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.
* Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor streaming bitrates toodouble hello. Ez nem kötelezők, ez segít csökkenteni hello hatását a hálózati torlódás.
* Ha szoftverrel kódolók, zárja be az el a felesleges programokat.

## <a name="create-a-channel"></a>Csatorna létrehozása
1. Hello AMSE eszköz, lépjen a toohello **Live** lapot, és hello csatorna területen kattintson a jobb gombbal. Válassza ki **csatorna létrehozása...** hello menüből.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni. A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTMP**. A többi beállítás, hagyhatja.

    Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.

3. Kattintson a **csatornát létrehozni**.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> hello csatorna mindaddig toostart 20 percet is igénybe vehet.
>
>

Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül. További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_wirecast_rtmp></a>Hello Telestream Wirecast kódoló konfigurálása
Az oktatóanyag hello következő kimeneti beállításokat használ. Ez a szakasz többi hello részletes konfigurációs lépéseit ismerteti.

**Videó**:

* Kodek: H.264
* Profil: High (szint 4.0-s)
* Átviteli sebesség: 5000 KB/s
* Kulcskocka: 2 másodperc (60 másodperc)
* Keret aránya: 30

**Hang**:

* A kodek: AAC (LC)
* Átviteli sebesség: 192 Kbit/s
* Mintavételi gyakoriság: 44,1 kHz

### <a name="configuration-steps"></a>Konfigurációs lépések
1. Nyissa meg a hello Telestream Wirecast alkalmazás hello gép éppen használja, és állítsa be az RTMP adatfolyamként történő.
2. Hello kimeneti konfigurálása toohello navigálva **kimeneti** fülre, és kattintson **kimeneti beállításai...** .

    Győződjön meg arról, hogy hello **kimeneti cél** értéke túl**RTMP Server**.
3. Kattintson az **OK** gombra.
4. Hello-beállítások lapján állítsa be a hello **cél** mező toobe **Azure Media Services**.

    hello kódolás profil előre be van jelölve túl**Azure H.264 720 p 16:9 (1280 x 720)**. toocustomize ezeket a beállításokat, jelölje be hello fogaskerék ikonra toohello sarkában hello legördülő listán, és válassza a **új készletet**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Kódoló készletek beállítása.

    Név hello az adott néven beállítás, és ellenőrizze hello következő ajánlott beállítások:

    **Videó**

   * Kódoló: MainConcept H.264
   * A képkockák másodpercenkénti: 30
   * Átlagos átviteli sebessége: 5000 kbit/s (módosítani lehet a hálózati korlátozás)
   * Profil: fő
   * Kulcskocka minden: 60 keretek

    **Hang**

   * Cél: 192 Kbit/s
   * Mintavételi gyakoriság: 44.100 kHz

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Nyomja le az **mentése**.

    hello Encoding mező már kijelölésnél érhetők el az újonnan létrehozott hello-profil.

    Győződjön meg arról, hogy ki van jelölve hello új profil.
7. Hello csatorna bemeneti URL-cím beszerzése a rendelés tooassign azt toohello Wirecast **RTMP végpont**.

    Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát. Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.

    Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. A hello Wirecast **kimeneti beállításainak** ablakban illessze be ezeket az információkat a hello **cím** hello output szakasz, és rendelje hozzá egy adatfolyam neve mezőjében.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. Kattintson az **OK** gombra.
2. A fő hello **Wirecast** képernyőjén ellenőrizze a bemeneti forrás a videó és hang készen áll, majd nyomja le **adatfolyam** hello felső bal oldali sarokban.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Kattintás előtt **adatfolyam**, hogy **kell** győződjön meg arról, hogy készen áll-e hello csatorna.
> Győződjön meg arról is, nem tooleave hello egy bemeneti hozzájárulás nélkül üzemkész állapotban csatorna hírcsatorna > 15 percnél hosszabb ideig.
>
>

## <a name="test-playback"></a>Teszt lejátszás

Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve. Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.

Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell. Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.  

## <a name="create-a-program"></a>Hozzon létre egy programot
1. Miután csatorna lejátszás megerősítjük, hozzon létre egy programot. A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. Hello program neve, és ha szükséges, módosítsa a hello **az archiválási időtartam** (mely alapértelmezett too4 óra). Adjon meg olyan tárolási helyen vagy hagyja hello alapértelmezett is.  
3. Ellenőrizze a hello **Start hello Program most** mezőbe.
4. Kattintson a **hozzon létre programot**.  

   >[!NOTE]
   >Program létrehozása gyorsabb a csatorna létrehozása.
       
5. Ha hello program fut, erősítse meg a lejátszás hello program jobb gombbal kattint rá, és a Navigálás túl**lejátszás hello programokról** és jelölje be **Azure Media Player**.  
6. Ha megerősítette, hello program ismét kattintson jobb gombbal, majd válassza ki **hello kimeneti URL-cím tooClipboard másolja** (vagy az adatok lekérését hello **Program információk és beállítások** hello menüből beállítás).

hello adatfolyama most már készen áll a player ágyazott toobe, és az elosztott tooan célközönség élő megtekintése.  

## <a name="troubleshooting"></a>Hibaelhárítás
Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
