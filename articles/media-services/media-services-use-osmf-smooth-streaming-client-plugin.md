---
title: "aaaSmooth hello forrás-adathordozó nyílt keretrendszert Streaming beépülő modul"
description: "Ismerje meg, hogyan toouse hello Azure Media Services Smooth Streaming beépülő modul hello Adobe nyílt forrás Media keretrendszert."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6068151f-b6b0-4507-9346-f03416d3d572
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 3cf8e4679279344cf79c3f0e5b28f63adf88179d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-microsoft-smooth-streaming-plugin-for-hello-adobe-open-source-media-framework"></a><span data-ttu-id="519ef-103">Hogyan tooUse hello hello Adobe nyílt forrás Media keretrendszert a Microsoft Smooth Streaming beépülő modul</span><span class="sxs-lookup"><span data-stu-id="519ef-103">How tooUse hello Microsoft Smooth Streaming Plugin for hello Adobe Open Source Media Framework</span></span>
## <a name="overview"></a><span data-ttu-id="519ef-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="519ef-104">Overview</span></span>
<span data-ttu-id="519ef-105">hello Microsoft Smooth Streaming beépülő modul megnyitása forrás Media keretrendszer 2.0-s (SS a OSMF) bővíti OSMF hello alapértelmezett képességeit, és hozzáad a tartalom lejátszását toonew Microsoft Smooth Streaming, valamint a meglévő OSMF játékosok.</span><span class="sxs-lookup"><span data-stu-id="519ef-105">hello Microsoft Smooth Streaming plugin for Open Source Media Framework 2.0 (SS for OSMF) extends hello default capabilities of OSMF and adds Microsoft Smooth Streaming content playback toonew and existing OSMF players.</span></span> <span data-ttu-id="519ef-106">hello beépülő modul hozzáadja a lejátszás képességek tooStrobe Smooth Streaming Media lejátszás (SMP) is.</span><span class="sxs-lookup"><span data-stu-id="519ef-106">hello plugin also adds Smooth Streaming playback capabilities tooStrobe Media Playback (SMP).</span></span>

<span data-ttu-id="519ef-107">SS OSMF a beépülő modul két verziója tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="519ef-107">SS for OSMF includes two versions of plugin:</span></span>

* <span data-ttu-id="519ef-108">Statikus Smooth Streaming beépülő modul OSMF (.swc)</span><span class="sxs-lookup"><span data-stu-id="519ef-108">Static Smooth Streaming plugin for OSMF (.swc)</span></span>
* <span data-ttu-id="519ef-109">Dinamikus Smooth Streaming beépülő modul OSMF (.swf)</span><span class="sxs-lookup"><span data-stu-id="519ef-109">Dynamic Smooth Streaming plugin for OSMF (.swf)</span></span>

<span data-ttu-id="519ef-110">Jelen dokumentum céljából feltételezzük, hogy rendelkezik-e OSMF és OSMF általános ismeretét hello olvasó beépülő modulok. OSMF kapcsolatos további információkért tekintse meg hello dokumentáció hello [hivatalos OSMF webhely](http://osmf.org/).</span><span class="sxs-lookup"><span data-stu-id="519ef-110">This document assumes that hello reader has a general working knowledge of OSMF and OSMF plug-ins. For more information about OSMF, please see hello documentation on hello [official OSMF site](http://osmf.org/).</span></span>

### <a name="smooth-streaming-plugin-for-osmf-20"></a><span data-ttu-id="519ef-111">OSMF 2.0 Smooth Streaming beépülő modul</span><span class="sxs-lookup"><span data-stu-id="519ef-111">Smooth Streaming plugin for OSMF 2.0</span></span>
<span data-ttu-id="519ef-112">hello beépülő modul betöltése és igény szerinti Smooth Streaming tartalom lejátszását támogatja a következő funkciók hello:</span><span class="sxs-lookup"><span data-stu-id="519ef-112">hello plugin supports loading and playback of on-demand Smooth Streaming content with hello following features:</span></span>

* <span data-ttu-id="519ef-113">Igény szerinti Smooth Streaming lejátszás (Play, szüneteltetése, Seek, Stop)</span><span class="sxs-lookup"><span data-stu-id="519ef-113">On-demand Smooth Streaming playback (Play, Pause, Seek, Stop)</span></span>
* <span data-ttu-id="519ef-114">Élő Smooth Streaming lejátszás (lejátszás)</span><span class="sxs-lookup"><span data-stu-id="519ef-114">Live Smooth Streaming playback (Play)</span></span>
* <span data-ttu-id="519ef-115">Élő DVR funkciók (szünet, Seek, DVR lejátszás, nyissa meg a működés közbeni)</span><span class="sxs-lookup"><span data-stu-id="519ef-115">Live DVR functions (Pause, Seek, DVR Playback, Go-to-Live)</span></span>
* <span data-ttu-id="519ef-116">Videó kodekek - H.264 támogatása</span><span class="sxs-lookup"><span data-stu-id="519ef-116">Support for video codecs - H.264</span></span>
* <span data-ttu-id="519ef-117">Hang kodekek - AAC támogatása</span><span class="sxs-lookup"><span data-stu-id="519ef-117">Support for Audio codecs - AAC</span></span>
* <span data-ttu-id="519ef-118">Több hang nyelvi váltás OSMF beépített API-khoz</span><span class="sxs-lookup"><span data-stu-id="519ef-118">Multiple audio language switching with OSMF built-in APIs</span></span>
* <span data-ttu-id="519ef-119">Maximális lejátszás minőségi kijelölés OSMF beépített API-khoz</span><span class="sxs-lookup"><span data-stu-id="519ef-119">Max playback quality selection with OSMF built-in APIs</span></span>
* <span data-ttu-id="519ef-120">Oldalkocsi OSMF feliratok beépülő modul a feliratok bezárása</span><span class="sxs-lookup"><span data-stu-id="519ef-120">Sidecar closed captions with OSMF captions plugin</span></span>
* <span data-ttu-id="519ef-121">Az Adobe&reg; Flash&reg; Player 11.4 vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="519ef-121">Adobe&reg; Flash&reg; Player 11.4 or higher.</span></span>
* <span data-ttu-id="519ef-122">Ebben a verzióban csak a OSMF 2.0 támogatja.</span><span class="sxs-lookup"><span data-stu-id="519ef-122">This version only supports OSMF 2.0.</span></span>

## <a name="supported-features-and-known-issues"></a><span data-ttu-id="519ef-123">Támogatott szolgáltatások és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="519ef-123">Supported features and known issues</span></span>
<span data-ttu-id="519ef-124">Támogatott szolgáltatások, a nem támogatott funkciókat és az ismert problémák teljes listáját lásd túl[Ez a dokumentum](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span><span class="sxs-lookup"><span data-stu-id="519ef-124">For a full list of supported features, unsupported features and known issues, refer too[this document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span></span>

## <a name="loading-hello-plugin"></a><span data-ttu-id="519ef-125">Beépülő modul betöltése hello</span><span class="sxs-lookup"><span data-stu-id="519ef-125">Loading hello Plugin</span></span>
<span data-ttu-id="519ef-126">OSMF beépülő modulok tölthetők be statikusan (fordítás során) vagy dinamikusan (futásidőben).</span><span class="sxs-lookup"><span data-stu-id="519ef-126">OSMF plugins can be loaded statically (at compile time) or dynamically (at run-time).</span></span> <span data-ttu-id="519ef-127">hello Smooth Streaming beépülő modul OSMF le a dinamikus és statikus verziójával együtt.</span><span class="sxs-lookup"><span data-stu-id="519ef-127">hello Smooth Streaming plugin for OSMF download includes both dynamic and static versions.</span></span>

* <span data-ttu-id="519ef-128">Statikus betöltése: tooload statikusan, a statikus könyvtárat (SWC) fájl megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="519ef-128">Static loading: tooload statically, a static library (SWC) file is required.</span></span> <span data-ttu-id="519ef-129">Statikus beépülő modulok hozzá szeretné adni egy hivatkozást toohello projektek és egyesítési belül hello végső kimenetet fájl hello fordítás során.</span><span class="sxs-lookup"><span data-stu-id="519ef-129">Static plugins are added as a reference toohello projects and merge inside hello final output file at hello compile time.</span></span>
* <span data-ttu-id="519ef-130">Dinamikus betöltése: tooload dinamikusan, a lefordított (SWF) fájl megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="519ef-130">Dynamic loading: tooload dynamically, a precompiled (SWF) file is required.</span></span> <span data-ttu-id="519ef-131">Dinamikus beépülő modulok hello futásidejű töltődnek be és nem szerepel a hello projekt kimeneti.</span><span class="sxs-lookup"><span data-stu-id="519ef-131">Dynamic plugins are loaded in hello runtime and not included in hello project output.</span></span> <span data-ttu-id="519ef-132">(Lefordított kimeneti) Dinamikus beépülő modulok tölthetők be HTTP- és protokollok használatával.</span><span class="sxs-lookup"><span data-stu-id="519ef-132">(Compiled output) Dynamic plugins can be loaded using HTTP and FILE protocols.</span></span>

<span data-ttu-id="519ef-133">A statikus és dinamikus betöltése további információkért lásd: hello hivatalos [OSMF beépülő modul oldal](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span><span class="sxs-lookup"><span data-stu-id="519ef-133">For more information on static and dynamic loading, see hello official [OSMF plugin page](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span></span>

### <a name="ss-for-osmf-static-loading"></a><span data-ttu-id="519ef-134">SS OSMF statikus be</span><span class="sxs-lookup"><span data-stu-id="519ef-134">SS for OSMF Static Loading</span></span>
<span data-ttu-id="519ef-135">hello kódrészletben jeleníti meg, hogyan tooload statikusan hello SS beépülő modul OSMF és OSMF MediaFactory osztály alapvető videó lejátszása.</span><span class="sxs-lookup"><span data-stu-id="519ef-135">hello code snippet below shows how tooload hello SS plugin for OSMF statically and play a basic video using OSMF MediaFactory class.</span></span> <span data-ttu-id="519ef-136">Mielőtt hello SS OSMF kódot, győződjön meg arról, hogy hello projektbe egy hivatkozást tartalmaz-e a "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" hello statikus beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="519ef-136">Before including hello SS for OSMF code, please ensure that hello project reference includes hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" static plugin.</span></span>

```
package 
{

    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;



    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }

        private function initMediaPlayer():void
        {

            // Create hello container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds hello container toohello stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add hello listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load hello plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;

            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // hello plugin is loaded successfully.
            // Your web server needs toohost a valid crossdomain.xml file tooallow plugin toodownload Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // hello plugin is failed tooload ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started tooload.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code toodeal with Player Ready when it is hit hello first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it toohello page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add hello media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a><span data-ttu-id="519ef-137">SS OSMF dinamikus be</span><span class="sxs-lookup"><span data-stu-id="519ef-137">SS for OSMF Dynamic Loading</span></span>
<span data-ttu-id="519ef-138">hello kódrészletben látható, hogyan tooload hello SS beépülő modul OSMF dinamikusan és videolejátszás egy alapszintű hello OSMF MediaFactory osztály használatával.</span><span class="sxs-lookup"><span data-stu-id="519ef-138">hello code snippet below shows how tooload hello SS plugin for OSMF dynamically and play a basic video using hello OSMF MediaFactory class.</span></span> <span data-ttu-id="519ef-139">Mielőtt hello SS OSMF kódot, hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dinamikus beépülő modul toohello projekt mappa másolása Ha azt szeretné tooload fájl protokoll használatával, vagy másolja a webkiszolgálón a HTTP-terhelés.</span><span class="sxs-lookup"><span data-stu-id="519ef-139">Before including hello SS for OSMF code, copy hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynamic plugin toohello project folder if you want tooload using FILE protocol, or copy under a web server for HTTP load.</span></span> <span data-ttu-id="519ef-140">Nincs szükség tooinclude "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" hello a projekt hivatkozásainak van.</span><span class="sxs-lookup"><span data-stu-id="519ef-140">There is no need tooinclude "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" in hello project references.</span></span>

<span data-ttu-id="519ef-141">csomag {</span><span class="sxs-lookup"><span data-stu-id="519ef-141">package {</span></span>

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets hello size of hello SWF

    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }

        private function initMediaPlayer():void
        {

            // Create hello container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds hello container toohello stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add hello listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load hello plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs toohost a valid crossdomain.xml file tooallow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // hello plugin is loaded successfully.

            // Your web server needs toohost a valid crossdomain.xml file tooallow plugin toodownload Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // hello plugin is failed tooload ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started tooload.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code toodeal with Player Ready when it is hit hello first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it toohello page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add hello media element
            _mediaPlayerSprite.media = element;
        }     

    }
<span data-ttu-id="519ef-142">}</span><span class="sxs-lookup"><span data-stu-id="519ef-142">}</span></span>

## <a name="strobe-media--playback-with-hello-ss-odmf-dynamic-plugin"></a><span data-ttu-id="519ef-143">Villogó Media lejátszás hello SS ODMF dinamikus beépülő modul</span><span class="sxs-lookup"><span data-stu-id="519ef-143">Strobe Media  Playback with hello SS ODMF Dynamic Plugin</span></span>
<span data-ttu-id="519ef-144">hello a Smooth Streaming OSMF dinamikus beépülő modul összeegyeztethető [villogó Media lejátszás (SMP)](http://osmf.org/strobe_mediaplayback.html).</span><span class="sxs-lookup"><span data-stu-id="519ef-144">hello Smooth Streaming for OSMF dynamic plugin is compatible with [Strobe Media Playback (SMP)](http://osmf.org/strobe_mediaplayback.html).</span></span> <span data-ttu-id="519ef-145">Hello SS OSMF beépülő modul tooadd Smooth Streaming tartalom lejátszását tooSMP használhat.</span><span class="sxs-lookup"><span data-stu-id="519ef-145">You can use hello SS for OSMF plugin tooadd Smooth Streaming content playback tooSMP.</span></span> <span data-ttu-id="519ef-146">toodo, példány "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" egy webkiszolgálón a HTTP-terhelést, használja a következő hello lépések:</span><span class="sxs-lookup"><span data-stu-id="519ef-146">toodo this, copy "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" under a web server for HTTP load using hello following steps:</span></span>

1. <span data-ttu-id="519ef-147">Keresse meg a hello [villogó Media lejátszás telepítőből](http://osmf.org/dev/2.0gm/setup.html).</span><span class="sxs-lookup"><span data-stu-id="519ef-147">Browse hello [Strobe Media Playback setup page](http://osmf.org/dev/2.0gm/setup.html).</span></span> 
2. <span data-ttu-id="519ef-148">Állítsa be a hello src tooa Smooth Streaming forrás, (pl. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span><span class="sxs-lookup"><span data-stu-id="519ef-148">Set hello src tooa Smooth Streaming source, (e.g. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span></span> 
3. <span data-ttu-id="519ef-149">Hello szükséges konfigurációs módosításokat, majd kattintson a kép és a frissítés.</span><span class="sxs-lookup"><span data-stu-id="519ef-149">Make hello desired configuration changes and click Preview and Update.</span></span>
   
   <span data-ttu-id="519ef-150">**Megjegyzés:** a tartalom webkiszolgáló kell egy érvényes crossdomain.xml.</span><span class="sxs-lookup"><span data-stu-id="519ef-150">**Note** Your content web server needs a valid crossdomain.xml.</span></span> 
4. <span data-ttu-id="519ef-151">Másolással illessze be a hello kód tooa egyszerű HTML-weblap kedvenc szövegszerkesztőjével, többek között a következő példa hello használatával:</span><span class="sxs-lookup"><span data-stu-id="519ef-151">Copy and paste hello code tooa simple HTML page using your favorite text editor, such as in hello following example:</span></span>

        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



1. <span data-ttu-id="519ef-152">Adja hozzá a Smooth Streaming OSMF beépülő modul toohello beágyazási kódot, és mentse.</span><span class="sxs-lookup"><span data-stu-id="519ef-152">Add Smooth Streaming OSMF plugin toohello embed code and save.</span></span>
   
        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>
2. <span data-ttu-id="519ef-153">Mentse a HTML-lapot, és tegye közzé a tooa webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="519ef-153">Save your HTML page and publish tooa web server.</span></span> <span data-ttu-id="519ef-154">Tallózás toohello közzétett weblapot a kedvenc Flash&reg; Player internetböngésző (az Internet Explorer, Chrome, Firefox, így tovább) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="519ef-154">Browse toohello published web page using your favorite Flash&reg; Player enabled Internet browser (Internet Explorer, Chrome, Firefox, so on).</span></span>
3. <span data-ttu-id="519ef-155">Adobe belüli Smooth Streaming tartalom élvez&reg; Flash&reg; Player.</span><span class="sxs-lookup"><span data-stu-id="519ef-155">Enjoy Smooth Streaming content inside Adobe&reg; Flash&reg; Player.</span></span>

<span data-ttu-id="519ef-156">Az általános OSMF fejlesztési további információkért lásd: hello hivatalos [OSMF fejlesztési lap](http://osmf.org/resources.html).</span><span class="sxs-lookup"><span data-stu-id="519ef-156">For more information on general OSMF development, please see hello official [OSMF development page](http://osmf.org/resources.html).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="519ef-157">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="519ef-157">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="519ef-158">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="519ef-158">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="519ef-159">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="519ef-159">See Also</span></span>
[<span data-ttu-id="519ef-160">Adaptív adatfolyam-beépülő modul OSMF frissítéshez Microsoft</span><span class="sxs-lookup"><span data-stu-id="519ef-160">Microsoft Adaptive Streaming Plugin for OSMF Update</span></span>](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

