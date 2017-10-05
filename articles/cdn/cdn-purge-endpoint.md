---
title: "Az Azure CDN-végpont törlése |} Microsoft Docs"
description: "Ismerje meg, hogy minden gyorsítótárazott tartalom kiürítése az Azure CDN-végponton."
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
ms.openlocfilehash: b035c232bb58d653960190d4974cc3789d55a51d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="purge-an-azure-cdn-endpoint"></a><span data-ttu-id="8aadb-103">Az Azure CDN-végpont törlése</span><span class="sxs-lookup"><span data-stu-id="8aadb-103">Purge an Azure CDN endpoint</span></span>
## <a name="overview"></a><span data-ttu-id="8aadb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8aadb-104">Overview</span></span>
<span data-ttu-id="8aadb-105">Azure CDN peremhálózati csomópontok gyorsítótárazni fog eszközök, amíg az eszköz ideje-élettartam (TTL) lejár.</span><span class="sxs-lookup"><span data-stu-id="8aadb-105">Azure CDN edge nodes will cache assets until the asset's time-to-live (TTL) expires.</span></span>  <span data-ttu-id="8aadb-106">Az eszköz TTL lejárata után Ha egy ügyfél az eszköz kér az élcsomóponthoz, élcsomópont be fogja olvasni az eszköz az ügyfélkérés kiszolgálására frissített új példányát, és a tároló a gyorsítótár frissítése.</span><span class="sxs-lookup"><span data-stu-id="8aadb-106">After the asset's TTL expires, when a client requests the asset from the edge node, the edge node will retrieve a new updated copy of the asset to serve the client request and store refresh the cache.</span></span>

<span data-ttu-id="8aadb-107">Győződjön meg arról, hogy a felhasználók mindig a legfrissebb verzióját a eszközöket szerezze be az ajánlott eljárás az objektumok minden egyes frissítés verzióra, és új URL-címeket is közzé őket.</span><span class="sxs-lookup"><span data-stu-id="8aadb-107">The best practice to make sure your users always obtain the latest copy of your assets is to version your assets for each update and publish them as new URLs.</span></span>  <span data-ttu-id="8aadb-108">CDN azonnal be fogja olvasni az új eszközök a következő ügyfélkéréseket.</span><span class="sxs-lookup"><span data-stu-id="8aadb-108">CDN will immediately retrieve the new assets for the next client requests.</span></span>  <span data-ttu-id="8aadb-109">Egyes esetekben előfordulhat, hogy kívánja kiüríteni a gyorsítótárazott tartalom peremhálózati összes csomópontjának, és annak kikényszerítéséhez őket az összes új és frissített eszközök beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8aadb-109">Sometimes you may wish to purge cached content from all edge nodes and force them all to retrieve new updated assets.</span></span>  <span data-ttu-id="8aadb-110">Ennek oka a webes alkalmazás, illetve gyorsan helytelen adatokat tartalmazó frissítési eszközök frissítések lehet.</span><span class="sxs-lookup"><span data-stu-id="8aadb-110">This might be due to updates to your web application, or to quickly update assets that contain incorrect information.</span></span>

> [!TIP]
> <span data-ttu-id="8aadb-111">Vegye figyelembe, hogy csak kiürítése törli a gyorsítótárazott tartalmat a CDN peremhálózati kiszolgálóinak.</span><span class="sxs-lookup"><span data-stu-id="8aadb-111">Note that purging only clears the cached content on the CDN edge servers.</span></span>  <span data-ttu-id="8aadb-112">Bármely alárendelt gyorsítótárak, például proxykiszolgáló, és helyi böngésző gyorsítótárát, előfordulhat, hogy továbbra is a gyorsítótárazott másolatának tárolására szolgál a fájl.</span><span class="sxs-lookup"><span data-stu-id="8aadb-112">Any downstream caches, such as proxy servers and local browser caches, may still hold a cached copy of the file.</span></span>  <span data-ttu-id="8aadb-113">Fontos, ha úgy állítja be a fájl élettartamát idő figyelembe kell venni.</span><span class="sxs-lookup"><span data-stu-id="8aadb-113">It's important to remember this when you set a file's time-to-live.</span></span>  <span data-ttu-id="8aadb-114">Beállíthatja, hogy adjon neki egy egyedi nevet minden alkalommal, amikor végezzen frissítést, vagy ha kihasználja a kérést a fájl legújabb verzióját az alsóbb rétegbeli ügyfele [lekérdezési karakterláncok gyorsítótárazása](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="8aadb-114">You can force a downstream client to request the latest version of your file by giving it a unique name every time you update it, or by taking advantage of [query string caching](cdn-query-string.md).</span></span>  
> 
> 

<span data-ttu-id="8aadb-115">Ez az oktatóanyag bemutatja, hogyan eszközök kiürítése a végpont összes peremhálózati csomópontjából.</span><span class="sxs-lookup"><span data-stu-id="8aadb-115">This tutorial walks you through purging assets from all edge nodes of an endpoint.</span></span>

## <a name="walkthrough"></a><span data-ttu-id="8aadb-116">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="8aadb-116">Walkthrough</span></span>
1. <span data-ttu-id="8aadb-117">Az a [Azure Portal](https://portal.azure.com), keresse meg a CDN-profilt, amely tartalmazza a kiüríteni kívánt végpont.</span><span class="sxs-lookup"><span data-stu-id="8aadb-117">In the [Azure Portal](https://portal.azure.com), browse to the CDN profile containing the endpoint you wish to purge.</span></span>
2. <span data-ttu-id="8aadb-118">A CDN-profil panelje a végleges törlése gombra.</span><span class="sxs-lookup"><span data-stu-id="8aadb-118">From the CDN profile blade, click the purge button.</span></span>
   
    ![CDN-profil panelje](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    <span data-ttu-id="8aadb-120">Ekkor megnyílik a kiürítési panel.</span><span class="sxs-lookup"><span data-stu-id="8aadb-120">The Purge blade opens.</span></span>
   
    ![CDN-kiürítési panelje](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. <span data-ttu-id="8aadb-122">A kiürítési panelen válassza ki a szolgáltatás címet kívánja kiüríteni a URL-cím legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="8aadb-122">On the Purge blade, select the service address you wish to purge from the URL dropdown.</span></span>
   
    ![Űrlap kiürítése](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > <span data-ttu-id="8aadb-124">Letöltheti a kiürítési paneljére kattint a **kiürítése** a CDN-végpont panelje gombjára.</span><span class="sxs-lookup"><span data-stu-id="8aadb-124">You can also get to the Purge blade by clicking the **Purge** button on the CDN endpoint blade.</span></span>  <span data-ttu-id="8aadb-125">Ebben az esetben a **URL-cím** mező ki van töltve, hogy az adott végponti szolgáltatás címével lesz.</span><span class="sxs-lookup"><span data-stu-id="8aadb-125">In that case, the **URL** field will be pre-populated with the service address of that specific endpoint.</span></span>
   > 
   > 
4. <span data-ttu-id="8aadb-126">Válassza ki milyen eszközöket kívánja kiüríteni peremhálózati csomópontjából.</span><span class="sxs-lookup"><span data-stu-id="8aadb-126">Select what assets you wish to purge from the edge nodes.</span></span>  <span data-ttu-id="8aadb-127">Ha törli az összes eszközt, kattintson a **összes** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8aadb-127">If you wish to clear all assets, click the **Purge all** checkbox.</span></span>  <span data-ttu-id="8aadb-128">Ellenkező esetben adja meg az elérési útját minden egyes a kiüríteni kívánt eszközt a **elérési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8aadb-128">Otherwise, type the path of each asset you wish to purge in the **Path** textbox.</span></span> <span data-ttu-id="8aadb-129">Az elérési út által támogatott formátumok alatt.</span><span class="sxs-lookup"><span data-stu-id="8aadb-129">Below formats are supported in the path.</span></span>
    1. <span data-ttu-id="8aadb-130">**Egyetlen URL-cím kiürítése**: a teljes URL-címet, vagy a fájl kiterjesztése például anélkül megadásával eszköz egyedi kiürítése`/pictures/strasbourg.png`;`/pictures/strasbourg`</span><span class="sxs-lookup"><span data-stu-id="8aadb-130">**Single URL purge**: Purge individual asset by specifying the full URL, with or without the file extension, e.g.,`/pictures/strasbourg.png`; `/pictures/strasbourg`</span></span>
    2. <span data-ttu-id="8aadb-131">**Helyettesítő karakteres kiürítés**: csillag (\*) helyettesítő karakter is használható.</span><span class="sxs-lookup"><span data-stu-id="8aadb-131">**Wildcard purge**: Asterisk (\*) may be used as a wildcard.</span></span> <span data-ttu-id="8aadb-132">Törli az összes mappa, almappák és fájlok a végpont `/*` kiürítése vagy az elérési utat minden almappák és fájlok egy adott mappában a mappa megadásával követ `/*`, például`/pictures/*`.</span><span class="sxs-lookup"><span data-stu-id="8aadb-132">Purge all folders, sub-folders and files under an endpoint with `/*` in the path or purge all sub-folders and files under a specific folder by specifying the folder followed by `/*`, e.g.,`/pictures/*`.</span></span>  <span data-ttu-id="8aadb-133">Vegye figyelembe, hogy helyettesítő karakteres kiürítés nem támogatott Akamai Azure CDN által jelenleg.</span><span class="sxs-lookup"><span data-stu-id="8aadb-133">Note that wildcard purge is not supported by Azure CDN from Akamai currently.</span></span> 
    3. <span data-ttu-id="8aadb-134">**Legfelső szintű tartomány kiürítése**: törli a végpont a "/" elérési gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="8aadb-134">**Root domain purge**: Purge the root of the endpoint with "/" in the path.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="8aadb-135">Elérési utak meg kell adni a kiürítési, és egy relatív URL-cím a következő illeszkedő kell [reguláris kifejezés](https://msdn.microsoft.com/library/az24scfc.aspx).</span><span class="sxs-lookup"><span data-stu-id="8aadb-135">Paths must be specified for purge and must be a relative URL that fit the following [regular expression](https://msdn.microsoft.com/library/az24scfc.aspx).</span></span> <span data-ttu-id="8aadb-136">**Összes** és **helyettesítő karakteres kiürítés** nem támogatja a **Akamai Azure CDN** jelenleg.</span><span class="sxs-lookup"><span data-stu-id="8aadb-136">**Purge all** and **Wildcard purge** not supported by **Azure CDN from Akamai** currently.</span></span>
   > > <span data-ttu-id="8aadb-137">Egyetlen URL-cím kiürítése`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span><span class="sxs-lookup"><span data-stu-id="8aadb-137">Single URL purge `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`</span></span>  
   > > <span data-ttu-id="8aadb-138">Lekérdezési karakterlánc`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span><span class="sxs-lookup"><span data-stu-id="8aadb-138">Query string `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`</span></span>  
   > > <span data-ttu-id="8aadb-139">Helyettesítő karakteres kiürítés `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span><span class="sxs-lookup"><span data-stu-id="8aadb-139">Wildcard purge `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`.</span></span> 
   > 
   > <span data-ttu-id="8aadb-140">További **elérési** szövegmezők lehetővé teszi több eszközök listájának összeállítása szöveg megadása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8aadb-140">More **Path** textboxes will appear after you enter text to allow you to build a list of multiple assets.</span></span>  <span data-ttu-id="8aadb-141">Eszközök a három ponttal (…) gombra kattintva törölheti a listából.</span><span class="sxs-lookup"><span data-stu-id="8aadb-141">You can delete assets from the list by clicking the ellipsis (...) button.</span></span>
   > 
5. <span data-ttu-id="8aadb-142">Kattintson a **kiürítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8aadb-142">Click the **Purge** button.</span></span>
   
    ![Kiürítése gomb](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> <span data-ttu-id="8aadb-144">Kiürítési kérelmeket feldolgozni a körülbelül 2-3 percet vehet igénybe **Azure CDN Verizon** (Standard és prémium), és körülbelül 7 perc **Akamai Azure CDN**.</span><span class="sxs-lookup"><span data-stu-id="8aadb-144">Purge requests take approximately 2-3 minutes to process with **Azure CDN from Verizon** (Standard and Premium), and approximately 7 minutes with **Azure CDN from Akamai**.</span></span>  <span data-ttu-id="8aadb-145">Az Azure CDN rendelkezik maximális száma 50 egyidejű kérelmek kiüríteni egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="8aadb-145">Azure CDN has a limit of 50 concurrent purge requests at any given time.</span></span> 
> 
> 

## <a name="see-also"></a><span data-ttu-id="8aadb-146">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8aadb-146">See also</span></span>
* [<span data-ttu-id="8aadb-147">Eszközök előzetes betöltése Azure CDN-végponton</span><span class="sxs-lookup"><span data-stu-id="8aadb-147">Pre-load assets on an Azure CDN endpoint</span></span>](cdn-preload-endpoint.md)
* [<span data-ttu-id="8aadb-148">Az Azure CDN REST API-referencia - kiürítése, vagy a végpont előzetes betöltése</span><span class="sxs-lookup"><span data-stu-id="8aadb-148">Azure CDN REST API reference - Purge or Pre-Load an Endpoint</span></span>](https://msdn.microsoft.com/library/mt634451.aspx)

