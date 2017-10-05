---
title: "Fájlok tömörítése Azure CDN szolgáltatás használata a jobb teljesítmény érdekében |} Microsoft Docs"
description: "Ismerje meg, hogyan lehet fokozni az fájl átviteli sebesség és az Azure CDN szolgáltatás használata a fájlok tömörítésével lap betöltési teljesítményét növeli."
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
ms.openlocfilehash: 7546650e6096a880f4fb4d0c94dd4ecc00b70160
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a><span data-ttu-id="12573-103">A jobb teljesítmény érdekében az Azure CDN fájlok tömörítése</span><span class="sxs-lookup"><span data-stu-id="12573-103">Improve performance by compressing files in Azure CDN</span></span>
<span data-ttu-id="12573-104">Tömörítés a fájl átviteli sebesség növelése és a lap betöltése teljesítmény növeléséhez a fájlméret, mielőtt a kiszolgáló egyszerű és hatékony módszer.</span><span class="sxs-lookup"><span data-stu-id="12573-104">Compression is a simple and effective method to improve file transfer speed and increase page load performance by reducing file size before it is sent from the server.</span></span> <span data-ttu-id="12573-105">Csökkenti a sávszélességgel kapcsolatos költségek, és gyorsabban élményt nyújt a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="12573-105">It reduces bandwidth costs and provides a more responsive experience for your users.</span></span>

<span data-ttu-id="12573-106">Két módon lehet engedélyezni a tömörítést:</span><span class="sxs-lookup"><span data-stu-id="12573-106">There are two ways to enable compression:</span></span>

* <span data-ttu-id="12573-107">Tömörítésének engedélyezéséhez a forrás kiszolgálón, eset a CDN a tömörített fájlok keresztül továbbítja, és tömörített fájlok átadása az azokat kérő ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="12573-107">You can enable compression on your origin server, in which case the CDN passes through the compressed files and deliver compressed files to clients that request them.</span></span>
* <span data-ttu-id="12573-108">Közvetlenül a CDN peremhálózati kiszolgálóinak, amelyben eset a CDN tömöríti a fájlok engedélyezni a tömörítést, és a végfelhasználók számára, akkor is, ha a forráskiszolgáló nem tömöríti szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="12573-108">You can enable compression directly on CDN edge servers, in which case the CDN compresses the files and serve them to end users, even if they are not compressed by the origin server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12573-109">CDN-konfigurációs módosítások a hálózaton belüli propagálásához egy kis idő múlva.</span><span class="sxs-lookup"><span data-stu-id="12573-109">CDN configuration changes take some time to propagate through the network.</span></span>  <span data-ttu-id="12573-110">A <b>Akamai Azure CDN</b> -profilok propagálása általában fejeződik be, egy perc alatt.</span><span class="sxs-lookup"><span data-stu-id="12573-110">For <b>Azure CDN from Akamai</b> profiles, propagation usually completes in under one minute.</span></span>  <span data-ttu-id="12573-111">A <b>Azure CDN Verizon</b> profilok, általában látni fogja a módosítások alkalmazásához 90 percen belül.</span><span class="sxs-lookup"><span data-stu-id="12573-111">For <b>Azure CDN from Verizon</b> profiles, you will usually see your changes apply within 90 minutes.</span></span>  <span data-ttu-id="12573-112">Ha az első alkalommal beállítása tömörítés a CDN-végpont, vegye figyelembe, hogy a tömörítési beállítások propagálása előtt hibaelhárítási POP az 1 – 2 óra várakozás</span><span class="sxs-lookup"><span data-stu-id="12573-112">If this is the first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours to be sure the compression settings have propagated to the POPs before troubleshooting</span></span>
> 
> 

## <a name="enabling-compression"></a><span data-ttu-id="12573-113">Tömörítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="12573-113">Enabling compression</span></span>
> [!NOTE]
> <span data-ttu-id="12573-114">A Standard és prémium szintű CDN rétegek funkcionalitása azonos tömörítést, de a felhasználói felület eltér.</span><span class="sxs-lookup"><span data-stu-id="12573-114">The Standard and Premium CDN tiers provide the same compression functionality, but the user interface differs.</span></span>  <span data-ttu-id="12573-115">Standard és prémium szintű CDN rétegek közötti különbségekről további információkért lásd: [Azure CDN áttekintése](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12573-115">For more information about the differences between Standard and Premium CDN tiers, see [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

### <a name="standard-tier"></a><span data-ttu-id="12573-116">Standard csomag</span><span class="sxs-lookup"><span data-stu-id="12573-116">Standard tier</span></span>
> [!NOTE]
> <span data-ttu-id="12573-117">Ez a szakasz vonatkozik **Azure CDN Standard verizon** és **Azure CDN Standard Akamai** profilok.</span><span class="sxs-lookup"><span data-stu-id="12573-117">This section applies to **Azure CDN Standard from Verizon** and **Azure CDN Standard from Akamai** profiles.</span></span>
> 
> 

1. <span data-ttu-id="12573-118">A CDN-profil lapon kattintson a CDN-végpont felügyelni kíván.</span><span class="sxs-lookup"><span data-stu-id="12573-118">From the CDN profile page, click the CDN endpoint you wish to manage.</span></span>
   
    ![CDN-profil lap végpontok](./media/cdn-file-compression/cdn-endpoints.png)
   
    <span data-ttu-id="12573-120">A CDN-végpont lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="12573-120">The CDN endpoint page opens.</span></span>
2. <span data-ttu-id="12573-121">Kattintson a **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="12573-121">Click the **Configure** button.</span></span>
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-config-btn.png)
   
    <span data-ttu-id="12573-123">A CDN-konfiguráció lapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="12573-123">The CDN Configuration page opens.</span></span>
3. <span data-ttu-id="12573-124">Kapcsolja be a **tömörítés**.</span><span class="sxs-lookup"><span data-stu-id="12573-124">Turn on **Compression**.</span></span>
   
    ![CDN-tömörítési beállítások](./media/cdn-file-compression/cdn-compress-standard.png)
4. <span data-ttu-id="12573-126">Használja az alapértelmezett típusokat, vagy módosítsa a listát fájltípusok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="12573-126">Use the default types, or modify the list by removing or adding file types.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="12573-127">Lehetséges, közben nem ajánlott tömörítés alkalmazása tömörített formátumban, például ZIP, MP3, MP4, JPG stb.</span><span class="sxs-lookup"><span data-stu-id="12573-127">While possible, it is not recommended to apply compression to compressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span>
   > 
   > 
5. <span data-ttu-id="12573-128">A módosítások elvégzése után kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12573-128">After making your changes, click the **Save** button.</span></span>

### <a name="premium-tier"></a><span data-ttu-id="12573-129">Premium szintű csomag</span><span class="sxs-lookup"><span data-stu-id="12573-129">Premium tier</span></span>
> [!NOTE]
> <span data-ttu-id="12573-130">Ez a szakasz vonatkozik **verizon Azure CDN Premium** profilok.</span><span class="sxs-lookup"><span data-stu-id="12573-130">This section applies to **Azure CDN Premium from Verizon** profiles.</span></span>
> 
> 

1. <span data-ttu-id="12573-131">A CDN-profil lapon kattintson a **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="12573-131">From the CDN profile page, click the **Manage** button.</span></span>
   
    ![CDN-profil lapon kezelheti a gomb](./media/cdn-file-compression/cdn-manage-btn.png)
   
    <span data-ttu-id="12573-133">Megnyitja a CDN-felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="12573-133">The CDN management portal opens.</span></span>
2. <span data-ttu-id="12573-134">Vigye a **HTTP nagy** lapra, és vigye a **gyorsítótár beállításainak** menü.</span><span class="sxs-lookup"><span data-stu-id="12573-134">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="12573-135">Kattintson a **tömörítés**.</span><span class="sxs-lookup"><span data-stu-id="12573-135">Click on **Compression**.</span></span>

    ![Fájl tömörítése kiválasztása](./media/cdn-file-compression/cdn-compress-select.png)
   
    <span data-ttu-id="12573-137">Tömörítési beállítások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="12573-137">Compression options are displayed.</span></span>
   
    ![Fájl tömörítése](./media/cdn-file-compression/cdn-compress-files.png)
3. <span data-ttu-id="12573-139">Tömörítésének engedélyezéséhez kattintson a **tömörítés engedélyezve** választógombot.</span><span class="sxs-lookup"><span data-stu-id="12573-139">Enable compression by clicking the **Compression Enabled** radio button.</span></span>  <span data-ttu-id="12573-140">A MIME-típusok kívánja tömörítése (szóközök nélkül) vesszővel tagolt lista formájában adja meg a **fájltípusok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="12573-140">Enter the MIME types you wish to compress as a comma-delimited list (no spaces) in the **File Types** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="12573-141">Lehetséges, közben nem ajánlott tömörítés alkalmazása tömörített formátumban, például ZIP, MP3, MP4, JPG stb.</span><span class="sxs-lookup"><span data-stu-id="12573-141">While possible, it is not recommended to apply compression to compressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span> 
   > 
   > 
4. <span data-ttu-id="12573-142">A módosítások elvégzése után kattintson a **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="12573-142">After making your changes, click the **Update** button.</span></span>

## <a name="compression-rules"></a><span data-ttu-id="12573-143">Tömörítés szabályok</span><span class="sxs-lookup"><span data-stu-id="12573-143">Compression rules</span></span>
<span data-ttu-id="12573-144">Ezek a táblázatok ismertetik Azure CDN tömörítési viselkedés minden forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="12573-144">These tables describe Azure CDN compression behavior for every scenario.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="12573-145">A **Azure CDN Verizon** (Standard és prémium), csak a megfelelő fájlok tömörítése.</span><span class="sxs-lookup"><span data-stu-id="12573-145">For **Azure CDN from Verizon** (Standard and Premium), only eligible files are compressed.</span></span>  <span data-ttu-id="12573-146">Jogosult tömörítési, egy fájlt kell:</span><span class="sxs-lookup"><span data-stu-id="12573-146">To be eligible for compression, a file must:</span></span>
> 
> * <span data-ttu-id="12573-147">Lehet nagyobb, mint 128 bájt.</span><span class="sxs-lookup"><span data-stu-id="12573-147">Be larger than 128 bytes.</span></span>
> * <span data-ttu-id="12573-148">Lehet kisebb, mint 1 MB.</span><span class="sxs-lookup"><span data-stu-id="12573-148">Be smaller than 1 MB.</span></span>
> 
> <span data-ttu-id="12573-149">A **Akamai Azure CDN**, tömörítési fájlok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="12573-149">For **Azure CDN from Akamai**, all files are eligible for compression.</span></span>
> 
> <span data-ttu-id="12573-150">Az összes Azure CDN-termékek, a kell lennie a MIME-típust, amelyet [tömörítési konfigurált](#enabling-compression).</span><span class="sxs-lookup"><span data-stu-id="12573-150">For all Azure CDN products, a file must be a MIME type that has been [configured for compression](#enabling-compression).</span></span>
> 
> <span data-ttu-id="12573-151">**Verizon Azure CDN** (Standard és prémium) támogatási profilok **gzip** (GNU zip), **deflate**, **bzip2**, vagy **br**(Brotli) kódolást.</span><span class="sxs-lookup"><span data-stu-id="12573-151">**Azure CDN from Verizon** profiles (Standard and Premium) support **gzip** (GNU zip), **deflate**, **bzip2**, or  **br** (Brotli) encoding.</span></span> <span data-ttu-id="12573-152">A kódolási Brotli, a tömörítés csak peremére végezhető el.</span><span class="sxs-lookup"><span data-stu-id="12573-152">For Brotli encoding, the compression is done only at the edge.</span></span> <span data-ttu-id="12573-153">Az ügyfélböngészőnek Brotli kódolási kérelmet kell küldenie, és a tömörített eszköz kell tömörített a forrás oldalon először.</span><span class="sxs-lookup"><span data-stu-id="12573-153">The client/browser must send the request for Brotli encoding and the compressed asset must have been compressed on the origin side first.</span></span> 
>
><span data-ttu-id="12573-154">**Akamai Azure CDN** profilok támogatása csak **gzip** kódolást.</span><span class="sxs-lookup"><span data-stu-id="12573-154">**Azure CDN from Akamai** profiles support only **gzip** encoding.</span></span>
> 
> <span data-ttu-id="12573-155">**Akamai Azure CDN** végpontok mindig kérjen **gzip** fájlt a forrásból, függetlenül az ügyfélkérés kódolású.</span><span class="sxs-lookup"><span data-stu-id="12573-155">**Azure CDN from Akamai** endpoints always request **gzip** encoded files from the origin, regardless of the client request.</span></span> 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a><span data-ttu-id="12573-156">A tömörítés le van tiltva, vagy a fájl nem tömörítés</span><span class="sxs-lookup"><span data-stu-id="12573-156">Compression disabled or file is ineligible for compression</span></span>
| <span data-ttu-id="12573-157">Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek)</span><span class="sxs-lookup"><span data-stu-id="12573-157">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="12573-158">Gyorsítótárazott fájlformátum</span><span class="sxs-lookup"><span data-stu-id="12573-158">Cached file format</span></span> | <span data-ttu-id="12573-159">Az ügyfél CDN válasz</span><span class="sxs-lookup"><span data-stu-id="12573-159">CDN response to the client</span></span> | <span data-ttu-id="12573-160">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="12573-160">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12573-161">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-161">Compressed</span></span> |<span data-ttu-id="12573-162">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-162">Compressed</span></span> |<span data-ttu-id="12573-163">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-163">Compressed</span></span> | |
| <span data-ttu-id="12573-164">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-164">Compressed</span></span> |<span data-ttu-id="12573-165">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-165">Uncompressed</span></span> |<span data-ttu-id="12573-166">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-166">Uncompressed</span></span> | |
| <span data-ttu-id="12573-167">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-167">Compressed</span></span> |<span data-ttu-id="12573-168">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="12573-168">Not cached</span></span> |<span data-ttu-id="12573-169">Tömörített és tömörítetlen</span><span class="sxs-lookup"><span data-stu-id="12573-169">Compressed or Uncompressed</span></span> |<span data-ttu-id="12573-170">Forrás válasz függ</span><span class="sxs-lookup"><span data-stu-id="12573-170">Depends on origin response</span></span> |
| <span data-ttu-id="12573-171">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-171">Uncompressed</span></span> |<span data-ttu-id="12573-172">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-172">Compressed</span></span> |<span data-ttu-id="12573-173">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-173">Uncompressed</span></span> | |
| <span data-ttu-id="12573-174">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-174">Uncompressed</span></span> |<span data-ttu-id="12573-175">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-175">Uncompressed</span></span> |<span data-ttu-id="12573-176">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-176">Uncompressed</span></span> | |
| <span data-ttu-id="12573-177">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-177">Uncompressed</span></span> |<span data-ttu-id="12573-178">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="12573-178">Not cached</span></span> |<span data-ttu-id="12573-179">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-179">Uncompressed</span></span> | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a><span data-ttu-id="12573-180">A tömörítés engedélyezve van és a fájl nem jogosult az tömörítés</span><span class="sxs-lookup"><span data-stu-id="12573-180">Compression enabled and file is eligible for compression</span></span>
| <span data-ttu-id="12573-181">Az ügyfél kért formátum (keresztül Accept-Encoding fejlécnek)</span><span class="sxs-lookup"><span data-stu-id="12573-181">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="12573-182">Gyorsítótárazott fájlformátum</span><span class="sxs-lookup"><span data-stu-id="12573-182">Cached file format</span></span> | <span data-ttu-id="12573-183">Az ügyfél CDN válasz</span><span class="sxs-lookup"><span data-stu-id="12573-183">CDN response to the client</span></span> | <span data-ttu-id="12573-184">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="12573-184">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12573-185">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-185">Compressed</span></span> |<span data-ttu-id="12573-186">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-186">Compressed</span></span> |<span data-ttu-id="12573-187">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-187">Compressed</span></span> |<span data-ttu-id="12573-188">CDN transcodes támogatott formátumok közötti</span><span class="sxs-lookup"><span data-stu-id="12573-188">CDN transcodes between supported formats</span></span> |
| <span data-ttu-id="12573-189">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-189">Compressed</span></span> |<span data-ttu-id="12573-190">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-190">Uncompressed</span></span> |<span data-ttu-id="12573-191">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-191">Compressed</span></span> |<span data-ttu-id="12573-192">CDN tömörítési hajt végre.</span><span class="sxs-lookup"><span data-stu-id="12573-192">CDN performs compression</span></span> |
| <span data-ttu-id="12573-193">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-193">Compressed</span></span> |<span data-ttu-id="12573-194">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="12573-194">Not cached</span></span> |<span data-ttu-id="12573-195">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-195">Compressed</span></span> |<span data-ttu-id="12573-196">CDN tömörítési hajt végre, ha a forrás visszaküldi tömörítetlen.</span><span class="sxs-lookup"><span data-stu-id="12573-196">CDN performs compression if origin returns uncompressed.</span></span>  <span data-ttu-id="12573-197">**Verizon Azure CDN** átadja az első kérésre tömörítetlen fájl és majd tömöríti és gyorsítótárazza a További kérelmeknél fájlt.</span><span class="sxs-lookup"><span data-stu-id="12573-197">**Azure CDN from Verizon** passes the uncompressed file on the first request and then compresses and caches the file for subsequent requests.</span></span>  <span data-ttu-id="12573-198">A fájlok `Cache-Control: no-cache` fejléc soha nem tömöríthetők.</span><span class="sxs-lookup"><span data-stu-id="12573-198">Files with `Cache-Control: no-cache` header will never be compressed.</span></span> |
| <span data-ttu-id="12573-199">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-199">Uncompressed</span></span> |<span data-ttu-id="12573-200">Tömörített</span><span class="sxs-lookup"><span data-stu-id="12573-200">Compressed</span></span> |<span data-ttu-id="12573-201">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-201">Uncompressed</span></span> |<span data-ttu-id="12573-202">CDN kitömörítés hajt végre.</span><span class="sxs-lookup"><span data-stu-id="12573-202">CDN performs decompression</span></span> |
| <span data-ttu-id="12573-203">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-203">Uncompressed</span></span> |<span data-ttu-id="12573-204">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-204">Uncompressed</span></span> |<span data-ttu-id="12573-205">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-205">Uncompressed</span></span> | |
| <span data-ttu-id="12573-206">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-206">Uncompressed</span></span> |<span data-ttu-id="12573-207">Nem gyorsítótárazott</span><span class="sxs-lookup"><span data-stu-id="12573-207">Not cached</span></span> |<span data-ttu-id="12573-208">Az Uncompressed</span><span class="sxs-lookup"><span data-stu-id="12573-208">Uncompressed</span></span> | |

## <a name="media-services-cdn-compression"></a><span data-ttu-id="12573-209">Media Services-CDN tömörítés</span><span class="sxs-lookup"><span data-stu-id="12573-209">Media Services CDN Compression</span></span>
<span data-ttu-id="12573-210">A Media Services CDN streamvégpontok engedélyezve van, tömörítés engedélyezve van-e alapértelmezés szerint a következő tartalomtípus: alkalmazás/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, alkalmazás vagy f4m + xml.</span><span class="sxs-lookup"><span data-stu-id="12573-210">For Media Services CDN enabled streaming endpoints, compression is enabled by default for the following content types: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m+xml.</span></span> <span data-ttu-id="12573-211">Ön nem engedélyezését vagy letiltását az említett típusa, az Azure portál használatával tömörítése.</span><span class="sxs-lookup"><span data-stu-id="12573-211">You cannot enable/disable compression for the mentioned types using the Azure portal.</span></span>  

## <a name="see-also"></a><span data-ttu-id="12573-212">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="12573-212">See also</span></span>
* [<span data-ttu-id="12573-213">CDN-fájltömörítés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="12573-213">Troubleshooting CDN file compression</span></span>](cdn-troubleshoot-compression.md)    

