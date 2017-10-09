---
title: "aaaFilters és dinamikus jegyzékfájlokban |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toocreate szűrők, az ügyfél használhassa őket toostream konkrét szakaszokra az adatfolyam. A Media Services hoz létre dinamikus jegyzékfájlokban tooarchive a szelektív streaming."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: ff102765-8cee-4c08-a6da-b603db9e2054
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 9527a011438c11da07a363d701ea736414412ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filters-and-dynamic-manifests"></a>Szűrők és dinamikus jegyzékfájlokban
2.11 kiadástól kezdve a Media Services lehetővé teszi az eszközök toodefine szűrők. Ezek a szűrők, amelyek lehetővé teszik az ügyfelek toochoose toodo többek között a kiszolgáló oldalán szabályok: lejátszás videó (lejátszása helyett hello teljes videó), csak egy részét, vagy adjon meg, hogy a felhasználói eszköz képes kezelni (hang- és interpretációk csak egy részét minden hello interpretációk helyett társított hello eszköz). Ez a szűrés a eszközök archivált keresztül **dinamikus Manifest**a megadott szűrő alapján létrehozott, a felhasználói kérelem toostream videó s.

A témakörök ismerteti a gyakori forgatókönyvek, amelyben szűrők segítségével nagyon hasznos tooyour ügyfelek és a hivatkozások tootopics, amelyek bemutatják, hogyan toocreate programozott módon szűrők lenne (jelenleg hozhatja létre szűrők REST API-kat csak).

## <a name="overview"></a>Áttekintés
Ha a tartalom toocustomers (élő esemény streamelését vagy video-on-demand) kézbesíti a cél van egy kiváló minőségű videó toovarious eszközök különböző hálózati körülmények toodeliver. a cél hello következő tooachieve:

* az adatfolyam toomulti sávszélességű kódolása ([adaptív sávszélességű](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) video-adatfolyamot (ez kezeli minőségi és hálózati körülményekhez) és 
* használja a Media Services [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md) toodynamically őket csomagolni az adatfolyam különböző protokollok (ez kezeli a különböző eszközökön streaming). A Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming vagy MPEG DASH. 

### <a name="manifest-files"></a>Fájlok
Ha Ön kódolása egy adaptív sávszélességű streamelés esetén az eszköz egy **manifest** (lista) fájl jön létre (hello fájl szöveges vagy XML-alapú). Hello **manifest** fájl tartalmazza, például a streaming metaadatok: típusa (hang, videó vagy), akkor a nyomon követése nevét, kezdő és záró idő, sávszélességű (Tulajdonságok), nyomon követése nyelveket, bemutató ablakban (csúszóablak rögzített időtartama), videó a kodek (FourCC). Hello következő lejátszható videó töredék elérhető és helyük információ megadásával hello player tooretrieve hello következő töredék is utasítja. Töredék (vagy szegmensek) hello tényleges "adattömbök" videó tartalom.

Íme egy példa a jegyzékfájl: 

    <?xml version="1.0" encoding="UTF-8"?>    
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">

    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />

    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>


    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />

    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>

    </SmoothStreamingMedia>

### <a name="dynamic-manifests"></a>Dinamikus jegyzékfájlokban
Nincsenek [forgatókönyvek](media-services-dynamic-manifest-overview.md#scenarios) mikor az ügyfél kell rugalmasabb sablontelepítést hello alapértelmezett eszköz jegyzékfájl leírtakhoz. Példa:

* Adott eszköz: csak hello megadott interpretációk és/vagy rendszerkötet megadott biztosításához használt tooplayback hello eszköz által támogatott nyelvi számok hello tartalom ("verzióinak szűrése"). 
* Csökkentse a hello jegyzék tooshow alárendelt klip egy élő esemény ("altípusa klip szűrése").
* A vágás hello ("díszítésre videó") videó elindítása.
* Rendelés tooprovide hello DVR ablak hello Player ("beállító bemutató ablak") korlátozott hosszát bemutató ablakban (DVR) módosítása

tooachieve a rugalmasságot, a Media Services ajánlatok **dinamikus jelentkezik** alapú az előre meghatározott [szűrők](media-services-dynamic-manifest-overview.md#filters).  Miután hello szűrőket határozhat meg, az ügyfelek volt a azokat egy adott verzióinak toostream vagy alárendelt videóklipeket a videó. A streamelési URL-cím hello közül néhányat kizárjanak volna adnia. Szűrők lehet alkalmazott tooadaptive sávszélességű adatfolyam-által támogatott protokollok [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md): HLS, MPEG-DASH vagy Smooth Streaming. Példa:

A szűrővel MPEG DASH URL-címe

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Smooth Streaming URL-cím elé szűrő

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


További információ toodeliver a tartalom és -buildek streamelési URL-címek, lásd: [tartalom áttekintése kézbesítéséhez](media-services-deliver-content-overview.md).

> [!NOTE]
> Vegye figyelembe, hogy dinamikus jelentkezik, nem módosítható hello eszköz hello alapértelmezett jegyzéket az adott eszköz számára. Az ügyfél kiválaszthatja toorequest vagy anélkül szűrők adatfolyam. 
> 
> 

### <a id="filters"></a>Szűrők
Az eszköz szűrők két típusa van: 

* Globális szűrők (kell alkalmazott tooany eszköz hello Azure Media Services-fiók, élettartama pedig hello fiók) és 
* Helyi szűrők (csak akkor alkalmazott tooan eszköz, mely hello szűrő lett rendelve a létrehozása után, hello eszköz élettartamáról). 

A globális és helyi szűrő típusoknak pontosan lehet hello ugyanazok a tulajdonságok. hello a fő különbség a két hello között, mely forgatókönyvek egy fájlkiszolgáló milyen típusú megfelelőbbek. Globális szűrők alkalmasak általában (verzióinak szűrés) eszközprofilok ahol helyi szűrők használt tootrim egy adott eszköz lehet.

## <a id="scenarios"></a>Gyakori helyzetek
Ahogyan volt előtt, ha a tartalom toocustomers (élő esemény streamelését vagy video-on-demand) kézbesíti a cél egy jó minőségű videó toodeliver toovarious eszközök különböző hálózati feltételek. Ezenkívül előfordulhat, hogy a szűrés az objektumok és a használatával további követelményekkel rendelkezik **dinamikus Manifest**s. a következő szakaszok hello biztosítják a különböző szűrési forgatókönyvek rövid áttekintést.

* Adja meg az egyes eszközök (az összes hello interpretációk társított hello eszköz) nem kezelhető hang- és interpretációk csak egy részét. 
* Lejátszás videó (helyett hello teljes videó lejátszása) részt.
* Állítsa be úgy a DVR megjelenítési ablakot.

## <a name="rendition-filtering"></a>Verzióinak szűrése
Az eszköz toomultiple kódolási profilok (H.264 alapterv, H.264 magas, AACL, AACH, Dolby digitális Plus) és több minőségi bitrates úgy is dönthet tooencode. Azonban nem minden ügyfél eszközök támogatják az eszköz profilok és bitrates. Például a régebbi Android-eszközök csak H.264 alapterv + AACL támogatja. Sávszélesség- és eszköz számítási küld nagyobb bitrates tooa eszköz, amely nem olvasható be a hello előnyeit, hulladékok. Ilyen eszköz kell dekódolása összes hello megadott adatokat, csak tooscale azt le megjelenítése.

A dinamikus Manifest eszközprofilok hozhat létre mobile, például konzol HD/SD stb., és hello követi nyomon és tulajdonságait, amelybe toobe az egyes profilok egy részét.

![Szűrési verzióinak – példa][renditions2]

A következő példa hello egy kódoló használt tooencode egy hét ISO MP4 videó interpretációk (a 180p too1080p) mezzanine eszköz volt. hello kódolt objektumhoz is dinamikusan csomagolja a rendszer a következő adatfolyam-továbbítási protokollok hello bármelyike: MPEG DASH, HLS és zökkenőmentes.  Hello diagram hello tetején jelenik meg a szűrők hello eszköz manifest HLS hello (tartalmaz minden hét interpretációk).  A bal alsó hello HLS manifest "ott" nevű szűrő lett alkalmazva toowhich hello jelenik meg. hello "ott" szűrő tooremove 1 MB/s, amely hello alsó két szolgáltatásminőségi szinteket alatt levágja, válaszként hello alatti összes bitrates határozza meg.  A hello jobb alsó sarok, hello HLS nevű, "mobilalkalmazás" szűrőt alkalmazott jegyzék toowhich jelenik meg. hello "mobileszköz" szűrő határozza meg, ahol hello. megoldás lehet nagyobb, mint 720p, amely hello levágja, hogy két 1080p interpretációk tooremove interpretációk.

![Verzióinak szűrése][renditions1]

## <a name="removing-language-tracks"></a>Nyelv eltávolítása számok
Az eszközök közé tartozik a több hang nyelv, például az angol, spanyol, francia, stb. Általában hello Player SDK kezelők alapértelmezett hang statisztikák nyomon követésének kiválasztása, és elérhető hang nyomon követi az egyes felhasználó kiválasztása. Nehéz toodevelop ilyen Player SDK-k, különböző megvalósítások eszközspecifikus player-keretrendszerek között van szükség. Is az egyes platformokon Player API-k korlátozva, és nem tartalmazzák a hang kijelölés szolgáltatás arról, hogy a felhasználók hol nem válasszon vagy módosítsa a hello alapértelmezett hang nyomon követése. Eszköz szűrőkkel, amelyek csak tartalmazzák a kívánt hang nyelvek szűrők létrehozásával szabályozhatja a hello működését.

![Szűrés nyelvi számok][language_filter]

## <a name="trimming-start-of-an-asset"></a>Egy eszköz tisztítás kezdő
A legtöbb események élő adatfolyamainak továbbítása operátorok néhány teszt hello tényleges esemény előtt futtassa. Előfordulhat, hogy például tartalmaznak egy lappal ilyen hello esemény hello elindítása előtt: "Program akkor kezdődik, rövid ideig gombra. Hello program archiválás van, ha hello teszt- és lappal adatokat is archivált, és hello bemutató fog szerepelni. Azonban ez az információ nem megjelenítendő toohello ügyfelek. A dinamikus Manifest létrehozhat egy szűrőt, kezdési időt, és hello jegyzékfájl hello nemkívánatos adatok eltávolítása.

![Tisztítás kezdő][trim_filter]

## <a name="creating-sub-clips-views-from-a-live-archive"></a>Élő archívumból alárendelt videóklipeket (nézetek) létrehozása
Számos élő esemény hosszú ideig futniuk és élő archív állhatnak több esemény. Hello élő esemény vége műsorszolgáltatók azt szeretné, hogy toobreak hello mentése után élő archívum be logikai programot, és állítsa le a feladatütemezések. Ezt követően a virtuális programok külön-külön nem közzé post hello élő archív feldolgozásához, és nem hozza létre külön az eszközöket, (amelyek nem kap a hello tartalomtovábbító előnye, hogy a meglévő gyorsítótárazott töredék hello). Ilyen virtuális programok (alárendelt videóklipeket) egy bemutatjuk vagy Kosárlabda játék, a baseball hello innings hello negyedéven belülre esik, vagy egy délután olimpia program az egyes események.

A dinamikus Manifest szűrőkkel kezdő és záró időpont használatával, és virtuális nézeteket hozhat létre az élő archívum hello felső keresztül. 

![Szűrő subclip][subclip_filter]

Szűrt eszköz:

![Terveztek][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Megjelenítési ablakot (DVR) beállítása
Azure Media Services jelenleg, ahol konfigurálható hello időtartama, 5 perc között - 25 óra körkörös archív kínál. Manifest szűrés alkalmas lehetnek használt toocreate működés közbeni DVR ablak hello felső hello archívum media törlése nélkül. Nincs több forgatókönyv áll rendelkezésre műsorszolgáltatók, ahová tooprovide egy korlátozott DVR ablakot, amivel a hello a peremhálózati live, és: hello ugyanannyi időt vesz igénybe tartsa egy nagyobb archiválási ablakot. A szórást küldő számítógép lehet, hogy szeretné, hogy toouse hello adatok hello DVR ablak toohighlight videóklipeket kívül esik, vagy he\she érdemes tooprovide különböző DVR windows különböző eszközökhöz. Például hello mobil eszközeinek (rendelkezhet egy 2 perces DVR ablakot a mobileszközökhöz és 1 óra asztali ügyfelek) nagy DVR windows kezelik.

![DVR ablak][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff (élő pozíciótól) beállítása
Manifest szűrés lehet használt tooremove szélétől hello élő élő program néhány másodpercig. Ez lehetővé teszi a műsorszolgáltatók toowatch hello bemutató hello preview kiadványon mutasson, és a hirdetmény beszúrási pontok létrehozása előtt hello megjelenítők kap hello adatfolyamot (általában a biztonsági indító által 30 másodperc). Műsorszolgáltatók majd tolható ezen hirdetmények tootheir ügyfél keretrendszerek számukra időben tooreceived és a folyamat hello adatokat előtt hello hirdetmény lehetőséget.

A fentiek mellett toohello hirdetmény támogatják, LiveBackoff használható ügyfél élő letöltési pozíció beállítja, hogy ha az ügyfelek eltolódás és hello élő peremhálózati találati továbbra is kaphatnak töredék kiszolgálóról helyett 412 vagy 404-es HTTP-hibák.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Kombinálásával egyetlen szűrőben több szabály
Kombinálásával egyetlen szűrőben több szűrési szabályok. Például adja meg a tartomány szabály tooremove lappal élő archívumból, és elérhető bitrates szűrni is. Több szűrési szabályok hello end eredménye hello összeállítás (csak metszetének) szabályt.

![több-szabályok][multiple-rules]

## <a name="create-filters-programmatically"></a>Hozzon létre szoftveres szűrők
hello a következő témakör ismerteti, amelyek kapcsolódó toofilters Media Services entitásokat. hello a témakör azt is bemutatja, hogyan hozza létre a tooprogrammatically a szűrőket.  

[Hozzon létre szűrők REST API-k](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombinált szűrők (szűrő összeállítás)
Az egy URL-cím több szűrő kombinálhatja is. 

hello következő forgatókönyv bemutatja, hogy miért érdemes toocombine szűrők:

1. Szüksége toofilter a videó minőségű mobil eszközök, például Android vagy iPAD (a rendelés toolimit videó jellemzők). tooremove hello nemkívánatos tulajdonságait, amely eszközprofilok megfelelő globális szűrőként hozna létre. Fent említett globális szűrők segítségével hello alatt az összes eszköz azonos media services-fiók nélkül semmilyen további társítást. 
2. Is szeretné, hogy tootrim hello kezdő és befejező időpontja eszköz. tooachieve, létrehozhat egy helyi szűrőt, és hello kezdő/záró idő beállítása. 
3. Azt szeretné, hogy toocombine mindkét, ezek a szűrők (nélkül kombinációja kellene tooadd minőségi szűrési toohello levágási szűrő, amely fog megnehezítik szűrő használata).

toocombine szűrők kell tooset hello szűrő nevek toohello jegyzékfájl/lista URL-cím elé pontosvesszővel tagolva. Tegyük fel nevű szűrést *MyMobileDevice* minőségű szűrők, és van egy másik nevű *MyStartTime* tooset egy adott kezdési időpontja. Ehhez hasonló egyesítheti őket:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Too3 szűrők mentése kombinálhatja. 

További információ: [ez](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a name="know-issues-and-limitations"></a>Tudja, problémák és korlátozások
* Dinamikus jegyzékfájl működik GOP határok (kulcs keretek), ezért díszítésre rendelkezik GOP pontosságát. 
* Használhatja a helyi és globális szűrők azonos szűrő nevét. Vegye figyelembe, hogy helyi szűrő magasabb prioritással rendelkezik, és felülírja a globális szűrők.
* Ha frissíti a szűrőt, streaming endpoint toorefresh hello szabályok too2 percig is eltarthat. Ha hello tartalom állítása és kiszolgálása között egyes szűrők használatával (és proxyk és a CDN a gyorsítótárba helyezett gyorsítótárak), ezek a szűrők frissítése okozhat player sikertelen. Javasoljuk, tooclear hello gyorsítótár hello szűrő frissítése után. Ha ezt a beállítást nem lehetséges, érdemes lehet egy másik szűrőt.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Olyan tartalom tooCustomers áttekintése](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
