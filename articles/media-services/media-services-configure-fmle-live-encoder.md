---
title: "aaaConfigure hello FMLE kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello Flash Media élő kódoló (FMLE) kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás."
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
ms.openlocfilehash: 780d911c5186ec09c784264f9a0d0c3f8059d305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-fmle-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="a93ed-103">Hello FMLE kódoló toosend egyféle sávszélességű élő adatfolyamot használata</span><span class="sxs-lookup"><span data-stu-id="a93ed-103">Use hello FMLE encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a93ed-104">FMLE</span><span class="sxs-lookup"><span data-stu-id="a93ed-104">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
> * [<span data-ttu-id="a93ed-105">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="a93ed-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="a93ed-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="a93ed-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="a93ed-107">Wirecast</span><span class="sxs-lookup"><span data-stu-id="a93ed-107">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
>
>

<span data-ttu-id="a93ed-108">Ez a témakör bemutatja, hogyan tooconfigure hello [Flash Live Media Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) kódoló toosend egyszeres sávszélességű adatfolyam-tooAMS élő kódolásra képes csatornák.</span><span class="sxs-lookup"><span data-stu-id="a93ed-108">This topic shows how tooconfigure hello [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="a93ed-109">További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="a93ed-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="a93ed-110">Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="a93ed-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="a93ed-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="a93ed-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="a93ed-112">Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="a93ed-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

<span data-ttu-id="a93ed-113">Vegye figyelembe, hogy ez az oktatóanyag leírja AAC használatával.</span><span class="sxs-lookup"><span data-stu-id="a93ed-113">Note that this tutorial describes using AAC.</span></span> <span data-ttu-id="a93ed-114">Azonban az FMLE nem támogatja az AAC alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a93ed-114">However, FMLE doesn’t supports AAC by default.</span></span> <span data-ttu-id="a93ed-115">Kellene toopurchase AAC kódolási beépülő modul például MainConcept: [AAC beépülő modul](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span><span class="sxs-lookup"><span data-stu-id="a93ed-115">You would need toopurchase a plugin for AAC encoding such as from MainConcept: [AAC plugin](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a93ed-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a93ed-116">Prerequisites</span></span>
* [<span data-ttu-id="a93ed-117">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a93ed-117">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="a93ed-118">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="a93ed-118">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="a93ed-119">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="a93ed-119">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="a93ed-120">Hello hello legújabb verziójának telepítéséhez [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="a93ed-120">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="a93ed-121">Hello eszköz indítása, és csatlakozzon a tooyour AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="a93ed-121">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="a93ed-122">Tippek</span><span class="sxs-lookup"><span data-stu-id="a93ed-122">Tips</span></span>
* <span data-ttu-id="a93ed-123">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="a93ed-123">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="a93ed-124">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor streaming bitrates toodouble hello.</span><span class="sxs-lookup"><span data-stu-id="a93ed-124">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="a93ed-125">Ez nem kötelezők, ez segít csökkenteni hello hatását a hálózati torlódás.</span><span class="sxs-lookup"><span data-stu-id="a93ed-125">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="a93ed-126">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="a93ed-126">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="a93ed-127">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="a93ed-127">Create a channel</span></span>
1. <span data-ttu-id="a93ed-128">Hello AMSE eszköz, lépjen a toohello **Live** lapot, és hello csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="a93ed-128">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="a93ed-129">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="a93ed-129">Select **Create channel…**</span></span> <span data-ttu-id="a93ed-130">hello menüből.</span><span class="sxs-lookup"><span data-stu-id="a93ed-130">from hello menu.</span></span>

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. <span data-ttu-id="a93ed-132">Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="a93ed-132">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="a93ed-133">A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-133">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="a93ed-134">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a93ed-134">You can leave all other settings as is.</span></span>

    <span data-ttu-id="a93ed-135">Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="a93ed-135">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="a93ed-136">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-136">Click **Create Channel**.</span></span>

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> <span data-ttu-id="a93ed-138">hello csatorna mindaddig toostart 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="a93ed-138">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="a93ed-139">Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span><span class="sxs-lookup"><span data-stu-id="a93ed-139">While hello channel is starting you can [configure hello encoder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a93ed-140">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="a93ed-140">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="a93ed-141">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="a93ed-141">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="a93ed-142"><a id=configure_fmle_rtmp></a>Hello FMLE kódoló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a93ed-142"><a id=configure_fmle_rtmp></a>Configure hello FMLE encoder</span></span>
<span data-ttu-id="a93ed-143">Az oktatóanyag hello következő kimeneti beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="a93ed-143">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="a93ed-144">Ez a szakasz többi hello részletes konfigurációs lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a93ed-144">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="a93ed-145">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="a93ed-145">**Video**:</span></span>

* <span data-ttu-id="a93ed-146">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="a93ed-146">Codec: H.264</span></span>
* <span data-ttu-id="a93ed-147">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="a93ed-147">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="a93ed-148">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="a93ed-148">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="a93ed-149">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="a93ed-149">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="a93ed-150">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="a93ed-150">Frame Rate: 30</span></span>

<span data-ttu-id="a93ed-151">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="a93ed-151">**Audio**:</span></span>

* <span data-ttu-id="a93ed-152">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="a93ed-152">Codec: AAC (LC)</span></span>
* <span data-ttu-id="a93ed-153">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="a93ed-153">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="a93ed-154">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="a93ed-154">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="a93ed-155">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="a93ed-155">Configuration steps</span></span>
1. <span data-ttu-id="a93ed-156">Keresse meg a Flash Media élő kódoló (FMLE) csatoló használt hello gépen toohello.</span><span class="sxs-lookup"><span data-stu-id="a93ed-156">Navigate toohello Flash Media Live Encoder’s (FMLE) interface on hello machine being used.</span></span>

    <span data-ttu-id="a93ed-157">hello objektumfelület beállítások egy fő lapján.</span><span class="sxs-lookup"><span data-stu-id="a93ed-157">hello interface is one main page of settings.</span></span> <span data-ttu-id="a93ed-158">Adjon jegyezze fel a hello következő ajánlott beállítások tooget használatába streaming FMLE használatával.</span><span class="sxs-lookup"><span data-stu-id="a93ed-158">Please take note of hello following recommended settings tooget started with streaming using FMLE.</span></span>

   * <span data-ttu-id="a93ed-159">Formátum: H.264 képkockasebessége: 30,00</span><span class="sxs-lookup"><span data-stu-id="a93ed-159">Format: H.264 Frame Rate: 30.00</span></span>
   * <span data-ttu-id="a93ed-160">Bemeneti mérete: 1280 x 720</span><span class="sxs-lookup"><span data-stu-id="a93ed-160">Input Size: 1280 x 720</span></span>
   * <span data-ttu-id="a93ed-161">Átviteli sebesség: 5000 KB/s (módosítani lehet a hálózati korlátozás)</span><span class="sxs-lookup"><span data-stu-id="a93ed-161">Bit Rate: 5000 Kbps (Can be adjusted based on network limitations)</span></span>  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     <span data-ttu-id="a93ed-163">Amikor források segítségével váltakozó, kérjük, pipa hello "összefűzés lehetőség" beállítás</span><span class="sxs-lookup"><span data-stu-id="a93ed-163">When using interlaced sources, please checkmark hello “Deinterlace” option</span></span>
2. <span data-ttu-id="a93ed-164">Jelölje be hello csavarkulcsot ábrázoló ikon következő tooFormat, ezeket a további beállításokat kell lennie:</span><span class="sxs-lookup"><span data-stu-id="a93ed-164">Select hello wrench icon next tooFormat, these additional settings should be:</span></span>

   * <span data-ttu-id="a93ed-165">Profil: fő</span><span class="sxs-lookup"><span data-stu-id="a93ed-165">Profile: Main</span></span>
   * <span data-ttu-id="a93ed-166">Szint: 4.0</span><span class="sxs-lookup"><span data-stu-id="a93ed-166">Level: 4.0</span></span>
   * <span data-ttu-id="a93ed-167">Keyframe gyakorisága: 2 másodpercben</span><span class="sxs-lookup"><span data-stu-id="a93ed-167">Keyframe Frequency: 2 seconds</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. <span data-ttu-id="a93ed-169">Állítsa be a következő fontos hang beállítás hello:</span><span class="sxs-lookup"><span data-stu-id="a93ed-169">Set hello following important audio setting:</span></span>

   * <span data-ttu-id="a93ed-170">Formátum: AAC</span><span class="sxs-lookup"><span data-stu-id="a93ed-170">Format: AAC</span></span>
   * <span data-ttu-id="a93ed-171">Mintavételi gyakoriság: 44100 Hz</span><span class="sxs-lookup"><span data-stu-id="a93ed-171">Sample Rate: 44100 Hz</span></span>
   * <span data-ttu-id="a93ed-172">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="a93ed-172">Bitrate: 192 Kbps</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. <span data-ttu-id="a93ed-174">Hello csatorna bemeneti URL-cím beszerzése a rendelés tooassign azt toohello FMLE **RTMP végpont**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-174">Get hello channel's input URL in order tooassign it toohello FMLE's **RTMP Endpoint**.</span></span>

    <span data-ttu-id="a93ed-175">Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="a93ed-175">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="a93ed-176">Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="a93ed-176">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="a93ed-177">Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-177">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. <span data-ttu-id="a93ed-179">Illessze be ezeket az információkat a hello **FMS URL-cím** hello output szakasz, és rendelje hozzá egy adatfolyam neve mezőjében.</span><span class="sxs-lookup"><span data-stu-id="a93ed-179">Paste this information in hello **FMS URL** field of hello output section, and assign a stream name.</span></span>

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    <span data-ttu-id="a93ed-181">További redundancia ismételje meg ezeket a lépéseket hello másodlagos bemeneti URL-cím.</span><span class="sxs-lookup"><span data-stu-id="a93ed-181">For extra redundancy, repeat these steps with hello Secondary Input URL.</span></span>
6. <span data-ttu-id="a93ed-182">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a93ed-182">Select **Connect**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a93ed-183">Kattintás előtt **Connect**, hogy **kell** győződjön meg arról, hogy készen áll-e hello csatorna.</span><span class="sxs-lookup"><span data-stu-id="a93ed-183">Before you click **Connect**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="a93ed-184">Győződjön meg arról is, nem tooleave hello egy bemeneti hozzájárulás nélkül üzemkész állapotban csatorna hírcsatorna > 15 percnél hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="a93ed-184">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="a93ed-185">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="a93ed-185">Test playback</span></span>

<span data-ttu-id="a93ed-186">Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve.</span><span class="sxs-lookup"><span data-stu-id="a93ed-186">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="a93ed-187">Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-187">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

<span data-ttu-id="a93ed-188">Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.</span><span class="sxs-lookup"><span data-stu-id="a93ed-188">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="a93ed-189">Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="a93ed-189">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="a93ed-190">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a93ed-190">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="a93ed-191">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="a93ed-191">Create a program</span></span>
1. <span data-ttu-id="a93ed-192">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="a93ed-192">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="a93ed-193">A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-193">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. <span data-ttu-id="a93ed-195">Hello program neve, és ha szükséges, módosítsa a hello **az archiválási időtartam** (mely alapértelmezett too4 óra).</span><span class="sxs-lookup"><span data-stu-id="a93ed-195">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="a93ed-196">Adjon meg olyan tárolási helyen vagy hagyja hello alapértelmezett is.</span><span class="sxs-lookup"><span data-stu-id="a93ed-196">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="a93ed-197">Ellenőrizze a hello **Start hello Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a93ed-197">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="a93ed-198">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-198">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="a93ed-199">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a93ed-199">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="a93ed-200">Ha hello program fut, erősítse meg a lejátszás hello program jobb gombbal kattint rá, és a Navigálás túl**lejátszás hello programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="a93ed-200">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="a93ed-201">Ha megerősítette, hello program ismét kattintson jobb gombbal, majd válassza ki **hello kimeneti URL-cím tooClipboard másolja** (vagy az adatok lekérését hello **Program információk és beállítások** hello menüből beállítás).</span><span class="sxs-lookup"><span data-stu-id="a93ed-201">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="a93ed-202">hello adatfolyama most már készen áll a player ágyazott toobe, és az elosztott tooan célközönség élő megtekintése.</span><span class="sxs-lookup"><span data-stu-id="a93ed-202">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="a93ed-203">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="a93ed-203">Troubleshooting</span></span>
<span data-ttu-id="a93ed-204">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a93ed-204">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="a93ed-205">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="a93ed-205">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a93ed-206">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="a93ed-206">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
