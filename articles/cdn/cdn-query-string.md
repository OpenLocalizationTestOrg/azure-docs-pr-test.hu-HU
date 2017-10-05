---
title: "Szabályozhatja az Azure CDN a lekérdezési karakterláncok gyorsítótárazásának |} Microsoft Docs"
description: "Az Azure CDN lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai gyorsítótárazni a lekérdezési karakterláncokat tartalmazó vezérlő."
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
ms.openlocfilehash: 8d79626fa8516f226a82d3dac693c2033904c91d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings"></a><span data-ttu-id="810ac-103">Vezérlő Azure CDN a lekérdezési karakterláncok gyorsítótárazásának</span><span class="sxs-lookup"><span data-stu-id="810ac-103">Control Azure CDN caching behavior with query strings</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="810ac-104">Standard</span><span class="sxs-lookup"><span data-stu-id="810ac-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="810ac-105">Verizon Azure CDN Premium</span><span class="sxs-lookup"><span data-stu-id="810ac-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="810ac-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="810ac-106">Overview</span></span>
<span data-ttu-id="810ac-107">Lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai gyorsítótárazni a lekérdezési karakterláncokat tartalmazó vezérlő.</span><span class="sxs-lookup"><span data-stu-id="810ac-107">Query string caching controls how files are to be cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="810ac-108">A Standard és prémium szintű CDN-termékek, adja meg ugyanazt a lekérdezési karakterláncot gyorsítótárazási funkció, de a felhasználói felület eltér.</span><span class="sxs-lookup"><span data-stu-id="810ac-108">The Standard and Premium CDN products provide the same query string caching functionality, but the user interface differs.</span></span>  <span data-ttu-id="810ac-109">Ez a dokumentum ismerteti a felület **Azure CDN Standard Akamai** és **Azure CDN Standard verizon**.</span><span class="sxs-lookup"><span data-stu-id="810ac-109">This document describes the interface for **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**.</span></span>  <span data-ttu-id="810ac-110">A lekérdezési karakterláncok gyorsítótárazása a **verizon Azure CDN Premium**, lásd: [a lekérdezési karakterláncok - prémium szintű CDN gyorsítótárazási viselkedésének vezérlése kérelmek](cdn-query-string-premium.md).</span><span class="sxs-lookup"><span data-stu-id="810ac-110">For query string caching with **Azure CDN Premium from Verizon**, see [Controlling caching behavior of CDN requests with query strings - Premium](cdn-query-string-premium.md).</span></span>
> 
> 

<span data-ttu-id="810ac-111">Három módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="810ac-111">Three modes are available:</span></span>

* <span data-ttu-id="810ac-112">**Lekérdezési karakterláncok figyelmen kívül**: Ez az az alapértelmezett mód.</span><span class="sxs-lookup"><span data-stu-id="810ac-112">**Ignore query strings**:  This is the default mode.</span></span>  <span data-ttu-id="810ac-113">A CDN élcsomópont fogja továbbítani a lekérdezési karakterláncot a kérelmezőnek az eredeti az első kérelem és a gyorsítótár az eszköz.</span><span class="sxs-lookup"><span data-stu-id="810ac-113">The CDN edge node will pass the query string from the requestor to the origin on the first request and cache the asset.</span></span>  <span data-ttu-id="810ac-114">Az összes további kérelmet, hogy az eszköz az élcsomópont szolgáltatott eléréséig a gyorsítótárazott eszköz figyelmen kívül hagyja a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="810ac-114">All subsequent requests for that asset that are served from the edge node will ignore the query string until the cached asset expires.</span></span>
* <span data-ttu-id="810ac-115">**URL-címet a lekérdezési sztringek gyorsítótárazásának mellőzése**: Ebben a módban a lekérdezési karakterláncot tartalmazó kérelmek nem gyorsítótárazzák a CDN a peremhálózati csomóponton.</span><span class="sxs-lookup"><span data-stu-id="810ac-115">**Bypass caching for URL with query strings**:  In this mode, requests with query strings are not cached at the CDN edge node.</span></span>  <span data-ttu-id="810ac-116">Élcsomópont olvassa be az objektumot közvetlenül a forrásból, és minden egyes kérelemmel a kérelmező számára továbbítja azokat.</span><span class="sxs-lookup"><span data-stu-id="810ac-116">The edge node retrieves the asset directly from the origin and passes it to the requestor with each request.</span></span>
* <span data-ttu-id="810ac-117">**Minden egyedi URL-cím gyorsítótárazása**: Ebben a módban a lekérdezési karakterlánc az egyes kérelmek kezeli a saját gyorsítótárával egyedi eszközként.</span><span class="sxs-lookup"><span data-stu-id="810ac-117">**Cache every unique URL**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="810ac-118">Például az eredeti adatforrást kérelmet kapott válasz *foo.ashx?q=bar* ehhez az élcsomóponthoz gyorsítótárazott és későbbi gyorsítótárak az, hogy ugyanazt a lekérdezési karakterláncot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="810ac-118">For example, the response from the origin for a request for *foo.ashx?q=bar* would be cached at the edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="810ac-119">A kérelem *foo.ashx?q=somethingelse* külön eszközként saját, amikor élő volna gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="810ac-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time to live.</span></span>

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a><span data-ttu-id="810ac-120">Lekérdezési karakterláncok gyorsítótárazásának standard szintű CDN-profil beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="810ac-120">Changing query string caching settings for standard CDN profiles</span></span>
1. <span data-ttu-id="810ac-121">A CDN-profil panelje kattintson a CDN-végpont felügyelni kíván.</span><span class="sxs-lookup"><span data-stu-id="810ac-121">From the CDN profile blade, click the CDN endpoint you wish to manage.</span></span>
   
    ![CDN-profil blade-végpontok](./media/cdn-query-string/cdn-endpoints.png)
   
    <span data-ttu-id="810ac-123">A CDN-végpont panelje megnyílik.</span><span class="sxs-lookup"><span data-stu-id="810ac-123">The CDN endpoint blade opens.</span></span>
2. <span data-ttu-id="810ac-124">Kattintson a **konfigurálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="810ac-124">Click the **Configure** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-query-string/cdn-config-btn.png)
   
    <span data-ttu-id="810ac-126">Ekkor megnyílik a CDN konfigurációs panel.</span><span class="sxs-lookup"><span data-stu-id="810ac-126">The CDN Configuration blade opens.</span></span>
3. <span data-ttu-id="810ac-127">A beállítás a **lekérdezési sztringek gyorsítótárazásának** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="810ac-127">Select a setting from the **Query string caching behavior** dropdown.</span></span>
   
    ![A CDN a lekérdezési karakterláncban a gyorsítótár](./media/cdn-query-string/cdn-query-string.png)
4. <span data-ttu-id="810ac-129">Miután választott, kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="810ac-129">After making your selection, click the **Save** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="810ac-130">A beállítások módosításait nem lehet azonnal láthatóak, mivel némi időre van a CDN propagálásához a regisztráció.</span><span class="sxs-lookup"><span data-stu-id="810ac-130">The settings changes may not be immediately visible, as it takes time for the registration to propagate through the CDN.</span></span>  <span data-ttu-id="810ac-131">Az <b>Akamai Azure CDN</b> típusú profilok propagálása általában egy percen belül befejeződik.</span><span class="sxs-lookup"><span data-stu-id="810ac-131">For <b>Azure CDN from Akamai</b> profiles, propagation will usually complete within one minute.</span></span>  <span data-ttu-id="810ac-132">A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="810ac-132">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

