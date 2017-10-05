---
title: "A NewTek TriCaster kódoló egyféle sávszélességű élő adatfolyamot küldeni |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan konfigurálhatja a Tricaster élő kódoló egy egyféle sávszélességű adatfolyamot küldeni AMS élő kódolásra képes csatornák."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 42b012fb98bd0504c931ce391d63aecca8c3d311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="58b21-103">Használja a NewTek TriCaster kódoló egyféle sávszélességű élő adatfolyamot küldeni</span><span class="sxs-lookup"><span data-stu-id="58b21-103">Use the NewTek TriCaster encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="58b21-104">Tricaster</span><span class="sxs-lookup"><span data-stu-id="58b21-104">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="58b21-105">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="58b21-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="58b21-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="58b21-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="58b21-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="58b21-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="58b21-108">Ez a témakör ismerteti, hogyan konfigurálható a [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) élő kódoló egy egyfajta sávszélességű adatfolyamot AMS-csatorna is küldhet a valós idejű kódolásra engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="58b21-108">This topic shows how to configure the [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="58b21-109">További információk: [Az Azure Media Services segítségével élő kódolásra képes csatornák használata](media-services-manage-live-encoder-enabled-channels.md)</span><span class="sxs-lookup"><span data-stu-id="58b21-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="58b21-110">Ez az oktatóanyag bemutatja, hogyan kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="58b21-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="58b21-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="58b21-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="58b21-112">Ha Mac vagy Linux, létrehozásához használja az Azure-portálon [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="58b21-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

> [!NOTE]
> <span data-ttu-id="58b21-113">Az AMS élő kódolásra képes csatornák hírcsatornát hozzájárulás küldési Tricaster használatakor lehet videó/hang hibáktól az élő esemény Tricaster, például a gyors közötti hírcsatornák kivágja, vagy táblagépükkel/való váltás az egyes szolgáltatások használatakor.</span><span class="sxs-lookup"><span data-stu-id="58b21-113">When using Tricaster for sending in a contribution feed to AMS channels that are enabled for live encoding, there can be video/audio glitches in your live event if you use certain features of Tricaster, such as rapid cutting between feeds, or switching to/from slates.</span></span> <span data-ttu-id="58b21-114">Ezek a problémák kijavítása addig az AMS-csapat dolgozik, akkor van nem célszerű szolgáltatások használatára.</span><span class="sxs-lookup"><span data-stu-id="58b21-114">The AMS team is working on fixing these issues, until then, it is not recommend to use these features.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="58b21-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="58b21-115">Prerequisites</span></span>
* [<span data-ttu-id="58b21-116">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="58b21-116">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="58b21-117">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="58b21-117">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="58b21-118">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="58b21-118">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="58b21-119">Telepítse a legújabb verzióját a [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="58b21-119">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="58b21-120">Indítsa el az eszközt, és csatlakozzon az AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="58b21-120">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="58b21-121">Tippek</span><span class="sxs-lookup"><span data-stu-id="58b21-121">Tips</span></span>
* <span data-ttu-id="58b21-122">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="58b21-122">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="58b21-123">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor, hogy az adatfolyam-továbbítási bitrates duplán.</span><span class="sxs-lookup"><span data-stu-id="58b21-123">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="58b21-124">Ez nem kötelezők, ez segít csökkenteni a hálózati torlódás hatását.</span><span class="sxs-lookup"><span data-stu-id="58b21-124">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="58b21-125">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="58b21-125">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="58b21-126">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="58b21-126">Create a channel</span></span>
1. <span data-ttu-id="58b21-127">Az AMSE eszköz navigáljon a **Live** lapot, és a csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="58b21-127">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="58b21-128">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="58b21-128">Select **Create channel…**</span></span> <span data-ttu-id="58b21-129">a menüből.</span><span class="sxs-lookup"><span data-stu-id="58b21-129">from the menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. <span data-ttu-id="58b21-131">Adjon meg egy csatorna nevét, a Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="58b21-131">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="58b21-132">A csatorna beállítások területen válassza a **szabványos** az élő kódolás lehetőségnél a bemeneti protokoll beállítása **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="58b21-132">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="58b21-133">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="58b21-133">You can leave all other settings as is.</span></span>

    <span data-ttu-id="58b21-134">Győződjön meg arról, hogy a **most indítsa el az új csatorna** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="58b21-134">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="58b21-135">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="58b21-135">Click **Create Channel**.</span></span>

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> <span data-ttu-id="58b21-137">A csatorna mindaddig elindításához 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="58b21-137">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="58b21-138">A csatorna indítása közben is [a kódoló](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span><span class="sxs-lookup"><span data-stu-id="58b21-138">While the channel is starting you can [configure the encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="58b21-139">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="58b21-139">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="58b21-140">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="58b21-140">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="58b21-141"><a id=configure_tricaster_rtmp></a>A NewTek TriCaster kódoló</span><span class="sxs-lookup"><span data-stu-id="58b21-141"><a id=configure_tricaster_rtmp></a>Configure the NewTek TriCaster encoder</span></span>
<span data-ttu-id="58b21-142">Ebben az oktatóanyagban a következő kimeneti beállításokat használják.</span><span class="sxs-lookup"><span data-stu-id="58b21-142">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="58b21-143">Ez a szakasz a további konfigurációs lépések részletesebben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="58b21-143">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="58b21-144">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="58b21-144">**Video**:</span></span>

* <span data-ttu-id="58b21-145">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="58b21-145">Codec: H.264</span></span>
* <span data-ttu-id="58b21-146">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="58b21-146">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="58b21-147">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="58b21-147">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="58b21-148">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="58b21-148">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="58b21-149">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="58b21-149">Frame Rate: 30</span></span>

<span data-ttu-id="58b21-150">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="58b21-150">**Audio**:</span></span>

* <span data-ttu-id="58b21-151">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="58b21-151">Codec: AAC (LC)</span></span>
* <span data-ttu-id="58b21-152">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="58b21-152">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="58b21-153">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="58b21-153">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="58b21-154">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="58b21-154">Configuration steps</span></span>
1. <span data-ttu-id="58b21-155">Hozzon létre egy új **NewTek TriCaster** projekt attól függően, hogy milyen bemeneti videoforrást használatban van.</span><span class="sxs-lookup"><span data-stu-id="58b21-155">Create a new **NewTek TriCaster** project depending on what video input source is being used.</span></span>
2. <span data-ttu-id="58b21-156">Egyszer, hogy a projektben található a **adatfolyam** gombra, és hozzáférjen a konfigurációs adatfolyam menü mellett fogaskerék ikonra.</span><span class="sxs-lookup"><span data-stu-id="58b21-156">Once within that project, find the **Stream** button, and click the gear icon next to it to access the stream configuration menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. <span data-ttu-id="58b21-158">Miután megnyílt a menüben, kattintson a **új** kapcsolat fejléc alatt.</span><span class="sxs-lookup"><span data-stu-id="58b21-158">Once the menu has opened, click **New** under the Connection heading.</span></span> <span data-ttu-id="58b21-159">Ha a rendszer kéri a kapcsolat típusát, válassza ki a **Adobe Flash**.</span><span class="sxs-lookup"><span data-stu-id="58b21-159">When prompted for the connection type, select **Adobe Flash**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. <span data-ttu-id="58b21-161">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="58b21-161">Click **OK**.</span></span>
5. <span data-ttu-id="58b21-162">A legördülő lista alatt nyílra kattintva most importálni egy FMLE profil **Streaming profil** és lépjen az **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="58b21-162">An FMLE profile can now be imported by clicking the drop down arrow under **Streaming Profile** and navigating to **Browse**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. <span data-ttu-id="58b21-164">Nyissa meg a konfigurált FMLE profilt menteni.</span><span class="sxs-lookup"><span data-stu-id="58b21-164">Navigate to where the configured FMLE profile was saved.</span></span>
7. <span data-ttu-id="58b21-165">Jelölje ki, majd nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="58b21-165">Select it, and press **OK**.</span></span>

    <span data-ttu-id="58b21-166">A profil a feltöltést követően folytassa a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="58b21-166">Once the profile is uploaded, proceed to the next step.</span></span>
8. <span data-ttu-id="58b21-167">A csatorna Get bemeneti URL-cím ahhoz, hogy rendelje hozzá a Tricaster **RTMP végpont**.</span><span class="sxs-lookup"><span data-stu-id="58b21-167">Get the channel's input URL in order to assign it to the Tricaster **RTMP Endpoint**.</span></span>

    <span data-ttu-id="58b21-168">Lépjen vissza az AMSE eszköz, és a csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="58b21-168">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="58b21-169">Miután állapota megváltozott, a **indítása** való **futtató**, kaphat a bemeneti URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="58b21-169">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="58b21-170">Ha a csatorna fut, kattintson a jobb gombbal a csatorna neve, felett az egérmutatót navigáljon **vágólapra másolás bemeneti URL-cím** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="58b21-170">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. <span data-ttu-id="58b21-172">Illessze be ezt az információt a **hely** eleménél **Flash Server** a Tricaster projektben.</span><span class="sxs-lookup"><span data-stu-id="58b21-172">Paste this information in the **Location** field under **Flash Server** within the Tricaster project.</span></span> <span data-ttu-id="58b21-173">Is hozzárendelhet a adatfolyam nevet adja meg a **adatfolyam-azonosító** mező.</span><span class="sxs-lookup"><span data-stu-id="58b21-173">Also assign a stream name in the **Stream ID** field.</span></span>

    <span data-ttu-id="58b21-174">Ha az adatfolyam információt adott FMLE profilt, azt is importálhatók ebben a szakaszban kattintva **beállítások importálása**, navigáljon a mentett FMLE profilt, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="58b21-174">If stream information was added to the FMLE profile, it can also be imported to this section by clicking **Import Settings**, navigating to the saved FMLE profile and clicking **OK**.</span></span> <span data-ttu-id="58b21-175">A kiszolgáló Flash mezőket fel kell töltenie FMLE származó információkkal.</span><span class="sxs-lookup"><span data-stu-id="58b21-175">The relevant Flash Server fields should populate with the information from FMLE.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. <span data-ttu-id="58b21-177">Ha befejezte, kattintson a **OK** a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="58b21-177">When finished, click **OK** at the bottom of the screen.</span></span> <span data-ttu-id="58b21-178">Készen áll a Tricaster video- és ráfordítások, kezdje kattintva AMS streamelés a **adatfolyam** gombra.</span><span class="sxs-lookup"><span data-stu-id="58b21-178">When video and audio inputs into the Tricaster are ready, begin streaming to AMS by clicking the **Stream** button.</span></span>

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> <span data-ttu-id="58b21-180">Kattintás előtt **adatfolyam**, akkor **kell** győződjön meg arról, hogy készen áll-e a csatornát.</span><span class="sxs-lookup"><span data-stu-id="58b21-180">Before you click **Stream**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="58b21-181">Győződjön meg arról, hogy nem a csatorna üzemkész állapotban hagyja meg az adatcsatorna-> 15 percnél hosszabb ideig bemeneti hozzájárulás nélkül.</span><span class="sxs-lookup"><span data-stu-id="58b21-181">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="58b21-182">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="58b21-182">Test playback</span></span>
<span data-ttu-id="58b21-183">Keresse meg az AMSE eszköz, és kattintson a jobb gombbal a csatornát kell tesztelni.</span><span class="sxs-lookup"><span data-stu-id="58b21-183">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="58b21-184">A menüben rámutat **lejátszás az előzetes** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="58b21-184">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

<span data-ttu-id="58b21-185">Ha az adatfolyam a Windows Media player jelenik meg, majd a kódoló megfelelően van konfigurálva AMS való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="58b21-185">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="58b21-186">Ha hibaüzenetet kap, a csatorna le kell állítani, és kódoló beállításai módosul.</span><span class="sxs-lookup"><span data-stu-id="58b21-186">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="58b21-187">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="58b21-187">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="58b21-188">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="58b21-188">Create a program</span></span>
1. <span data-ttu-id="58b21-189">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="58b21-189">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="58b21-190">Az a **Live** az AMSE eszköz lapján, a program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="58b21-190">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. <span data-ttu-id="58b21-192">A program neve, és szükség esetén módosítsa a **az archiválási időtartam** (amely alapértelmezés szerint 4 óra).</span><span class="sxs-lookup"><span data-stu-id="58b21-192">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="58b21-193">Adja meg a tárolási helyet is, vagy hagyja meg az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="58b21-193">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="58b21-194">Ellenőrizze a **indítsa el a Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="58b21-194">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="58b21-195">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="58b21-195">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="58b21-196">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="58b21-196">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="58b21-197">Ha a program fut, erősítse meg a lejátszás jobb gombbal kattint rá a program, és lépjen az **lejátszás a programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="58b21-197">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="58b21-198">Ha megerősítette, kattintson a jobb gombbal a program újra, és válassza ki **a kimeneti URL-Címének másolása a vágólapra** (vagy az adatok lekérésére a **Program információk és beállítások** lehetőséget a menüből).</span><span class="sxs-lookup"><span data-stu-id="58b21-198">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="58b21-199">Az adatfolyam egy Player beágyazott, vagy olyan célközönségnek juttathatja el élő megtekintésre elosztott készen áll.</span><span class="sxs-lookup"><span data-stu-id="58b21-199">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="58b21-200">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="58b21-200">Troubleshooting</span></span>
<span data-ttu-id="58b21-201">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="58b21-201">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="next-step"></a><span data-ttu-id="58b21-202">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="58b21-202">Next step</span></span>
<span data-ttu-id="58b21-203">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="58b21-203">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="58b21-204">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="58b21-204">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
