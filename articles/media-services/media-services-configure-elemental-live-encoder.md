---
title: "aaaConfigure hello elemi élő kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello elemi élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 9a5de6189bfb123768a9da038b8c8db69cf85e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-elemental-live-encoder-toosend-a-single-bitrate-live-stream"></a>Hello elemi élő kódoló toosend egyféle sávszélességű élő adatfolyamot használata
> [!div class="op_single_selector"]
> * [Élő elemi](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Ez a témakör bemutatja, hogyan tooconfigure hello [elemi élő](http://www.elementaltechnologies.com/products/elemental-live) kódoló toosend egyszeres sávszélességű adatfolyam-tooAMS élő kódolásra képes csatornák.  További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel. Ez az eszköz csak a Windows rendszerű Számítógépeken fut. Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Előfeltételek
* Rendelkeznie kell egy ismeretét elemi élő webes felület toocreate élő eseményeket használ.
* [Azure Media Services-fiók létrehozása](media-services-portal-create-account.md)
* Gondoskodjon arról, hogy fut egy adatfolyam-végpontot. További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md).
* Hello hello legújabb verziójának telepítéséhez [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.
* Hello eszköz indítása, és csatlakozzon a tooyour AMS-fiók.

## <a name="tips"></a>Tippek
* Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.
* Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor streaming bitrates toodouble hello. Ez nem kötelezők, ez segít csökkenteni hello hatását a hálózati torlódás.
* Ha szoftverrel kódolók, zárja be az el a felesleges programokat.

## <a name="elemental-live-with-rtp-ingest"></a>A RTP elemi Live betöltési
Ez a szakasz bemutatja, hogyan tooconfigure hello elemi élő kódoló által küldött egy egyféle sávszélességű élő keresztül RTP adatfolyamot.  További információkért lásd: [MPEG-TS stream RTP keresztül](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Csatorna létrehozása

1. Hello AMSE eszköz, lépjen a toohello **Live** lapot, és hello csatorna területen kattintson a jobb gombbal. Válassza ki **csatorna létrehozása...** hello menüből.

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni. A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTP (MPEG-TS)**. A többi beállítás, hagyhatja.

    Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.

3. Kattintson a **csatornát létrehozni**.

   ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> hello csatorna mindaddig toostart 20 percet is igénybe vehet.
>
>

Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

> [!IMPORTANT]
> Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül. További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

### <a id=configure_elemental_rtp></a>Hello elemi élő kódoló konfigurálása
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

#### <a name="configuration-steps"></a>Konfigurációs lépések
1. Keresse meg a toohello **elemi élő** webes felület, és állítsa be a hello kódoló **UDP/TS** streaming.
2. Új esemény létrehozása után görgessen lefelé toohello kimeneti csoportok, és adja hozzá a hello **UDP/TS** kimeneti csoport.
3. Hozzon létre egy új kimeneti kiválasztásával **új adatfolyam** , majd **hozzáadása kimeneti**.  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > Ajánlott, hogy hello elemi esemény hello időkód túl beállítása "Rendszeróra" toohelp hello kódoló újracsatlakozás hello esetben, ha az adatfolyam nem.
   >
   >
4. Most, hogy hello kimeneti létrehozása után kattintson **adatfolyam adható hozzá**. hello kimeneti beállításait mostantól konfigurálható.
5. Görgessen lefelé toohello "Stream 1", amely éppen most hozták létre, kattintson a hello **videó** hello bal oldali lapon, és bontsa ki a hello **speciális** beállítások szakaszban.

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Míg elemi élő számos elérhető testreszabása, hello következő beállítások használata ajánlott streaming tooAMS való ismerkedésre.

   * Megoldás: 1280 x 720
   * Képkockasebesség: 30
   * GOP mérete: 60 keretek
   * Váltott mód: fokozatos
   * Átviteli sebesség: 5000000 bit/mp (ezt úgy kell beállítani a hálózati korlátozás)

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. Hello csatorna bemeneti URL-cím beszerzése.

    Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát. Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.

    Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. Illessze be ezeket az információkat a hello **elsődleges cél** hello elemi mezőjében. A többi beállítás hello alapértelmezett maradjanak.

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    További redundancia érdekében ismételje meg ezeket a lépéseket hello másodlagos bemeneti URL-cím UDP/TS Streaming hozzon létre egy külön "Kimeneti" lapon.
3. Kattintson a **létrehozása** (Ha új esemény létrehozása) vagy **frissítés** (Ha egy már meglévő esemény szerkesztése) és folytathatja a toostart hello kódoló.

> [!IMPORTANT]
> Végül kattintson **Start** a hello elemi élő webes felület, akkor **kell** győződjön meg arról, hogy készen áll-e hello csatorna.
> Győződjön meg arról is, nem tooleave hello csatorna nélkül > 15 percnél hosszabb ideig esemény üzemkész állapotban.
>
>

Után hello adatfolyam 30 másodpercig fut, nyissa meg a visszafelé toohello AMSE eszköz és a vizsgálati lejátszás.  

### <a name="test-playback"></a>Teszt lejátszás

Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve. Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.

Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell. Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.   

### <a name="create-a-program"></a>Hozzon létre egy programot
1. Miután csatorna lejátszás megerősítjük, hozzon létre egy programot. A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. Hello program neve, és ha szükséges, módosítsa a hello **az archiválási időtartam** (mely alapértelmezett too4 óra). Adjon meg olyan tárolási helyen vagy hagyja hello alapértelmezett is.  
3. Ellenőrizze a hello **Start hello Program most** mezőbe.
4. Kattintson a **hozzon létre programot**.  

    >[!NOTE]
    > Program létrehozása gyorsabb a csatorna létrehozása.   
      
5. Ha hello program fut, erősítse meg a lejátszás hello program jobb gombbal kattint rá, és a Navigálás túl**lejátszás hello programokról** és jelölje be **Azure Media Player**.  
6. Ha megerősítette, hello program ismét kattintson jobb gombbal, majd válassza ki **hello kimeneti URL-cím tooClipboard másolja** (vagy az adatok lekérését hello **Program információk és beállítások** hello menüből beállítás).

hello adatfolyama most már készen áll a player ágyazott toobe, és az elosztott tooan célközönség élő megtekintése.  

## <a name="troubleshooting"></a>Hibaelhárítás
Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
