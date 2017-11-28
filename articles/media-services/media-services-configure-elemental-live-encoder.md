---
title: "A elemi élő kódoló egy egyféle sávszélességű élő adatfolyamot küldeni |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan AMS élő kódolásra képes csatornák egyféle sávszélességű adatfolyamot küldeni a elemi élő kódoló."
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
ms.openlocfilehash: 668a3ab46a70c0ee25fa87031d27c0f4333ec89c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="de965-103">Használja a elemi élő kódoló egy egyféle sávszélességű élő adatfolyamot küldeni</span><span class="sxs-lookup"><span data-stu-id="de965-103">Use the Elemental Live encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="de965-104">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="de965-104">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="de965-105">Tricaster</span><span class="sxs-lookup"><span data-stu-id="de965-105">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="de965-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="de965-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="de965-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="de965-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="de965-108">Ez a témakör ismerteti, hogyan konfigurálható a [elemi élő](http://www.elementaltechnologies.com/products/elemental-live) kódoló egy egyfajta sávszélességű adatfolyamot AMS-csatorna is küldhet a valós idejű kódolásra engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="de965-108">This topic shows how to configure the [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="de965-109">További információk: [Az Azure Media Services segítségével élő kódolásra képes csatornák használata](media-services-manage-live-encoder-enabled-channels.md)</span><span class="sxs-lookup"><span data-stu-id="de965-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="de965-110">Ez az oktatóanyag bemutatja, hogyan kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="de965-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="de965-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="de965-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="de965-112">Ha Mac vagy Linux, létrehozásához használja az Azure-portálon [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="de965-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de965-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de965-113">Prerequisites</span></span>
* <span data-ttu-id="de965-114">Élő események létrehozása elemi élő webes felülete segítségével ismeretét kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="de965-114">Must have a working knowledge of using Elemental Live web interface to create live events.</span></span>
* [<span data-ttu-id="de965-115">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="de965-115">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="de965-116">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="de965-116">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="de965-117">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="de965-117">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>
* <span data-ttu-id="de965-118">Telepítse a legújabb verzióját a [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="de965-118">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="de965-119">Indítsa el az eszközt, és csatlakozzon az AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="de965-119">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="de965-120">Tippek</span><span class="sxs-lookup"><span data-stu-id="de965-120">Tips</span></span>
* <span data-ttu-id="de965-121">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="de965-121">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="de965-122">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor, hogy az adatfolyam-továbbítási bitrates duplán.</span><span class="sxs-lookup"><span data-stu-id="de965-122">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="de965-123">Ez nem kötelezők, ez segít csökkenteni a hálózati torlódás hatását.</span><span class="sxs-lookup"><span data-stu-id="de965-123">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="de965-124">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="de965-124">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="elemental-live-with-rtp-ingest"></a><span data-ttu-id="de965-125">A RTP elemi Live betöltési</span><span class="sxs-lookup"><span data-stu-id="de965-125">Elemental Live with RTP ingest</span></span>
<span data-ttu-id="de965-126">Ez a szakasz bemutatja, hogyan a elemi élő kódoló egy egyféle sávszélességű élő adatfolyamot küldő RTP keresztül.</span><span class="sxs-lookup"><span data-stu-id="de965-126">This section shows how to configure the Elemental Live encoder that sends a single bitrate live stream over RTP.</span></span>  <span data-ttu-id="de965-127">További információkért lásd: [MPEG-TS stream RTP keresztül](media-services-manage-live-encoder-enabled-channels.md#channel).</span><span class="sxs-lookup"><span data-stu-id="de965-127">For more information, see [MPEG-TS stream over RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span></span>

### <a name="create-a-channel"></a><span data-ttu-id="de965-128">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="de965-128">Create a channel</span></span>

1. <span data-ttu-id="de965-129">Az AMSE eszköz navigáljon a **Live** lapot, és a csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="de965-129">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="de965-130">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="de965-130">Select **Create channel…**</span></span> <span data-ttu-id="de965-131">a menüből.</span><span class="sxs-lookup"><span data-stu-id="de965-131">from the menu.</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. <span data-ttu-id="de965-133">Adjon meg egy csatorna nevét, a Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="de965-133">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="de965-134">A csatorna beállítások területen válassza a **szabványos** az élő kódolás lehetőségnél a bemeneti protokoll beállítása **RTP (MPEG-TS)**.</span><span class="sxs-lookup"><span data-stu-id="de965-134">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTP (MPEG-TS)**.</span></span> <span data-ttu-id="de965-135">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="de965-135">You can leave all other settings as is.</span></span>

    <span data-ttu-id="de965-136">Győződjön meg arról, hogy a **most indítsa el az új csatorna** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="de965-136">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="de965-137">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="de965-137">Click **Create Channel**.</span></span>

   ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> <span data-ttu-id="de965-139">A csatorna mindaddig elindításához 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="de965-139">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="de965-140">A csatorna indítása közben is [a kódoló](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span><span class="sxs-lookup"><span data-stu-id="de965-140">While the channel is starting you can [configure the encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="de965-141">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="de965-141">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="de965-142">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="de965-142">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

### <span data-ttu-id="de965-143"><a id=configure_elemental_rtp></a>A elemi élő kódoló</span><span class="sxs-lookup"><span data-stu-id="de965-143"><a id=configure_elemental_rtp></a>Configure the Elemental Live encoder</span></span>
<span data-ttu-id="de965-144">Ebben az oktatóanyagban a következő kimeneti beállításokat használják.</span><span class="sxs-lookup"><span data-stu-id="de965-144">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="de965-145">Ez a szakasz a további konfigurációs lépések részletesebben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="de965-145">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="de965-146">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="de965-146">**Video**:</span></span>

* <span data-ttu-id="de965-147">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="de965-147">Codec: H.264</span></span>
* <span data-ttu-id="de965-148">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="de965-148">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="de965-149">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="de965-149">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="de965-150">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="de965-150">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="de965-151">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="de965-151">Frame Rate: 30</span></span>

<span data-ttu-id="de965-152">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="de965-152">**Audio**:</span></span>

* <span data-ttu-id="de965-153">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="de965-153">Codec: AAC (LC)</span></span>
* <span data-ttu-id="de965-154">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="de965-154">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="de965-155">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="de965-155">Sample Rate: 44.1 kHz</span></span>

#### <a name="configuration-steps"></a><span data-ttu-id="de965-156">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="de965-156">Configuration steps</span></span>
1. <span data-ttu-id="de965-157">Keresse meg a **elemi élő** webes felület, és állítsa be a kódoló **UDP/TS** streaming.</span><span class="sxs-lookup"><span data-stu-id="de965-157">Navigate to the **Elemental Live** web interface, and set up the encoder for **UDP/TS** streaming.</span></span>
2. <span data-ttu-id="de965-158">Új esemény létrehozása után görgessen le a kimeneti csoportokat, és adja hozzá a **UDP/TS** kimeneti csoport.</span><span class="sxs-lookup"><span data-stu-id="de965-158">Once a new event is created, scroll down to the output groups and add the **UDP/TS** output group.</span></span>
3. <span data-ttu-id="de965-159">Hozzon létre egy új kimeneti kiválasztásával **új adatfolyam** , majd **hozzáadása kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="de965-159">Create a new output by selecting **New Stream** and then clicking **Add Output**.</span></span>  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > <span data-ttu-id="de965-161">Javasoljuk, hogy rendelkezik-e a "Rendszeróra" értékre a kódoló adatfolyam hiba esetén újra segítségével időkód elemi esemény.</span><span class="sxs-lookup"><span data-stu-id="de965-161">It is recommended that the Elemental event has the timecode set to "System Clock" to help the encoder reconnect in the case of a stream failure.</span></span>
   >
   >
4. <span data-ttu-id="de965-162">Most, hogy a kimenet létrehozása után kattintson **adatfolyam adható hozzá**.</span><span class="sxs-lookup"><span data-stu-id="de965-162">Now that the Output has been created, click **Add Stream**.</span></span> <span data-ttu-id="de965-163">A kimeneti beállításait mostantól konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="de965-163">The output settings can now be configured.</span></span>
5. <span data-ttu-id="de965-164">Görgessen le a "Stream 1", amely éppen most hozták létre, kattintson a **videó** a bal oldali lapon, és bontsa ki a **speciális** beállítások szakaszban.</span><span class="sxs-lookup"><span data-stu-id="de965-164">Scroll down to the "Stream 1" that was just created, click the **Video** tab on the left and expand the **Advanced** settings section.</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    <span data-ttu-id="de965-166">Míg elemi élő elérhető testreszabása számos, a következő beállítások ajánlott első lépések az AMS streaming.</span><span class="sxs-lookup"><span data-stu-id="de965-166">While Elemental Live has a wide range of available customizing, the following settings are recommended for getting started with streaming to AMS.</span></span>

   * <span data-ttu-id="de965-167">Megoldás: 1280 x 720</span><span class="sxs-lookup"><span data-stu-id="de965-167">Resolution: 1280 x 720</span></span>
   * <span data-ttu-id="de965-168">Képkockasebesség: 30</span><span class="sxs-lookup"><span data-stu-id="de965-168">Framerate: 30</span></span>
   * <span data-ttu-id="de965-169">GOP mérete: 60 keretek</span><span class="sxs-lookup"><span data-stu-id="de965-169">GOP Size: 60 frames</span></span>
   * <span data-ttu-id="de965-170">Váltott mód: fokozatos</span><span class="sxs-lookup"><span data-stu-id="de965-170">Interlace Mode: Progressive</span></span>
   * <span data-ttu-id="de965-171">Átviteli sebesség: 5000000 bit/mp (ezt úgy kell beállítani a hálózati korlátozás)</span><span class="sxs-lookup"><span data-stu-id="de965-171">Bitrate: 5000000 bit/s (This can be adjusted based on network limitations)</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. <span data-ttu-id="de965-173">A csatorna bemeneti URL-cím beszerzése.</span><span class="sxs-lookup"><span data-stu-id="de965-173">Get the channel's input URL.</span></span>

    <span data-ttu-id="de965-174">Lépjen vissza az AMSE eszköz, és a csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="de965-174">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="de965-175">Miután állapota megváltozott, a **indítása** való **futtató**, kaphat a bemeneti URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="de965-175">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="de965-176">Ha a csatorna fut, kattintson a jobb gombbal a csatorna neve, felett az egérmutatót navigáljon **vágólapra másolás bemeneti URL-cím** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="de965-176">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. <span data-ttu-id="de965-178">Illessze be ezt az információt a **elsődleges cél** elemi mezőjében.</span><span class="sxs-lookup"><span data-stu-id="de965-178">Paste this information in the **Primary Destination** field of the Elemental.</span></span> <span data-ttu-id="de965-179">A többi beállítás az alapértelmezett maradjanak.</span><span class="sxs-lookup"><span data-stu-id="de965-179">All other settings can remain the default.</span></span>

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    <span data-ttu-id="de965-181">További redundancia ismételje meg ezeket a lépéseket, a másodlagos bemeneti URL-címet a UDP/TS Streaming hozzon létre egy külön "Kimeneti" lapon.</span><span class="sxs-lookup"><span data-stu-id="de965-181">For extra redundancy, repeat these steps with the Secondary Input URL by creating a separate "Output" tab for UDP/TS Streaming.</span></span>
3. <span data-ttu-id="de965-182">Kattintson a **létrehozása** (Ha új esemény létrehozása) vagy **frissítés** (Ha egy már meglévő esemény szerkesztése) és folytathatja a kódoló elindításához.</span><span class="sxs-lookup"><span data-stu-id="de965-182">Click **Create** (if a new event was created) or **Update** (if editing a pre-existing event) and then proceed to start the encoder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="de965-183">Végül kattintson **Start** a elemi élő webes felülete, **kell** győződjön meg arról, hogy készen áll-e a csatorna.</span><span class="sxs-lookup"><span data-stu-id="de965-183">Before you click **Start** on the Elemental Live web interface, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="de965-184">Bizonyosodjon meg róla, hogy ne maradjon a csatorna egy esemény > 15 percnél hosszabb ideig nem üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="de965-184">Also, make sure not to leave the Channel in a ready state without an event for longer than > 15 minutes.</span></span>
>
>

<span data-ttu-id="de965-185">Miután az adatfolyam 30 másodpercig fut, lépjen vissza az AMSE eszköz és a vizsgálati lejátszás.</span><span class="sxs-lookup"><span data-stu-id="de965-185">After the stream has been running for 30 seconds, navigate back to the AMSE tool and test playback.</span></span>  

### <a name="test-playback"></a><span data-ttu-id="de965-186">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="de965-186">Test playback</span></span>

<span data-ttu-id="de965-187">Keresse meg az AMSE eszköz, és kattintson a jobb gombbal a csatornát kell tesztelni.</span><span class="sxs-lookup"><span data-stu-id="de965-187">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="de965-188">A menüben rámutat **lejátszás az előzetes** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="de965-188">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

<span data-ttu-id="de965-189">Ha az adatfolyam a Windows Media player jelenik meg, majd a kódoló megfelelően van konfigurálva AMS való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="de965-189">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="de965-190">Ha hibaüzenetet kap, a csatorna le kell állítani, és kódoló beállításai módosul.</span><span class="sxs-lookup"><span data-stu-id="de965-190">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="de965-191">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="de965-191">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>   

### <a name="create-a-program"></a><span data-ttu-id="de965-192">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="de965-192">Create a program</span></span>
1. <span data-ttu-id="de965-193">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="de965-193">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="de965-194">Az a **Live** az AMSE eszköz lapján, a program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="de965-194">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![Elemi](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. <span data-ttu-id="de965-196">A program neve, és szükség esetén módosítsa a **az archiválási időtartam** (amely alapértelmezés szerint 4 óra).</span><span class="sxs-lookup"><span data-stu-id="de965-196">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="de965-197">Adja meg a tárolási helyet is, vagy hagyja meg az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="de965-197">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="de965-198">Ellenőrizze a **indítsa el a Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="de965-198">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="de965-199">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="de965-199">Click **Create Program**.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="de965-200">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="de965-200">Program creation takes less time than channel creation.</span></span>   
      
5. <span data-ttu-id="de965-201">Ha a program fut, erősítse meg a lejátszás jobb gombbal kattint rá a program, és lépjen az **lejátszás a programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="de965-201">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="de965-202">Ha megerősítette, kattintson a jobb gombbal a program újra, és válassza ki **a kimeneti URL-Címének másolása a vágólapra** (vagy az adatok lekérésére a **Program információk és beállítások** lehetőséget a menüből).</span><span class="sxs-lookup"><span data-stu-id="de965-202">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="de965-203">Az adatfolyam egy Player beágyazott, vagy olyan célközönségnek juttathatja el élő megtekintésre elosztott készen áll.</span><span class="sxs-lookup"><span data-stu-id="de965-203">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="de965-204">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="de965-204">Troubleshooting</span></span>
<span data-ttu-id="de965-205">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="de965-205">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="de965-206">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="de965-206">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="de965-207">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="de965-207">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
