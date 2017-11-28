---
title: "A Telestream Wirecast kódoló egyféle sávszélességű élő adatfolyamot küldeni |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan konfigurálhatja a Wirecast élő kódoló egy egyféle sávszélességű adatfolyamot küldeni AMS élő kódolásra képes csatornák. "
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
ms.openlocfilehash: c4df14f24650ce431dfb31cc774cab6d3cf3aef0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="5fc3d-103">Használja a Wirecast kódoló egyféle sávszélességű élő adatfolyamot küldeni</span><span class="sxs-lookup"><span data-stu-id="5fc3d-103">Use the Wirecast encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5fc3d-104">Wirecast</span><span class="sxs-lookup"><span data-stu-id="5fc3d-104">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="5fc3d-105">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="5fc3d-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="5fc3d-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="5fc3d-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="5fc3d-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="5fc3d-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="5fc3d-108">Ez a témakör ismerteti, hogyan konfigurálható a [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) élő kódoló egy egyfajta sávszélességű adatfolyamot AMS-csatorna is küldhet a valós idejű kódolásra engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-108">This topic shows how to configure the [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="5fc3d-109">További információk: [Az Azure Media Services segítségével élő kódolásra képes csatornák használata](media-services-manage-live-encoder-enabled-channels.md)</span><span class="sxs-lookup"><span data-stu-id="5fc3d-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="5fc3d-110">Ez az oktatóanyag bemutatja, hogyan kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="5fc3d-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="5fc3d-112">Ha Mac vagy Linux, létrehozásához használja az Azure-portálon [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="5fc3d-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5fc3d-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5fc3d-113">Prerequisites</span></span>
* [<span data-ttu-id="5fc3d-114">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5fc3d-114">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="5fc3d-115">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-115">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="5fc3d-116">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="5fc3d-116">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="5fc3d-117">Telepítse a legújabb verzióját a [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-117">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="5fc3d-118">Indítsa el az eszközt, és csatlakozzon az AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-118">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="5fc3d-119">Tippek</span><span class="sxs-lookup"><span data-stu-id="5fc3d-119">Tips</span></span>
* <span data-ttu-id="5fc3d-120">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-120">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="5fc3d-121">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor, hogy az adatfolyam-továbbítási bitrates duplán.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-121">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="5fc3d-122">Ez nem kötelezők, ez segít csökkenteni a hálózati torlódás hatását.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-122">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="5fc3d-123">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-123">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="5fc3d-124">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="5fc3d-124">Create a channel</span></span>
1. <span data-ttu-id="5fc3d-125">Az AMSE eszköz navigáljon a **Live** lapot, és a csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-125">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="5fc3d-126">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="5fc3d-126">Select **Create channel…**</span></span> <span data-ttu-id="5fc3d-127">a menüből.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-127">from the menu.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. <span data-ttu-id="5fc3d-129">Adjon meg egy csatorna nevét, a Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-129">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="5fc3d-130">A csatorna beállítások területen válassza a **szabványos** az élő kódolás lehetőségnél a bemeneti protokoll beállítása **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-130">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="5fc3d-131">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-131">You can leave all other settings as is.</span></span>

    <span data-ttu-id="5fc3d-132">Győződjön meg arról, hogy a **most indítsa el az új csatorna** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-132">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="5fc3d-133">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-133">Click **Create Channel**.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> <span data-ttu-id="5fc3d-135">A csatorna mindaddig elindításához 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-135">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="5fc3d-136">A csatorna indítása közben is [a kódoló](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span><span class="sxs-lookup"><span data-stu-id="5fc3d-136">While the channel is starting you can [configure the encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5fc3d-137">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-137">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="5fc3d-138">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="5fc3d-138">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="5fc3d-139"><a id=configure_wirecast_rtmp></a>A Telestream Wirecast kódoló</span><span class="sxs-lookup"><span data-stu-id="5fc3d-139"><a id=configure_wirecast_rtmp></a>Configure the Telestream Wirecast encoder</span></span>
<span data-ttu-id="5fc3d-140">Ebben az oktatóanyagban a következő kimeneti beállításokat használják.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-140">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="5fc3d-141">Ez a szakasz a további konfigurációs lépések részletesebben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-141">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="5fc3d-142">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="5fc3d-142">**Video**:</span></span>

* <span data-ttu-id="5fc3d-143">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="5fc3d-143">Codec: H.264</span></span>
* <span data-ttu-id="5fc3d-144">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="5fc3d-144">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="5fc3d-145">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="5fc3d-145">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="5fc3d-146">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="5fc3d-146">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="5fc3d-147">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="5fc3d-147">Frame Rate: 30</span></span>

<span data-ttu-id="5fc3d-148">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="5fc3d-148">**Audio**:</span></span>

* <span data-ttu-id="5fc3d-149">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="5fc3d-149">Codec: AAC (LC)</span></span>
* <span data-ttu-id="5fc3d-150">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="5fc3d-150">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="5fc3d-151">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="5fc3d-151">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="5fc3d-152">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="5fc3d-152">Configuration steps</span></span>
1. <span data-ttu-id="5fc3d-153">Indítsa el a Telestream Wirecast alkalmazást azon a gépen használt, és állítsa be az RTMP adatfolyamként történő.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-153">Open the Telestream Wirecast application on the machine being used, and set up for RTMP streaming.</span></span>
2. <span data-ttu-id="5fc3d-154">Konfigurálja a kimeneti navigáljon a **kimeneti** fülre, és kattintson **kimeneti beállításai...** .</span><span class="sxs-lookup"><span data-stu-id="5fc3d-154">Configure the output by navigating to the **Output** tab and selecting **Output Settings…**.</span></span>

    <span data-ttu-id="5fc3d-155">Győződjön meg arról, hogy a **kimeneti cél** értéke **RTMP Server**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-155">Make sure the **Output Destination** is set to **RTMP Server**.</span></span>
3. <span data-ttu-id="5fc3d-156">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-156">Click **OK**.</span></span>
4. <span data-ttu-id="5fc3d-157">A beállítások lapon állítsa be a **cél** mezőre, amely **Azure Media Services**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-157">On the settings page, set the **Destination** field to be **Azure Media Services**.</span></span>

    <span data-ttu-id="5fc3d-158">A kódolás profil csak az előre kiválasztott **Azure H.264 720 p 16:9 (1280 x 720)**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-158">The Encoding profile is pre-selected to **Azure H.264 720p 16:9 (1280x720)**.</span></span> <span data-ttu-id="5fc3d-159">Ezek a beállítások testreszabásához jelölje ki a fogaskerék ikonra az vetett jobb le, és válassza **új készletet**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-159">To customize these settings, select the gear icon to the right of the drop down, and then choose **New Preset**.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. <span data-ttu-id="5fc3d-161">Kódoló készletek beállítása.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-161">Configure encoder presets.</span></span>

    <span data-ttu-id="5fc3d-162">Nevezze el a készletet, és ellenőrizze a következőket ajánlott beállítások:</span><span class="sxs-lookup"><span data-stu-id="5fc3d-162">Name the preset, and check for the following recommended settings:</span></span>

    <span data-ttu-id="5fc3d-163">**Videó**</span><span class="sxs-lookup"><span data-stu-id="5fc3d-163">**Video**</span></span>

   * <span data-ttu-id="5fc3d-164">Kódoló: MainConcept H.264</span><span class="sxs-lookup"><span data-stu-id="5fc3d-164">Encoder: MainConcept H.264</span></span>
   * <span data-ttu-id="5fc3d-165">A képkockák másodpercenkénti: 30</span><span class="sxs-lookup"><span data-stu-id="5fc3d-165">Frames per Second: 30</span></span>
   * <span data-ttu-id="5fc3d-166">Átlagos átviteli sebessége: 5000 kbit/s (módosítani lehet a hálózati korlátozás)</span><span class="sxs-lookup"><span data-stu-id="5fc3d-166">Average bit rate: 5000 kbits/sec (Can be adjusted based on network limitations)</span></span>
   * <span data-ttu-id="5fc3d-167">Profil: fő</span><span class="sxs-lookup"><span data-stu-id="5fc3d-167">Profile: Main</span></span>
   * <span data-ttu-id="5fc3d-168">Kulcskocka minden: 60 keretek</span><span class="sxs-lookup"><span data-stu-id="5fc3d-168">Key frame every: 60 frames</span></span>

    <span data-ttu-id="5fc3d-169">**Hang**</span><span class="sxs-lookup"><span data-stu-id="5fc3d-169">**Audio**</span></span>

   * <span data-ttu-id="5fc3d-170">Cél: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="5fc3d-170">Target bit rate: 192 kbits/sec</span></span>
   * <span data-ttu-id="5fc3d-171">Mintavételi gyakoriság: 44.100 kHz</span><span class="sxs-lookup"><span data-stu-id="5fc3d-171">Sample Rate: 44.100 kHz</span></span>

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. <span data-ttu-id="5fc3d-173">Nyomja le az **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-173">Press **Save**.</span></span>

    <span data-ttu-id="5fc3d-174">A Encoding mező most már az újonnan létrehozott profil kijelölésnél érhetők el.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-174">The Encoding field now has the newly created profile available for selection.</span></span>

    <span data-ttu-id="5fc3d-175">Győződjön meg arról, hogy az új profil van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-175">Make sure the new profile is selected.</span></span>
7. <span data-ttu-id="5fc3d-176">A csatorna Get bemeneti URL-cím ahhoz, hogy rendelje hozzá a Wirecast **RTMP végpont**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-176">Get the channel's input URL in order to assign it to the Wirecast **RTMP Endpoint**.</span></span>

    <span data-ttu-id="5fc3d-177">Lépjen vissza az AMSE eszköz, és a csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-177">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="5fc3d-178">Miután állapota megváltozott, a **indítása** való **futtató**, kaphat a bemeneti URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-178">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="5fc3d-179">Ha a csatorna fut, kattintson a jobb gombbal a csatorna neve, felett az egérmutatót navigáljon **vágólapra másolás bemeneti URL-cím** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-179">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. <span data-ttu-id="5fc3d-181">A Wirecast a **kimeneti beállításainak** ablakban illessze be ezt az információt a **cím** output szakasz, és rendelje hozzá egy adatfolyam neve mezőben.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-181">In the Wirecast **Output Settings** window, paste this information in the **Address** field of the output section, and assign a stream name.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. <span data-ttu-id="5fc3d-183">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-183">Select **OK**.</span></span>
2. <span data-ttu-id="5fc3d-184">A fő **Wirecast** képernyőjén ellenőrizze a bemeneti forrás a videó és hang készen áll, majd nyomja le **adatfolyam** felső bal oldali sarokban.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-184">On the main **Wirecast** screen, confirm input sources for video and audio are ready and then hit **Stream** in the top left hand corner.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> <span data-ttu-id="5fc3d-186">Kattintás előtt **adatfolyam**, akkor **kell** győződjön meg arról, hogy készen áll-e a csatornát.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-186">Before you click **Stream**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="5fc3d-187">Győződjön meg arról, hogy nem a csatorna üzemkész állapotban hagyja meg az adatcsatorna-> 15 percnél hosszabb ideig bemeneti hozzájárulás nélkül.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-187">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="5fc3d-188">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="5fc3d-188">Test playback</span></span>

<span data-ttu-id="5fc3d-189">Keresse meg az AMSE eszköz, és kattintson a jobb gombbal a csatornát kell tesztelni.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-189">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="5fc3d-190">A menüben rámutat **lejátszás az előzetes** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-190">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

<span data-ttu-id="5fc3d-191">Ha az adatfolyam a Windows Media player jelenik meg, majd a kódoló megfelelően van konfigurálva AMS való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-191">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="5fc3d-192">Ha hibaüzenetet kap, a csatorna le kell állítani, és kódoló beállításai módosul.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-192">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="5fc3d-193">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-193">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="5fc3d-194">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="5fc3d-194">Create a program</span></span>
1. <span data-ttu-id="5fc3d-195">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-195">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="5fc3d-196">Az a **Live** az AMSE eszköz lapján, a program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-196">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. <span data-ttu-id="5fc3d-198">A program neve, és szükség esetén módosítsa a **az archiválási időtartam** (amely alapértelmezés szerint 4 óra).</span><span class="sxs-lookup"><span data-stu-id="5fc3d-198">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="5fc3d-199">Adja meg a tárolási helyet is, vagy hagyja meg az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-199">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="5fc3d-200">Ellenőrizze a **indítsa el a Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-200">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="5fc3d-201">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-201">Click **Create Program**.</span></span>  

   >[!NOTE]
   ><span data-ttu-id="5fc3d-202">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-202">Program creation takes less time than channel creation.</span></span>
       
5. <span data-ttu-id="5fc3d-203">Ha a program fut, erősítse meg a lejátszás jobb gombbal kattint rá a program, és lépjen az **lejátszás a programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-203">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="5fc3d-204">Ha megerősítette, kattintson a jobb gombbal a program újra, és válassza ki **a kimeneti URL-Címének másolása a vágólapra** (vagy az adatok lekérésére a **Program információk és beállítások** lehetőséget a menüből).</span><span class="sxs-lookup"><span data-stu-id="5fc3d-204">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="5fc3d-205">Az adatfolyam egy Player beágyazott, vagy olyan célközönségnek juttathatja el élő megtekintésre elosztott készen áll.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-205">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="5fc3d-206">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="5fc3d-206">Troubleshooting</span></span>
<span data-ttu-id="5fc3d-207">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="5fc3d-207">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="5fc3d-208">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="5fc3d-208">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5fc3d-209">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="5fc3d-209">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
