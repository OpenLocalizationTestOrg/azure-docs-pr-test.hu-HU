---
title: "Azure CDN gyorsítótár-viselkedést lekérdezési karakterláncokat tartalmazó aaaControl |} Microsoft Docs"
description: "Az Azure CDN lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e7a138b2decec624a29eb703ad9a291d19c44ee8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a><span data-ttu-id="ca41d-103">Vezérlő Azure CDN a lekérdezési karakterláncok gyorsítótárazásának</span><span class="sxs-lookup"><span data-stu-id="ca41d-103">Control Azure CDN caching behavior with query strings</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca41d-104">Standard</span><span class="sxs-lookup"><span data-stu-id="ca41d-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="ca41d-105">Verizon Azure CDN Premium</span><span class="sxs-lookup"><span data-stu-id="ca41d-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="ca41d-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ca41d-106">Overview</span></span>
<span data-ttu-id="ca41d-107">Lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő.</span><span class="sxs-lookup"><span data-stu-id="ca41d-107">Query string caching controls how files are toobe cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ca41d-108">hello Standard és prémium szintű CDN-termékek rendelkeznek hello azonos lekérdezési karakterlánc-gyorsítótár működése, de hello felhasználói felület eltér.</span><span class="sxs-lookup"><span data-stu-id="ca41d-108">hello Standard and Premium CDN products provide hello same query string caching functionality, but hello user interface differs.</span></span>  <span data-ttu-id="ca41d-109">Ez a dokumentum ismerteti a hello felület **Azure CDN Standard Akamai** és **Azure CDN Standard verizon**.</span><span class="sxs-lookup"><span data-stu-id="ca41d-109">This document describes hello interface for **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**.</span></span>  <span data-ttu-id="ca41d-110">A lekérdezési karakterláncok gyorsítótárazása a **verizon Azure CDN Premium**, lásd: [a lekérdezési karakterláncok - prémium szintű CDN gyorsítótárazási viselkedésének vezérlése kérelmek](cdn-query-string-premium.md).</span><span class="sxs-lookup"><span data-stu-id="ca41d-110">For query string caching with **Azure CDN Premium from Verizon**, see [Controlling caching behavior of CDN requests with query strings - Premium](cdn-query-string-premium.md).</span></span>
> 
> 

<span data-ttu-id="ca41d-111">Három módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="ca41d-111">Three modes are available:</span></span>

* <span data-ttu-id="ca41d-112">**Lekérdezési karakterláncok figyelmen kívül**: Ez az alapértelmezett mód hello.</span><span class="sxs-lookup"><span data-stu-id="ca41d-112">**Ignore query strings**:  This is hello default mode.</span></span>  <span data-ttu-id="ca41d-113">hello CDN élcsomópont továbbítják hello lekérdezési karakterlánc hello kérelmező toohello forrásból hello első kérelem és a gyorsítótár hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="ca41d-113">hello CDN edge node will pass hello query string from hello requestor toohello origin on hello first request and cache hello asset.</span></span>  <span data-ttu-id="ca41d-114">Az összes további kérelmet, hogy az eszköz a hello élcsomópont szolgáltatott figyelmen kívül hagyja majd hello lekérdezési karakterlánc eléréséig hello gyorsítótárazott eszköz.</span><span class="sxs-lookup"><span data-stu-id="ca41d-114">All subsequent requests for that asset that are served from hello edge node will ignore hello query string until hello cached asset expires.</span></span>
* <span data-ttu-id="ca41d-115">**URL-címet a lekérdezési sztringek gyorsítótárazásának mellőzése**: Ebben a módban a lekérdezési karakterláncot tartalmazó kérelmek nem gyorsítótárazzák a hello CDN peremhálózati csomópontban.</span><span class="sxs-lookup"><span data-stu-id="ca41d-115">**Bypass caching for URL with query strings**:  In this mode, requests with query strings are not cached at hello CDN edge node.</span></span>  <span data-ttu-id="ca41d-116">hello élcsomópont hello eszköz lekérdezi közvetlenül hello forrásból, és átadja toohello kérelmező minden egyes kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="ca41d-116">hello edge node retrieves hello asset directly from hello origin and passes it toohello requestor with each request.</span></span>
* <span data-ttu-id="ca41d-117">**Minden egyedi URL-cím gyorsítótárazása**: Ebben a módban a lekérdezési karakterlánc az egyes kérelmek kezeli a saját gyorsítótárával egyedi eszközként.</span><span class="sxs-lookup"><span data-stu-id="ca41d-117">**Cache every unique URL**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="ca41d-118">Például a hello eredeti adatforrást kérelmet kapott válasz hello *foo.ashx?q=bar* ehhez hello élcsomópont gyorsítótárazott és későbbi gyorsítótárak az, hogy ugyanazt a lekérdezési karakterláncot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="ca41d-118">For example, hello response from hello origin for a request for *foo.ashx?q=bar* would be cached at hello edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="ca41d-119">A kérelem *foo.ashx?q=somethingelse* volna a saját toolive idővel külön eszközként gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="ca41d-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time toolive.</span></span>

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a><span data-ttu-id="ca41d-120">Lekérdezési karakterláncok gyorsítótárazásának standard szintű CDN-profil beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ca41d-120">Changing query string caching settings for standard CDN profiles</span></span>
1. <span data-ttu-id="ca41d-121">A CDN-profil panelje hello kattintson az hello CDN-végpont toomanage kívánja.</span><span class="sxs-lookup"><span data-stu-id="ca41d-121">From hello CDN profile blade, click hello CDN endpoint you wish toomanage.</span></span>
   
    ![CDN-profil blade-végpontok](./media/cdn-query-string/cdn-endpoints.png)
   
    <span data-ttu-id="ca41d-123">CDN-végpont hello panelje megnyílik.</span><span class="sxs-lookup"><span data-stu-id="ca41d-123">hello CDN endpoint blade opens.</span></span>
2. <span data-ttu-id="ca41d-124">Kattintson a hello **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca41d-124">Click hello **Configure** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-query-string/cdn-config-btn.png)
   
    <span data-ttu-id="ca41d-126">hello CDN konfigurációs panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="ca41d-126">hello CDN Configuration blade opens.</span></span>
3. <span data-ttu-id="ca41d-127">Válasszon egy beállítást a hello **lekérdezési sztringek gyorsítótárazásának** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="ca41d-127">Select a setting from hello **Query string caching behavior** dropdown.</span></span>
   
    ![A CDN a lekérdezési karakterláncban a gyorsítótár](./media/cdn-query-string/cdn-query-string.png)
4. <span data-ttu-id="ca41d-129">Miután választott, kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca41d-129">After making your selection, click hello **Save** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ca41d-130">hello módosításait nem lehet azonnal láthatóak, mivel némi időre van szükség hello regisztrációs toopropagate hello CDN keresztül.</span><span class="sxs-lookup"><span data-stu-id="ca41d-130">hello settings changes may not be immediately visible, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="ca41d-131">Az <b>Akamai Azure CDN</b> típusú profilok propagálása általában egy percen belül befejeződik.</span><span class="sxs-lookup"><span data-stu-id="ca41d-131">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="ca41d-132">A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="ca41d-132">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

