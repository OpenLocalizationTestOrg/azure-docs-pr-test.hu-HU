---
title: "Az Azure CDN-végpont eszközök előzetes betöltése |} Microsoft Docs"
description: "Megtudhatja, hogyan előzetes betöltése Azure CDN-végpont gyorsítótárazott tartalmat."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1f2dcd9a91bb6e883cbef06373c1acd98bf8d45f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a><span data-ttu-id="edb10-103">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="edb10-103">Pre-load assets on an Azure CDN endpoint</span></span>
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="edb10-104">Alapértelmezés szerint eszközök először gyorsítótárba kerüljenek-e, erre felkérést kapnak.</span><span class="sxs-lookup"><span data-stu-id="edb10-104">By default, assets are first cached as they are requested.</span></span> <span data-ttu-id="edb10-105">Ez azt jelenti, hogy az első kérésre minden régióban hosszabb időt vehet igénybe, mivel a peremhálózati kiszolgálóinak nem kell a tartalom gyorsítótárazva, és továbbítja a kérelmet az eredeti kiszolgálóra kell.</span><span class="sxs-lookup"><span data-stu-id="edb10-105">This means that the first request from each region may take longer, since the edge servers will not have the content cached and will need to forward the request to the origin server.</span></span> <span data-ttu-id="edb10-106">Előre a tartalom betöltése elkerülhető az első találati késés.</span><span class="sxs-lookup"><span data-stu-id="edb10-106">Pre-loading content avoids this first hit latency.</span></span>

<span data-ttu-id="edb10-107">Mellett jobb felhasználói élményt biztosít, a gyorsítótárazott eszközök előzetes betöltése is csökkentheti a forráskiszolgáló hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="edb10-107">In addition to providing a better customer experience, pre-loading your cached assets can also reduce network traffic on the origin server.</span></span>

> [!NOTE]
> <span data-ttu-id="edb10-108">Eszközök előzetes betöltése esetén hasznos nagy események vagy a tartalmat, hogy a felhasználók számára, például egy új movie kiadás vagy szoftverfrissítés nagy számú egyidejűleg elérhetővé válnak.</span><span class="sxs-lookup"><span data-stu-id="edb10-108">Pre-loading assets is useful for  large events or content that becomes simultaneously available to a large number of users, such as a new movie release or a software update.</span></span>
> 
> 

<span data-ttu-id="edb10-109">Ez az oktatóanyag bemutatja, hogyan előzetes betöltését a gyorsítótárazott tartalom minden Azure CDN peremhálózati csomópontján.</span><span class="sxs-lookup"><span data-stu-id="edb10-109">This tutorial walks you through pre-loading cached content on all Azure CDN edge nodes.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="edb10-110">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="edb10-110">Walkthrough</span></span>
1. <span data-ttu-id="edb10-111">Az a [Azure Portal](https://portal.azure.com), keresse meg a CDN-profilt, amely tartalmazza a előzetes betöltése kívánt végpont.</span><span class="sxs-lookup"><span data-stu-id="edb10-111">In the [Azure Portal](https://portal.azure.com), browse to the CDN profile containing the endpoint you wish to pre-load.</span></span>  <span data-ttu-id="edb10-112">A profil panelje megnyílik.</span><span class="sxs-lookup"><span data-stu-id="edb10-112">The profile blade opens.</span></span>
2. <span data-ttu-id="edb10-113">Kattintson a végpont a listában.</span><span class="sxs-lookup"><span data-stu-id="edb10-113">Click the endpoint in the list.</span></span>  <span data-ttu-id="edb10-114">A végpont panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="edb10-114">The endpoint blade opens.</span></span>
3. <span data-ttu-id="edb10-115">A CDN-végpont paneljéről a terhelés gombra.</span><span class="sxs-lookup"><span data-stu-id="edb10-115">From the CDN endpoint blade, click the load button.</span></span>
   
    ![CDN-végpont panelje](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    <span data-ttu-id="edb10-117">Ekkor megnyílik a terhelés panel.</span><span class="sxs-lookup"><span data-stu-id="edb10-117">The Load blade opens.</span></span>
   
    ![CDN-betöltési panelje](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. <span data-ttu-id="edb10-119">Írja be a betölteni kívánt minden egyes eszköz elérési útját (például `/pictures/kitten.png`) található a **elérési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="edb10-119">Enter the full path of each asset you wish to load (e.g., `/pictures/kitten.png`) in the **Path** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="edb10-120">További **elérési** szövegmezők lehetővé teszi több eszközök listájának összeállítása szöveg megadása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="edb10-120">More **Path** textboxes will appear after you enter text to allow you to build a list of multiple assets.</span></span>  <span data-ttu-id="edb10-121">Eszközök a három ponttal (…) gombra kattintva törölheti a listából.</span><span class="sxs-lookup"><span data-stu-id="edb10-121">You can delete assets from the list by clicking the ellipsis (...) button.</span></span>
   > 
   > <span data-ttu-id="edb10-122">Elérési út lehet egy relatív URL-címet, amely megfelel a következő [reguláris kifejezés](https://msdn.microsoft.com/library/az24scfc.aspx):</span><span class="sxs-lookup"><span data-stu-id="edb10-122">Paths must be a relative URL that fits the following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx):</span></span>  
   > ><span data-ttu-id="edb10-123">Egy egyetlen fájl elérési útját betöltése `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span><span class="sxs-lookup"><span data-stu-id="edb10-123">Load a single file path `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span></span>  
   > ><span data-ttu-id="edb10-124">A lekérdezési karakterlánc egyetlen fájl betöltése`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="edb10-124">Load a single file with query string `@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span></span>  
   > 
   > <span data-ttu-id="edb10-125">Minden eszköz a saját elérési utat kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="edb10-125">Each asset must have its own path.</span></span>  <span data-ttu-id="edb10-126">Nincs előre betöltése eszközök helyettesítő funkció sem.</span><span class="sxs-lookup"><span data-stu-id="edb10-126">There is no wildcard functionality for pre-loading assets.</span></span>
   > 
   > 
   
    ![Betöltési gomb](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. <span data-ttu-id="edb10-128">Kattintson a **terhelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="edb10-128">Click the **Load** button.</span></span>
   
    ![Betöltési gomb](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> <span data-ttu-id="edb10-130">CDN-profil percenként 10 vonatkozó korlátozása van.</span><span class="sxs-lookup"><span data-stu-id="edb10-130">There is a limitation of 10 load requests per minute per CDN profile.</span></span> <span data-ttu-id="edb10-131">50 elérési utat kérelmenként engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="edb10-131">50 paths are allowed per request.</span></span> <span data-ttu-id="edb10-132">Mindegyik elérési útból útvonal-hossza legfeljebb 1024 karakterből állhat.</span><span class="sxs-lookup"><span data-stu-id="edb10-132">Each path has a path-length limit of 1024 characters.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="edb10-133">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="edb10-133">See also</span></span>
* [<span data-ttu-id="edb10-134">Az Azure CDN-végpont törlése</span><span class="sxs-lookup"><span data-stu-id="edb10-134">Purge an Azure CDN endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="edb10-135">Az Azure CDN REST API-referencia - kiürítése, vagy a végpont előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="edb10-135">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

