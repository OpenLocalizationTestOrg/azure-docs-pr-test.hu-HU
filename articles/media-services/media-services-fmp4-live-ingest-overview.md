---
title: "aaaAzure Media Services töredezett MP4 élő betöltési specification |} Microsoft Docs"
description: "Ez az előírás hello protokoll és formátumának megadása töredezett MP4-alapú élő adatfolyam-továbbítási adatfeldolgozást Azure Media Services ismerteti. Azure Media Services toostream élő eseményeket használ, és valós időben tartalom szórási hello felhő platform Azure segítségével. Ez a dokumentum is ismerteti, ajánlott eljárások a magas redundáns és robusztus működés közbeni felépítése betöltési mechanizmusokat."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 0c191f8d6c5a595621feaba0e571fb984b666f34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Az Azure Media Services töredezett MP4 élő betöltési meghatározása
Ez az előírás hello protokoll és formátumának megadása töredezett MP4-alapú élő adatfolyam-továbbítási adatfeldolgozást Azure Media Services ismerteti. A Media Services egy élő adatfolyam-szolgáltatásra, hogy az ügyfelek használja toostream élő eseményeket és valós idejű tartalom szórási hello felhő platform Azure segítségével biztosít. Ez a dokumentum is ismerteti, ajánlott eljárások a magas redundáns és robusztus működés közbeni felépítése betöltési mechanizmusokat.

## <a name="1-conformance-notation"></a>1. Megfelelési jelöléssel
hello kulcsszavakat "kell," "nem""Kötelező," "SHALL," "kell nem lehet" "SHOULD," "Kell nem," "Ajánlott", "Lehet, hogy," és "OPTIONAL" Ebben a dokumentumban is értelmezésre, hogy-e az RFC 2119 toobe.

## <a name="2-service-diagram"></a>2. Szolgáltatás ábrája
hello alábbi ábrán látható hello magas szintű architektúrájának hello élő adatfolyam-szolgáltatás a Media Services:

1. Egy élő kódoló kimenő élő hírcsatornák toochannels létrehozott és üzembe hello Azure Media Services SDK használatával.
2. Csatornák, programok és adatfolyam-továbbítási végpontok összes hello élő adatfolyam-funkciók, beleértve a feldolgozást, a formázást, a Media Services leíró felhő DVR-t, a biztonsági, a méretezhetőség és a redundancia érdekében.
3. Szükség esetén az ügyfelek választható toodeploy az Azure Content Delivery Network layer hello streaming endpoint és hello ügyfél-végpontok közötti.
4. Ügyfél végpontok adatfolyam a hello adatfolyam-továbbítási végpontra adaptív Streameléshez HTTP protokoll használatával. Például a Microsoft Smooth Streaming, dinamikus adaptív adatfolyam-(kötőjel vagy MPEG-DASH) HTTP és a Apple HTTP Live Streaming (HLS) keresztül.

![a betöltési folyamat][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Bitstream formátumban – ISO 14496-12 töredékes MP4
hello egybeírt az élő adatfolyam-betöltési ismertet a dokumentum [ISO-14496-12] alapul. A részletes ismertetése a töredezett MP4 formátumban és bővítmények egyaránt videotartalom fájl-és élő adatfolyam-továbbítási adatfeldolgozást [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Működés közbeni betöltési formátum definíciók
hello alábbi lista részletesen ismerteti az Azure Media Services betöltési toolive vonatkozó definíciókat különleges formátumú:

1. Hello **ftyp**, **kiszolgáló Manifest mezőbe élő**, és **moov** mezőkbe (HTTP POST) minden egyes kérelemmel el kell küldeni. Ezek a mezők kell küldeni a hello adatfolyam hello elején, és bármikor hello kódoló csatlakoznia kell a tooresume adatfolyam betöltési. További információkért lásd a 6 [1].
2. [1] 3.3.2 című meghatározása nevű egy nem kötelező mező **StreamManifestBox** a működés közbeni betöltési. Lejáró toohello útválasztási logika hello Azure terheléselosztó ebben a mezőben használata elavult. hello mező nem lehet jelenik meg, ha a Media Services választásával dolgozhat fel. Ha ezt a jelölőnégyzetet, a rendszer a Media Services csendes figyelmen kívül hagyja azt.
3. Hello **TrackFragmentExtendedHeaderBox** [1] 3.2.3.2 definiált mezőben minden töredék jelen kell lennie.
4. 2-es verziójának hello **TrackFragmentExtendedHeaderBox** mezőben használt toogenerate media szegmenseket, amelyeknek azonos URL-címek több adatközpontot kell lennie. hello töredék index mezője REQUIRED formátumban index alapján például az Apple HLS cross-datacenter feladatátvételt és az index alapján MPEG-DASH. tooenable cross-datacenter feladatátvétel, hello töredék index több kódolók keresztül kell szinkronizálva, és minden egymást követő adathordozók töredék 1 akár kódoló újraindítások vagy hibák keresztül növelhető.
5. [1] 3.3.6 című határozza meg egy nevű listában **MovieFragmentRandomAccessBox** (**mfra**), amely küldhet élő adatfeldolgozást tooindicate vége az adatfolyam (EOS) toohello csatorna hello végén. Esedékes toohello feldolgozó logika a Media Services EOS használata elavult, és hello **mfra** mezőben, az élő adatfeldolgozást nem lesz elküldve. Ha kap, a Media Services csendes figyelmen kívül hagyja azt. hello tooreset hello állapotának betöltési pont, azt javasoljuk, hogy használjon [csatorna alaphelyzetbe](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). Azt javasoljuk, hogy használjon [Program leállításához](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) tooend bemutatása és az adatfolyam.
6. hello MP4-töredéket időtartama legyen állandó, tooreduce hello mérete hello ügyfél jegyzékfájljai. Egy állandó MP4-töredéket időtartam is javítja az ügyfél letöltési heurisztikus ismétlődő címkék hello használata. hello időtartama előfordulhat, hogy a nem egész keret díjszabás toocompensate hajlamos.
7. hello MP4-töredéket időtartama körülbelül 2 – 6 másodperc között kell lennie.
8. MP4 darabolható időbélyegeket és indexek (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` és `fragment_index`) növekvő sorrendben kell érkeznek. Bár a Media Services rugalmas tooduplicate töredék, korlátozott képességét tooreorder töredék toohello media ütemterv szerint.

## <a name="4-protocol-format--http"></a>4. Protokoll-formátum – a HTTP
ISO töredékes MP4-alapú működés közbeni betöltési, a Media Services egy szabványos hosszan futó HTTP POST kérelem kódolású tootransmit media adatokat használ, amely toohello szolgáltatás töredezett MP4 formátumban van csomagolva. Minden egyes HTTP POST küld, teljes töredékes MP4 bitstream ("stream"), fejléc mezőkbe hello kezdődő kezdve (**ftyp**, **kiszolgáló Manifest mezőbe élő**, és **moov** mező esetén), és töredék sorozatát folytatása (**moof** és **mdat** be). URL-szintaxisa hello HTTP POST-kérelmet a(z) című rész 9.2 in [1]. Hello POST URL-címe például a következő: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Követelmények
Az alábbiakban a hello részletes követelményeket:

1. hello kódoló webalkalmazásokba úgy, hogy egy üres HTTP POST kérést küld szórási hello "törzs" (nulla tartalom hossza) használatával hello adatfeldolgozást URL-CÍMÉRE. Ez segítheti a hello élő adatfeldolgozást végpont érvényes-e, és semmilyen hitelesítési vagy más feltételek, szükség esetén gyorsan észlelése hello kódoló. HTTP protokoll / hello kiszolgáló nem küldhető vissza egy HTTP-válasz fogadásáig hello teljes kérelemfeldolgozási, beleértve a FELADÁS egy vagy több szervezet hello. Hello hosszan futó jellegéből élő esemény, ebben a lépésben hello kódolás nélkül nem feltétlenül tudja toodetect hiba alatt végig az összes hello adatküldés.
2. (1) miatt az hello kódoló kezelni kell az esetleges hibákat vagy hitelesítési kihívást. Ha (1) sikeres 200 választ, továbbra is.
3. hello kódoló egy új HTTP POST-kérelmet kell kezdődnie hello töredékes MP4 adatfolyam. hello hasznos hello fejléc jelölőnégyzetéből, töredék követnie kell kezdődnie. Vegye figyelembe, hogy hello **ftyp**, **kiszolgáló Manifest mezőbe élő**, és **moov** mezőiben (ebben a sorrendben) kell küldött minden egyes kérelemmel, még akkor is, ha hello kódoló kell csatlakozzon újra, mert hello előző kérelem meg lett szakítva előzetes toohello hello adatfolyam végét. 
4. hello kódoló kell használni a darabolásos átviteli kódolás feltöltése, mert lehetetlen toopredict hello teljes tartalom hossza hello élő esemény.
5. Ha hello esemény keresztül, hello utolsó töredék, elküldése után hello kódoló szabályosan hello darabolásos átviteli kódolás a üzenetsort (HTTP-ügyfél a legtöbb verem kezelnie automatikusan) kell végződnie. hello kódoló kell hello szolgáltatás tooreturn hello végső válaszkód Várjon, majd állítsa le a hello kapcsolatot. 
6. NEM kell hello kódoló hello használata `Events()` főnév élő szempontjából a Media Services 9.2 in [1] leírtak szerint.
7. Ha hello HTTP POST-kérelmet leállítása vagy időpontokban egy hello adatfolyam TCP hiba előzetes toohello végén hello kódoló kell adjon ki egy új POST kérelmet egy új kapcsolat használatával, és hajtsa végre a fenti követelmények hello. Emellett hello kódoló kell hello újraküldi a hello adatfolyam minden nyomon követése az előző két MP4 töredék, majd folytassa hello media ütemterv kihagyást, anélkül. Újraküldés hello minden nyomon követése az utolsó két MP4-töredék biztosítja, hogy van-e az adatvesztés. Ez azt jelenti adatfolyamot tartalmaz egy hang- és a videó nyomon, és hello aktuális POST kérelem sikertelen lesz, ha hello kódoló kell újra és Újraküldte hello hello hang nyomon követése, az utolsó két töredék, amelyeket korábban már sikeresen, és utolsó két hello töredékei hello videó nyomon követése, amelyeket korábban már sikeresen, tooensure, hogy adatvesztés nélküli. hello kódoló kezelnie kell a "továbbítás" puffer media töredékek, amely azt az újracsatlakozáskor újraküldése.

## <a name="5-timescale"></a>5. időskálára
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) teljesítési hello használatát ismerteti **SmoothStreamingMedia** (2.2.2.1. szakaszát), **StreamElement** (2.2.2.3 szakaszát), **StreamFragmentElement**(2.2.2.6 szakaszban), és **LiveSMIL** (2.2.7.3.1 szakaszt). Ha hello időskálára érték nincs jelen, hello használt alapérték 10,000,000 (10 MHz). Hello Smooth Streaming fájlformátum-specifikációnak nem tiltja a más időskálára értékek használatát, bár a legtöbb kódoló megvalósítások használja-e ez az alapértelmezett érték (10 MHz) toogenerate Smooth Streaming betöltik az adatokat. Esedékes toohello [Azure Media dinamikus becsomagolás](media-services-dynamic-packaging-overview.md) funkciót, azt javasoljuk, hogy használjon egy 90-KHz időskálára a video-adatfolyamot és 44,1 KHz vagy 48.1 KHz hang adatfolyamokat. Ha másik időskálára értékeket fogja használni a különböző adatfolyamokba, hello adatfolyam-szintű időskálára kell elküldeni. További információkért lásd: [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. "Stream" meghatározása
A adatfolyama hello alapvető egysége élő bemutatók létrehozására élő adatfeldolgozást művelete adatfolyam feladatátvételi és a redundancia forgatókönyvek kezelésével. Az adatfolyam egy egyedi, töredezett MP4 bitstream tartalmazhat egyetlen nyomon vagy több számok típusúként van definiálva. Teljes bemutató tartalmazhat egy vagy több adatfolyamot, az élő kódolók hello hello konfigurációjától függően. a következő példák hello adatfolyamok toocompose használatának különböző lehetőségek teljes bemutató mutatják be.

**Példa** 

Egy ügyfél egy élő adatfolyam-továbbítási bemutató, amely tartalmazza a következő hang-és videófolyamot bitrates hello toocreate szeretne:

Videó – 3000 kbit/s, 1500 kbit/s, 750 KB/s

Hang – 128 kb/s

### <a name="option-1-all-tracks-in-one-stream"></a>1. lehetőség: Az összes nyomon követi a egy adatfolyam
Ezt a beállítást, az egyetlen kódoló hoz létre minden hang-és videófolyamot nyomon követi, és majd azokat egy töredezett MP4 bitstream bundles. hello töredékes MP4 bitstream küldi el a rendszer egyetlen HTTP POST-kapcsolaton keresztül. Ebben a példában a élő bemutató csak egy adatfolyam van.

![Adatfolyam-egy nyomon követése][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>2. lehetőség: Minden követése külön Stream
Ezt a beállítást, a hello kódoló egy követése elhelyezi minden töredék MP4 bitstream, és majd küldi hello adatfolyamok mindegyikét külön HTTP-kapcsolaton keresztül. Ez egy Encoder és több kódolókkal is végrehajthatja. hello élő adatfeldolgozást az élő adás látja, négy adatfolyamokat, ahogy.

![Az elkülönített adatfolyamok nyomon követi][image3]

### <a name="option-3-bundle-audio-track-with-hello-lowest-bitrate-video-track-into-one-stream"></a>3. lehetőség: Köteg hang követése hello legalacsonyabb sávszélességű videó követése egy adatfolyamba való mentésre
Ezt a beállítást hello ügyfél úgy dönt, toobundle hello hang hello legalacsonyabb sávszélességű videó nyomon követése a egy töredék MP4 bitstream nyomon követni, és hagyja hello önálló adatfolyamokként más két videó nyomait. 

![Adatfolyam-hang- és videofunkcióinak nyomon követi][image4]

### <a name="summary"></a>Összefoglalás
Ez a nem egy lista tartalmazza az ebben a példában az összes lehetséges adatfeldolgozást lehetőség. Bármely csoportosítási adatfolyamok való felvételek, valójában, élő adatfeldolgozást támogatja. Az ügyfelek és a szállítók kódoló választhatja a saját megvalósítások mérnöki összetettségét, a kódoló kapacitás, és a redundancia és a feladatátvételi szempontokat részletező cikkben alapján. A legtöbb esetben van azonban csak egy hang követése hello teljes élő bemutatáshoz. Igen az fontos tooensure hello egészséges legyen a hello betöltési adatfolyam, amely tartalmazza a hello hang nyomon követése. Gyakran ez a megfontolás hello hang nyomon követése és a saját adatfolyamban (ahogy a 2. lehetőség) vagy a hello legalacsonyabb sávszélességű videó nyomon követése (ahogy a 3. lehetőség) kötegelése azt eredményezi. Emellett jobb redundancia és a hibatűrést, küldése hello azonos hang a két különböző adatfolyamokba (redundáns zeneszámok beállítás 2), vagy a kötegelése hello hang nyomon követése és legalább két hello legalacsonyabb sávszélességű videó követi nyomon (a 3 beállítás a következő kötegelve hang legalább két video-adatfolyamot) erősen ajánlott a működés közbeni a Media Services betöltési.

## <a name="7-service-failover"></a>7. Szolgáltatás feladatátvétele
Élő Stream továbbítása hello jellege, a megadott jó a feladatátvételre fontos hello hello szolgáltatás rendelkezésre állásának biztosításához. A Media Services tervezett toohandle különböző típusú hibák, beleértve a hálózati hibák, a kiszolgáló hibákat és a tárolási problémák van. Megfelelő feladatátvételi logika hello élő kódoló oldaláról együtt használják, amikor a felhasználók hello felhőből nagymértékben megbízható élő adatfolyam-továbbítási szolgáltatás érhető el.

Ebben a szakaszban arról lesz szó szolgáltatás feladatátvételi forgatókönyvek. Ebben az esetben hello hibát hello szolgáltatás valahol történik, és akkor jelentkezik hálózati hiba. Az alábbiakban néhány javaslatot hello kódoló végrehajtási szolgáltatás feladatátvétele kezelése:

1. Használjon egy 10 másodperces időtúllépési hello TCP-kapcsolat létrehozása. Ha egy kísérlet tooestablish hello kapcsolat 10 másodpercnél hosszabb időt vesz igénybe, hello művelet megszakításához, és próbálkozzon újra. 
2. Használjon egy rövid időtúllépési hello HTTP-kérelem üzenet adattömbök küldése. Ha hello cél MP4 töredék időtartama N másodperces, akkor egy küldési határidő közötti N és 2 N másodperces; Ha hello MP4 töredék időtartam 6 másodperc, például egy 6 too12 másodperces időtúllépési használja. Időtúllépés történik, ha alaphelyzetbe állítása hello kapcsolat, nyissa meg az új kapcsolat és folytatása adatfolyam betöltési hello új kapcsolat. 
3. A működés közbeni puffer, amelynek hello utolsó két részlete minden követése sikeresen és teljesen elküldött toohello szolgáltatás karbantartása.  Hello HTTP POST-kérelmet adatfolyam megszakadt, vagy időtúllépés előzetes toohello hello adatfolyam végét, ha egy új kapcsolat megnyitásához és egy másik HTTP POST-kérelmet megkezdéséhez, hello adatfolyam fejlécek újraküldi, újraküldi hello utolsó két részlete minden nyomon követése és hello adatfolyam nélkül folytatása hello media ütemterv kihagyást bemutatása. Ez csökkenti a hello adatvesztés esélyét.
4. Azt javasoljuk, hogy hello kódoló nem újrapróbálkozások tooestablish kapcsolatot hello számának korlátozása vagy folytatása a streaming után TCP-hiba akkor fordul elő.
5. Miután a TCP-hiba:
  
    a. aktuális kapcsolati hello be kell zárni, és egy új kapcsolatot kell létrehozni egy új HTTP POST-kérelmet.

    b. új HTTP POST URL-címe nem lehet hello hello megegyeznek a hello kezdeti POST URL-CÍMÉT.
  
    c. hello új HTTP POST tartalmaznia kell az adatfolyam fejlécek (**ftyp**, **kiszolgáló Manifest mezőbe élő**, és **moov** be), amelyek azonos toohello adatfolyam fejlécének hello utáni kezdeti.
  
    d. hello utolsó két töredék minden követése küldött kell küldeni, és adatfolyam-kell folytatása hello media ütemterv kihagyást, anélkül. hello MP4-töredéket időbélyegeket folyamatosan, növelnie kell akár HTTP POST-kérésnél keresztül.
6. hello kódoló kell leáll hello HTTP POST-kérelmet, ha az adatok nem küldi hello MP4 töredék időtartama arányos sebességgel.  Egy HTTP POST-kérelmet, amely nem küld adatokat megakadályozhatja, hogy a Media Services gyorsan hello szolgáltatás frissítése esetén hello kódoló leválasztása. Emiatt a HTTP POST hello ritka (ad jel) nyomon követi lehet rövid életű, amint hello ritka töredéket küldött leáll.

## <a name="8-encoder-failover"></a>8. kódoló feladatátvétel
Kódoló feladatátvételi típus hello második feladatátvételi forgatókönyv, amelyet a címzett toobe a végpontok közötti élő adatfolyamként történő továbbítását. Ebben a forgatókönyvben hello hiba állapot akkor fordul elő, a hello kódoló oldalon. 

![kódoló feladatátvétel][image5]

a következő verziójával kapcsolatos elvárások hello érvényesek hello élő adatfeldolgozást végpont a kódoló feladatátvétel történik:

1. Új kódoló példányt kell létrehozni toocontinue streaming, hello diagramja (adatfolyam a video-szaggatott vonallal 3000-k) ismertetett módon.
2. hello új kódoló kell használata hello azonos URL-Címének HTTP POST kérelmek hello, a példány nem sikerült.
3. hello új kódoló POST kérelem tartalmaznia kell hello azonos töredezett MP4 fejléc jelölőnégyzetéből, a sikertelen előfordulás hello.
4. hello új kódoló kell megfelelően szinkronizálva minden más futó kódolókkal a hello azonos élő bemutató toogenerate szinkronizált hang-és videófolyamot minták igazított töredék határokat.
5. lehet, hogy hello új adatfolyam szemantikailag megfelelője az előző adatfolyam hello, és cserélhető hello fejléc és töredék szinten.
6. hello új kódoló kell próbálnia toominimize adatvesztés. Hello `fragment_absolute_time` és `fragment_index` adathordozó töredék növelje, ahol hello kódoló utolsó leállítás hello pontról. Hello `fragment_absolute_time` és `fragment_index` folyamatosan növelje, de megengedett toointroduce kihagyást, ha szükséges. Media Services figyelmen kívül hagyja azt már fogadása és feldolgozása, töredék, hogy jobban tooerr mint toointroduce folytonosság megszakítását hello media idővonalon töredék Újraküldés hello oldalán. 

## <a name="9-encoder-redundancy"></a>9. kódoló redundancia
Az egyes kritikus helyezkednek el, hogy igény szerint még magasabb rendelkezésre állást és minőséget felhasználói felület, azt javasoljuk, hogy használja-e aktív-aktív redundáns kódolók tooachieve zökkenőmentes feladatátvétel adatvesztés nélküli események.

![kódoló redundancia][image6]

Ezen a diagramon módon kódolók két csoportját leküldéses két példányt készítsen az egyes adatfolyamokkal egyidejűleg hello élő szolgáltatásba. A telepítő utasítás támogatott, mert a Media Services ismétlődő töredék adatfolyam-Azonosítót és töredék időbélyeg alapján is szűrheti. hello eredményül kapott élő adatfolyam és archiválási egy példányát minden hello adatfolyamot, amely hello lehető legjobb összesítési hello két forrásból származó. Például elméleti szélsőséges esetben mindaddig, amíg nincs egy kódoló (toobe hello azonos egy nem rendelkezik) futtató álljon az egyes adatfolyamokkal időben hello élő adatfolyam hello szolgáltatásból származó folyamatos adatvesztés nélkül. 

a forgatókönyv követelményei hello szinte hello azonos hello követelmény hello "Kódoló feladatátvételi" esetében, az kódolók második együttesét hello hello történő futtatása hello kivétellel azonos egyidejűleg elsődleges kódolók hello.

## <a name="10-service-redundancy"></a>10. szolgáltatás redundancia
Magas redundáns globális terjesztési néha rendelkeznie kell kereszt-régió biztonsági mentési toohandle regionális vészhelyzetek esetére. Hello "Kódoló redundancia" topológia kibővítve, az ügyfelek választhat toohave redundáns szolgáltatástelepítésben egy másik régióban kódolók második együttesét hello kapcsolódnak. Az ügyfelek is használhatják az a Content Delivery Network szolgáltató toodeploy egy globális Traffic Manager hello két szolgáltatás központi telepítések tooseamlessly útvonal ügyfélforgalom elé. hello kódolók hello követelményei, ugyanaz, mint a case "Kódoló redundancia" hello hello. hello csak kivétel ez alól, hogy kódolók igények toobe második együttesét hello különböző tooa emelni, működés közbeni betöltési végpont. hello alábbi ábrán ez a beállítás:

![szolgáltatás redundancia][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Speciális típusú adatfeldolgozást formátumok
Ez a szakasz ismerteti, amelyek a tervezett toohandle meghatározott forgatókönyvek élő adatfeldolgozást formátumok speciális típusú.

### <a name="sparse-track"></a>Ritka nyomon követése
Ha a bemutató élő adatfolyam-továbbítási gazdagügyfél nyújthassunk, gyakran szükséges tootransmit idő szinkronizálva események vagy sávon hello fő media adatokkal jelzi. Ilyen például a dinamikus élő ad beszúrási. Az ilyen típusú esemény jelzés rendszeres hang-és videófolyamot ritka jellegénél streaming eltér. Más szóval hello jelzés adatok általában nem fordulhat elő, folyamatosan, és hello időköz rögzített toopredict lehet. ritka követése hello fogalma nem tervezett tooingest, valamint a szórásos sávon jelzésátviteli adatokat.

hello lépések megegyeznek a javasolt megvalósításának választásával dolgozhat fel ritka nyomon követése:

1. Hozzon létre egy külön töredezett MP4 bitstream csak ritka nyomon követi, hang-és videófolyamot számok nélkül tartalmazó.
2. A hello **kiszolgáló Manifest mezőbe élő** meghatározott a 6 [1], használja a hello *parentTrackName* toospecify hello paraméternév hello szülő track. További információkért lásd: [1] 4.2.1.2.1.2 szakasz.
3. A hello **kiszolgáló Manifest mezőbe élő**, **manifestOutput** be kell állítani túl**igaz**.
4. Jellegéből hello ritka esemény jelzés hello, ajánlott hello következő:
   
    a. A hello élő esemény hello elején hello kódolók hello kezdeti fejléc mezőkbe toohello szolgáltatást, amely lehetővé teszi, hogy hello szolgáltatás tooregister hello ritka követése hello ügyfél jegyzékben.
   
    b. hello kódoló hello HTTP POST-kérelmet kell véget, amikor az adatok küldése nem működik. Amely adatokat küldeni a hosszan futó HTTP POST megakadályozhatja, hogy a Media Services gyorsan hello esemény, a szolgáltatás frissítése vagy a kiszolgáló újraindítását hello kódoló leválasztása. Ezekben az esetekben hello kiszolgáló átmenetileg blokkolva van a hello szoftvercsatorna fogadási művelet.
   
    c. Hello időpontot, amikor adatokat jelzés nem érhető el, során hello kódoló zárjon hello HTTP POST-kérelmet. Amíg aktív hello POST-kérelmet, hello kódoló küldjön adatokat.

    d. Ritka töredék küldésekor hello kódoló beállíthatja egy explicit content-length fejlécet, amennyiben az rendelkezésre áll.

    e. Az új kapcsolat ritka töredék küldésekor hello kódoló kell értesítésküldés hello fejléc mezőkben, hello új töredék követ. Ez olyan esetekben a feladatátvételt végző között történik, és új ritka kapcsolat hello folyamatban van a meghatározott tooa új kiszolgálót, amely nem tapasztalt hello ritka követése előtt.

    f. hello ritka követése töredék elérhető toohello ügyfél kerül, ha a hello megfelelő szülő követése töredék egyenlő vagy nagyobb timestamp érték legyen elérhető toohello ügyfél. Például ha hello ritka töredék t időbélyegző = 1000, várhatóan, amely után a hello ügyfél látja "videó" (feltéve hello szülő követése neve "videó") darabolható időbélyeg 1000, vagy túl, hogy letölthesse hello ritka töredék t = 1000. Vegye figyelembe, hogy a tényleges jel hello hello bemutató ütemterv egy másik pozícióba a kijelölt céljának felhasználhatók. Ebben a példában, előfordulhat, hogy a ritka kódrészletet t hello tartozó = 1000 rendelkezik egy XML-adattartalmat, amely beszúrásához ad egy helyen, amely néhány másodperc később.

    g. ritka követése töredék hello hasznos lehet a különböző formátumokban (pl. XML, szöveges vagy bináris), hello forgatókönyvtől függően.

### <a name="redundant-audio-track"></a>Redundáns hang nyomon követése
Egy tipikus HTTP adaptív adatfolyam-továbbítási forgatókönyv (például Smooth Streaming vagy kötőjel) gyakran, nincs hello teljes bemutató csak egy hang követése. Videó nyomon követi, amelyeket a hello ügyfél toochoose a hibaüzenet feltételekben több szolgáltatásminőségi szinteket, eltérően hello hang követése lehet a hibaérzékeny pontok kialakulását Ha hello adatfeldolgozást hello hang követése tartalmazó hello adatfolyam sérült. 

a probléma, a Media Services támogatja toosolve élő redundáns zeneszámok az adatfeldolgozást. hello lényege, hogy ugyanazon hang követése küldhető többször a különböző adatfolyamokba hello. Bár hello szolgáltatás csak regisztrál hello hang követése egyszer hello ügyfél jegyzékben, azt is használhatja redundáns zeneszámok tartalékként hang töredék lekéréséhez, ha hello elsődleges hang követése problémákkal rendelkezik. redundáns zeneszámok tooingest, hello kódoló kell megfelelnie:

1. Hozzon létre több töredék MP4 bitstreams hello azonos hang nyomon követéséhez. hello redundáns zeneszámok szemantikailag kell lennie, a hello azonos időbélyegeket darabolható, cserélhető hello fejléc és töredék szintjén kell.
2. Győződjön meg arról, hogy hello "hang" bejegyzést a hello élő Server Manifest (6 [1]. szakasz) van hello azonos minden redundáns zeneszámok.

a következő megvalósítási hello redundáns zeneszámok ajánlott:

1. Minden egyedi hang követése küldése-adatfolyamot ad át önmagába. Is, az egyes ezek zenei adatfolyamot, amelyen hello második adatfolyam eltér hello először csak a HTTP POST URL-cím hello hello azonosító által a redundáns adatfolyam küldése: {protokoll} :// {server-cím} / {pont path}/Streams({identifier}) közzététele.
2. Használjon külön adatfolyamok toosend hello két legalacsonyabb videó bitrates. Egyes ezekbe az adatfolyamokba ugyancsak tartalmaznia kell egy példányát minden egyedi hang nyomon követése. Ha több nyelvet is támogat, például ezekbe az adatfolyamokba tartalmaznia kell az egyes nyelvekhez zeneszámok.
3. Külön kiszolgáló (kódoló) példányok tooencode használja, és küldi hello redundáns adatfolyamok szerepel (1) és (2). 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
