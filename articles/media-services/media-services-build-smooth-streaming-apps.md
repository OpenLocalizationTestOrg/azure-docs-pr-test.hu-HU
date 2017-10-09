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
# <a name="how-toobuild-a-smooth-streaming-windows-store-application"></a><span data-ttu-id="04c57-103">Hogyan tooBuild Smooth Streaming Windows Áruházbeli alkalmazás</span><span class="sxs-lookup"><span data-stu-id="04c57-103">How tooBuild a Smooth Streaming Windows Store Application</span></span>

<span data-ttu-id="04c57-104">hello Smooth Streaming ügyfél SDK a Windows 8 lehetővé teszi, hogy a fejlesztők toobuild Windows Áruházbeli alkalmazásokat, amelyek játszhatja igény szerinti és élő Smooth Streaming tartalmát.</span><span class="sxs-lookup"><span data-stu-id="04c57-104">hello Smooth Streaming Client SDK for Windows 8 enables developers toobuild Windows Store applications that can play on-demand and live Smooth Streaming content.</span></span> <span data-ttu-id="04c57-105">Ezenkívül toohello Smooth Streaming is tartalom, hello SDK alapvető lejátszását gazdag olyan funkciókat biztosít, például a Microsoft PlayReady védelmi, minőségi korlátozást, Live DVR, hangadatfolyam váltás, állapot frissítéseket (például a minőségi megváltozik a figyeli ) és a hibaesemények, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="04c57-105">In addition toohello basic playback of Smooth Streaming content, hello SDK also provides rich features like Microsoft PlayReady protection, quality level restriction, Live DVR, audio stream switching, listening for status updates (such as quality level changes) and error events, and so on.</span></span> <span data-ttu-id="04c57-106">Hello támogatott szolgáltatások további információkért lásd: hello [kibocsátási megjegyzéseket](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes).</span><span class="sxs-lookup"><span data-stu-id="04c57-106">For more information of hello supported features, see hello [release notes](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes).</span></span> <span data-ttu-id="04c57-107">További információkért lásd: [Player keretrendszer Windows 8](http://playerframework.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="04c57-107">For more information, see [Player Framework for Windows 8](http://playerframework.codeplex.com/).</span></span> 

<span data-ttu-id="04c57-108">Ez az oktatóanyag során négy tapasztalatokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="04c57-108">This tutorial contains four lessons:</span></span>

1. <span data-ttu-id="04c57-109">Alapszintű zökkenőmentes adatfolyam áruház-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c57-109">Create a Basic Smooth Streaming Store Application</span></span>
2. <span data-ttu-id="04c57-110">A csúszka tooControl hello Media folyamatjelző sáv hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04c57-110">Add a Slider Bar tooControl hello Media Progress</span></span>
3. <span data-ttu-id="04c57-111">Válassza ki a Smooth Streaming adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="04c57-111">Select Smooth Streaming Streams</span></span>
4. <span data-ttu-id="04c57-112">Válassza ki a Smooth Streaming nyomon követi</span><span class="sxs-lookup"><span data-stu-id="04c57-112">Select Smooth Streaming Tracks</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04c57-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="04c57-113">Prerequisites</span></span>
* <span data-ttu-id="04c57-114">Windows 8 rendszeren futó 32 bites vagy 64 bites.</span><span class="sxs-lookup"><span data-stu-id="04c57-114">Windows 8 32-bit or 64-bit.</span></span> <span data-ttu-id="04c57-115">Beszerezheti [Windows 8 Enterprise Evaluation](http://msdn.microsoft.com/evalcenter/jj554510.aspx) msdn.</span><span class="sxs-lookup"><span data-stu-id="04c57-115">You can get [Windows 8 Enterprise Evaluation](http://msdn.microsoft.com/evalcenter/jj554510.aspx) from MSDN.</span></span>
* <span data-ttu-id="04c57-116">A Visual Studio 2012 vagy Visual Studio Express 2012 (vagy újabb verzió).</span><span class="sxs-lookup"><span data-stu-id="04c57-116">Visual Studio 2012 or Visual Studio Express 2012 (or a later version).</span></span> <span data-ttu-id="04c57-117">Beszerezheti a hello próbaverzióját [Itt](http://www.microsoft.com/visualstudio/11/downloads).</span><span class="sxs-lookup"><span data-stu-id="04c57-117">You can get hello trial version from [here](http://www.microsoft.com/visualstudio/11/downloads).</span></span>
* <span data-ttu-id="04c57-118">[Microsoft Smooth Streaming ügyfél SDK a Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).</span><span class="sxs-lookup"><span data-stu-id="04c57-118">[Microsoft Smooth Streaming Client SDK for Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).</span></span>

<span data-ttu-id="04c57-119">az egyes befejeződött hello megoldás letölthető MSDN fejlesztői mintakódok (Kódgalériából):</span><span class="sxs-lookup"><span data-stu-id="04c57-119">hello completed solution for each lesson can be downloaded from MSDN Developer Code Samples (Code Gallery):</span></span> 

* <span data-ttu-id="04c57-120">[1 rész](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – egy egyszerű Windows 8 Smooth Streaming Media Player</span><span class="sxs-lookup"><span data-stu-id="04c57-120">[Lesson 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - A Simple Windows 8 Smooth Streaming Media Player,</span></span> 
* <span data-ttu-id="04c57-121">[2 rész](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – egy egyszerű Windows 8 Smooth Streaming Media Player a csúszka a vezérlő,</span><span class="sxs-lookup"><span data-stu-id="04c57-121">[Lesson 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - A Simple Windows 8 Smooth Streaming Media Player with a Slider Bar Control,</span></span> 
* <span data-ttu-id="04c57-122">[3 rész](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – A Windows 8 Smooth Streaming Media Player adatfolyam választás</span><span class="sxs-lookup"><span data-stu-id="04c57-122">[Lesson 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - A Windows 8 Smooth Streaming Media Player with Stream Selection,</span></span>  
* <span data-ttu-id="04c57-123">[Rész 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – A Windows 8 Smooth Streaming Media Player követése választás.</span><span class="sxs-lookup"><span data-stu-id="04c57-123">[Lesson 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907)  - A Windows 8 Smooth Streaming Media Player with Track Selection.</span></span>

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a><span data-ttu-id="04c57-124">1. lecke: Alapvető zökkenőmentes adatfolyam áruház-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c57-124">Lesson 1: Create a Basic Smooth Streaming Store Application</span></span>

<span data-ttu-id="04c57-125">Ez a lecke hoz létre egy Windows Áruházbeli alkalmazással rendelkező MediaElement vezérlő tooplay Smooth Stream tartalom.</span><span class="sxs-lookup"><span data-stu-id="04c57-125">In this lesson, you will create a Windows Store application with a MediaElement control tooplay Smooth Stream content.</span></span>  <span data-ttu-id="04c57-126">hello futó alkalmazás néz ki:</span><span class="sxs-lookup"><span data-stu-id="04c57-126">hello running application looks like:</span></span>

![Példa a Smooth Streaming Windows Áruházbeli alkalmazás][PlayerApplication]

<span data-ttu-id="04c57-128">További információ a Windows Áruházbeli alkalmazások fejlesztése: [fejlesztése kiváló alkalmazások a Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx).</span><span class="sxs-lookup"><span data-stu-id="04c57-128">For more information on developing Windows Store application, see [Develop Great Apps for Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx).</span></span> <span data-ttu-id="04c57-129">Ez a lecke hello az alábbi eljárásokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="04c57-129">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="04c57-130">Windows áruház-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c57-130">Create a Windows Store project</span></span>
2. <span data-ttu-id="04c57-131">Tervezési hello felhasználói felület (XAML)</span><span class="sxs-lookup"><span data-stu-id="04c57-131">Design hello user interface (XAML)</span></span>
3. <span data-ttu-id="04c57-132">Módosítsa a fájl mögötti kódban hello</span><span class="sxs-lookup"><span data-stu-id="04c57-132">Modify hello code behind file</span></span>
4. <span data-ttu-id="04c57-133">Fordítsa le és hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="04c57-133">Compile and test hello application</span></span>

<span data-ttu-id="04c57-134">**a Windows áruház projekt toocreate**</span><span class="sxs-lookup"><span data-stu-id="04c57-134">**toocreate a Windows Store project**</span></span>

1. <span data-ttu-id="04c57-135">Visual Studio 2012 vagy újabb fut.</span><span class="sxs-lookup"><span data-stu-id="04c57-135">Run Visual Studio 2012 or later.</span></span>
2. <span data-ttu-id="04c57-136">A hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="04c57-136">From hello **FILE** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="04c57-137">Hello új projekt párbeszédpanel ha típusa, vagy jelölje be a következő hello értékeket:</span><span class="sxs-lookup"><span data-stu-id="04c57-137">From hello New Project dialog, type or select  hello following values:</span></span>

| <span data-ttu-id="04c57-138">Név</span><span class="sxs-lookup"><span data-stu-id="04c57-138">Name</span></span> | <span data-ttu-id="04c57-139">Érték</span><span class="sxs-lookup"><span data-stu-id="04c57-139">Value</span></span> |
| --- | --- |
| <span data-ttu-id="04c57-140">Sablon csoport</span><span class="sxs-lookup"><span data-stu-id="04c57-140">Template group</span></span> |<span data-ttu-id="04c57-141">Telepített/sablonok/Visual C# / Windows Áruházbeli</span><span class="sxs-lookup"><span data-stu-id="04c57-141">Installed/Templates/Visual C#/Windows Store</span></span> |
| <span data-ttu-id="04c57-142">Sablon</span><span class="sxs-lookup"><span data-stu-id="04c57-142">Template</span></span> |<span data-ttu-id="04c57-143">Üres alkalmazás (XAML)</span><span class="sxs-lookup"><span data-stu-id="04c57-143">Blank App (XAML)</span></span> |
| <span data-ttu-id="04c57-144">Név</span><span class="sxs-lookup"><span data-stu-id="04c57-144">Name</span></span> |<span data-ttu-id="04c57-145">SSPlayer</span><span class="sxs-lookup"><span data-stu-id="04c57-145">SSPlayer</span></span> |
| <span data-ttu-id="04c57-146">Hely</span><span class="sxs-lookup"><span data-stu-id="04c57-146">Location</span></span> |<span data-ttu-id="04c57-147">C:\SSTutorials</span><span class="sxs-lookup"><span data-stu-id="04c57-147">C:\SSTutorials</span></span> |
| <span data-ttu-id="04c57-148">Megoldás neve</span><span class="sxs-lookup"><span data-stu-id="04c57-148">Solution Name</span></span> |<span data-ttu-id="04c57-149">SSPlayer</span><span class="sxs-lookup"><span data-stu-id="04c57-149">SSPlayer</span></span> |
| <span data-ttu-id="04c57-150">A megoldáshoz könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="04c57-150">Create directory for solution</span></span> |<span data-ttu-id="04c57-151">(kiválasztva)</span><span class="sxs-lookup"><span data-stu-id="04c57-151">(selected)</span></span> |

1. <span data-ttu-id="04c57-152">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="04c57-152">Click **OK**.</span></span>

<span data-ttu-id="04c57-153">**egy hivatkozási toohello Smooth Streaming ügyfél SDK tooadd**</span><span class="sxs-lookup"><span data-stu-id="04c57-153">**tooadd a reference toohello Smooth Streaming Client SDK**</span></span>

1. <span data-ttu-id="04c57-154">A Megoldáskezelőben kattintson a jobb gombbal **SSPlayer**, és kattintson a **hivatkozás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="04c57-154">From Solution Explorer, right-click **SSPlayer**, and then click **Add Reference**.</span></span>
2. <span data-ttu-id="04c57-155">Írja be vagy válassza ki a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-155">Type or select hello following values:</span></span>

| <span data-ttu-id="04c57-156">Név</span><span class="sxs-lookup"><span data-stu-id="04c57-156">Name</span></span> | <span data-ttu-id="04c57-157">Érték</span><span class="sxs-lookup"><span data-stu-id="04c57-157">Value</span></span> |
| --- | --- |
| <span data-ttu-id="04c57-158">Referencia-csoport</span><span class="sxs-lookup"><span data-stu-id="04c57-158">Reference group</span></span> |<span data-ttu-id="04c57-159">Windows/bővítmények</span><span class="sxs-lookup"><span data-stu-id="04c57-159">Windows/Extensions</span></span> |
| <span data-ttu-id="04c57-160">Referencia</span><span class="sxs-lookup"><span data-stu-id="04c57-160">Reference</span></span> |<span data-ttu-id="04c57-161">Válassza ki a Microsoft Smooth Streaming ügyfél SDK a Windows 8 és a Microsoft Visual C++ futásidejű csomag</span><span class="sxs-lookup"><span data-stu-id="04c57-161">Select Microsoft Smooth Streaming Client SDK for Windows 8 and Microsoft Visual C++ Runtime Package</span></span> |

1. <span data-ttu-id="04c57-162">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="04c57-162">Click **OK**.</span></span> 

<span data-ttu-id="04c57-163">A felvett hello hivatkozik, hello megcélzott platform (x64 vagy x86) ki kell választania, Any CPU platform konfiguráció hozzáadása hivatkozások fog működni.</span><span class="sxs-lookup"><span data-stu-id="04c57-163">After adding hello references, you must select hello targeted platform (x64 or x86), adding references will not work for Any CPU platform configuration.</span></span>  <span data-ttu-id="04c57-164">A megoldáskezelőben látni fogja, sárga figyelmeztető megjelölés ezek hozzá hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="04c57-164">In solution explorer, you will see yellow warning mark for these added references.</span></span>

<span data-ttu-id="04c57-165">**toodesign hello player felhasználói felülete**</span><span class="sxs-lookup"><span data-stu-id="04c57-165">**toodesign hello player user interface**</span></span>

1. <span data-ttu-id="04c57-166">A Megoldáskezelőben kattintson duplán **MainPage.xaml** tooopen azt hello kialakításában megtekintése.</span><span class="sxs-lookup"><span data-stu-id="04c57-166">From Solution Explorer, double click **MainPage.xaml** tooopen it in hello design view.</span></span>
2. <span data-ttu-id="04c57-167">Keresse meg a hello  **&lt;rács&gt;**  és  **&lt;/Grid&gt;**  címkék hello XAML-fájl, és a Beillesztés hello következő kódot a két hello címkék között:</span><span class="sxs-lookup"><span data-stu-id="04c57-167">Locate hello **&lt;Grid&gt;** and **&lt;/Grid&gt;**  tags hello XAML file, and paste hello following code between hello two tags:</span></span>

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
   
   <span data-ttu-id="04c57-168">hello MediaElement vezérlő használt tooplayback media.</span><span class="sxs-lookup"><span data-stu-id="04c57-168">hello MediaElement control is used tooplayback media.</span></span> <span data-ttu-id="04c57-169">hello csúszkavezérlő sliderProgress nevű folyamatban hello következő lecke toocontrol hello adathordozó használható.</span><span class="sxs-lookup"><span data-stu-id="04c57-169">hello slider control named sliderProgress will be used in hello next lesson toocontrol hello media progress.</span></span>
3. <span data-ttu-id="04c57-170">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-170">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-171">hello MediaElement vezérlő nem támogatja a Smooth Streaming tartalom out-of-box.</span><span class="sxs-lookup"><span data-stu-id="04c57-171">hello MediaElement control does not support Smooth Streaming content out-of-box.</span></span> <span data-ttu-id="04c57-172">tooenable hello Smooth Streaming támogatást, regisztrálnia kell a Smooth Streaming bájtos-adatfolyam hello kezelő kiterjesztésű és MIME-típus.</span><span class="sxs-lookup"><span data-stu-id="04c57-172">tooenable hello Smooth Streaming support, you must register hello Smooth Streaming byte-stream handler by file name extension and MIME type.</span></span>  <span data-ttu-id="04c57-173">tooregister, hello Windows.Media névtér hello MediaExtensionManager.RegisterByteStremHandler módszert használja.</span><span class="sxs-lookup"><span data-stu-id="04c57-173">tooregister, you use hello MediaExtensionManager.RegisterByteStremHandler method of hello Windows.Media namespace.</span></span>

<span data-ttu-id="04c57-174">Az XAML-fájl az egyes eseménykezelők társított hello vezérlők.</span><span class="sxs-lookup"><span data-stu-id="04c57-174">In this XAML file, some event handlers are associated with hello controls.</span></span>  <span data-ttu-id="04c57-175">Meg kell adnia azokat eseménykezelők.</span><span class="sxs-lookup"><span data-stu-id="04c57-175">You must define those event handlers.</span></span>

<span data-ttu-id="04c57-176">**toomodify hello fájl mögötti kódban**</span><span class="sxs-lookup"><span data-stu-id="04c57-176">**toomodify hello code behind file**</span></span>

1. <span data-ttu-id="04c57-177">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-177">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-178">Hello fájl hello tetején adja hozzá a hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="04c57-178">At hello top of hello file, add hello following using statement:</span></span>
   
        using Windows.Media;
3. <span data-ttu-id="04c57-179">Hello hello elején **MainPage** osztály, adja hozzá a következő adatelem hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-179">At hello beginning of hello **MainPage** class, add hello following data member:</span></span>
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. <span data-ttu-id="04c57-180">Hello hello végén **MainPage** konstruktor, adja hozzá az alábbi két hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-180">At hello end of hello **MainPage** constructor, add hello following two lines:</span></span>
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. <span data-ttu-id="04c57-181">Hello hello végén **MainPage** osztály, illessze be a kódját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-181">At hello end of hello **MainPage** class, paste hello following code:</span></span>
   
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

<span data-ttu-id="04c57-182">hello sliderProgress_PointerPressed eseménykezelő itt van definiálva.</span><span class="sxs-lookup"><span data-stu-id="04c57-182">hello sliderProgress_PointerPressed event handler is defined here.</span></span>  <span data-ttu-id="04c57-183">Nincsenek további works toodo tooget akkor működik, amelyek szerepelnek hello a jelen oktatóanyag következő lecke.</span><span class="sxs-lookup"><span data-stu-id="04c57-183">There are more works toodo tooget it working, which will be covered in hello next lesson of this tutorial.</span></span>
6. <span data-ttu-id="04c57-184">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-184">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-185">hello végzett hello fájl mögötti kódban kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="04c57-185">hello finished hello code behind file shall look like this:</span></span>

![A Visual Studio, Smooth Streaming Windows Áruházbeli alkalmazás Codeview][CodeViewPic]

<span data-ttu-id="04c57-187">**toocompile és tesztelési hello alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="04c57-187">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="04c57-188">A hello **BUILD** menüben kattintson a **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="04c57-188">From hello **BUILD** menu, click **Configuration Manager**.</span></span>
2. <span data-ttu-id="04c57-189">Változás **aktív megoldás platform** toomatch a fejlesztői platform.</span><span class="sxs-lookup"><span data-stu-id="04c57-189">Change **Active solution platform** toomatch your development platform.</span></span>
3. <span data-ttu-id="04c57-190">Nyomja le az **F6** toocompile hello projekt.</span><span class="sxs-lookup"><span data-stu-id="04c57-190">Press **F6** toocompile hello project.</span></span> 
4. <span data-ttu-id="04c57-191">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c57-191">Press **F5** toorun hello application.</span></span>
5. <span data-ttu-id="04c57-192">Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik.</span><span class="sxs-lookup"><span data-stu-id="04c57-192">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
6. <span data-ttu-id="04c57-193">Kattintson a **forrás beállítása**.</span><span class="sxs-lookup"><span data-stu-id="04c57-193">Click **Set Source**.</span></span> <span data-ttu-id="04c57-194">Mivel **automatikus lejátszása** media automatikusan játszik hello alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="04c57-194">Because **Auto Play** is enabled by default, hello media shall play automatically.</span></span>  <span data-ttu-id="04c57-195">Hello media hello segítségével szabályozhatja **lejátszása**, **szünet** és **leállítása** gombokat.</span><span class="sxs-lookup"><span data-stu-id="04c57-195">You can control hello media using hello **Play**, **Pause** and **Stop** buttons.</span></span>  <span data-ttu-id="04c57-196">Hello media kötet hello függőleges csúszka segítségével szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="04c57-196">You can control hello media volume using hello vertical slider.</span></span>  <span data-ttu-id="04c57-197">Azonban hello vízszintes csúszkán szabályozása hello media folyamatban teljesen még nincs implementálva.</span><span class="sxs-lookup"><span data-stu-id="04c57-197">However hello horizontal slider for controlling hello media progress is not fully implemented yet.</span></span> 

<span data-ttu-id="04c57-198">Lesson1 befejeződött.</span><span class="sxs-lookup"><span data-stu-id="04c57-198">You have completed lesson1.</span></span>  <span data-ttu-id="04c57-199">Ez a lecke egy MediaElement vezérlő tooplayback Smooth Streaming tartalmat használ.</span><span class="sxs-lookup"><span data-stu-id="04c57-199">In this lesson, you use a MediaElement control tooplayback Smooth Streaming content.</span></span>  <span data-ttu-id="04c57-200">Hello a következő leckében adhat egy Smooth Streaming tartalom hello csúszkát toocontrol hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="04c57-200">In hello next lesson, you will add a slider toocontrol hello progress of hello Smooth Streaming content.</span></span>

## <a name="lesson-2-add-a-slider-bar-toocontrol-hello-media-progress"></a><span data-ttu-id="04c57-201">2. lecke: Egy csúszkát tooControl hello Media folyamatjelző sáv hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04c57-201">Lesson 2: Add a Slider Bar tooControl hello Media Progress</span></span>

<span data-ttu-id="04c57-202">1. lecke a Windows Áruházbeli alkalmazások egy MediaElement XAML vezérlő tooplayback médiatartalom Smooth Streaming segítségével létrehozott.</span><span class="sxs-lookup"><span data-stu-id="04c57-202">In lesson 1, you created a Windows Store application with a MediaElement XAML control tooplayback Smooth Streaming media content.</span></span>  <span data-ttu-id="04c57-203">Néhány alapvető adathordozó funkciók, például a start, stop, várjon származik.</span><span class="sxs-lookup"><span data-stu-id="04c57-203">It comes some basic media functions like start, stop and pause.</span></span>  <span data-ttu-id="04c57-204">Ez a lecke adhat a csúszka sávjának vezérlő toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c57-204">In this lesson, you will add a slider bar control toohello application.</span></span>

<span data-ttu-id="04c57-205">Ebben az oktatóanyagban egy időzítő tooupdate hello csúszka helyzete hello MediaElement vezérlő aktuális helyzete hello alapján használjuk.</span><span class="sxs-lookup"><span data-stu-id="04c57-205">In this tutorial, we will use a timer tooupdate hello slider position based on hello current position of hello MediaElement control.</span></span>  <span data-ttu-id="04c57-206">hello csúszkát indítsa el, és a Befejezés időpontja is élő tartalmak esetén frissíteni kell toobe.</span><span class="sxs-lookup"><span data-stu-id="04c57-206">hello slider start and end time also need toobe updated in case of live content.</span></span>  <span data-ttu-id="04c57-207">Ez jobban kezelhető hello adaptív forrás frissítés esemény.</span><span class="sxs-lookup"><span data-stu-id="04c57-207">This can be better handled in hello adaptive source update event.</span></span>

<span data-ttu-id="04c57-208">Források olyan objektumok, amelyek hozhat létre a media adatokat.</span><span class="sxs-lookup"><span data-stu-id="04c57-208">Media sources are objects that generate media data.</span></span>  <span data-ttu-id="04c57-209">hello forrás feloldó egy URL-címe vagy bájtos adatfolyam vesz igénybe, és létrehozza a hello megfelelő médiaforrást az adott tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="04c57-209">hello source resolver takes a URL or byte stream and creates hello appropriate media source for that content.</span></span>  <span data-ttu-id="04c57-210">hello forrás feloldó hello alkalmazások toocreate források hello általános esetben.</span><span class="sxs-lookup"><span data-stu-id="04c57-210">hello source resolver is hello standard way for hello applications toocreate media sources.</span></span> 

<span data-ttu-id="04c57-211">Ez a lecke hello az alábbi eljárásokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="04c57-211">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="04c57-212">Hello Smooth Streaming kezelő regisztrálása</span><span class="sxs-lookup"><span data-stu-id="04c57-212">Register hello Smooth Streaming handler</span></span> 
2. <span data-ttu-id="04c57-213">Hello adaptív forrás manager szintű eseménykezelőinek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04c57-213">Add hello adaptive source manager level event handlers</span></span>
3. <span data-ttu-id="04c57-214">Hello adaptív forrás szintű eseménykezelőinek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04c57-214">Add hello adaptive source level event handlers</span></span>
4. <span data-ttu-id="04c57-215">Az eseménykezelők MediaElement hozzáadása</span><span class="sxs-lookup"><span data-stu-id="04c57-215">Add MediaElement event handlers</span></span>
5. <span data-ttu-id="04c57-216">Adja hozzá a csúszka kapcsolódó vonalkódja</span><span class="sxs-lookup"><span data-stu-id="04c57-216">Add slider bar related code</span></span>
6. <span data-ttu-id="04c57-217">Fordítsa le és hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="04c57-217">Compile and test hello application</span></span>

<span data-ttu-id="04c57-218">**tooregister hello Smooth Streaming bájtos-adatfolyam-kezelő és pass hello propertyset**</span><span class="sxs-lookup"><span data-stu-id="04c57-218">**tooregister hello Smooth Streaming byte-stream handler and pass hello propertyset**</span></span>

1. <span data-ttu-id="04c57-219">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-219">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-220">Elején hello hello fájlt, adja hozzá a hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="04c57-220">At hello beginning of hello file, add hello following using statement:</span></span>

        using Microsoft.Media.AdaptiveStreaming;
3. <span data-ttu-id="04c57-221">Elején hello hello MainPage osztály, adja hozzá a következő adattagok hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-221">At hello beginning of hello MainPage class, add hello following data members:</span></span>

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. <span data-ttu-id="04c57-222">Belső hello **MainPage** konstruktor, adja hozzá a következő kód után hello hello **ez. Components(); inicializálása**  vonal- és hello regisztrációs kód sorok hello előző lecke:</span><span class="sxs-lookup"><span data-stu-id="04c57-222">Inside hello **MainPage** constructor, add hello following code after hello **this.Initialize Components();** line and hello registration code lines written in hello previous lesson:</span></span>

        // Gets hello default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value tooAdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. <span data-ttu-id="04c57-223">Belső hello **MainPage** konstruktor, módosítsa a két hello RegisterByteStreamHandler módszerek tooadd hello oda-paramétereket:</span><span class="sxs-lookup"><span data-stu-id="04c57-223">Inside hello **MainPage** constructor, modify hello two RegisterByteStreamHandler methods tooadd hello forth parameters:</span></span>

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
6. <span data-ttu-id="04c57-224">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-224">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-225">**tooadd hello adaptív forrás manager szintű eseménykezelő**</span><span class="sxs-lookup"><span data-stu-id="04c57-225">**tooadd hello adaptive source manager level event handler**</span></span>

1. <span data-ttu-id="04c57-226">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-226">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-227">Belső hello **MainPage** osztály, adja hozzá a következő adatelem hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-227">Inside hello **MainPage** class, add hello following data member:</span></span>
   
     <span data-ttu-id="04c57-228">személyes AdaptiveSource adaptiveSource = null;</span><span class="sxs-lookup"><span data-stu-id="04c57-228">private AdaptiveSource adaptiveSource = null;</span></span>
3. <span data-ttu-id="04c57-229">Hello hello végén **MainPage** osztály, adja hozzá a következő eseménykezelő hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-229">At hello end of hello **MainPage** class, add hello following event handler:</span></span>
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. <span data-ttu-id="04c57-230">Hello hello végén **MainPage** konstruktor, adja hozzá a következő sor toosubscribe toohello adaptív forrás open esemény hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-230">At hello end of hello **MainPage** constructor, add hello following line toosubscribe toohello adaptive source open event:</span></span>
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. <span data-ttu-id="04c57-231">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-231">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-232">**tooadd adaptív forrás szintű eseménykezelők**</span><span class="sxs-lookup"><span data-stu-id="04c57-232">**tooadd adaptive source level event handlers**</span></span>

1. <span data-ttu-id="04c57-233">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-233">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-234">Belső hello **MainPage** osztály, adja hozzá a következő adatelem hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-234">Inside hello **MainPage** class, add hello following data member:</span></span>
   
     <span data-ttu-id="04c57-235">személyes AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   személyes jegyzék manifestObject;</span><span class="sxs-lookup"><span data-stu-id="04c57-235">private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   private Manifest manifestObject;</span></span>
3. <span data-ttu-id="04c57-236">Hello hello végén **MainPage** osztály, adja hozzá a következő eseménykezelők hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-236">At hello end of hello **MainPage** class, add hello following event handlers:</span></span>

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
4. <span data-ttu-id="04c57-237">Hello hello végén **mediaElement AdaptiveSourceOpened** módszer, adja hozzá a következő kód toosubscribe toohello események hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-237">At hello end of hello **mediaElement AdaptiveSourceOpened** method, add hello following code toosubscribe toohello events:</span></span>
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. <span data-ttu-id="04c57-238">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-238">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-239">hello azonos események állnak rendelkezésre adaptív forrás Manager szinten is, amely funkció közös tooall media elemek hello alkalmazásban kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="04c57-239">hello same events are available on Adaptive Source manger level as well, which can be used for handling functionality common tooall media elements in hello app.</span></span> <span data-ttu-id="04c57-240">Minden egyes AdaptiveSource saját eseményeket is tartalmazza, és minden AdaptiveSource események átkerül a AdaptiveSourceManager.</span><span class="sxs-lookup"><span data-stu-id="04c57-240">Each AdaptiveSource includes its own events and all AdaptiveSource events will be cascaded under AdaptiveSourceManager.</span></span>

<span data-ttu-id="04c57-241">**tooadd Media elem eseménykezelők**</span><span class="sxs-lookup"><span data-stu-id="04c57-241">**tooadd Media Element event handlers**</span></span>

1. <span data-ttu-id="04c57-242">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-242">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-243">Hello hello végén **MainPage** osztály, adja hozzá a következő eseménykezelők hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-243">At hello end of hello **MainPage** class, add hello following event handlers:</span></span>

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
3. <span data-ttu-id="04c57-244">Hello hello végén **MainPage** konstruktor, adja hozzá a következő kód toosubscript toohello események hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-244">At hello end of hello **MainPage** constructor, add hello following code toosubscript toohello events:</span></span>

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. <span data-ttu-id="04c57-245">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-245">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-246">**kapcsolódó vonalkód tooadd csúszka**</span><span class="sxs-lookup"><span data-stu-id="04c57-246">**tooadd slider bar related code**</span></span>

1. <span data-ttu-id="04c57-247">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-247">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-248">Elején hello hello fájlt, adja hozzá a hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="04c57-248">At hello beginning of hello file, add hello following using statement:</span></span>
      
        using Windows.UI.Core;
3. <span data-ttu-id="04c57-249">Belső hello **MainPage** osztály, adja hozzá a következő adattagok hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-249">Inside hello **MainPage** class, add hello following data members:</span></span>
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. <span data-ttu-id="04c57-250">Hello hello végén **MainPage** konstruktor, adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-250">At hello end of hello **MainPage** constructor, add hello following code:</span></span>
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. <span data-ttu-id="04c57-251">Hello hello végén **MainPage** osztály, adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-251">At hello end of hello **MainPage** class, add hello following code:</span></span>

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
><span data-ttu-id="04c57-252">CoreDispatcher toohello felhasználói felület szálából nem UI-szálból használt toomake módosítások.</span><span class="sxs-lookup"><span data-stu-id="04c57-252">CoreDispatcher is used toomake changes toohello UI thread from non UI Thread.</span></span> <span data-ttu-id="04c57-253">Esetén a dispatcher száltól szűk keresztmetszet fejlesztői választható UI-elemet által biztosított toouse kézbesítő többé tooupdate százalékát.</span><span class="sxs-lookup"><span data-stu-id="04c57-253">In case of bottleneck on dispatcher thread, developer can choose toouse dispatcher provided by UI-element he/she intends tooupdate.</span></span>  <span data-ttu-id="04c57-254">Példa:</span><span class="sxs-lookup"><span data-stu-id="04c57-254">For example:</span></span>
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. <span data-ttu-id="04c57-255">Hello hello végén **mediaElement_AdaptiveSourceStatusUpdated** módszer, adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-255">At hello end of hello **mediaElement_AdaptiveSourceStatusUpdated** method, add hello following code:</span></span>

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. <span data-ttu-id="04c57-256">Hello hello végén **MediaOpened** módszer, adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-256">At hello end of hello **MediaOpened** method, add hello following code:</span></span>

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. <span data-ttu-id="04c57-257">Nyomja le az **CTRL + S** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="04c57-257">Press **CTRL+S** toosave hello file.</span></span>

<span data-ttu-id="04c57-258">**toocompile és tesztelési hello alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="04c57-258">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="04c57-259">Nyomja le az **F6** toocompile hello projekt.</span><span class="sxs-lookup"><span data-stu-id="04c57-259">Press **F6** toocompile hello project.</span></span> 
2. <span data-ttu-id="04c57-260">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c57-260">Press **F5** toorun hello application.</span></span>
3. <span data-ttu-id="04c57-261">Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik.</span><span class="sxs-lookup"><span data-stu-id="04c57-261">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="04c57-262">Kattintson a **forrás beállítása**.</span><span class="sxs-lookup"><span data-stu-id="04c57-262">Click **Set Source**.</span></span> 
5. <span data-ttu-id="04c57-263">Teszt hello csúszka.</span><span class="sxs-lookup"><span data-stu-id="04c57-263">Test hello slider bar.</span></span>

<span data-ttu-id="04c57-264">2. lecke befejeződött.</span><span class="sxs-lookup"><span data-stu-id="04c57-264">You have completed lesson 2.</span></span>  <span data-ttu-id="04c57-265">Ez a lecke hozzáadott egy csúszkát tooapplication.</span><span class="sxs-lookup"><span data-stu-id="04c57-265">In this lesson you added a slider tooapplication.</span></span> 

## <a name="lesson-3-select-smooth-streaming-streams"></a><span data-ttu-id="04c57-266">3. lecke: Válassza ki a Smooth Streaming adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="04c57-266">Lesson 3: Select Smooth Streaming Streams</span></span>
<span data-ttu-id="04c57-267">Smooth Streaming több nyelv zeneszámok, amelyek választható hello hozzáférhetnek a toostream képes-e.</span><span class="sxs-lookup"><span data-stu-id="04c57-267">Smooth Streaming is capable toostream content with multiple language audio tracks that are selectable by hello viewers.</span></span>  <span data-ttu-id="04c57-268">Ez a lecke teszi lehetővé megjelenítők tooselect adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="04c57-268">In this lesson, you will enable viewers tooselect streams.</span></span> <span data-ttu-id="04c57-269">Ez a lecke hello az alábbi eljárásokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="04c57-269">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="04c57-270">Hello XAML-fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="04c57-270">Modify hello XAML file</span></span>
2. <span data-ttu-id="04c57-271">Hello kód behand fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="04c57-271">Modify hello code behand file</span></span>
3. <span data-ttu-id="04c57-272">Fordítsa le és hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="04c57-272">Compile and test hello application</span></span>

<span data-ttu-id="04c57-273">**toomodify hello XAML-fájl**</span><span class="sxs-lookup"><span data-stu-id="04c57-273">**toomodify hello XAML file**</span></span>

1. <span data-ttu-id="04c57-274">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **adatforrásnézet-tervezőből**.</span><span class="sxs-lookup"><span data-stu-id="04c57-274">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Designer**.</span></span>
2. <span data-ttu-id="04c57-275">Keresse meg &lt;Grid.RowDefinitions&gt;, és módosítja a hello RowDefinitions, azok a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="04c57-275">Locate &lt;Grid.RowDefinitions&gt;, and modify hello RowDefinitions so they looks like:</span></span>
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. <span data-ttu-id="04c57-276">Belső hello &lt;rács&gt;&lt;/Grid&gt; címkék hozzáadása hello következő kódot toodefine egy listbox vezérlőt, így a felhasználók elérhető adatfolyamok hello tartalmazó lista, és válassza ki az adatfolyamok:</span><span class="sxs-lookup"><span data-stu-id="04c57-276">Inside hello &lt;Grid&gt;&lt;/Grid&gt; tags, add hello following code toodefine a listbox control, so users can see hello list of available streams, and select streams:</span></span>

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
4. <span data-ttu-id="04c57-277">Nyomja le az **CTRL + S** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="04c57-277">Press **CTRL+S** toosave hello changes.</span></span>

<span data-ttu-id="04c57-278">**toomodify hello fájl mögötti kódban**</span><span class="sxs-lookup"><span data-stu-id="04c57-278">**toomodify hello code behind file**</span></span>

1. <span data-ttu-id="04c57-279">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-279">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-280">Hello SSPlayer névtéren belül adjon meg egy új osztályt:</span><span class="sxs-lookup"><span data-stu-id="04c57-280">Inside hello SSPlayer namespace, add a new class:</span></span>
   
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
3. <span data-ttu-id="04c57-281">A hello MainPage osztály hello elején adja hozzá a következő változó definíciók hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-281">At hello beginning of hello MainPage class, add hello following variable definitions:</span></span>
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. <span data-ttu-id="04c57-282">Belül hello MainPage osztály adja hozzá a következő régióban hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-282">Inside hello MainPage class, add hello following region:</span></span>
   
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
5. <span data-ttu-id="04c57-283">Keresse meg a hello mediaElement_ManifestReady metódus, a következő kódot a hello végén hello függvény hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="04c57-283">Locate hello mediaElement_ManifestReady method, append hello following code at hello end of hello function:</span></span>
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    <span data-ttu-id="04c57-284">Ezért MediaElement jegyzékfájl készen áll, amikor hello kód hello elérhető adatfolyamok listájának lekérése, tölti fel hello felhasználói felület lista hello listájával.</span><span class="sxs-lookup"><span data-stu-id="04c57-284">So when MediaElement manifest is ready, hello code gets a list of hello available streams, and populates hello UI list box with hello list.</span></span>
6. <span data-ttu-id="04c57-285">Hello MainPage osztály, belül található hello UI gombokat események régió kattintson, és adja hozzá a következő függvény definíciójának hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-285">Inside hello MainPage class, locate hello UI buttons click events region, and then add hello following function definition:</span></span>
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on hello presentation
            changeStreams(selectedStreams);
        }

<span data-ttu-id="04c57-286">**toocompile és tesztelési hello alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="04c57-286">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="04c57-287">Nyomja le az **F6** toocompile hello projekt.</span><span class="sxs-lookup"><span data-stu-id="04c57-287">Press **F6** toocompile hello project.</span></span> 
2. <span data-ttu-id="04c57-288">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c57-288">Press **F5** toorun hello application.</span></span>
3. <span data-ttu-id="04c57-289">Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik.</span><span class="sxs-lookup"><span data-stu-id="04c57-289">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="04c57-290">Kattintson a **forrás beállítása**.</span><span class="sxs-lookup"><span data-stu-id="04c57-290">Click **Set Source**.</span></span> 
5. <span data-ttu-id="04c57-291">hello alapértelmezett nyelv a audio_eng.</span><span class="sxs-lookup"><span data-stu-id="04c57-291">hello default language is audio_eng.</span></span> <span data-ttu-id="04c57-292">Próbálja meg tooswitch audio_eng és audio_es között.</span><span class="sxs-lookup"><span data-stu-id="04c57-292">Try tooswitch between audio_eng and audio_es.</span></span> <span data-ttu-id="04c57-293">Everytime, válasszon egy új adatfolyam, hello Elküldés gombra kell kattintania.</span><span class="sxs-lookup"><span data-stu-id="04c57-293">Everytime, you select a new stream, you must click hello Submit button.</span></span>

<span data-ttu-id="04c57-294">3. lecke befejeződött.</span><span class="sxs-lookup"><span data-stu-id="04c57-294">You have completed lesson 3.</span></span>  <span data-ttu-id="04c57-295">Ez a lecke hello funkció toochoose adatfolyamok adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="04c57-295">In this lesson, you add hello functionality toochoose streams.</span></span>

## <a name="lesson-4-select-smooth-streaming-tracks"></a><span data-ttu-id="04c57-296">4. lecke: Válassza ki a Smooth Streaming nyomon követi</span><span class="sxs-lookup"><span data-stu-id="04c57-296">Lesson 4: Select Smooth Streaming Tracks</span></span>
<span data-ttu-id="04c57-297">Egy Smooth Streaming bemutató különböző szolgáltatásminőségi szinteket (átviteli sebességek) és a megoldások kódolású több videó fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="04c57-297">A Smooth Streaming presentation can contain multiple video files encoded with different quality levels (bit rates) and resolutions.</span></span> <span data-ttu-id="04c57-298">Ez a lecke lehetővé teszi felhasználók tooselect követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="04c57-298">In this lesson, you will enable users tooselect tracks.</span></span> <span data-ttu-id="04c57-299">Ez a lecke hello az alábbi eljárásokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="04c57-299">This lesson contains hello following procedures:</span></span>

1. <span data-ttu-id="04c57-300">Hello XAML-fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="04c57-300">Modify hello XAML file</span></span>
2. <span data-ttu-id="04c57-301">Hello kód behand fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="04c57-301">Modify hello code behand file</span></span>
3. <span data-ttu-id="04c57-302">Fordítsa le és hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="04c57-302">Compile and test hello application</span></span>

<span data-ttu-id="04c57-303">**toomodify hello XAML-fájl**</span><span class="sxs-lookup"><span data-stu-id="04c57-303">**toomodify hello XAML file**</span></span>

1. <span data-ttu-id="04c57-304">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **adatforrásnézet-tervezőből**.</span><span class="sxs-lookup"><span data-stu-id="04c57-304">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Designer**.</span></span>
2. <span data-ttu-id="04c57-305">Keresse meg a hello &lt;rács&gt; hello nevű címke **gridStreamAndBitrateSelection**, a következő kód hello címke hello végén hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="04c57-305">Locate hello &lt;Grid&gt; tag with hello name **gridStreamAndBitrateSelection**, append hello following code at hello end of hello tag:</span></span>
   
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
3. <span data-ttu-id="04c57-306">Nyomja le az **CTRL + S** toosave ő változik</span><span class="sxs-lookup"><span data-stu-id="04c57-306">Press **CTRL+S** toosave he changes</span></span>

<span data-ttu-id="04c57-307">**toomodify hello fájl mögötti kódban**</span><span class="sxs-lookup"><span data-stu-id="04c57-307">**toomodify hello code behind file**</span></span>

1. <span data-ttu-id="04c57-308">A Megoldáskezelőben kattintson a jobb gombbal **MainPage.xaml**, és kattintson a **nézet kód**.</span><span class="sxs-lookup"><span data-stu-id="04c57-308">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="04c57-309">Hello SSPlayer névtéren belül adjon meg egy új osztályt:</span><span class="sxs-lookup"><span data-stu-id="04c57-309">Inside hello SSPlayer namespace, add a new class:</span></span>
   
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
3. <span data-ttu-id="04c57-310">A hello MainPage osztály hello elején adja hozzá a következő változó definíciók hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-310">At hello beginning of hello MainPage class, add hello following variable definitions:</span></span>
   
        private List<Track> availableTracks;
4. <span data-ttu-id="04c57-311">Belül hello MainPage osztály adja hozzá a következő régióban hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-311">Inside hello MainPage class, add hello following region:</span></span>
   
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
5. <span data-ttu-id="04c57-312">Keresse meg a hello mediaElement_ManifestReady metódus, a következő kódot a hello végén hello függvény hello hozzáfűzése:</span><span class="sxs-lookup"><span data-stu-id="04c57-312">Locate hello mediaElement_ManifestReady method, append hello following code at hello end of hello function:</span></span>
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. <span data-ttu-id="04c57-313">Hello MainPage osztály, belül található hello UI gombokat események régió kattintson, és adja hozzá a következő függvény definíciójának hello:</span><span class="sxs-lookup"><span data-stu-id="04c57-313">Inside hello MainPage class, locate hello UI buttons click events region, and then add hello following function definition:</span></span>
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on hello presentation
            changeStreams(selectedStreams);
         }

<span data-ttu-id="04c57-314">**toocompile és tesztelési hello alkalmazás**</span><span class="sxs-lookup"><span data-stu-id="04c57-314">**toocompile and test hello application**</span></span>

1. <span data-ttu-id="04c57-315">Nyomja le az **F6** toocompile hello projekt.</span><span class="sxs-lookup"><span data-stu-id="04c57-315">Press **F6** toocompile hello project.</span></span> 
2. <span data-ttu-id="04c57-316">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04c57-316">Press **F5** toorun hello application.</span></span>
3. <span data-ttu-id="04c57-317">Hello alkalmazás hello tetején hello alapértelmezett Smooth Streaming URL-címet használja, vagy adjon meg egy másik.</span><span class="sxs-lookup"><span data-stu-id="04c57-317">At hello top of hello application, you can either use hello default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="04c57-318">Kattintson a **forrás beállítása**.</span><span class="sxs-lookup"><span data-stu-id="04c57-318">Click **Set Source**.</span></span> 
5. <span data-ttu-id="04c57-319">Alapértelmezés szerint összes hello nyomon követi a hello video-adatfolyammá alakítja ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="04c57-319">By default, all of hello tracks of hello video stream are selected.</span></span> <span data-ttu-id="04c57-320">tooexperiment hello bit arány módosításokat, válassza ki a hello legalacsonyabb átviteli sebesség érhető el, és válassza a hello legnagyobb átviteli sebesség érhető el.</span><span class="sxs-lookup"><span data-stu-id="04c57-320">tooexperiment hello bit rate changes, you can select hello lowest bit rate available, and then select hello highest bit rate available.</span></span> <span data-ttu-id="04c57-321">Minden egyes módosítása után kattintson a küldés.</span><span class="sxs-lookup"><span data-stu-id="04c57-321">You must click Submit after each change.</span></span>  <span data-ttu-id="04c57-322">Hello videominőséget módosításokat is láthatják.</span><span class="sxs-lookup"><span data-stu-id="04c57-322">You can see hello video quality changes.</span></span>

<span data-ttu-id="04c57-323">4. lecke befejeződött.</span><span class="sxs-lookup"><span data-stu-id="04c57-323">You have completed lesson 4.</span></span>  <span data-ttu-id="04c57-324">Ez a lecke hozzáadása hello funkció toochoose követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="04c57-324">In this lesson, you add hello functionality toochoose tracks.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="04c57-325">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="04c57-325">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="04c57-326">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="04c57-326">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a><span data-ttu-id="04c57-327">Egyéb erőforrások:</span><span class="sxs-lookup"><span data-stu-id="04c57-327">Other Resources:</span></span>
* [<span data-ttu-id="04c57-328">Hogyan toobuild egy Smooth Streaming Windows 8 JavaScript-alkalmazást, az összetett funkciók</span><span class="sxs-lookup"><span data-stu-id="04c57-328">How toobuild a Smooth Streaming Windows 8 JavaScript application with advanced features</span></span>](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [<span data-ttu-id="04c57-329">Zökkenőmentes adatfolyam műszaki áttekintése</span><span class="sxs-lookup"><span data-stu-id="04c57-329">Smooth Streaming Technical Overview</span></span>](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

