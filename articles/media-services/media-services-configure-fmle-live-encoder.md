---
title: "A FMLE kódoló egyféle sávszélességű élő adatfolyamot küldeni |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan konfigurálhatja a Flash Media élő kódoló (FMLE) kódoló egyféle sávszélességű adatfolyamot küldeni AMS élő kódolásra képes csatornák."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e831048f34ecf6e89595adc4bfd58b5977e04bdb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="c8ce6-103">Használja a FMLE kódoló egyféle sávszélességű élő adatfolyamot küldeni</span><span class="sxs-lookup"><span data-stu-id="c8ce6-103">Use the FMLE encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c8ce6-104">FMLE</span><span class="sxs-lookup"><span data-stu-id="c8ce6-104">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
> * [<span data-ttu-id="c8ce6-105">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="c8ce6-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="c8ce6-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="c8ce6-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="c8ce6-107">Wirecast</span><span class="sxs-lookup"><span data-stu-id="c8ce6-107">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
>
>

<span data-ttu-id="c8ce6-108">Ez a témakör ismerteti, hogyan konfigurálható a [Flash Live Media Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) kódoló egy egyfajta sávszélességű adatfolyamot AMS-csatorna is küldhet a valós idejű kódolásra engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-108">This topic shows how to configure the [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="c8ce6-109">További információk: [Az Azure Media Services segítségével élő kódolásra képes csatornák használata](media-services-manage-live-encoder-enabled-channels.md)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="c8ce6-110">Ez az oktatóanyag bemutatja, hogyan kezelheti az Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="c8ce6-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="c8ce6-112">Ha Mac vagy Linux, létrehozásához használja az Azure-portálon [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="c8ce6-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

<span data-ttu-id="c8ce6-113">Vegye figyelembe, hogy ez az oktatóanyag leírja AAC használatával.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-113">Note that this tutorial describes using AAC.</span></span> <span data-ttu-id="c8ce6-114">Azonban az FMLE nem támogatja az AAC alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-114">However, FMLE doesn’t supports AAC by default.</span></span> <span data-ttu-id="c8ce6-115">Vásároljon egy beépülő modul a AAC kódolás például MainConcept kellene: [AAC beépülő modul](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-115">You would need to purchase a plugin for AAC encoding such as from MainConcept: [AAC plugin](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8ce6-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8ce6-116">Prerequisites</span></span>
* [<span data-ttu-id="c8ce6-117">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8ce6-117">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="c8ce6-118">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-118">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="c8ce6-119">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-119">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="c8ce6-120">Telepítse a legújabb verzióját a [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-120">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="c8ce6-121">Indítsa el az eszközt, és csatlakozzon az AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-121">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="c8ce6-122">Tippek</span><span class="sxs-lookup"><span data-stu-id="c8ce6-122">Tips</span></span>
* <span data-ttu-id="c8ce6-123">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-123">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="c8ce6-124">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor, hogy az adatfolyam-továbbítási bitrates duplán.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-124">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="c8ce6-125">Ez nem kötelezők, ez segít csökkenteni a hálózati torlódás hatását.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-125">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="c8ce6-126">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-126">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="c8ce6-127">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8ce6-127">Create a channel</span></span>
1. <span data-ttu-id="c8ce6-128">Az AMSE eszköz navigáljon a **Live** lapot, és a csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-128">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="c8ce6-129">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="c8ce6-129">Select **Create channel…**</span></span> <span data-ttu-id="c8ce6-130">a menüből.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-130">from the menu.</span></span>

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. <span data-ttu-id="c8ce6-132">Adjon meg egy csatorna nevét, a Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-132">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="c8ce6-133">A csatorna beállítások területen válassza a **szabványos** az élő kódolás lehetőségnél a bemeneti protokoll beállítása **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-133">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="c8ce6-134">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-134">You can leave all other settings as is.</span></span>

    <span data-ttu-id="c8ce6-135">Győződjön meg arról, hogy a **most indítsa el az új csatorna** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-135">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="c8ce6-136">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-136">Click **Create Channel**.</span></span>

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> <span data-ttu-id="c8ce6-138">A csatorna mindaddig elindításához 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-138">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="c8ce6-139">A csatorna indítása közben is [a kódoló](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span><span class="sxs-lookup"><span data-stu-id="c8ce6-139">While the channel is starting you can [configure the encoder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8ce6-140">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-140">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="c8ce6-141">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="c8ce6-141">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="c8ce6-142"><a id=configure_fmle_rtmp></a>A FMLE kódoló</span><span class="sxs-lookup"><span data-stu-id="c8ce6-142"><a id=configure_fmle_rtmp></a>Configure the FMLE encoder</span></span>
<span data-ttu-id="c8ce6-143">Ebben az oktatóanyagban a következő kimeneti beállításokat használják.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-143">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="c8ce6-144">Ez a szakasz a további konfigurációs lépések részletesebben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-144">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="c8ce6-145">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-145">**Video**:</span></span>

* <span data-ttu-id="c8ce6-146">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="c8ce6-146">Codec: H.264</span></span>
* <span data-ttu-id="c8ce6-147">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-147">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="c8ce6-148">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="c8ce6-148">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="c8ce6-149">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-149">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="c8ce6-150">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="c8ce6-150">Frame Rate: 30</span></span>

<span data-ttu-id="c8ce6-151">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-151">**Audio**:</span></span>

* <span data-ttu-id="c8ce6-152">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-152">Codec: AAC (LC)</span></span>
* <span data-ttu-id="c8ce6-153">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="c8ce6-153">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="c8ce6-154">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="c8ce6-154">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="c8ce6-155">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="c8ce6-155">Configuration steps</span></span>
1. <span data-ttu-id="c8ce6-156">Keresse meg a Flash Media élő kódoló (FMLE) a gép, használja a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-156">Navigate to the Flash Media Live Encoder’s (FMLE) interface on the machine being used.</span></span>

    <span data-ttu-id="c8ce6-157">Az objektumfelület beállítások egy fő lapján.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-157">The interface is one main page of settings.</span></span> <span data-ttu-id="c8ce6-158">Kérjük, szánjon jegyezze fel a következő beállítások használatába streaming FMLE használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-158">Please take note of the following recommended settings to get started with streaming using FMLE.</span></span>

   * <span data-ttu-id="c8ce6-159">Formátum: H.264 képkockasebessége: 30,00</span><span class="sxs-lookup"><span data-stu-id="c8ce6-159">Format: H.264 Frame Rate: 30.00</span></span>
   * <span data-ttu-id="c8ce6-160">Bemeneti mérete: 1280 x 720</span><span class="sxs-lookup"><span data-stu-id="c8ce6-160">Input Size: 1280 x 720</span></span>
   * <span data-ttu-id="c8ce6-161">Átviteli sebesség: 5000 KB/s (módosítani lehet a hálózati korlátozás)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-161">Bit Rate: 5000 Kbps (Can be adjusted based on network limitations)</span></span>  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     <span data-ttu-id="c8ce6-163">Amikor források segítségével váltakozó, kérjük, pipa "összefűzés lehetőség" beállítás</span><span class="sxs-lookup"><span data-stu-id="c8ce6-163">When using interlaced sources, please checkmark the “Deinterlace” option</span></span>
2. <span data-ttu-id="c8ce6-164">Válassza ki a csavarkulcsot ábrázoló ikon formátum, a további beállítások:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-164">Select the wrench icon next to Format, these additional settings should be:</span></span>

   * <span data-ttu-id="c8ce6-165">Profil: fő</span><span class="sxs-lookup"><span data-stu-id="c8ce6-165">Profile: Main</span></span>
   * <span data-ttu-id="c8ce6-166">Szint: 4.0</span><span class="sxs-lookup"><span data-stu-id="c8ce6-166">Level: 4.0</span></span>
   * <span data-ttu-id="c8ce6-167">Keyframe gyakorisága: 2 másodpercben</span><span class="sxs-lookup"><span data-stu-id="c8ce6-167">Keyframe Frequency: 2 seconds</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. <span data-ttu-id="c8ce6-169">Állítsa be a következő fontos hang beállítást:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-169">Set the following important audio setting:</span></span>

   * <span data-ttu-id="c8ce6-170">Formátum: AAC</span><span class="sxs-lookup"><span data-stu-id="c8ce6-170">Format: AAC</span></span>
   * <span data-ttu-id="c8ce6-171">Mintavételi gyakoriság: 44100 Hz</span><span class="sxs-lookup"><span data-stu-id="c8ce6-171">Sample Rate: 44100 Hz</span></span>
   * <span data-ttu-id="c8ce6-172">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="c8ce6-172">Bitrate: 192 Kbps</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. <span data-ttu-id="c8ce6-174">A csatorna Get bemeneti URL-cím ahhoz, hogy rendelje hozzá a FMLE **RTMP végpont**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-174">Get the channel's input URL in order to assign it to the FMLE's **RTMP Endpoint**.</span></span>

    <span data-ttu-id="c8ce6-175">Lépjen vissza az AMSE eszköz, és a csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-175">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="c8ce6-176">Miután állapota megváltozott, a **indítása** való **futtató**, kaphat a bemeneti URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-176">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="c8ce6-177">Ha a csatorna fut, kattintson a jobb gombbal a csatorna neve, felett az egérmutatót navigáljon **vágólapra másolás bemeneti URL-cím** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-177">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. <span data-ttu-id="c8ce6-179">Illessze be ezt az információt a **FMS URL-cím** output szakasz, és rendelje hozzá egy adatfolyam neve mezőben.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-179">Paste this information in the **FMS URL** field of the output section, and assign a stream name.</span></span>

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    <span data-ttu-id="c8ce6-181">A további redundáns működéshez ismételje meg ezeket a lépéseket, a másodlagos bemeneti URL-címet.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-181">For extra redundancy, repeat these steps with the Secondary Input URL.</span></span>
6. <span data-ttu-id="c8ce6-182">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-182">Select **Connect**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8ce6-183">Kattintás előtt **Connect**, akkor **kell** győződjön meg arról, hogy készen áll-e a csatornát.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-183">Before you click **Connect**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="c8ce6-184">Győződjön meg arról, hogy nem a csatorna üzemkész állapotban hagyja meg az adatcsatorna-> 15 percnél hosszabb ideig bemeneti hozzájárulás nélkül.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-184">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="c8ce6-185">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="c8ce6-185">Test playback</span></span>

<span data-ttu-id="c8ce6-186">Keresse meg az AMSE eszköz, és kattintson a jobb gombbal a csatornát kell tesztelni.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-186">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="c8ce6-187">A menüben rámutat **lejátszás az előzetes** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-187">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

<span data-ttu-id="c8ce6-188">Ha az adatfolyam a Windows Media player jelenik meg, majd a kódoló megfelelően van konfigurálva AMS való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-188">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="c8ce6-189">Ha hibaüzenetet kap, a csatorna le kell állítani, és kódoló beállításai módosul.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-189">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="c8ce6-190">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-190">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="c8ce6-191">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="c8ce6-191">Create a program</span></span>
1. <span data-ttu-id="c8ce6-192">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-192">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="c8ce6-193">Az a **Live** az AMSE eszköz lapján, a program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-193">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. <span data-ttu-id="c8ce6-195">A program neve, és szükség esetén módosítsa a **az archiválási időtartam** (amely alapértelmezés szerint 4 óra).</span><span class="sxs-lookup"><span data-stu-id="c8ce6-195">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="c8ce6-196">Adja meg a tárolási helyet is, vagy hagyja meg az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-196">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="c8ce6-197">Ellenőrizze a **indítsa el a Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-197">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="c8ce6-198">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-198">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="c8ce6-199">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-199">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="c8ce6-200">Ha a program fut, erősítse meg a lejátszás jobb gombbal kattint rá a program, és lépjen az **lejátszás a programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-200">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="c8ce6-201">Ha megerősítette, kattintson a jobb gombbal a program újra, és válassza ki **a kimeneti URL-Címének másolása a vágólapra** (vagy az adatok lekérésére a **Program információk és beállítások** lehetőséget a menüből).</span><span class="sxs-lookup"><span data-stu-id="c8ce6-201">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="c8ce6-202">Az adatfolyam egy Player beágyazott, vagy olyan célközönségnek juttathatja el élő megtekintésre elosztott készen áll.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-202">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="c8ce6-203">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="c8ce6-203">Troubleshooting</span></span>
<span data-ttu-id="c8ce6-204">Tekintse meg a [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-204">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c8ce6-205">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="c8ce6-205">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c8ce6-206">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c8ce6-206">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
