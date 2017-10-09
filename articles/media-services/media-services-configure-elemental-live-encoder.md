---
title: "aaaConfigure hello elemi élő kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello elemi élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 9a5de6189bfb123768a9da038b8c8db69cf85e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-elemental-live-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="84105-103">Hello elemi élő kódoló toosend egyféle sávszélességű élő adatfolyamot használata</span><span class="sxs-lookup"><span data-stu-id="84105-103">Use hello Elemental Live encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="84105-104">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="84105-104">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="84105-105">Tricaster</span><span class="sxs-lookup"><span data-stu-id="84105-105">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="84105-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="84105-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="84105-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="84105-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="84105-108">Ez a témakör bemutatja, hogyan tooconfigure hello [elemi élő](http://www.elementaltechnologies.com/products/elemental-live) kódoló toosend egyszeres sávszélességű adatfolyam-tooAMS élő kódolásra képes csatornák.</span><span class="sxs-lookup"><span data-stu-id="84105-108">This topic shows how tooconfigure hello [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="84105-109">További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="84105-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="84105-110">Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="84105-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="84105-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="84105-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="84105-112">Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="84105-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84105-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84105-113">Prerequisites</span></span>
* <span data-ttu-id="84105-114">Rendelkeznie kell egy ismeretét elemi élő webes felület toocreate élő eseményeket használ.</span><span class="sxs-lookup"><span data-stu-id="84105-114">Must have a working knowledge of using Elemental Live web interface toocreate live events.</span></span>
* [<span data-ttu-id="84105-115">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="84105-115">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="84105-116">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="84105-116">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="84105-117">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="84105-117">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>
* <span data-ttu-id="84105-118">Hello hello legújabb verziójának telepítéséhez [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="84105-118">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="84105-119">Hello eszköz indítása, és csatlakozzon a tooyour AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="84105-119">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="84105-120">Tippek</span><span class="sxs-lookup"><span data-stu-id="84105-120">Tips</span></span>
* <span data-ttu-id="84105-121">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="84105-121">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="84105-122">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor streaming bitrates toodouble hello.</span><span class="sxs-lookup"><span data-stu-id="84105-122">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="84105-123">Ez nem kötelezők, ez segít csökkenteni hello hatását a hálózati torlódás.</span><span class="sxs-lookup"><span data-stu-id="84105-123">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="84105-124">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="84105-124">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="elemental-live-with-rtp-ingest"></a><span data-ttu-id="84105-125">A RTP elemi Live betöltési</span><span class="sxs-lookup"><span data-stu-id="84105-125">Elemental Live with RTP ingest</span></span>
<span data-ttu-id="84105-126">Ez a szakasz bemutatja, hogyan tooconfigure hello elemi élő kódoló által küldött egy egyféle sávszélességű élő keresztül RTP adatfolyamot.</span><span class="sxs-lookup"><span data-stu-id="84105-126">This section shows how tooconfigure hello Elemental Live encoder that sends a single bitrate live stream over RTP.</span></span>  <span data-ttu-id="84105-127">További információkért lásd: [MPEG-TS stream RTP keresztül](media-services-manage-live-encoder-enabled-channels.md#channel).</span><span class="sxs-lookup"><span data-stu-id="84105-127">For more information, see [MPEG-TS stream over RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span></span>

### <a name="create-a-channel"></a><span data-ttu-id="84105-128">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="84105-128">Create a channel</span></span>

1. <span data-ttu-id="84105-129">Hello AMSE eszköz, lépjen a toohello **Live** lapot, és hello csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="84105-129">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="84105-130">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="84105-130">Select **Create channel…**</span></span> <span data-ttu-id="84105-131">hello menüből.</span><span class="sxs-lookup"><span data-stu-id="84105-131">from hello menu.</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. <span data-ttu-id="84105-133">Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="84105-133">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="84105-134">A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTP (MPEG-TS)**.</span><span class="sxs-lookup"><span data-stu-id="84105-134">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTP (MPEG-TS)**.</span></span> <span data-ttu-id="84105-135">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="84105-135">You can leave all other settings as is.</span></span>

    <span data-ttu-id="84105-136">Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="84105-136">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="84105-137">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="84105-137">Click **Create Channel**.</span></span>

   ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> <span data-ttu-id="84105-139">hello csatorna mindaddig toostart 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="84105-139">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="84105-140">Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span><span class="sxs-lookup"><span data-stu-id="84105-140">While hello channel is starting you can [configure hello encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84105-141">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="84105-141">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="84105-142">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="84105-142">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

### <span data-ttu-id="84105-143"><a id=configure_elemental_rtp></a>Hello elemi élő kódoló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84105-143"><a id=configure_elemental_rtp></a>Configure hello Elemental Live encoder</span></span>
<span data-ttu-id="84105-144">Az oktatóanyag hello következő kimeneti beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="84105-144">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="84105-145">Ez a szakasz többi hello részletes konfigurációs lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="84105-145">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="84105-146">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="84105-146">**Video**:</span></span>

* <span data-ttu-id="84105-147">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="84105-147">Codec: H.264</span></span>
* <span data-ttu-id="84105-148">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="84105-148">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="84105-149">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="84105-149">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="84105-150">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="84105-150">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="84105-151">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="84105-151">Frame Rate: 30</span></span>

<span data-ttu-id="84105-152">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="84105-152">**Audio**:</span></span>

* <span data-ttu-id="84105-153">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="84105-153">Codec: AAC (LC)</span></span>
* <span data-ttu-id="84105-154">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="84105-154">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="84105-155">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="84105-155">Sample Rate: 44.1 kHz</span></span>

#### <a name="configuration-steps"></a><span data-ttu-id="84105-156">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="84105-156">Configuration steps</span></span>
1. <span data-ttu-id="84105-157">Keresse meg a toohello **elemi élő** webes felület, és állítsa be a hello kódoló **UDP/TS** streaming.</span><span class="sxs-lookup"><span data-stu-id="84105-157">Navigate toohello **Elemental Live** web interface, and set up hello encoder for **UDP/TS** streaming.</span></span>
2. <span data-ttu-id="84105-158">Új esemény létrehozása után görgessen lefelé toohello kimeneti csoportok, és adja hozzá a hello **UDP/TS** kimeneti csoport.</span><span class="sxs-lookup"><span data-stu-id="84105-158">Once a new event is created, scroll down toohello output groups and add hello **UDP/TS** output group.</span></span>
3. <span data-ttu-id="84105-159">Hozzon létre egy új kimeneti kiválasztásával **új adatfolyam** , majd **hozzáadása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="84105-159">Create a new output by selecting **New Stream** and then clicking **Add Output**.</span></span>  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > <span data-ttu-id="84105-161">Ajánlott, hogy hello elemi esemény hello időkód túl beállítása "Rendszeróra" toohelp hello kódoló újracsatlakozás hello esetben, ha az adatfolyam nem.</span><span class="sxs-lookup"><span data-stu-id="84105-161">It is recommended that hello Elemental event has hello timecode set too"System Clock" toohelp hello encoder reconnect in hello case of a stream failure.</span></span>
   >
   >
4. <span data-ttu-id="84105-162">Most, hogy hello kimeneti létrehozása után kattintson **adatfolyam adható hozzá**.</span><span class="sxs-lookup"><span data-stu-id="84105-162">Now that hello Output has been created, click **Add Stream**.</span></span> <span data-ttu-id="84105-163">hello kimeneti beállításait mostantól konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="84105-163">hello output settings can now be configured.</span></span>
5. <span data-ttu-id="84105-164">Görgessen lefelé toohello "Stream 1", amely éppen most hozták létre, kattintson a hello **videó** hello bal oldali lapon, és bontsa ki a hello **speciális** beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="84105-164">Scroll down toohello "Stream 1" that was just created, click hello **Video** tab on hello left and expand hello **Advanced** settings section.</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    <span data-ttu-id="84105-166">Míg elemi élő számos elérhető testreszabása, hello következő beállítások használata ajánlott streaming tooAMS való ismerkedésre.</span><span class="sxs-lookup"><span data-stu-id="84105-166">While Elemental Live has a wide range of available customizing, hello following settings are recommended for getting started with streaming tooAMS.</span></span>

   * <span data-ttu-id="84105-167">Megoldás: 1280 x 720</span><span class="sxs-lookup"><span data-stu-id="84105-167">Resolution: 1280 x 720</span></span>
   * <span data-ttu-id="84105-168">Képkockasebesség: 30</span><span class="sxs-lookup"><span data-stu-id="84105-168">Framerate: 30</span></span>
   * <span data-ttu-id="84105-169">GOP mérete: 60 keretek</span><span class="sxs-lookup"><span data-stu-id="84105-169">GOP Size: 60 frames</span></span>
   * <span data-ttu-id="84105-170">Váltott mód: fokozatos</span><span class="sxs-lookup"><span data-stu-id="84105-170">Interlace Mode: Progressive</span></span>
   * <span data-ttu-id="84105-171">Átviteli sebesség: 5000000 bit/mp (ezt úgy kell beállítani a hálózati korlátozás)</span><span class="sxs-lookup"><span data-stu-id="84105-171">Bitrate: 5000000 bit/s (This can be adjusted based on network limitations)</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. <span data-ttu-id="84105-173">Hello csatorna bemeneti URL-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="84105-173">Get hello channel's input URL.</span></span>

    <span data-ttu-id="84105-174">Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="84105-174">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="84105-175">Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="84105-175">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="84105-176">Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="84105-176">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. <span data-ttu-id="84105-178">Illessze be ezeket az információkat a hello **elsődleges cél** hello elemi mezőjében.</span><span class="sxs-lookup"><span data-stu-id="84105-178">Paste this information in hello **Primary Destination** field of hello Elemental.</span></span> <span data-ttu-id="84105-179">A többi beállítás hello alapértelmezett maradjanak.</span><span class="sxs-lookup"><span data-stu-id="84105-179">All other settings can remain hello default.</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    <span data-ttu-id="84105-181">További redundancia érdekében ismételje meg ezeket a lépéseket hello másodlagos bemeneti URL-cím UDP/TS Streaming hozzon létre egy külön "Kimeneti" lapon.</span><span class="sxs-lookup"><span data-stu-id="84105-181">For extra redundancy, repeat these steps with hello Secondary Input URL by creating a separate "Output" tab for UDP/TS Streaming.</span></span>
3. <span data-ttu-id="84105-182">Kattintson a **létrehozása** (Ha új esemény létrehozása) vagy **frissítés** (Ha egy már meglévő esemény szerkesztése) és folytathatja a toostart hello kódoló.</span><span class="sxs-lookup"><span data-stu-id="84105-182">Click **Create** (if a new event was created) or **Update** (if editing a pre-existing event) and then proceed toostart hello encoder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84105-183">Végül kattintson **Start** a hello elemi élő webes felület, akkor **kell** győződjön meg arról, hogy készen áll-e hello csatorna.</span><span class="sxs-lookup"><span data-stu-id="84105-183">Before you click **Start** on hello Elemental Live web interface, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="84105-184">Győződjön meg arról is, nem tooleave hello csatorna nélkül > 15 percnél hosszabb ideig esemény üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="84105-184">Also, make sure not tooleave hello Channel in a ready state without an event for longer than > 15 minutes.</span></span>
>
>

<span data-ttu-id="84105-185">Után hello adatfolyam 30 másodpercig fut, nyissa meg a visszafelé toohello AMSE eszköz és a vizsgálati lejátszás.</span><span class="sxs-lookup"><span data-stu-id="84105-185">After hello stream has been running for 30 seconds, navigate back toohello AMSE tool and test playback.</span></span>  

### <a name="test-playback"></a><span data-ttu-id="84105-186">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="84105-186">Test playback</span></span>

<span data-ttu-id="84105-187">Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve.</span><span class="sxs-lookup"><span data-stu-id="84105-187">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="84105-188">Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="84105-188">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

<span data-ttu-id="84105-189">Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.</span><span class="sxs-lookup"><span data-stu-id="84105-189">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="84105-190">Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="84105-190">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="84105-191">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="84105-191">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>   

### <a name="create-a-program"></a><span data-ttu-id="84105-192">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="84105-192">Create a program</span></span>
1. <span data-ttu-id="84105-193">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="84105-193">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="84105-194">A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="84105-194">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. <span data-ttu-id="84105-196">Hello program neve, és ha szükséges, módosítsa a hello **az archiválási időtartam** (mely alapértelmezett too4 óra).</span><span class="sxs-lookup"><span data-stu-id="84105-196">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="84105-197">Adjon meg olyan tárolási helyen vagy hagyja hello alapértelmezett is.</span><span class="sxs-lookup"><span data-stu-id="84105-197">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="84105-198">Ellenőrizze a hello **Start hello Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="84105-198">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="84105-199">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="84105-199">Click **Create Program**.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="84105-200">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84105-200">Program creation takes less time than channel creation.</span></span>   
      
5. <span data-ttu-id="84105-201">Ha hello program fut, erősítse meg a lejátszás hello program jobb gombbal kattint rá, és a Navigálás túl**lejátszás hello programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="84105-201">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="84105-202">Ha megerősítette, hello program ismét kattintson jobb gombbal, majd válassza ki **hello kimeneti URL-cím tooClipboard másolja** (vagy az adatok lekérését hello **Program információk és beállítások** hello menüből beállítás).</span><span class="sxs-lookup"><span data-stu-id="84105-202">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="84105-203">hello adatfolyama most már készen áll a player ágyazott toobe, és az elosztott tooan célközönség élő megtekintése.</span><span class="sxs-lookup"><span data-stu-id="84105-203">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="84105-204">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="84105-204">Troubleshooting</span></span>
<span data-ttu-id="84105-205">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="84105-205">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="84105-206">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="84105-206">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="84105-207">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="84105-207">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
