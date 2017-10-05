---
title: "Az Azure CDN fájltömörítés elhárítása |} Microsoft Docs"
description: "Azure CDN fájltömörítés elhárítása."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5ef8a8262eb40aa827161764f03a63d031e43273
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-file-compression"></a><span data-ttu-id="55c8b-103">A CDN-fájlok tömörítési hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="55c8b-103">Troubleshooting CDN file compression</span></span>
<span data-ttu-id="55c8b-104">Ez a cikk segít elhárítása [CDN fájltömörítés](cdn-improve-performance.md).</span><span class="sxs-lookup"><span data-stu-id="55c8b-104">This article helps you troubleshoot issues with [CDN file compression](cdn-improve-performance.md).</span></span>

<span data-ttu-id="55c8b-105">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők a [az MSDN Azure és a Stack Overflow fórumok](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="55c8b-105">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and the Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="55c8b-106">Másik lehetőségként is fájl is az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="55c8b-106">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="55c8b-107">Lépjen a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) kattintson **támogatás**.</span><span class="sxs-lookup"><span data-stu-id="55c8b-107">Go to the [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="55c8b-108">Jelenség</span><span class="sxs-lookup"><span data-stu-id="55c8b-108">Symptom</span></span>
<span data-ttu-id="55c8b-109">A végponthoz tömörítés engedélyezve van, de tömörített fájlok vannak visszaküldött.</span><span class="sxs-lookup"><span data-stu-id="55c8b-109">Compression for your endpoint is enabled, but files are being returned uncompressed.</span></span>

> [!TIP]
> <span data-ttu-id="55c8b-110">Annak ellenőrzéséhez, hogy a fájlok tömörített vannak visszaküldött kell használnia egy eszköz, például [Fiddler](http://www.telerik.com/fiddler) vagy a böngésző [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="55c8b-110">To check whether your files are being returned compressed, you need to use a tool like [Fiddler](http://www.telerik.com/fiddler) or your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>  <span data-ttu-id="55c8b-111">Ellenőrizze a HTTP-válaszfejlécek hibát adott vissza a gyorsítótárazott CDN tartalom.</span><span class="sxs-lookup"><span data-stu-id="55c8b-111">Check the HTTP response headers returned with your cached CDN content.</span></span>  <span data-ttu-id="55c8b-112">Ha nincs nevű fejléc `Content-Encoding` értékkel rendelkező **gzip**, **bzip2**, vagy **deflate**, a tartalom tömörített.</span><span class="sxs-lookup"><span data-stu-id="55c8b-112">If there is a header named `Content-Encoding` with a value of **gzip**, **bzip2**, or **deflate**, your content is compressed.</span></span>
> 
> ![Tartalom-Encoding fejlécnek](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a><span data-ttu-id="55c8b-114">Ok</span><span class="sxs-lookup"><span data-stu-id="55c8b-114">Cause</span></span>
<span data-ttu-id="55c8b-115">Van több lehetséges oka, beleértve:</span><span class="sxs-lookup"><span data-stu-id="55c8b-115">There are several possible causes, including:</span></span>

* <span data-ttu-id="55c8b-116">A kért tartalma nem jogosult a tömörítés.</span><span class="sxs-lookup"><span data-stu-id="55c8b-116">The requested content is not eligible for compression.</span></span>
* <span data-ttu-id="55c8b-117">Tömörítés nincs engedélyezve a kért fájl típusa.</span><span class="sxs-lookup"><span data-stu-id="55c8b-117">Compression is not enabled for the requested file type.</span></span>
* <span data-ttu-id="55c8b-118">A HTTP-kérelem nem tartalmazza az egy érvényes tömörítési típus a kért fejléc.</span><span class="sxs-lookup"><span data-stu-id="55c8b-118">The HTTP request did not include a header requesting a valid compression type.</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="55c8b-119">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="55c8b-119">Troubleshooting steps</span></span>
> [!TIP]
> <span data-ttu-id="55c8b-120">Csakúgy, mint a központi telepítése új végpontok, CDN konfigurációs módosítások időigényes lehet a hálózaton belüli propagálásához.</span><span class="sxs-lookup"><span data-stu-id="55c8b-120">As with deploying new endpoints, CDN configuration changes take some time to propagate through the network.</span></span>  <span data-ttu-id="55c8b-121">Általában a módosításai érvényesek lesznek 90 percen belül.</span><span class="sxs-lookup"><span data-stu-id="55c8b-121">Usually, changes are applied within 90 minutes.</span></span>  <span data-ttu-id="55c8b-122">Ha az első alkalommal beállítása tömörítés a CDN-végpont, vegye figyelembe, hogy a beállítások a POP való propagálása tömörítés 1 – 2 óra vár.</span><span class="sxs-lookup"><span data-stu-id="55c8b-122">If this is the first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours to be sure the compression settings have propagated to the POPs.</span></span> 
> 
> 

### <a name="verify-the-request"></a><span data-ttu-id="55c8b-123">A kérelem ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="55c8b-123">Verify the request</span></span>
<span data-ttu-id="55c8b-124">Először azt kell tennie a kérelem gyors megerősítést ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="55c8b-124">First, we should do a quick sanity check on the request.</span></span>  <span data-ttu-id="55c8b-125">A böngésző használható [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) kérelem alatt megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="55c8b-125">You can use your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) to view the requests being made.</span></span>

* <span data-ttu-id="55c8b-126">Ellenőrizze, hogy a kérelmet küld a végponti URL-cím `<endpointname>.azureedge.net`, és nem a forrása.</span><span class="sxs-lookup"><span data-stu-id="55c8b-126">Verify the request is being sent to your endpoint URL, `<endpointname>.azureedge.net`, and not your origin.</span></span>
* <span data-ttu-id="55c8b-127">Ellenőrizze a kérelem tartalmazza egy **elfogadás kódolás** fejlécet, valamint az, hogy a fejléc értéke tartalmaz **gzip**, **deflate**, vagy **bzip2**.</span><span class="sxs-lookup"><span data-stu-id="55c8b-127">Verify the request contains an **Accept-Encoding** header, and the value for that header contains **gzip**, **deflate**, or **bzip2**.</span></span>

> [!NOTE]
> <span data-ttu-id="55c8b-128">**Akamai Azure CDN** csak támogatási profilok **gzip** kódolást.</span><span class="sxs-lookup"><span data-stu-id="55c8b-128">**Azure CDN from Akamai** profiles only support **gzip** encoding.</span></span>
> 
> 

![CDN-kérelemfejlécekben](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a><span data-ttu-id="55c8b-130">Ellenőrizze a tömörítési beállítások (standard szintű CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="55c8b-130">Verify compression settings (Standard CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="55c8b-131">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Standard verizon** vagy **Azure CDN Standard Akamai** profil.</span><span class="sxs-lookup"><span data-stu-id="55c8b-131">This step only applies if your CDN profile is an **Azure CDN Standard from Verizon** or **Azure CDN Standard from Akamai** profile.</span></span> 
> 
> 

<span data-ttu-id="55c8b-132">A következőben szereplő végpontnál: keresse meg a [Azure-portálon](https://portal.azure.com) , és kattintson a **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="55c8b-132">Navigate to your endpoint in the [Azure portal](https://portal.azure.com) and click the **Configure** button.</span></span>

* <span data-ttu-id="55c8b-133">Ellenőrizze a tömörítés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="55c8b-133">Verify compression is enabled.</span></span>
* <span data-ttu-id="55c8b-134">Ellenőrizze, hogy a tömörített formátumok listája szerepel a MIME-típus a tartalom tömörítését.</span><span class="sxs-lookup"><span data-stu-id="55c8b-134">Verify the MIME type for the content to be compressed is included in the list of compressed formats.</span></span>

![CDN-tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a><span data-ttu-id="55c8b-136">Ellenőrizze a tömörítési beállítások (prémium szintű CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="55c8b-136">Verify compression settings (Premium CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="55c8b-137">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **verizon Azure CDN Premium** profil.</span><span class="sxs-lookup"><span data-stu-id="55c8b-137">This step only applies if your CDN profile is an **Azure CDN Premium from Verizon** profile.</span></span>
> 
> 

<span data-ttu-id="55c8b-138">A következőben szereplő végpontnál: keresse meg a [Azure-portálon](https://portal.azure.com) , és kattintson a **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="55c8b-138">Navigate to your endpoint in the [Azure portal](https://portal.azure.com) and click the **Manage** button.</span></span>  <span data-ttu-id="55c8b-139">Ekkor megnyílik a kiegészítő portálon.</span><span class="sxs-lookup"><span data-stu-id="55c8b-139">The supplemental portal will open.</span></span>  <span data-ttu-id="55c8b-140">Vigye a **HTTP nagy** lapra, és vigye a **gyorsítótár beállításainak** menü.</span><span class="sxs-lookup"><span data-stu-id="55c8b-140">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="55c8b-141">Kattintson a **tömörítés**.</span><span class="sxs-lookup"><span data-stu-id="55c8b-141">Click **Compression**.</span></span> 

* <span data-ttu-id="55c8b-142">Ellenőrizze a tömörítés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="55c8b-142">Verify compression is enabled.</span></span>
* <span data-ttu-id="55c8b-143">Ellenőrizze a **fájltípusok** lista tartalmazza-e egy vesszővel tagolt listája (szóközök nélkül) a MIME-típusok.</span><span class="sxs-lookup"><span data-stu-id="55c8b-143">Verify the **File Types** list contains a comma-separated list (no spaces) of MIME types.</span></span>
* <span data-ttu-id="55c8b-144">Ellenőrizze, hogy a tömörített formátumok listája szerepel a MIME-típus a tartalom tömörítését.</span><span class="sxs-lookup"><span data-stu-id="55c8b-144">Verify the MIME type for the content to be compressed is included in the list of compressed formats.</span></span>

![CDN premium tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a><span data-ttu-id="55c8b-146">Ellenőrizze, hogy a tartalom gyorsítótárazva van</span><span class="sxs-lookup"><span data-stu-id="55c8b-146">Verify the content is cached</span></span>
> [!NOTE]
> <span data-ttu-id="55c8b-147">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Verizon** profil (Standard vagy prémium).</span><span class="sxs-lookup"><span data-stu-id="55c8b-147">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="55c8b-148">A böngésző fejlesztői eszközök segítségével, ellenőrizze annak érdekében, hogy a fájl tárolja a rendszer a régió, ahol kérik a response fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="55c8b-148">Using your browser's developer tools, check the response headers to ensure the file is cached in the region where it is being requested.</span></span>

* <span data-ttu-id="55c8b-149">Ellenőrizze a **Server** válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="55c8b-149">Check the **Server** response header.</span></span>  <span data-ttu-id="55c8b-150">A fejléc kell rendelkeznie a formátum **Platform (POP-kiszolgáló azonosítója)**, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="55c8b-150">The header should have the format **Platform (POP/Server ID)**, as seen in the following example.</span></span>
* <span data-ttu-id="55c8b-151">Ellenőrizze a **X-gyorsítótár** válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="55c8b-151">Check the **X-Cache** response header.</span></span>  <span data-ttu-id="55c8b-152">Olvassa el a fejléc **TALÁLATI**.</span><span class="sxs-lookup"><span data-stu-id="55c8b-152">The header should read **HIT**.</span></span>  

![CDN-válaszfejlécek](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a><span data-ttu-id="55c8b-154">Ellenőrizze a fájl megfelel-e a kötetméretek követelményeit</span><span class="sxs-lookup"><span data-stu-id="55c8b-154">Verify the file meets the size requirements</span></span>
> [!NOTE]
> <span data-ttu-id="55c8b-155">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Verizon** profil (Standard vagy prémium).</span><span class="sxs-lookup"><span data-stu-id="55c8b-155">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="55c8b-156">Jogosult tömörítési, egy fájlt a következő méret követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="55c8b-156">To be eligible for compression, a file must meet the following size requirements:</span></span>

* <span data-ttu-id="55c8b-157">Nagyobb, mint 128 bájt.</span><span class="sxs-lookup"><span data-stu-id="55c8b-157">Larger than 128 bytes.</span></span>
* <span data-ttu-id="55c8b-158">1 MB-nál kisebb.</span><span class="sxs-lookup"><span data-stu-id="55c8b-158">Smaller than 1 MB.</span></span>

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a><span data-ttu-id="55c8b-159">A forrás kiszolgálón a kérés ellenőrzéséhez egy **keresztül** fejléc</span><span class="sxs-lookup"><span data-stu-id="55c8b-159">Check the request at the origin server for a **Via** header</span></span>
<span data-ttu-id="55c8b-160">A **keresztül** HTTP-fejléc határozza meg, hogy a webalkalmazás-kiszolgáló proxykiszolgáló által átadott a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="55c8b-160">The **Via** HTTP header indicates to the web server that the request is being passed by a proxy server.</span></span>  <span data-ttu-id="55c8b-161">Microsoft IIS-webkiszolgálókkal alapértelmezés szerint nem a válaszok tömörítésére vonatkozó Ha a kérelem tartalmazza a **keresztül** fejléc.</span><span class="sxs-lookup"><span data-stu-id="55c8b-161">Microsoft IIS web servers by default do not compress responses when the request contains a **Via** header.</span></span>  <span data-ttu-id="55c8b-162">Bírálja felül ezt a viselkedést, hajtsa végre a következő:</span><span class="sxs-lookup"><span data-stu-id="55c8b-162">To override this behavior, perform the following:</span></span>

* <span data-ttu-id="55c8b-163">**Az IIS 6**: [HcNoCompressionForProxies állítsa "FALSE" = az IIS-metabázis-tulajdonságok](https://msdn.microsoft.com/library/ms525390.aspx)</span><span class="sxs-lookup"><span data-stu-id="55c8b-163">**IIS 6**: [Set HcNoCompressionForProxies="FALSE" in the IIS Metabase properties](https://msdn.microsoft.com/library/ms525390.aspx)</span></span>
* <span data-ttu-id="55c8b-164">**Az IIS 7 és legfeljebb**: [mind **noCompressionForHttp10** és **noCompressionForProxies** FALSE, a kiszolgáló konfigurációjában](http://www.iis.net/configreference/system.webserver/httpcompression)</span><span class="sxs-lookup"><span data-stu-id="55c8b-164">**IIS 7 and up**: [Set both **noCompressionForHttp10** and **noCompressionForProxies** to False in the server configuration](http://www.iis.net/configreference/system.webserver/httpcompression)</span></span>

