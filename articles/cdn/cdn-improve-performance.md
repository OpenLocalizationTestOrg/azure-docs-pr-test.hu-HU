---
title: "Azure CDN szolgáltatás használata a fájlok tömörítésével aaaImprove teljesítmény |} Microsoft Docs"
description: "Ismerje meg, hogyan tooimprove fájl átvitele az Azure CDN szolgáltatás használata a fájlok tömörítésével sebesség és növeli a lap betöltése teljesítményét."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9a1698b6c29233c2e2e6fb17cdd8e919ae8bfa1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a><span data-ttu-id="6a1ff-103">A jobb teljesítmény érdekében az Azure CDN fájlok tömörítése</span><span class="sxs-lookup"><span data-stu-id="6a1ff-103">Improve performance by compressing files in Azure CDN</span></span>
<span data-ttu-id="6a1ff-104">Tömörítés egyszerű és hatékony módszer tooimprove fájl átviteli sebesség és növekedése lap betöltési teljesítményét előtte méretének csökkentésével küldi hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-104">Compression is a simple and effective method tooimprove file transfer speed and increase page load performance by reducing file size before it is sent from hello server.</span></span> <span data-ttu-id="6a1ff-105">Csökkenti a sávszélességgel kapcsolatos költségek, és gyorsabban élményt nyújt a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-105">It reduces bandwidth costs and provides a more responsive experience for your users.</span></span>

<span data-ttu-id="6a1ff-106">Két módon tooenable tömörítése:</span><span class="sxs-lookup"><span data-stu-id="6a1ff-106">There are two ways tooenable compression:</span></span>

* <span data-ttu-id="6a1ff-107">Engedélyezheti tömörítés a forrás kiszolgálón, ebben az esetben hello hello CDN átmegy tömörített fájlok és tömörített fájlok tooclients azokat kérő biztosításához.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-107">You can enable compression on your origin server, in which case hello CDN passes through hello compressed files and deliver compressed files tooclients that request them.</span></span>
* <span data-ttu-id="6a1ff-108">Engedélyezheti a tömörítést közvetlenül a CDN peremhálózati kiszolgálóinak, eset hello CDN tömöríti fájlok hello és azokat tooend felhasználók kiszolgáló akkor is, ha azok nem tömöríti az hello eredeti kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-108">You can enable compression directly on CDN edge servers, in which case hello CDN compresses hello files and serve them tooend users, even if they are not compressed by hello origin server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a1ff-109">CDN konfigurációs módosítások lépnek bizonyos idő toopropagate hello a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-109">CDN configuration changes take some time toopropagate through hello network.</span></span>  <span data-ttu-id="6a1ff-110">A <b>Akamai Azure CDN</b> -profilok propagálása általában fejeződik be, egy perc alatt.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-110">For <b>Azure CDN from Akamai</b> profiles, propagation usually completes in under one minute.</span></span>  <span data-ttu-id="6a1ff-111">A <b>Azure CDN Verizon</b> profilok, általában látni fogja a módosítások alkalmazásához 90 percen belül.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-111">For <b>Azure CDN from Verizon</b> profiles, you will usually see your changes apply within 90 minutes.</span></span>  <span data-ttu-id="6a1ff-112">Ha hello első alkalommal tömörítés a CDN-végpont kiépítését, érdemes lehet 1 – 2 óra toobe meg arról, hogy hello tömörítési beállítások propagálása előtt hibaelhárítási toohello POP vár</span><span class="sxs-lookup"><span data-stu-id="6a1ff-112">If this is hello first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours toobe sure hello compression settings have propagated toohello POPs before troubleshooting</span></span>
> 
> 

## <a name="enabling-compression"></a><span data-ttu-id="6a1ff-113">Tömörítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6a1ff-113">Enabling compression</span></span>
> [!NOTE]
> <span data-ttu-id="6a1ff-114">hello Standard és prémium szintű CDN rétegek biztosítanak hello tömörítési ugyanezeket a funkciókat, de hello felhasználói felület eltér.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-114">hello Standard and Premium CDN tiers provide hello same compression functionality, but hello user interface differs.</span></span>  <span data-ttu-id="6a1ff-115">Standard és prémium szintű CDN rétegek hello különbségei kapcsolatos további információkért lásd: [Azure CDN áttekintése](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a1ff-115">For more information about hello differences between Standard and Premium CDN tiers, see [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

### <a name="standard-tier"></a><span data-ttu-id="6a1ff-116">Standard csomag</span><span class="sxs-lookup"><span data-stu-id="6a1ff-116">Standard tier</span></span>
> [!NOTE]
> <span data-ttu-id="6a1ff-117">Ez a szakasz vonatkozik túl**Azure CDN Standard verizon** és **Azure CDN Standard Akamai** profilok.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-117">This section applies too**Azure CDN Standard from Verizon** and **Azure CDN Standard from Akamai** profiles.</span></span>
> 
> 

1. <span data-ttu-id="6a1ff-118">Hello CDN profil lapon kattintson hello CDN-végpont toomanage kívánja.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-118">From hello CDN profile page, click hello CDN endpoint you wish toomanage.</span></span>
   
    ![CDN-profil lap végpontok](./media/cdn-file-compression/cdn-endpoints.png)
   
    <span data-ttu-id="6a1ff-120">hello CDN-végpont lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-120">hello CDN endpoint page opens.</span></span>
2. <span data-ttu-id="6a1ff-121">Kattintson a hello **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-121">Click hello **Configure** button.</span></span>
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-config-btn.png)
   
    <span data-ttu-id="6a1ff-123">hello CDN konfigurálása lapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-123">hello CDN Configuration page opens.</span></span>
3. <span data-ttu-id="6a1ff-124">Kapcsolja be a **tömörítés**.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-124">Turn on **Compression**.</span></span>
   
    ![CDN-tömörítési beállítások](./media/cdn-file-compression/cdn-compress-standard.png)
4. <span data-ttu-id="6a1ff-126">Hello alapértelmezett típusát használhatja, vagy módosítsa a hello lista fájltípusok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-126">Use hello default types, or modify hello list by removing or adding file types.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="6a1ff-127">While lehetséges, nem ajánlott tooapply tömörítési toocompressed formátumban, például ZIP, MP3, MP4, JPG stb.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-127">While possible, it is not recommended tooapply compression toocompressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span>
   > 
   > 
5. <span data-ttu-id="6a1ff-128">A módosítások elvégzése után kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-128">After making your changes, click hello **Save** button.</span></span>

### <a name="premium-tier"></a><span data-ttu-id="6a1ff-129">Premium szintű csomag</span><span class="sxs-lookup"><span data-stu-id="6a1ff-129">Premium tier</span></span>
> [!NOTE]
> <span data-ttu-id="6a1ff-130">Ez a szakasz vonatkozik túl**verizon Azure CDN Premium** profilok.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-130">This section applies too**Azure CDN Premium from Verizon** profiles.</span></span>
> 
> 

1. <span data-ttu-id="6a1ff-131">Hello CDN-profil lapján, kattintson a hello **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-131">From hello CDN profile page, click hello **Manage** button.</span></span>
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-manage-btn.png)
   
    <span data-ttu-id="6a1ff-133">Megnyílik a hello CDN felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-133">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="6a1ff-134">Hello az egérmutatót **HTTP nagy** fülre, majd az egérmutatót hello **gyorsítótár beállításainak** menü.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-134">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="6a1ff-135">Kattintson a **tömörítés**.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-135">Click on **Compression**.</span></span>

    ![Fájl tömörítése kiválasztása](./media/cdn-file-compression/cdn-compress-select.png)
   
    <span data-ttu-id="6a1ff-137">Tömörítési beállítások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-137">Compression options are displayed.</span></span>
   
    ![Fájl tömörítése](./media/cdn-file-compression/cdn-compress-files.png)
3. <span data-ttu-id="6a1ff-139">Tömörítés engedélyezése hello kattintva **tömörítés engedélyezve** választógombot.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-139">Enable compression by clicking hello **Compression Enabled** radio button.</span></span>  <span data-ttu-id="6a1ff-140">Adja meg a hello MIME-típusok vesszővel tagolt lista formájában (szóközök nélkül) a hello toocompress kívánja **fájltípusok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-140">Enter hello MIME types you wish toocompress as a comma-delimited list (no spaces) in hello **File Types** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="6a1ff-141">While lehetséges, nem ajánlott tooapply tömörítési toocompressed formátumban, például ZIP, MP3, MP4, JPG stb.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-141">While possible, it is not recommended tooapply compression toocompressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span> 
   > 
   > 
4. <span data-ttu-id="6a1ff-142">A módosítások elvégzése után kattintson a hello **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-142">After making your changes, click hello **Update** button.</span></span>

## <a name="compression-rules"></a><span data-ttu-id="6a1ff-143">Tömörítés szabályok</span><span class="sxs-lookup"><span data-stu-id="6a1ff-143">Compression rules</span></span>
<span data-ttu-id="6a1ff-144">Ezek a táblázatok ismertetik Azure CDN tömörítési viselkedés minden forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-144">These tables describe Azure CDN compression behavior for every scenario.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a1ff-145">A **Azure CDN Verizon** (Standard és prémium), csak a megfelelő fájlok tömörítése.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-145">For **Azure CDN from Verizon** (Standard and Premium), only eligible files are compressed.</span></span>  <span data-ttu-id="6a1ff-146">jogosult a tömörítési toobe, egy fájlt kell:</span><span class="sxs-lookup"><span data-stu-id="6a1ff-146">toobe eligible for compression, a file must:</span></span>
> 
> * <span data-ttu-id="6a1ff-147">Lehet nagyobb, mint 128 bájt.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-147">Be larger than 128 bytes.</span></span>
> * <span data-ttu-id="6a1ff-148">Lehet kisebb, mint 1 MB.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-148">Be smaller than 1 MB.</span></span>
> 
> <span data-ttu-id="6a1ff-149">A **Akamai Azure CDN**, tömörítési fájlok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-149">For **Azure CDN from Akamai**, all files are eligible for compression.</span></span>
> 
> <span data-ttu-id="6a1ff-150">Az összes Azure CDN-termékek, a kell lennie a MIME-típust, amelyet [tömörítési konfigurált](#enabling-compression).</span><span class="sxs-lookup"><span data-stu-id="6a1ff-150">For all Azure CDN products, a file must be a MIME type that has been [configured for compression](#enabling-compression).</span></span>
> 
> <span data-ttu-id="6a1ff-151">**Verizon Azure CDN** (Standard és prémium) támogatási profilok **gzip** (GNU zip), **deflate**, **bzip2**, vagy **br**(Brotli) kódolást.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-151">**Azure CDN from Verizon** profiles (Standard and Premium) support **gzip** (GNU zip), **deflate**, **bzip2**, or  **br** (Brotli) encoding.</span></span> <span data-ttu-id="6a1ff-152">A kódolási Brotli, hello tömörítés csak hello szélén végezhető el.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-152">For Brotli encoding, hello compression is done only at hello edge.</span></span> <span data-ttu-id="6a1ff-153">hello ügyfélböngészőnek Brotli kódolási hello kérelmet kell küldenie és hello tömörített eszköz kell tömörített hello forrás oldalon először.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-153">hello client/browser must send hello request for Brotli encoding and hello compressed asset must have been compressed on hello origin side first.</span></span> 
>
><span data-ttu-id="6a1ff-154">**Akamai Azure CDN** profilok támogatása csak **gzip** kódolást.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-154">**Azure CDN from Akamai** profiles support only **gzip** encoding.</span></span>
> 
> <span data-ttu-id="6a1ff-155">**Akamai Azure CDN** végpontok mindig kérjen **gzip** kódolású hello a forrásból, függetlenül attól, hello ügyfélkérés fájl.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-155">**Azure CDN from Akamai** endpoints always request **gzip** encoded files from hello origin, regardless of hello client request.</span></span> 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a><span data-ttu-id="6a1ff-156">A tömörítés le van tiltva, vagy a fájl nem tömörítés</span><span class="sxs-lookup"><span data-stu-id="6a1ff-156">Compression disabled or file is ineligible for compression</span></span>
| <span data-ttu-id="6a1ff-157">Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek)</span><span class="sxs-lookup"><span data-stu-id="6a1ff-157">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="6a1ff-158">Gyorsítótárazott fájlformátum</span><span class="sxs-lookup"><span data-stu-id="6a1ff-158">Cached file format</span></span> | <span data-ttu-id="6a1ff-159">CDN-válasz toohello ügyfél</span><span class="sxs-lookup"><span data-stu-id="6a1ff-159">CDN response toohello client</span></span> | <span data-ttu-id="6a1ff-160">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6a1ff-160">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6a1ff-161">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-161">Compressed</span></span> |<span data-ttu-id="6a1ff-162">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-162">Compressed</span></span> |<span data-ttu-id="6a1ff-163">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-163">Compressed</span></span> | |
| <span data-ttu-id="6a1ff-164">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-164">Compressed</span></span> |<span data-ttu-id="6a1ff-165">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-165">Uncompressed</span></span> |<span data-ttu-id="6a1ff-166">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-166">Uncompressed</span></span> | |
| <span data-ttu-id="6a1ff-167">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-167">Compressed</span></span> |<span data-ttu-id="6a1ff-168">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="6a1ff-168">Not cached</span></span> |<span data-ttu-id="6a1ff-169">Tömörített és tömörítetlen</span><span class="sxs-lookup"><span data-stu-id="6a1ff-169">Compressed or Uncompressed</span></span> |<span data-ttu-id="6a1ff-170">Forrás válasz függ</span><span class="sxs-lookup"><span data-stu-id="6a1ff-170">Depends on origin response</span></span> |
| <span data-ttu-id="6a1ff-171">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-171">Uncompressed</span></span> |<span data-ttu-id="6a1ff-172">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-172">Compressed</span></span> |<span data-ttu-id="6a1ff-173">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-173">Uncompressed</span></span> | |
| <span data-ttu-id="6a1ff-174">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-174">Uncompressed</span></span> |<span data-ttu-id="6a1ff-175">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-175">Uncompressed</span></span> |<span data-ttu-id="6a1ff-176">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-176">Uncompressed</span></span> | |
| <span data-ttu-id="6a1ff-177">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-177">Uncompressed</span></span> |<span data-ttu-id="6a1ff-178">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="6a1ff-178">Not cached</span></span> |<span data-ttu-id="6a1ff-179">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-179">Uncompressed</span></span> | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a><span data-ttu-id="6a1ff-180">A tömörítés engedélyezve van és a fájl nem jogosult az tömörítés</span><span class="sxs-lookup"><span data-stu-id="6a1ff-180">Compression enabled and file is eligible for compression</span></span>
| <span data-ttu-id="6a1ff-181">Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek)</span><span class="sxs-lookup"><span data-stu-id="6a1ff-181">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="6a1ff-182">Gyorsítótárazott fájlformátum</span><span class="sxs-lookup"><span data-stu-id="6a1ff-182">Cached file format</span></span> | <span data-ttu-id="6a1ff-183">CDN-válasz toohello ügyfél</span><span class="sxs-lookup"><span data-stu-id="6a1ff-183">CDN response toohello client</span></span> | <span data-ttu-id="6a1ff-184">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6a1ff-184">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6a1ff-185">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-185">Compressed</span></span> |<span data-ttu-id="6a1ff-186">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-186">Compressed</span></span> |<span data-ttu-id="6a1ff-187">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-187">Compressed</span></span> |<span data-ttu-id="6a1ff-188">CDN transcodes támogatott formátumok közötti</span><span class="sxs-lookup"><span data-stu-id="6a1ff-188">CDN transcodes between supported formats</span></span> |
| <span data-ttu-id="6a1ff-189">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-189">Compressed</span></span> |<span data-ttu-id="6a1ff-190">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-190">Uncompressed</span></span> |<span data-ttu-id="6a1ff-191">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-191">Compressed</span></span> |<span data-ttu-id="6a1ff-192">CDN tömörítési hajt végre.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-192">CDN performs compression</span></span> |
| <span data-ttu-id="6a1ff-193">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-193">Compressed</span></span> |<span data-ttu-id="6a1ff-194">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="6a1ff-194">Not cached</span></span> |<span data-ttu-id="6a1ff-195">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-195">Compressed</span></span> |<span data-ttu-id="6a1ff-196">CDN tömörítési hajt végre, ha a forrás visszaküldi tömörítetlen.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-196">CDN performs compression if origin returns uncompressed.</span></span>  <span data-ttu-id="6a1ff-197">**Verizon Azure CDN** fázisok hello tömörítetlen fájl hello első kérésre, majd tömöríti és gyorsítótárak hello további kérelmeknél fájlt.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-197">**Azure CDN from Verizon** passes hello uncompressed file on hello first request and then compresses and caches hello file for subsequent requests.</span></span>  <span data-ttu-id="6a1ff-198">A fájlok `Cache-Control: no-cache` fejléc soha nem tömöríthetők.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-198">Files with `Cache-Control: no-cache` header will never be compressed.</span></span> |
| <span data-ttu-id="6a1ff-199">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-199">Uncompressed</span></span> |<span data-ttu-id="6a1ff-200">Tömörített</span><span class="sxs-lookup"><span data-stu-id="6a1ff-200">Compressed</span></span> |<span data-ttu-id="6a1ff-201">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-201">Uncompressed</span></span> |<span data-ttu-id="6a1ff-202">CDN kitömörítés hajt végre.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-202">CDN performs decompression</span></span> |
| <span data-ttu-id="6a1ff-203">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-203">Uncompressed</span></span> |<span data-ttu-id="6a1ff-204">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-204">Uncompressed</span></span> |<span data-ttu-id="6a1ff-205">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-205">Uncompressed</span></span> | |
| <span data-ttu-id="6a1ff-206">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-206">Uncompressed</span></span> |<span data-ttu-id="6a1ff-207">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="6a1ff-207">Not cached</span></span> |<span data-ttu-id="6a1ff-208">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="6a1ff-208">Uncompressed</span></span> | |

## <a name="media-services-cdn-compression"></a><span data-ttu-id="6a1ff-209">Media Services-CDN tömörítés</span><span class="sxs-lookup"><span data-stu-id="6a1ff-209">Media Services CDN Compression</span></span>
<span data-ttu-id="6a1ff-210">A Media Services CDN streamvégpontok engedélyezve van, tömörítés engedélyezve van-e alapértelmezés szerint a következő tartalomtípus hello: alkalmazás/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, alkalmazás vagy f4m + xml.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-210">For Media Services CDN enabled streaming endpoints, compression is enabled by default for hello following content types: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m+xml.</span></span> <span data-ttu-id="6a1ff-211">Engedélyezi/letiltja hello tömörítése nem említett hello Azure portál segítségével.</span><span class="sxs-lookup"><span data-stu-id="6a1ff-211">You cannot enable/disable compression for hello mentioned types using hello Azure portal.</span></span>  

## <a name="see-also"></a><span data-ttu-id="6a1ff-212">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6a1ff-212">See also</span></span>
* [<span data-ttu-id="6a1ff-213">CDN-fájltömörítés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="6a1ff-213">Troubleshooting CDN file compression</span></span>](cdn-troubleshoot-compression.md)    

