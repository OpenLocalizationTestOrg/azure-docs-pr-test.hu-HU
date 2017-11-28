---
title: "az Azure CDN aaaTroubleshooting fájltömörítés |} Microsoft Docs"
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
ms.openlocfilehash: f00b98beaf6b3b3cd30108ece65a8191edc06ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-file-compression"></a><span data-ttu-id="01c49-103">A CDN-fájlok tömörítési hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="01c49-103">Troubleshooting CDN file compression</span></span>
<span data-ttu-id="01c49-104">Ez a cikk segít elhárítása [CDN fájltömörítés](cdn-improve-performance.md).</span><span class="sxs-lookup"><span data-stu-id="01c49-104">This article helps you troubleshoot issues with [CDN file compression](cdn-improve-performance.md).</span></span>

<span data-ttu-id="01c49-105">Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure hello és fórumok Stack Overflow hello](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="01c49-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="01c49-106">Másik lehetőségként is fájl is az Azure támogatási incidens.</span><span class="sxs-lookup"><span data-stu-id="01c49-106">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="01c49-107">Nyissa meg toohello [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) kattintson **támogatás**.</span><span class="sxs-lookup"><span data-stu-id="01c49-107">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="01c49-108">Jelenség</span><span class="sxs-lookup"><span data-stu-id="01c49-108">Symptom</span></span>
<span data-ttu-id="01c49-109">A végponthoz tömörítés engedélyezve van, de tömörített fájlok vannak visszaküldött.</span><span class="sxs-lookup"><span data-stu-id="01c49-109">Compression for your endpoint is enabled, but files are being returned uncompressed.</span></span>

> [!TIP]
> <span data-ttu-id="01c49-110">toocheck kell-e a fájlok tömörített vannak visszaadott, egy eszköz, például toouse [Fiddler](http://www.telerik.com/fiddler) vagy a böngésző [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="01c49-110">toocheck whether your files are being returned compressed, you need toouse a tool like [Fiddler](http://www.telerik.com/fiddler) or your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>  <span data-ttu-id="01c49-111">Jelölőnégyzet hello HTTP-válaszfejlécek hibát adott vissza a gyorsítótárazott CDN tartalom.</span><span class="sxs-lookup"><span data-stu-id="01c49-111">Check hello HTTP response headers returned with your cached CDN content.</span></span>  <span data-ttu-id="01c49-112">Ha nincs nevű fejléc `Content-Encoding` értékkel rendelkező **gzip**, **bzip2**, vagy **deflate**, a tartalom tömörített.</span><span class="sxs-lookup"><span data-stu-id="01c49-112">If there is a header named `Content-Encoding` with a value of **gzip**, **bzip2**, or **deflate**, your content is compressed.</span></span>
> 
> ![Tartalom-Encoding fejlécnek](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a><span data-ttu-id="01c49-114">Ok</span><span class="sxs-lookup"><span data-stu-id="01c49-114">Cause</span></span>
<span data-ttu-id="01c49-115">Van több lehetséges oka, beleértve:</span><span class="sxs-lookup"><span data-stu-id="01c49-115">There are several possible causes, including:</span></span>

* <span data-ttu-id="01c49-116">a kért hello tartalma nem jogosult a tömörítést.</span><span class="sxs-lookup"><span data-stu-id="01c49-116">hello requested content is not eligible for compression.</span></span>
* <span data-ttu-id="01c49-117">Tömörítés hello nincs engedélyezve a kért fájl típusa.</span><span class="sxs-lookup"><span data-stu-id="01c49-117">Compression is not enabled for hello requested file type.</span></span>
* <span data-ttu-id="01c49-118">hello HTTP-kérelem nem tartalmazza a kért egy érvényes tömörítési típus fejléc.</span><span class="sxs-lookup"><span data-stu-id="01c49-118">hello HTTP request did not include a header requesting a valid compression type.</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="01c49-119">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="01c49-119">Troubleshooting steps</span></span>
> [!TIP]
> <span data-ttu-id="01c49-120">Csakúgy, mint a központi telepítése új végpontok, CDN konfigurációs módosításokat bizonyos idő toopropagate hello a hálózaton keresztül igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="01c49-120">As with deploying new endpoints, CDN configuration changes take some time toopropagate through hello network.</span></span>  <span data-ttu-id="01c49-121">Általában a módosításai érvényesek lesznek 90 percen belül.</span><span class="sxs-lookup"><span data-stu-id="01c49-121">Usually, changes are applied within 90 minutes.</span></span>  <span data-ttu-id="01c49-122">Ha hello első alkalommal tömörítés a CDN-végpont kiépítését, érdemes 1 – 2 óra toobe meg arról, hogy hello tömörítési beállítások propagálása toohello POP vár.</span><span class="sxs-lookup"><span data-stu-id="01c49-122">If this is hello first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours toobe sure hello compression settings have propagated toohello POPs.</span></span> 
> 
> 

### <a name="verify-hello-request"></a><span data-ttu-id="01c49-123">Hello kérelem ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="01c49-123">Verify hello request</span></span>
<span data-ttu-id="01c49-124">Először azt kell tennie hello kérelem gyors megerősítést ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="01c49-124">First, we should do a quick sanity check on hello request.</span></span>  <span data-ttu-id="01c49-125">A böngésző használható [fejlesztői eszközök](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello küldött kéréseket.</span><span class="sxs-lookup"><span data-stu-id="01c49-125">You can use your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello requests being made.</span></span>

* <span data-ttu-id="01c49-126">Ellenőrizze a hello kérelmet küld a tooyour végponti URL-cím, `<endpointname>.azureedge.net`, és nem a forrása.</span><span class="sxs-lookup"><span data-stu-id="01c49-126">Verify hello request is being sent tooyour endpoint URL, `<endpointname>.azureedge.net`, and not your origin.</span></span>
* <span data-ttu-id="01c49-127">Ellenőrizze a hello kérelem tartalmaz egy **elfogadás kódolás** fejlécet, és hello értéke, hogy a fejléc tartalmazza **gzip**, **deflate**, vagy **bzip2** .</span><span class="sxs-lookup"><span data-stu-id="01c49-127">Verify hello request contains an **Accept-Encoding** header, and hello value for that header contains **gzip**, **deflate**, or **bzip2**.</span></span>

> [!NOTE]
> <span data-ttu-id="01c49-128">**Akamai Azure CDN** csak támogatási profilok **gzip** kódolást.</span><span class="sxs-lookup"><span data-stu-id="01c49-128">**Azure CDN from Akamai** profiles only support **gzip** encoding.</span></span>
> 
> 

![CDN-kérelemfejlécekben](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a><span data-ttu-id="01c49-130">Ellenőrizze a tömörítési beállítások (standard szintű CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="01c49-130">Verify compression settings (Standard CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="01c49-131">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Standard verizon** vagy **Azure CDN Standard Akamai** profil.</span><span class="sxs-lookup"><span data-stu-id="01c49-131">This step only applies if your CDN profile is an **Azure CDN Standard from Verizon** or **Azure CDN Standard from Akamai** profile.</span></span> 
> 
> 

<span data-ttu-id="01c49-132">Keresse meg a hello tooyour végpont [Azure-portálon](https://portal.azure.com) hello kattintson **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="01c49-132">Navigate tooyour endpoint in hello [Azure portal](https://portal.azure.com) and click hello **Configure** button.</span></span>

* <span data-ttu-id="01c49-133">Ellenőrizze a tömörítés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="01c49-133">Verify compression is enabled.</span></span>
* <span data-ttu-id="01c49-134">Ellenőrizze a tartalom tömörített toobe hello tömörített formátumok listája szerepel hello hello MIME-típust.</span><span class="sxs-lookup"><span data-stu-id="01c49-134">Verify hello MIME type for hello content toobe compressed is included in hello list of compressed formats.</span></span>

![CDN-tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a><span data-ttu-id="01c49-136">Ellenőrizze a tömörítési beállítások (prémium szintű CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="01c49-136">Verify compression settings (Premium CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="01c49-137">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **verizon Azure CDN Premium** profil.</span><span class="sxs-lookup"><span data-stu-id="01c49-137">This step only applies if your CDN profile is an **Azure CDN Premium from Verizon** profile.</span></span>
> 
> 

<span data-ttu-id="01c49-138">Keresse meg a hello tooyour végpont [Azure-portálon](https://portal.azure.com) hello kattintson **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="01c49-138">Navigate tooyour endpoint in hello [Azure portal](https://portal.azure.com) and click hello **Manage** button.</span></span>  <span data-ttu-id="01c49-139">Ekkor megnyílik a hello kiegészítő portálon.</span><span class="sxs-lookup"><span data-stu-id="01c49-139">hello supplemental portal will open.</span></span>  <span data-ttu-id="01c49-140">Hello az egérmutatót **HTTP nagy** fülre, majd az egérmutatót hello **gyorsítótár beállításainak** menü.</span><span class="sxs-lookup"><span data-stu-id="01c49-140">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="01c49-141">Kattintson a **tömörítés**.</span><span class="sxs-lookup"><span data-stu-id="01c49-141">Click **Compression**.</span></span> 

* <span data-ttu-id="01c49-142">Ellenőrizze a tömörítés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="01c49-142">Verify compression is enabled.</span></span>
* <span data-ttu-id="01c49-143">Ellenőrizze a hello **fájltípusok** lista tartalmazza-e egy vesszővel tagolt listája (szóközök nélkül) a MIME-típusok.</span><span class="sxs-lookup"><span data-stu-id="01c49-143">Verify hello **File Types** list contains a comma-separated list (no spaces) of MIME types.</span></span>
* <span data-ttu-id="01c49-144">Ellenőrizze a tartalom tömörített toobe hello tömörített formátumok listája szerepel hello hello MIME-típust.</span><span class="sxs-lookup"><span data-stu-id="01c49-144">Verify hello MIME type for hello content toobe compressed is included in hello list of compressed formats.</span></span>

![CDN premium tömörítési beállítások](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-hello-content-is-cached"></a><span data-ttu-id="01c49-146">Ellenőrizze a hello tartalom található a gyorsítótárban</span><span class="sxs-lookup"><span data-stu-id="01c49-146">Verify hello content is cached</span></span>
> [!NOTE]
> <span data-ttu-id="01c49-147">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Verizon** profil (Standard vagy prémium).</span><span class="sxs-lookup"><span data-stu-id="01c49-147">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="01c49-148">A böngésző fejlesztői eszközök segítségével ellenőrizze a hello fejlécek tooensure hello válaszfájl tárolja a rendszer hello régió, ahol kérik.</span><span class="sxs-lookup"><span data-stu-id="01c49-148">Using your browser's developer tools, check hello response headers tooensure hello file is cached in hello region where it is being requested.</span></span>

* <span data-ttu-id="01c49-149">Ellenőrizze a hello **Server** válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="01c49-149">Check hello **Server** response header.</span></span>  <span data-ttu-id="01c49-150">hello fejléc kell hello formátum **Platform (POP-kiszolgáló azonosítója)**, hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="01c49-150">hello header should have hello format **Platform (POP/Server ID)**, as seen in hello following example.</span></span>
* <span data-ttu-id="01c49-151">Ellenőrizze a hello **X-gyorsítótár** válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="01c49-151">Check hello **X-Cache** response header.</span></span>  <span data-ttu-id="01c49-152">olvassa el a hello fejléc **TALÁLATI**.</span><span class="sxs-lookup"><span data-stu-id="01c49-152">hello header should read **HIT**.</span></span>  

![CDN-válaszfejlécek](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-hello-file-meets-hello-size-requirements"></a><span data-ttu-id="01c49-154">Ellenőrizze a hello fájl megfelel hello mérete</span><span class="sxs-lookup"><span data-stu-id="01c49-154">Verify hello file meets hello size requirements</span></span>
> [!NOTE]
> <span data-ttu-id="01c49-155">Ez a lépés csak akkor alkalmazza, ha a CDN-profilt egy **Azure CDN Verizon** profil (Standard vagy prémium).</span><span class="sxs-lookup"><span data-stu-id="01c49-155">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="01c49-156">toobe tömörítési támogatható, egy fájl hello mérete követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="01c49-156">toobe eligible for compression, a file must meet hello following size requirements:</span></span>

* <span data-ttu-id="01c49-157">Nagyobb, mint 128 bájt.</span><span class="sxs-lookup"><span data-stu-id="01c49-157">Larger than 128 bytes.</span></span>
* <span data-ttu-id="01c49-158">1 MB-nál kisebb.</span><span class="sxs-lookup"><span data-stu-id="01c49-158">Smaller than 1 MB.</span></span>

### <a name="check-hello-request-at-hello-origin-server-for-a-via-header"></a><span data-ttu-id="01c49-159">Hello származási kiszolgálóján hello kérés ellenőrzéséhez egy **keresztül** fejléc</span><span class="sxs-lookup"><span data-stu-id="01c49-159">Check hello request at hello origin server for a **Via** header</span></span>
<span data-ttu-id="01c49-160">Hello **keresztül** HTTP-fejléc azt jelzi, hogy kérelem hello toohello webkiszolgálón van átadta-e a proxykiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="01c49-160">hello **Via** HTTP header indicates toohello web server that hello request is being passed by a proxy server.</span></span>  <span data-ttu-id="01c49-161">Microsoft IIS-webkiszolgálókkal alapértelmezés szerint nem a válaszok tömörítésére vonatkozó Ha hello kérelmet tartalmaz egy **keresztül** fejléc.</span><span class="sxs-lookup"><span data-stu-id="01c49-161">Microsoft IIS web servers by default do not compress responses when hello request contains a **Via** header.</span></span>  <span data-ttu-id="01c49-162">toooverride ezt a viselkedést hello következőket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="01c49-162">toooverride this behavior, perform hello following:</span></span>

* <span data-ttu-id="01c49-163">**Az IIS 6**: [beállítása HcNoCompressionForProxies = "FALSE" hello IIS-metaadatbázis tulajdonságai](https://msdn.microsoft.com/library/ms525390.aspx)</span><span class="sxs-lookup"><span data-stu-id="01c49-163">**IIS 6**: [Set HcNoCompressionForProxies="FALSE" in hello IIS Metabase properties](https://msdn.microsoft.com/library/ms525390.aspx)</span></span>
* <span data-ttu-id="01c49-164">**Az IIS 7 és legfeljebb**: [mind **noCompressionForHttp10** és **noCompressionForProxies** tooFalse hello-kiszolgálói konfigurációban](http://www.iis.net/configreference/system.webserver/httpcompression)</span><span class="sxs-lookup"><span data-stu-id="01c49-164">**IIS 7 and up**: [Set both **noCompressionForHttp10** and **noCompressionForProxies** tooFalse in hello server configuration](http://www.iis.net/configreference/system.webserver/httpcompression)</span></span>

