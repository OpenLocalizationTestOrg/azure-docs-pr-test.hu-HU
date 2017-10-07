---
title: "hello ügyféloldalon aaaInserting ads |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooinsert ads a hello ügyféloldali."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: e6eab4aa92918ad734db8ac3a4e7818d02ed7fe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="inserting-ads-on-hello-client-side"></a>Hello ügyféloldalon ads beszúrása
Ez a témakör ismerteti tooinsert hello ügyféloldalon ads különböző típusú.

Az élő adatfolyam-továbbítási videók lezárt feliratok és az ad támogatásával kapcsolatos további információkért lásd: [támogatott kódolt feliratok és Ad beszúrási szabványok](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Az Azure Media Player jelenleg nem támogatja a hirdetések.
> 
> 

## <a id="insert_ads_into_media"></a>A Media Ads beszúrása
Az Azure Media Services támogatást nyújt a Windows Media Platform hello keresztül ad beszúrási: lejátszó-Keretrendszerekhez. Ad-támogatással rendelkező lejátszó-keretrendszerekhez Windows 8, a Silverlight, a Windows Phone 8 és az iOS-eszközök érhetők el. Minden egyes player keretrendszer minta kódot tartalmaz, amely bemutatja, hogyan tooimplement a lejátszóalkalmazás. Nincsenek media: listájába szúrhatók ads három különböző típusú.

* **Lineáris** – teljes keret hirdetések hello fő videó felfüggesztése.
* **Nem lineáris** – jelennek meg hello fő videó lejátszása hirdetések átmeneti területre, általában embléma vagy egyéb statikus kép elhelyezett hello player belül.
* **Kiegészítő** – hello player kívül megjelenített ads.

ADs hello fő videó idősorán bármikor helyezhető. Meg kell állapítani, hogy hello player amikor tooplay hello ad, és amely ads tooplay. Ebben az esetben a szabványos XML alapú fájlok készletből: videó Ad szolgáltatás sablon (VAST), a digitális videót több Ad lista (VMAP), a Media absztrakt alkalmazás-előkészítés sablon (OSZLOPOS) és a digitális videót Player Ad felület Definition (VPAID). NAGY fájlok adja meg, milyen ads toodisplay. VMAP fájlok adja meg, mikor tooplay különböző hirdetések és HATALMAS XML kódot tartalmaz. A fájlok OSZLOPOS egy másik módja toosequence ads is tartalmazó túlnyomó XML. VPAID fájlok meghatározása hello videólejátszó és hello ad vagy ad-kiszolgáló közötti illesztőfelületet szolgáltasson.

Minden egyes player keretrendszer eltérő módon működik, és minden egyes saját témakör tárgyalja. Ez a témakör ismerteti, hello használt alapvető mechanizmusok tooinsert ads. Videólejátszó alkalmazások ads kérhet egy ad-kiszolgáló. hello ad kiszolgáló többféle módon válaszolhat:

* Térjen vissza a túlnyomó fájl
* Térjen vissza a VMAP fájlt (a beágyazott VAST)
* Egy OSZLOPOS fájlt (a beágyazott VAST) visszaadása
* Térjen vissza a VPAID ads túlnyomó fájl

### <a name="using-a-video-ad-service-template-vast-file"></a>Videó Ad szolgáltatás sablon (VAST) fájl használatával
Egy túlnyomó fájlt határozza meg, milyen ad vagy ads toodisplay. hello következő XML-kódja egy túlnyomó fájlt egy lineáris ad példát:

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>

hello lineáris ad hello le <**lineáris**> elemet. Hello ad hello időtartama határoz meg, nyomon követés, átkattintással, követési kattintson, és számos **MediaFile** elemek. Nyomkövetési események hello belül vannak megadva <**TrackingEvents**> elemet, és teszi lehetővé az Active server tootrack hello ad megtekintése közben előforduló különféle események. Ebben az esetben hello start, középpont, befejeződött, és bontsa ki a események nyomon követi. hello start esemény következik be, amikor hello ad megjelenik. hello középpont esemény következik be, legalább 50 % hello ad idősor már megtekintett. hello befejeződésének eseményét hello ad toohello end futtatásakor következik be. hello felhasználói bővíti hello videólejátszó toofull képernyő hello kibontott esemény következik be. Clickthroughs vannak megadva, a <**Átkattintós**> elemen belül egy <**VideoClicks**> elemet, és egy URI tooa erőforrás toodisplay határozza meg, ha hello felhasználó hello ad kattint. ClickTracking van megadva a <**ClickTracking**> elemet, is hello <**VideoClicks**> elem és hello player toorequest követési erőforrás határozza meg, amikor hello felhasználó kattint a hello ad.hello <**MediaFile**> elemek adjon meg egy adott kódolását, az ad információt. Ha egynél több <**MediaFile**> elem, hello videólejátszó választhat hello legjobb kódolásának hello platform. 

Lineáris ads megjeleníthető a megadott sorrendben. toodo, adja hozzá a további <Ad> elemek toohello VAST fájlt, és adja meg a hello rendelés hello feladatütemezési attribútum használatával. hello a következő példa ezt mutatja be:

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>

Nem lineáris ads megadott egy <Creative> elemet is. a következő példa azt mutatja meg hello egy <Creative> elem az lineáris ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>


hello <**NonLinearAds**> elem tartalmazhat egy vagy több <**NonLinear**> elemek, amelyek leírhatja lineáris ad. hello <**NonLinear**> elem hello erőforrás hello lineáris ad határozza meg. hello erőforrás lehet egy <**StaticResouce**>, <**IFrameResource**>, vagy egy <**HTMLResouce**>. <**StaticResource**> nem HTML erőforrás ismerteti, és határozza meg a creativeType attribútum, amely meghatározza, hogyan hello erőforrás jelenik meg:

Kép/gif, a kép/jpeg, a lemezkép vagy png – hello erőforrás megjelenik egy HTML <**img**> címke.

Alkalmazás/x-javascript – hello erőforrás megjelenik egy HTML <**parancsfájl**> címke.

Alkalmazás/x-shockwave-flash – hello erőforrás Flash player jelenik meg.

**IFrameResource** egy HTML-erőforrás IFRAME megjeleníthető ismerteti. **HTMLResource** szúrhatók be egy weblap, HTML-kódja egy adat ismerteti. **TrackingEvents** adja meg a nyomkövetési események és URI toorequest hello hello esemény bekövetkezésekor. Ez a minta hello acceptInvitation és összecsukása események követi. További információ a hello **NonLinearAds** elem és a gyermekek, lásd: IAB.NET/VAST. Vegye figyelembe, hogy hello **TrackingEvents** elem a helyen belüli hello **NonLinearAds** hello helyett elem **NonLinear** elemet.

Kiegészítő ads meghatározott egy <CompanionAds> elemet. Hello <CompanionAds> elem tartalmazhat egy vagy több <Companion> elemek. Minden egyes <Companion> elem egy kiegészítő ad ismerteti, és tartalmazhat egy <StaticResource>, <IFrameResource>, vagy <HTMLResource> amely meg van határozva a hello azonos módon, egy lineáris az ad-ben. A túlnyomó is tartalmazhatnak, több kiegészítő ads, és hello lejátszóalkalmazás hello legmegfelelőbb ad toodisplay választhat. VAST kapcsolatos további információkért lásd: [túlnyomó 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Egy digitális videót több Ad-lista (VMAP) fájl használatával
Egy VMAP fájl lehetővé teszi a toospecify ad oldaltörések esetén, mennyi ideig egyes szünetek, hány ads belül szünet jeleníthető meg, és ads típusú lehet megszakítás alatt jelenik meg. a következő egy példa VMAP fájl, amely meghatározza egy egyetlen ad break hello:

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Egy VMAP fájl kezdődik egy <VMAP> elem, amely tartalmazza egy vagy több <AdBreak> elemek, egy ad break meghatározása. Minden ad break határozza meg, egy break típusát, a sortörés azonosítója és a idő eltolódását. hello breakType attribútum meghatározza során hello break lejátszható ad hello típusát: lineáris, nem lineáris, vagy megjelenítése. Megjelenítési ads tooVAST kiegészítő ads hozzárendelését. Egynél több ad-típus (szóközök nélkül) vesszővel elválasztott listában adható meg. hello breakID pedig hello ad azonosítója, amelyet a nem kötelező. hello timeOffset határozza meg, amikor hello ad üzenetnek kell megjelennie. Ez a következő módokon hello egyikében adható meg:

1. Idő óó: pp: vagy hh:mm:ss.mmm formátumban, ahol .mmm az ezredmásodperc. Ez az attribútum értékének hello hello idejét hello videó ütemterv toohello elejére hello ad break hello kezdetén határozza meg.
2. Százalékos – n % formátumban, ahol n az hello ad lejátszás előtt hello videó ütemterv tooplay hello százaléka
3. Kezdő és záró – Megadja, hogy egy ad üzenetnek kell megjelennie, előtt vagy után hello videó meg lett jelenítve.
4. Helyezze – hello ad oldaltörések hello időzítése ismeretlen, például az élő adatfolyam-esetén adja meg a ad oldaltörések hello sorrendjét. minden ad break hello sorrendjének hello #n formátumban, ahol n az 1 vagy nagyobb egész szám van megadva. 1 fiókoldala hello ad lejátszani hello első alkalommal, 2 fiókoldala hello ad lejátszani hello második lehetőség, és így tovább.

Hello belül <**AdBreak**> elem nem lehet az egyik <**AdSource**> elemet. hello <**AdSource**> elem tartalmazza-e a következő attribútumok hello:

1. Azonosító – Megadja hello ad forrás azonosítója
2. allowMultipleAds – egy logikai érték, amely meghatározza, hogy több ads is megjelenjen-e hello ad break során
3. followRedirects – egy választható logikai érték, amely meghatározza, hogy ha hello videólejátszó kell tiszteletben átirányítja a felhasználókat egy ad választ belül

hello <**AdSource**> elem biztosít hello player egy beágyazott ad választ, vagy hivatkozás tooan ad választ. A következő elemek hello egyik tartalmazhat:

* <VASTAdData>azt jelzi, hogy túlnyomó ad választ hello VMAP fájlban van beágyazva.
* <AdTagURI>URI, amely egy ad választ hivatkozik másik rendszerről
* <CustomAdData>– egy tetszőleges karakterlánc, adott respresents nem túlnyomó választ

Ebben a példában egy beágyazott ad választ meg van adva egy <VASTAdData> elem, amely tartalmazza a túlnyomó ad választ. További információ hello más elemek: [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

hello <**AdBreak**> elem is tartalmazhat egy <**TrackingEvents**> elemet. hello <**TrackingEvents**> elem lehetővé teszi a tootrack hello kezdeti vagy záró egy ad break vagy e hiba történ hello ad sortörés. hello <**TrackingEvents**> elem tartalmaz egy vagy több <**követési**> elemek, amelyek mindegyike egy nyomkövetési esemény és egy követési URI adja meg. hello lehetséges nyomkövetési események a következők:

1. breakStart – nyomon követi az ad-break hello kezdete
2. breakEnd – egy ad break hello befejezésének nyomon követése
3. Hiba – hello ad break bekövetkezett hiba követi nyomon.

a következő példa hello jeleníti meg, amely meghatározza a nyomkövetési események VMAP fájl

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

További információ a hello <**TrackingEvents**> elem és a gyermekek, lásd: http://iab.org/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Egy Media absztrakt alkalmazás-előkészítés sablonfájl (OSZLOPOS) használatával
Egy OSZLOPOS fájl lehetővé teszi toospecify eseményindítókat, amelyek meghatározzák, ha az ad jelenik meg. hello az alábbiakban látható egy példa egy előtti összegző ad, egy közepes összegző ad és a utáni összegző ad eseményindítók tartalmazó OSZLOPOS fájlt.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' hello trigger for hello next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>



Egy OSZLOPOS fájl kezdődik egy **OSZLOPOS** elem, amely egy **eseményindítók** elemet. Hello <triggers> egy vagy több elemet tartalmaz **eseményindító** határozza meg, ha az ad lejátszani. 

Hello **eseményindító** elem tartalmazza-e egy **startConditions** elem, amelynek adja meg, ha az ad tooplay kell kezdődnie. Hello **startConditions** egy vagy több elemet tartalmaz <condition> elemek. Ha minden <condition> tootrue eseményindító kezdeményezett, vagy visszavonták, attól függően, hogy kiértékeli hello <condition> magában foglal egy **startConditions** vagy **endConditions** elem kulcsattribútumokkal. Ha több <condition> elem is szerepel, azokat egy implicit vagy számít, a feltétel kiértékelése tootrue hatására hello eseményindító tooinitiate. <condition>elemek egymásba ágyazható. Ha gyermek <condition> elemek előre be van állítva, akkor számít egy implicit és, minden feltétel ki kell értékelnie, hogy a hello eseményindító tooinitiate tootrue. Hello <condition> elem tartalmazza-e a következő attribútum is definiál hello feltétel hello: 

1. **típus** – hello adja meg az állapot, esemény vagy tulajdonság
2. **név** – hello kiértékelés során használt hello tulajdonság vagy esemény toobe neve
3. **érték** – hello érték, amely egy tulajdonság értékelni
4. **operátor** – hello művelet toouse kiértékelése közben: EQ (egyenlő), a NEQ (nem egyenlő), a GTR (nagyobb), a GEQ (nagyobb vagy egyenlő), a LT (kisebb), LEQ (kisebb vagy egyenlő), MOD ELEMET (Maradékos osztás)

**endConditions** is tartalmazhat, <condition> elemek. Ha a feltétel tootrue hello eseményindító-e reset.hello <trigger> elem is tartalmaz egy <sources> elem, amely tartalmazza egy vagy több <source> elemek. Hello <source> elemek hello URI toohello ad választ és ad válasz hello típusának megadása. Ebben a példában egy URI tooa túlnyomó választ kap. 

    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>


### <a name="using-video-player-ad-interface-definition-vpaid"></a>Videó Player Ad felületdefiníció (VPAID) használatával
VPAID az API-k engedélyezésének végrehajtható ad egységek toocommunicate videó lejátszóval. Ez lehetővé teszi a magas interaktív ad lép. hello felhasználók beavatkozhatnak-hello ad-val, és hello ad válaszolhassanak tooactions hello megjelenítő venni. Például az ad gombok, amelyek lehetővé teszik a hello felhasználói tooview hello ad hosszabb verzióját vagy további információk megjelenítésére. hello videólejátszó támogatnia kell a hello VPAID API, és hello végrehajtható ad meg kell valósítania az hello API. Ha egy player kéri le az Active server hello kiszolgáló az ad jelenhetnek meg, amely tartalmaz egy VPAID ad túlnyomó választ.

Egy végrehajtható ad Adobe Flash™ vagy a webböngészőben végrehajtható JavaScript futásidejű környezetben kell végrehajtani kód jön létre. Ha egy ad-kiszolgáló egy VPAID ad tartalmazó túlnyomó választ ad vissza, hello hello hello apiFramework attribútumának értéke <MediaFile> elemnek kell lennie a "VPAID". Ez az attribútum határozza meg, hogy tartalmazott hello ad VPAID végrehajtható ad. hello típusú attribútum értéke toohello MIME-típusát hello végrehajtható, például az "application/x-shockwave-flash" vagy "application/x-javascript". hello következő XML-részletet látható hello <MediaFile> VPAID végrehajtható ad tartalmazó túlnyomó választ elemét. 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI tooexecutable ad -->
       </MediaFile>
    </MediaFiles>


Egy végrehajtható ad hello inicializálhatók <AdParameters> hello elemet <Linear> vagy <NonLinear> elemek túlnyomó választ. További információ a hello <AdParameters> elem, lásd: [túlnyomó 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Hello VPAID API kapcsolatos további információkért lásd: [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>A Windows vagy Windows Phone 8 Player Ad-támogatással rendelkező megvalósítása
Microsoft Media Platform hello: Player keretrendszer Windows 8 és Windows Phone 8-alkalmazásokra, amelyek bemutatják, hogyan videólejátszó alkalmazások használatával tooimplement hello keretrendszer gyűjteményét tartalmazza. Letöltheti a hello Player keretrendszer és hello mintákat az [Player keretrendszer Windows 8 és Windows Phone 8](https://playerframework.codeplex.com).

Hello Microsoft.PlayerFramework.Xaml.Samples megoldás megnyitásakor látni fogja a mappák hello projekten belül több. hello hirdetési mappa hello minta kód megfelelő toocreating egy ad-támogatással rendelkező videólejátszó tartalmazza. Belső hello hirdetési mappa egy XAML/cs fájljainak száma minden mely megjelenítése hogyan tooinsert ads eltérő módon. a következő lista hello ismerteti:

* AdPodPage.xaml bemutatja, hogyan toodisplay ad pod.
* AdSchedulingPage.xaml bemutatja hogyan tooschedule ads.
* FreeWheelPage.xaml bemutatja, hogyan toouse hello FreeWheel beépülő modul tooschedule ads.
* MastPage.xaml bemutatja hogyan tooschedule ads OSZLOPOS fájllal.
* ProgrammaticAdPage.xaml bemutatja, hogyan tooprogrammatically ütemezni a videóban ads.
* ScheduleClipPage.xaml bemutatja hogyan tooschedule egy ad túlnyomó fájl nélkül.
* VastLinearCompanionPage.xaml bemutatja hogyan egy lineáris tooinsert és kiegészítő ad.
* VastNonLinearPage.xaml bemutatja hogyan tooinsert nem lineáris ad.
* VmapPage.xaml bemutatja hogyan toospecify ads VMAP fájllal.

Ezeket a mintákat mindegyikének használ hello Media Player hello player keretrendszer határozzák meg. A legtöbb minták beépülő modulok, amelyek különböző ad választ formátumban támogatásához használja. hello ProgrammaticAdPage minta programokon keresztül kommunikál a Media Player példánya.

### <a name="adpodpage-sample"></a>AdPodPage minta
Ezt a mintát használ hello AdSchedulerPlugin toodefine amikor toodisplay ad. Ebben a példában egy közepes összegző hirdetmény ütemezett toobe lejátszott 5 másodpercen belül. hello ad pod (ads toodisplay sorrendben csoportja) egy ad-kiszolgáló által visszaadott túlnyomó fájlban van megadva. hello megadva hello URI toohello túlnyomó fájl <RemoteAdSource> elemet.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Hello AdSchedulerPlugin kapcsolatos további információkért lásd: [reklám a hello Player keretrendszer a Windows 8 és Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Ez a minta hello AdSchedulerPlugin is használ. Három ads, egy előtti összegző ad, egy közepes összegző ad és a utáni összegző ad ütemezés. hello URI toohello VAST, az egyes ad van megadva egy <RemoteAdSource> elemet.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


### <a name="freewheelpage"></a>FreeWheelPage
Ez a minta hello FreeWheelPlugin, amely meghatározza a forrásattribútumot, amely meghatározza a pontok tooa SmartXML fájlt ad tartalmat, valamint ad ütemezési adatait megadó URI használja.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a>MastPage
Ezt a mintát használja, amely lehetővé teszi egy OSZLOPOS fájl toouse MastSchedulerPlugin hello. hello forrásattribútum hello OSZLOPOS fájl hello helyét adja meg.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a>ProgrammaticAdPage
Ez a minta hello MediaPlayer programokon keresztül kommunikál. hello ProgrammaticAdPage.xaml fájl hello Media Player példányosítja:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

hello ProgrammaticAdPage.xaml.cs fájl veszi fel egy TimelineMarker toospecify, amikor az ad üzenetnek kell megjelennie, majd a hello MarkerReached esemény, amely betölti a RemoteAdSource URI tooa túlnyomó fájl megadása, és majd játszik kezelőtársítást ad hozzá, létrehoz egy AdHandlerPlugin hello ad.

    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

### <a name="scheduleclippage"></a>ScheduleClipPage
Ez a minta egy .wmv-fájlt, amely tartalmazza a hello ad megadásával hello AdSchedulerPlugin tooschedule egy közepes összegző ad használja.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
Ez a minta bemutatja, hogyan toouse hello AdSchedulerPlugin tooschedule egy közepes összegző lineáris ad egy kiegészítő ad-val. Hello <RemoteAdSource> elem hello túlnyomó fájl hello helyét adja meg.

    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
A példa egy lineáris és egy nem lineáris ad a hello AdSchedulerPlugin tooschedule használja. hello túlnyomó fájl helye meg van adva hello <RemoteAdSource> elemet.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vmappage"></a>VMAPPage
Ezt a mintát használ hello VmapSchedulerPlugin tooschedule ads VMAP fájl használatával. hello URI toohello VMAP fájl van megadva a hello forrásattribútumának hello <VmapSchedulerPlugin> elemet.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a>IOS rendszerű Ad-támogatással rendelkező videó Player megvalósítása
Microsoft Media Platform hello: Player keretrendszer az iOS-alkalmazásokra, amelyek bemutatják, hogyan videólejátszó alkalmazások használatával tooimplement hello keretrendszer gyűjteményét tartalmazza. Letöltheti a hello Player keretrendszer és hello mintákat az [Azure Media Player keretrendszer](https://github.com/Azure/azure-media-player-framework). hello github-oldalon tartalmaz egy hivatkozást tooa hello player keretrendszer és a bevezetés toohello player minta kiegészítő tudnivalókat tartalmazó Wiki: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>VMAP hirdetések ütemezése
a következő példa azt mutatja meg hogyan hello tooschedule ads VMAP fájl használatával.

    // How tooschedule an Ad using VMAP.
    //First download hello VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using hello downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a>VAST hirdetések ütemezése
hello alábbi példa azt mutatja be hogyan tooschedule egy késői kötés túlnyomó ad.

    //Example:3 How tooschedule a late binding VAST ad.
    // set hello start time for hello ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify hello URI of hello VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL tooVAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

   hello alábbi példa azt mutatja be hogyan tooschedule egy korai kötés túlnyomó ad.
Példa: 4 ütemezés egy korai kötés túlnyomó ad //Download hello VAST fájlt, ha (! [ framework.adResolver downloadManifest: & jegyzék withURL: [által igényelt NSURL URLWithString: @"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[self logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

hello alábbi példa azt mutatja be hogyan tooinsert egy ad nyers Kivágás szerkesztése (RCE) használatával

    //Example:1 How toouse RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

hello következő példa bemutatja, hogyan tooschedule ad pod.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL toocontent
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI tooad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

a következő példa azt mutatja meg hogyan hello tooschedule a nem kapcsolódó közepes összegző ad. A nem kapcsolódó ad csak lejátszása után függetlenül bármilyen pozicionálási hello viewer hajt végre.

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL toocontent
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

a következő példa azt mutatja meg hogyan hello tooschedule egy állandóságát közepes összegző ad. A kapcsolódó ad fog megjelenni minden alkalommal, amikor hello megadott hello videó idősoron pont elérésekor.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI tooad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


hello alábbi példa azt mutatja be hogyan tooschedule egy utáni összegző ad.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

hello alábbi példa azt mutatja be hogyan tooschedule egy előtti összegző ad.

    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

a következő minta hello bemutatja, hogyan tooschedule közepes összegző átfedő ad.

    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Videolejátszó alkalmazások fejlesztése](media-services-develop-video-players.md)

