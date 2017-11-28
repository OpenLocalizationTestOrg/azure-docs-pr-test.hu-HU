---
title: "Microsoft® Smooth Streaming ügyfél eljárás Kit aaaLicensing"
description: "További információk a hogyan toolicensing hello Microsoft® Smooth Streaming ügyfél eljárás készlet."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 56c3dccda73dd02207bb4dbe8109ba6fda917a6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a><span data-ttu-id="c3be5-103">Licencelési Microsoft® zökkenőmentes adatfolyam ügyfél Kit eljárás</span><span class="sxs-lookup"><span data-stu-id="c3be5-103">Licensing Microsoft® Smooth Streaming Client Porting Kit</span></span>
## <a name="overview"></a><span data-ttu-id="c3be5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c3be5-104">Overview</span></span>
<span data-ttu-id="c3be5-105">Microsoft Smooth Streaming ügyfél eljárás Kit (**SSPK** röviden) a Smooth Streaming ügyfél megvalósítása beágyazott optimalizált toohelp eszközgyártók, a kábel és a mobil operátorok, a tartalom szolgáltatók, a kézibeszélő gyártók, független szoftvergyártók (ISV-k), és megoldást szolgáltatók toocreate termékek és szolgáltatások folyamatos adaptív adatfolyam-tartalmak Smooth Streaming formátumban.</span><span class="sxs-lookup"><span data-stu-id="c3be5-105">Microsoft Smooth Streaming Client Porting Kit (**SSPK** for short) is a Smooth Streaming client implementation that is optimized toohelp embedded device manufacturers, cable and mobile operators, content service providers, handset manufacturers, independent software vendors (ISVs), and solution providers toocreate products and services for streaming adaptive streaming content in Smooth Streaming format.</span></span> <span data-ttu-id="c3be5-106">SSPK egy Smooth Streaming, hello licencbe tooany és a platformok által is tartalomfájlokat ügyfél és a platformok független megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="c3be5-106">SSPK is a device and platform independent implementation of Smooth Streaming client that can be ported by hello licensee tooany device and platform.</span></span> 

<span data-ttu-id="c3be5-107">Alábbi egy magas szintű architektúra és IIS Smooth Streaming eljárás Kit hello Smooth Streaming ügyfél megvalósítása a Microsoft által biztosított és Smooth Streaming tartalom lejátszását az összes hello alapvető logikát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c3be5-107">Included below is a high level architecture and IIS Smooth Streaming Porting Kit box is hello Smooth Streaming Client implementation provided by Microsoft and includes all hello core logic for playback of Smooth Streaming content.</span></span> <span data-ttu-id="c3be5-108">Ez az majd legelterjedtebb partnerek egy adott eszköz vagy a platform által megfelelő felületek alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="c3be5-108">This is then ported by partners for a specific device or platform by implementing appropriate interfaces.</span></span> 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a><span data-ttu-id="c3be5-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="c3be5-110">Description</span></span>
<span data-ttu-id="c3be5-111">SSPK licencelt által biztosított érték kiváló üzleti igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="c3be5-111">SSPK is licensed on terms that offer excellent business value.</span></span> <span data-ttu-id="c3be5-112">SSPK licenc hello iparág biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c3be5-112">SSPK license provides hello industry with:</span></span>

* <span data-ttu-id="c3be5-113">A c++ Smooth Streaming eljárás Kit forrás</span><span class="sxs-lookup"><span data-stu-id="c3be5-113">Smooth Streaming Porting Kit source in C++</span></span> 
  * <span data-ttu-id="c3be5-114">megvalósítja a Smooth Streaming ügyfélfunkciókat</span><span class="sxs-lookup"><span data-stu-id="c3be5-114">implements Smooth Streaming Client functionality</span></span>
  * <span data-ttu-id="c3be5-115">hozzáadja a formátum elemzése, heurisztikus pufferelés logika stb.</span><span class="sxs-lookup"><span data-stu-id="c3be5-115">adds format parsing, heuristics, buffering logic, etc.</span></span>
* <span data-ttu-id="c3be5-116">Lejátszóalkalmazás API-k</span><span class="sxs-lookup"><span data-stu-id="c3be5-116">Player application APIs</span></span> 
  * <span data-ttu-id="c3be5-117">az alkalmazásprogramozási felületek és egy media player alkalmazás közötti interakció</span><span class="sxs-lookup"><span data-stu-id="c3be5-117">programming interfaces for interaction with a media player application</span></span>
* <span data-ttu-id="c3be5-118">Platform absztrakciós réteg (PAL) kapcsolat</span><span class="sxs-lookup"><span data-stu-id="c3be5-118">Platform Abstraction Layer (PAL) Interface</span></span> 
  * <span data-ttu-id="c3be5-119">az alkalmazásprogramozási felületek (szálak, sockets) hello operációs rendszerrel folytatott kommunikációjuktól</span><span class="sxs-lookup"><span data-stu-id="c3be5-119">programming interfaces for interaction with hello operating system (threads, sockets)</span></span>
* <span data-ttu-id="c3be5-120">Hardver absztrakciós réteg (HAL) kapcsolat</span><span class="sxs-lookup"><span data-stu-id="c3be5-120">Hardware Abstraction Layer (HAL) Interface</span></span> 
  * <span data-ttu-id="c3be5-121">programozási felületek kölcsönhatások a hardver A / V dekóderek (dekódolás, megjelenítése)</span><span class="sxs-lookup"><span data-stu-id="c3be5-121">programming interfaces for interaction with hardware A/V decoders (decoding, rendering)</span></span>
* <span data-ttu-id="c3be5-122">Digitális tartalomvédelmi (DRM) felügyeleti felület</span><span class="sxs-lookup"><span data-stu-id="c3be5-122">Digital Rights Management (DRM) Interface</span></span> 
  * <span data-ttu-id="c3be5-123">alkalmazásprogramozási felületek hello DRM-absztrakciós réteg (DAL) keresztül DRM kezelése</span><span class="sxs-lookup"><span data-stu-id="c3be5-123">programming interfaces for handling DRM through hello DRM Abstraction Layer (DAL)</span></span>
  * <span data-ttu-id="c3be5-124">Microsoft PlayReady eljárás Kit külön érhető el, de integrálja a felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="c3be5-124">Microsoft PlayReady Porting Kit ships separately but integrates through this interface.</span></span> <span data-ttu-id="c3be5-125">Microsoft PlayReady Device engedélyezéséről további információért kattintson [Itt](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span><span class="sxs-lookup"><span data-stu-id="c3be5-125">For more details on Microsoft PlayReady Device licensing, click [here](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span></span>
* <span data-ttu-id="c3be5-126">Megvalósítása – minták</span><span class="sxs-lookup"><span data-stu-id="c3be5-126">Implementation samples</span></span> 
  * <span data-ttu-id="c3be5-127">a minta PAL implementációja Linux</span><span class="sxs-lookup"><span data-stu-id="c3be5-127">sample PAL implementation for Linux</span></span>
  * <span data-ttu-id="c3be5-128">a minta HAL implementációja GStreamer</span><span class="sxs-lookup"><span data-stu-id="c3be5-128">sample HAL implementation for GStreamer</span></span>

## <a name="licensing-options"></a><span data-ttu-id="c3be5-129">Licencelési lehetőségek</span><span class="sxs-lookup"><span data-stu-id="c3be5-129">Licensing Options</span></span>
<span data-ttu-id="c3be5-130">Microsoft Smooth Streaming ügyfél eljárás készlet két különböző licencszerződések alapján történik elérhető toolicensees: egy a Smooth Streaming ügyfél ideiglenes termékek és egy másikat a Smooth Streaming ügyfél végső termékek tooend felhasználók terjesztése.</span><span class="sxs-lookup"><span data-stu-id="c3be5-130">Microsoft Smooth Streaming Client Porting Kit is made available toolicensees under two distinct license agreements: one for developing Smooth Streaming Client Interim Products and another for distributing Smooth Streaming Client Final Products tooend users.</span></span>

* <span data-ttu-id="c3be5-131">Lapkakészlet gyártók, rendszerintegrátorok vagy független keresve igénylő forrás kód eljárás kit toodevelop ideiglenes termékek, a Microsoft Smooth Streaming ügyfél eljárás Kit **ideiglenes terméklicenc** kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="c3be5-131">For chipset manufacturers, system integrators, or independent software vendors (ISVs) who require a source code porting kit toodevelop Interim Products, a Microsoft Smooth Streaming Client Porting Kit **Interim Product License** should be executed.</span></span>
* <span data-ttu-id="c3be5-132">Az eszközgyártók vagy Smooth Streaming ügyfél végső termékek tooend felhasználók, terjesztési jogokat igénylő ISV-k hello Microsoft Smooth Streaming ügyfél eljárás Kit **végső terméklicenc** végre kell hajtani.</span><span class="sxs-lookup"><span data-stu-id="c3be5-132">For device manufacturers or ISVs who require distribution rights for Smooth Streaming Client Final Products tooend users, hello Microsoft Smooth Streaming Client Porting Kit **Final Product License** should be executed.</span></span>

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a><span data-ttu-id="c3be5-133">Microsoft Smooth Streaming Kit ideiglenes terméklicenc eljárás ügyfél</span><span class="sxs-lookup"><span data-stu-id="c3be5-133">Microsoft Smooth Streaming Client Porting Kit Interim Product License</span></span>
<span data-ttu-id="c3be5-134">Ezt a licencet, a Microsoft nyújt egy Smooth Streaming ügyfél eljárás Kit és szükséges szellemi tulajdonra vonatkozó jog toodevelop hello, és terjesztése Smooth Streaming ügyfél ideiglenes termékek tooother Smooth Streaming ügyfél eljárás Kit eszköz licenctulajdonosok esetében, amelyek terjessze a Smooth Streaming ügyfél végső termékek.</span><span class="sxs-lookup"><span data-stu-id="c3be5-134">Under this license, Microsoft offers a Smooth Streaming Client Porting Kit and hello necessary intellectual property rights toodevelop and distribute Smooth Streaming Client Interim Products tooother Smooth Streaming Client Porting Kit device licensees that distribute Smooth Streaming Client Final Products.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="c3be5-135">Díjszabása</span><span class="sxs-lookup"><span data-stu-id="c3be5-135">Fee structure</span></span>
<span data-ttu-id="c3be5-136">USA $ 50 000 egyszeri licenc anélkül biztosít hozzáférést toohello Smooth Streaming ügyfél eljárás Kit.</span><span class="sxs-lookup"><span data-stu-id="c3be5-136">A U.S. $50,000 one-time license fee provides access toohello Smooth Streaming Client Porting Kit.</span></span> 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a><span data-ttu-id="c3be5-137">Microsoft Smooth Streaming Kit végleges terméklicenc eljárás ügyfél</span><span class="sxs-lookup"><span data-stu-id="c3be5-137">Microsoft Smooth Streaming Client Porting Kit Final Product License</span></span>
<span data-ttu-id="c3be5-138">Ezt a licencet, a Microsoft minden szükséges szellemi tulajdonra vonatkozó jog tooreceive Smooth Streaming ügyfél ideiglenes termékek kínál más Smooth Streaming ügyfél eljárás Kit licenctulajdonosok esetében és a vállalat védjegyét Smooth Streaming ügyfél végső toodistribute Termékek tooend felhasználók.</span><span class="sxs-lookup"><span data-stu-id="c3be5-138">Under this license, Microsoft offers all necessary intellectual property rights tooreceive Smooth Streaming Client Interim Products from other Smooth Streaming Client Porting Kit licensees and toodistribute company-branded Smooth Streaming Client Final Products tooend users.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="c3be5-139">Díjszabása</span><span class="sxs-lookup"><span data-stu-id="c3be5-139">Fee structure</span></span>
<span data-ttu-id="c3be5-140">hello Smooth Streaming ügyfél végső termék alatt, a kiemelt modellben kínálják:</span><span class="sxs-lookup"><span data-stu-id="c3be5-140">hello Smooth Streaming Client Final Product is offered under a royalty model as under:</span></span>

* <span data-ttu-id="c3be5-141">$0.10 / rendszerrel szállított eszköz végrehajtása</span><span class="sxs-lookup"><span data-stu-id="c3be5-141">$0.10 per device implementation shipped</span></span>
* <span data-ttu-id="c3be5-142">hello kiemelt tárfiókonként maximum 50 000 $ minden évben</span><span class="sxs-lookup"><span data-stu-id="c3be5-142">hello royalty is capped at $50,000 each year</span></span>
* <span data-ttu-id="c3be5-143">Nincsenek kiemelt első 10 000 eszköz-megvalósítások esetében minden évben</span><span class="sxs-lookup"><span data-stu-id="c3be5-143">No royalty for first 10,000 device implementations each year</span></span> 

## <a name="licensing-procedure-and-sspk-access"></a><span data-ttu-id="c3be5-144">Licencelési eljárás és SSPK hozzáférés</span><span class="sxs-lookup"><span data-stu-id="c3be5-144">Licensing Procedure and SSPK access</span></span>
<span data-ttu-id="c3be5-145">Írjon e-mailt [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) összes licencelési lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="c3be5-145">Please email [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) for all licensing queries.</span></span>

<span data-ttu-id="c3be5-146">Hello [SSPK terjesztési portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) van elérhető tooregistered ideiglenes licenctulajdonosok esetében.</span><span class="sxs-lookup"><span data-stu-id="c3be5-146">hello [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is accessible tooregistered Interim licensees.</span></span>

<span data-ttu-id="c3be5-147">Ideiglenes és végső SSPK licenctulajdonosok esetében túl is elküldhetik technikai kérdésekkel kapcsolatos[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c3be5-147">Interim and Final SSPK licensees can submit technical questions too[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span></span>

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a><span data-ttu-id="c3be5-148">Microsoft Smooth Streaming ügyfél ideiglenes termék megállapodás licenctulajdonosok esetében</span><span class="sxs-lookup"><span data-stu-id="c3be5-148">Microsoft Smooth Streaming Client Interim Product Agreement Licensees</span></span>
* <span data-ttu-id="c3be5-149">Adroit üzleti megoldások, Inc</span><span class="sxs-lookup"><span data-stu-id="c3be5-149">Adroit Business Solutions, Inc</span></span>
* <span data-ttu-id="c3be5-150">Speciális digitális szórás rendszergazdai (SA)</span><span class="sxs-lookup"><span data-stu-id="c3be5-150">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="c3be5-151">AirTies Kablosuz Iletism Sanayive hagyása Ticaret A.S.</span><span class="sxs-lookup"><span data-stu-id="c3be5-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="c3be5-152">Albis technológiák Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-152">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="c3be5-153">Alticast Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-153">Alticast Corporation</span></span>
* <span data-ttu-id="c3be5-154">Amazon digitális szolgáltatások, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-154">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="c3be5-155">Arion technológia, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-155">Arion Technology, Inc.</span></span>
* <span data-ttu-id="c3be5-156">AVC multimédia szoftver Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-156">AVC Multimedia Software Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-157">Cavium, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-157">Cavium, Inc.</span></span>
* <span data-ttu-id="c3be5-158">EchoStar megvásárlásáról Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-158">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="c3be5-159">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-159">Enseo, Inc.</span></span>
* <span data-ttu-id="c3be5-160">Dél Fluendo</span><span class="sxs-lookup"><span data-stu-id="c3be5-160">Fluendo S.A.</span></span>
* <span data-ttu-id="c3be5-161">HANDAN BroadInfoCom Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-161">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-162">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="c3be5-162">Infomir GMBH</span></span>
* <span data-ttu-id="c3be5-163">Irdeto USA Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-163">Irdeto USA Inc.</span></span>
* <span data-ttu-id="c3be5-164">Dél iWEDIA</span><span class="sxs-lookup"><span data-stu-id="c3be5-164">iWEDIA S.A.</span></span> 
* <span data-ttu-id="c3be5-165">Liberty globális szolgáltatások BV</span><span class="sxs-lookup"><span data-stu-id="c3be5-165">Liberty Global Services BV</span></span>
* <span data-ttu-id="c3be5-166">MediaTek Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-166">MediaTek Inc.</span></span>
* <span data-ttu-id="c3be5-167">MStar Co, Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-167">MStar Co, Ltd</span></span>
* <span data-ttu-id="c3be5-168">Nintendo Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-168">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-169">OpenTV, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-169">OpenTV, Inc.</span></span>
* <span data-ttu-id="c3be5-170">Sáfrány digitális korlátozott</span><span class="sxs-lookup"><span data-stu-id="c3be5-170">Saffron Digital Limited</span></span>
* <span data-ttu-id="c3be5-171">Szecsuan Changhong elektromos Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-171">Sichuan Changhong Electric Co., Ltd</span></span>
* <span data-ttu-id="c3be5-172">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="c3be5-172">SoftAtHome</span></span>
* <span data-ttu-id="c3be5-173">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-173">Sony Corporation</span></span>
* <span data-ttu-id="c3be5-174">Tatung technológia Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-174">Tatung Technology Inc.</span></span>
* <span data-ttu-id="c3be5-175">TCL Technoly Electronics (Huizhou) Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-176">Felső győzelem beruházások értékét, Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-176">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="c3be5-177">Ticaret A.S. Vestel Elektronik Sanayi megtörtént</span><span class="sxs-lookup"><span data-stu-id="c3be5-177">Vestel Elektronik Sanayi ve Ticaret A.S.</span></span>
* <span data-ttu-id="c3be5-178">VisualOn, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-178">VisualOn, Inc.</span></span>
* <span data-ttu-id="c3be5-179">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-179">ZTE Corporation</span></span>

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a><span data-ttu-id="c3be5-180">Microsoft Smooth Streaming ügyfél végső termék megállapodás licenctulajdonosok esetében</span><span class="sxs-lookup"><span data-stu-id="c3be5-180">Microsoft Smooth Streaming Client Final Product Agreement Licensees</span></span>
* <span data-ttu-id="c3be5-181">Speciális digitális szórás rendszergazdai (SA)</span><span class="sxs-lookup"><span data-stu-id="c3be5-181">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="c3be5-182">AirTies Kablosuz Iletism Sanayive hagyása Ticaret A.S.</span><span class="sxs-lookup"><span data-stu-id="c3be5-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="c3be5-183">Albis technológiák Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-183">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="c3be5-184">Amazon digitális szolgáltatások, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-184">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="c3be5-185">AmTRAN technológia Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-185">AmTRAN Technology Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-186">Arcadyan technológia Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-186">Arcadyan Technology Corporation</span></span>
* <span data-ttu-id="c3be5-187">Arion technológia, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-187">Arion Technology, Inc.</span></span>
* <span data-ttu-id="c3be5-188">ATMACA ELEKTRONİK SAN.</span><span class="sxs-lookup"><span data-stu-id="c3be5-188">ATMACA ELEKTRONİK SAN.</span></span> <span data-ttu-id="c3be5-189">TİC MEGTÖRTÉNT.</span><span class="sxs-lookup"><span data-stu-id="c3be5-189">VE TİC.</span></span> <span data-ttu-id="c3be5-190">A.Ş</span><span class="sxs-lookup"><span data-stu-id="c3be5-190">A.Ş</span></span>
* <span data-ttu-id="c3be5-191">Brit égbolt szórásos korlátozott</span><span class="sxs-lookup"><span data-stu-id="c3be5-191">British Sky Broadcasting Limited</span></span>
* <span data-ttu-id="c3be5-192">CastPal technológia Inc. Shenzhen</span><span class="sxs-lookup"><span data-stu-id="c3be5-192">CastPal Technology Inc., Shenzhen</span></span>
* <span data-ttu-id="c3be5-193">Compal Electronics, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-193">Compal Electronics, Inc.</span></span>
* <span data-ttu-id="c3be5-194">Dongguan digitális AV technológia Corp., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-194">Dongguan Digital AV Technology Corp., Ltd.</span></span>
* <span data-ttu-id="c3be5-195">EchoStar megvásárlásáról Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-195">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="c3be5-196">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-196">Enseo, Inc.</span></span>
* <span data-ttu-id="c3be5-197">Filmflex filmek korlátozott</span><span class="sxs-lookup"><span data-stu-id="c3be5-197">Filmflex Movies Limited</span></span>
* <span data-ttu-id="c3be5-198">Dél Fluendo</span><span class="sxs-lookup"><span data-stu-id="c3be5-198">Fluendo S.A.</span></span>
* <span data-ttu-id="c3be5-199">Gibson Innovációinak korlátozott</span><span class="sxs-lookup"><span data-stu-id="c3be5-199">Gibson Innovations Limited</span></span>
* <span data-ttu-id="c3be5-200">Haier információk Applicantion S.R.L</span><span class="sxs-lookup"><span data-stu-id="c3be5-200">Haier Information Applicantion S.R.L</span></span>
* <span data-ttu-id="c3be5-201">HANDAN BroadInfoCom Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-201">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-202">Nemzetközi Hisense, Reinhard</span><span class="sxs-lookup"><span data-stu-id="c3be5-202">Hisense International Co., Ltd.</span></span> 
* <span data-ttu-id="c3be5-203">Homecast Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-203">Homecast Co.,Ltd</span></span>
* <span data-ttu-id="c3be5-204">HON Hai pontosság iparági Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-204">Hon Hai Precision Industry Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-205">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="c3be5-205">Infomir GMBH</span></span>
* <span data-ttu-id="c3be5-206">Kaonmedia Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-206">Kaonmedia Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-207">KDDI Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-207">KDDI Corporation</span></span>
* <span data-ttu-id="c3be5-208">Nintendo Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-208">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-209">Narancssárga rendszergazdai (SA)</span><span class="sxs-lookup"><span data-stu-id="c3be5-209">Orange SA</span></span>
* <span data-ttu-id="c3be5-210">Sáfrány digitális korlátozott</span><span class="sxs-lookup"><span data-stu-id="c3be5-210">Saffron Digital Limited</span></span>
* <span data-ttu-id="c3be5-211">Sagemcom szélessávú SAS</span><span class="sxs-lookup"><span data-stu-id="c3be5-211">Sagemcom Broadband SAS</span></span>
* <span data-ttu-id="c3be5-212">Shenzhen Coship Electronics CO., LTD</span><span class="sxs-lookup"><span data-stu-id="c3be5-212">Shenzhen Coship Electronics CO., LTD</span></span>
* <span data-ttu-id="c3be5-213">Shenzhen Jiuzhou elektromos Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-213">Shenzhen Jiuzhou Electric Co.,Ltd</span></span>
* <span data-ttu-id="c3be5-214">Shenzhen Skyworth digitális technológia Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-214">Shenzhen Skyworth Digital Technology Co., Ltd</span></span>
* <span data-ttu-id="c3be5-215">Szecsuan Changhong elektromos Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-215">Sichuan Changhong Electric Co., Ltd.</span></span>
* <span data-ttu-id="c3be5-216">Skardin ipari vállalat esetében.</span><span class="sxs-lookup"><span data-stu-id="c3be5-216">Skardin Industrial Corp.</span></span>
* <span data-ttu-id="c3be5-217">Égbolt Deutschland Fernsehen GmbH & Co. KG</span><span class="sxs-lookup"><span data-stu-id="c3be5-217">Sky Deutschland Fernsehen GmbH & Co. KG</span></span>
* <span data-ttu-id="c3be5-218">Dél SmarDTV</span><span class="sxs-lookup"><span data-stu-id="c3be5-218">SmarDTV S.A.</span></span>
* <span data-ttu-id="c3be5-219">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="c3be5-219">SoftAtHome</span></span>
* <span data-ttu-id="c3be5-220">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-220">Sony Corporation</span></span>
* <span data-ttu-id="c3be5-221">Korlátozott TCL tengerentúli Marketing (tengeri Makaó kereskedelmi)</span><span class="sxs-lookup"><span data-stu-id="c3be5-221">TCL Overseas Marketing (Macao Commercial Offshore) Limited</span></span>
* <span data-ttu-id="c3be5-222">Technicolor szállítási technológiák, SAS</span><span class="sxs-lookup"><span data-stu-id="c3be5-222">Technicolor Delivery Technologies, SAS</span></span>
* <span data-ttu-id="c3be5-223">Tongfang globális Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-223">Tongfang Global Ltd.</span></span>
* <span data-ttu-id="c3be5-224">Felső győzelem beruházások értékét, Ltd</span><span class="sxs-lookup"><span data-stu-id="c3be5-224">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="c3be5-225">Toshiba Lifestyle termékek és szolgáltatások Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-225">Toshiba Lifestyle Products & Services Corporation</span></span>
* <span data-ttu-id="c3be5-226">Univerzális Media Corporation /Slovakia/ s.r.o.</span><span class="sxs-lookup"><span data-stu-id="c3be5-226">Universal Media Corporation /Slovakia/ s.r.o.</span></span>
* <span data-ttu-id="c3be5-227">VIZIO, Inc.</span><span class="sxs-lookup"><span data-stu-id="c3be5-227">VIZIO, Inc.</span></span>
* <span data-ttu-id="c3be5-228">Wistron Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-228">Wistron Corporation</span></span>
* <span data-ttu-id="c3be5-229">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="c3be5-229">ZTE Corporation</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="c3be5-230">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="c3be5-230">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c3be5-231">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c3be5-231">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

