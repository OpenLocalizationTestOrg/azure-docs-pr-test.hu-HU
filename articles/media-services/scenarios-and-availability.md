---
title: "Azure Media Services aaaMicrosoft forgatókönyvek és funkciók üzemeltetésében rendelkezésre állását |} Microsoft Docs"
description: "Ez a témakör a Microsoft Azure Media Services-forgatókönyvek áttekintését és a funkciók és szolgáltatások rendelkezésre állását mutatja be az egyes adatközpontokban."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 3dbab6998ed5da738baf8f1e2fb096dfba336e19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a>Forgatókönyvek és a Media Services-szolgáltatások rendelkezésre állása az egyes adatközpontokban

A Microsoft Azure Media Services (AMS) lehetővé teszi a toosecurely feltöltési, tárolásához, kódolása és video- vagy tartalom csomag mind igény szerinti és élő adatfolyam-továbbítási kézbesítési toovarious ügyfelek (például TV, számítógépek és mobileszközök).

AMS hello világ több különböző adatközponthoz működik. Ezekben az adatközpontokban toogeographic régiókból, így rugalmasságot helyének kiválasztása vannak csoportosítva toobuild az alkalmazások. Tekintse át hello [régiók és a helyek listáját](https://azure.microsoft.com/regions/). 

Ezen témakör a tartalmak [élő](#live_scenarios) vagy [igény szerinti](#vod_scenarios) továbbításának leggyakoribb eseteit mutatja be. hello is a témakör az adathordozó-szolgáltatások és szolgáltatások rendelkezésre állási adatait adatközpontok között.

## <a name="overview"></a>Áttekintés

### <a name="prerequisites"></a>Előfeltételek

az Azure Media Services toostart hello következő szükséges:

* Egy Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com).
* Egy Azure Media Services-fiók. További információ: [Fiók létrehozása](media-services-portal-create-account.md)
* adatfolyam-továbbítási végpontra, amelyből el kívánja toostream tartalom hello toobe rendelkezik hello **futtató** állapotát.

    Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás, az adatfolyam-továbbítási végpontra hello tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart toobe rendelkezik hello **futtató** állapotát.

### <a name="commonly-used-objects-when-developing-against-hello-ams-odata-model"></a>Leggyakrabban használt objektumok, ha hello AMS OData modell történő fejlesztésről

hello következő kép bemutatja a leggyakrabban használt hello objektumok hello Media Services OData modellre fejlesztése során.

Kattintson a hello kép tooview, teljes méret.  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

Megtekintheti a teljes minta hello [Itt](https://media.windows.net/API/$metadata?api-version=2.15).  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-hello-clear-non-encrypted"></a>Tartalom védelme a tárolón és kézbesítése adatfolyamokat a hello törlése (titkosítatlan)

![VoD-munkafolyamat](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. Töltsön fel egy kiváló minőségű médiafájlt egy adategységbe.

    Ajánlott tooapply tooyour tárolási titkosítási beállítás eszköz rendelés tooprotect során a tartalmat feltöltés és tárolás közben során.
2. Kódolja adaptív sávszélességű MP4-fájlsorozattá tooa készletét.

    Ajánlott tooapply tárolási titkosítási beállítás toohello kimeneti rendelés tooprotect az eszköz a tartalmat tárolás közben.
3. Konfigurálja az adategység továbbítási házirendjét (amelyet a dinamikus csomagolás használ).

    Ha az adategységen tárolótitkosítást alkalmaz, konfigurálnia **kell** az adategység továbbítási házirendjét.
4. Tegye közzé a hello adategységet egy OnDemand-kereső létrehozásával.
5. Továbbítsa a közzétett tartalmat.

Rendelkezésre állás biztosításához az adatközpontok kapcsolatos információkért lásd: hello [rendelkezésre állási](#availability) szakasz.

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Tartalom védelme a tárolón és dinamikusan titkosított folyamatos médiatovábbítás

![Védelem biztosítása a PlayReadyvel](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. Töltsön fel egy kiváló minőségű médiafájlt egy adategységbe. Tárolás titkosítási beállítás toohello eszköz alkalmazni.
2. Kódolja adaptív sávszélességű MP4-fájlsorozattá tooa készletét. Tárolás titkosítási beállítás toohello kimeneti adategységen alkalmazni.
3. Titkosítási tartalomkulcsot hello eszköz toobe lejátszás során dinamikusan titkosítani szeretné létrehozni.
4. Konfigurálja a tartalomkulcs-engedélyezési házirendet.
5. Konfigurálja az adategység továbbítási házirendjét (amelyet a dinamikus csomagolás és a dinamikus titkosítás használ).
6. Tegye közzé a hello adategységet egy OnDemand-kereső létrehozásával.
7. Továbbítsa a közzétett tartalmat.

Rendelkezésre állás biztosításához az adatközpontok kapcsolatos információkért lásd: hello [rendelkezésre állási](#availability) szakasz.

## <a name="use-media-analytics-tooderive-actionable-insights-from-your-videos"></a>Media Analytics tooderive elemzéseket készítsenek videofájljaikból videók használata

Médiaelemzés beszéd- és vizuális összetevők, amelyek megkönnyítik a szervezetek és vállalatok számára tooderive gyakorlatban használható elemzések készítsenek gyűjteménye. További információk: [Az Azure Media Services Elemző áttekintése](media-services-analytics-overview.md)

1. Töltsön fel egy kiváló minőségű médiafájlt egy adategységbe.
2. Hello ismertetett hello Médiaelemzés-szolgáltatások egyikét a videók feldolgozásához [Media Analytics áttekintése](media-services-analytics-overview.md) szakasz.
3. A Médiaelemzés médiafeldolgozói MP4- vagy JSON-fájlokat hoznak létre. Ha egy media processzor MP4-fájlokat, hello fájl fokozatosan lehet letölteni. Ha egy media processzor egy JSON-fájl, hello fájlt letöltheti hello Azure blob Storage tárolóban.

Rendelkezésre állás biztosításához az adatközpontok kapcsolatos információkért lásd: hello [rendelkezésre állási](#availability) szakasz.

## <a name="deliver-progressive-download"></a>Progresszív letöltés továbbítása

1. Töltsön fel egy kiváló minőségű médiafájlt egy adategységbe.
2. A kódolás kimenete egyetlen MP4-fájl tooa.
3. Tegye közzé a hello adategységet egy OnDemand- vagy SAS-kereső létrehozásával.

    SAS-kereső használata esetén hello tartalmat letölti hello Azure blob Storage tárolóban. Ebben az esetben nem kell toohave adatfolyam-végpontok elindított állapotban.
4. Töltse le fokozatosan a tartalmat.

## <a id="live_scenarios"></a>Események élő közvetítése 

1. Élő tartalmakat dolgozhat fel különböző élő streamelési protokollok (például RTMP vagy Smooth Streaming) használatával.
2. A streamet adaptív sávszélességűvé kódolhatja (opcionális).
3. Megtekintheti az élő stream előnézetét.
4. hello tartalom gyakori adatfolyam-továbbítási protokollok (például MPEG DASH, Smooth, HLS) keresztül közvetlenül tooyour ügyfelek vagy a Content Delivery Network (CDN) tooa későbbi terjesztés.

    – vagy –

    Rendelés toobe rekord és a tároló okozhatnak hello tartalma folyamatos átviteli újabb (Video-on-Demand).

Ennek az élő Stream továbbítása során, beállíthatja irányítja a következő hello:

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Helyszíni kódolóktól többszörös átviteli sebességű adatfolyamot fogadó (áteresztő) csatornák használata

hello következő diagramon láthatók hello hello AMS platform fontosabb részei, amelyek szerepet játszanak az hello **áteresztő** munkafolyamat.

![Élő munkafolyamat](./media/scenarios-and-availability/media-services-live-streaming-current.png)

Tovább információk: [Helyszíni kódolóktól többféle sávszélességű adatfolyamot fogadó csatornák használata](media-services-live-streaming-with-onprem-encoders.md)

### <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a>Csatornákat végzett élő kódolás az Azure Media Services tooperform engedélyezve

hello alábbi ábrán látható fő hello hello AMS platform részei, amelyek szerepet játszanak az élő adatfolyam-továbbítási munkafolyamat ahol a csatorna egy olyan élő kódolás Media Services tooperform engedélyezve van.

![Élő munkafolyamat](./media/scenarios-and-availability/media-services-live-streaming-new.png)

További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Rendelkezésre állás biztosításához az adatközpontok kapcsolatos információkért lásd: hello [rendelkezésre állási](#availability) szakasz.

## <a name="consuming-content"></a>Tartalmak felhasználása

Azure Media Services biztosította hello eszközök segítségével kell toocreate gazdag, dinamikus ügyféloldali lejátszóalkalmazások a legtöbb platformra, többek között: iOS-eszközök, Android-eszközök, Windows, Windows Phone, Xbox és dekóder jelölőnégyzetéből. hello a következő témakör nyújt hivatkozások tooSDKs és lejátszó-Keretrendszerekhez, amelyeket felhasználhat toodevelop a saját ügyfélalkalmazásait a Media Services médiafolyamot médiafolyamainak fogadására. További információkért lásd a [videólejátszó alkalmazások fejlesztését](media-services-develop-video-players.md) ismertető cikket.

## <a name="enabling-azure-cdn"></a>Az Azure CDN engedélyezése

A Media Services támogatja az Azure CDN-integrációt. Információ tooenable Azure CDN, lásd: [hogyan tooManage adatfolyam-továbbítási végpontok Media Services-fiók](media-services-portal-manage-streaming-endpoints.md).

## <a id="scaling"></a>Media Services-fiók méretezése

Az AMS-ügyfelek méretezhetik a streamvégpontokat, a médiafeldolgozást és a tárolást az AMS-fiókjukon.

* A Media Services ügyfelei **standard** szintű streamvégpontot vagy **prémium** szintű streamvégpontot választhatnak. A **standard** streamvégpont a legtöbb streamelési feladat ellátására alkalmas. Ugyanaz, mint szolgáltatások hello tartalmaz egy **prémium** automatikusan adatfolyam-végpontok és méretezik kimenő sávszélesség. 

    A **prémium** szintű streamvégpontok a speciális feladatokhoz ideálisak, mert dedikált és méretezhető sávszélesség-kapacitást nyújtanak. A **prémium** streamvégponttal rendelkező ügyfelek alapértelmezés szerint kapnak egy adategységet (SU-t). adatfolyam-továbbítási végpontra hello SUs hozzáadásával is méretezhető. Minden egyes SU további sávszélesség kapacitás toohello alkalmazásokat tartalmaz. További információt a skálázás **prémium szintű** hello adatfolyam-végpontok, lásd: [streamvégpontok skálázás](media-services-portal-scale-streaming-endpoints.md) témakör.

* Egy Media Services-fiók fenntartott egység típusú, amely megadja, hogy hello sebesség, amellyel a feladatok feldolgozása media feldolgozása hozzá rendelve. Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: **S1**, **S2**, vagy **S3**. Például ugyanazon kódolási feladat fut gyorsabban hello használatakor hello **S2** fenntartott egységnek típus összehasonlítása toohello **S1** típusa.

    Ezenkívül toospecifying hello fenntartott egység típusát, akkor megadhatja tooprovision fiókját **fenntartott egységek** (RUs). kiépített RUs hello száma határozza meg, egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok hello száma.

    >[!NOTE]
    >A Fenntartott egységek az összes médiafeldolgozás párhuzamossá tételéért felelősek, beleértve az Azure Media Indexerrel végzett indexelési feladatokat is. De a kódolással ellentétben az indexelési feladatok feldolgozása nem lesz gyorsabb a gyorsabb Fenntartott egységekkel.

    További információkért olvassa el a [médiafeldolgozás méretezését](media-services-portal-scale-media-processing.md) ismertető cikket.
* A Media Services-fiók storage-fiókok tooit hozzáadásával is méretezheti. Minden tárfiók legfeljebb too500 TB kerül. tooexpand túl hello alapértelmezett korlátozások tárhelyét választhat tooattach több tároló fiókok tooa egyetlen Media Services-fiók. További információkért olvassa el a [tárfiókok kezelését](meda-services-managing-multiple-storage-accounts.md) ismertető cikket.

##<a id="availability"></a> A Media Services-funkciók rendelkezésre állása az egyes adatközpontokban

Ez a szakasz a Media Services-funkciók az adatközpontok közötti rendelkezésre állásáról nyújt részleteket.

### <a name="ams-accounts"></a>AMS-fiókok

#### <a name="availability"></a>Rendelkezésre állás

A következő régiókban hello Media Services-fiókot hozhat létre: Észak-Európában, Nyugat-Európa, USA nyugati régiója, USA keleti régiója, Délkelet-Ázsiában, Kelet-Ázsia, Nyugat-japán, kelet-japán, Dél-Brazília, Nyugat-India, Dél-India és közép-India. 

### <a name="streaming-endpoints"></a>Streamvégpontok 

A Media Services ügyfelei **standard** szintű streamvégpontot vagy **prémium** szintű streamvégpontot választhatnak. További információkért lásd: hello [skálázás](#scaling) szakasz.

#### <a name="availability"></a>Rendelkezésre állás

|Név|status|Adatközpontok
|---|---|---|
|Standard|FE|Összes|
|Prémium|FE|Összes|

### <a name="live-encoding"></a>Live Encoding

#### <a name="availability"></a>Rendelkezésre állás

Az összes adatközpontban elérhető a következők kivételével: Németország, Dél-Brazília, Nyugat-India, Dél-India és Közép-India. 

### <a name="encoding-media-processors"></a>Médiafeldolgozók kódolása

Az AMS két igény szerinti kódolót nyújt: a **Media Encoder Standard** kódolót és a **Media Encoder Premium-munkafolyamatot**. További információkért olvassa el [az Azure igény szerinti médiakódolók áttekintését és összehasonlítását](media-services-encode-asset.md). 

#### <a name="availability"></a>Rendelkezésre állás

|Médiafeldolgozó neve|status|Adatközpontok
|---|---|---|
|Media Encoder Standard|FE|Összes|
|Media Encoder Premium-munkafolyamat|FE|Kína kivételével|

### <a name="analytics-media-processors"></a>Elemzési médiafeldolgozók

Médiaelemzés beszéd- és vizuális összetevők gyűjteménye, amely egyszerűbbé teszi a szervezetek és vállalatok számára tooderive gyakorlatban használható elemzések készítsenek. További információk: [Az Azure Media Services Elemző áttekintése](media-services-analytics-overview.md)

#### <a name="availability"></a>Rendelkezésre állás

|Médiafeldolgozó neve|status|Adatközpontok
|---|---|---|
|Azure Media Face Detector|Előzetes verzió|Összes|
|Azure Media Hyperlapse|Előzetes verzió|Összes|
|Azure Media Indexer|FE|Összes|
|Azure Media Motion Detector|Előzetes verzió|Összes|
|Azure Media OCR|Előzetes verzió|Összes|
|Azure Media Redactor|Előzetes verzió|Összes|
|Azure Media Stabilizer|Előzetes verzió|Összes|
|Azure Media Video Thumbnails|Előzetes verzió|Összes|
|Azure Media Indexer 2|Előzetes verzió|Kína és a szövetségi kormányzati régió kivételével|

### <a name="protection"></a>Védelem

Microsoft Azure Media Services lehetővé teszi, hogy Ön toosecure hello idő keresztül tárhely, feldolgozás és kézbesítési elhagyják a adathordozókról. További információért olvassa el az [AMS-tartalmak védelmét](media-services-content-protection-overview.md) ismertető cikket.

#### <a name="availability"></a>Rendelkezésre állás

|Titkosítás|status|Adatközpontok|
|---|---|---| 
|Tárolás|FE|Összes|
|AES-128-kulcsok|FE|Összes|
|FairPlay|FE|Összes|
|PlayReady|FE|Összes|
|Widevine|FE|Mindenhol, kivéve Németországot, a szövetségi kormányzati régiót és Kínát.

### <a name="reserved-units-rus"></a>Fenntartott egységek (RU-k)

kiépített fenntartott egységek számának hello hello egyidejűleg dolgozhatók fel egy adott fiókhoz media feladatok száma határozza meg. 

További információkért lásd: hello [skálázás](#scaling) szakasz.

#### <a name="availability"></a>Rendelkezésre állás

Minden adatközpontban elérhető.

### <a name="reserved-unit-ru-type"></a>Fenntartott egység (RU) típusa

Egy Media Services-fiók fenntartott egységnek típusú, amely megadja, hogy hello sebesség, amellyel a feladatok feldolgozása media feldolgozása hozzá rendelve. Hello következő közötti választhatja ki a szolgáltatás számára fenntartott egység: S1, S2 vagy S3.

További információkért lásd: hello [skálázás](#scaling) szakasz.

#### <a name="availability"></a>Rendelkezésre állás

|RU típusának neve|status|Adatközpontok
|---|---|---|
|S1|FE|Összes|
|S2|FE|Mindenhol, kivéve Dél-Brazíliát és Nyugat-Indiát|
|S3|FE|Mindenhol, kivéve Nyugat-Indiát|

## <a name="next-steps"></a>Következő lépések

Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

