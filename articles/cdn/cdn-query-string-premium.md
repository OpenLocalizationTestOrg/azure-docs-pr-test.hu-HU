---
title: "Szabályozhatja az Azure CDN a lekérdezési karakterláncok - prémium gyorsítótárazásának |} Microsoft Docs"
description: "Az Azure CDN lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai gyorsítótárazni a lekérdezési karakterláncokat tartalmazó vezérlő."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 145067c2ce50b41c4783f4de4052c0e7cb529fc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a><span data-ttu-id="b80ed-103">Vezérlő Azure CDN a lekérdezési karakterláncok - prémium gyorsítótárazásának</span><span class="sxs-lookup"><span data-stu-id="b80ed-103">Control Azure CDN caching behavior with query strings - Premium</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b80ed-104">Standard</span><span class="sxs-lookup"><span data-stu-id="b80ed-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="b80ed-105">Verizon Azure CDN Premium</span><span class="sxs-lookup"><span data-stu-id="b80ed-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="b80ed-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b80ed-106">Overview</span></span>
<span data-ttu-id="b80ed-107">Lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai gyorsítótárazni a lekérdezési karakterláncokat tartalmazó vezérlő.</span><span class="sxs-lookup"><span data-stu-id="b80ed-107">Query string caching controls how files are to be cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b80ed-108">A Standard és prémium szintű CDN-termékek, adja meg ugyanazt a lekérdezési karakterláncot gyorsítótárazási funkció, de a felhasználói felület eltér.</span><span class="sxs-lookup"><span data-stu-id="b80ed-108">The Standard and Premium CDN products provide the same query string caching functionality, but the user interface differs.</span></span>  <span data-ttu-id="b80ed-109">Ez a dokumentum ismerteti a felület **verizon Azure CDN Premium**.</span><span class="sxs-lookup"><span data-stu-id="b80ed-109">This document describes the interface for **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="b80ed-110">A lekérdezési karakterláncok gyorsítótárazása a **Azure CDN Standard Akamai** és **Azure CDN Standard verizon**, lásd: [lekérdezési karakterláncokat tartalmazó kérelmek gyorsítótárazási CDN viselkedésének vezérlése](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="b80ed-110">For query string caching with **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**, see [Controlling caching behavior of CDN requests with query strings](cdn-query-string.md).</span></span>
> 
> 

<span data-ttu-id="b80ed-111">Három módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="b80ed-111">Three modes are available:</span></span>

* <span data-ttu-id="b80ed-112">**Standard-gyorsítótár**: Ez az az alapértelmezett mód.</span><span class="sxs-lookup"><span data-stu-id="b80ed-112">**standard-cache**:  This is the default mode.</span></span>  <span data-ttu-id="b80ed-113">A CDN élcsomópont fogja továbbítani a lekérdezési karakterláncot a kérelmezőnek az eredeti az első kérelem és a gyorsítótár az eszköz.</span><span class="sxs-lookup"><span data-stu-id="b80ed-113">The CDN edge node will pass the query string from the requestor to the origin on the first request and cache the asset.</span></span>  <span data-ttu-id="b80ed-114">Az összes további kérelmet, hogy az eszköz az élcsomópont szolgáltatott eléréséig a gyorsítótárazott eszköz figyelmen kívül hagyja a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b80ed-114">All subsequent requests for that asset that are served from the edge node will ignore the query string until the cached asset expires.</span></span>
* <span data-ttu-id="b80ed-115">**no-cache**: Ebben a módban a lekérdezési karakterláncot tartalmazó kérelmek nem gyorsítótárazzák a CDN a peremhálózati csomóponton.</span><span class="sxs-lookup"><span data-stu-id="b80ed-115">**no-cache**:  In this mode, requests with query strings are not cached at the CDN edge node.</span></span>  <span data-ttu-id="b80ed-116">Élcsomópont olvassa be az objektumot közvetlenül a forrásból, és minden egyes kérelemmel a kérelmező számára továbbítja azokat.</span><span class="sxs-lookup"><span data-stu-id="b80ed-116">The edge node retrieves the asset directly from the origin and passes it to the requestor with each request.</span></span>
* <span data-ttu-id="b80ed-117">**egyedi gyorsítótár**: Ebben a módban a lekérdezési karakterlánc az egyes kérelmek kezeli a saját gyorsítótárával egyedi eszközként.</span><span class="sxs-lookup"><span data-stu-id="b80ed-117">**unique-cache**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="b80ed-118">Például az eredeti adatforrást kérelmet kapott válasz *foo.ashx?q=bar* ehhez az élcsomóponthoz gyorsítótárazott és későbbi gyorsítótárak az, hogy ugyanazt a lekérdezési karakterláncot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="b80ed-118">For example, the response from the origin for a request for *foo.ashx?q=bar* would be cached at the edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="b80ed-119">A kérelem *foo.ashx?q=somethingelse* külön eszközként saját, amikor élő volna gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="b80ed-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time to live.</span></span>

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a><span data-ttu-id="b80ed-120">Lekérdezési karakterláncok gyorsítótárazásának prémium szintű CDN-profil beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="b80ed-120">Changing query string caching settings for premium CDN profiles</span></span>
1. <span data-ttu-id="b80ed-121">A CDN-profil panelje, kattintson a **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b80ed-121">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    <span data-ttu-id="b80ed-123">Megnyitja a CDN-felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="b80ed-123">The CDN management portal opens.</span></span>
2. <span data-ttu-id="b80ed-124">Vigye a **HTTP nagy** lapra, és vigye a **gyorsítótár beállításainak** menü.</span><span class="sxs-lookup"><span data-stu-id="b80ed-124">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="b80ed-125">Kattintson a **lekérdezési karakterlánc-gyorsítótár**.</span><span class="sxs-lookup"><span data-stu-id="b80ed-125">Click on **Query-String Caching**.</span></span>
   
    <span data-ttu-id="b80ed-126">Lekérdezési karakterlánc gyorsítótárazási beállítások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b80ed-126">Query string caching options are displayed.</span></span>
   
    ![A CDN a lekérdezési karakterláncban a gyorsítótár](./media/cdn-query-string-premium/cdn-query-string.png)
3. <span data-ttu-id="b80ed-128">Miután választott, kattintson a **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b80ed-128">After making your selection, click the **Update** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b80ed-129">A beállítások módosításait nem lehet azonnal láthatóak, mivel némi időre van a CDN propagálásához a regisztráció.</span><span class="sxs-lookup"><span data-stu-id="b80ed-129">The settings changes may not be immediately visible, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="b80ed-130">A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b80ed-130">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

