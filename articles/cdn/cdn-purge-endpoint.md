---
title: "az Azure CDN-végpont aaaPurge |} Microsoft Docs"
description: "Ismerje meg, hogyan toopurge minden gyorsítótárazott tartalom az Azure CDN-végponton."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: a09f4a49aa1e2d7655ecae44b5126c11c28fd599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a><span data-ttu-id="defb5-103">Az Azure CDN-végpont törlése</span><span class="sxs-lookup"><span data-stu-id="defb5-103">Purge an Azure CDN endpoint</span></span>
## <a name="overview"></a><span data-ttu-id="defb5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="defb5-104">Overview</span></span>
<span data-ttu-id="defb5-105">Az Azure CDN peremhálózati csomópontok eszközök gyorsítótárazhatják az eléréséig hello eszköz idő-Élettartam (TTL).</span><span class="sxs-lookup"><span data-stu-id="defb5-105">Azure CDN edge nodes will cache assets until hello asset's time-to-live (TTL) expires.</span></span>  <span data-ttu-id="defb5-106">Hello eszköz TTL lejárata után Ha egy ügyfél hello eszköz kér hello élcsomópont, hello élcsomópont frissített másolatát hello eszköz tooserve hello ügyfélkérés lekéri és frissítési hello gyorsítótárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="defb5-106">After hello asset's TTL expires, when a client requests hello asset from hello edge node, hello edge node will retrieve a new updated copy of hello asset tooserve hello client request and store refresh hello cache.</span></span>

<span data-ttu-id="defb5-107">hello bevált gyakorlat toomake meg arról, hogy a felhasználók mindig hello legfrissebb verzióját a eszközöket szerezze be az eszközök, az egyes frissítése, és új URL-jeiként közzététele tooversion.</span><span class="sxs-lookup"><span data-stu-id="defb5-107">hello best practice toomake sure your users always obtain hello latest copy of your assets is tooversion your assets for each update and publish them as new URLs.</span></span>  <span data-ttu-id="defb5-108">CDN közvetlenül kér le új eszközök hello hello következő ügyfélkéréseket.</span><span class="sxs-lookup"><span data-stu-id="defb5-108">CDN will immediately retrieve hello new assets for hello next client requests.</span></span>  <span data-ttu-id="defb5-109">Egyes esetekben érdemes toopurge gyorsítótárba helyezték a tartalmat minden peremhálózati csomópontról és annak kikényszerítéséhez, azok összes tooretrieve új frissített eszközök.</span><span class="sxs-lookup"><span data-stu-id="defb5-109">Sometimes you may wish toopurge cached content from all edge nodes and force them all tooretrieve new updated assets.</span></span>  <span data-ttu-id="defb5-110">Előfordulhat, hogy ennek lehet tooupdates tooyour webalkalmazás, vagy helytelen adatokat tartalmazó tooquickly frissítési eszközök.</span><span class="sxs-lookup"><span data-stu-id="defb5-110">This might be due tooupdates tooyour web application, or tooquickly update assets that contain incorrect information.</span></span>

> [!TIP]
> <span data-ttu-id="defb5-111">Vegye figyelembe, hogy csak kiürítése törli hello gyorsítótárba helyezték a tartalmat a hello CDN peremhálózati kiszolgálóinak.</span><span class="sxs-lookup"><span data-stu-id="defb5-111">Note that purging only clears hello cached content on hello CDN edge servers.</span></span>  <span data-ttu-id="defb5-112">Bármely alárendelt gyorsítótárak, például proxykiszolgáló, és helyi böngésző gyorsítótárát, előfordulhat, hogy továbbra is gyorsítótárazott másolatának tárolására szolgál hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="defb5-112">Any downstream caches, such as proxy servers and local browser caches, may still hold a cached copy of hello file.</span></span>  <span data-ttu-id="defb5-113">Fontos tooremember a megadása után a fájl élettartamát idő.</span><span class="sxs-lookup"><span data-stu-id="defb5-113">It's important tooremember this when you set a file's time-to-live.</span></span>  <span data-ttu-id="defb5-114">Egy alárendelt ügyfél toorequest hello legújabb verzióját a fájl adjon neki egy egyedi nevet minden alkalommal, amikor végezzen frissítést, vagy ha kihasználja a kényszerítheti [lekérdezési karakterláncok gyorsítótárazása](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="defb5-114">You can force a downstream client toorequest hello latest version of your file by giving it a unique name every time you update it, or by taking advantage of [query string caching](cdn-query-string.md).</span></span>  
> 
> 

<span data-ttu-id="defb5-115">Ez az oktatóanyag bemutatja, hogyan eszközök kiürítése a végpont összes peremhálózati csomópontjából.</span><span class="sxs-lookup"><span data-stu-id="defb5-115">This tutorial walks you through purging assets from all edge nodes of an endpoint.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="defb5-116">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="defb5-116">Walkthrough</span></span>
1. <span data-ttu-id="defb5-117">A hello [Azure Portal](https://portal.azure.com), keresse meg a toopurge kívánja hello végpont tartalmazó toohello CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="defb5-117">In hello [Azure Portal](https://portal.azure.com), browse toohello CDN profile containing hello endpoint you wish toopurge.</span></span>
2. <span data-ttu-id="defb5-118">A CDN-profil panelje hello kattintson a hello végleges törlése gombra.</span><span class="sxs-lookup"><span data-stu-id="defb5-118">From hello CDN profile blade, click hello purge button.</span></span>
   
    ![CDN-profil panelje](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    <span data-ttu-id="defb5-120">hello kiürítése panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="defb5-120">hello Purge blade opens.</span></span>
   
    ![CDN-kiürítési panelje](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. <span data-ttu-id="defb5-122">A hello üríteni a panelen, hello szolgáltatás címét kívánja toopurge hello URL-cím legördülő listából válassza ki.</span><span class="sxs-lookup"><span data-stu-id="defb5-122">On hello Purge blade, select hello service address you wish toopurge from hello URL dropdown.</span></span>
   
    ![Űrlap kiürítése](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > <span data-ttu-id="defb5-124">Hello kattintva is megkapható toohello kiürítése panel **kiürítése** hello CDN-végpont panelje gombjára.</span><span class="sxs-lookup"><span data-stu-id="defb5-124">You can also get toohello Purge blade by clicking hello **Purge** button on hello CDN endpoint blade.</span></span>  <span data-ttu-id="defb5-125">Ebben az esetben hello **URL-cím** mező ki van töltve a hello szolgáltatást, hogy adott végpont címe lesz.</span><span class="sxs-lookup"><span data-stu-id="defb5-125">In that case, hello **URL** field will be pre-populated with hello service address of that specific endpoint.</span></span>
   > 
   > 
4. <span data-ttu-id="defb5-126">Válassza ki, milyen eszközöket kívánja a hello toopurge peremhálózati csomópontok.</span><span class="sxs-lookup"><span data-stu-id="defb5-126">Select what assets you wish toopurge from hello edge nodes.</span></span>  <span data-ttu-id="defb5-127">Ha tooclear az összes eszköz, kattintson hello **összes** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="defb5-127">If you wish tooclear all assets, click hello **Purge all** checkbox.</span></span>  <span data-ttu-id="defb5-128">Ellenkező esetben a típus hello elérési útjának minden egyes eszköz kívánja toopurge a hello **elérési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="defb5-128">Otherwise, type hello path of each asset you wish toopurge in hello **Path** textbox.</span></span> <span data-ttu-id="defb5-129">Hello elérési út által támogatott formátumok alatt.</span><span class="sxs-lookup"><span data-stu-id="defb5-129">Below formats are supported in hello path.</span></span>
    1. <span data-ttu-id="defb5-130">**Egyetlen URL-cím kiürítése**: hello teljes URL-címet, vagy anélkül hello fájlnévkiterjesztés, például megadásával eszköz egyedi kiürítése`/pictures/strasbourg.png`;`/pictures/strasbourg`</span><span class="sxs-lookup"><span data-stu-id="defb5-130">**Single URL purge**: Purge individual asset by specifying hello full URL, with or without hello file extension, e.g.,`/pictures/strasbourg.png`; `/pictures/strasbourg`</span></span>
    2. <span data-ttu-id="defb5-131">**Helyettesítő karakteres kiürítés**: csillag (\*) helyettesítő karakter is használható.</span><span class="sxs-lookup"><span data-stu-id="defb5-131">**Wildcard purge**: Asterisk (\*) may be used as a wildcard.</span></span> <span data-ttu-id="defb5-132">Törli az összes mappa, almappák és fájlok a végpont `/*` a hello elérési útját, vagy minden almappák és fájlok egy adott mappában törölhetők követ hello mappa megadásával `/*`, például`/pictures/*`.</span><span class="sxs-lookup"><span data-stu-id="defb5-132">Purge all folders, sub-folders and files under an endpoint with `/*` in hello path or purge all sub-folders and files under a specific folder by specifying hello folder followed by `/*`, e.g.,`/pictures/*`.</span></span>  <span data-ttu-id="defb5-133">Vegye figyelembe, hogy helyettesítő karakteres kiürítés nem támogatott Akamai Azure CDN által jelenleg.</span><span class="sxs-lookup"><span data-stu-id="defb5-133">Note that wildcard purge is not supported by Azure CDN from Akamai currently.</span></span> 
    3. <span data-ttu-id="defb5-134">**Legfelső szintű tartomány kiürítése**: kiürítése hello legfelső szintű hello végpont a "/" hello elérési úton.</span><span class="sxs-lookup"><span data-stu-id="defb5-134">**Root domain purge**: Purge hello root of hello endpoint with "/" in hello path.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="defb5-135">Elérési utak meg kell adni a kiürítési és kell lennie egy relatív URL-cím hello következő illeszkedő [reguláris kifejezés](https://msdn.microsoft.com/library/az24scfc.aspx).</span><span class="sxs-lookup"><span data-stu-id="defb5-135">Paths must be specified for purge and must be a relative URL that fit hello following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx).</span></span> <span data-ttu-id="defb5-136">**Összes** és **helyettesítő karakteres kiürítés** nem támogatja a **Akamai Azure CDN** jelenleg.</span><span class="sxs-lookup"><span data-stu-id="defb5-136">**Purge all** and **Wildcard purge** not supported by **Azure CDN from Akamai** currently.</span></span>
   > > <span data-ttu-id="defb5-137">Egyetlen URL-cím kiürítése`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span><span class="sxs-lookup"><span data-stu-id="defb5-137">Single URL purge `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span></span>  
   > > <span data-ttu-id="defb5-138">Lekérdezési karakterlánc`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="defb5-138">Query string `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span></span>  
   > > <span data-ttu-id="defb5-139">Helyettesítő karakteres kiürítés `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span><span class="sxs-lookup"><span data-stu-id="defb5-139">Wildcard purge `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span></span> 
   > 
   > <span data-ttu-id="defb5-140">További **elérési** szövegmezők megjelenik a szöveges tooallow megadása után toobuild több eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="defb5-140">More **Path** textboxes will appear after you enter text tooallow you toobuild a list of multiple assets.</span></span>  <span data-ttu-id="defb5-141">Eszközök hello listából hello három ponttal (…) gombra kattintva törölheti.</span><span class="sxs-lookup"><span data-stu-id="defb5-141">You can delete assets from hello list by clicking hello ellipsis (...) button.</span></span>
   > 
5. <span data-ttu-id="defb5-142">Kattintson a hello **kiürítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="defb5-142">Click hello **Purge** button.</span></span>
   
    ![Kiürítése gomb](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> <span data-ttu-id="defb5-144">Kiürítése kérelmek igénybe vehet a körülbelül 2-3 percet tooprocess **Azure CDN Verizon** (Standard és prémium), és körülbelül 7 perc **Akamai Azure CDN**.</span><span class="sxs-lookup"><span data-stu-id="defb5-144">Purge requests take approximately 2-3 minutes tooprocess with **Azure CDN from Verizon** (Standard and Premium), and approximately 7 minutes with **Azure CDN from Akamai**.</span></span>  <span data-ttu-id="defb5-145">Az Azure CDN rendelkezik maximális száma 50 egyidejű kérelmek kiüríteni egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="defb5-145">Azure CDN has a limit of 50 concurrent purge requests at any given time.</span></span> 
> 
> 

## <a name="see-also"></a><span data-ttu-id="defb5-146">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="defb5-146">See also</span></span>
* [<span data-ttu-id="defb5-147">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="defb5-147">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="defb5-148">Az Azure CDN REST API-referencia - kiürítése, vagy a végpont előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="defb5-148">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

