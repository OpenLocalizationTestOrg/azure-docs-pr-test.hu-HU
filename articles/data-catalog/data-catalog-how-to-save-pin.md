---
title: "aaaSave keres, és a PIN-kód adategységeket az Azure Data Catalog |} Microsoft Docs"
description: "Hogyan-tooarticle kijelölő képességek az Azure Data Catalog adatforrások és a későbbi használatra adategységeket mentéséhez."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0ad0a31d4b84782fed9d80acc2734912eecd6d74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a><span data-ttu-id="5280e-103">Az Azure Data Catalog keres, és a PIN-kód adategységeket mentése</span><span class="sxs-lookup"><span data-stu-id="5280e-103">Save searches and pin data assets in Azure Data Catalog</span></span>
## <a name="introduction"></a><span data-ttu-id="5280e-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5280e-104">Introduction</span></span>
<span data-ttu-id="5280e-105">Az Azure Data Catalog az adatforrás-felderítés képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="5280e-105">Azure Data Catalog provides capabilities for data source discovery.</span></span> <span data-ttu-id="5280e-106">Gyorsan megkeresheti és hello katalógus toolocate adatforrások szűrése és megértése azok használatának célját, így könnyebben toofind hello megfelelő adatok az elvégzendő hello feladat.</span><span class="sxs-lookup"><span data-stu-id="5280e-106">You can quickly search and filter hello catalog toolocate data sources and understand their intended purpose, making it easier toofind hello right data for hello job at hand.</span></span>

<span data-ttu-id="5280e-107">De mi történik, ha szüksége tooregularly hello használata azonos adatokat?</span><span class="sxs-lookup"><span data-stu-id="5280e-107">But what if you need tooregularly work with hello same data?</span></span> <span data-ttu-id="5280e-108">Mi történik, ha más felhasználók rendszeresen hozzájárulhatnak a Tudásbázis toohello és azonos adatforrások hello katalógusban?</span><span class="sxs-lookup"><span data-stu-id="5280e-108">And what if you and other users regularly contribute your knowledge toohello same data sources in hello catalog?</span></span> <span data-ttu-id="5280e-109">Ezekben a helyzetekben, amelyek toorepeatedly probléma hello azonos lehet, hogy a keresés nem hatékony.</span><span class="sxs-lookup"><span data-stu-id="5280e-109">In these situations, having toorepeatedly issue hello same searches can be inefficient.</span></span> <span data-ttu-id="5280e-110">Ez azért, ahol mentett keresés és a rögzített adatok eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="5280e-110">This is where saved search and pinned data assets can help.</span></span>

## <a name="saved-searches"></a><span data-ttu-id="5280e-111">Mentett keresések</span><span class="sxs-lookup"><span data-stu-id="5280e-111">Saved searches</span></span>
<span data-ttu-id="5280e-112">A mentett kereséseket a Data Catalog az újrafelhasználható, felhasználókon keresési definícióját.</span><span class="sxs-lookup"><span data-stu-id="5280e-112">A saved search in Data Catalog is a reusable, per-user search definition.</span></span> <span data-ttu-id="5280e-113">Adja meg a keresés – többek között a keresési feltételek, a címkék és a többi szűrőt, és mentse.</span><span class="sxs-lookup"><span data-stu-id="5280e-113">You can define a search, including search terms, tags, and other filters, and then save it.</span></span> <span data-ttu-id="5280e-114">Újbóli futtatásával hello mentett keresés definition újabb tooreturn bármely az adategységekhez a keresési feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="5280e-114">You can re-run hello saved search definition later tooreturn any data assets that match its search criteria.</span></span>

### <a name="create-a-saved-search"></a><span data-ttu-id="5280e-115">A mentett keresés létrehozása</span><span class="sxs-lookup"><span data-stu-id="5280e-115">Create a saved search</span></span>
<span data-ttu-id="5280e-116">a mentett kereséseket toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="5280e-116">toocreate a saved search, do hello following:</span></span>
1. <span data-ttu-id="5280e-117">Hello Azure Data Catalog portálon, a hello **aktuális keresés** ablak, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5280e-117">In hello Azure Data Catalog portal, in hello **Current Search** window, click **Save**.</span></span> 

    ![Aktuális keresési beállítások mentése hivatkozás](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. <span data-ttu-id="5280e-119">Adja meg a hello keresési feltételeket, hogy szeretné, hogy tooreuse, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5280e-119">Enter hello search criteria that you want tooreuse, and then click **Save**.</span></span>

    ![Aktuális keresési beállítások mentett keresési neve](./media/data-catalog-how-to-save-pin/02-name.png)

3. <span data-ttu-id="5280e-121">Amikor a rendszer kéri, adja meg a mentett keresés hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5280e-121">When you are prompted, enter a name for hello saved search.</span></span> <span data-ttu-id="5280e-122">Adjon meg egy megfelelő, kifejező nevet és, amely leírja, hogy hello keresés által visszaadott hello adategységeket.</span><span class="sxs-lookup"><span data-stu-id="5280e-122">Pick a name that is meaningful and that describes hello data assets that will be returned by hello search.</span></span>

### <a name="manage-saved-searches"></a><span data-ttu-id="5280e-123">Mentett keresések kezelése</span><span class="sxs-lookup"><span data-stu-id="5280e-123">Manage saved searches</span></span>
<span data-ttu-id="5280e-124">Egy vagy több keresés mentése után a **mentett keresések** hello alatt jelennek meg, a beállítás **aktuális keresés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="5280e-124">After you have saved one or more searches, a **Saved Searches** option is displayed beneath hello **Current Search** box.</span></span> <span data-ttu-id="5280e-125">Amikor hello listában ki van bontva, minden mentett keresések jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5280e-125">When hello list is expanded, all saved searches are displayed.</span></span>

 ![Mentett keresések listája](./media/data-catalog-how-to-save-pin/03-list.png)

<span data-ttu-id="5280e-127">Hajtsa végre a hello műveletet:</span><span class="sxs-lookup"><span data-stu-id="5280e-127">Do any of hello following:</span></span>

* <span data-ttu-id="5280e-128">tooexecute keresés – hello listában válassza ki a mentett kereséseket.</span><span class="sxs-lookup"><span data-stu-id="5280e-128">tooexecute a search, select a saved search in hello list.</span></span>

* <span data-ttu-id="5280e-129">tooview a mentett keresések beállítások listáját kattintson a lefelé mutató nyílra a következő toohello keresési neve hello.</span><span class="sxs-lookup"><span data-stu-id="5280e-129">tooview a list of management options for a saved search, click hello down arrow next toohello search name.</span></span>

    ![Mentett keresések kezelésére vonatkozó beállítások](./media/data-catalog-how-to-save-pin/04-managing.png)

* <span data-ttu-id="5280e-131">tooenter hello mentett keresés új nevet válasszon **átnevezése**.</span><span class="sxs-lookup"><span data-stu-id="5280e-131">tooenter a new name for hello saved search, select **Rename**.</span></span> <span data-ttu-id="5280e-132">hello keresési definíció nem változott.</span><span class="sxs-lookup"><span data-stu-id="5280e-132">hello search definition is not changed.</span></span>

* <span data-ttu-id="5280e-133">tooremove hello mentett keresés a listáról válassza ki a **törlése**, majd erősítse meg a hello törlését.</span><span class="sxs-lookup"><span data-stu-id="5280e-133">tooremove hello saved search from your list, select **Delete**, and then confirm hello deletion.</span></span>

* <span data-ttu-id="5280e-134">az alapértelmezett keresési, jelölje be toomark hello mentett keresést **mentése alapértelmezett**.</span><span class="sxs-lookup"><span data-stu-id="5280e-134">toomark hello saved search as your default search, select **Save As Default**.</span></span> <span data-ttu-id="5280e-135">Az Azure Data Catalog-kezdőlap hello "empty" keresést hajt végre, ha az alapértelmezett keresés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="5280e-135">If you perform an “empty” search from hello Azure Data Catalog home page, your default search is executed.</span></span> <span data-ttu-id="5280e-136">Ezenkívül hello hello alapértelmezett keresési hello hello tetején jelenik meg van jelölve, amely **mentett keresések** listája.</span><span class="sxs-lookup"><span data-stu-id="5280e-136">In addition, hello search that's marked as hello default search is displayed at hello top of hello **Saved Searches** list.</span></span>

### <a name="organizational-saved-searches"></a><span data-ttu-id="5280e-137">Szervezeti mentett keresések</span><span class="sxs-lookup"><span data-stu-id="5280e-137">Organizational saved searches</span></span>
<span data-ttu-id="5280e-138">A szervezet minden felhasználója keresés saját használatra takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="5280e-138">All user in your organization can save searches for their own use.</span></span> <span data-ttu-id="5280e-139">Katalógus-rendszergazdák is mentheti hello szervezeten belüli összes felhasználók keres.</span><span class="sxs-lookup"><span data-stu-id="5280e-139">Data Catalog administrators can also save searches for all users within hello organization.</span></span> <span data-ttu-id="5280e-140">Amikor a rendszergazdák a Keresés mentése, hogy találkozik egy **megosztás hello vállalaton belül** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5280e-140">When administrators save a search, they're presented with a **Share within hello company** option.</span></span> <span data-ttu-id="5280e-141">Ez a beállítás megosztások hello mentett keresés az összes felhasználó hello szervezet kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="5280e-141">Selecting this option shares hello saved search for all users in hello organization.</span></span>

 ![Szervezeti mentett keresések](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a><span data-ttu-id="5280e-143">Rögzített adategységeket</span><span class="sxs-lookup"><span data-stu-id="5280e-143">Pinned data assets</span></span>
<span data-ttu-id="5280e-144">Mentett keresések mentheti, és ismét felhasználni a keresési definíciókat.</span><span class="sxs-lookup"><span data-stu-id="5280e-144">With saved searches, you can save and reuse search definitions.</span></span> <span data-ttu-id="5280e-145">hello adategységeket hello keresés által visszaadott hello katalógus módosítás hello tartalmát idővel változhatnak.</span><span class="sxs-lookup"><span data-stu-id="5280e-145">hello data assets that are returned by hello searches might change over time as hello contents of hello catalog change.</span></span> <span data-ttu-id="5280e-146">Amikor adategységek, meghatározott eszközök toomake explicit módon azonosíthatja azokat könnyebben tooaccess anélkül, hogy a keresés toouse.</span><span class="sxs-lookup"><span data-stu-id="5280e-146">When you pin data assets, you can explicitly identify specific data assets toomake them easier tooaccess without needing toouse a search.</span></span>

<span data-ttu-id="5280e-147">Egy adategységet rögzítési egyszerű.</span><span class="sxs-lookup"><span data-stu-id="5280e-147">Pinning a data asset is straightforward.</span></span> <span data-ttu-id="5280e-148">tooadd hello adatok rögzített tooyour lista, akkor egyszerűen kattintson hello **PIN-kód** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5280e-148">tooadd hello data asset tooyour pinned list, you simply click hello **pin** icon.</span></span> <span data-ttu-id="5280e-149">hello ikon hello sarkában hello eszköz csempe hello mozaik elrendezés nézetben, és hello listanézetben hello Azure Data Catalog portálon hello bal szélső oszlopában.</span><span class="sxs-lookup"><span data-stu-id="5280e-149">hello icon is displayed in hello corner of hello asset tile in hello tile view, and in hello left-most column in hello list view in hello Azure Data Catalog portal.</span></span>

![hello adategységet rögzítés ikonja](./media/data-catalog-how-to-save-pin/05-pinning.png)

<span data-ttu-id="5280e-151">Egy adategységet feloldásával egyaránt egyszerű.</span><span class="sxs-lookup"><span data-stu-id="5280e-151">Unpinning a data asset is equally straightforward.</span></span> <span data-ttu-id="5280e-152">Egyszerűen kattintson a hello **rögzítésének** ikon tootoggle hello beállítás hello kiválasztott eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="5280e-152">Simply click hello **unpin** icon tootoggle hello setting for hello selected asset.</span></span>

![hello adategységet rögzítésének ikon](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="hello-my-assets-section"></a><span data-ttu-id="5280e-154">Saját eszközök szakasz hello</span><span class="sxs-lookup"><span data-stu-id="5280e-154">hello My Assets section</span></span>
<span data-ttu-id="5280e-155">hello tartalmazza a portál kezdőlapján a Data Catalog egy **saját eszközök** szakaszt, amely megjeleníti az eszközök érdeklődési toohello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="5280e-155">hello Data Catalog portal home page includes a **My Assets** section that displays assets of interest toohello current user.</span></span> <span data-ttu-id="5280e-156">Ez a szakasz mindkét rögzített eszközöket tartalmazza, és a mentett kereséseket.</span><span class="sxs-lookup"><span data-stu-id="5280e-156">This section includes both pinned assets and saved searches.</span></span>

![hello saját eszközök szakasz hello kezdőlapján](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a><span data-ttu-id="5280e-158">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="5280e-158">Summary</span></span>
<span data-ttu-id="5280e-159">Az Azure Data Catalog biztosít képességeket, amelyek könnyebb toodiscover hello adatforrások van szüksége, ezért más szervezet tagja is kevesebb időt töltenek az adatok és dolgozni vele több időt keres.</span><span class="sxs-lookup"><span data-stu-id="5280e-159">Azure Data Catalog provides capabilities that make it easier toodiscover hello data sources you need, so you and other organization members can spend less time looking for data and more time working with it.</span></span> <span data-ttu-id="5280e-160">Mentett keresések, és a rögzített adatok eszközök ezek alapképességek létrehozásához, így a felhasználók könnyen azonosíthassák, hogy az adatforrások, amelyek működnek a ismételten.</span><span class="sxs-lookup"><span data-stu-id="5280e-160">Saved searches and pinned data assets build on these core capabilities so users can easily identify data sources that they work with repeatedly.</span></span>
