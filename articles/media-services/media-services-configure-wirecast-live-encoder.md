---
title: "aaaConfigure hello Telestream Wirecast kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello Wirecast élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e373f6c08232c652e65db584ded409c405d8cffe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-wirecast-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="88a92-103">Hello Wirecast kódoló toosend egyféle sávszélességű élő adatfolyamot használata</span><span class="sxs-lookup"><span data-stu-id="88a92-103">Use hello Wirecast encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="88a92-104">Wirecast</span><span class="sxs-lookup"><span data-stu-id="88a92-104">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="88a92-105">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="88a92-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="88a92-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="88a92-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="88a92-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="88a92-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="88a92-108">Ez a témakör bemutatja, hogyan tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás.</span><span class="sxs-lookup"><span data-stu-id="88a92-108">This topic shows how tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="88a92-109">További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="88a92-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="88a92-110">Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="88a92-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="88a92-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="88a92-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="88a92-112">Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="88a92-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88a92-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="88a92-113">Prerequisites</span></span>
* [<span data-ttu-id="88a92-114">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="88a92-114">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="88a92-115">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="88a92-115">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="88a92-116">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="88a92-116">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="88a92-117">Hello hello legújabb verziójának telepítéséhez [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="88a92-117">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="88a92-118">Hello eszköz indítása, és csatlakozzon a tooyour AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="88a92-118">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="88a92-119">Tippek</span><span class="sxs-lookup"><span data-stu-id="88a92-119">Tips</span></span>
* <span data-ttu-id="88a92-120">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="88a92-120">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="88a92-121">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor streaming bitrates toodouble hello.</span><span class="sxs-lookup"><span data-stu-id="88a92-121">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="88a92-122">Ez nem kötelezők, ez segít csökkenteni hello hatását a hálózati torlódás.</span><span class="sxs-lookup"><span data-stu-id="88a92-122">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="88a92-123">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="88a92-123">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="88a92-124">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="88a92-124">Create a channel</span></span>
1. <span data-ttu-id="88a92-125">Hello AMSE eszköz, lépjen a toohello **Live** lapot, és hello csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="88a92-125">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="88a92-126">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="88a92-126">Select **Create channel…**</span></span> <span data-ttu-id="88a92-127">hello menüből.</span><span class="sxs-lookup"><span data-stu-id="88a92-127">from hello menu.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. <span data-ttu-id="88a92-129">Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="88a92-129">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="88a92-130">A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="88a92-130">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="88a92-131">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="88a92-131">You can leave all other settings as is.</span></span>

    <span data-ttu-id="88a92-132">Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="88a92-132">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="88a92-133">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="88a92-133">Click **Create Channel**.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> <span data-ttu-id="88a92-135">hello csatorna mindaddig toostart 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="88a92-135">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="88a92-136">Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span><span class="sxs-lookup"><span data-stu-id="88a92-136">While hello channel is starting you can [configure hello encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="88a92-137">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="88a92-137">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="88a92-138">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="88a92-138">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="88a92-139"><a id=configure_wirecast_rtmp></a>Hello Telestream Wirecast kódoló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="88a92-139"><a id=configure_wirecast_rtmp></a>Configure hello Telestream Wirecast encoder</span></span>
<span data-ttu-id="88a92-140">Az oktatóanyag hello következő kimeneti beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="88a92-140">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="88a92-141">Ez a szakasz többi hello részletes konfigurációs lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="88a92-141">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="88a92-142">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="88a92-142">**Video**:</span></span>

* <span data-ttu-id="88a92-143">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="88a92-143">Codec: H.264</span></span>
* <span data-ttu-id="88a92-144">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="88a92-144">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="88a92-145">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="88a92-145">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="88a92-146">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="88a92-146">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="88a92-147">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="88a92-147">Frame Rate: 30</span></span>

<span data-ttu-id="88a92-148">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="88a92-148">**Audio**:</span></span>

* <span data-ttu-id="88a92-149">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="88a92-149">Codec: AAC (LC)</span></span>
* <span data-ttu-id="88a92-150">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="88a92-150">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="88a92-151">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="88a92-151">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="88a92-152">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="88a92-152">Configuration steps</span></span>
1. <span data-ttu-id="88a92-153">Nyissa meg a hello Telestream Wirecast alkalmazás hello gép éppen használja, és állítsa be az RTMP adatfolyamként történő.</span><span class="sxs-lookup"><span data-stu-id="88a92-153">Open hello Telestream Wirecast application on hello machine being used, and set up for RTMP streaming.</span></span>
2. <span data-ttu-id="88a92-154">Hello kimeneti konfigurálása toohello navigálva **kimeneti** fülre, és kattintson **kimeneti beállításai...** .</span><span class="sxs-lookup"><span data-stu-id="88a92-154">Configure hello output by navigating toohello **Output** tab and selecting **Output Settings…**.</span></span>

    <span data-ttu-id="88a92-155">Győződjön meg arról, hogy hello **kimeneti cél** értéke túl**RTMP Server**.</span><span class="sxs-lookup"><span data-stu-id="88a92-155">Make sure hello **Output Destination** is set too**RTMP Server**.</span></span>
3. <span data-ttu-id="88a92-156">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="88a92-156">Click **OK**.</span></span>
4. <span data-ttu-id="88a92-157">Hello-beállítások lapján állítsa be a hello **cél** mező toobe **Azure Media Services**.</span><span class="sxs-lookup"><span data-stu-id="88a92-157">On hello settings page, set hello **Destination** field toobe **Azure Media Services**.</span></span>

    <span data-ttu-id="88a92-158">hello kódolás profil előre be van jelölve túl**Azure H.264 720 p 16:9 (1280 x 720)**.</span><span class="sxs-lookup"><span data-stu-id="88a92-158">hello Encoding profile is pre-selected too**Azure H.264 720p 16:9 (1280x720)**.</span></span> <span data-ttu-id="88a92-159">toocustomize ezeket a beállításokat, jelölje be hello fogaskerék ikonra toohello sarkában hello legördülő listán, és válassza a **új készletet**.</span><span class="sxs-lookup"><span data-stu-id="88a92-159">toocustomize these settings, select hello gear icon toohello right of hello drop down, and then choose **New Preset**.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. <span data-ttu-id="88a92-161">Kódoló készletek beállítása.</span><span class="sxs-lookup"><span data-stu-id="88a92-161">Configure encoder presets.</span></span>

    <span data-ttu-id="88a92-162">Név hello az adott néven beállítás, és ellenőrizze hello következő ajánlott beállítások:</span><span class="sxs-lookup"><span data-stu-id="88a92-162">Name hello preset, and check for hello following recommended settings:</span></span>

    <span data-ttu-id="88a92-163">**Videó**</span><span class="sxs-lookup"><span data-stu-id="88a92-163">**Video**</span></span>

   * <span data-ttu-id="88a92-164">Kódoló: MainConcept H.264</span><span class="sxs-lookup"><span data-stu-id="88a92-164">Encoder: MainConcept H.264</span></span>
   * <span data-ttu-id="88a92-165">A képkockák másodpercenkénti: 30</span><span class="sxs-lookup"><span data-stu-id="88a92-165">Frames per Second: 30</span></span>
   * <span data-ttu-id="88a92-166">Átlagos átviteli sebessége: 5000 kbit/s (módosítani lehet a hálózati korlátozás)</span><span class="sxs-lookup"><span data-stu-id="88a92-166">Average bit rate: 5000 kbits/sec (Can be adjusted based on network limitations)</span></span>
   * <span data-ttu-id="88a92-167">Profil: fő</span><span class="sxs-lookup"><span data-stu-id="88a92-167">Profile: Main</span></span>
   * <span data-ttu-id="88a92-168">Kulcskocka minden: 60 keretek</span><span class="sxs-lookup"><span data-stu-id="88a92-168">Key frame every: 60 frames</span></span>

    <span data-ttu-id="88a92-169">**Hang**</span><span class="sxs-lookup"><span data-stu-id="88a92-169">**Audio**</span></span>

   * <span data-ttu-id="88a92-170">Cél: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="88a92-170">Target bit rate: 192 kbits/sec</span></span>
   * <span data-ttu-id="88a92-171">Mintavételi gyakoriság: 44.100 kHz</span><span class="sxs-lookup"><span data-stu-id="88a92-171">Sample Rate: 44.100 kHz</span></span>

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. <span data-ttu-id="88a92-173">Nyomja le az **mentése**.</span><span class="sxs-lookup"><span data-stu-id="88a92-173">Press **Save**.</span></span>

    <span data-ttu-id="88a92-174">hello Encoding mező már kijelölésnél érhetők el az újonnan létrehozott hello-profil.</span><span class="sxs-lookup"><span data-stu-id="88a92-174">hello Encoding field now has hello newly created profile available for selection.</span></span>

    <span data-ttu-id="88a92-175">Győződjön meg arról, hogy ki van jelölve hello új profil.</span><span class="sxs-lookup"><span data-stu-id="88a92-175">Make sure hello new profile is selected.</span></span>
7. <span data-ttu-id="88a92-176">Hello csatorna bemeneti URL-cím beszerzése a rendelés tooassign azt toohello Wirecast **RTMP végpont**.</span><span class="sxs-lookup"><span data-stu-id="88a92-176">Get hello channel's input URL in order tooassign it toohello Wirecast **RTMP Endpoint**.</span></span>

    <span data-ttu-id="88a92-177">Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="88a92-177">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="88a92-178">Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="88a92-178">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="88a92-179">Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="88a92-179">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. <span data-ttu-id="88a92-181">A hello Wirecast **kimeneti beállításainak** ablakban illessze be ezeket az információkat a hello **cím** hello output szakasz, és rendelje hozzá egy adatfolyam neve mezőjében.</span><span class="sxs-lookup"><span data-stu-id="88a92-181">In hello Wirecast **Output Settings** window, paste this information in hello **Address** field of hello output section, and assign a stream name.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. <span data-ttu-id="88a92-183">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="88a92-183">Select **OK**.</span></span>
2. <span data-ttu-id="88a92-184">A fő hello **Wirecast** képernyőjén ellenőrizze a bemeneti forrás a videó és hang készen áll, majd nyomja le **adatfolyam** hello felső bal oldali sarokban.</span><span class="sxs-lookup"><span data-stu-id="88a92-184">On hello main **Wirecast** screen, confirm input sources for video and audio are ready and then hit **Stream** in hello top left hand corner.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> <span data-ttu-id="88a92-186">Kattintás előtt **adatfolyam**, hogy **kell** győződjön meg arról, hogy készen áll-e hello csatorna.</span><span class="sxs-lookup"><span data-stu-id="88a92-186">Before you click **Stream**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="88a92-187">Győződjön meg arról is, nem tooleave hello egy bemeneti hozzájárulás nélkül üzemkész állapotban csatorna hírcsatorna > 15 percnél hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="88a92-187">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="88a92-188">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="88a92-188">Test playback</span></span>

<span data-ttu-id="88a92-189">Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve.</span><span class="sxs-lookup"><span data-stu-id="88a92-189">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="88a92-190">Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="88a92-190">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

<span data-ttu-id="88a92-191">Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.</span><span class="sxs-lookup"><span data-stu-id="88a92-191">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="88a92-192">Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="88a92-192">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="88a92-193">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="88a92-193">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="88a92-194">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="88a92-194">Create a program</span></span>
1. <span data-ttu-id="88a92-195">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="88a92-195">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="88a92-196">A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="88a92-196">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. <span data-ttu-id="88a92-198">Hello program neve, és ha szükséges, módosítsa a hello **az archiválási időtartam** (mely alapértelmezett too4 óra).</span><span class="sxs-lookup"><span data-stu-id="88a92-198">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="88a92-199">Adjon meg olyan tárolási helyen vagy hagyja hello alapértelmezett is.</span><span class="sxs-lookup"><span data-stu-id="88a92-199">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="88a92-200">Ellenőrizze a hello **Start hello Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="88a92-200">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="88a92-201">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="88a92-201">Click **Create Program**.</span></span>  

   >[!NOTE]
   ><span data-ttu-id="88a92-202">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="88a92-202">Program creation takes less time than channel creation.</span></span>
       
5. <span data-ttu-id="88a92-203">Ha hello program fut, erősítse meg a lejátszás hello program jobb gombbal kattint rá, és a Navigálás túl**lejátszás hello programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="88a92-203">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="88a92-204">Ha megerősítette, hello program ismét kattintson jobb gombbal, majd válassza ki **hello kimeneti URL-cím tooClipboard másolja** (vagy az adatok lekérését hello **Program információk és beállítások** hello menüből beállítás).</span><span class="sxs-lookup"><span data-stu-id="88a92-204">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="88a92-205">hello adatfolyama most már készen áll a player ágyazott toobe, és az elosztott tooan célközönség élő megtekintése.</span><span class="sxs-lookup"><span data-stu-id="88a92-205">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="88a92-206">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="88a92-206">Troubleshooting</span></span>
<span data-ttu-id="88a92-207">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="88a92-207">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="88a92-208">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="88a92-208">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="88a92-209">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="88a92-209">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
