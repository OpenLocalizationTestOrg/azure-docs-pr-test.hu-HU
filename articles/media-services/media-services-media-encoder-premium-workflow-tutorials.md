---
title: "aaaAvanced Media Encoder prémium munkafolyamat oktatóprogramok"
description: "Ez a dokumentum tartalmaz, hogyan tooperform speciális Media Encoder prémium munkafolyamat rendelkező tevékenységek megjelenítése forgatókönyvek és hogyan is toocreate bonyolult munkafolyamatok a munkafolyamat-tervezővel."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 641bd1be3bd795b5e138fa7ffdf299ff6710904e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Speciális Media Encoder prémium munkafolyamat oktatóprogramok
## <a name="overview"></a>Áttekintés
Ez a dokumentum tartalmaz, amelyek megjelenítik forgatókönyvek hogyan toocustomize munkafolyamatok **munkafolyamat-Tervező**. Található hello tényleges munkafolyamatfájlokat [Itt](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>TARTALOMJEGYZÉK
hello a következő témakörök ismertetnek:

* [Az egyszeres sávszélességű MP4 kódolási MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Új munkafolyamat indítása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Media fájl bemeneti hello használata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Tanulmányozza az adatfolyamok](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Az egy videókódoló hozzáadása. MP4-fájl létrehozása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [Kódolási hello hangadatfolyam](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Audió és videó-adatfolyamokat multiplexáló egy MP4-tárolóba](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [Hello MP4-fájl írása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Egy Media Services eszköz hello kimeneti fájl létrehozása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [Teszt hello befejeződött helyben munkafolyamat](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [Kódolás MXF multibitrate MP4 - a dinamikus becsomagolás engedélyezve](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Egy vagy több további MP4 kimenetek hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [Kimeneti nevek konfigurálása hello fájl](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Egy külön lejátszása hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [Hello hozzáadása. ISM SMIL fájl](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [Kódolási MXF multibitrate MP4 - továbbfejlesztett tervezetének be](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [Munkafolyamat áttekintése tooenhance](#workflow-overview-to-enhance)
  * [A fájlok elnevezési konvenciók](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [Hello munkafolyamat legfelső szintű alakzatot közzétételi összetevő tulajdonságai](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Kimeneti fájl nevét közzétett tulajdonságértékek támaszkodnak hozta létre.](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Miniatűrök toomultibitrate MP4 kimenet hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [Munkafolyamat áttekintése tooadd miniatűrök](#workflow-overview-to-add-thumbnails-to)
  * [JPG kódolás hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [A szín terület átalakítás foglalkozó](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Írás hello miniatűrök](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [A munkafolyamat hibáinak észleléséhez](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Befejezett munkafolyamat](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Tisztítás időalapú multibitrate MP4 kimenet](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Munkafolyamat áttekintése toostart levágási történő hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Az adatfolyam vágó hello használata](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Befejezett munkafolyamat](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Introducing hello összetevő parancsprogrammal létrehozva](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [A munkafolyamaton belül Scripting: hello world](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Keret-alapú levágási multibitrate MP4 kimenet](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Tervezetének áttekintése toostart levágási történő hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Hello klip lista XML használatával](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [Egy parancsprogram összetevő hello klip listájának módosítása](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [Egy ClippingEnabled kényelmi tulajdonság hozzáadása](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>Az egyszeres sávszélességű MP4 kódolási MXF
A forgatókönyv létrehozunk egy egyféle sávszélességű. MP4-fájlokat a AAC-HE kódolású hangja egy. MXF bemeneti fájl.

### <a id="MXF_to_MP4_start_new"></a>Új munkafolyamat indítása
Nyissa meg a munkafolyamat-tervezőben, és válassza ki a "Fájl"-"új munkaterület"-"átkódolására tervezetének"

Új munkafolyamat hello 3 elemek jelennek meg:

* Elsődleges forrásfájl
* XML klip listázása
* Kimeneti fájl vagy eszköz  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Új kódolási munkafolyamat*

### <a id="MXF_to_MP4_with_file_input"></a>Media fájl bemeneti hello használata
A sorrend tooaccept a bemeneti médiafájl egy kezdődik-e Media fájl bemeneti összetevő hozzáadása. tooadd összetevő toohello munkafolyamat keressen hello tárház keresési mezőbe, majd szükséges hello bejegyzés húzza hello Tervező ablak. Ezt a lépést a hello Media fájl bemeneti, és csatlakoztassa hello elsődleges forrásfájl összetevő toohello fájlnév bemeneti PIN-kód Media fájl bemeneti hello.

![Csatlakoztatott médiafájl bemeneti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Csatlakoztatott médiafájl bemeneti*

Mielőtt tehetünk ennek sok más, először kell tooindicate toohello munkafolyamat-Tervező milyen mintafájl toouse toodesign tapasztalatairól a munkafolyamatot. toodo tehát hello Tervező ablak háttér kattintson, és keresse meg hello elsődleges forrásfájl tulajdonság a hello tulajdonság jobb oldali panelen. Kattintson a hello ikonja, és válassza hello szükséges fájl tootest hello munkafolyamat. Amint ez történik, hello Media fájl bemeneti összetevő hello fájl vizsgálata, és a PIN-kódok tooreflect hello kimenetfájlba ellenőrizni azt tölteni.

![Ki van töltve médiafájl bemeneti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Ki van töltve médiafájl bemeneti*

Azt határozza meg, mi adjon meg szeretnénk az toowork rendelkező, amíg nem arról, hogy még ahol kódolású hello kimeneti kell lépjen. Elsődleges forrásfájl be lett állítva, hasonló toohow hello hello kimeneti mappa változóját megadó tulajdonság, akkor alatt konfigurálhatja.

![Konfigurált bemeneti és kimeneti tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Konfigurált bemeneti és kimeneti tulajdonságai*

### <a id="MXF_to_MP4_streams"></a>Tanulmányozza az adatfolyamok
Gyakran kívánt hello adatfolyam megjelenésének hasonlóan tooknow áthaladó hello munkafolyamat. hello munkafolyamat bármely pontján adatfolyam tooinspect kattintson egy kimeneti vagy bemeneti PIN-kódjának hello az összetevőket. Ebben az esetben próbálkozzon hello tömörítetlen videó kimeneti PIN-kód az adathordozó fájl bemeneti kattint. A párbeszédpanel ekkor megnyílik, amely lehetővé teszi a tooinspect hello kimenő videó.

![Ellenőrző hello tömörítetlen videó kimeneti PIN-kód](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Ellenőrző hello tömörítetlen videó kimeneti PIN-kód*

Ebben az esetben közli a gép például, hogy azt még foglalkozó egy 1920 x 1080 ráfordítás 24 képkockák másodpercenkénti 4:2:2 mintavételi szinte 2 perces videót.

### <a id="MXF_to_MP4_file_generation"></a>Az egy videókódoló hozzáadása. MP4-fájl létrehozása
Vegye figyelembe, hogy most már, egy tömörítetlen videó és a több tömörítetlen hang kimeneti PIN-kód az adathordozó fájl bemeneti használhatók. A sorrend tooencode hello bejövő videó, igazolnia kell egy kódolási összetevő - ebben az esetben előállítása érdekében. MP4-fájlokat.

tooencode hello video-adatfolyamot tooH.264, vegye fel a hello AVC Videókódoló összetevő toohello Tervező felületére. Ez az összetevő egy Kibontás video-adatfolyamot bemenetként veszi, és kézbesíti az AVC tömörített video-adatfolyamot a kimeneti PIN-kód a.

![Frissíthető AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Frissíthető AVC kódoló*

A tulajdonság határozza meg, hogyan hello kódolás pontosan történik. Most rá egy pillantást, hello néhány fontosabb beállítások:

* Kimeneti szélességének és magasságának kimeneti: ezek határozzák meg a kódolt hello videó hello feloldását. Abban az esetben, ha pedig ugorjunk az 640 x 360
* Képkockasebessége: set toopassthrough azt fogja csak hello forrás képkockasebessége elfogadják, esetén lehetséges toooverride azonban ebben. Vegye figyelembe, hogy ilyen képkockasebességhez átalakítás nem mozgásérzékelő – a kompenzációt.
* Profil és szint: ezek határozzák meg, hello AVC-profil és szintjét. További információ a feladatnaplókban tooconveniently hello különböző szintű és profilok, kattintson a hello kérdőjel ikon hello AVC videó kódoló összetevő, és hello súgólap megjeleníti az egyes hello szintek további információkra. A minta ugorjunk fő profillal 3.2 (hello alapértelmezett) szinten.
* Értékelje a mód és átviteli sebesség (KB/s): a mi esetünkben azt egy állandó átviteli sebesség (CBR), 1200-as kbps kimeneti választhat
* : Videó ez formátuma kapcsolatos hello VUI (videó használhatóság adatokat), amely lekérdezi hello H.264 adatfolyamba való írása (ügyféloldali információt, amelyik alkalmas lehet a dekóder tooenhance hello megjelenítési, de nem feltétlenül toocorrectly dekódolása):
* NTSC (Egyesült Államok vagy japán, 30 fps használatával a jellemző)
* PAL (Európa, 25 fps használatával a jellemző)
* GOP méretezési módját: konfigurálását végezzük el GOP rögzített méretű a lezárt GOPs 2 másodperc kulcs időközt a célokra. Ez biztosítja a kompatibilitást a hello Azure-Media Services dinamikus becsomagolást biztosít.

toofeed az AVC kódoló hello tömörítetlen videó kimeneti PIN-kód csatlakoztatja a hello Media fájl bemeneti összetevő toohello tömörítetlen videó bemeneti PIN-kódot hello AVC kódoló.

![Csatlakoztatott AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Csatlakoztatott AVC fő kódoló*

### <a id="MXF_to_MP4_audio"></a>Kódolási hello hangadatfolyam
Ezen a ponton azt videó van kódolva, de hello eredeti tömörítetlen hangadatfolyam továbbra is hozzá kell tömörített toobe. Ez azt fogja látogassa meg AAC kódolással hello AAC kódoló (Dolby) összetevő. Toohello munkafolyamat adja hozzá.

![Frissíthető AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Frissíthető AAC kódoló*

Most nincs inkompatibilitás: nincs csak egyetlen tömörítetlen hang bemeneti PIN-kódot a hello AAC kódoló, amíg több mint valószínű hello Media fájl bemeneti kell két különböző tömörítetlen hangadatfolyam érhető el: egy a bal oldali hang csatorna és az egyik az hello hello jogosultság. (Ha között legyen hang foglalkozó, ez 6 csatornák.) Ezért nem lehetséges toodirectly hello Media fájl bemeneti forrás hello hang kapcsolódnak hello AAC hang kódoló. hello AAC összetevő vár egy úgynevezett "kihagyásos" hangadatfolyam:, amelyek mindegyikét egy adatfolyam hello balra és hello jobb csatornák időosztásos egymással. Után tudjuk a forrás-adathordozó fájlból, mely zeneszámok hello forrás milyen pozíciója a azt hozhat létre a hello ilyen kihagyásos hangadatfolyam megfelelően rendelt hangalapú pozíciók bal és jobb.

Először egy olyan kihagyásos adatfolyam a szükséges hello forrás hang csatornák toogenerated érdemes. hello hang adatfolyam Interleaver összetevő kezelnek Ez az USA. Toohello munkafolyamat hozzáadása, és csatlakoztassa hello hang kimenetek hello Media fájl bemeneti bele.

![Csatlakoztatott hangadatfolyam Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Csatlakoztatott hangadatfolyam Interleaver*

Most, hogy egy kihagyásos hangadatfolyam, azt még nem adott meg ahol tooassign hello balra vagy jobbra hangalapú pozíciót szeretnénk. Ennek rendelés toospecify, azt hello hangalapú pozíció Assigner használhatják fel.

![Egy hangalapú pozíció Assigner hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Egy hangalapú pozíció Assigner hozzáadása*

Hello hangalapú pozíció Assigner használatra sztereó bemeneti adatfolyam keresztül egy kódoló beállított szűrő az "Egyéni" konfigurálásához, valamint hello csatorna beállított "(L, R) 2.0" nevezik. (Ez rendeli hello hangalapú bal oldali pozíciója toochannel 1 és hello jobb hangalapú pozíció toochannel 2.)

Csatlakozás hello hangalapú pozíció Assigner toohello bemeneti hello AAC kódoló hello kimenetét. Ezt követően adja a hello AAC kódoló toowork egy "2.0 (L, R)" csatorna-készletet, így az tudni fogja, hogy a bemeneti sztereó hang toodeal.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Audió és videó-adatfolyamokat multiplexáló egy MP4-tárolóba
Az AVC megadott kódolt video-adatfolyamot és a AAC kódolású hangadatfolyam, azt is rögzítheti, mindkettő egy. MP4-tároló. hello különböző adatfolyamokba keverési be egyetlen egy folyamathoz "multiplexáló" (vagy a "muxing"). Ebben az esetben azt még kihagyásos hello hang- és hello video-adatfolyamot összefüggő egyetlen. MP4-csomag. hello összetevő koordinálására ezt egy. MP4-tárolónak a neve hello ISO MPEG-4 Multiplexer. Adja hozzá egy toohello Tervező felületére, és csatlakoztassa a hello AVC videó kódoló és a hello AAC kódoló tooits bemeneti adatok.

![Csatlakoztatott MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Csatlakoztatott MPEG4 Multiplexer*

### <a id="MXF_to_MP4_writing_mp4"></a>Hello MP4-fájl írása
Kimeneti fájl írásakor hello kimeneti fájl összetevő szolgál. A toohello kimenetét ISO MPEG-4 Multiplexer hello azt is elérheti, így az a kimeneti lekérdezi toodisk. toodo, csatlakozás hello (MPEG-4) tároló kimeneti PIN-kód toohello írási bemeneti PIN-kódja hello kimeneti fájl.

![Kimeneti fájl csatlakoztatva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Kimeneti fájl csatlakoztatva*

hello fájlnév használandó hello fájl tulajdonság határozza meg. Tulajdonság értéke szoftveresen kötött tooa lehetnek, nagy valószínűséggel tooset érdemes azt a kifejezést helyette.

toohave hello munkafolyamat automatikusan meghatározni a hello kimeneti fájlt a name tulajdonság kifejezésből, kattintson a hello buton következő toohello fájl neve (következő toohello ikonja). Hello a legördülő menüből, majd válassza a "Kifejezése". Megjelenik a hello Kifejezésszerkesztő. Először törölje a hello szerkesztő hello tartalmát.

![Üres kifejezés-szerkesztő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Üres kifejezés-szerkesztő*

hello kifejezés szerkesztő segítségével tooenter szöveges értéket, és egy vagy több változót. Változók dollárjelet kezdődik. Találati hello $ kulcs, mert a hello szerkesztő változók közül választhat a legördülő listában jelennek meg. A mi esetünkben hello kimeneti könyvtár változó és a hello alap bemeneti fájlnév változó fogjuk használni:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Kitöltött kimenő kifejezés-szerkesztő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Kitöltött kimenő kifejezés-szerkesztő*

> [!NOTE]
> Ahhoz, toosee az Azure-ban a kódolási feladat kimeneti fájl megtekintéséhez hello kifejezés szerkesztőben értéket kell megadnia.
>
>

Ha meggyőződött róla hello kifejezés által elérte-e az OK gombra, hello tulajdonság ablakban ezen a ponton a időben történő toowhat érték hello fájl tulajdonság oldja fel a rendszer előzetes megtekintéséhez.

![Fájl kifejezés kimeneti dir oldja fel.](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Fájl kifejezés kimeneti dir oldja fel.*

### <a id="MXF_to_MP4_asset_from_output"></a>Egy Media Services eszköz hello kimeneti fájl létrehozása
Amíg azt írt kimeneti MP4-fájlokat, továbbra is szükséges tooindicate, hogy a fájl tartozik toohello kimeneti eszköz mely media services hoz létre a munkafolyamat végrehajtása miatt. toothis célból hello kimeneti fájl/eszköz csomópontot a vásznon a hello munkafolyamat szolgál. Ez a csomópont minden bejövő fájlok megkönnyítő hello eredményül kapott Azure Media Services eszközt.

Csatlakozás hello kimeneti fájl összetevő toohello kimeneti fájl/eszköz összetevő toofinish hello munkafolyamat.

![Befejezett munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Befejezett munkafolyamat*

### <a id="MXF_to_MP4_test"></a>Teszt hello befejeződött helyben munkafolyamat
helyileg, a tootest hello munkafolyamat találati hello eszköztár hello felső hello lejátszás gombra. Befejezésekor hello munkafolyamat végrehajtása vizsgálhatja hello kimeneti konfigurált hello kimeneti mappában jön létre. Láthatja, hogy hello hello MXF bemeneti forrás fájlból kódolt MP4 kimeneti fájlok befejeződött.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Kódolás MXF MP4 - multibitrate a dinamikus becsomagolás engedélyezve
A forgatókönyv nem fogja létrehozni a több sávszélességű MP4-fájlokat kódolású AAC hang egyetlen. MXF bemeneti fájl.

Ha egy többszörös sávszélességű eszköz kimeneti van szükség együttesen használják az Azure Media Services szolgáltatásban több GOP igazított MP4-fájlok az egyes egy másik átviteli sebesség és a feloldási kell generált toobe által kínált hello dinamikus becsomagolás funkcióival. Igen, hello toodo [kódolás MXF be egy egyféle sávszélességű MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) útmutatóul szolgál, az jó kiindulási pont.

![Munkafolyamat indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Munkafolyamat indítása*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Egy vagy több további MP4 kimenetek hozzáadása
Az eredményül kapott Azure Media Services eszközt minden MP4-fájlokat egy másik átviteli sebesség és a feloldási fogja támogatni. Egy vagy több MP4 kimeneti fájlok toohello munkafolyamat adjuk hozzá.

toomake meg arról, hogy tudunk a videó kódolók létrehozott összes hello ugyanazokat a beállításokat, fennállt legkényelmesebben tooduplicate már meglévő AVC Videókódoló hello, és állítsa be névfeloldási és sávszélességű egy másik kombinációja (adjuk hozzá 960 x 540 egyikét: 25 képkockák másodpercenkénti: 2,5 MB/s). tooduplicate hello meglévő kódoló másolási illessze be a hello Tervező felületére.

Csatlakozás hello tömörítetlen videó kimeneti hello Media fájl bemeneti az új AVC összetevő be PIN kódját.

![Második AVC kódoló csatlakoztatva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Második AVC kódoló csatlakoztatva*

Most alkalmazkodnak az új AVC kódoló toooutput 960 x 540 2,5 Mbit/s hello konfigurációját. (Használható tulajdonságok "kimeneti szélességének", "Kimeneti magasság" és "Átviteli sebesség (KB/s)" a ez.)

Megadott toouse hello eredményül kapott eszköz együtt az Azure Media Services dinamikus becsomagolást szeretnénk, hello adatfolyam-továbbítási végpontra kell alkalmas MP4 fájlokat, amelyek pontosan igazított tooeach más módon HLS/Fragmented MP4/DASH-töredék toobe hogy az ügyfelek, amelyek között különböző bitrates vált lekérni egy egyetlen zökkenőmentes folyamatos video- és felhasználói élmény. amely fordulhat elő, igazolnia kell, hogy mindkét AVC kódolók hello tulajdonságaiban hello mindkét MP4-fájlok mérete GOP ("csoport képek") értéke tooensure toomake too2 másodpercen belül végezhető el:

* hello GOP méretezési módját tooFixed GOP méret a következőre és
* hello kulcs keret időköz tootwo (másodperc).
* hello GOP IDR vezérlő tooClosed GOP tooensure összes GOP vannak állandó is be a saját nélkül függőségek

toomake a munkafolyamat kényelmes toounderstand hello első AVC kódoló túl átnevezése "AVC videó kódoló 640 x 360 1200-as kbps" és a második AVC kódoló hello "AVC videó kódoló 960 x 540 2500 kbit/s".

Ezután adja hozzá a második ISO MPEG-4 Multiplexer és egy második fájl kimenet. Csatlakozás hello multiplexer toohello új AVC kódoló, és győződjön meg arról, annak kimenetét a kimeneti fájl hello van átirányítva. Ezután is csatlakoznak hello AAC hang kódoló kimeneti toohello új multiplexer tartozó bemeneti. hello kimeneti fájl pedig majd lehet csatlakoztatott toohello kimeneti fájl/eszköz csomópont tooadd azt toohello Media Services eszköz, amely jön létre.

![Második Muxer és a kimeneti fájl csatlakoztatva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Második Muxer és a kimeneti fájl csatlakoztatva*

Azure Media Services dinamikus becsomagolást is kompatibilisek, a multiplexer tartozó hello adatrészlet mód tooGOP számát vagy a duration konfigurálása, valamint hello GOPs adatrészlet too1 / beállítása. (Az alapértelmezett hello ennek kell lennie.)

![Muxer adatrészlet módok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer adatrészlet módok*

Megjegyzés: érdemes lehet toorepeat ezt a folyamatot az átviteli sebesség és feloldási toohave kívánt kombinációk toohello eszköz kimenet hozzáadva.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Kimeneti nevek konfigurálása hello fájl
Egynél több egyfájlos hozzáadott toohello kimeneti adategységen van. Ez lehetővé teszi egy szükséges toomake meg arról, hogy hello az egyes hello fájlnevek kimeneti fájlok különbözik egymástól, és lehet, hogy még érvényes a fájl-elnevezési konvenció így világossá válik, a fájl nevéből hello a most foglalkozó.

Kimeneti fájlelnevezésnél szabályozható kifejezések hello tervezőben. Nyissa meg a kimeneti fájl összetevői hello tulajdonság ablaktábla hello, és nyissa meg a kifejezés objektumszerkesztője hello hello fájl tulajdonság. Az első kimeneti fájl lett konfigurálva, a következő kifejezés hello keresztül (lásd: hello útmutató át [MXF tooa egyféle sávszélességű MP4 kimeneti](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Ez azt jelenti, hogy a fájlnév határozza meg két változót: hello kimeneti könyvtár toowrite a és hello adatforrás fájl neve. hello volt hello munkafolyamat legfelső szintű tulajdonságként van közzétéve, és ez utóbbi hello hello bejövő fájl határozza meg. Vegye figyelembe, hogy hello kimeneti könyvtár helyi tesztelési; használata Ez a tulajdonság felülbírálja hello munkafolyamat-motor hello munkafolyamat hello felhőalapú media feldolgozó Azure Media Services eljárás végrehajtásakor.
toogive mind a kimeneti fájlok elnevezési konzisztens kimeneti, a módosítás hello először fájl elnevezési kifejezés:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

és a második hello:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Hajtsa végre egy köztes teszt futtatása toomake meg arról, hogy mindkét MP4 kimeneti fájlok megfelelően jönnek létre.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Egy külön lejátszása hozzáadása
Megtanulhatja, később generálásakor azt egy .ism-fájlt toogo az MP4 kimeneti fájlokat, mivel azt is szüksége van egy csak MP4-fájlokat hello hang nyomon követése, a az adaptív streameléshez. toocreate a fájl, adja hozzá a további muxer toohello munkafolyamat (Multiplexer ISO-MPEG-4), és csatlakozzon a hello AAC kódoló kimeneti PIN-kód és a bemeneti PIN-kód követése 1.

![Hang Muxer hozzáadva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Hang Muxer hozzáadva*

Kimeneti fájl összetevő toooutput hello kimenő adatfolyam harmadik alapján hello muxer és hello fájl elnevezési kifejezés konfigurálása:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Hang Muxer kimeneti fájl létrehozása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Hang Muxer kimeneti fájl létrehozása*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Hello hozzáadása. ISM SMIL fájl
A hello dinamikus becsomagolás toowork együtt mindkét MP4-fájlokat (és hello csak MP4) a Media Services eszközt is kell a jegyzékfájlt (más néven "SMIL" fájlba: multimédia integrációs nyelvi szinkronizálva). Ez a fájl milyen MP4-fájlok érhetők el a dinamikus csomagolás és amely ezeket az hello hang adatfolyamként történő tooconsider tooAzure Media Services jelzi. Egy tipikus jegyzékfájl állítja be a MP4 meg egyetlen hangadatfolyam a következőképpen néz ki:

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

hello .ism-fájlt tartalmaz egy switch utasításban, egy hivatkozás tooeach hello egyedi MP4 videó fájlok belül, és emellett toothose is egy (vagy több) hangfájl hivatkozik tooan hello hang tartalmazó MP4.

Hello jegyzékfájl létrehozása az MP4 tartozó számú végezhető el hello "AMS Manifest író" nevű összetevőt. toouse, húzza hello felületet, és csatlakoztassa hello "Írási kész" kimeneti PIN-kódok hello három kimeneti fájl összetevők toohello AMS Manifest író adjon meg. Győződjön meg arról, hogy tooconnect hello kimeneti hello AMS Manifest író toohello kimeneti fájl vagy eszköz.

A többi fájl kimeneti összetevők konfigurálását a hello .ism kimeneti fájlnév kifejezéssel:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

A befejezett munkafolyamat alábbi hello néz ki:

![Befejezett MXF toomultibitrate MP4 munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Befejezett MXF toomultibitrate MP4 munkafolyamat*

## <a id="MXF_to__multibitrate_MP4"></a>Kódolási MXF multibitrate MP4 - továbbfejlesztett tervezetének be
A hello [előző munkafolyamat forgatókönyv](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) azt láthatta, hogyan MXF egyetlen bemeneti eszköz létrehozható egy kimeneti eszköz többszörös sávszélességű MP4-fájlok, a csak MP4-fájlokat és a jegyzékfájlt, az Azure Media együtt használható Services dinamikus becsomagolást.

Ez a forgatókönyv hogyan néhány hello szempontok melyek fejlesztése és kényelmesebb végrehajtott jelennek meg.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Munkafolyamat áttekintése tooenhance
![Multibitrate MP4 munkafolyamat tooenhance](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4 munkafolyamat tooenhance*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>A fájlok elnevezési konvenciók
Hello előző munkafolyamat létrehozásának kimeneti fájl nevének hello alapjául azt meg egyetlen egyszerű kifejezésbe. Néhány duplikálva lettek-e, ha van: hello hello egyes kimeneti fájl összetevőket megadott ilyen kifejezés.

Például a fájl kimeneti összetevőjének hello első videofájl ebben a kifejezésben van konfigurálva:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

A hello második kimeneti videó, például a kifejezés vezetünk be:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Lenne kisebb hiba nagyon eséllyel fordulnak elő, és több akkor hasznos, ha azt nem sikerült távolítson el néhányat a ismétlődést és tétele több konfigurálható helyette tisztító? Szerencsére alábbiakat tehetjük: hello designer kifejezés képességek hello képességét toocreate egyéni tulajdonságok a munkafolyamat legfelső szintű együtt fog biztosítják egy hozzáadott réteget kényelmét szolgálja.

Tegyük fel, azt fogja hello bitrates hello egyedi MP4-fájlokat a fájlnév konfigurációs meghajtó. Ezek bitrates lesz igyekszünk tooconfigure a egy központi helyezze (hello gyökere a diagramhoz), a ahol azok lesz használt tooconfigure és a meghajtó fájlnév létrehozása. toodo, először hello sávszélességű tulajdonság mindkét AVC kódolók toohello legfelső szintű a munkafolyamat közzétételével, hogy az hello AVC kódolók maga mindkét hello legfelső szintű is elérhető. (Még akkor is, ha két különböző tesztüzeméhez jelenik meg, nincs csak egy alapul szolgáló érték.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>Hello munkafolyamat legfelső szintű alakzatot közzétételi összetevő tulajdonságai
Nyissa meg a hello első AVC kódoló toohello sávszélességű (kbps) tulajdonság nyissa és hello legördülő menüből válassza a Publish.

![Közzétételi hello sávszélességű tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Közzétételi hello sávszélességű tulajdonság*

Hello konfigurálása közzététele párbeszédpanelen toopublish toohello legfelső szintű a munkafolyamat gráf egy közzétett neve "video1bitrate" és "Videó 1 sávszélességű" olvasható megjelenítendő neve. Konfigurálja az egyéni csoport neve "Streaming Bitrates" néven, majd nyomja le a közzététel.

![Közzétételi hello sávszélességű tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Átviteli sebesség tulajdonság közzétételi párbeszédpanel*

Ismétlődő hello hello hello sávszélességű tulajdonságának azonos a második AVC kódoló, és nevezze el "Videó 2 sávszélességű" megjelenítendő nevű "video2bitrate", a hello azonos egyéni csoport "Streaming Bitrates".

A Microsoft hello munkafolyamat legfelső szintű tulajdonságok most vizsgálja meg, ha megtanulhatja az egyéni csoport hello két közzétett tulajdonság jelenik meg. Mindkét van tükrözve hello értékének a megfelelő AVC kódoló sávszélességű.

![A munkafolyamat legfelső szintű közzétett sávszélességű tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Ha azt szeretné, hogy tooaccess ezeket a tulajdonságokat, kódból vagy egy kifejezésből, azt is megteheti ehhez hasonló:

* a beágyazott kód egy összetevő hello gyökér alatti jobb oldali: node.getPropertyAsString('.. / video1bitrate ", null)
* kifejezésben: ${ROOT_video1bitrate}

Hello "Streaming Bitrates" csoport befejezéseként közzététele a zenei sávszélességű rajta is. Belül hello AAC kódoló hello tulajdonságait keresse meg a hello sávszélességű beállítás, és válassza a Publish hello legördülő következő tooit parancsát. A neve "audio1bitrate" hello graph toohello gyökérmappájában közzététele, és megjelenítendő név "Hang 1 sávszélességű" a "Folyamatos átviteli Bitrates" egyéni csoporton belül.

![A hang sávszélességű közzétételi párbeszédpanel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*A hang sávszélességű közzétételi párbeszédpanel*

![A gyökérszintű eredményül kapott video- és tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*A gyökérszintű eredményül kapott video- és tulajdonságai*

Ne feledje, hogy ezen három megváltoztatásával is értékei, konfigurálja újra módosítások hello hello megfelelő összetevőinek kapcsolódnak az értékeket (és egyes esetekben közzétett).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Kimeneti fájl nevét közzétett tulajdonságértékek támaszkodnak hozta létre.
Hardcoding helyett a létrehozott fájl nevének azt módosíthatja az egyes hello kimeneti fájl összetevők toorely hello sávszélességű tulajdonságainál azt csak közzétett hello gráf legfelső szintű fájlnév kifejezés. Az első kimeneti fájl verziótól kezdődően található hello fájl tulajdonság, és ehhez hasonló hello kifejezés szerkesztése:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

hello különböző paraméterek ebben a kifejezésben érhető el, és szerezze meg a hello dollárjel hello billentyűzet hello kifejezés ablakban adja meg. Hello rendelkezésre álló paraméterek egyike a video1bitrate tulajdonság, amely azt a korábban közzétett.

![Paraméterek kifejezésben elérése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Paraméterek kifejezésben elérése*

Hello ugyanazt a hello kimenetét a második videót:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

és hello csak fájl kimeneti:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Ha most módosítjuk hello sávszélességű bármely hello video- vagy hangfájlok, hello megfelelő kódoló újrakonfigurálni, és hello sávszélességű alapú fájl neve egyezmény szerződéses kötelezettségeket összes automatikus.

## <a id="thumbnails_to__multibitrate_MP4"></a>Miniatűrök toomultibitrate MP4 kimenet hozzáadása
Amely hoz létre egy munkafolyamat-től kezdődő [egy multibitrate MP4 kimenete egy bemeneti MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), azt most lehet szüksége a miniatűrök toohello kimenet hozzáadása.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Munkafolyamat áttekintése tooadd miniatűrök
![A Multibitrate MP4 munkafolyamat toostart](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*A Multibitrate MP4 munkafolyamat toostart*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG kódolás hozzáadása
a miniatűr generációs hello szív hello JPG kódoló összetevő, JPG-fájlokat képes toooutput lesz.

![JPG kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG kódoló*

Az adathordozó fájl bemeneti hello hello JPG kódoló be azonban közvetlenül a tömörítetlen Video-adatfolyamot nem kapcsolódni. Ehelyett vár toobe egyes keretek átadni. Ez a Microsoft hello videó keret kapu összetevő segítségével teheti meg.

![A keret kapu toohello JPG kódoló csatlakozás](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*A keret kapu toohello JPG kódoló csatlakozás*

hello keret kapu, miután minden olyan sok másodperceken vagy keretek lehetővé teszi, hogy a videó keret toopass. hello időköz és hello időeltolódás, amelyhez ez akkor fordul elő hello tulajdonságaiban konfigurálható.

![Videó keret kapu tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Videó keret kapu tulajdonságai*

Most percenként miniatűr létrehozása úgy, hogy hello mód tooTime (másodpercben), és időköz too60 hello.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>A szín terület átalakítás foglalkozó
Közben logikai tűnik mindkét hello keret kapu és hello Media fájl bemeneti tömörítetlen videó PIN-kód most csatlakozhat, a figyelmeztetés azt visszajelzést kap, ha azt szeretné ehhez.

![Bemeneti szín helyének hibája](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Bemeneti szín helyének hibája*

Ennek oka az, mely színű információk jelennek meg az eredeti nyers tömörítetlen video-adatfolyammá alakítja, a MXF érkező hello módja eltér milyen hello JPG kódoló által várt paraméterekkel. Több, egy úgynevezett "szín lemezterület" "RGB" vagy "Szürkeárnyalatos" van tooflow a várt. Ez azt jelenti, hogy hello videó keret kapu tartozó bejövő video-adatfolyamot kell toohave alkalmazza a szín terület vonatkozó először az átalakításhoz.

Hello munkafolyamat hello szín terület konverter - Intel alakzatot, és csatlakoztassa tooour keret kapu.

![Csatlakozás egy szín terület konverter](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Csatlakozás egy szín terület konverter*

Hello tulajdonságai ablakban válassza ki a hello BGR 24 bejegyzést hello előre definiált listából.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Írás hello miniatűrök
Eltér a MP4 videó, hello összetevő kimeneteként JPG kódoló több mint egy fájl. Ennek rendelés toodeal, a leképezni kívánt jelenetben keresési JPG fájl író összetevőt is használható: hello bejövő JPG miniatűrök igénybe vehet, és beírhatók, minden egyes eltérő számú által éppen utótaggal fájlnév. (hello azonosítószámát általában hello hello adatfolyamban mely hello miniatűr megrajzolása a másodperc/egységek számát.)

![Hello helyszín keresési JPG fájl író bemutatása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Hello helyszín keresési JPG fájl író bemutatása*

Hello kimeneti mappa elérési útja tulajdonság beállítása a hello kifejezéssel: ${ROOT_outputWriteDirectory}

és a fájlnév előtag tulajdonság hello:

    ${ROOT_sourceFileBaseName}_thumb_

hello előtag határozza meg, hogyan hello miniatűr fájlok neve alatt. Azok a rendszer a hello adatfolyam pozíciója egy számot jelző hello görgetőgomb tartozó utótaggal.

![Megjelenítés keresési JPG fájl író tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Megjelenítés keresési JPG fájl író tulajdonságai*

Csatlakozás hello helyszín keresési JPG fájl író toohello kimeneti fájl/eszköz csomópont.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>A munkafolyamat hibáinak észleléséhez
Csatlakozás hello szín terület konverter toohello nyers tömörítetlen videokimenetéhez hello bevitelt. Most futtassa a hello munkafolyamat helyi tesztjének elvégzéséhez. Egy jó eséllyel hello munkafolyamat hirtelen végrehajtása leállítása és egy piros vázlatot hello összetevő, amely a hibát jelző van:

![Szín terület konverter hiba](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Szín terület konverter hiba*

Kattintson a "E" ikonra kissé piros hello hello jobb felső sarkában hello szín terület konverter összetevő toosee mi hello OK hello kódolási kísérlet sikertelen volt.

![Szín terület konverter hiba-párbeszédpanelen.](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Szín terület konverter hiba-párbeszédpanelen.*

Változik, ahogy látja, hogy rendelkezik-e a kért átalakításához YUV tooRGB toobe rec601 hello bejövő szín terület hello szín terület konverter szabvány. Az adatfolyam látszólag nem jelzi annak rec601. (Jav. 601 a digitális videót formában váltakozó analóg videó jelek kódolási szabványos. Azt adja meg az aktív terület 720 fénysűrűség mintákat és 360 chrominance minták soronként. rendszer kódolás hello szín YCbCr 4 néven: 2:2.)

toofix, azt fogja az adatfolyam, amely jelenleg éppen foglalkozó rec601 tartalom hello metaadatainak jelzi. toodo, egy videó adatok típusa Frissítőjének összetevő, amelyeket igazolnia kell a Between a nyers forrás- és hello szín terület átalakítás összetevőjének fogjuk használni. Ezen adatok típusa frissítőjének hello kézi frissítés egyes videokártya adatok típustulajdonságokat teszi lehetővé. A szín terület szabványos a "Rec 601" tooindicate konfigurálja. Ennek hatására hello videó adatok típusa Frissítőjének tootag hello adatfolyam hello "Rec 601" szín területtel rendelkező Ha nincs meghatározva szín terület történt. (Nem felülírja a meglévő metaadatokat, kivéve, ha hello felülbírálás jelölőnégyzetet, de.)

![Az adatok típusa Frissítőjének hello szín terület szabványos frissítése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Az adatok típusa Frissítőjének hello szín terület szabványos frissítése*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Befejezett munkafolyamat
Most, hogy az a munkafolyamat befejeződött, akkor adjon át egy másik teszt futtatásakor toosee tegye.

![Befejezett munkafolyamat vázlattal multi-mp4-kimenet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Befejezett munkafolyamat vázlattal multi-mp4-kimenet*

## <a id="time_based_trim"></a>Tisztítás időalapú multibitrate MP4 kimenet
Amely hoz létre egy munkafolyamat-től kezdődő [egy multibitrate MP4 kimenete egy bemeneti MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), azt most kell keresése a díszítésre hello forrás videó időbélyegeket alapján.

### <a id="time_based_trim_start"></a>Munkafolyamat áttekintése toostart levágási történő hozzáadása
![A munkafolyamat tooadd levágási indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*A munkafolyamat tooadd levágási indítása*

### <a id="time_based_trim_use_stream_trimmer"></a>Az adatfolyam vágó hello használata
hello adatfolyam vágó összetevő lehetővé teszi, hogy tootrim hello kezdetét és végét egy bemeneti adatfolyam alapjául információk (másodperc, perc,...). hello vágó nem támogatja a keret-alapú tisztítás.

![Az adatfolyam vágó](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Az adatfolyam vágó*

Helyett linking hello AVC kódolók és hangalapú pozíció assigner toohello Media fájl bemeneti közvetlenül, azt fogja helyezze azokat hello adatfolyam vágó Between. (Egy hello videó jel, egy másik kihagyásos hang jel hello pedig.)

![Az adatfolyam vágó helyezze a kettő között](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Az adatfolyam vágó helyezze a kettő között*

Hello vágó pedig konfiguráljuk az, hogy a videó és hang 15 másodperc és hello videó 60 másodperc között csak azt fogja feldolgozni.

Nyissa meg hello videó adatfolyam vágó toohello tulajdonságait, és (15 mp) kezdete és a befejező időpont (60s) tulajdonságok konfigurálása. arról, hogy mind a hang- és vágó mindig azonos mikor kezdődjön és fejeződjön értékek konfigurált toohello toomake fogunk közzé tenni azokat hello munkafolyamat toohello gyökérmappájában.

![A kezdési idő tulajdonságot adatfolyam vágó közzététele](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*A kezdési idő tulajdonságot adatfolyam vágó közzététele*

![A kezdő időpont tulajdonság párbeszédpanel közzététele](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*A kezdő időpont tulajdonság párbeszédpanel közzététele*

![Tulajdonság párbeszédpanel közzé a befejezési időpontja](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Tulajdonság párbeszédpanel közzé a befejezési időpontja*

Azt a munkafolyamat hello gyökérmappájában most vizsgálja meg, ha mindkét tulajdonság lesz egyszerű jelennek meg és konfigurálható onnan.

![A gyökérszintű elérhető közzétett tulajdonságok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*A gyökérszintű elérhető közzétett tulajdonságok*

Most hello levágási tulajdonságainak megnyitása hello hang vágó, és állítsa be a kezdő és záró időpontjának toohello hivatkozó kifejezést hello legfelső szintű a munkafolyamat tulajdonságok közzététele.

A hello hang tisztítás kezdő időpontja:

    ${ROOT_TrimmingStartTime}

és a befejezési ideje:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Befejezett munkafolyamat
![Befejezett munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Befejezett munkafolyamat*

## <a id="scripting"></a>Introducing hello összetevő parancsprogrammal létrehozva
Parancsprogram-alapú összetevők tetszőleges parancsfájlokat futtathat a munkafolyamat hello végrehajtási fázisában. Négy különböző parancsprogramok hajt végre, az adott jellemzőit és a saját hello munkafolyamat életciklus-helyet:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

hello hello dokumentációját parancsprogrammal létrehozva összetevő kerül részletesebben minden fenti hello. A [szakasz következő hello](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** hello munkafolyamat indításakor parancsfájl-kezelési összetevője használt tooconstruct egy cliplist xml a hello menet közben. A parancsprogram neve csak egyszer történik meg életciklusának hello összetevő telepítése során.

### <a id="scripting_hello_world"></a>A munkafolyamaton belül Scripting: hello world
Húzzon egy parancsprogrammal létrehozva összetevő hello Tervező felületére, és adjon neki (például "SetClipListXML").

![A parancsfájlalapú összetevő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*A parancsfájlalapú összetevő hozzáadása*

Amikor hello tulajdonságainak nézze meg hello összetevő parancsprogrammal létrehozva, hello négy különböző parancsfájl típusok megjelenik, minden konfigurálható tooa másik parancsprogramot.

![A parancsfájlalapú összetevő tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*A parancsfájlalapú összetevő tulajdonságai*

Törölje a hello processInputScript és hello realizeScript hello-szerkesztő megnyitásához. Most azt beállításokat, és kész a toostart parancsfájlok.

Parancsfájlok Groovy, dinamikusan lefordított parancsnyelv hello Java platform, amely megőrzi a kompatibilitást a Java nyelven íródtak. A legtöbb Java-kóddal ténylegesen, érvényes Groovy kód.

Ideje lefuttatni egy egyszerű hello world groovy parancsfájl a realizeScript hello környezetében. Írja be a hello következő hello szerkesztőben:

    node.log("hello world");

Egy tesztcélú helyi Futtatás most végrehajtani. Ehhez a futtató után vizsgálja meg (hello rendszer lapját, amelyen keresztül hello összetevő parancsprogrammal létrehozva) hello naplók tulajdonság.

![Hello world kimenet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello world kimenet*

hello csomóponti objektum hello napló metódus hívása, tooour aktuális "csomópont" vagy a jelenleg éppen scripting belül hello összetevő hivatkozik. Minden összetevő használja, így azt hello képességét toooutput naplózási adatokat, hello lap keresztül érhető el. Ebben az esetben azt a kimeneti hello a literál "hello world" karakterlánc. Itt fontos toounderstand, hogy ez bizonyítja toobe egy hasznos információt hibakereső eszköz, így a ismereteket milyen hello parancsfájl ténylegesen tesz a.

A belül a parancsfájl-kezelési környezet, azt is rendelkezik hozzáféréssel tooproperties más összetevők. Próbálkozzon a következővel:

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up tooother nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

A napló ablakban jelennek meg velünk a következő hello:

![Napló kimeneti elérési útjának eléréséhez](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Napló kimeneti elérési útjának eléréséhez*

## <a id="frame_based_trim"></a>Keret-alapú levágási multibitrate MP4 kimenet
Amely hoz létre egy munkafolyamat-től kezdődő [egy multibitrate MP4 kimenete egy bemeneti MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), azt most kell keresése a díszítésre hello forrás videó keret érintett alapján.

### <a id="frame_based_trim_start"></a>Tervezetének áttekintése toostart levágási történő hozzáadása
![Munkafolyamat toostart levágási történő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Munkafolyamat toostart levágási történő hozzáadása*

### <a id="frame_based_trim_clip_list"></a>Hello klip lista XML használatával
Az összes korábbi munkafolyamat oktatóanyag a bemeneti videoforrást hello Media fájl bemeneti összetevő használja azt. Ebben a forgatókönyvben azonban használni fogjuk hello klip forráslista összetevő helyette. Vegye figyelembe, hogy lehetőleg ne legyen működő; hello előnyben részesített módja csak akkor alkalmazza hello klip forráslista, ha egy valódi OK toodo úgy van (például az alábbi esetet, ahol azt hajt hello hello klip lista levágási képességek használatát).

az adathordozó fájl bemeneti toohello klip forráslista, a tooswitch hello klip forráslista összetevő húzza hello a tervezési felülethez, és csatlakozzon a hello klip lista XML PIN-kód toohello klip lista XML-csomópont hello munkafolyamat-Tervező. Ez kitölti hello klip lista forrása a kimeneti PIN-kód, tooour bemeneti videó alapján történik. Most kapcsolódó hello tömörítetlen videóban és tömörítetlen hang PIN-kódok hello hello klip forráslista toohello megfelelő AVC kódolók és hang adatfolyam Interleaver. Hello Media fájl bemeneti eltávolítása.

![Hello Media fájl bemeneti helyére hello klip forráslista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Hello Media fájl bemeneti helyére hello klip forráslista*

hello klip forráslista összetevő "Klip lista XML" fogadja a bemeneti adatként. Lehetőséget választva hello forrás fájl tootest rendelkező helyi, a a klip lista XML-kódja, automatikus feltöltve meg.

![Automatikus feltöltve klip lista XML-tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatikus feltöltve klip lista XML-tulajdonság*

Egy kicsit szorosabb toohello xml keresése, ez az hogyan hasonlóan néz ki:

![Szerkesztése klip lista párbeszédpanel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Szerkesztése klip lista párbeszédpanel*

Ez azonban nem tükrözi hello klip lista xml hello képességeit. Egy tudunk elem tooadd egy "Vágás" elem alatt hello hang- és hang forrás ehhez hasonló:

![A vágás toohello klip elemlista hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*A vágás toohello klip elemlista hozzáadása*

Ha módosítja a hello klip lista xml ilyen felett, és helyi ellenőrzéséhez futtassa, látni fogja a hello videó megfelelően lett rövidített hello videó 10 és 20 másodperc között.

Ezt megteheti a helyi futtatás, bár ez nagyon azonos cliplist xml nem rendelkezik azonos érvényesíti az Azure Media Services futó munkafolyamatot alkalmazásakor hello ellentétes toowhat történik. Ha Azure prémium szintű kódolás elindul, hello cliplist xml jön létre minden alkalommal, amikor újra, alapján hello bemeneti hello fájlkódolás feladat lett megadva. Ez azt jelenti, hogy módosításokat hello XML-végezzük volna sajnos bírálható felül.

a kódolási feladat indításakor adatainak törlése toocounter hello cliplist xml azt is létre újból azt a hello menet közben a munkafolyamat hello elindítása után. Ilyen egyéni műveletek lehessen állítani a "Component parancsprogrammal létrehozva" úgynevezett keresztül. További információkért lásd: [Introducing hello parancsprogrammal létrehozva összetevő](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Húzzon egy parancsprogrammal létrehozva összetevő hello Tervező felületére, és nevezze át túl "SetClipListXML".

![A parancsfájlalapú összetevő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*A parancsfájlalapú összetevő hozzáadása*

Amikor hello tulajdonságainak nézze meg hello összetevő parancsprogrammal létrehozva, hello négy különböző parancsfájl típusok megjelenik, minden konfigurálható tooa másik parancsprogramot.

![A parancsfájlalapú összetevő tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*A parancsfájlalapú összetevő tulajdonságai*

### <a id="frame_based_trim_modify_clip_list"></a>Egy parancsprogram összetevő hello klip listájának módosítása
Mielőtt azt újra írhatna hello cliplist xml munkafolyamat indítása során létrehozott, szükség lesz toohave hozzáférés toohello cliplist xml tulajdonságot és tartalmát. Ehhez hasonló azt is megteheti:

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![A naplózott bejövő klip listája](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*A naplózott bejövő klip listája*

Először igazolnia kell a módon toodetermine vége: amikor tootrim azt szeretnénk, mely pontról hello videó. toomake Ez kényelmes toohello kevésbé-technikai felhasználói hello munkafolyamat közzététele hello graph két tulajdonságok toohello gyökérmappájában. toodo, kattintson jobb gombbal a Tervező felületére hello és "Tulajdonság hozzáadása" Válasszon:

* Első tulajdonság: "ClippingTimeStart" típusú: "IDŐKÓD"
* A második tulajdonság: "ClippingTimeEnd" típusú: "IDŐKÓD"

![Hozzáadása tulajdonsághoz párbeszédpanel Kivágás kezdési ideje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Hozzáadása tulajdonsághoz párbeszédpanel Kivágás kezdési ideje*

![Közzétett a munkafolyamat legfelső szintű idő tulajdonságai Kivágás](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Közzétett a munkafolyamat legfelső szintű idő tulajdonságai Kivágás*

Adja meg mindkét tulajdonságok tooa megfelelő értéket:

![Kivágási, kezdő és záró tulajdonságok hello konfigurálása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Kivágási, kezdő és záró tulajdonságok hello konfigurálása*

Most a belül a parancsfájl azt férhetnek hozzá mindkét tulajdonságok ehhez hasonló:

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Kezdő és záró a Kivágás tartalmazó napló ablak](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Kezdő és záró a Kivágás tartalmazó napló ablak*

Most elemezni hello időkód karakterláncok formába kényelmesebb toouse, egy egyszerű reguláris kifejezés használatával:

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Napló ablakban az elemzett időkód kimenete](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Napló ablakban az elemzett időkód kimenete*

Az információ az elvégzendő azt mostantól módosíthatja a hello cliplist xml tooreflect hello kezdő, és befejezési idejének hello hello film keret pontos Kivágás szükséges.

![A parancsfájlok kód tooadd vágás elemei](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*A parancsfájlok kód tooadd vágás elemei*

Ez normál karakterlánc fájlkezelési műveleteket keresztül végezhető el. hello eredményül kapott módosított klip lista xml írása vissza toohello clipListXML tulajdonság hello munkafolyamat legfelső szintű hello "setProperty" metódussal. hello napló ablakban egy másik teszt futtatása után szeretné megjelenítése velünk a következő hello:

![Naplózási a klip listájában hello](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Naplózási a klip listájában hello*

Hajtsa végre a vizsgálat toosee, hogyan lettek hello video- és adatfolyamok levágva. Módon teheti meg egynél több vizsgálat hello levágási pontok különböző értékekkel, láthatja, hogy a rendszer nem figyelembe kell venni azonban! hello ennek az oka, hogy hello Tervező hello Azure futásidejű eltérően, does nem felülbírálás hello cliplist xml minden futtatás. Ez azt jelenti, hogy csak hello állított hello első alkalommal bejövő és kimenő adatforgalma pontok, akkor hello xml tootransform, az összes hello más időpontokban, a őr záradék (Ha (clipListXML.indexOf ("<trim>") == -1)) megakadályozza, hogy a hello munkafolyamat vágás hozzáadása Ha már létezik egy elem.

toomake a munkafolyamat kényelmes tootest helyileg, a Microsoft ajánlott hozzáadása néhány house-karbantartási kódot, amely megvizsgálja, ha a vágás elem már található. Ha igen, azt eltávolítja azt hello xml hello új értékekkel módosításával a folytatás előtt. Ahelyett, hogy az egyszerű karakterlánc-feldolgozás, akkor valószínűleg biztonságosabb toodo elemzési modell ez valós xml-objektumon keresztül.

Ahhoz azonban vehessen fel ilyen kód, kell hello importálási utasításokat számos indítsa el a parancsfájl először tooadd:

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

Ezt követően hozzá lehessen adni hello tisztítás kód szükséges:

    //for local testing: delete any pre-existing trim elements from hello clip list xml by parsing hello xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
     //delete hello trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

Ez a kód fölött, amellyel jelenleg felvenni hello vágás elemek toohello cliplist xml hello pont kerül.

Ezen a ponton azt futtathatja és módosíthatja a munkafolyamat, mint azt szeretnénk, ha valaha is alkalmazott hello módosítások mellett idő.    

### <a id="frame_based_trim_clippingenabled_prop"></a>Egy ClippingEnabled kényelmi tulajdonság hozzáadása
Előfordulhat, hogy nem mindig kívánt levágási toohappen, most Befejezés ki a munkafolyamat hozzáadásával egy kényelmes logikai jelző, amely azt jelzi-e azt szeretnénk, hogy tooenable díszítésre / kivágást.

Ahogy előtt, tegye közzé a munkafolyamat "ClippingEnabled" nevű új tulajdonság toohello gyökér "Logikai" típusúnak.

![Közzétett egy tulajdonság Kivágás engedélyezése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Közzétett egy tulajdonság Kivágás engedélyezése*

Az alábbi egyszerű őr záradék hello azt ellenőrizze, hogy a tisztítás szükség, és döntse el, hogy a klip listáját használja, így kell-e a módosítása, illetve nem toobe.

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <a id="code"></a>Teljes kód
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from hello clip list xml by parsing hello xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
    //delete hello trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements toocliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }


## <a name="also-see"></a>Lásd még:
[Prémium szintű kódolás az Azure Media Services bemutatása](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Hogyan prémium szintű Azure Media Services kódolási tooUse](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Az Azure Media Services kódolási igény tartalom](media-services-encode-asset.md#media-encoder-premium-workflow)

[A Media Encoder Premium munkafolyamat formátumai és kodekei](media-services-premium-workflow-encoder-formats.md)

[Minta munkafolyamat-fájlok](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer eszköz](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
