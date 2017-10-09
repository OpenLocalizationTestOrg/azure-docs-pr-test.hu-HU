---
title: "Azure CDN-végpont aaaPre terhelési eszközök |} Microsoft Docs"
description: "Ismerje meg, hogyan toopre terhelésű gyorsítótárazott tartalom Azure CDN-végpont."
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
ms.openlocfilehash: 08ac4b834f1ac8ce59d22e65fa8adea11bafcf17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a><span data-ttu-id="f3106-103">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="f3106-103">Pre-load assets on an Azure CDN endpoint</span></span>
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="f3106-104">Alapértelmezés szerint eszközök először gyorsítótárba kerüljenek-e, erre felkérést kapnak.</span><span class="sxs-lookup"><span data-stu-id="f3106-104">By default, assets are first cached as they are requested.</span></span> <span data-ttu-id="f3106-105">Ez azt jelenti, hogy minden egyes régió hello első kérelmet hosszabb időt vehet igénybe, mivel hello peremhálózati kiszolgálóinak nem kell hello tartalom gyorsítótárazva, és tooforward hello kérelem toohello eredeti kiszolgálóra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="f3106-105">This means that hello first request from each region may take longer, since hello edge servers will not have hello content cached and will need tooforward hello request toohello origin server.</span></span> <span data-ttu-id="f3106-106">Előre a tartalom betöltése elkerülhető az első találati késés.</span><span class="sxs-lookup"><span data-stu-id="f3106-106">Pre-loading content avoids this first hit latency.</span></span>

<span data-ttu-id="f3106-107">Továbbá tooproviding jobb felhasználói élmény, a gyorsítótárazott eszközök előzetes betöltése hálózati forgalmat a hello eredeti kiszolgálóra is csökkentheti.</span><span class="sxs-lookup"><span data-stu-id="f3106-107">In addition tooproviding a better customer experience, pre-loading your cached assets can also reduce network traffic on hello origin server.</span></span>

> [!NOTE]
> <span data-ttu-id="f3106-108">Előzetes betöltését az eszközök akkor hasznos, ha nagy események vagy tartalom, amely elérhetővé válik egy időben elérhető tooa sok felhasználó, például egy új movie kiadás vagy szoftverfrissítés.</span><span class="sxs-lookup"><span data-stu-id="f3106-108">Pre-loading assets is useful for  large events or content that becomes simultaneously available tooa large number of users, such as a new movie release or a software update.</span></span>
> 
> 

<span data-ttu-id="f3106-109">Ez az oktatóanyag bemutatja, hogyan előzetes betöltését a gyorsítótárazott tartalom minden Azure CDN peremhálózati csomópontján.</span><span class="sxs-lookup"><span data-stu-id="f3106-109">This tutorial walks you through pre-loading cached content on all Azure CDN edge nodes.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="f3106-110">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="f3106-110">Walkthrough</span></span>
1. <span data-ttu-id="f3106-111">A hello [Azure Portal](https://portal.azure.com), keresse meg a terhelési toopre kívánja hello végpont tartalmazó toohello CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="f3106-111">In hello [Azure Portal](https://portal.azure.com), browse toohello CDN profile containing hello endpoint you wish toopre-load.</span></span>  <span data-ttu-id="f3106-112">hello-profil panelje megnyílik.</span><span class="sxs-lookup"><span data-stu-id="f3106-112">hello profile blade opens.</span></span>
2. <span data-ttu-id="f3106-113">Kattintson a hello végpont hello listában.</span><span class="sxs-lookup"><span data-stu-id="f3106-113">Click hello endpoint in hello list.</span></span>  <span data-ttu-id="f3106-114">hello végpont panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f3106-114">hello endpoint blade opens.</span></span>
3. <span data-ttu-id="f3106-115">Hello CDN-végpont panelen kattintson a hello Betöltés gombra.</span><span class="sxs-lookup"><span data-stu-id="f3106-115">From hello CDN endpoint blade, click hello load button.</span></span>
   
    ![CDN-végpont panelje](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    <span data-ttu-id="f3106-117">hello terhelés panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f3106-117">hello Load blade opens.</span></span>
   
    ![CDN-betöltési panelje](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. <span data-ttu-id="f3106-119">Hello elérési útját adja meg minden egyes eszköz tooload kívánja (pl. `/pictures/kitten.png`) a hello **elérési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="f3106-119">Enter hello full path of each asset you wish tooload (e.g., `/pictures/kitten.png`) in hello **Path** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="f3106-120">További **elérési** szövegmezők megjelenik a szöveges tooallow megadása után toobuild több eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="f3106-120">More **Path** textboxes will appear after you enter text tooallow you toobuild a list of multiple assets.</span></span>  <span data-ttu-id="f3106-121">Eszközök hello listából hello három ponttal (…) gombra kattintva törölheti.</span><span class="sxs-lookup"><span data-stu-id="f3106-121">You can delete assets from hello list by clicking hello ellipsis (...) button.</span></span>
   > 
   > <span data-ttu-id="f3106-122">Elérési út lehet egy relatív URL-címet, amely megfelel a következő hello [reguláris kifejezés](https://msdn.microsoft.com/library/az24scfc.aspx):</span><span class="sxs-lookup"><span data-stu-id="f3106-122">Paths must be a relative URL that fits hello following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx):</span></span>  
   > ><span data-ttu-id="f3106-123">Egy egyetlen fájl elérési útját betöltése `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span><span class="sxs-lookup"><span data-stu-id="f3106-123">Load a single file path `@"^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$"`;</span></span>  
   > ><span data-ttu-id="f3106-124">A lekérdezési karakterlánc egyetlen fájl betöltése`@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="f3106-124">Load a single file with query string `@"^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$";`</span></span>  
   > 
   > <span data-ttu-id="f3106-125">Minden eszköz a saját elérési utat kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f3106-125">Each asset must have its own path.</span></span>  <span data-ttu-id="f3106-126">Nincs előre betöltése eszközök helyettesítő funkció sem.</span><span class="sxs-lookup"><span data-stu-id="f3106-126">There is no wildcard functionality for pre-loading assets.</span></span>
   > 
   > 
   
    ![Betöltési gomb](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. <span data-ttu-id="f3106-128">Kattintson a hello **terhelés** gombra.</span><span class="sxs-lookup"><span data-stu-id="f3106-128">Click hello **Load** button.</span></span>
   
    ![Betöltési gomb](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> <span data-ttu-id="f3106-130">CDN-profil percenként 10 vonatkozó korlátozása van.</span><span class="sxs-lookup"><span data-stu-id="f3106-130">There is a limitation of 10 load requests per minute per CDN profile.</span></span> <span data-ttu-id="f3106-131">50 elérési utat kérelmenként engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="f3106-131">50 paths are allowed per request.</span></span> <span data-ttu-id="f3106-132">Mindegyik elérési útból útvonal-hossza legfeljebb 1024 karakterből állhat.</span><span class="sxs-lookup"><span data-stu-id="f3106-132">Each path has a path-length limit of 1024 characters.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="f3106-133">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f3106-133">See also</span></span>
* [<span data-ttu-id="f3106-134">Az Azure CDN-végpont törlése</span><span class="sxs-lookup"><span data-stu-id="f3106-134">Purge an Azure CDN endpoint</span></span>](cdn-purge-endpoint.md)
* [<span data-ttu-id="f3106-135">Az Azure CDN REST API-referencia - kiürítése, vagy a végpont előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="f3106-135">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

