---
title: "A nyílt forráskódú Media keretrendszer zökkenőmentes adatfolyam beépülő modul"
description: "Ismerje meg, hogyan használható az Azure Media Services Smooth Streaming beépülő modul az Adobe nyílt forrás Media keretrendszert."
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
ms.openlocfilehash: 9c764f176ae75085320882de3fb26d8e7d52daaf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a><span data-ttu-id="a9e5a-103">A Microsoft Smooth Streaming beépülő modul az Adobe nyílt forráskódú Media keretrendszer használata</span><span class="sxs-lookup"><span data-stu-id="a9e5a-103">How to Use the Microsoft Smooth Streaming Plugin for the Adobe Open Source Media Framework</span></span>
## <a name="overview"></a><span data-ttu-id="a9e5a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a9e5a-104">Overview</span></span>
<span data-ttu-id="a9e5a-105">A Microsoft Smooth Streaming beépülő modul megnyitása forrás Media keretrendszer 2.0-s (SS a OSMF) OSMF alapértelmezett bővíti ki, és hozzáadja a tartalom lejátszását Microsoft Smooth Streaming új és meglévő OSMF játékosok.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-105">The Microsoft Smooth Streaming plugin for Open Source Media Framework 2.0 (SS for OSMF) extends the default capabilities of OSMF and adds Microsoft Smooth Streaming content playback to new and existing OSMF players.</span></span> <span data-ttu-id="a9e5a-106">A beépülő modul villogó Media lejátszás (SMP) is hozzáadja Smooth Streaming lejátszás képességeit.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-106">The plugin also adds Smooth Streaming playback capabilities to Strobe Media Playback (SMP).</span></span>

<span data-ttu-id="a9e5a-107">SS OSMF a beépülő modul két verziója tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a9e5a-107">SS for OSMF includes two versions of plugin:</span></span>

* <span data-ttu-id="a9e5a-108">Statikus Smooth Streaming beépülő modul OSMF (.swc)</span><span class="sxs-lookup"><span data-stu-id="a9e5a-108">Static Smooth Streaming plugin for OSMF (.swc)</span></span>
* <span data-ttu-id="a9e5a-109">Dinamikus Smooth Streaming beépülő modul OSMF (.swf)</span><span class="sxs-lookup"><span data-stu-id="a9e5a-109">Dynamic Smooth Streaming plugin for OSMF (.swf)</span></span>

<span data-ttu-id="a9e5a-110">Jelen dokumentum céljából feltételezzük, hogy az olvasó rendelkezik OSMF és OSMF általános ismeretét beépülő modulok. OSMF kapcsolatos további információkért tekintse meg a dokumentáció a [hivatalos OSMF webhely](http://osmf.org/).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-110">This document assumes that the reader has a general working knowledge of OSMF and OSMF plug-ins. For more information about OSMF, please see the documentation on the [official OSMF site](http://osmf.org/).</span></span>

### <a name="smooth-streaming-plugin-for-osmf-20"></a><span data-ttu-id="a9e5a-111">OSMF 2.0 Smooth Streaming beépülő modul</span><span class="sxs-lookup"><span data-stu-id="a9e5a-111">Smooth Streaming plugin for OSMF 2.0</span></span>
<span data-ttu-id="a9e5a-112">A beépülő modul betöltése és igény szerinti, Smooth Streaming a következő funkciókkal tartalom lejátszását támogatja:</span><span class="sxs-lookup"><span data-stu-id="a9e5a-112">The plugin supports loading and playback of on-demand Smooth Streaming content with the following features:</span></span>

* <span data-ttu-id="a9e5a-113">Igény szerinti Smooth Streaming lejátszás (Play, szüneteltetése, Seek, Stop)</span><span class="sxs-lookup"><span data-stu-id="a9e5a-113">On-demand Smooth Streaming playback (Play, Pause, Seek, Stop)</span></span>
* <span data-ttu-id="a9e5a-114">Élő Smooth Streaming lejátszás (lejátszás)</span><span class="sxs-lookup"><span data-stu-id="a9e5a-114">Live Smooth Streaming playback (Play)</span></span>
* <span data-ttu-id="a9e5a-115">Élő DVR funkciók (szünet, Seek, DVR lejátszás, nyissa meg a működés közbeni)</span><span class="sxs-lookup"><span data-stu-id="a9e5a-115">Live DVR functions (Pause, Seek, DVR Playback, Go-to-Live)</span></span>
* <span data-ttu-id="a9e5a-116">Videó kodekek - H.264 támogatása</span><span class="sxs-lookup"><span data-stu-id="a9e5a-116">Support for video codecs - H.264</span></span>
* <span data-ttu-id="a9e5a-117">Hang kodekek - AAC támogatása</span><span class="sxs-lookup"><span data-stu-id="a9e5a-117">Support for Audio codecs - AAC</span></span>
* <span data-ttu-id="a9e5a-118">Több hang nyelvi váltás OSMF beépített API-khoz</span><span class="sxs-lookup"><span data-stu-id="a9e5a-118">Multiple audio language switching with OSMF built-in APIs</span></span>
* <span data-ttu-id="a9e5a-119">Maximális lejátszás minőségi kijelölés OSMF beépített API-khoz</span><span class="sxs-lookup"><span data-stu-id="a9e5a-119">Max playback quality selection with OSMF built-in APIs</span></span>
* <span data-ttu-id="a9e5a-120">Oldalkocsi OSMF feliratok beépülő modul a feliratok bezárása</span><span class="sxs-lookup"><span data-stu-id="a9e5a-120">Sidecar closed captions with OSMF captions plugin</span></span>
* <span data-ttu-id="a9e5a-121">Az Adobe&reg; Flash&reg; Player 11.4 vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-121">Adobe&reg; Flash&reg; Player 11.4 or higher.</span></span>
* <span data-ttu-id="a9e5a-122">Ebben a verzióban csak a OSMF 2.0 támogatja.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-122">This version only supports OSMF 2.0.</span></span>

## <a name="supported-features-and-known-issues"></a><span data-ttu-id="a9e5a-123">Támogatott szolgáltatások és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="a9e5a-123">Supported features and known issues</span></span>
<span data-ttu-id="a9e5a-124">Támogatott szolgáltatások, a nem támogatott funkciókat és az ismert problémák teljes listáját lásd [Ez a dokumentum](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-124">For a full list of supported features, unsupported features and known issues, refer to [this document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span></span>

## <a name="loading-the-plugin"></a><span data-ttu-id="a9e5a-125">A beépülő modul betöltése</span><span class="sxs-lookup"><span data-stu-id="a9e5a-125">Loading the Plugin</span></span>
<span data-ttu-id="a9e5a-126">OSMF beépülő modulok tölthetők be statikusan (fordítás során) vagy dinamikusan (futásidőben).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-126">OSMF plugins can be loaded statically (at compile time) or dynamically (at run-time).</span></span> <span data-ttu-id="a9e5a-127">A Smooth Streaming beépülő modul OSMF le a dinamikus és statikus verziójával együtt.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-127">The Smooth Streaming plugin for OSMF download includes both dynamic and static versions.</span></span>

* <span data-ttu-id="a9e5a-128">Statikus betöltése: statikusan betöltése, a statikus könyvtárat (SWC) fájl megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-128">Static loading: To load statically, a static library (SWC) file is required.</span></span> <span data-ttu-id="a9e5a-129">A projektek és a végső kimenetet fájl egyesítése mutató hivatkozás statikus beépülő modulok hozzá szeretné adni a fordítás során.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-129">Static plugins are added as a reference to the projects and merge inside the final output file at the compile time.</span></span>
* <span data-ttu-id="a9e5a-130">Dinamikus betöltése: dinamikusan betöltéséhez a lefordított (SWF) fájl megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-130">Dynamic loading: To load dynamically, a precompiled (SWF) file is required.</span></span> <span data-ttu-id="a9e5a-131">Dinamikus beépülő modulok betöltése a futásidejű, és nem szerepel a projekt kimenet.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-131">Dynamic plugins are loaded in the runtime and not included in the project output.</span></span> <span data-ttu-id="a9e5a-132">(Lefordított kimeneti) Dinamikus beépülő modulok tölthetők be HTTP- és protokollok használatával.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-132">(Compiled output) Dynamic plugins can be loaded using HTTP and FILE protocols.</span></span>

<span data-ttu-id="a9e5a-133">A statikus és dinamikus betöltése további információkért lásd: a hivatalos [OSMF beépülő modul oldal](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-133">For more information on static and dynamic loading, see the official [OSMF plugin page](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span></span>

### <a name="ss-for-osmf-static-loading"></a><span data-ttu-id="a9e5a-134">SS OSMF statikus be</span><span class="sxs-lookup"><span data-stu-id="a9e5a-134">SS for OSMF Static Loading</span></span>
<span data-ttu-id="a9e5a-135">Az alábbi kódrészlet a SS beépülő modul betöltése a OSMF statikusan és OSMF MediaFactory osztály alapvető videó lejátszása ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-135">The code snippet below shows how to load the SS plugin for OSMF statically and play a basic video using OSMF MediaFactory class.</span></span> <span data-ttu-id="a9e5a-136">Többek között a következő OSMF: SS, előtt győződjön meg arról, hogy a projektbe egy hivatkozást tartalmaz-e a "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" statikus beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-136">Before including the SS for OSMF code, please ensure that the project reference includes the "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" static plugin.</span></span>

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

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
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
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

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
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a><span data-ttu-id="a9e5a-137">SS OSMF dinamikus be</span><span class="sxs-lookup"><span data-stu-id="a9e5a-137">SS for OSMF Dynamic Loading</span></span>
<span data-ttu-id="a9e5a-138">Az alábbi kódrészlet szemlélteti a SS beépülő modul betöltése a OSMF dinamikusan és alapvető lejátszása videót a OSMF MediaFactory osztály használatával.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-138">The code snippet below shows how to load the SS plugin for OSMF dynamically and play a basic video using the OSMF MediaFactory class.</span></span> <span data-ttu-id="a9e5a-139">Többek között a következő OSMF: SS, mielőtt a "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dinamikus beépülő modul másolása a projekt mappába, ha betölti a fájl protokoll használatával, vagy a webkiszolgálóra HTTP terhelést másolja.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-139">Before including the SS for OSMF code, copy the "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynamic plugin to the project folder if you want to load using FILE protocol, or copy under a web server for HTTP load.</span></span> <span data-ttu-id="a9e5a-140">Nincs szükség ahhoz, hogy a projekt referenciáihoz "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" szerepeljen.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-140">There is no need to include "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" in the project references.</span></span>

<span data-ttu-id="a9e5a-141">csomag {</span><span class="sxs-lookup"><span data-stu-id="a9e5a-141">package {</span></span>

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets the size of the SWF

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

            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started to load.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 

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
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     

    }
<span data-ttu-id="a9e5a-142">}</span><span class="sxs-lookup"><span data-stu-id="a9e5a-142">}</span></span>

## <a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a><span data-ttu-id="a9e5a-143">Az SS ODMF dinamikus beépülő modul villogó Media lejátszás</span><span class="sxs-lookup"><span data-stu-id="a9e5a-143">Strobe Media  Playback with the SS ODMF Dynamic Plugin</span></span>
<span data-ttu-id="a9e5a-144">OSMF dinamikus beépülő modul Smooth Streaming összeegyeztethető [villogó Media lejátszás (SMP)](http://osmf.org/strobe_mediaplayback.html).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-144">The Smooth Streaming for OSMF dynamic plugin is compatible with [Strobe Media Playback (SMP)](http://osmf.org/strobe_mediaplayback.html).</span></span> <span data-ttu-id="a9e5a-145">A SS a OSMF beépülő modul segítségével adja hozzá a tartalom lejátszását Smooth Streaming SMP.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-145">You can use the SS for OSMF plugin to add Smooth Streaming content playback to SMP.</span></span> <span data-ttu-id="a9e5a-146">Ehhez másolja "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" alatt webkiszolgálón a HTTP-terhelést, az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="a9e5a-146">To do this, copy "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" under a web server for HTTP load using the following steps:</span></span>

1. <span data-ttu-id="a9e5a-147">Keresse meg a [villogó Media lejátszás telepítőből](http://osmf.org/dev/2.0gm/setup.html).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-147">Browse the [Strobe Media Playback setup page](http://osmf.org/dev/2.0gm/setup.html).</span></span> 
2. <span data-ttu-id="a9e5a-148">Állítsa be az src Smooth Streaming forráshoz, (pl. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span><span class="sxs-lookup"><span data-stu-id="a9e5a-148">Set the src to a Smooth Streaming source, (e.g. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span></span> 
3. <span data-ttu-id="a9e5a-149">A kívánt konfigurációs módosításokat, majd kattintson a kép és a frissítés.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-149">Make the desired configuration changes and click Preview and Update.</span></span>
   
   <span data-ttu-id="a9e5a-150">**Megjegyzés:** a tartalom webkiszolgáló kell egy érvényes crossdomain.xml.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-150">**Note** Your content web server needs a valid crossdomain.xml.</span></span> 
4. <span data-ttu-id="a9e5a-151">Másolja és illessze be a kódot egy egyszerű HTML-weblap kedvenc szövegszerkesztőjével, többek között az alábbi példa használatával:</span><span class="sxs-lookup"><span data-stu-id="a9e5a-151">Copy and paste the code to a simple HTML page using your favorite text editor, such as in the following example:</span></span>

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



1. <span data-ttu-id="a9e5a-152">Smooth Streaming OSMF beépülő modul hozzáadása a beágyazási kódot, és mentse.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-152">Add Smooth Streaming OSMF plugin to the embed code and save.</span></span>
   
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
2. <span data-ttu-id="a9e5a-153">Mentse a HTML-lapot, és tegye közzé a webkiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-153">Save your HTML page and publish to a web server.</span></span> <span data-ttu-id="a9e5a-154">Keresse meg a közzétett weblapot a kedvenc Flash&reg; Player internetböngésző (az Internet Explorer, Chrome, Firefox, így tovább) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-154">Browse to the published web page using your favorite Flash&reg; Player enabled Internet browser (Internet Explorer, Chrome, Firefox, so on).</span></span>
3. <span data-ttu-id="a9e5a-155">Adobe belüli Smooth Streaming tartalom élvez&reg; Flash&reg; Player.</span><span class="sxs-lookup"><span data-stu-id="a9e5a-155">Enjoy Smooth Streaming content inside Adobe&reg; Flash&reg; Player.</span></span>

<span data-ttu-id="a9e5a-156">Az általános OSMF fejlesztési további információkért lásd: a hivatalos [OSMF fejlesztési lap](http://osmf.org/resources.html).</span><span class="sxs-lookup"><span data-stu-id="a9e5a-156">For more information on general OSMF development, please see the official [OSMF development page](http://osmf.org/resources.html).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="a9e5a-157">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="a9e5a-157">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a9e5a-158">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="a9e5a-158">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="a9e5a-159">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a9e5a-159">See Also</span></span>
[<span data-ttu-id="a9e5a-160">Adaptív adatfolyam-beépülő modul OSMF frissítéshez Microsoft</span><span class="sxs-lookup"><span data-stu-id="a9e5a-160">Microsoft Adaptive Streaming Plugin for OSMF Update</span></span>](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

