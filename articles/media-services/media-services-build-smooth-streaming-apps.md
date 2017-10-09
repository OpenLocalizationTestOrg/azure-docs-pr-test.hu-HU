---
title: "aaaSmooth Streaming Windows Áruházbeli alkalmazás oktatóanyag |} Microsoft Docs"
description: "Ismerje meg, hogyan szabályozza a toouse Azure Media Services toocreate egy C# Windows Áruházbeli alkalmazással rendelkező XML MediaElement a tooplayback Smooth Stream tartalmát."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: juliako
ms.openlocfilehash: b02aa2c7f68fe22a23ea846d72fdd23bfba2b19c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobuild-a-smooth-streaming-windows-store-application"></a>Hogyan tooBuild Smooth Streaming Windows Áruházbeli alkalmazás

hello Smooth Streaming ügyfél SDK a Windows 8 lehetővé teszi, hogy a fejlesztők toobuild Windows Áruházbeli alkalmazásokat, amelyek játszhatja igény szerinti és élő Smooth Streaming tartalmát. Ezenkívül toohello Smooth Streaming is tartalom, hello SDK alapvető lejátszását gazdag olyan funkciókat biztosít, például a Microsoft PlayReady védelmi, minőségi korlátozást, Live DVR, hangadatfolyam váltás, állapot frissítéseket (például a minőségi megváltozik a figyeli ) és a hibaesemények, és így tovább. Hello támogatott szolgáltatások további információkért lásd: hello [kibocsátási megjegyzéseket](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). További információkért lásd: [Player keretrendszer Windows 8](http://playerframework.codeplex.com/). 

Ez az oktatóanyag során négy tapasztalatokat tartalmazza:

1. Alapszintű zökkenőmentes adatfolyam áruház-alkalmazás létrehozása
2. A csúszka tooControl hello Media folyamatjelző sáv hozzáadása
3. Válassza ki a Smooth Streaming adatfolyamok
4. Válassza ki a Smooth Streaming nyomon követi

## <a name="prerequisites"></a>Előfeltételek
* Windows 8 rendszeren futó 32 bites vagy 64 bites. Beszerezheti [Windows 8 Enterprise Evaluation](http://msdn.microsoft.com/evalcenter/jj554510.aspx) msdn.
* A Visual Studio 2012 vagy Visual Studio Express 2012 (vagy újabb verzió). Beszerezheti a hello próbaverzióját [Itt](http://www.microsoft.com/visualstudio/11/downloads).
* [Microsoft Smooth Streaming ügyfél SDK a Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

az egyes befejeződött hello megoldás letölthető MSDN fejlesztői mintakódok (Kódgalériából): 

* [1 rész](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – egy egyszerű Windows 8 Smooth Streaming Media Player 
* [2 rész](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – egy egyszerű Windows 8 Smooth Streaming Media Player a csúszka a vezérlő, 
* [3 rész](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – A Windows 8 Smooth Streaming Media Player adatfolyam választás  
* [Rész 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – A Windows 8 Smooth Streaming Media Player követése választás.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>1. lecke: Alapvető zökkenőmentes adatfolyam áruház-alkalmazás létrehozása

Ez a lecke hoz létre egy Windows Áruházbeli alkalmazással rendelkező MediaElement vezérlő tooplay Smooth Stream tartalom.  hello futó alkalmazás néz ki:

![Példa a Smooth Streaming Windows Áruházbeli alkalmazás][PlayerApplication]

További információ a Windows Áruházbeli alkalmazások fejlesztése: [fejlesztése kiváló alkalmazások a Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). Ez a lecke hello az alábbi eljárásokat tartalmazza:

1. Windows áruház-projekt létrehozása
2. Tervezési hello felhasználói felület (XAML)
3. Módosítsa a fájl mögötti kódban hello
4. Fordítsa le és hello alkalmazás tesztelése

**a Windows áruház projekt toocreate**

1. Visual Studio 2012 vagy újabb fut.
2. A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
3. Hello új projekt párbeszédpanel ha típusa, vagy jelölje be a következő hello értékeket:

| Név | Érték |
| --- | --- |
| Sablon csoport |Telepített/sablonok/Visual C# / Windows Áruházbeli |
| Sablon |Üres alkalmazás (XAML) |
| Név |SSPlayer |
| Hely |C:\SSTutorials |
| Megoldás neve |SSPlayer |
| A megoldáshoz könyvtár létrehozása |(kiválasztva) |

1. Kattintson az **OK** gombra.

**egy hivatkozási toohello Smooth Streaming ügyfél SDK tooadd**

1. A Megoldáskezelőben kattintson a jobb gombbal **SSPlayer**, és kattintson a **hivatkozás hozzáadása**.
2. Írja be vagy válassza ki a következő értékek hello:

| Név | Érték |
| --- | --- |
| Referencia-csoport |Windows/bővítmények |
| Referencia |Válassza ki a Microsoft Smooth Streaming ügyfél SDK a Windows 8 és a Microsoft Visual C++ futásidejű csomag |

1. Kattintson az **OK** gombra. 

A felvett hello hivatkozik, hello megcélzott platform (x64 vagy x86) ki kell választania, Any CPU platform konfiguráció hozzáadása hivatkozások fog működni.  A megoldáskezelőben látni fogja, sárga figyelmeztető megjelölés ezek hozzá hivatkozásokat.

**toodesign hello player felhasználói felülete**

1. A Megoldáskezelőben kattintson duplán **MainPage.xaml** tooopen azt hello kialakításában megtekintése.
2. Keresse meg a hello  **&lt;rács&gt;**  és  **&lt;/Grid&gt;**  címkék hello XAML-fájl, és a Beillesztés hello következő kódot a két hello címkék között:

         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   
   hello MediaElement vezérlő használt tooplayback media. hello csúszkavezérlő sliderProgress nevű folyamatban hello következő lecke toocontrol hello adathordozó használható.
3. Nyomja le az **CTRL + S** toosave hello fájlt.

hello MediaElement vezérlő nem támogatja a Smooth Streaming tartalom out-of-box. tooenable hello Smooth Streaming támogatást, regisztrálnia kell a Smooth Streaming bájtos-adatfolyam hello kezelő kiterjesztésű és MIME-típus.  tooregister, hello Windows.Media névtér hello MediaExtensionManager.RegisterByteStremHandler módszert használja.

Az XAML-fájl az egyes eseménykezelők társított hello vezérlők.  Meg kell adnia azokat eseménykezelők.

**toomodify hello fájl mögötti kódban**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Hello fájl hello tetején adja hozzá a hello következő using utasítást:
   
        using Windows.Media;
3. Hello hello elején **MainPage** osztály, adja hozzá a következő adatelem hello:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Hello hello végén **MainPage** konstruktor, adja hozzá az alábbi két hello:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Hello hello végén **MainPage** osztály, illessze be a kódját a következő hello:
   
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click hello Play button tooplay hello media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek tooposition " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion

hello sliderProgress_PointerPressed eseménykezelő itt van definiálva.  Nincsenek további works toodo tooget akkor működik, amelyek szerepelnek hello a jelen oktatóanyag következő lecke.
6. Nyomja le az **CTRL + S** toosave hello fájlt.

hello végzett hello fájl mögötti kódban kell kinéznie:

![A Visual Studio, Smooth Streaming Windows Áruházbeli alkalmazás Codeview][CodeViewPic]

**toocompile és tesztelési hello alkalmazás**

1. A hello **BUILD** menüben kattintson a **Configuration Manager**.
2. Változás **aktív megoldás platform** toomatch a fejlesztői platform.
3. Nyomja le az **F6** toocompile hello projekt. 
4. Nyomja le az **F5** toorun hello alkalmazás.
5. Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik. 
6. Kattintson a **forrás beállítása**. Mivel **automatikus lejátszása** media automatikusan játszik hello alapértelmezés szerint engedélyezve van.  Hello media hello segítségével szabályozhatja **lejátszása**, **szünet** és **leállítása** gombokat.  Hello media kötet hello függőleges csúszka segítségével szabályozhatja.  Azonban hello vízszintes csúszkán szabályozása hello media folyamatban teljesen még nincs implementálva. 

Lesson1 befejeződött.  Ez a lecke egy MediaElement vezérlő tooplayback Smooth Streaming tartalmat használ.  Hello a következő leckében adhat egy Smooth Streaming tartalom hello csúszkát toocontrol hello előrehaladását.

## <a name="lesson-2-add-a-slider-bar-toocontrol-hello-media-progress"></a>2. lecke: Egy csúszkát tooControl hello Media folyamatjelző sáv hozzáadása

1. lecke a Windows Áruházbeli alkalmazások egy MediaElement XAML vezérlő tooplayback médiatartalom Smooth Streaming segítségével létrehozott.  Néhány alapvető adathordozó funkciók, például a start, stop, várjon származik.  Ez a lecke adhat a csúszka sávjának vezérlő toohello alkalmazás.

Ebben az oktatóanyagban egy időzítő tooupdate hello csúszka helyzete hello MediaElement vezérlő aktuális helyzete hello alapján használjuk.  hello csúszkát indítsa el, és a Befejezés időpontja is élő tartalmak esetén frissíteni kell toobe.  Ez jobban kezelhető hello adaptív forrás frissítés esemény.

Források olyan objektumok, amelyek hozhat létre a media adatokat.  hello forrás feloldó egy URL-címe vagy bájtos adatfolyam vesz igénybe, és létrehozza a hello megfelelő médiaforrást az adott tartalomhoz.  hello forrás feloldó hello alkalmazások toocreate források hello általános esetben. 

Ez a lecke hello az alábbi eljárásokat tartalmazza:

1. Hello Smooth Streaming kezelő regisztrálása 
2. Hello adaptív forrás manager szintű eseménykezelőinek hozzáadása
3. Hello adaptív forrás szintű eseménykezelőinek hozzáadása
4. Az eseménykezelők MediaElement hozzáadása
5. Adja hozzá a csúszka kapcsolódó vonalkódja
6. Fordítsa le és hello alkalmazás tesztelése

**tooregister hello Smooth Streaming bájtos-adatfolyam-kezelő és pass hello propertyset**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Elején hello hello fájlt, adja hozzá a hello következő using utasítást:

        using Microsoft.Media.AdaptiveStreaming;
3. Elején hello hello MainPage osztály, adja hozzá a következő adattagok hello:

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. Belső hello **MainPage** konstruktor, adja hozzá a következő kód után hello hello **ez. Components(); inicializálása**  vonal- és hello regisztrációs kód sorok hello előző lecke:

        // Gets hello default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value tooAdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. Belső hello **MainPage** konstruktor, módosítsa a két hello RegisterByteStreamHandler módszerek tooadd hello oda-paramétereket:

         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass hello propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
6. Nyomja le az **CTRL + S** toosave hello fájlt.

**tooadd hello adaptív forrás manager szintű eseménykezelő**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Belső hello **MainPage** osztály, adja hozzá a következő adatelem hello:
   
     személyes AdaptiveSource adaptiveSource = null;
3. Hello hello végén **MainPage** osztály, adja hozzá a következő eseménykezelő hello:
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. Hello hello végén **MainPage** konstruktor, adja hozzá a következő sor toosubscribe toohello adaptív forrás open esemény hello:
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. Nyomja le az **CTRL + S** toosave hello fájlt.

**tooadd adaptív forrás szintű eseménykezelők**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Belső hello **MainPage** osztály, adja hozzá a következő adatelem hello:
   
     személyes AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   személyes jegyzék manifestObject;
3. Hello hello végén **MainPage** osztály, adja hozzá a következő eseménykezelők hello:

         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
4. Hello hello végén **mediaElement AdaptiveSourceOpened** módszer, adja hozzá a következő kód toosubscribe toohello események hello:
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. Nyomja le az **CTRL + S** toosave hello fájlt.

hello azonos események állnak rendelkezésre adaptív forrás Manager szinten is, amely funkció közös tooall media elemek hello alkalmazásban kezelésére használható. Minden egyes AdaptiveSource saját eseményeket is tartalmazza, és minden AdaptiveSource események átkerül a AdaptiveSourceManager.

**tooadd Media elem eseménykezelők**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Hello hello végén **MainPage** osztály, adja hozzá a következő eseménykezelők hello:

         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
3. Hello hello végén **MainPage** konstruktor, adja hozzá a következő kód toosubscript toohello események hello:

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. Nyomja le az **CTRL + S** toosave hello fájlt.

**kapcsolódó vonalkód tooadd csúszka**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Elején hello hello fájlt, adja hozzá a hello következő using utasítást:
      
        using Windows.UI.Core;
3. Belső hello **MainPage** osztály, adja hozzá a következő adattagok hello:
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. Hello hello végén **MainPage** konstruktor, adja hozzá a következő kód hello:
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. Hello hello végén **MainPage** osztály, adja hozzá a következő kód hello:

         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
      
>[!NOTE]
>CoreDispatcher toohello felhasználói felület szálából nem UI-szálból használt toomake módosítások. Esetén a dispatcher száltól szűk keresztmetszet fejlesztői választható UI-elemet által biztosított toouse kézbesítő többé tooupdate százalékát.  Példa:
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. Hello hello végén **mediaElement_AdaptiveSourceStatusUpdated** módszer, adja hozzá a következő kód hello:

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. Hello hello végén **MediaOpened** módszer, adja hozzá a következő kód hello:

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. Nyomja le az **CTRL + S** toosave hello fájlt.

**toocompile és tesztelési hello alkalmazás**

1. Nyomja le az **F6** toocompile hello projekt. 
2. Nyomja le az **F5** toorun hello alkalmazás.
3. Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik. 
4. Kattintson a **forrás beállítása**. 
5. Teszt hello csúszka.

2. lecke befejeződött.  Ez a lecke hozzáadott egy csúszkát tooapplication. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>3. lecke: Válassza ki a Smooth Streaming adatfolyamok
Smooth Streaming több nyelv zeneszámok, amelyek választható hello hozzáférhetnek a toostream képes-e.  Ez a lecke teszi lehetővé megjelenítők tooselect adatfolyamokat. Ez a lecke hello az alábbi eljárásokat tartalmazza:

1. Hello XAML-fájl módosítása
2. Hello kód behand fájl módosítása
3. Fordítsa le és hello alkalmazás tesztelése

**toomodify hello XAML-fájl**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **adatforrásnézet-tervezőből**.
2. Keresse meg &lt;Grid.RowDefinitions&gt;, és módosítja a hello RowDefinitions, azok a következőhöz hasonló:
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. Belső hello &lt;rács&gt;&lt;/Grid&gt; címkék hozzáadása hello következő kódot toodefine egy listbox vezérlőt, így a felhasználók elérhető adatfolyamok hello tartalmazó lista, és válassza ki az adatfolyamok:

         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
4. Nyomja le az **CTRL + S** toosave hello módosításokat.

**toomodify hello fájl mögötti kódban**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Hello SSPlayer névtéren belül adjon meg egy új osztályt:
   
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke hello video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
3. A hello MainPage osztály hello elején adja hozzá a következő változó definíciók hello:
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. Belül hello MainPage osztály adja hozzá a következő régióban hello:
   
        #region stream selection
        ///<summary>
        ///Functionality tooselect streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from hello mediaElement_ManifestReady event handler 
        // tooretrieve hello streams and populate them toohello local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate hello stream lists based on hello types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select hello default selected streams from hello manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set hello list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update hello stream check box list on hello UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select hello frist video stream from hello list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "hello audio stream is changed too" + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select hello frist audio stream from hello list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
5. Keresse meg a hello mediaElement_ManifestReady metódus, a következő kódot a hello végén hello függvény hello hozzáfűzése:
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    Ezért MediaElement jegyzékfájl készen áll, amikor hello kód hello elérhető adatfolyamok listájának lekérése, tölti fel hello felhasználói felület lista hello listájával.
6. Hello MainPage osztály, belül található hello UI gombokat események régió kattintson, és adja hozzá a következő függvény definíciójának hello:
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on hello presentation
            changeStreams(selectedStreams);
        }

**toocompile és tesztelési hello alkalmazás**

1. Nyomja le az **F6** toocompile hello projekt. 
2. Nyomja le az **F5** toorun hello alkalmazás.
3. Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik. 
4. Kattintson a **forrás beállítása**. 
5. hello alapértelmezett nyelv a audio_eng. Próbálja meg tooswitch audio_eng és audio_es között. Everytime, válasszon egy új adatfolyam, hello Elküldés gombra kell kattintania.

3. lecke befejeződött.  Ez a lecke hello funkció toochoose adatfolyamok adja hozzá.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>4. lecke: Válassza ki a Smooth Streaming nyomon követi
Egy Smooth Streaming bemutató különböző szolgáltatásminőségi szinteket (átviteli sebességek) és a megoldások kódolású több videó fájlokat tartalmazza. Ez a lecke lehetővé teszi felhasználók tooselect követi nyomon. Ez a lecke hello az alábbi eljárásokat tartalmazza:

1. Hello XAML-fájl módosítása
2. Hello kód behand fájl módosítása
3. Fordítsa le és hello alkalmazás tesztelése

**toomodify hello XAML-fájl**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **adatforrásnézet-tervezőből**.
2. Keresse meg a hello &lt;rács&gt; hello nevű címke **gridStreamAndBitrateSelection**, a következő kód hello címke hello végén hello hozzáfűzése:
   
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
3. Nyomja le az **CTRL + S** toosave ő változik

**toomodify hello fájl mögötti kódban**

1. A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.
2. Hello SSPlayer névtéren belül adjon meg egy új osztályt:
   
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
3. A hello MainPage osztály hello elején adja hozzá a következő változó definíciók hello:
   
        private List<Track> availableTracks;
4. Belül hello MainPage osztály adja hozzá a következő régióban hello:
   
        #region track selection
        /// <summary>
        /// Functionality tooselect video streams
        /// </summary>
   
        /// This Function gets hello tracks for hello selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets hello video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set hello UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update hello track check box list on hello UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of hello selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects hello tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
5. Keresse meg a hello mediaElement_ManifestReady metódus, a következő kódot a hello végén hello függvény hello hozzáfűzése:
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. Hello MainPage osztály, belül található hello UI gombokat események régió kattintson, és adja hozzá a következő függvény definíciójának hello:
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on hello presentation
            changeStreams(selectedStreams);
         }

**toocompile és tesztelési hello alkalmazás**

1. Nyomja le az **F6** toocompile hello projekt. 
2. Nyomja le az **F5** toorun hello alkalmazás.
3. Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik. 
4. Kattintson a **forrás beállítása**. 
5. Alapértelmezés szerint összes hello nyomon követi a hello video-adatfolyammá alakítja ki van jelölve. tooexperiment hello bit arány módosításokat, válassza ki a hello legalacsonyabb átviteli sebesség érhető el, és válassza a hello legnagyobb átviteli sebesség érhető el. Minden egyes módosítása után kattintson a küldés.  Hello videominőséget módosításokat is láthatják.

4. lecke befejeződött.  Ez a lecke hozzáadása hello funkció toochoose követi nyomon.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Egyéb erőforrások:
* [Hogyan toobuild egy Smooth Streaming Windows 8 JavaScript-alkalmazást, az összetett funkciók](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Zökkenőmentes adatfolyam műszaki áttekintése](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

