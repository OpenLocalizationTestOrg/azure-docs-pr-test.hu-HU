---
title: "aaaLive adatfolyam helyszíni kódolókkal hello Azure-portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello létre egy csatornát, amely átmenő közvetítésre van konfigurálva."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 1fb341e022f66f33903e13e07d3e84c0216cad77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-hello-azure-portal"></a><span data-ttu-id="7ca76-103">Hogyan tooperform élő Stream továbbítása helyszíni kódolókkal hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="7ca76-103">How tooperform live streaming with on-premises encoders using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7ca76-104">Portal</span><span class="sxs-lookup"><span data-stu-id="7ca76-104">Portal</span></span>](media-services-portal-live-passthrough-get-started.md)
> * [<span data-ttu-id="7ca76-105">.NET</span><span class="sxs-lookup"><span data-stu-id="7ca76-105">.NET</span></span>](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [<span data-ttu-id="7ca76-106">REST</span><span class="sxs-lookup"><span data-stu-id="7ca76-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

<span data-ttu-id="7ca76-107">Ez az oktatóanyag végigvezeti hello hello Azure portál toocreate használatával egy **csatorna** , amely átmenő közvetítésre van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7ca76-107">This tutorial walks you through hello steps of using hello Azure portal toocreate a **Channel** that is configured for a pass-through delivery.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7ca76-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7ca76-108">Prerequisites</span></span>
<span data-ttu-id="7ca76-109">Az alábbiakban hello szükséges toocomplete hello oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="7ca76-109">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="7ca76-110">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="7ca76-110">An Azure account.</span></span> <span data-ttu-id="7ca76-111">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7ca76-111">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="7ca76-112">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="7ca76-112">A Media Services account.</span></span> <span data-ttu-id="7ca76-113">egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="7ca76-113">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="7ca76-114">Egy webkamera.</span><span class="sxs-lookup"><span data-stu-id="7ca76-114">A webcam.</span></span> <span data-ttu-id="7ca76-115">Például a [Telestream Wirecast kódoló](http://www.telestream.net/wirecast/overview.htm).</span><span class="sxs-lookup"><span data-stu-id="7ca76-115">For example, [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).</span></span>

<span data-ttu-id="7ca76-116">A következő cikkek tooreview hello javasoljuk:</span><span class="sxs-lookup"><span data-stu-id="7ca76-116">It is highly recommended tooreview hello following articles:</span></span>

* <span data-ttu-id="7ca76-117">[Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/) (Az Azure Media Services RTMP-támogatása és az élő kódolók)</span><span class="sxs-lookup"><span data-stu-id="7ca76-117">[Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)</span></span>
* <span data-ttu-id="7ca76-118">[Overview of Live Streaming using Azure Media Services](media-services-manage-channels-overview.md) (Az Azure Media Services segítségével történő élő streamelés áttekintése)</span><span class="sxs-lookup"><span data-stu-id="7ca76-118">[Overview of Live Steaming using Azure Media Services](media-services-manage-channels-overview.md)</span></span>
* <span data-ttu-id="7ca76-119">[Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) (Élő stream továbbítása többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal)</span><span class="sxs-lookup"><span data-stu-id="7ca76-119">[Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md)</span></span>

## <span data-ttu-id="7ca76-120"><a id="scenario"></a>Az élő adatfolyamok egy gyakori alkalmazási helyzete</span><span class="sxs-lookup"><span data-stu-id="7ca76-120"><a id="scenario"></a>Common live streaming scenario</span></span>
<span data-ttu-id="7ca76-121">hello lépései streamelő alkalmazásokat, amelyek átmenő közvetítésre konfigurált csatornák létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7ca76-121">hello following steps describe tasks involved in creating common live streaming applications that use channels that are configured for pass-through delivery.</span></span> <span data-ttu-id="7ca76-122">Ez az oktatóanyag bemutatja, hogyan toocreate átmenő csatornát és élő eseményeket és kezelése.</span><span class="sxs-lookup"><span data-stu-id="7ca76-122">This tutorial shows how toocreate and manage a pass-through channel and live events.</span></span>

>[!NOTE]
><span data-ttu-id="7ca76-123">Győződjön meg arról, amelyből el kívánja toostream tartalom streamvégpontra hello hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="7ca76-123">Make sure hello streaming endpoint from which you want toostream content is in hello **Running** state.</span></span> 
    
1. <span data-ttu-id="7ca76-124">Csatlakozás egy videokamerát tooa számítógép.</span><span class="sxs-lookup"><span data-stu-id="7ca76-124">Connect a video camera tooa computer.</span></span> <span data-ttu-id="7ca76-125">Indítson el és konfiguráljon egy élő helyszíni kódolót, amely többszörös sávszélességű RTMP- vagy fragmentált MP4-streamet állít elő.</span><span class="sxs-lookup"><span data-stu-id="7ca76-125">Launch and configure an on-premises live encoder that outputs a multi-bitrate RTMP or Fragmented MP4 stream.</span></span> <span data-ttu-id="7ca76-126">További tájékoztatást az [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824) (Az Azure Media Services RTMP-támogatása és az élő kódolók) című cikk nyújt.</span><span class="sxs-lookup"><span data-stu-id="7ca76-126">For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).</span></span>
   
    <span data-ttu-id="7ca76-127">Ezt a lépést a csatorna létrehozása után is el lehet végezni.</span><span class="sxs-lookup"><span data-stu-id="7ca76-127">This step could also be performed after you create your Channel.</span></span>
2. <span data-ttu-id="7ca76-128">Hozzon létre és indítson el egy átmenő csatornát.</span><span class="sxs-lookup"><span data-stu-id="7ca76-128">Create and start a pass-through Channel.</span></span>
3. <span data-ttu-id="7ca76-129">Kérje le hello csatorna feldolgozó URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="7ca76-129">Retrieve hello Channel ingest URL.</span></span> 
   
    <span data-ttu-id="7ca76-130">hello betöltési URL-cím hello élő kódoló toosend hello adatfolyam toohello csatorna által használt.</span><span class="sxs-lookup"><span data-stu-id="7ca76-130">hello ingest URL is used by hello live encoder toosend hello stream toohello Channel.</span></span>
4. <span data-ttu-id="7ca76-131">Hello csatorna előnézeti URL-cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7ca76-131">Retrieve hello Channel preview URL.</span></span> 
   
    <span data-ttu-id="7ca76-132">Az URL-cím tooverify, hogy a csatornája megfelelően fogadja-hello élő adatfolyam használata.</span><span class="sxs-lookup"><span data-stu-id="7ca76-132">Use this URL tooverify that your channel is properly receiving hello live stream.</span></span>
5. <span data-ttu-id="7ca76-133">Hozzon létre egy élő eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="7ca76-133">Create a live event/program.</span></span> 
   
    <span data-ttu-id="7ca76-134">Az Azure portál használatával hello, amikor élő esemény létrehozása egy objektumot is létrehoz.</span><span class="sxs-lookup"><span data-stu-id="7ca76-134">When using hello Azure portal, creating a live event also creates an asset.</span></span> 

6. <span data-ttu-id="7ca76-135">Amikor készen áll a toostart streamelésre és az archiválásra, indítsa el hello eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="7ca76-135">Start hello event/program when you are ready toostart streaming and archiving.</span></span>
7. <span data-ttu-id="7ca76-136">Másik lehetőségként hello élő kódoló jelzett toostart hirdetést is lehet.</span><span class="sxs-lookup"><span data-stu-id="7ca76-136">Optionally, hello live encoder can be signaled toostart an advertisement.</span></span> <span data-ttu-id="7ca76-137">hello hirdetés bekerül hello kimeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="7ca76-137">hello advertisement is inserted in hello output stream.</span></span>
8. <span data-ttu-id="7ca76-138">Amikor azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény, állítsa le hello eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="7ca76-138">Stop hello event/program whenever you want toostop streaming and archiving hello event.</span></span>
9. <span data-ttu-id="7ca76-139">Törölje a hello eseményt/programot (és választhatóan a hello eszköz törlése).</span><span class="sxs-lookup"><span data-stu-id="7ca76-139">Delete hello event/program (and optionally delete hello asset).</span></span>     

> [!IMPORTANT]
> <span data-ttu-id="7ca76-140">Tekintse át [élő Stream továbbítása helyszíni kódolókkal, amely többféle sávszélességű adatfolyamok létrehozása](media-services-live-streaming-with-onprem-encoders.md) kapcsolatos fogalmakat és szempontokat toolearn kapcsolódó toolive Stream továbbítása helyszíni kódolókkal és átmenő csatornákkal.</span><span class="sxs-lookup"><span data-stu-id="7ca76-140">Please review [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) toolearn about concepts and considerations related toolive streaming with on-premises encoders and pass-through channels.</span></span>
> 
> 

## <a name="tooview-notifications-and-errors"></a><span data-ttu-id="7ca76-141">tooview értesítéseket és hibaüzeneteket</span><span class="sxs-lookup"><span data-stu-id="7ca76-141">tooview notifications and errors</span></span>
<span data-ttu-id="7ca76-142">Ha azt szeretné, hogy tooview értesítések és hibák hello Azure-portálon, kattintson a hello értesítés ikonra.</span><span class="sxs-lookup"><span data-stu-id="7ca76-142">If you want tooview notifications and errors produced by hello Azure portal, click on hello Notification icon.</span></span>

![Értesítések](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a><span data-ttu-id="7ca76-144">Átmenő csatornák és események létrehozása és elindítása.</span><span class="sxs-lookup"><span data-stu-id="7ca76-144">Create and start pass-through channels and events</span></span>
<span data-ttu-id="7ca76-145">A csatorna egy események/programok, amelyek lehetővé teszik a toocontrol hello közzétételét és tárolását élő stream szegmenseinek társítva.</span><span class="sxs-lookup"><span data-stu-id="7ca76-145">A channel is associated with events/programs that enable you toocontrol hello publishing and storage of segments in a live stream.</span></span> <span data-ttu-id="7ca76-146">Az eseményeket a csatornák kezelik.</span><span class="sxs-lookup"><span data-stu-id="7ca76-146">Channels manage events.</span></span> 

<span data-ttu-id="7ca76-147">Megadhatja a hello óraszámon belül hello program hello beállítása kívánt tooretain rögzített hello tartalom **archiválási időtartammal** hossza.</span><span class="sxs-lookup"><span data-stu-id="7ca76-147">You can specify hello number of hours you want tooretain hello recorded content for hello program by setting hello **Archive Window** length.</span></span> <span data-ttu-id="7ca76-148">Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be.</span><span class="sxs-lookup"><span data-stu-id="7ca76-148">This value can be set from a minimum of 5 minutes tooa maximum of 25 hours.</span></span> <span data-ttu-id="7ca76-149">Az archiválási időtartam is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól.</span><span class="sxs-lookup"><span data-stu-id="7ca76-149">Archive window length also dictates hello maximum amount of time clients can seek back in time from hello current live position.</span></span> <span data-ttu-id="7ca76-150">Események hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál.</span><span class="sxs-lookup"><span data-stu-id="7ca76-150">Events can run over hello specified amount of time, but content that falls behind hello window length is continuously discarded.</span></span> <span data-ttu-id="7ca76-151">Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.</span><span class="sxs-lookup"><span data-stu-id="7ca76-151">This value of this property also determines how long hello client manifests can grow.</span></span>

<span data-ttu-id="7ca76-152">Minden esemény egy objektumhoz van társítva.</span><span class="sxs-lookup"><span data-stu-id="7ca76-152">Each event is associated with an asset.</span></span> <span data-ttu-id="7ca76-153">toopublish hello esemény, létre kell hoznia egy OnDemand-kereső hello társított eszköz.</span><span class="sxs-lookup"><span data-stu-id="7ca76-153">toopublish hello event, you must create an OnDemand locator for hello associated asset.</span></span> <span data-ttu-id="7ca76-154">Ez a lokátor lehetővé teszi, hogy a toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.</span><span class="sxs-lookup"><span data-stu-id="7ca76-154">Having this locator enables you toobuild a streaming URL that you can provide tooyour clients.</span></span>

<span data-ttu-id="7ca76-155">Egy csatorna legfeljebb események egyidejűleg fut, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből.</span><span class="sxs-lookup"><span data-stu-id="7ca76-155">A channel supports up toothree concurrently running events so you can create multiple archives of hello same incoming stream.</span></span> <span data-ttu-id="7ca76-156">Ez lehetővé teszi toopublish kapcsolatos és archiválási különböző részei egy eseményt, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="7ca76-156">This allows you toopublish and archive different parts of an event as needed.</span></span> <span data-ttu-id="7ca76-157">Például az üzleti igény szerint tooarchive program, de toobroadcast 6 óra csak az elmúlt 10 perc.</span><span class="sxs-lookup"><span data-stu-id="7ca76-157">For example, your business requirement is tooarchive 6 hours of a program, but toobroadcast only last 10 minutes.</span></span> <span data-ttu-id="7ca76-158">tooaccomplish, toocreate két egyidejűleg zajló program van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7ca76-158">tooaccomplish this, you need toocreate two concurrently running programs.</span></span> <span data-ttu-id="7ca76-159">Egy program van beállítva a tooarchive hello esemény 6 óra, de hello program nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="7ca76-159">One program is set tooarchive 6 hours of hello event but hello program is not published.</span></span> <span data-ttu-id="7ca76-160">hello másik program beállítva tooarchive 10 percig, és a program közzé van téve.</span><span class="sxs-lookup"><span data-stu-id="7ca76-160">hello other program is set tooarchive for 10 minutes and this program is published.</span></span>

<span data-ttu-id="7ca76-161">A meglévő élő eseményeket nem szabad újra felhasználni.</span><span class="sxs-lookup"><span data-stu-id="7ca76-161">You should not reuse existing live events.</span></span> <span data-ttu-id="7ca76-162">Ehelyett hozzon létre egy új eseményt minden eseményhez, és indítsa el.</span><span class="sxs-lookup"><span data-stu-id="7ca76-162">Instead, create and start a new event for each event.</span></span>

<span data-ttu-id="7ca76-163">Amikor készen áll a toostart streamelésre és az archiválásra hello esemény indítása.</span><span class="sxs-lookup"><span data-stu-id="7ca76-163">Start hello event when you are ready toostart streaming and archiving.</span></span> <span data-ttu-id="7ca76-164">Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény.</span><span class="sxs-lookup"><span data-stu-id="7ca76-164">Stop hello program whenever you want toostop streaming and archiving hello event.</span></span> 

<span data-ttu-id="7ca76-165">toodelete archivált tartalmat, állítsa le és hello esemény törlése, és törölje a hozzá társított objektumot hello.</span><span class="sxs-lookup"><span data-stu-id="7ca76-165">toodelete archived content, stop and delete hello event and then delete hello associated asset.</span></span> <span data-ttu-id="7ca76-166">Egy eszköz nem törölhető, ha egy esemény; használják hello esemény először törölni kell.</span><span class="sxs-lookup"><span data-stu-id="7ca76-166">An asset cannot be deleted if it is used by an event; hello event must be deleted first.</span></span> 

<span data-ttu-id="7ca76-167">Állítsa le és a hello esemény törlése után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="7ca76-167">Even after you stop and delete hello event, hello users would be able toostream your archived content as a video on demand, for as long as you do not delete hello asset.</span></span>

<span data-ttu-id="7ca76-168">Ha szeretné tooretain hello archivált tartalom, de nem rendelkezik az adatfolyamként történő elérhetővé, törölje a streamelési locator hello.</span><span class="sxs-lookup"><span data-stu-id="7ca76-168">If you do want tooretain hello archived content, but not have it available for streaming, delete hello streaming locator.</span></span>

### <a name="toouse-hello-portal-toocreate-a-channel"></a><span data-ttu-id="7ca76-169">toouse hello portál toocreate csatorna</span><span class="sxs-lookup"><span data-stu-id="7ca76-169">toouse hello portal toocreate a channel</span></span>
<span data-ttu-id="7ca76-170">Ez a szakasz bemutatja, hogyan toouse hello **Gyorslétrehozás** beállítás toocreate átmenő csatornát.</span><span class="sxs-lookup"><span data-stu-id="7ca76-170">This section shows how toouse hello **Quick Create** option toocreate a pass-through channel.</span></span>

<span data-ttu-id="7ca76-171">Az átmenő csatornákról az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikk nyújt részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="7ca76-171">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

1. <span data-ttu-id="7ca76-172">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="7ca76-172">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="7ca76-173">A hello **beállítások** ablak, kattintson a **Live streaming**.</span><span class="sxs-lookup"><span data-stu-id="7ca76-173">In hello **Settings** window, click **Live streaming**.</span></span> 
   
    ![Bevezetés](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    <span data-ttu-id="7ca76-175">Hello **élő adatfolyam-** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7ca76-175">hello **Live streaming** window appears.</span></span>
3. <span data-ttu-id="7ca76-176">Kattintson a **Gyorslétrehozás** toocreate átmenő csatornát a hello RTMP betöltési protokollt.</span><span class="sxs-lookup"><span data-stu-id="7ca76-176">Click **Quick Create** toocreate a pass-through channel with hello RTMP ingest protocol.</span></span>
   
    <span data-ttu-id="7ca76-177">Hello **új csatorna létrehozása** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7ca76-177">hello **CREATE A NEW CHANNEL** window appears.</span></span>
4. <span data-ttu-id="7ca76-178">Nevezze el hello új csatornát, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7ca76-178">Give hello new channel a name and click **Create**.</span></span> 
   
    <span data-ttu-id="7ca76-179">Ez egy átmenő csatornát hello hoz létre RTMP betöltési protokollt.</span><span class="sxs-lookup"><span data-stu-id="7ca76-179">This creates a pass-through channel with hello RTMP ingest protocol.</span></span>

## <a name="create-events"></a><span data-ttu-id="7ca76-180">Események létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ca76-180">Create events</span></span>
1. <span data-ttu-id="7ca76-181">Válassza ki a csatorna toowhich kívánt tooadd egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="7ca76-181">Select a channel toowhich you want tooadd an event.</span></span>
2. <span data-ttu-id="7ca76-182">Kattintson a **Live Event** (Élő esemény) gombra.</span><span class="sxs-lookup"><span data-stu-id="7ca76-182">Press **Live Event** button.</span></span>

![Esemény](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a><span data-ttu-id="7ca76-184">A betöltési URL-címek beolvasása</span><span class="sxs-lookup"><span data-stu-id="7ca76-184">Get ingest URLs</span></span>
<span data-ttu-id="7ca76-185">Hello csatorna létrehozása után kaphat a betöltési URL-címeket adja meg az élő kódoló toohello.</span><span class="sxs-lookup"><span data-stu-id="7ca76-185">Once hello channel is created, you can get ingest URLs that you will provide toohello live encoder.</span></span> <span data-ttu-id="7ca76-186">a kódoló hello ezen URL-címek tooinput élő adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="7ca76-186">hello encoder uses these URLs tooinput a live stream.</span></span>

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-hello-event"></a><span data-ttu-id="7ca76-188">A figyelési hello esemény</span><span class="sxs-lookup"><span data-stu-id="7ca76-188">Watch hello event</span></span>
<span data-ttu-id="7ca76-189">toowatch hello esemény, kattintson a **figyelési** a streamelési URL-cím az Azure-portál vagy a példány hello hello és a Windows Media Player az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="7ca76-189">toowatch hello event, click **Watch** in hello Azure portal or copy hello streaming URL and use a player of your choice.</span></span> 

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

<span data-ttu-id="7ca76-191">Élő esemény automatikusan megkapja az átalakított tooon igény szerinti tartalomterjesztésről leállításakor.</span><span class="sxs-lookup"><span data-stu-id="7ca76-191">Live event automatically get converted tooon-demand content when stopped.</span></span>

## <a name="clean-up"></a><span data-ttu-id="7ca76-192">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7ca76-192">Clean up</span></span>
<span data-ttu-id="7ca76-193">Az átmenő csatornákról az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikk nyújt részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="7ca76-193">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

* <span data-ttu-id="7ca76-194">Egy csatornát csak akkor, ha le lett állítva az összes események/programok hello csatornán állítható le.</span><span class="sxs-lookup"><span data-stu-id="7ca76-194">A channel can be stopped only when all events/programs on hello channel have been stopped.</span></span>  <span data-ttu-id="7ca76-195">Hello csatorna leállítását követően nem számítunk fel díjakat.</span><span class="sxs-lookup"><span data-stu-id="7ca76-195">Once hello Channel is stopped, it does not incur any charges.</span></span> <span data-ttu-id="7ca76-196">Ha toostart kell azt újra, hogy fog rendelkezni hello azonos feldolgozó URL-CÍMÉT, akkor nem kell tooreconfigure a kódoló.</span><span class="sxs-lookup"><span data-stu-id="7ca76-196">When you need toostart it again, it will have hello same ingest URL so you won't need tooreconfigure your encoder.</span></span>
* <span data-ttu-id="7ca76-197">Egy csatorna törölhetők, csak akkor, ha hello csatornán összes élő eseményt törölték.</span><span class="sxs-lookup"><span data-stu-id="7ca76-197">A channel can be deleted only when all live events on hello channel have been deleted.</span></span>

## <a name="view-archived-content"></a><span data-ttu-id="7ca76-198">Archivált tartalom megtekintése</span><span class="sxs-lookup"><span data-stu-id="7ca76-198">View archived content</span></span>
<span data-ttu-id="7ca76-199">Állítsa le és a hello esemény törlése után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="7ca76-199">Even after you stop and delete hello event, hello users would be able toostream your archived content as a video on demand, for as long as you do not delete hello asset.</span></span> <span data-ttu-id="7ca76-200">Egy eszköz nem törölhető, ha egy esemény; használják hello esemény először törölni kell.</span><span class="sxs-lookup"><span data-stu-id="7ca76-200">An asset cannot be deleted if it is used by an event; hello event must be deleted first.</span></span> 

<span data-ttu-id="7ca76-201">toomanage az objektumok kiválasztása **beállítás** kattintson **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="7ca76-201">toomanage your assets, select **Setting** and click **Assets**.</span></span>

![Objektumok](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a><span data-ttu-id="7ca76-203">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="7ca76-203">Next step</span></span>
<span data-ttu-id="7ca76-204">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="7ca76-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7ca76-205">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7ca76-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

