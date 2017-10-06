---
title: "egy MPEG-DASH adaptív adatfolyam-videót a HTML5 alkalmazást a DASH.js aaaEmbedding |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooembed egy MPEG-DASH adaptív adatfolyam-videót a DASH.js HTML5 alkalmazás."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: a73713d20f95262654532b94576ae9669d829354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Egy MPEG-DASH adaptív adatfolyam-továbbítási videó beágyazása DASH.js HTML5 alkalmazás
## <a name="overview"></a>Áttekintés
MPEG-DASH egy ISO szabvány hello adaptív adatfolyam-videó tartalom, akik toodeliver kiváló minőségű, adaptív videoadatfolyam kimeneti kívánja jelentős előnyt kínál, amelyek a rendszer. A MPEG-DASH hello video-adatfolyamot lesz automatikusan dobja el tooa alacsonyabb definition hello hálózati sávszélességből válik. Ez csökkenti a "felfüggesztett" videó jelent meg, amíg a hello player letölti hello mellett néhány másodperc tooplay (más néven pufferelés) hello viewer hello valószínűségét. Mivel csökkenti a hálózati torlódás, hello videólejátszó pedig tooa magasabb minőségi adatfolyam ad vissza. Ez képességét tooadapt hello sávszélességre van szüksége a videó gyorsabb idejét is eredményezi. Hogy azt jelenti, hogy az első néhány másodpercig hello fast-letöltési alacsonyabb minőségi szegmens le lehessen játszani és az majd tooa jobb minőségű után elegendő tartalom eltárolva a pufferben.

Dash.js egy nyílt forráskódú MPEG-DASH videólejátszó JavaScript nyelven írt. A célja az tooprovide egy robusztus, platformfüggetlen player, amelyek a videó lejátszása igénylő alkalmazások szabadon felhasználható. A böngészőben, amely támogatja a hello W3C Media forrás Extensions (MSE), vagyis ma Chrome, Microsoft Edge és IE11 (a más böngészőkkel jelezték a leképezési toosupport MSE) MPEG-DASH lejátszás biztosít. Js DASH.js kapcsolatos további információkért lásd: hello GitHub dash.js tárházba.

## <a name="creating-a-browser-based-streaming-video-player"></a>Webböngésző-alapú adatfolyam videólejátszó létrehozása
megjelenik egy videólejátszó várt hello egyszerű weblapot toocreate szabályozza, ilyen a play, szüneteltetése, Visszatekerés stb., szüksége lesz:

1. Hozzon létre egy HTML-weblap
2. Hello videó címke hozzáadása
3. Hello dash.js player hozzáadása
4. Hello player inicializálása
5. Adja hozzá az egyes CSS-stílus
6. Egy böngészőben, amely megvalósítja az MSE hello eredmények megtekintése

Inicializálási hello player a csupán néhány sornyi JavaScript-kód olyan hajthatók végre. Dash.js használ, hogy valóban a böngészőalapú alkalmazások egyszerű tooembed MPEG-DASH-videoeszközök.

## <a name="creating-hello-html-page"></a>HTML-weblap hello létrehozása
első lépés hello hello tartalmazó toocreate normál HTML-lapot **videó** elem, mentse a fájlt basicPlayer.html, mint például a következő hello mutatja be:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-hello-dashjs-player"></a>Hozzáadását hello DASH.js Player
tooadd hello dash.js megvalósítási toohello referenciaalkalmazás, toograb hello dash.all.js fájl hello 1.0 kiadásáról dash.js projekt lesz szüksége. Ez az alkalmazás hello JavaScript mappában menteni kell. Ezt a fájlt egy olyan kényelmi fájl együtt az összes hello szükséges dash.js kód lekéri egyetlen fájlba. Ha egy hely hello dash.js tárház körül, fogja hello található fájlokat, tesztelje a kódot, és még sok más, de ha az összes kívánt toodo dash.js használja, akkor hello dash.all.js fájl az alábbiakra lesz szüksége.

tooadd tooyour hello dash.js lejátszóalkalmazások, hozzáadása egy parancsfájl címke toohello központi szakasza basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Ezután hozzon létre egy függvény tooinitialize hello player hello oldal betöltésekor. Adja hozzá a hello parancsfájl hello sor, amelyben dash.all.js betöltése után a következő:

    <script>
    // setup hello video element and attach it toohello Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Ez a funkció először létrehoz egy DashContext. Ez az egy adott futtatókörnyezetben használt tooconfigure hello alkalmazás. Technikai szempontból meghatározza hello osztállyal, amelynek függőségi injektálási keretrendszer hello használnia kell, amikor hello alkalmazást hozhat létre. A legtöbb esetben Dash.di.DashContext fogja használni.

A következő példányosítható hello elsődleges osztály hello dash.js keretrendszer, Media Player. Ez az osztály hello core módszerek, például a szükséges lejátszásához és szüneteltetése, hello videó elem hello kapcsolattal valamint kezelésére is hello Media bemutató leírása (MPD) fájlt, amely ismerteti a lejátszott hello videó toobe hello értelmezésének tartalmaz.

hello startup() funkciója hello MediaPlayer osztály elnevezése tooensure adott hello player kész tooplay videó. Többek között a funkció biztosítja, hogy minden szükséges hello-osztály (hello környezet által meghatározott) lett betöltve. Ha készen áll a hello player, csatolhat hello videó elem tooit hello attachView() függvény használatával. Ez lehetővé teszi, hogy hello Media Player tooinject hello video-adatfolyamot hello elem be, és is szabályozhatja a lejátszás szükség szerint.

Továbbítsa hello MPD fájl toohello Media Player hello URL-CÍMÉT, hogy azt ismer videó hello várható tooplay.hello setupVideo() függvény létrehozott kell toobe végrehajtása után hello lap teljes mértékben be van töltve. Ezt úgy teheti meg hello betöltésre hello törzs elem. Módosítsa a <body> elemet:

    <body onload="setupVideo()">

Végezetül be hello videó elem stíluslappal hello méretének. Adaptív adatfolyam-továbbítási környezetben ez különösen fontos, mert a videó lejátszását hello hello méretét, a lejátszás alkalmazkodik toochanging hálózati körülmények módosíthatja. Ez egyszerű bemutató a egyszerűen kényszerítése hello videó elem toobe 80 %-át hello elérhető böngészőablakban adja hozzá a következő CSS toohello központi szakasz hello lap hello:

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a>A videó lejátszása
tooplay videó irányítsa a böngészőt hello basicPlayback.html fájlt, és a hello videólejátszó jelenik meg a lejátszás gombra.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Videolejátszó alkalmazások fejlesztése](media-services-develop-video-players.md)

[GitHub-tárházban dash.js](https://github.com/Dash-Industry-Forum/dash.js) 

