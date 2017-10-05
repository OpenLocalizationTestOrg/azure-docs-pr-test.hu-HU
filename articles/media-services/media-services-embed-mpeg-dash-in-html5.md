---
title: "Egy MPEG-DASH adaptív adatfolyam-továbbítási videó beágyazása DASH.js HTML5 alkalmazás |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan egy MPEG-DASH adaptív adatfolyam-videó beágyazása DASH.js HTML5 alkalmazás."
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
ms.openlocfilehash: 27ce6325773ba1f9fd9cd9ab9e07ea9f5e2488ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a><span data-ttu-id="113d5-103">Egy MPEG-DASH adaptív adatfolyam-továbbítási videó beágyazása DASH.js HTML5 alkalmazás</span><span class="sxs-lookup"><span data-stu-id="113d5-103">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>
## <a name="overview"></a><span data-ttu-id="113d5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="113d5-104">Overview</span></span>
<span data-ttu-id="113d5-105">MPEG-DASH egy ISO szabvány a videotartalom, azok számára kiváló minőségű, adaptív video-kimeneti adatfolyam továbbítására kívánnak jelentős előnyt kínál, amelyek adaptív streameléshez.</span><span class="sxs-lookup"><span data-stu-id="113d5-105">MPEG-DASH is an ISO standard for the adaptive streaming of video content, which offers significant benefits for those who wish to deliver high-quality, adaptive video streaming output.</span></span> <span data-ttu-id="113d5-106">A MPEG-DASH a video-adatfolyamot automatikusan eldobja alacsonyabb definition a hálózati sávszélességből válik.</span><span class="sxs-lookup"><span data-stu-id="113d5-106">With MPEG-DASH, the video stream will automatically drop to a lower definition when the network becomes congested.</span></span> <span data-ttu-id="113d5-107">Ez csökkenti a "felfüggesztett" videó jelent meg, amíg a Windows Media player számára, hogy (más néven pufferelés) a következő néhány másodpercig letölti megjelenítő valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="113d5-107">This reduces the likelihood of the viewer seeing a "paused" video while the player downloads the next few seconds to play (aka buffering).</span></span> <span data-ttu-id="113d5-108">Csökkenti a hálózati torlódás, mert a videólejátszó pedig visszatérhet egy magasabb minőségi adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="113d5-108">As network congestion reduces, the video player will in turn return to a higher quality stream.</span></span> <span data-ttu-id="113d5-109">Így meghatározhatja, hogy a szükséges sávszélesség igazítja videó gyorsabb idejét is eredményezi.</span><span class="sxs-lookup"><span data-stu-id="113d5-109">This ability to adapt the bandwidth required also results in a faster start time for video.</span></span> <span data-ttu-id="113d5-110">Amely azt jelenti, hogy az első néhány másodperc is lehet lejátszott egy gyors-letöltési alacsonyabb minőségi szegmens majd eltárolva a lépés akár egy magasabb minőségi egyszer elegendő tartalom pufferben.</span><span class="sxs-lookup"><span data-stu-id="113d5-110">That means that the first few seconds can be played in a fast-to-download lower quality segment and then step up to a higher quality once sufficient content has been buffered.</span></span>

<span data-ttu-id="113d5-111">Dash.js egy nyílt forráskódú MPEG-DASH videólejátszó JavaScript nyelven írt.</span><span class="sxs-lookup"><span data-stu-id="113d5-111">Dash.js is an open source MPEG-DASH video player written in JavaScript.</span></span> <span data-ttu-id="113d5-112">A cél, hogy adjon meg egy robusztus, platformfüggetlen player, amelyek a videó lejátszása igénylő alkalmazások szabadon felhasználható.</span><span class="sxs-lookup"><span data-stu-id="113d5-112">Its goal is to provide a robust, cross-platform player that can be freely reused in applications that require video playback.</span></span> <span data-ttu-id="113d5-113">A böngészők támogat a W3C Media forrás Extensions (MSE) Chrome, Microsoft Edge és (a más böngészőkkel MSE támogatásához a leképezés jelezte) IE11 MPEG-DASH lejátszás biztosít.</span><span class="sxs-lookup"><span data-stu-id="113d5-113">It provides MPEG-DASH playback in any browser that supports the W3C Media Source Extensions (MSE), today that is Chrome, Microsoft Edge and IE11 (other browsers have indicated their intent to support MSE).</span></span> <span data-ttu-id="113d5-114">Js DASH.js kapcsolatos további információkért tekintse meg a GitHub-tárházban dash.js.</span><span class="sxs-lookup"><span data-stu-id="113d5-114">For more information about DASH.js, js see the GitHub dash.js repository.</span></span>

## <a name="creating-a-browser-based-streaming-video-player"></a><span data-ttu-id="113d5-115">Webböngésző-alapú adatfolyam videólejátszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="113d5-115">Creating a browser-based streaming video player</span></span>
<span data-ttu-id="113d5-116">Létrehozása, amely megjeleníti a videólejátszó várt egyszerű weblapot szabályozza ilyen a play, szüneteltetése, Visszatekerés stb., szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="113d5-116">To create a simple web page that displays a video player with the expected controls such a play, pause, rewind etc., you will need to:</span></span>

1. <span data-ttu-id="113d5-117">Hozzon létre egy HTML-weblap</span><span class="sxs-lookup"><span data-stu-id="113d5-117">Create an HTML page</span></span>
2. <span data-ttu-id="113d5-118">A videó címke hozzáadása</span><span class="sxs-lookup"><span data-stu-id="113d5-118">Add the video tag</span></span>
3. <span data-ttu-id="113d5-119">Adja hozzá a dash.js player</span><span class="sxs-lookup"><span data-stu-id="113d5-119">Add the dash.js player</span></span>
4. <span data-ttu-id="113d5-120">A Windows Media player inicializálása</span><span class="sxs-lookup"><span data-stu-id="113d5-120">Initialize the player</span></span>
5. <span data-ttu-id="113d5-121">Adja hozzá az egyes CSS-stílus</span><span class="sxs-lookup"><span data-stu-id="113d5-121">Add some CSS style</span></span>
6. <span data-ttu-id="113d5-122">Az eredmények megtekintése a böngészőben, amely megvalósítja az MSE</span><span class="sxs-lookup"><span data-stu-id="113d5-122">View the results in a browser that implements MSE</span></span>

<span data-ttu-id="113d5-123">A Windows Media player inicializálása a csupán néhány sornyi JavaScript-kód olyan hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="113d5-123">Initializing the player can be completed in just a handful of lines of JavaScript code.</span></span> <span data-ttu-id="113d5-124">Dash.js használ, hogy valóban, amely egyszerű MPEG-DASH videó beágyazása a böngészőalapú alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="113d5-124">Using dash.js, it really is that simple to embed MPEG-DASH video in your browser based applications.</span></span>

## <a name="creating-the-html-page"></a><span data-ttu-id="113d5-125">A HTML-weblap létrehozása</span><span class="sxs-lookup"><span data-stu-id="113d5-125">Creating the HTML Page</span></span>
<span data-ttu-id="113d5-126">Az első lépés az, hogy hozzon létre egy szabványos HTML tartalmazó oldalra a **videó** elem, a fájl basicPlayer.html, a következő példa szemlélteti:</span><span class="sxs-lookup"><span data-stu-id="113d5-126">The first step is to create a standard HTML page containing the **video** element, save this file as basicPlayer.html, as the following example illustrates:</span></span>

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-the-dashjs-player"></a><span data-ttu-id="113d5-127">A DASH.js Player hozzáadása</span><span class="sxs-lookup"><span data-stu-id="113d5-127">Adding the DASH.js Player</span></span>
<span data-ttu-id="113d5-128">A dash.js hivatkozás végrehajtása hozzáadni az alkalmazáshoz, lesz szüksége az 1.0-ás kiadásba dash.js projekt dash.all.js a fájl megértéséhez.</span><span class="sxs-lookup"><span data-stu-id="113d5-128">To add the dash.js reference implementation to the application, you’ll need to grab the dash.all.js file from the 1.0 release of dash.js project.</span></span> <span data-ttu-id="113d5-129">Ez az alkalmazás a JavaScript mappában mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="113d5-129">This should be saved in the JavaScript folder of your application.</span></span> <span data-ttu-id="113d5-130">Ezt a fájlt egy olyan kényelmi fájl együtt az összes szükséges dash.js kód lekéri egyetlen fájlba.</span><span class="sxs-lookup"><span data-stu-id="113d5-130">This file is a convenience file that pulls together all the necessary dash.js code into a single file.</span></span> <span data-ttu-id="113d5-131">Ha tekintse meg a dash.js tárház körül, fogja az egyes fájlok található, tesztelje a kódot, és még sok más, de ha az összes szeretné használata dash.js, akkor a dash.all.js fájl mire van szüksége.</span><span class="sxs-lookup"><span data-stu-id="113d5-131">If you have a look around the dash.js repository, you will find the individual files, test code and much more, but if all you want to do is use dash.js, then the dash.all.js file is what you need.</span></span>

<span data-ttu-id="113d5-132">Az alkalmazások a dash.js player hozzáadásához basicPlayer.html központi szakaszába parancsfájl címke hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="113d5-132">To add the dash.js player to your applications, add a script tag to the head section of basicPlayer.html:</span></span>

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


<span data-ttu-id="113d5-133">Ezután hozzon létre egy működnek, mint a Windows Media player inicializálni az oldal betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="113d5-133">Next, create a function to initialize the player when the page loads.</span></span> <span data-ttu-id="113d5-134">Vegye fel a következő parancsfájlt a sort, amelyben dash.all.js betöltése után:</span><span class="sxs-lookup"><span data-stu-id="113d5-134">Add the following script after the line in which you load dash.all.js:</span></span>

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

<span data-ttu-id="113d5-135">Ez a funkció először létrehoz egy DashContext.</span><span class="sxs-lookup"><span data-stu-id="113d5-135">This function first creates a DashContext.</span></span> <span data-ttu-id="113d5-136">Ez az alkalmazás egy adott futásidejű környezet konfigurálására szolgál.</span><span class="sxs-lookup"><span data-stu-id="113d5-136">This is used to configure the application for a specific runtime environment.</span></span> <span data-ttu-id="113d5-137">Technikai szempontból meghatározza az osztályokat a függőségi injektálási keretrendszer kell használni, amikor az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="113d5-137">From a technical point of view, it defines the classes that the dependency injection framework should use when constructing the application.</span></span> <span data-ttu-id="113d5-138">A legtöbb esetben Dash.di.DashContext fogja használni.</span><span class="sxs-lookup"><span data-stu-id="113d5-138">In most cases, you will use Dash.di.DashContext.</span></span>

<span data-ttu-id="113d5-139">A következő példányosítható dash.js keretrendszer, Media Player elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="113d5-139">Next, instantiate the primary class of the dash.js framework, MediaPlayer.</span></span> <span data-ttu-id="113d5-140">Ez az osztály tartalmazza a core módszerek, például a szükséges lejátszása és szüneteltetése, a kapcsolat a videó elemmel valamint kezelésére is lejátszható a videó bemutatja a Media bemutató leírása (MPD) fájl értelmezése.</span><span class="sxs-lookup"><span data-stu-id="113d5-140">This class contains the core methods needed such as play and pause, manages the relationship with the video element and also manages the interpretation of the Media Presentation Description (MPD) file which describes the video to be played.</span></span>

<span data-ttu-id="113d5-141">A Media Player osztály startup() funkciót, hogy a Windows Media player készen áll a videó lejátszása nevezik.</span><span class="sxs-lookup"><span data-stu-id="113d5-141">The startup() function of the MediaPlayer class is called to ensure that the player is ready to play video.</span></span> <span data-ttu-id="113d5-142">Többek között a funkció biztosítja, hogy a szükséges osztályokat (a környezet által meghatározott) lett betöltve.</span><span class="sxs-lookup"><span data-stu-id="113d5-142">Amongst other things this function ensures that all the necessary classes (as defined by the context) have been loaded.</span></span> <span data-ttu-id="113d5-143">Ha készen áll a Windows Media player, csatolhat a videó elem a attachView() függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="113d5-143">Once the player is ready, you can attach the video element to it using the attachView() function.</span></span> <span data-ttu-id="113d5-144">Ez lehetővé teszi a Media Player a video-adatfolyamot behelyezése elem, és szükség szerint lejátszás is szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="113d5-144">This enables the MediaPlayer to inject the video stream into the element and also control playback as necessary.</span></span>

<span data-ttu-id="113d5-145">Így az tudni fogja, kapcsolatos a videó lejátszása várható a Media Player átadása a MPD fájl URL-CÍMÉT. Az imént létrehozott setupVideo() függvényt kell hajtható végre, ha az oldal teljes mértékben be van töltve.</span><span class="sxs-lookup"><span data-stu-id="113d5-145">Pass the URL of the MPD file to the MediaPlayer so that it knows about the video it is expected to play.The setupVideo() function just created will need to be executed once the page has fully loaded.</span></span> <span data-ttu-id="113d5-146">Ezt úgy teheti meg a törzs elem betöltésre.</span><span class="sxs-lookup"><span data-stu-id="113d5-146">Do this by using the onload event of the body element.</span></span> <span data-ttu-id="113d5-147">Módosítsa a <body> elemet:</span><span class="sxs-lookup"><span data-stu-id="113d5-147">Change your <body> element to:</span></span>

    <body onload="setupVideo()">

<span data-ttu-id="113d5-148">Végezetül be a videó elem stíluslappal méretének.</span><span class="sxs-lookup"><span data-stu-id="113d5-148">Finally, set the size of the video element using CSS.</span></span> <span data-ttu-id="113d5-149">Adaptív adatfolyam-továbbítási környezetben ez különösen fontos, mert a videó lejátszását méretét, a lejátszás alkalmazkodik a változó hálózati körülmények módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="113d5-149">In an adaptive streaming environment, this is especially important because the size of the video being played may change as playback adapts to changing network conditions.</span></span> <span data-ttu-id="113d5-150">Ez egyszerű bemutató a egyszerűen kényszerítése videó elemként 80 %-át a rendelkezésre álló böngészőablakban adja hozzá a következő CSS a head részt a lap:</span><span class="sxs-lookup"><span data-stu-id="113d5-150">In this simple demo simply force the video element to be 80% of the available browser window by adding the following CSS to the head section of the page:</span></span>

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a><span data-ttu-id="113d5-151">A videó lejátszása</span><span class="sxs-lookup"><span data-stu-id="113d5-151">Playing a Video</span></span>
<span data-ttu-id="113d5-152">A videó lejátszása, irányítsa a böngészőt a basicPlayback.html fájlt, és a videólejátszó jelenik meg a lejátszás gombra.</span><span class="sxs-lookup"><span data-stu-id="113d5-152">To play a video, point your browser at the basicPlayback.html file and click play on the video player displayed.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="113d5-153">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="113d5-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="113d5-154">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="113d5-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="113d5-155">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="113d5-155">See Also</span></span>
[<span data-ttu-id="113d5-156">Videolejátszó alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="113d5-156">Develop video player applications</span></span>](media-services-develop-video-players.md)

[<span data-ttu-id="113d5-157">GitHub-tárházban dash.js</span><span class="sxs-lookup"><span data-stu-id="113d5-157">GitHub dash.js repository</span></span>](https://github.com/Dash-Industry-Forum/dash.js) 

