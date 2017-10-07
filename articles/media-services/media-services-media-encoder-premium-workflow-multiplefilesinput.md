---
title: "aaaMultiple bemeneti fájlok és a prémium szintű kódolás - Azure összetevő tulajdonságai |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan toouse setRuntimeProperties toouse több bemeneti fájl, és adja át egyéni adatok toohello Media Encoder prémium munkafolyamat media processzor."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: e14d10fbf9669e0b88e5ba1c519f1ba5e0bafdd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>A prémium szintű kódolás több bemeneti fájlok és összetevő tulajdonságai használja
## <a name="overview"></a>Áttekintés
Forgatókönyv, ahol szükség lehet a toocustomize összetevő tulajdonságai, adja meg a klip lista XML-tartalom, vagy több bemeneti fájlok küldése hello kötegazonosítójú feladat elküldése **Media Encoder prémium munkafolyamat** media processzor. Néhány példa:

* A videó szöveg felirataként és hello szöveges érték (például a jelenlegi dátum hello) beállítása az egyes bemeneti videó futásidőben.
* Testreszabás hello klip lista XML (toospecify egy vagy több forrás-fájlok, vagy anélkül díszítésre stb.).
* Egy embléma felirataként hello bemeneti videóhoz, miközben hello videó.
* Több hang nyelvi kódolást.

toolet hello **Media Encoder prémium munkafolyamat** tudja, hogy néhány tulajdonság hello munkafolyamat módosítani hello feladat létrehozásakor, vagy több bemeneti fájlok küldése telepítette, toouse tartalmazó konfigurációs karakterlánc  **setRuntimeProperties** és/vagy **transcodeSource**. Ez a témakör azt ismerteti, hogyan toouse őket.

## <a name="configuration-string-syntax"></a>Konfigurációs karakterlánc-formátum:
hello konfigurációs karakterlánc tooset a kódolási feladat hello használja az XML-dokumentum, amely a következőképpen néz ki:

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

hello következő hello C# kóddal, amely hello XML konfigurációs fájlból olvassa be, frissítse hello jobb videó fájlnév, és továbbítja azokat a feladatok toohello feladat:

```c#
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass tooit hello name of hello 
// processor toouse for hello specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with hello encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify hello input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset toocontain hello results of hello job. 
// This output is specified as AssetCreationOptions.None, which 
// means hello output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a>Összetevő tulajdonságai testreszabása
### <a name="property-with-a-simple-value"></a>Egyszerű értéke
Bizonyos esetekben hasznos toocustomize együtt hello munkafolyamat fájl, amelyet hajtja végre a Media Encoder prémium munkafolyamat toobe összetevő tulajdonság.

Tegyük fel, a munkafolyamat átfedések szöveg a videók, és a következő hello szöveg (például a jelenlegi dátum hello) toobe set kellene futásidőben. Ehhez hello szöveg toobe hello text tulajdonságához hello átfedő összetevő hello új értéket állítja be a kódolási feladat hello küldésével. A mechanizmus toochange egyéb tulajdonságok használhatók az összetevő hello munkafolyamatban (például hello pozícióját vagy hello felirat színét, hello sávszélességű hello AVC kódoló, stb.).

**setRuntimeProperties** használt toooverride hello munkafolyamat hello összetevői tulajdonság értéke.

Példa:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text tooImage Converter/text" value="Today is Friday hello 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a>Az XML-értéke tulajdonság
tooset vár egy XML-érték tulajdonság használatával beágyazására `<![CDATA[ and ]]>`.

Példa:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> Győződjön meg arról, hogy nem tooput kocsivissza csak után térjen vissza `<![CDATA[`.

### <a name="propertypath-value"></a>a propertyPath érték
Az előző példákban hello hello propertyPath lett "/ Media File bemeneti/filename" vagy "/ inactiveTimeout" vagy "clipListXml".
Ez az általában hello hello összetevő neve, majd hello tulajdonság hello nevét. hello elérési utat is van több vagy kevesebb, mint "/ primarySourceFile" (mert hello tulajdonság hello gyökerében hello munkafolyamat) vagy "/ videó feldolgozási/kép átfedő/átlátszatlanság" (mert átfedő hello csoportban).    

toocheck hello elérési útját és a tulajdonság nevét, használja hello akciógombra kattinthat, amely közvetlenül mellett minden egyes tulajdonsága. A művelet gombra kattintva, és válassza ki **szerkesztése**. Ez azt mutatja majd, hello tényleges név: hello tulajdonság, és azonnal felette, hello névtér.

![A művelet/szerkesztése](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Tulajdonság](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Több bemeneti fájl
Egyes feladatokat, hogy elküldését toohello **Media Encoder prémium munkafolyamat** két eszközök igényel:

* hello először még egy *munkafolyamat eszköz* , amely a munkafolyamat-fájlt tartalmaz. Hello segítségével megtervezheti a munkafolyamat-fájlok [munkafolyamat-Tervező](media-services-workflow-designer.md).
* hello második van egy *Media eszköz* hello media (oka) t, amelyet az tooencode tartalmazza.

Ha több adathordozó fájlok toohello küldjük **Media Encoder prémium munkafolyamat** kódoló, hello a következő korlátozások vonatkoznak:

* Fájlok kell lennie minden hello media hello azonos *Media eszköz*. Több adathordozó eszközök használata nem támogatott.
* Meg kell adnia hello elsődleges fájl a Media eszköz (ideális esetben ez a hello kódoló hello fő videofájl kapcsolatba tooprocess).
* Hello tartalmazó toopass szükséges konfigurációs adatok **setRuntimeProperties** és/vagy **transcodeSource** elem toohello processzor.
  * **setRuntimeProperties** használt toooverride hello filename tulajdonságban vagy a hello összetevők hello munkafolyamat egy másik tulajdonság.
  * **transcodeSource** használt toospecify hello klip lista XML-tartalom.

Hello munkafolyamat kapcsolatok:

* Ha egy vagy több adathordozó fájl bemeneti összetevőket használnak, és tervezze meg toouse **setRuntimeProperties** toospecify hello fájl nevét, majd hello elsődleges fájl összetevő PIN-kód toothem csatlakoztatásának mellőzése. Győződjön meg arról, hogy nincs-e hello elsődleges fájl objektum és hello Media fájl Input(s) közötti kapcsolat.
* Ha jobban szeret toouse klip lista XML és egy forrás-adathordozó összetevőt, majd csatlakozhat mindkét együtt.

![Nincs elsődleges forrásfájl tooMedia fájl bemeneti közötti kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Nincs kapcsolat hello elsődleges fájl tooMedia fájl bemeneti elemét/elemeit az setRuntimeProperties tooset hello filename tulajdonságban használatakor.*

![Klip lista XML tooClip forráslista közötti kapcsolat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Forrás klip lista XML tooMedia csatlakozhat, és transcodeSource használja.*

### <a name="clip-list-xml-customization"></a>Lista XML testreszabási levágása
Megadhat hello klip lista XML hello munkafolyamat futásidőben használatával **transcodeSource** hello konfigurációban karakterlánc XML. Ehhez a hello klip lista XML PIN-kód csatlakoztatott toobe toohello forrás-adathordozó összetevő hello munkafolyamatban.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Ha azt szeretné, hogy ez a tulajdonság tooname hello kimeneti fájlok "Kifejezések" segítségével toospecify /primarySourceFile toouse, akkor javasoljuk, hogy átadja hello klip lista XML-tulajdonságként *után* hello /primarySourceFile tulajdonság, tooavoid hello rendelkező klip lista felülbírálnia hello /primarySourceFile beállítást.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

A további keret pontos tisztítás:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-hello-video"></a>1. példa: Átfedő fölött videó hello kép

### <a name="presentation"></a>Bemutató
Tekintse meg egy példát kívánt toooverlay egy embléma hello bemeneti videóhoz közben hello videó. Ebben a példában hello bemeneti videó "Microsoft_HoloLens_Possibilities_816p24.mp4" nevű és hello embléma neve "logo.png". Végre kell hajtania az alábbi lépésekkel hello:

* Hozzon létre egy munkafolyamat-eszközt a hello munkafolyamat fájlt (lásd a következő példa hello).
* Hozzon létre egy adathordozó eszköz, amely két fájlt tartalmaz: MyInputVideo.mp4, az elsődleges fájl- és MyLogo.png hello.
* Egy feladat toohello Media Encoder prémium munkafolyamat media processzor, a fenti hello bemeneti eszközök küldése, és adja meg a következő konfigurációs karakterlánc hello.

A konfiguráció:

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Hello a fenti példában a hello videofájl hello neve küldött toohello Media fájl bemeneti összetevő és hello primarySourceFile tulajdonság. hello embléma fájl neve hello tooanother Media fájl bemeneti csatlakoztatott toohello grafikus átfedő összetevő által küldött.

> [!NOTE]
> hello videó fájlnév küldött toohello primarySourceFile tulajdonság. Ennek az oka hello toouse hello munkafolyamat készítéséhez hello megfelelő kimeneti fájl nevét kifejezésekkel, például a tulajdonság értéke.

### <a name="step-by-step-workflow-creation"></a>A munkafolyamat részletes létrehozása
Az alábbiakban hello lépéseket toocreate olyan munkafolyamatot, amely két fájlt fogadja bemeneti adatként: videó és lemezképet. Akkor lesz átfedő hello kép videó hello felett.

Nyissa meg **munkafolyamat-Tervező** válassza **fájl** > **új munkaterület** > **átkódolására tervezetének**.

Új munkafolyamat hello három elemeit tartalmazza:

* Elsődleges forrásfájl
* XML klip listázása
* Kimeneti fájl vagy eszköz  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Új kódolási munkafolyamat*

Rendelés tooaccept hello bemeneti adathordozófájlba első lépésként adja hozzá az adathordozó fájl bemeneti összetevőt. tooadd összetevő toohello munkafolyamat keressen hello tárház keresési mezőbe, majd szükséges hello bejegyzés húzza hello Tervező ablak.

Ezután adja hozzá a munkafolyamat tervezéséhez használt hello videofájl toobe. toodo tehát hello háttér ablaktábla munkafolyamat-tervezőben kattintson, és keresse meg hello elsődleges forrásfájl tulajdonság a hello tulajdonság jobb oldali panelen. Hello mappa ikonra, és válassza ki a megfelelő videofájl hello.

![Elsődleges fájl forrás](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Elsődleges fájl forrás*

Adja meg a következő hello videofájl hello Media fájl bemeneti összetevő.   

![A médiafájl bemeneti forrása](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*A médiafájl bemeneti forrása*

Amint ez történik, hello Media fájl bemeneti összetevő hello fájl vizsgálata, és a kimeneti PIN-kódok tooreflect hello fájl, amely megvizsgálja az feltöltése.

hello következő lépés egy "Videó adatok típusa Frissítőjének" toospecify hello szín terület tooRec.709 tooadd. Adja hozzá a "videó konverter" tooData elrendezés/típusának beállított = síkbeli konfigurálható. A művelet konvertálja hello video-adatfolyamot tooa formátum hello átfedő összetevő forrásaként lehet tenni.

![videó típusú frissítési adatok és konverter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videó adatok típusa Frissítőjének és konverter*

![Típusának konfigurálható síkbeli =](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Elrendezés típus síkbeli konfigurálható:*

A következő videó átfedő összetevőt, és csatlakozzon a hello (tömörítetlen) video PIN-kód toohello (tömörítetlen) video PIN-kód hello media fájl bemeneti.

Adja hozzá egy másik Media fájl bemeneti (tooload hello embléma fájl), kattintson a ezt az összetevőt, és nevezze át túl "Media fájl bemeneti embléma", és válassza a hello fájltulajdonság lemezkép (.png fájl például). Csatlakozás hello átfedő hello tömörítetlen lemezkép tömörített kép PIN-kód toohello PIN kódját.

![Összetevő- és képfájlok forrásfájlt átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Összetevő- és képfájlok forrásfájlt átfedő*

Ha azt szeretné, hogy toomodify hello pozíciója a hello videó hello embléma (például érdemes tooposition 10 százalékát hello felső ki a bal oldali hello videó sarkában), törölje a jelet hello "Manuális bevitel" jelölőnégyzetet. Ez végezhető el, mert a média fájl bemeneti tooprovide hello embléma fájl toohello átfedő összetevőt használ.

![Átfedő pozíciója](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Átfedő pozíciója*

tooencode hello video-adatfolyamot tooH.264, vegye fel a hello AVC Videókódoló és AAC kódoló összetevők toohello Tervező felületére. Csatlakozás hello PIN-kód.
Hello AAC kódoló beállítását, és hang formátum konverziós/készlet kiválasztása: 2.0 (L, R).

![Hang- és kódolók](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Hang- és kódolók*

Ezután adja hozzá a hello **ISO Mpeg-4 Multiplexer** és **kimeneti fájl** összetevők és csatlakoztassa hello PIN-kód.

![MP4 multiplexer és a kimeneti fájl](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexer és a kimeneti fájl*

Meg kell tooset hello hello kimeneti fájl nevét. Kattintson a hello **kimeneti fájl** hello fájl összetevő és a Szerkesztés hello kifejezése:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Kimeneti fájlnév](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Kimeneti fájlnév*

Hello munkafolyamat futtatása helyileg toocheck, hogy fut-e megfelelően.

Miután a Befejezés után futtatható Azure Media Services.

Először készítsen egy eszköz két fájlokat az Azure Media Services: hello videofájl és hello embléma. Ehhez hello .NET vagy a REST API használatával. Is ehhez hello Azure-portál használatával vagy [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Az oktatóanyag bemutatja, hogyan AMSE toomanage eszközöket. Két módon tooadd fájlok tooan eszköz van:

* Hozzon létre egy helyi mappát, másolja hello két fájlokat, és áthúzása hello mappa toohello **eszköz** fülre.
* Eszközként hello videofájl feltöltése hello eszköz információk megjelenítése, nyissa meg toohello fájlok fülre, és töltse fel egy további fájlt (embléma).

> [!NOTE]
> Győződjön meg arról, hogy tooset elsődleges fájl hello eszközt (hello fő videofájl).

![Az AMSE eszköz fájlok](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Az AMSE eszköz fájlok*

Válassza ki a hello eszközt, és válassza ki a tooencode a prémium szintű kódolás. Töltse fel a hello munkafolyamat, és válassza ki azt.

Hello toohello gomb toopass adatfeldolgozó kattintson, és adja hozzá a következő XML-tooset hello futásidejű tulajdonságok hello:

![Prémium szintű kódolás az AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Prémium szintű kódolás az AMSE*

Majd illessze be a következő XML-adatok hello. Hello videofájl toospecify hello neve hello Media fájl bemeneti és primarySourceFile is szükség van. Adja meg hello hello embléma hello fájlnév túl.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*

Ha hello .NET SDK toocreate használja, és hello feladat futtatása az XML-adatok toobe átadott hello konfigurációs karakterláncban.

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

Hello feladat befejezése után a hello MP4-fájlt a hello kimeneti eszköz megjeleníti hello átfedő!

![A videó hello átfedő](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*A videó hello átfedő*

Letöltheti a minta-munkafolyamat hello [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>2. példa: Több nyelvi hang kódolás

Példa kódolási workfkow érhető el több hang nyelvi [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Ez a mappa tartalmaz egy minta-munkafolyamat egy MXF fájl tooa multi MP4 fájlok eszköz több zeneszámok használt tooencode lesz.

Ez a munkafolyamat azt feltételezi, hogy hello MXF fájl tartalmaz egy hang követése; hello további zeneszámok (WAV vagy MP4...) külön hang fájlokként kell átadni.

tooencode, kövesse az alábbi lépéseket:

* Hozzon létre egy Media Services eszközt a hello MXF fájl- és hello hang fájlok (0 too18 hang).
* Győződjön meg arról, hogy hello MXF fájl elsődleges fájl be van állítva.
* Hozzon létre egy feladat- és a prémium szintű munkafolyamat kódolás processzor hello segítségével. A megadott (MultiMP4-1080p-19audio-v1.workflow) hello munkafolyamat használja.
* (Ha használja az Azure Media Services Explorer, használata hello "XML-adatok toohello munkafolyamata átadni" gombot), adja át hello setruntime.xml adatok toohello feladat.
  * Frissítse a hello XML-adatok toospecify hello megfelelő fájl nevét és nyelvek címkék.
  * hello munkafolyamat nevű hang 1 tooAudio 18 hang részből áll.
  * RFC5646 nyelvcímkének hello támogatott.

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* hello kódolású eszköz több nyelv zeneszámok fogja tartalmazni, és ezek nyomon követi az Azure Media Player választható kell lennie.

## <a name="see-also"></a>Lásd még:
* [Prémium szintű kódolás az Azure Media Services bemutatása](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Hogyan prémium szintű Azure Media Services kódolási toouse](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Az Azure Media Services kódolási igény tartalom](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Media Encoder prémium szintű munkafolyamat-formátumok és kodekek](media-services-premium-workflow-encoder-formats.md)
* [Minta munkafolyamat-fájlok](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [Azure Media Services Explorer eszköz](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
