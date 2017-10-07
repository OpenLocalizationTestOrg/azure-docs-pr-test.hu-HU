---
title: "aaaHow tooperform élő adatfolyam-Azure Media Services toocreate többféle sávszélességű adatfolyamok használata a .NET |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan hello lépéseit egy olyan csatorna létrehozásának egy egyféle sávszélességű élő streamet és a .NET SDK használatával toomulti sávszélességűvé kódolja."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 4df5e690-ff63-47cc-879b-9c57cb8ec240
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 22088e6a78a49bd839575614a7c17a411ae8081c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams-with-net"></a><span data-ttu-id="c4909-103">A .NET hogyan tooperform élő adatfolyam-használó Azure Media Services toocreate többféle sávszélességű adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="c4909-103">How tooperform live streaming using Azure Media Services toocreate multi-bitrate streams with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c4909-104">Portal</span><span class="sxs-lookup"><span data-stu-id="c4909-104">Portal</span></span>](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [<span data-ttu-id="c4909-105">.NET</span><span class="sxs-lookup"><span data-stu-id="c4909-105">.NET</span></span>](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [<span data-ttu-id="c4909-106">REST API</span><span class="sxs-lookup"><span data-stu-id="c4909-106">REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> [!NOTE]
> <span data-ttu-id="c4909-107">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c4909-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="c4909-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c4909-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="c4909-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c4909-109">Overview</span></span>
<span data-ttu-id="c4909-110">Ez az oktatóanyag végigvezeti hello létrehozásának egy **csatorna** , amely egy egyszeres sávszélességű élő streamet és toomulti sávszélességűvé kódolja.</span><span class="sxs-lookup"><span data-stu-id="c4909-110">This tutorial walks you through hello steps of creating a **Channel** that receives a single-bitrate live stream and encodes it toomulti-bitrate stream.</span></span>

<span data-ttu-id="c4909-111">További információ az élő kódolás engedélyezett kapcsolódó tooChannels lásd [használó Azure Media Services toocreate többféle sávszélességű adatfolyamot élő Stream továbbítása](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="c4909-111">For more conceptual information related tooChannels that are enabled for live encoding, see [Live streaming using Azure Media Services toocreate multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md).</span></span>

## <a name="common-live-streaming-scenario"></a><span data-ttu-id="c4909-112">Gyakori élő adatfolyam-továbbítási forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="c4909-112">Common Live Streaming Scenario</span></span>
<span data-ttu-id="c4909-113">a lépéseket követve hello gyakori élő adatfolyam-továbbítási alkalmazások létrehozásához elvégzendő feladatokat írják le.</span><span class="sxs-lookup"><span data-stu-id="c4909-113">hello following steps describe tasks involved in creating common live streaming applications.</span></span>

> [!NOTE]
> <span data-ttu-id="c4909-114">Maximális hello élő esemény időtartama ajánlott 8 órára is jelenleg.</span><span class="sxs-lookup"><span data-stu-id="c4909-114">Currently, hello max recommended duration of a live event is 8 hours.</span></span> <span data-ttu-id="c4909-115">Ha hosszabb ideig kell toorun egy csatornát, forduljon amslived@Microsoft.com e-mail címen.</span><span class="sxs-lookup"><span data-stu-id="c4909-115">Please contact amslived at Microsoft.com if you need toorun a Channel for longer periods of time.</span></span>
> 
> 

1. <span data-ttu-id="c4909-116">Csatlakozás egy videokamerát tooa számítógép.</span><span class="sxs-lookup"><span data-stu-id="c4909-116">Connect a video camera tooa computer.</span></span> <span data-ttu-id="c4909-117">Indítson el és konfiguráljon egy helyszíni élő kódoló, amely a kimenetre küldheti egyféle sávszélességű adatfolyamot hello a következő protokollok valamelyikével: RTMP, Smooth Streaming vagy RTP (MPEG-TS).</span><span class="sxs-lookup"><span data-stu-id="c4909-117">Launch and configure an on-premises live encoder that can output a single bitrate stream in one of hello following protocols: RTMP, Smooth Streaming, or RTP (MPEG-TS).</span></span> <span data-ttu-id="c4909-118">További tudnivalók: [Azure Media Services RMTP-támogatása és valós idejű kódolók](http://go.microsoft.com/fwlink/?LinkId=532824)</span><span class="sxs-lookup"><span data-stu-id="c4909-118">For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).</span></span>

    <span data-ttu-id="c4909-119">Ezt a lépést a csatorna létrehozása után is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="c4909-119">This step could also be performed after you create your Channel.</span></span>

2. <span data-ttu-id="c4909-120">Hozzon létre és indítson el egy csatornát.</span><span class="sxs-lookup"><span data-stu-id="c4909-120">Create and start a Channel.</span></span>
3. <span data-ttu-id="c4909-121">Kérje le hello csatorna feldolgozó URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c4909-121">Retrieve hello Channel ingest URL.</span></span>

    <span data-ttu-id="c4909-122">hello betöltési URL-cím hello élő kódoló toosend hello adatfolyam toohello csatorna által használt.</span><span class="sxs-lookup"><span data-stu-id="c4909-122">hello ingest URL is used by hello live encoder toosend hello stream toohello Channel.</span></span>

4. <span data-ttu-id="c4909-123">Hello csatorna előnézeti URL-cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c4909-123">Retrieve hello Channel preview URL.</span></span>

    <span data-ttu-id="c4909-124">Az URL-cím tooverify, hogy a csatornája megfelelően fogadja-hello élő adatfolyam használata.</span><span class="sxs-lookup"><span data-stu-id="c4909-124">Use this URL tooverify that your channel is properly receiving hello live stream.</span></span>

5. <span data-ttu-id="c4909-125">Hozzon létre egy adategységet.</span><span class="sxs-lookup"><span data-stu-id="c4909-125">Create an asset.</span></span>
6. <span data-ttu-id="c4909-126">Ha azt szeretné, a lejátszás során dinamikusan titkosítani hello eszköz toobe, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="c4909-126">If you want for hello asset toobe dynamically encrypted during playback, do hello following:</span></span>
7. <span data-ttu-id="c4909-127">Hozzon létre egy tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="c4909-127">Create a content key.</span></span>
8. <span data-ttu-id="c4909-128">Hello tartalomkulcs hitelesítési szabályzatának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c4909-128">Configure hello content key's authorization policy.</span></span>
9. <span data-ttu-id="c4909-129">Konfigurálja az adategység továbbítási házirendjét (amelyet a dinamikus csomagolás és a dinamikus titkosítás használ).</span><span class="sxs-lookup"><span data-stu-id="c4909-129">Configure asset delivery policy (used by dynamic packaging and dynamic encryption).</span></span>
10. <span data-ttu-id="c4909-130">Hozzon létre egy programot, és adja meg a létrehozott toouse hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="c4909-130">Create a program and specify toouse hello asset that you created.</span></span>
11. <span data-ttu-id="c4909-131">Tegye közzé hello adategységet egy OnDemand-kereső létrehozásával hello programhoz társított.</span><span class="sxs-lookup"><span data-stu-id="c4909-131">Publish hello asset associated with hello program by creating an OnDemand locator.</span></span>

    >[!NOTE]
    ><span data-ttu-id="c4909-132">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="c4909-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="c4909-133">adatfolyam-továbbítási végpontra, amelyből el kívánja toostream tartalom hello toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="c4909-133">hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

12. <span data-ttu-id="c4909-134">Amikor készen áll a toostart streamelésre és az archiválásra, indítsa el hello programot.</span><span class="sxs-lookup"><span data-stu-id="c4909-134">Start hello program when you are ready toostart streaming and archiving.</span></span>
13. <span data-ttu-id="c4909-135">Másik lehetőségként hello élő kódoló jelzett toostart hirdetést is lehet.</span><span class="sxs-lookup"><span data-stu-id="c4909-135">Optionally, hello live encoder can be signaled toostart an advertisement.</span></span> <span data-ttu-id="c4909-136">hello hirdetés bekerül hello kimeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="c4909-136">hello advertisement is inserted in hello output stream.</span></span>
14. <span data-ttu-id="c4909-137">Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény.</span><span class="sxs-lookup"><span data-stu-id="c4909-137">Stop hello program whenever you want toostop streaming and archiving hello event.</span></span>
15. <span data-ttu-id="c4909-138">Törli a hello Program (és opcionálisan a hello eszköz törlése).</span><span class="sxs-lookup"><span data-stu-id="c4909-138">Delete hello Program (and optionally delete hello asset).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="c4909-139">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="c4909-139">What you'll learn</span></span>
<span data-ttu-id="c4909-140">Ez a témakör bemutatja, hogyan tooexecute különböző műveleteket csatornákon és programokon a Media Services .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="c4909-140">This topic shows you how tooexecute different operations on channels and programs using Media Services .NET SDK.</span></span> <span data-ttu-id="c4909-141">A műveletek között számos hosszú futású művelet található, így hosszú futású műveleteket felügyelő .NET API-kat használunk.</span><span class="sxs-lookup"><span data-stu-id="c4909-141">Because many operations are long-running .NET APIs that manage long running operations are used.</span></span>

<span data-ttu-id="c4909-142">a témakör azt mutatja be hello hogyan toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="c4909-142">hello topic shows how toodo hello following:</span></span>

1. <span data-ttu-id="c4909-143">Csatorna létrehozása és elindítása.</span><span class="sxs-lookup"><span data-stu-id="c4909-143">Create and start a channel.</span></span> <span data-ttu-id="c4909-144">Hosszú futású API-k vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="c4909-144">Long-running APIs are used.</span></span>
2. <span data-ttu-id="c4909-145">Első hello csatorna feldolgozó (bemeneti) végpontjának.</span><span class="sxs-lookup"><span data-stu-id="c4909-145">Get hello channels ingest (input) endpoint.</span></span> <span data-ttu-id="c4909-146">Ezt a végpontot kell megadni a toohello egyféle sávszélességű élő adatfolyamot küldő kódolónak.</span><span class="sxs-lookup"><span data-stu-id="c4909-146">This endpoint should be provided toohello encoder that can send a single bitrate live stream.</span></span>
3. <span data-ttu-id="c4909-147">Hello előnézeti végpont lekérése.</span><span class="sxs-lookup"><span data-stu-id="c4909-147">Get hello preview endpoint.</span></span> <span data-ttu-id="c4909-148">Ez a végpont egy használt toopreview az adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="c4909-148">This endpoint is used toopreview your stream.</span></span>
4. <span data-ttu-id="c4909-149">Hozzon létre egy eszköz, amely használt toostore lesz a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="c4909-149">Create an asset that will be used toostore your content.</span></span> <span data-ttu-id="c4909-150">hello adategység továbbítási házirendjeit úgy kell konfigurálni, ahogy az alábbi példa mutatja.</span><span class="sxs-lookup"><span data-stu-id="c4909-150">hello asset delivery policies should be configured as well, as shown in this example.</span></span>
5. <span data-ttu-id="c4909-151">Hozzon létre egy programot, és adja meg a korábban létrehozott toouse hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="c4909-151">Create a program and specify toouse hello asset that was created earlier.</span></span> <span data-ttu-id="c4909-152">Indítsa el a hello programot.</span><span class="sxs-lookup"><span data-stu-id="c4909-152">Start hello program.</span></span> <span data-ttu-id="c4909-153">Hosszú futású API-k vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="c4909-153">Long-running APIs are used.</span></span>
6. <span data-ttu-id="c4909-154">Hello eszköz, egy kereső létrehozása, így hello tartalom közzétételre kerüljön és továbbítható tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="c4909-154">Create a locator for hello asset, so hello content gets published and can be streamed tooyour clients.</span></span>
7. <span data-ttu-id="c4909-155">A befutók megjelenítése és elrejtése.</span><span class="sxs-lookup"><span data-stu-id="c4909-155">Show and hide slates.</span></span> <span data-ttu-id="c4909-156">A hirdetések elindítása és leállítása.</span><span class="sxs-lookup"><span data-stu-id="c4909-156">Start and stop advertisements.</span></span> <span data-ttu-id="c4909-157">Hosszú futású API-k vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="c4909-157">Long-running APIs are used.</span></span>
8. <span data-ttu-id="c4909-158">Számolja fel a csatornát, és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="c4909-158">Clean up your channel and all hello associated resources.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4909-159">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c4909-159">Prerequisites</span></span>
<span data-ttu-id="c4909-160">Az alábbiakban hello szükséges toocomplete hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c4909-160">hello following are required toocomplete hello tutorial.</span></span>

* <span data-ttu-id="c4909-161">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c4909-161">An Azure account.</span></span> <span data-ttu-id="c4909-162">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c4909-162">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c4909-163">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c4909-163">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span> <span data-ttu-id="c4909-164">Amelyek lehetnek kimenő használt tootry fizetős Azure-szolgáltatások jóváírásokat kap.</span><span class="sxs-lookup"><span data-stu-id="c4909-164">You get credits that can be used tootry out paid Azure services.</span></span> <span data-ttu-id="c4909-165">Hello jóváírásokat el is használta, után is megtarthatja hello fiókot, és ingyenes Azure-szolgáltatások és funkciók, például az Azure App Service Web Apps szolgáltatása hello használata.</span><span class="sxs-lookup"><span data-stu-id="c4909-165">Even after hello credits are used up, you can keep hello account and use free Azure services and features, such as hello Web Apps feature in Azure App Service.</span></span>
* <span data-ttu-id="c4909-166">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="c4909-166">A Media Services account.</span></span> <span data-ttu-id="c4909-167">egy Media Services-fiók toocreate lásd [fiók létrehozása](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="c4909-167">toocreate a Media Services account, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="c4909-168">Visual Studio 2010 SP1 (Professional, Premium, Ultimate vagy Express) vagy későbbi verzió.</span><span class="sxs-lookup"><span data-stu-id="c4909-168">Visual Studio 2010 SP1 (Professional, Premium, Ultimate, or Express) or later versions.</span></span>
* <span data-ttu-id="c4909-169">A Media Services .NET SDK legalább 3.2.0.0 vagy újabb verziójával kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="c4909-169">You must use Media Services .NET SDK version 3.2.0.0 or newer.</span></span>
* <span data-ttu-id="c4909-170">Egy webkamera és egy egyféle sávszélességű élő adatfolyamot küldő kódoló.</span><span class="sxs-lookup"><span data-stu-id="c4909-170">A webcam and an encoder that can send a single bitrate live stream.</span></span>

## <a name="considerations"></a><span data-ttu-id="c4909-171">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="c4909-171">Considerations</span></span>
* <span data-ttu-id="c4909-172">Maximális hello élő esemény időtartama ajánlott 8 órára is jelenleg.</span><span class="sxs-lookup"><span data-stu-id="c4909-172">Currently, hello max recommended duration of a live event is 8 hours.</span></span> <span data-ttu-id="c4909-173">Ha hosszabb ideig kell toorun egy csatornát, forduljon amslived@Microsoft.com e-mail címen.</span><span class="sxs-lookup"><span data-stu-id="c4909-173">Please contact amslived at Microsoft.com if you need toorun a Channel for longer periods of time.</span></span>
* <span data-ttu-id="c4909-174">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="c4909-174">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="c4909-175">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="c4909-175">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="c4909-176">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="c4909-176">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="download-sample"></a><span data-ttu-id="c4909-177">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="c4909-177">Download sample</span></span>

<span data-ttu-id="c4909-178">Letöltheti az ebben a témakörben ismertetett hello minta [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).</span><span class="sxs-lookup"><span data-stu-id="c4909-178">You can download hello sample that is described in this topic from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).</span></span>

## <a name="set-up-for-development-with-media-services-sdk-for-net"></a><span data-ttu-id="c4909-179">A .NET-keretrendszerhez készült Media Services SDK-val történő fejlesztés előkészítése</span><span class="sxs-lookup"><span data-stu-id="c4909-179">Set up for development with Media Services SDK for .NET</span></span>

<span data-ttu-id="c4909-180">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c4909-180">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="code-example"></a><span data-ttu-id="c4909-181">Mintakód</span><span class="sxs-lookup"><span data-stu-id="c4909-181">Code example</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
        private const string ChannelName = "channel001";
        private const string AssetlName = "asset001";
        private const string ProgramlName = "program001";

        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            IChannel channel = CreateAndStartChannel();

            // hello channel's input endpoint:
            string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();

            Console.WriteLine("Intest URL: {0}", ingestUrl);


            // Use hello previewEndpoint toopreview and verify 
            // that hello input from hello encoder is actually reaching hello Channel. 
            string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();

            Console.WriteLine("Preview URL: {0}", previewEndpoint);

            // When Live Encoding is enabled, you can now get a preview of hello live feed as it reaches hello Channel. 
            // This can be a valuable tool toocheck whether your live feed is actually reaching hello Channel. 
            // hello thumbnail is exposed via hello same end-point as hello Channel Preview URL.
            string thumbnailUri = new UriBuilder
            {
            Scheme = Uri.UriSchemeHttps,
            Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
            Path = "thumbnails/input.jpg"
            }.Uri.ToString();

            Console.WriteLine("Thumbain URL: {0}", thumbnailUri);

            // Once you previewed your stream and verified that it is flowing into your Channel, 
            // you can create an event by creating an Asset, Program, and Streaming Locator. 
            IAsset asset = CreateAndConfigureAsset();

            IProgram program = CreateAndStartProgram(channel, asset);

            ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);

            // You can use slates and ads only if hello channel type is Standard.  
            StartStopAdsSlates(channel);

            // Once you are done streaming, clean up your resources.
            Cleanup(channel);
        }

        public static IChannel CreateAndStartChannel()
        {
            var channelInput = CreateChannelInput();
            var channePreview = CreateChannelPreview();
            var channelEncoding = CreateChannelEncoding();

            ChannelCreationOptions options = new ChannelCreationOptions
            {
            EncodingType = ChannelEncodingType.Standard,
            Name = ChannelName,
            Input = channelInput,
            Preview = channePreview,
            Encoding = channelEncoding
            };

            Log("Creating channel");
            IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
            string channelId = TrackOperation(channelCreateOperation, "Channel create");

            IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();

            Log("Starting channel");
            var channelStartOperation = channel.SendStartOperation();
            TrackOperation(channelStartOperation, "Channel start");

            return channel;
        }

        /// <summary>
        /// Create channel input, used in channel creation options. 
        /// </summary>
        /// <returns></returns>
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
            StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                {
                    new IPRange
                    {
                    Name = "TestChannelInput001",
                    Address = IPAddress.Parse("0.0.0.0"),
                    SubnetPrefixLength = 0
                    }
                }
            }
            };
        }

        /// <summary>
        /// Create channel preview, used in channel creation options. 
        /// </summary>
        /// <returns></returns>
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                {
                    new IPRange
                    {
                    Name = "TestChannelPreview001",
                    Address = IPAddress.Parse("0.0.0.0"),
                    SubnetPrefixLength = 0
                    }
                }
            }
            };
        }

        /// <summary>
        /// Create channel encoding, used in channel creation options. 
        /// </summary>
        /// <returns></returns>
        private static ChannelEncoding CreateChannelEncoding()
        {
            return new ChannelEncoding
            {
            SystemPreset = "Default720p",
            IgnoreCea708ClosedCaptions = false,
            AdMarkerSource = AdMarkerSource.Api,
            // You can only set audio if streaming protocol is set tooStreamingProtocol.RTPMPEG2TS.
            AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
            };
        }

        /// <summary>
        /// Create an asset and configure asset delivery policies.
        /// </summary>
        /// <returns></returns>
        public static IAsset CreateAndConfigureAsset()
        {
            IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);

            IAssetDeliveryPolicy policy =
            _context.AssetDeliveryPolicies.Create("Clear Policy",
            AssetDeliveryPolicyType.NoDynamicEncryption,
            AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

            asset.DeliveryPolicies.Add(policy);

            return asset;
        }

        /// <summary>
        /// Create a Program on hello Channel. You can have multiple Programs that overlap or are sequential;
        /// however each Program must have a unique name within your Media Services account.
        /// </summary>
        /// <param name="channel"></param>
        /// <param name="asset"></param>
        /// <returns></returns>
        public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
        {
            IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
            Log("Program created", program.Id);

            Log("Starting program");
            var programStartOperation = program.SendStartOperation();
            TrackOperation(programStartOperation, "Program start");

            return program;
        }

        /// <summary>
        /// Create locators in order toobe able toopublish and stream hello video.
        /// </summary>
        /// <param name="asset"></param>
        /// <param name="ArchiveWindowLength"></param>
        /// <returns></returns>
        public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
        {
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
            var locator = _context.Locators.CreateLocator
            (
                LocatorType.OnDemandOrigin,
                asset,
                _context.AccessPolicies.Create
                (
                    "Live Stream Policy",
                    ArchiveWindowLength,
                    AccessPermissions.Read
                )
            );

            return locator;
        }

        /// <summary>
        /// Perform operations on slates.
        /// </summary>
        /// <param name="channel"></param>
        public static void StartStopAdsSlates(IChannel channel)
        {
            int cueId = new Random().Next(int.MaxValue);
            var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));

            Log("Creating asset");
            var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
            Log("Slate asset created", slateAsset.Id);

            Log("Uploading file");
            var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
            assetFile.Upload(path);
            assetFile.IsPrimary = true;
            assetFile.Update();

            Log("Showing slate");
            var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
            TrackOperation(showSlateOpeartion, "Show slate");

            Log("Hiding slate");
            var hideSlateOperation = channel.SendHideSlateOperation();
            TrackOperation(hideSlateOperation, "Hide slate");

            Log("Starting ad");
            var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
            TrackOperation(startAdOperation, "Start ad");

            Log("Ending ad");
            var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
            TrackOperation(endAdOperation, "End ad");

            Log("Deleting slate asset");
            slateAsset.Delete();
        }

        /// <summary>
        /// Clean up resources associated with hello channel.
        /// </summary>
        /// <param name="channel"></param>
        public static void Cleanup(IChannel channel)
        {
            IAsset asset;
            if (channel != null)
            {
            foreach (var program in channel.Programs)
            {
                asset = _context.Assets.Where(se => se.Id == program.AssetId)
                            .FirstOrDefault();

                Log("Stopping program");
                var programStopOperation = program.SendStopOperation();
                TrackOperation(programStopOperation, "Program stop");

                program.Delete();

                if (asset != null)
                {
                Log("Deleting locators");
                foreach (var l in asset.Locators)
                    l.Delete();

                Log("Deleting asset");
                asset.Delete();
                }
            }

            Log("Stopping channel");
            var channelStopOperation = channel.SendStopOperation();
            TrackOperation(channelStopOperation, "Channel stop");

            Log("Deleting channel");
            var channelDeleteOperation = channel.SendDeleteOperation();
            TrackOperation(channelDeleteOperation, "Channel delete");
            }
        }

        /// <summary>
        /// Track long running operations.
        /// </summary>
        /// <param name="operation"></param>
        /// <param name="description"></param>
        /// <returns></returns>
        public static string TrackOperation(IOperation operation, string description)
        {
            string entityId = null;
            bool isCompleted = false;

            Log("starting tootrack ", null, operation.Id);
            while (isCompleted == false)
            {
            operation = _context.Operations.GetOperation(operation.Id);
            isCompleted = IsCompleted(operation, out entityId);
            System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
            }
            // If we got here, hello operation succeeded.
            Log(description + " in completed", operation.TargetEntityId, operation.Id);

            return entityId;
        }

        /// <summary> 
        /// Checks if hello operation has been completed. 
        /// If hello operation succeeded, hello created entity Id is returned in hello out parameter.
        /// </summary> 
        /// <param name="operationId">hello operation Id.</param> 
        /// <param name="channel">
        /// If hello operation succeeded, 
        /// hello entity Id associated with hello sucessful operation is returned in hello out parameter.</param>
        /// <returns>Returns false if hello operation is still in progress; otherwise, true.</returns> 
        private static bool IsCompleted(IOperation operation, out string entityId)
        {
            bool completed = false;

            entityId = null;

            switch (operation.State)
            {
            case OperationState.Failed:
                // Handle hello failure. 
                // For example, throw an exception. 
                // Use hello following information in hello exception: operationId, operation.ErrorMessage.
                Log("operation failed", operation.TargetEntityId, operation.Id);
                break;
            case OperationState.Succeeded:
                completed = true;
                entityId = operation.TargetEntityId;
                break;
            case OperationState.InProgress:
                completed = false;
                Log("operation in progress", operation.TargetEntityId, operation.Id);
                break;
            }
            return completed;
        }

        private static void Log(string action, string entityId = null, string operationId = null)
        {
            Console.WriteLine(
            "{0,-21}{1,-51}{2,-51}{3,-51}",
            DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
            action,
            entityId ?? string.Empty,
            operationId ?? string.Empty);
        }
        }
    }

## <a name="next-step"></a><span data-ttu-id="c4909-182">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="c4909-182">Next step</span></span>
<span data-ttu-id="c4909-183">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="c4909-183">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c4909-184">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c4909-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


