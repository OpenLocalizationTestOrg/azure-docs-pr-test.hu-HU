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
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a><span data-ttu-id="2a818-103">Egy MPEG-DASH adaptív adatfolyam-továbbítási videó beágyazása DASH.js HTML5 alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2a818-103">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>
## <a name="overview"></a><span data-ttu-id="2a818-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2a818-104">Overview</span></span>
<span data-ttu-id="2a818-105">MPEG-DASH egy ISO szabvány hello adaptív adatfolyam-videó tartalom, akik toodeliver kiváló minőségű, adaptív videoadatfolyam kimeneti kívánja jelentős előnyt kínál, amelyek a rendszer.</span><span class="sxs-lookup"><span data-stu-id="2a818-105">MPEG-DASH is an ISO standard for hello adaptive streaming of video content, which offers significant benefits for those who wish toodeliver high-quality, adaptive video streaming output.</span></span> <span data-ttu-id="2a818-106">A MPEG-DASH hello video-adatfolyamot lesz automatikusan dobja el tooa alacsonyabb definition hello hálózati sávszélességből válik.</span><span class="sxs-lookup"><span data-stu-id="2a818-106">With MPEG-DASH, hello video stream will automatically drop tooa lower definition when hello network becomes congested.</span></span> <span data-ttu-id="2a818-107">Ez csökkenti a "felfüggesztett" videó jelent meg, amíg a hello player letölti hello mellett néhány másodperc tooplay (más néven pufferelés) hello viewer hello valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="2a818-107">This reduces hello likelihood of hello viewer seeing a "paused" video while hello player downloads hello next few seconds tooplay (aka buffering).</span></span> <span data-ttu-id="2a818-108">Mivel csökkenti a hálózati torlódás, hello videólejátszó pedig tooa magasabb minőségi adatfolyam ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2a818-108">As network congestion reduces, hello video player will in turn return tooa higher quality stream.</span></span> <span data-ttu-id="2a818-109">Ez képességét tooadapt hello sávszélességre van szüksége a videó gyorsabb idejét is eredményezi.</span><span class="sxs-lookup"><span data-stu-id="2a818-109">This ability tooadapt hello bandwidth required also results in a faster start time for video.</span></span> <span data-ttu-id="2a818-110">Hogy azt jelenti, hogy az első néhány másodpercig hello fast-letöltési alacsonyabb minőségi szegmens le lehessen játszani és az majd tooa jobb minőségű után elegendő tartalom eltárolva a pufferben.</span><span class="sxs-lookup"><span data-stu-id="2a818-110">That means that hello first few seconds can be played in a fast-to-download lower quality segment and then step up tooa higher quality once sufficient content has been buffered.</span></span>

<span data-ttu-id="2a818-111">Dash.js egy nyílt forráskódú MPEG-DASH videólejátszó JavaScript nyelven írt.</span><span class="sxs-lookup"><span data-stu-id="2a818-111">Dash.js is an open source MPEG-DASH video player written in JavaScript.</span></span> <span data-ttu-id="2a818-112">A célja az tooprovide egy robusztus, platformfüggetlen player, amelyek a videó lejátszása igénylő alkalmazások szabadon felhasználható.</span><span class="sxs-lookup"><span data-stu-id="2a818-112">Its goal is tooprovide a robust, cross-platform player that can be freely reused in applications that require video playback.</span></span> <span data-ttu-id="2a818-113">A böngészőben, amely támogatja a hello W3C Media forrás Extensions (MSE), vagyis ma Chrome, Microsoft Edge és IE11 (a más böngészőkkel jelezték a leképezési toosupport MSE) MPEG-DASH lejátszás biztosít.</span><span class="sxs-lookup"><span data-stu-id="2a818-113">It provides MPEG-DASH playback in any browser that supports hello W3C Media Source Extensions (MSE), today that is Chrome, Microsoft Edge and IE11 (other browsers have indicated their intent toosupport MSE).</span></span> <span data-ttu-id="2a818-114">Js DASH.js kapcsolatos további információkért lásd: hello GitHub dash.js tárházba.</span><span class="sxs-lookup"><span data-stu-id="2a818-114">For more information about DASH.js, js see hello GitHub dash.js repository.</span></span>

## <a name="creating-a-browser-based-streaming-video-player"></a><span data-ttu-id="2a818-115">Webböngésző-alapú adatfolyam videólejátszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a818-115">Creating a browser-based streaming video player</span></span>
<span data-ttu-id="2a818-116">megjelenik egy videólejátszó várt hello egyszerű weblapot toocreate szabályozza, ilyen a play, szüneteltetése, Visszatekerés stb., szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="2a818-116">toocreate a simple web page that displays a video player with hello expected controls such a play, pause, rewind etc., you will need to:</span></span>

1. <span data-ttu-id="2a818-117">Hozzon létre egy HTML-weblap</span><span class="sxs-lookup"><span data-stu-id="2a818-117">Create an HTML page</span></span>
2. <span data-ttu-id="2a818-118">Hello videó címke hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a818-118">Add hello video tag</span></span>
3. <span data-ttu-id="2a818-119">Hello dash.js player hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2a818-119">Add hello dash.js player</span></span>
4. <span data-ttu-id="2a818-120">Hello player inicializálása</span><span class="sxs-lookup"><span data-stu-id="2a818-120">Initialize hello player</span></span>
5. <span data-ttu-id="2a818-121">Adja hozzá az egyes CSS-stílus</span><span class="sxs-lookup"><span data-stu-id="2a818-121">Add some CSS style</span></span>
6. <span data-ttu-id="2a818-122">Egy böngészőben, amely megvalósítja az MSE hello eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="2a818-122">View hello results in a browser that implements MSE</span></span>

<span data-ttu-id="2a818-123">Inicializálási hello player a csupán néhány sornyi JavaScript-kód olyan hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="2a818-123">Initializing hello player can be completed in just a handful of lines of JavaScript code.</span></span> <span data-ttu-id="2a818-124">Dash.js használ, hogy valóban a böngészőalapú alkalmazások egyszerű tooembed MPEG-DASH-videoeszközök.</span><span class="sxs-lookup"><span data-stu-id="2a818-124">Using dash.js, it really is that simple tooembed MPEG-DASH video in your browser based applications.</span></span>

## <a name="creating-hello-html-page"></a><span data-ttu-id="2a818-125">HTML-weblap hello létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a818-125">Creating hello HTML Page</span></span>
<span data-ttu-id="2a818-126">első lépés hello hello tartalmazó toocreate normál HTML-lapot **videó** elem, mentse a fájlt basicPlayer.html, mint például a következő hello mutatja be:</span><span class="sxs-lookup"><span data-stu-id="2a818-126">hello first step is toocreate a standard HTML page containing hello **video** element, save this file as basicPlayer.html, as hello following example illustrates:</span></span>

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-hello-dashjs-player"></a><span data-ttu-id="2a818-127">Hozzáadását hello DASH.js Player</span><span class="sxs-lookup"><span data-stu-id="2a818-127">Adding hello DASH.js Player</span></span>
<span data-ttu-id="2a818-128">tooadd hello dash.js megvalósítási toohello referenciaalkalmazás, toograb hello dash.all.js fájl hello 1.0 kiadásáról dash.js projekt lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="2a818-128">tooadd hello dash.js reference implementation toohello application, you’ll need toograb hello dash.all.js file from hello 1.0 release of dash.js project.</span></span> <span data-ttu-id="2a818-129">Ez az alkalmazás hello JavaScript mappában menteni kell.</span><span class="sxs-lookup"><span data-stu-id="2a818-129">This should be saved in hello JavaScript folder of your application.</span></span> <span data-ttu-id="2a818-130">Ezt a fájlt egy olyan kényelmi fájl együtt az összes hello szükséges dash.js kód lekéri egyetlen fájlba.</span><span class="sxs-lookup"><span data-stu-id="2a818-130">This file is a convenience file that pulls together all hello necessary dash.js code into a single file.</span></span> <span data-ttu-id="2a818-131">Ha egy hely hello dash.js tárház körül, fogja hello található fájlokat, tesztelje a kódot, és még sok más, de ha az összes kívánt toodo dash.js használja, akkor hello dash.all.js fájl az alábbiakra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="2a818-131">If you have a look around hello dash.js repository, you will find hello individual files, test code and much more, but if all you want toodo is use dash.js, then hello dash.all.js file is what you need.</span></span>

<span data-ttu-id="2a818-132">tooadd tooyour hello dash.js lejátszóalkalmazások, hozzáadása egy parancsfájl címke toohello központi szakasza basicPlayer.html:</span><span class="sxs-lookup"><span data-stu-id="2a818-132">tooadd hello dash.js player tooyour applications, add a script tag toohello head section of basicPlayer.html:</span></span>

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


<span data-ttu-id="2a818-133">Ezután hozzon létre egy függvény tooinitialize hello player hello oldal betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="2a818-133">Next, create a function tooinitialize hello player when hello page loads.</span></span> <span data-ttu-id="2a818-134">Adja hozzá a hello parancsfájl hello sor, amelyben dash.all.js betöltése után a következő:</span><span class="sxs-lookup"><span data-stu-id="2a818-134">Add hello following script after hello line in which you load dash.all.js:</span></span>

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

<span data-ttu-id="2a818-135">Ez a funkció először létrehoz egy DashContext.</span><span class="sxs-lookup"><span data-stu-id="2a818-135">This function first creates a DashContext.</span></span> <span data-ttu-id="2a818-136">Ez az egy adott futtatókörnyezetben használt tooconfigure hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2a818-136">This is used tooconfigure hello application for a specific runtime environment.</span></span> <span data-ttu-id="2a818-137">Technikai szempontból meghatározza hello osztállyal, amelynek függőségi injektálási keretrendszer hello használnia kell, amikor hello alkalmazást hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2a818-137">From a technical point of view, it defines hello classes that hello dependency injection framework should use when constructing hello application.</span></span> <span data-ttu-id="2a818-138">A legtöbb esetben Dash.di.DashContext fogja használni.</span><span class="sxs-lookup"><span data-stu-id="2a818-138">In most cases, you will use Dash.di.DashContext.</span></span>

<span data-ttu-id="2a818-139">A következő példányosítható hello elsődleges osztály hello dash.js keretrendszer, Media Player.</span><span class="sxs-lookup"><span data-stu-id="2a818-139">Next, instantiate hello primary class of hello dash.js framework, MediaPlayer.</span></span> <span data-ttu-id="2a818-140">Ez az osztály hello core módszerek, például a szükséges lejátszásához és szüneteltetése, hello videó elem hello kapcsolattal valamint kezelésére is hello Media bemutató leírása (MPD) fájlt, amely ismerteti a lejátszott hello videó toobe hello értelmezésének tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2a818-140">This class contains hello core methods needed such as play and pause, manages hello relationship with hello video element and also manages hello interpretation of hello Media Presentation Description (MPD) file which describes hello video toobe played.</span></span>

<span data-ttu-id="2a818-141">hello startup() funkciója hello MediaPlayer osztály elnevezése tooensure adott hello player kész tooplay videó.</span><span class="sxs-lookup"><span data-stu-id="2a818-141">hello startup() function of hello MediaPlayer class is called tooensure that hello player is ready tooplay video.</span></span> <span data-ttu-id="2a818-142">Többek között a funkció biztosítja, hogy minden szükséges hello-osztály (hello környezet által meghatározott) lett betöltve.</span><span class="sxs-lookup"><span data-stu-id="2a818-142">Amongst other things this function ensures that all hello necessary classes (as defined by hello context) have been loaded.</span></span> <span data-ttu-id="2a818-143">Ha készen áll a hello player, csatolhat hello videó elem tooit hello attachView() függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="2a818-143">Once hello player is ready, you can attach hello video element tooit using hello attachView() function.</span></span> <span data-ttu-id="2a818-144">Ez lehetővé teszi, hogy hello Media Player tooinject hello video-adatfolyamot hello elem be, és is szabályozhatja a lejátszás szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="2a818-144">This enables hello MediaPlayer tooinject hello video stream into hello element and also control playback as necessary.</span></span>

<span data-ttu-id="2a818-145">Továbbítsa hello MPD fájl toohello Media Player hello URL-CÍMÉT, hogy azt ismer videó hello várható tooplay.hello setupVideo() függvény létrehozott kell toobe végrehajtása után hello lap teljes mértékben be van töltve.</span><span class="sxs-lookup"><span data-stu-id="2a818-145">Pass hello URL of hello MPD file toohello MediaPlayer so that it knows about hello video it is expected tooplay.hello setupVideo() function just created will need toobe executed once hello page has fully loaded.</span></span> <span data-ttu-id="2a818-146">Ezt úgy teheti meg hello betöltésre hello törzs elem.</span><span class="sxs-lookup"><span data-stu-id="2a818-146">Do this by using hello onload event of hello body element.</span></span> <span data-ttu-id="2a818-147">Módosítsa a <body> elemet:</span><span class="sxs-lookup"><span data-stu-id="2a818-147">Change your <body> element to:</span></span>

    <body onload="setupVideo()">

<span data-ttu-id="2a818-148">Végezetül be hello videó elem stíluslappal hello méretének.</span><span class="sxs-lookup"><span data-stu-id="2a818-148">Finally, set hello size of hello video element using CSS.</span></span> <span data-ttu-id="2a818-149">Adaptív adatfolyam-továbbítási környezetben ez különösen fontos, mert a videó lejátszását hello hello méretét, a lejátszás alkalmazkodik toochanging hálózati körülmények módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2a818-149">In an adaptive streaming environment, this is especially important because hello size of hello video being played may change as playback adapts toochanging network conditions.</span></span> <span data-ttu-id="2a818-150">Ez egyszerű bemutató a egyszerűen kényszerítése hello videó elem toobe 80 %-át hello elérhető böngészőablakban adja hozzá a következő CSS toohello központi szakasz hello lap hello:</span><span class="sxs-lookup"><span data-stu-id="2a818-150">In this simple demo simply force hello video element toobe 80% of hello available browser window by adding hello following CSS toohello head section of hello page:</span></span>

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a><span data-ttu-id="2a818-151">A videó lejátszása</span><span class="sxs-lookup"><span data-stu-id="2a818-151">Playing a Video</span></span>
<span data-ttu-id="2a818-152">tooplay videó irányítsa a böngészőt hello basicPlayback.html fájlt, és a hello videólejátszó jelenik meg a lejátszás gombra.</span><span class="sxs-lookup"><span data-stu-id="2a818-152">tooplay a video, point your browser at hello basicPlayback.html file and click play on hello video player displayed.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="2a818-153">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="2a818-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2a818-154">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="2a818-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="2a818-155">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2a818-155">See Also</span></span>
[<span data-ttu-id="2a818-156">Videolejátszó alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="2a818-156">Develop video player applications</span></span>](media-services-develop-video-players.md)

[<span data-ttu-id="2a818-157">GitHub-tárházban dash.js</span><span class="sxs-lookup"><span data-stu-id="2a818-157">GitHub dash.js repository</span></span>](https://github.com/Dash-Industry-Forum/dash.js) 

