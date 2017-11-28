---
title: "lekérdezési karakterláncok - prémium szintű Azure CDN gyorsítótárazási működését aaaControl |} Microsoft Docs"
description: "Az Azure CDN lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő."
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
ms.openlocfilehash: 5c97cf0230ae13fd378bfce49481f3135a5ef101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium"></a><span data-ttu-id="48533-103">Vezérlő Azure CDN a lekérdezési karakterláncok - prémium gyorsítótárazásának</span><span class="sxs-lookup"><span data-stu-id="48533-103">Control Azure CDN caching behavior with query strings - Premium</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48533-104">Standard</span><span class="sxs-lookup"><span data-stu-id="48533-104">Standard</span></span>](cdn-query-string.md)
> * [<span data-ttu-id="48533-105">Verizon Azure CDN Premium</span><span class="sxs-lookup"><span data-stu-id="48533-105">Azure CDN Premium from Verizon</span></span>](cdn-query-string-premium.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="48533-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="48533-106">Overview</span></span>
<span data-ttu-id="48533-107">Lekérdezési karakterláncok gyorsítótárazásának hogyan fájljai toobe gyorsítótárazza a lekérdezési karakterláncokat tartalmazó vezérlő.</span><span class="sxs-lookup"><span data-stu-id="48533-107">Query string caching controls how files are toobe cached when they contain query strings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48533-108">hello Standard és prémium szintű CDN-termékek rendelkeznek hello azonos lekérdezési karakterlánc-gyorsítótár működése, de hello felhasználói felület eltér.</span><span class="sxs-lookup"><span data-stu-id="48533-108">hello Standard and Premium CDN products provide hello same query string caching functionality, but hello user interface differs.</span></span>  <span data-ttu-id="48533-109">Ez a dokumentum ismerteti a hello felület **verizon Azure CDN Premium**.</span><span class="sxs-lookup"><span data-stu-id="48533-109">This document describes hello interface for **Azure CDN Premium from Verizon**.</span></span>  <span data-ttu-id="48533-110">A lekérdezési karakterláncok gyorsítótárazása a **Azure CDN Standard Akamai** és **Azure CDN Standard verizon**, lásd: [lekérdezési karakterláncokat tartalmazó kérelmek gyorsítótárazási CDN viselkedésének vezérlése](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="48533-110">For query string caching with **Azure CDN Standard from Akamai** and **Azure CDN Standard from Verizon**, see [Controlling caching behavior of CDN requests with query strings](cdn-query-string.md).</span></span>
> 
> 

<span data-ttu-id="48533-111">Három módszer áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="48533-111">Three modes are available:</span></span>

* <span data-ttu-id="48533-112">**Standard-gyorsítótár**: Ez az alapértelmezett mód hello.</span><span class="sxs-lookup"><span data-stu-id="48533-112">**standard-cache**:  This is hello default mode.</span></span>  <span data-ttu-id="48533-113">hello CDN élcsomópont továbbítják hello lekérdezési karakterlánc hello kérelmező toohello forrásból hello első kérelem és a gyorsítótár hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="48533-113">hello CDN edge node will pass hello query string from hello requestor toohello origin on hello first request and cache hello asset.</span></span>  <span data-ttu-id="48533-114">Az összes további kérelmet, hogy az eszköz a hello élcsomópont szolgáltatott figyelmen kívül hagyja majd hello lekérdezési karakterlánc eléréséig hello gyorsítótárazott eszköz.</span><span class="sxs-lookup"><span data-stu-id="48533-114">All subsequent requests for that asset that are served from hello edge node will ignore hello query string until hello cached asset expires.</span></span>
* <span data-ttu-id="48533-115">**no-cache**: Ebben a módban a lekérdezési karakterláncot tartalmazó kérelmek nem gyorsítótárazzák a hello CDN peremhálózati csomópontban.</span><span class="sxs-lookup"><span data-stu-id="48533-115">**no-cache**:  In this mode, requests with query strings are not cached at hello CDN edge node.</span></span>  <span data-ttu-id="48533-116">hello élcsomópont hello eszköz lekérdezi közvetlenül hello forrásból, és átadja toohello kérelmező minden egyes kérelemmel.</span><span class="sxs-lookup"><span data-stu-id="48533-116">hello edge node retrieves hello asset directly from hello origin and passes it toohello requestor with each request.</span></span>
* <span data-ttu-id="48533-117">**egyedi gyorsítótár**: Ebben a módban a lekérdezési karakterlánc az egyes kérelmek kezeli a saját gyorsítótárával egyedi eszközként.</span><span class="sxs-lookup"><span data-stu-id="48533-117">**unique-cache**:  This mode treats each request with a query string as a unique asset with its own cache.</span></span>  <span data-ttu-id="48533-118">Például a hello eredeti adatforrást kérelmet kapott válasz hello *foo.ashx?q=bar* ehhez hello élcsomópont gyorsítótárazott és későbbi gyorsítótárak az, hogy ugyanazt a lekérdezési karakterláncot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="48533-118">For example, hello response from hello origin for a request for *foo.ashx?q=bar* would be cached at hello edge node and returned for subsequent caches with that same query string.</span></span>  <span data-ttu-id="48533-119">A kérelem *foo.ashx?q=somethingelse* volna a saját toolive idővel külön eszközként gyorsítótárazható.</span><span class="sxs-lookup"><span data-stu-id="48533-119">A request for *foo.ashx?q=somethingelse* would be cached as a separate asset with its own time toolive.</span></span>

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a><span data-ttu-id="48533-120">Lekérdezési karakterláncok gyorsítótárazásának prémium szintű CDN-profil beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="48533-120">Changing query string caching settings for premium CDN profiles</span></span>
1. <span data-ttu-id="48533-121">A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="48533-121">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    <span data-ttu-id="48533-123">Megnyílik a hello CDN felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="48533-123">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="48533-124">Hello az egérmutatót **HTTP nagy** fülre, majd az egérmutatót hello **gyorsítótár beállításainak** menü.</span><span class="sxs-lookup"><span data-stu-id="48533-124">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="48533-125">Kattintson a **lekérdezési karakterlánc-gyorsítótár**.</span><span class="sxs-lookup"><span data-stu-id="48533-125">Click on **Query-String Caching**.</span></span>
   
    <span data-ttu-id="48533-126">Lekérdezési karakterlánc gyorsítótárazási beállítások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="48533-126">Query string caching options are displayed.</span></span>
   
    ![A CDN a lekérdezési karakterláncban a gyorsítótár](./media/cdn-query-string-premium/cdn-query-string.png)
3. <span data-ttu-id="48533-128">Miután választott, kattintson a hello **frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="48533-128">After making your selection, click hello **Update** button.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48533-129">hello módosításait nem lehet azonnal láthatóak, mivel némi időre van szükség hello regisztrációs toopropagate hello CDN keresztül.</span><span class="sxs-lookup"><span data-stu-id="48533-129">hello settings changes may not be immediately visible, as it takes time for hello registration toopropagate through hello CDN.</span></span>  <span data-ttu-id="48533-130">A <b>Verizon Azure CDN</b> típusú profilok propagálása általában 90 percen belül befejeződik, ám egyes esetekben több időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="48533-130">For <b>Azure CDN from Verizon</b> profiles, propagation will usually complete within 90 minutes, but in some cases can take longer.</span></span>
> 
> 

