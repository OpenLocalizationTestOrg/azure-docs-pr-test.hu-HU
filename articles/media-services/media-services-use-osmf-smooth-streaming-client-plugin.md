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
# <a name="how-toouse-hello-microsoft-smooth-streaming-plugin-for-hello-adobe-open-source-media-framework"></a>Hogyan tooUse hello hello Adobe nyílt forrás Media keretrendszert a Microsoft Smooth Streaming beépülő modul
## <a name="overview"></a>Áttekintés
hello Microsoft Smooth Streaming beépülő modul megnyitása forrás Media keretrendszer 2.0-s (SS a OSMF) bővíti OSMF hello alapértelmezett képességeit, és hozzáad a tartalom lejátszását toonew Microsoft Smooth Streaming, valamint a meglévő OSMF játékosok. hello beépülő modul hozzáadja a lejátszás képességek tooStrobe Smooth Streaming Media lejátszás (SMP) is.

SS OSMF a beépülő modul két verziója tartalmazza:

* Statikus Smooth Streaming beépülő modul OSMF (.swc)
* Dinamikus Smooth Streaming beépülő modul OSMF (.swf)

Jelen dokumentum céljából feltételezzük, hogy rendelkezik-e OSMF és OSMF általános ismeretét hello olvasó beépülő modulok. OSMF kapcsolatos további információkért tekintse meg hello dokumentáció hello [hivatalos OSMF webhely](http://osmf.org/).

### <a name="smooth-streaming-plugin-for-osmf-20"></a>OSMF 2.0 Smooth Streaming beépülő modul
hello beépülő modul betöltése és igény szerinti Smooth Streaming tartalom lejátszását támogatja a következő funkciók hello:

* Igény szerinti Smooth Streaming lejátszás (Play, szüneteltetése, Seek, Stop)
* Élő Smooth Streaming lejátszás (lejátszás)
* Élő DVR funkciók (szünet, Seek, DVR lejátszás, nyissa meg a működés közbeni)
* Videó kodekek - H.264 támogatása
* Hang kodekek - AAC támogatása
* Több hang nyelvi váltás OSMF beépített API-khoz
* Maximális lejátszás minőségi kijelölés OSMF beépített API-khoz
* Oldalkocsi OSMF feliratok beépülő modul a feliratok bezárása
* Az Adobe&reg; Flash&reg; Player 11.4 vagy újabb verzióját.
* Ebben a verzióban csak a OSMF 2.0 támogatja.

## <a name="supported-features-and-known-issues"></a>Támogatott szolgáltatások és ismert problémák
Támogatott szolgáltatások, a nem támogatott funkciókat és az ismert problémák teljes listáját lásd túl[Ez a dokumentum](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).

## <a name="loading-hello-plugin"></a>Beépülő modul betöltése hello
OSMF beépülő modulok tölthetők be statikusan (fordítás során) vagy dinamikusan (futásidőben). hello Smooth Streaming beépülő modul OSMF le a dinamikus és statikus verziójával együtt.

* Statikus betöltése: tooload statikusan, a statikus könyvtárat (SWC) fájl megadása kötelező. Statikus beépülő modulok hozzá szeretné adni egy hivatkozást toohello projektek és egyesítési belül hello végső kimenetet fájl hello fordítás során.
* Dinamikus betöltése: tooload dinamikusan, a lefordított (SWF) fájl megadása kötelező. Dinamikus beépülő modulok hello futásidejű töltődnek be és nem szerepel a hello projekt kimeneti. (Lefordított kimeneti) Dinamikus beépülő modulok tölthetők be HTTP- és protokollok használatával.

A statikus és dinamikus betöltése további információkért lásd: hello hivatalos [OSMF beépülő modul oldal](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

### <a name="ss-for-osmf-static-loading"></a>SS OSMF statikus be
hello kódrészletben jeleníti meg, hogyan tooload statikusan hello SS beépülő modul OSMF és OSMF MediaFactory osztály alapvető videó lejátszása. Mielőtt hello SS OSMF kódot, győződjön meg arról, hogy hello projektbe egy hivatkozást tartalmaz-e a "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" hello statikus beépülő modul.

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


### <a name="ss-for-osmf-dynamic-loading"></a>SS OSMF dinamikus be
hello kódrészletben látható, hogyan tooload hello SS beépülő modul OSMF dinamikusan és videolejátszás egy alapszintű hello OSMF MediaFactory osztály használatával. Mielőtt hello SS OSMF kódot, hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dinamikus beépülő modul toohello projekt mappa másolása Ha azt szeretné tooload fájl protokoll használatával, vagy másolja a webkiszolgálón a HTTP-terhelés. Nincs szükség tooinclude "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" hello a projekt hivatkozásainak van.

csomag {

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
}

## <a name="strobe-media--playback-with-hello-ss-odmf-dynamic-plugin"></a>Villogó Media lejátszás hello SS ODMF dinamikus beépülő modul
hello a Smooth Streaming OSMF dinamikus beépülő modul összeegyeztethető [villogó Media lejátszás (SMP)](http://osmf.org/strobe_mediaplayback.html). Hello SS OSMF beépülő modul tooadd Smooth Streaming tartalom lejátszását tooSMP használhat. toodo, példány "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" egy webkiszolgálón a HTTP-terhelést, használja a következő hello lépések:

1. Keresse meg a hello [villogó Media lejátszás telepítőből](http://osmf.org/dev/2.0gm/setup.html). 
2. Állítsa be a hello src tooa Smooth Streaming forrás, (pl. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3. Hello szükséges konfigurációs módosításokat, majd kattintson a kép és a frissítés.
   
   **Megjegyzés:** a tartalom webkiszolgáló kell egy érvényes crossdomain.xml. 
4. Másolással illessze be a hello kód tooa egyszerű HTML-weblap kedvenc szövegszerkesztőjével, többek között a következő példa hello használatával:

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



1. Adja hozzá a Smooth Streaming OSMF beépülő modul toohello beágyazási kódot, és mentse.
   
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
2. Mentse a HTML-lapot, és tegye közzé a tooa webkiszolgáló. Tallózás toohello közzétett weblapot a kedvenc Flash&reg; Player internetböngésző (az Internet Explorer, Chrome, Firefox, így tovább) engedélyezve van.
3. Adobe belüli Smooth Streaming tartalom élvez&reg; Flash&reg; Player.

Az általános OSMF fejlesztési további információkért lásd: hello hivatalos [OSMF fejlesztési lap](http://osmf.org/resources.html).

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Adaptív adatfolyam-beépülő modul OSMF frissítéshez Microsoft](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

