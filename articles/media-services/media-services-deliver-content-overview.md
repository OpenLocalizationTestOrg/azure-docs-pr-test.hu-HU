---
title: tartalom toocustomers aaaDelivering |} Microsoft Docs
description: "Ez a témakör áttekintést mi részt vesz a szerinti tartalomtovábbítás az Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 0570bd62d9d42633df0132f9449b357e2abb4086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deliver-content-toocustomers"></a>Tartalom toocustomers biztosításához
Ha a streaming vagy videotartalom tartalom toocustomers rendelkezik, a cél, toodeliver kiváló minőségű videó toovarious eszközök különböző hálózati körülmények.

tooachieve ezen cél esetében is:

* Az adatfolyam tooa többféle sávszélességű (adaptív sávszélességűvé) video-adatfolyammá alakítja kódolása. Ez a minőségi és hálózati körülményekhez kezeli.
* Használja a Microsoft Azure Media Services [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md) toodynamically őket csomagolni az adatfolyam különböző protokollokat. Ez az adatfolyam a különböző eszközökön kezeli. A Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming vagy MPEG-DASH.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

Ez a cikk áttekintést fontos tartalomkézbesítési fogalmakat.

ismert problémák, toocheck lásd: [ismert problémák](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dinamikus csomagolás
A dinamikus csomagolás hello, hogy a Media Services biztosít, a Media Services (MPEG-DASH, HLS, Smooth Streaming) által támogatott streamformátumok adaptív sávszélességű MP4 vagy Smooth Streaming-kódolású tartalmak biztosíthat anélkül, hogy toore-csomagját Ezek formátumban. Azt javasoljuk, hogy a dinamikus becsomagolás révén a tartalmak továbbításával.

tootake előny dinamikus becsomagolás tooencode a mezzanine (forrás) fájl szükséges adaptív sávszélességű MP4-fájlokká vagy Smooth Streaming-fájlsorozattá be.

A dinamikus csomagolás tárolja, és egyetlen tárolási formátumban hello fájlok díj ellenében. A Media Services elkészíti és kiszolgálja hello a kérésnek megfelelő választ.

A dinamikus csomagolás standard és premium adatfolyam-végpontok érhető el. 

További információkért lásd: [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Szűrők és dinamikus jegyzékfájlokban
A Media Services eszközök szűrőket adhat meg. Ezek a szűrők és kiszolgálóoldali szabályok, amelyeket az ügyfelek számára, például egy adott részének videó lejátszása, vagy adjon meg egy részhalmazát, amelyet a felhasználói eszköz kezelni tud (helyett minden hello interpretációk hello eszköz társított hang- és interpretációk ). A szűrés sorrendekben *dinamikus jegyzékfájlokban* , amely jönnek létre, ha a felhasználói kérelmek toostream videó alapján egy vagy több megadott szűrők.

További információkért lásd: [szűrőket és dinamikus jegyzékfájlokban](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Keresők
tooprovide a felhasználó használt toostream vagy a tartalom letöltésére URL-címet, először toopublish az objektumot egy kereső létrehozásával. Egy kereső biztosít egy belépési pont tooaccess hello egy eszköz található fájlokat. A Media Services két lokátortípust támogat:

* OnDemandOrigin keresők. Ezek a használt toostream adathordozó (például MPEG-DASH, HLS vagy Smooth Streaming), vagy fokozatosan letölteni a fájlokat.
* Közös hozzáférésű jogosultságkód (SAS) URL-cím lokátorokat. Ezek a használt toodownload media fájlok tooyour helyi számítógépen.

Egy *házirendhez* van használt toodefine hello engedélyek (például az olvasási, írási és lista) és az időtartamot, amelynek az ügyfél rendelkezik hozzáféréssel egy adott eszközre. Vegye figyelembe, hogy az hello lista engedély (AccessPermissions.List) nem használható a egy OrDemandOrigin-kereső létrehozásával.

Keresők lejárati dátummal rendelkezik. hello Azure-portál beállítása 100 éves lejárati dátumot hello jövőbeli a lokátorokat.

> [!NOTE]
> 2015. márciusi korábban használt hello Azure portál toocreate lokátorokat, ha ezeket a lokátorokat beállított tooexpire két év után.
> 
> 

a lokátor, használja a lejárati dátum tooupdate [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) vagy [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) API-k. Vegye figyelembe, hogy egy SAS-lokátor lejárati dátuma hello frissítésekor módosítja hello URL-CÍMÉT.

Lokátorokat, amelyek nem tervezett toomanage felhasználói hozzáférés-vezérlés. Különböző hozzáférést biztosíthat jogosultsággal tooindividual rendelkező felhasználók a digitális tartalomvédelmi (DRM) megoldásokat. További információkért lásd: [biztonságossá tétele Media](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Ha egy kereső létrehozása az Azure Storage tárolási és propagálás folyamatok toorequired miatt 30 másodperces késés előfordulhat.

## <a name="adaptive-streaming"></a>Adaptív adatfolyam
Adaptív sávszélességű technológiák lehetővé teszik a videólejátszó alkalmazások toodetermine hálózati feltételek mellett, és jelölje ki több bitrates. Ha csökkenti a hálózati kommunikációt, hello ügyfél választja ki egy alacsonyabb sávszélességű így lejátszási alacsonyabb videominőséget folytatása. Javítása a hálózati feltételek mellett, mint a hello ügyfél tooa nagyobb átviteli sebesség a továbbfejlesztett videominőséget válthat. Az Azure Media Services a következő adaptív sávszélességű technológiák hello támogatja: HTTP Live Streaming (HLS), Smooth Streaming vagy MPEG-DASH.

tooprovide felhasználók adatfolyam-továbbítási URL-ekkel, akkor először hozzon létre egy OnDemandOrigin lokátort. Hello lokátor ad meg hello alap elérési útja toohello eszköz hello tartalmat tartalmazó létrehozása kívánt toostream. Azonban ez a tartalom, az elérési út további kell toomodify toobe képes toostream. a teljes URL-cím toohello adatfolyam-jegyzékfájlt, kell összefűzésére hello lokátor elérési tooconstruct érték és hello jegyzékfájl (filename.ism) neve. Majd fűzze **/Manifest** és egy (ha szükséges) megfelelő formátumban toohello lokátor elérési útját.

> [!NOTE]
> Is SSL-kapcsolaton keresztül adatfolyam formájában a tartalmat. toodo, ellenőrizze, hogy a HTTPS adatfolyam-továbbítási URL-címek kezdődik. Vegye figyelembe, hogy jelenleg AMS nem támogatja az SSL az egyéni tartomány.  
> 


Akkor is csak adatfolyam SSL-en keresztül Ha streamvégpontra, amelyről a tartalmat továbbít hello 2014. szeptember 10 után készült. Ha a streamelési URL-címek adatfolyam-végpontok után 2014. szeptember 10 hello alapuló hello URL-címet tartalmazza-e a "streaming.mediaservices.windows.net." Adatfolyam-továbbítási URL-címek, amelyek tartalmazzák a "origin.mediaservices.windows.net" (hello régi formátum) nem támogatja az SSL. Ha SSL-en keresztül szeretné toobe képes toostream hello régi formátumban kell megadni az URL-cím, hozzon létre egy új streamvégpont. Új streaming endpoint toostream a tartalom SSL-en keresztül hello alapján URL-címeket használnak.

## <a name="streaming-url-formats"></a>Adatfolyam-továbbítási URL-formátumokra
### <a name="mpeg-dash-format"></a>MPEG-DASH-formátum
{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=mpd-Time-CSF)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4 formátumban
{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3 formátumban
{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS) formátum csak szűrővel
Alapértelmezés szerint csak számok HLS manifest hello szerepelnek. Ez azért szükséges, az Apple Store tanúsításhoz cellás hálózatokhoz. Ebben az esetben ha egy ügyfél nem rendelkezik elegendő sávszélesség, vagy 2/g. kapcsolaton keresztül csatlakozik, a lejátszás kapcsolók csak tooaudio. Ez segít a pufferelés nélkül streaming tookeep tartalmat, de nincs videó van. Bizonyos esetekben pufferelés player lehet előnyben részesített csak keresztül. Ha azt szeretné, hogy tooremove hello csak nyomon követése, vegye fel **csak = false** toohello URL-CÍMÉT.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,audio-only=FALSE)

További információkért lásd: [dinamikus jegyzékfájl létrehozása támogatási és HLS kimeneti további funkciók](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Smooth Streaming formátumban
{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest

Példa:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Smooth Streaming 2.0 jegyzékfájl (örökölt jegyzékfájl)
Alapértelmezés szerint a jegyzékfájl formátuma Smooth Streaming hello ismétlési címke (r-kód) tartalmazza. Néhány lejátszó azonban nem támogatja a hello r-kód. Ezeket az ügyfelek használhatnak hello r-tag letiltása formátuma:

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Progresszív letöltés
Progresszív letöltés el lehet indítani játszott media, mielőtt hello teljes fájl be van töltve. Nem tudja fokozatosan .ism * (ismv, isma, ismt vagy ismc) fájlok letöltése.

tooprogressively tartalom letöltése, lokátor hello OnDemandOrigin típusú. hello alábbi példa bemutatja hello lokátor OnDemandOrigin típusú alapuló hello URL-címe:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Vissza kell fejtenie, amelyet a progresszív letöltés hello származási szolgáltatásból toostream tárolási titkosított eszközökre.

## <a name="download"></a>Letöltés
toodownload a tartalom tooa ügyféleszköz, létre kell hoznia egy SAS-kereső. hello SAS-lokátor által biztosított hozzáférés toohello Azure Storage tárolót, ha a fájl nem található. toobuild hello letöltési URL-CÍMRE, hogy tooembed hello fájlnév hello gazdagép és a SAS-aláírás közötti.

hello alábbi példa bemutatja az SAS-kereső hello alapuló hello URL-cím:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

a következő szempontok hello vonatkoznak:

* Vissza kell fejtenie, amelyet a progresszív letöltés hello származási szolgáltatásból toostream tárolási titkosított eszközökre.
* Egy letöltést, amely még nem fejeződött be 12 órában sikertelen lesz.

## <a name="streaming-endpoints"></a>Streamvégpontok

A streamvégpont egy adatfolyam-szolgáltatás által biztosított tartalom közvetlen tooa ügyfél player alkalmazás vagy tooa tartalomkézbesítési hálózat (CDN) a további terjesztési jelöli. hello kimenő adatfolyam adatfolyam-továbbítási végpont szolgáltatásból egy élő adatfolyam vagy a Media Services-fiók egy video-on-demand eszköz lehet. Adatfolyam-végpontok, két típusa van **szabványos** és **prémium**. További információkért lásd: [Streaming végpontok áttekintése](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

## <a name="known-issues"></a>Ismert problémák
### <a name="changes-toosmooth-streaming-manifest-version"></a>Módosítások tooSmooth Streaming fürtjegyzék verziója
2016. július szolgáltatás kiadásban--Media Encoder Standard, Media Encoder prémium munkafolyamat vagy hello korábbi Azure Media Encoder továbbítva lettek dinamikus becsomagolás--Smooth Streaming hello segítségével által előállított eszközök manifest hello visszaadott előtt volna felel meg tooversion 2.0. A 2.0-s verziójának hello töredék időtartamok nem hello ("r") az úgynevezett ismételt címkék használata. Példa:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Hello 2016. július szolgáltatás kiadása, a létrehozott hello Smooth Streaming jegyzékfájl megfelel-e tooversion 2.2, töredék időtartamok ismétlődő címkék használatával. Példa:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Néhány hello Smooth Streaming tanúsítványbeléptetési hello ismétlődő címkék nem támogatja, és tooload hello jegyzékfájl sikertelen lesz. toomitigate a probléma hello örökölt jegyzékfájl formátuma paraméter használható **(formátum = fmp4-v20)** vagy a toohello legújabb ügyfélverzió, amely támogatja az ismétlődő címkék frissítése. További információkért lásd: [Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Kapcsolódó témakörök
[A Media Services keresők frissítése után működés közbeni tárolási kulcsok](media-services-roll-storage-access-keys.md)

