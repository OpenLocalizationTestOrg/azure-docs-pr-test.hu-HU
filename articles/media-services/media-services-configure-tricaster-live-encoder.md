---
title: "aaaConfigure hello NewTek TriCaster kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello Tricaster élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 57dcf62a6a76b04e69f147a738be78ccb3c3ecdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-newtek-tricaster-encoder-toosend-a-single-bitrate-live-stream"></a>Hello NewTek TriCaster kódoló toosend egyféle sávszélességű élő adatfolyamot használata
> [!div class="op_single_selector"]
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Élő elemi](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Ez a témakör bemutatja, hogyan tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás. További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel. Ez az eszköz csak a Windows rendszerű Számítógépeken fut. Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> Ha a valós idejű kódolásra képes csatornák tooAMS Tricaster használatával való elküldésére hozzájárulás hírcsatorna, lehet videó/hang hibáktól az élő esemény Tricaster, például a gyors közötti hírcsatornák kivágja, vagy táblagépükkel/való váltás az egyes szolgáltatások használatakor . hello AMS csoport dolgozik megoldásán ezeket a problémákat, amíg ez nem sikerül, nem javasolt ezeket a szolgáltatásokat az toouse.
>
>

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

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni. A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTMP**. A többi beállítás, hagyhatja.

    Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.

3. Kattintson a **csatornát létrehozni**.

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> hello csatorna mindaddig toostart 20 percet is igénybe vehet.
>
>

Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül. További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_tricaster_rtmp></a>Hello NewTek TriCaster kódoló konfigurálása
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
1. Hozzon létre egy új **NewTek TriCaster** projekt attól függően, hogy milyen bemeneti videoforrást használatban van.
2. Egyszer, hogy a projektben található hello **adatfolyam** gombra, majd kattintson a hello fogaskerék ikonra következő tooit tooaccess hello adatfolyam konfigurációs menü.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. Miután megnyílt hello menüben, kattintson a **új** hello kapcsolat címszó alatt. Amikor hello kapcsolattípus megadását kéri, válassza ki a **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. Kattintson az **OK** gombra.
5. Egy FMLE profil most importálható hello legördülő lista alatt nyílra kattintva **Streaming profil** túl nézetre lépne, és**Tallózás**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Keresse meg a toowhere konfigurált hello FMLE profil lett mentve.
7. Jelölje ki, majd nyomja le az ENTER **OK**.

    Hello-profil a feltöltést követően folytassa toohello következő lépésre.
8. Hello csatorna bemeneti URL-cím beszerzése a rendelés tooassign azt toohello Tricaster **RTMP végpont**.

    Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát. Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.

    Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Illessze be ezeket az információkat a hello **hely** eleménél **Flash Server** hello Tricaster projektben. Is hozzárendelhet egy adatfolyam-nevet, a hello **adatfolyam-azonosító** mező.

    Ha adatfolyam információt adott toohello FMLE profilt, azt is importálhatók toothis szakasz kattintva **beállítások importálása**, lépjen a mentett toohello FMLE profil csomópontra, és kattintson a **OK**. hello megfelelő Flash Server mezőt fel kell töltenie FMLE hello adataival.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. Ha befejezte, kattintson a **OK** üdvözlő képernyőt hello alján. Készen áll a video- és ráfordítások hello Tricaster, kezdje tooAMS streaming hello kattintva **adatfolyam** gombra.

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Kattintás előtt **adatfolyam**, hogy **kell** győződjön meg arról, hogy készen áll-e hello csatorna.
> Győződjön meg arról is, nem tooleave hello egy bemeneti hozzájárulás nélkül üzemkész állapotban csatorna hírcsatorna > 15 percnél hosszabb ideig.
>
>

## <a name="test-playback"></a>Teszt lejátszás
Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve. Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.

Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell. Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.  

## <a name="create-a-program"></a>Hozzon létre egy programot
1. Miután csatorna lejátszás megerősítjük, hozzon létre egy programot. A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
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

## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
