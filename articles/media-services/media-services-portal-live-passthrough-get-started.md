---
title: "Élő stream továbbítása helyszíni kódolókkal az Azure Portal használatával | Microsoft Docs"
description: "Ez az ismertető végigkalauzolja egy olyan csatorna létrehozásának folyamatán, amely átmenő közvetítésre van konfigurálva."
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
ms.openlocfilehash: 6939e3b31c3c1b514df4c559c2d9408fce122a4e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-perform-live-streaming-with-on-premises-encoders-using-the-azure-portal"></a><span data-ttu-id="544ad-103">Élő stream továbbítása helyszíni kódolókkal az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="544ad-103">How to perform live streaming with on-premises encoders using the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="544ad-104">Portal</span><span class="sxs-lookup"><span data-stu-id="544ad-104">Portal</span></span>](media-services-portal-live-passthrough-get-started.md)
> * [<span data-ttu-id="544ad-105">.NET</span><span class="sxs-lookup"><span data-stu-id="544ad-105">.NET</span></span>](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [<span data-ttu-id="544ad-106">REST</span><span class="sxs-lookup"><span data-stu-id="544ad-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

<span data-ttu-id="544ad-107">Ez az ismertető végigkalauzolja egy olyan **csatorna** létrehozásának folyamatán, amely átmenő közvetítésre van konfigurálva az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="544ad-107">This tutorial walks you through the steps of using the Azure portal to create a **Channel** that is configured for a pass-through delivery.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="544ad-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="544ad-108">Prerequisites</span></span>
<span data-ttu-id="544ad-109">Az ismertetett eljárás végrehajtásához a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="544ad-109">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="544ad-110">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="544ad-110">An Azure account.</span></span> <span data-ttu-id="544ad-111">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="544ad-111">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="544ad-112">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="544ad-112">A Media Services account.</span></span> <span data-ttu-id="544ad-113">A Media Services-fiók létrehozásáról a [Media Services-fiók létrehozása](media-services-portal-create-account.md) című cikk nyújt tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="544ad-113">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="544ad-114">Egy webkamera.</span><span class="sxs-lookup"><span data-stu-id="544ad-114">A webcam.</span></span> <span data-ttu-id="544ad-115">Például a [Telestream Wirecast kódoló](http://www.telestream.net/wirecast/overview.htm).</span><span class="sxs-lookup"><span data-stu-id="544ad-115">For example, [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).</span></span>

<span data-ttu-id="544ad-116">Kifejezetten ajánljuk, hogy olvassa el a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="544ad-116">It is highly recommended to review the following articles:</span></span>

* <span data-ttu-id="544ad-117">[Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/) (Az Azure Media Services RTMP-támogatása és az élő kódolók)</span><span class="sxs-lookup"><span data-stu-id="544ad-117">[Azure Media Services RTMP Support and Live Encoders](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)</span></span>
* <span data-ttu-id="544ad-118">[Overview of Live Streaming using Azure Media Services](media-services-manage-channels-overview.md) (Az Azure Media Services segítségével történő élő streamelés áttekintése)</span><span class="sxs-lookup"><span data-stu-id="544ad-118">[Overview of Live Steaming using Azure Media Services](media-services-manage-channels-overview.md)</span></span>
* <span data-ttu-id="544ad-119">[Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) (Élő stream továbbítása többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal)</span><span class="sxs-lookup"><span data-stu-id="544ad-119">[Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md)</span></span>

## <span data-ttu-id="544ad-120"><a id="scenario"></a>Az élő adatfolyamok egy gyakori alkalmazási helyzete</span><span class="sxs-lookup"><span data-stu-id="544ad-120"><a id="scenario"></a>Common live streaming scenario</span></span>
<span data-ttu-id="544ad-121">A következő lépések ismertetik, hogy milyen lépésekkel lehet olyan streamelő alkalmazásokat létrehozni, amelyek átmenő közvetítésre vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="544ad-121">The following steps describe tasks involved in creating common live streaming applications that use channels that are configured for pass-through delivery.</span></span> <span data-ttu-id="544ad-122">Ez az oktatóprogram bemutatja, hogyan hozhat létre és kezelhet átmenő csatornát és élő eseményeket.</span><span class="sxs-lookup"><span data-stu-id="544ad-122">This tutorial shows how to create and manage a pass-through channel and live events.</span></span>

>[!NOTE]
><span data-ttu-id="544ad-123">Győződjön meg arról, hogy a tartalomstreameléshez használt streamvégpont **Fut** állapotban legyen.</span><span class="sxs-lookup"><span data-stu-id="544ad-123">Make sure the streaming endpoint from which you want to stream content is in the **Running** state.</span></span> 
    
1. <span data-ttu-id="544ad-124">Csatlakoztasson egy videokamerát a számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="544ad-124">Connect a video camera to a computer.</span></span> <span data-ttu-id="544ad-125">Indítson el és konfiguráljon egy élő helyszíni kódolót, amely többszörös sávszélességű RTMP- vagy fragmentált MP4-streamet állít elő.</span><span class="sxs-lookup"><span data-stu-id="544ad-125">Launch and configure an on-premises live encoder that outputs a multi-bitrate RTMP or Fragmented MP4 stream.</span></span> <span data-ttu-id="544ad-126">További tájékoztatást az [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824) (Az Azure Media Services RTMP-támogatása és az élő kódolók) című cikk nyújt.</span><span class="sxs-lookup"><span data-stu-id="544ad-126">For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).</span></span>
   
    <span data-ttu-id="544ad-127">Ezt a lépést a csatorna létrehozása után is el lehet végezni.</span><span class="sxs-lookup"><span data-stu-id="544ad-127">This step could also be performed after you create your Channel.</span></span>
2. <span data-ttu-id="544ad-128">Hozzon létre és indítson el egy átmenő csatornát.</span><span class="sxs-lookup"><span data-stu-id="544ad-128">Create and start a pass-through Channel.</span></span>
3. <span data-ttu-id="544ad-129">Kérje le a Channel ingest URL (Csatorna betöltési URL-címe) értékét.</span><span class="sxs-lookup"><span data-stu-id="544ad-129">Retrieve the Channel ingest URL.</span></span> 
   
    <span data-ttu-id="544ad-130">Az élő kódoló a bemeneti URL-címet használva küldi el a streamet a csatornának.</span><span class="sxs-lookup"><span data-stu-id="544ad-130">The ingest URL is used by the live encoder to send the stream to the Channel.</span></span>
4. <span data-ttu-id="544ad-131">Kérje le a csatorna előnézeti URL-címét.</span><span class="sxs-lookup"><span data-stu-id="544ad-131">Retrieve the Channel preview URL.</span></span> 
   
    <span data-ttu-id="544ad-132">Ezen az URL használatával ellenőrizheti, hogy a csatornája megfelelően fogadja-e az élő adatfolyamot.</span><span class="sxs-lookup"><span data-stu-id="544ad-132">Use this URL to verify that your channel is properly receiving the live stream.</span></span>
5. <span data-ttu-id="544ad-133">Hozzon létre egy élő eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="544ad-133">Create a live event/program.</span></span> 
   
    <span data-ttu-id="544ad-134">Az Azure portál használata esetén az élő esemény létrehozása egy objektumot is létrehoz.</span><span class="sxs-lookup"><span data-stu-id="544ad-134">When using the Azure portal, creating a live event also creates an asset.</span></span> 

6. <span data-ttu-id="544ad-135">Amikor készen áll a streamelésre és az archiválásra, indítsa el az eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="544ad-135">Start the event/program when you are ready to start streaming and archiving.</span></span>
7. <span data-ttu-id="544ad-136">Ha kívánja, a kódolólónak küldött jelzéssel hirdetést is elindíthat.</span><span class="sxs-lookup"><span data-stu-id="544ad-136">Optionally, the live encoder can be signaled to start an advertisement.</span></span> <span data-ttu-id="544ad-137">A hirdetés bekerül a kimenő streambe.</span><span class="sxs-lookup"><span data-stu-id="544ad-137">The advertisement is inserted in the output stream.</span></span>
8. <span data-ttu-id="544ad-138">Amikor le kívánja állítani az esemény streamelését és archiválását, állítsa le az eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="544ad-138">Stop the event/program whenever you want to stop streaming and archiving the event.</span></span>
9. <span data-ttu-id="544ad-139">Törölje az eseményt/programot (és ha kívánja, törölje az objektumot).</span><span class="sxs-lookup"><span data-stu-id="544ad-139">Delete the event/program (and optionally delete the asset).</span></span>     

> [!IMPORTANT]
> <span data-ttu-id="544ad-140">Az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikkből tájékozódhat a helyszíni kódolókkal és átmenő csatornákkal történő élő streamelés fogalmairól és fontos szempontjairól.</span><span class="sxs-lookup"><span data-stu-id="544ad-140">Please review [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) to learn about concepts and considerations related to live streaming with on-premises encoders and pass-through channels.</span></span>
> 
> 

## <a name="to-view-notifications-and-errors"></a><span data-ttu-id="544ad-141">Az értesítések és a hibák megtekintése</span><span class="sxs-lookup"><span data-stu-id="544ad-141">To view notifications and errors</span></span>
<span data-ttu-id="544ad-142">Ha meg szeretné tekinteni az Azure portál által előállított értesítéseket és hibaüzeneteket, kattintson az Értesítés ikonra.</span><span class="sxs-lookup"><span data-stu-id="544ad-142">If you want to view notifications and errors produced by the Azure portal, click on the Notification icon.</span></span>

![Értesítések](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a><span data-ttu-id="544ad-144">Átmenő csatornák és események létrehozása és elindítása.</span><span class="sxs-lookup"><span data-stu-id="544ad-144">Create and start pass-through channels and events</span></span>
<span data-ttu-id="544ad-145">A csatornákhoz események/programok vannak társítva. Ezek lehetővé teszik az élő stream szegmenseinek közzétételét és tárolását.</span><span class="sxs-lookup"><span data-stu-id="544ad-145">A channel is associated with events/programs that enable you to control the publishing and storage of segments in a live stream.</span></span> <span data-ttu-id="544ad-146">Az eseményeket a csatornák kezelik.</span><span class="sxs-lookup"><span data-stu-id="544ad-146">Channels manage events.</span></span> 

<span data-ttu-id="544ad-147">Az **Archive Window** (Archiválás időtartama) beállításnál megadhatja, hogy hány órára szeretné megőrizni a program felvett tartalmát.</span><span class="sxs-lookup"><span data-stu-id="544ad-147">You can specify the number of hours you want to retain the recorded content for the program by setting the **Archive Window** length.</span></span> <span data-ttu-id="544ad-148">Ez az érték 5 perc és 25 óra közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="544ad-148">This value can be set from a minimum of 5 minutes to a maximum of 25 hours.</span></span> <span data-ttu-id="544ad-149">Az archiválási időtartam határozza meg azt is, hogy mennyi idővel ugorhatnak vissza az ügyfelek az aktuális élő pozíciótól.</span><span class="sxs-lookup"><span data-stu-id="544ad-149">Archive window length also dictates the maximum amount of time clients can seek back in time from the current live position.</span></span> <span data-ttu-id="544ad-150">Az események hosszabbak lehetnek a megadott időtartamnál, de a rendszer folyamatosan eldobja azt a tartalmat, amely korábbi a megadott időtartamnál.</span><span class="sxs-lookup"><span data-stu-id="544ad-150">Events can run over the specified amount of time, but content that falls behind the window length is continuously discarded.</span></span> <span data-ttu-id="544ad-151">Ennek a tulajdonságnak az értéke határozza meg azt is, hogy milyen hosszúra nőhetnek az ügyfél jegyzékfájljai.</span><span class="sxs-lookup"><span data-stu-id="544ad-151">This value of this property also determines how long the client manifests can grow.</span></span>

<span data-ttu-id="544ad-152">Minden esemény egy objektumhoz van társítva.</span><span class="sxs-lookup"><span data-stu-id="544ad-152">Each event is associated with an asset.</span></span> <span data-ttu-id="544ad-153">Az esemény közzétételéhez létre kell hoznia egy OnDemand-lokátort a társított objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="544ad-153">To publish the event, you must create an OnDemand locator for the associated asset.</span></span> <span data-ttu-id="544ad-154">Ez a lokátor teszi lehetővé az ügyfelek számára megadható streamelő URL-cím összeállítását.</span><span class="sxs-lookup"><span data-stu-id="544ad-154">Having this locator enables you to build a streaming URL that you can provide to your clients.</span></span>

<span data-ttu-id="544ad-155">Egy csatorna három egyidejűleg zajló esemény támogat, hogy több archívumot lehessen létrehozni ugyanabból a bejövő streamből.</span><span class="sxs-lookup"><span data-stu-id="544ad-155">A channel supports up to three concurrently running events so you can create multiple archives of the same incoming stream.</span></span> <span data-ttu-id="544ad-156">Ez lehetővé teszi az események különféle részeinek szükség szerinti közzétételét és archiválását.</span><span class="sxs-lookup"><span data-stu-id="544ad-156">This allows you to publish and archive different parts of an event as needed.</span></span> <span data-ttu-id="544ad-157">Az üzleti igény szerint például 6 órát kell archiválni egy programból, de csak az utolsó 10 percet kell közvetíteni.</span><span class="sxs-lookup"><span data-stu-id="544ad-157">For example, your business requirement is to archive 6 hours of a program, but to broadcast only last 10 minutes.</span></span> <span data-ttu-id="544ad-158">Ezt két egyidejűleg zajló program létrehozásával érheti el.</span><span class="sxs-lookup"><span data-stu-id="544ad-158">To accomplish this, you need to create two concurrently running programs.</span></span> <span data-ttu-id="544ad-159">Ebben az esetben állítsa be az egyik programot az esemény 6 órájának archiválására, de ne tegye közzé.</span><span class="sxs-lookup"><span data-stu-id="544ad-159">One program is set to archive 6 hours of the event but the program is not published.</span></span> <span data-ttu-id="544ad-160">A másik programot 10 perc archiválására állítsa be, és tegye is közzé.</span><span class="sxs-lookup"><span data-stu-id="544ad-160">The other program is set to archive for 10 minutes and this program is published.</span></span>

<span data-ttu-id="544ad-161">A meglévő élő eseményeket nem szabad újra felhasználni.</span><span class="sxs-lookup"><span data-stu-id="544ad-161">You should not reuse existing live events.</span></span> <span data-ttu-id="544ad-162">Ehelyett hozzon létre egy új eseményt minden eseményhez, és indítsa el.</span><span class="sxs-lookup"><span data-stu-id="544ad-162">Instead, create and start a new event for each event.</span></span>

<span data-ttu-id="544ad-163">Amikor készen áll a streamelésre és az archiválásra, indítsa el az eseményt.</span><span class="sxs-lookup"><span data-stu-id="544ad-163">Start the event when you are ready to start streaming and archiving.</span></span> <span data-ttu-id="544ad-164">Állítsa le a programot, ha szeretné megállítani az adatfolyam-továbbítást, és archiválni kívánja az eseményt.</span><span class="sxs-lookup"><span data-stu-id="544ad-164">Stop the program whenever you want to stop streaming and archiving the event.</span></span> 

<span data-ttu-id="544ad-165">Az archivált tartalom törléséhez állítsa le és törölje az eseményt, majd törölje a hozzá társított objektumot.</span><span class="sxs-lookup"><span data-stu-id="544ad-165">To delete archived content, stop and delete the event and then delete the associated asset.</span></span> <span data-ttu-id="544ad-166">Olyan objektumot nem lehet törölni, amelyet használ egy esemény. Először az eseményt kell törölni.</span><span class="sxs-lookup"><span data-stu-id="544ad-166">An asset cannot be deleted if it is used by an event; the event must be deleted first.</span></span> 

<span data-ttu-id="544ad-167">Ha már leállította és törölte is az eseményt, a felhasználók igény szerinti videóként le tudják játszani az archivált tartalmat mindaddig, amíg az objektumot nem törli.</span><span class="sxs-lookup"><span data-stu-id="544ad-167">Even after you stop and delete the event, the users would be able to stream your archived content as a video on demand, for as long as you do not delete the asset.</span></span>

<span data-ttu-id="544ad-168">Ha szeretné megtartani az archivált tartalmakat, de nem szeretné elérhetővé tenni őket streamelésre, törölje a streamelési lokátort.</span><span class="sxs-lookup"><span data-stu-id="544ad-168">If you do want to retain the archived content, but not have it available for streaming, delete the streaming locator.</span></span>

### <a name="to-use-the-portal-to-create-a-channel"></a><span data-ttu-id="544ad-169">Csatorna létrehozása a portállal</span><span class="sxs-lookup"><span data-stu-id="544ad-169">To use the portal to create a channel</span></span>
<span data-ttu-id="544ad-170">Ez a szakasz azt mutatja be, hogyan lehet átmenő csatornát létrehozni a **Gyorslétrehozás** beállítással.</span><span class="sxs-lookup"><span data-stu-id="544ad-170">This section shows how to use the **Quick Create** option to create a pass-through channel.</span></span>

<span data-ttu-id="544ad-171">Az átmenő csatornákról az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikk nyújt részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="544ad-171">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

1. <span data-ttu-id="544ad-172">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="544ad-172">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="544ad-173">Kattintson a **Settings** (Beállítások) ablak **Live streaming** (Élő stream) elemére.</span><span class="sxs-lookup"><span data-stu-id="544ad-173">In the **Settings** window, click **Live streaming**.</span></span> 
   
    ![Bevezetés](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    <span data-ttu-id="544ad-175">Megjelenik a **Live streaming** (Élő stream) ablak.</span><span class="sxs-lookup"><span data-stu-id="544ad-175">The **Live streaming** window appears.</span></span>
3. <span data-ttu-id="544ad-176">A **Quick Create** (Gyorslétrehozás) gombra kattintva létrehozhat egy az RTMP betöltési protokollt használó átmenő csatornát.</span><span class="sxs-lookup"><span data-stu-id="544ad-176">Click **Quick Create** to create a pass-through channel with the RTMP ingest protocol.</span></span>
   
    <span data-ttu-id="544ad-177">Megjelenik a **CREATE A NEW CHANNEL** (Új csatorna létrehozása) ablak.</span><span class="sxs-lookup"><span data-stu-id="544ad-177">The **CREATE A NEW CHANNEL** window appears.</span></span>
4. <span data-ttu-id="544ad-178">Nevezze el az új csatornát, és kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="544ad-178">Give the new channel a name and click **Create**.</span></span> 
   
    <span data-ttu-id="544ad-179">Ennek hatására létrejön egy, az RTMP-betöltési protokollt használó csatorna.</span><span class="sxs-lookup"><span data-stu-id="544ad-179">This creates a pass-through channel with the RTMP ingest protocol.</span></span>

## <a name="create-events"></a><span data-ttu-id="544ad-180">Események létrehozása</span><span class="sxs-lookup"><span data-stu-id="544ad-180">Create events</span></span>
1. <span data-ttu-id="544ad-181">Válassza ki azt a csatornát, amelyhez eseményt szeretne hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="544ad-181">Select a channel to which you want to add an event.</span></span>
2. <span data-ttu-id="544ad-182">Kattintson a **Live Event** (Élő esemény) gombra.</span><span class="sxs-lookup"><span data-stu-id="544ad-182">Press **Live Event** button.</span></span>

![Esemény](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a><span data-ttu-id="544ad-184">A betöltési URL-címek beolvasása</span><span class="sxs-lookup"><span data-stu-id="544ad-184">Get ingest URLs</span></span>
<span data-ttu-id="544ad-185">A csatorna létrehozása után beolvashatja a betöltési URL-címeket. Ezeket kell megadnia az élő kódolónak.</span><span class="sxs-lookup"><span data-stu-id="544ad-185">Once the channel is created, you can get ingest URLs that you will provide to the live encoder.</span></span> <span data-ttu-id="544ad-186">A kódoló ezekre az URL-címekre küldi a bemeneti élő streamet.</span><span class="sxs-lookup"><span data-stu-id="544ad-186">The encoder uses these URLs to input a live stream.</span></span>

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-the-event"></a><span data-ttu-id="544ad-188">Esemény megtekintése</span><span class="sxs-lookup"><span data-stu-id="544ad-188">Watch the event</span></span>
<span data-ttu-id="544ad-189">Ha meg szeretne tekinteni egy eseményt, kattintson az Azure Portal **Watch** (Megtekintés) elemére.</span><span class="sxs-lookup"><span data-stu-id="544ad-189">To watch the event, click **Watch** in the Azure portal or copy the streaming URL and use a player of your choice.</span></span> 

![Létrehozva](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

<span data-ttu-id="544ad-191">Leállítása után az élő esemény automatikusan átalakul igény szerinti tartalommá.</span><span class="sxs-lookup"><span data-stu-id="544ad-191">Live event automatically get converted to on-demand content when stopped.</span></span>

## <a name="clean-up"></a><span data-ttu-id="544ad-192">A fölöslegessé vált elemek eltávolítása</span><span class="sxs-lookup"><span data-stu-id="544ad-192">Clean up</span></span>
<span data-ttu-id="544ad-193">Az átmenő csatornákról az [élő stream többszörös átviteli sebességű streamet létrehozó helyszíni kódolókkal történő továbbítását](media-services-live-streaming-with-onprem-encoders.md) ismertető cikk nyújt részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="544ad-193">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

* <span data-ttu-id="544ad-194">Egy csatornát csak akkor lehet  leállítani, ha már leállítottak minden, a csatornán közvetített eseményt/programot.</span><span class="sxs-lookup"><span data-stu-id="544ad-194">A channel can be stopped only when all events/programs on the channel have been stopped.</span></span>  <span data-ttu-id="544ad-195">A csatorna leállítását követően nem számítunk fel további díjakat.</span><span class="sxs-lookup"><span data-stu-id="544ad-195">Once the Channel is stopped, it does not incur any charges.</span></span> <span data-ttu-id="544ad-196">A betöltési URL-cím nem módosul, ezért a csatorna ismételt elindításához nem szükséges újrakonfigurálni a kódolót.</span><span class="sxs-lookup"><span data-stu-id="544ad-196">When you need to start it again, it will have the same ingest URL so you won't need to reconfigure your encoder.</span></span>
* <span data-ttu-id="544ad-197">Egy csatornát csak akkor lehet törölni, ha töröltek minden, a csatornán közvetített élő eseményt.</span><span class="sxs-lookup"><span data-stu-id="544ad-197">A channel can be deleted only when all live events on the channel have been deleted.</span></span>

## <a name="view-archived-content"></a><span data-ttu-id="544ad-198">Archivált tartalom megtekintése</span><span class="sxs-lookup"><span data-stu-id="544ad-198">View archived content</span></span>
<span data-ttu-id="544ad-199">Ha már leállította és törölte is az eseményt, a felhasználók igény szerinti videóként le tudják játszani az archivált tartalmat mindaddig, amíg az objektumot nem törli.</span><span class="sxs-lookup"><span data-stu-id="544ad-199">Even after you stop and delete the event, the users would be able to stream your archived content as a video on demand, for as long as you do not delete the asset.</span></span> <span data-ttu-id="544ad-200">Olyan objektumot nem lehet törölni, amelyet használ egy esemény. Először az eseményt kell törölni.</span><span class="sxs-lookup"><span data-stu-id="544ad-200">An asset cannot be deleted if it is used by an event; the event must be deleted first.</span></span> 

<span data-ttu-id="544ad-201">Az objektumok kezeléséhez válassza a **Setting** (Beállítás) elemet, majd kattintson az **Assets** (Objektumok) elemre.</span><span class="sxs-lookup"><span data-stu-id="544ad-201">To manage your assets, select **Setting** and click **Assets**.</span></span>

![Objektumok](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a><span data-ttu-id="544ad-203">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="544ad-203">Next step</span></span>
<span data-ttu-id="544ad-204">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="544ad-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="544ad-205">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="544ad-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

