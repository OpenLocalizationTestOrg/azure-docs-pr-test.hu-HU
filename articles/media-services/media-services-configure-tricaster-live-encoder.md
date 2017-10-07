---
title: "aaaConfigure hello NewTek TriCaster kódoló toosend egyféle sávszélességű élő adatfolyamot |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure hello Tricaster élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás."
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
ms.openlocfilehash: 57dcf62a6a76b04e69f147a738be78ccb3c3ecdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-newtek-tricaster-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="f37d7-103">Hello NewTek TriCaster kódoló toosend egyféle sávszélességű élő adatfolyamot használata</span><span class="sxs-lookup"><span data-stu-id="f37d7-103">Use hello NewTek TriCaster encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f37d7-104">Tricaster</span><span class="sxs-lookup"><span data-stu-id="f37d7-104">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="f37d7-105">Élő elemi</span><span class="sxs-lookup"><span data-stu-id="f37d7-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="f37d7-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="f37d7-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="f37d7-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="f37d7-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="f37d7-108">Ez a témakör bemutatja, hogyan tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) élő kódoló toosend egy egyféle sávszélességű adatfolyamot tooAMS képes csatornák az élő kódolás.</span><span class="sxs-lookup"><span data-stu-id="f37d7-108">This topic shows how tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="f37d7-109">További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="f37d7-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="f37d7-110">Ez az oktatóanyag bemutatja, hogyan toomanage Azure Media Services (AMS) Azure Media Services Explorer (AMSE) eszközzel.</span><span class="sxs-lookup"><span data-stu-id="f37d7-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="f37d7-111">Ez az eszköz csak a Windows rendszerű Számítógépeken fut.</span><span class="sxs-lookup"><span data-stu-id="f37d7-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="f37d7-112">Ha Mac vagy Linux, használja az Azure portál toocreate hello [csatornák](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) és [programok](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="f37d7-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f37d7-113">Ha a valós idejű kódolásra képes csatornák tooAMS Tricaster használatával való elküldésére hozzájárulás hírcsatorna, lehet videó/hang hibáktól az élő esemény Tricaster, például a gyors közötti hírcsatornák kivágja, vagy táblagépükkel/való váltás az egyes szolgáltatások használatakor .</span><span class="sxs-lookup"><span data-stu-id="f37d7-113">When using Tricaster for sending in a contribution feed tooAMS channels that are enabled for live encoding, there can be video/audio glitches in your live event if you use certain features of Tricaster, such as rapid cutting between feeds, or switching to/from slates.</span></span> <span data-ttu-id="f37d7-114">hello AMS csoport dolgozik megoldásán ezeket a problémákat, amíg ez nem sikerül, nem javasolt ezeket a szolgáltatásokat az toouse.</span><span class="sxs-lookup"><span data-stu-id="f37d7-114">hello AMS team is working on fixing these issues, until then, it is not recommend toouse these features.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="f37d7-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f37d7-115">Prerequisites</span></span>
* [<span data-ttu-id="f37d7-116">Azure Media Services-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f37d7-116">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="f37d7-117">Gondoskodjon arról, hogy fut egy adatfolyam-végpontot.</span><span class="sxs-lookup"><span data-stu-id="f37d7-117">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="f37d7-118">További információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="f37d7-118">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="f37d7-119">Hello hello legújabb verziójának telepítéséhez [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) eszköz.</span><span class="sxs-lookup"><span data-stu-id="f37d7-119">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="f37d7-120">Hello eszköz indítása, és csatlakozzon a tooyour AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="f37d7-120">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="f37d7-121">Tippek</span><span class="sxs-lookup"><span data-stu-id="f37d7-121">Tips</span></span>
* <span data-ttu-id="f37d7-122">Amikor csak lehetséges, hardveresen rögzített beállítású internetkapcsolat használatának.</span><span class="sxs-lookup"><span data-stu-id="f37d7-122">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="f37d7-123">Jó tapasztalatok sávszélesség-követelményekkel meghatározásakor streaming bitrates toodouble hello.</span><span class="sxs-lookup"><span data-stu-id="f37d7-123">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="f37d7-124">Ez nem kötelezők, ez segít csökkenteni hello hatását a hálózati torlódás.</span><span class="sxs-lookup"><span data-stu-id="f37d7-124">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="f37d7-125">Ha szoftverrel kódolók, zárja be az el a felesleges programokat.</span><span class="sxs-lookup"><span data-stu-id="f37d7-125">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="f37d7-126">Csatorna létrehozása</span><span class="sxs-lookup"><span data-stu-id="f37d7-126">Create a channel</span></span>
1. <span data-ttu-id="f37d7-127">Hello AMSE eszköz, lépjen a toohello **Live** lapot, és hello csatorna területen kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="f37d7-127">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="f37d7-128">Válassza ki **csatorna létrehozása...**</span><span class="sxs-lookup"><span data-stu-id="f37d7-128">Select **Create channel…**</span></span> <span data-ttu-id="f37d7-129">hello menüből.</span><span class="sxs-lookup"><span data-stu-id="f37d7-129">from hello menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. <span data-ttu-id="f37d7-131">Adjon meg egy csatorna nevét hello Leírás mezőt nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="f37d7-131">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="f37d7-132">A csatorna beállítások területen válassza a **szabványos** hello élő kódolás beállítást, a hello a bemeneti protokoll beállítása túl**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-132">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="f37d7-133">A többi beállítás, hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="f37d7-133">You can leave all other settings as is.</span></span>

    <span data-ttu-id="f37d7-134">Győződjön meg arról, hogy hello **Start hello új csatorna most** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="f37d7-134">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="f37d7-135">Kattintson a **csatornát létrehozni**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-135">Click **Create Channel**.</span></span>

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> <span data-ttu-id="f37d7-137">hello csatorna mindaddig toostart 20 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f37d7-137">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="f37d7-138">Hello csatorna indítása közben is [hello kódoló konfigurálása](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span><span class="sxs-lookup"><span data-stu-id="f37d7-138">While hello channel is starting you can [configure hello encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f37d7-139">Vegye figyelembe, hogy a számlázás indul, amint csatorna kész állapotba kerül.</span><span class="sxs-lookup"><span data-stu-id="f37d7-139">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="f37d7-140">További információkért lásd: [csatorna állapotok](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="f37d7-140">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="f37d7-141"><a id=configure_tricaster_rtmp></a>Hello NewTek TriCaster kódoló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f37d7-141"><a id=configure_tricaster_rtmp></a>Configure hello NewTek TriCaster encoder</span></span>
<span data-ttu-id="f37d7-142">Az oktatóanyag hello következő kimeneti beállításokat használ.</span><span class="sxs-lookup"><span data-stu-id="f37d7-142">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="f37d7-143">Ez a szakasz többi hello részletes konfigurációs lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f37d7-143">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="f37d7-144">**Videó**:</span><span class="sxs-lookup"><span data-stu-id="f37d7-144">**Video**:</span></span>

* <span data-ttu-id="f37d7-145">Kodek: H.264</span><span class="sxs-lookup"><span data-stu-id="f37d7-145">Codec: H.264</span></span>
* <span data-ttu-id="f37d7-146">Profil: High (szint 4.0-s)</span><span class="sxs-lookup"><span data-stu-id="f37d7-146">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="f37d7-147">Átviteli sebesség: 5000 KB/s</span><span class="sxs-lookup"><span data-stu-id="f37d7-147">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="f37d7-148">Kulcskocka: 2 másodperc (60 másodperc)</span><span class="sxs-lookup"><span data-stu-id="f37d7-148">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="f37d7-149">Keret aránya: 30</span><span class="sxs-lookup"><span data-stu-id="f37d7-149">Frame Rate: 30</span></span>

<span data-ttu-id="f37d7-150">**Hang**:</span><span class="sxs-lookup"><span data-stu-id="f37d7-150">**Audio**:</span></span>

* <span data-ttu-id="f37d7-151">A kodek: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="f37d7-151">Codec: AAC (LC)</span></span>
* <span data-ttu-id="f37d7-152">Átviteli sebesség: 192 Kbit/s</span><span class="sxs-lookup"><span data-stu-id="f37d7-152">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="f37d7-153">Mintavételi gyakoriság: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="f37d7-153">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="f37d7-154">Konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="f37d7-154">Configuration steps</span></span>
1. <span data-ttu-id="f37d7-155">Hozzon létre egy új **NewTek TriCaster** projekt attól függően, hogy milyen bemeneti videoforrást használatban van.</span><span class="sxs-lookup"><span data-stu-id="f37d7-155">Create a new **NewTek TriCaster** project depending on what video input source is being used.</span></span>
2. <span data-ttu-id="f37d7-156">Egyszer, hogy a projektben található hello **adatfolyam** gombra, majd kattintson a hello fogaskerék ikonra következő tooit tooaccess hello adatfolyam konfigurációs menü.</span><span class="sxs-lookup"><span data-stu-id="f37d7-156">Once within that project, find hello **Stream** button, and click hello gear icon next tooit tooaccess hello stream configuration menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. <span data-ttu-id="f37d7-158">Miután megnyílt hello menüben, kattintson a **új** hello kapcsolat címszó alatt.</span><span class="sxs-lookup"><span data-stu-id="f37d7-158">Once hello menu has opened, click **New** under hello Connection heading.</span></span> <span data-ttu-id="f37d7-159">Amikor hello kapcsolattípus megadását kéri, válassza ki a **Adobe Flash**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-159">When prompted for hello connection type, select **Adobe Flash**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. <span data-ttu-id="f37d7-161">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f37d7-161">Click **OK**.</span></span>
5. <span data-ttu-id="f37d7-162">Egy FMLE profil most importálható hello legördülő lista alatt nyílra kattintva **Streaming profil** túl nézetre lépne, és**Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-162">An FMLE profile can now be imported by clicking hello drop down arrow under **Streaming Profile** and navigating too**Browse**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. <span data-ttu-id="f37d7-164">Keresse meg a toowhere konfigurált hello FMLE profil lett mentve.</span><span class="sxs-lookup"><span data-stu-id="f37d7-164">Navigate toowhere hello configured FMLE profile was saved.</span></span>
7. <span data-ttu-id="f37d7-165">Jelölje ki, majd nyomja le az ENTER **OK**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-165">Select it, and press **OK**.</span></span>

    <span data-ttu-id="f37d7-166">Hello-profil a feltöltést követően folytassa toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="f37d7-166">Once hello profile is uploaded, proceed toohello next step.</span></span>
8. <span data-ttu-id="f37d7-167">Hello csatorna bemeneti URL-cím beszerzése a rendelés tooassign azt toohello Tricaster **RTMP végpont**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-167">Get hello channel's input URL in order tooassign it toohello Tricaster **RTMP Endpoint**.</span></span>

    <span data-ttu-id="f37d7-168">Keresse meg a visszafelé toohello AMSE eszköz, és ellenőrizze a hello csatorna befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="f37d7-168">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="f37d7-169">Miután hello állapota megváltozott **indítása** túl**futtató**, kaphat a hello megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="f37d7-169">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="f37d7-170">Hello csatorna futtatásakor kattintson jobb gombbal a hello csatorna neve, a Navigálás le toohover keresztül **bemeneti URL-CÍMÉT tooclipboard** , és válassza **elsődleges bemeneti URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-170">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. <span data-ttu-id="f37d7-172">Illessze be ezeket az információkat a hello **hely** eleménél **Flash Server** hello Tricaster projektben.</span><span class="sxs-lookup"><span data-stu-id="f37d7-172">Paste this information in hello **Location** field under **Flash Server** within hello Tricaster project.</span></span> <span data-ttu-id="f37d7-173">Is hozzárendelhet egy adatfolyam-nevet, a hello **adatfolyam-azonosító** mező.</span><span class="sxs-lookup"><span data-stu-id="f37d7-173">Also assign a stream name in hello **Stream ID** field.</span></span>

    <span data-ttu-id="f37d7-174">Ha adatfolyam információt adott toohello FMLE profilt, azt is importálhatók toothis szakasz kattintva **beállítások importálása**, lépjen a mentett toohello FMLE profil csomópontra, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-174">If stream information was added toohello FMLE profile, it can also be imported toothis section by clicking **Import Settings**, navigating toohello saved FMLE profile and clicking **OK**.</span></span> <span data-ttu-id="f37d7-175">hello megfelelő Flash Server mezőt fel kell töltenie FMLE hello adataival.</span><span class="sxs-lookup"><span data-stu-id="f37d7-175">hello relevant Flash Server fields should populate with hello information from FMLE.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. <span data-ttu-id="f37d7-177">Ha befejezte, kattintson a **OK** üdvözlő képernyőt hello alján.</span><span class="sxs-lookup"><span data-stu-id="f37d7-177">When finished, click **OK** at hello bottom of hello screen.</span></span> <span data-ttu-id="f37d7-178">Készen áll a video- és ráfordítások hello Tricaster, kezdje tooAMS streaming hello kattintva **adatfolyam** gombra.</span><span class="sxs-lookup"><span data-stu-id="f37d7-178">When video and audio inputs into hello Tricaster are ready, begin streaming tooAMS by clicking hello **Stream** button.</span></span>

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> <span data-ttu-id="f37d7-180">Kattintás előtt **adatfolyam**, hogy **kell** győződjön meg arról, hogy készen áll-e hello csatorna.</span><span class="sxs-lookup"><span data-stu-id="f37d7-180">Before you click **Stream**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="f37d7-181">Győződjön meg arról is, nem tooleave hello egy bemeneti hozzájárulás nélkül üzemkész állapotban csatorna hírcsatorna > 15 percnél hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="f37d7-181">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="f37d7-182">Teszt lejátszás</span><span class="sxs-lookup"><span data-stu-id="f37d7-182">Test playback</span></span>
<span data-ttu-id="f37d7-183">Keresse meg a toohello AMSE eszköz, és kattintson a jobb gombbal hello csatorna toobe tesztelve.</span><span class="sxs-lookup"><span data-stu-id="f37d7-183">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="f37d7-184">Hello menüben rámutat **lejátszás hello Preview** válassza **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-184">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

<span data-ttu-id="f37d7-185">Hello adatfolyam hello player jelenik meg, ha hello kódoló megfelelően konfigurált tooconnect tooAMS lett.</span><span class="sxs-lookup"><span data-stu-id="f37d7-185">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="f37d7-186">Ha hibaüzenetet kap, hello csatorna toobe alaphelyzetbe állítása és a kódoló beállításait módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="f37d7-186">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="f37d7-187">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f37d7-187">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="f37d7-188">Hozzon létre egy programot</span><span class="sxs-lookup"><span data-stu-id="f37d7-188">Create a program</span></span>
1. <span data-ttu-id="f37d7-189">Miután csatorna lejátszás megerősítjük, hozzon létre egy programot.</span><span class="sxs-lookup"><span data-stu-id="f37d7-189">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="f37d7-190">A hello **Live** hello AMSE eszköz lapon hello program területen kattintson a jobb gombbal, és válassza ki **hozzon létre új Program**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-190">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. <span data-ttu-id="f37d7-192">Hello program neve, és ha szükséges, módosítsa a hello **az archiválási időtartam** (mely alapértelmezett too4 óra).</span><span class="sxs-lookup"><span data-stu-id="f37d7-192">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="f37d7-193">Adjon meg olyan tárolási helyen vagy hagyja hello alapértelmezett is.</span><span class="sxs-lookup"><span data-stu-id="f37d7-193">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="f37d7-194">Ellenőrizze a hello **Start hello Program most** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f37d7-194">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="f37d7-195">Kattintson a **hozzon létre programot**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-195">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="f37d7-196">Program létrehozása gyorsabb a csatorna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f37d7-196">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="f37d7-197">Ha hello program fut, erősítse meg a lejátszás hello program jobb gombbal kattint rá, és a Navigálás túl**lejátszás hello programokról** és jelölje be **Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="f37d7-197">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="f37d7-198">Ha megerősítette, hello program ismét kattintson jobb gombbal, majd válassza ki **hello kimeneti URL-cím tooClipboard másolja** (vagy az adatok lekérését hello **Program információk és beállítások** hello menüből beállítás).</span><span class="sxs-lookup"><span data-stu-id="f37d7-198">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="f37d7-199">hello adatfolyama most már készen áll a player ágyazott toobe, és az elosztott tooan célközönség élő megtekintése.</span><span class="sxs-lookup"><span data-stu-id="f37d7-199">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="f37d7-200">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="f37d7-200">Troubleshooting</span></span>
<span data-ttu-id="f37d7-201">Tekintse meg a hello [hibaelhárítási](media-services-troubleshooting-live-streaming.md) témakör útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f37d7-201">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="next-step"></a><span data-ttu-id="f37d7-202">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f37d7-202">Next step</span></span>
<span data-ttu-id="f37d7-203">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="f37d7-203">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f37d7-204">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f37d7-204">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
