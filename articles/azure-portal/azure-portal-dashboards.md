---
title: "aaaCreate és az Azure portál irányítópultok megosztása |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan toocreate és Szerkesztés irányítópultok a hello Azure-portálon."
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 0facd10fe3284d7ad9a9e29791e5af5b5b95c97f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-dashboards-in-hello-azure-portal"></a><span data-ttu-id="efed5-103">Hozzon létre és osszon irányítópultok a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="efed5-103">Create and share dashboards in hello Azure portal</span></span>
<span data-ttu-id="efed5-104">Több irányítópulton létrehozhat és megoszthatja őket másokkal, akiknek hozzáférést tooyour Azure előfizetések.</span><span class="sxs-lookup"><span data-stu-id="efed5-104">You can create multiple dashboards and share them with others who have access tooyour Azure subscriptions.</span></span>  <span data-ttu-id="efed5-105">Ez a cikk végighalad létrehozása, szerkesztése, közzététele és kezelése, hozzáférés toodashboards hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="efed5-105">This article goes through hello basics of creating, editing, publishing, and managing access toodashboards.</span></span>

## <a name="create-a-dashboard"></a><span data-ttu-id="efed5-106">Irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="efed5-106">Create a dashboard</span></span>
<span data-ttu-id="efed5-107">toocreate egy irányítópultot, válassza hello **új irányítópult** következő toohello aktuális irányítópult neve gombra.</span><span class="sxs-lookup"><span data-stu-id="efed5-107">toocreate a dashboard, select hello **New dashboard** button next toohello current dashboard's name.</span></span>  

![irányítópult létrehozása](./media/azure-portal-dashboards/new-dashboard.png)

<span data-ttu-id="efed5-109">Ez a művelet létrehoz egy új, üres, személyes irányítópultot, és elhelyezi, amelyen az irányítópult neve, és adja hozzá vagy átrendezheti a csempéket testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="efed5-109">This action creates a new, empty, private dashboard and puts you into customization mode where you can name your dashboard and add or rearrange tiles.</span></span>  <span data-ttu-id="efed5-110">Ebben a módban a hello összecsukható hello bal oldali navigációs menü csempére a gyűjtemény vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="efed5-110">When in this mode, hello collapsible tile gallery takes over hello left navigation menu.</span></span>  <span data-ttu-id="efed5-111">hello csempe gyűjtemény lehetővé teszi, hogy az Azure-erőforrások különböző módokon csempék találhatók: által tallózással [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups), az erőforrás írja be, ha [címke](../azure-resource-manager/resource-group-using-tags.md), vagy az erőforrás neve alapján keres.</span><span class="sxs-lookup"><span data-stu-id="efed5-111">hello tile gallery lets you find tiles for your Azure resources in various ways: you can browse by [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups), by resource type, by [tag](../azure-resource-manager/resource-group-using-tags.md), or by searching for your resource by name.</span></span>  

![testre szabhatja az irányítópultot](./media/azure-portal-dashboards/customize-dashboard.png)

<span data-ttu-id="efed5-113">Húzással őket hello irányítópult felület, ahol azt szeretné, hozzáadhatja a csempéket.</span><span class="sxs-lookup"><span data-stu-id="efed5-113">Add tiles by dragging and dropping them onto hello dashboard surface wherever you want.</span></span>

<span data-ttu-id="efed5-114">Van egy új kategóriába **általános** csempék, amelyek nincsenek társítva van egy adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="efed5-114">There's a new category called **General** for tiles that are not associated with a particular resource.</span></span>  <span data-ttu-id="efed5-115">Ebben a példában azt PIN-kód hello Markdown csempe.</span><span class="sxs-lookup"><span data-stu-id="efed5-115">In this example, we pin hello Markdown tile.</span></span>  <span data-ttu-id="efed5-116">A csempe tooadd egyéni tartalom tooyour irányítópult használja.</span><span class="sxs-lookup"><span data-stu-id="efed5-116">You use this tile tooadd custom content tooyour dashboard.</span></span>  <span data-ttu-id="efed5-117">hello csempe támogatja egyszerű szöveges [Markdown-szintaxis](https://daringfireball.net/projects/markdown/syntax), és a HTML egy korlátozott körét.</span><span class="sxs-lookup"><span data-stu-id="efed5-117">hello tile supports plain text, [Markdown syntax](https://daringfireball.net/projects/markdown/syntax), and a limited set of HTML.</span></span>  <span data-ttu-id="efed5-118">(A biztonsági, mint szúrjon nem hajtható végre `<script>` címkéket, vagy bizonyos stílusbeállításokat elemmel, amely hello portal CSS.)</span><span class="sxs-lookup"><span data-stu-id="efed5-118">(For safety, you can't do things like inject `<script>` tags or use certain styling element of CSS that might interfere with hello portal.)</span></span> 

![markdown hozzáadása](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a><span data-ttu-id="efed5-120">Irányítópult szerkesztése</span><span class="sxs-lookup"><span data-stu-id="efed5-120">Edit a dashboard</span></span>
<span data-ttu-id="efed5-121">Miután létrehozta az irányítópulton, PIN-kód hello csempe vagy a paneleken hello csempe ábrázolását csempét.</span><span class="sxs-lookup"><span data-stu-id="efed5-121">After creating your dashboard, you can pin tiles from hello tile gallery or hello tile representation of blades.</span></span> <span data-ttu-id="efed5-122">Most PIN-kód az erőforráscsoport hello ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="efed5-122">Let's pin hello representation of our resource group.</span></span> <span data-ttu-id="efed5-123">Hello böngészésekor, elem vagy hello erőforráscsoport panel vagy rögzíti.</span><span class="sxs-lookup"><span data-stu-id="efed5-123">You can either pin when browsing hello item, or from hello resource group blade.</span></span> <span data-ttu-id="efed5-124">Mindkét megközelítés eredményez hello erőforráscsoport ábrázolását hello csempe rögzítését.</span><span class="sxs-lookup"><span data-stu-id="efed5-124">Both approaches result in pinning hello tile representation of hello resource group.</span></span>

![PIN-kód toodashboard](./media/azure-portal-dashboards/pin-to-dashboard.png)

<span data-ttu-id="efed5-126">Miután hello elem, megjelenik az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="efed5-126">After pinning hello item, it appears on your dashboard.</span></span>

![Irányítópult nézet](./media/azure-portal-dashboards/view-dashboard.png)

<span data-ttu-id="efed5-128">Most, hogy a Markdown csempe- és erőforrás csoport rögzített toohello irányítópultot, azt átméretezése, és megfelelő elrendezésekben hello csempék átrendezését.</span><span class="sxs-lookup"><span data-stu-id="efed5-128">Now that we have a Markdown tile and a resource group pinned toohello dashboard, we can resize and rearrange hello tiles into a suitable layout.</span></span>

<span data-ttu-id="efed5-129">Rámutató és válassza a "...", vagy kattintson a jobb gombbal a mozaikokra összes hello környezetfüggő parancsok mozaik tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="efed5-129">By hovering and selecting "…" or right-clicking on a tile you can see all hello contextual commands for that tile.</span></span> <span data-ttu-id="efed5-130">Alapértelmezés szerint nincsenek két elemek:</span><span class="sxs-lookup"><span data-stu-id="efed5-130">By default, there are two items:</span></span>

1. <span data-ttu-id="efed5-131">**Az irányítópult rögzítését** – hello irányítópultról eltávolítja hello csempe</span><span class="sxs-lookup"><span data-stu-id="efed5-131">**Unpin from dashboard** – removes hello tile from hello dashboard</span></span>
2. <span data-ttu-id="efed5-132">**Testre szabhatja** – kerül üzemmód testreszabása</span><span class="sxs-lookup"><span data-stu-id="efed5-132">**Customize** – enters customize mode</span></span>

![Mozaik elrendezés testreszabása](./media/azure-portal-dashboards/customize-tile.png)

<span data-ttu-id="efed5-134">Kiválasztásával testreszabása, méretezze át, és újra sorrendbe állítja a csempéket.</span><span class="sxs-lookup"><span data-stu-id="efed5-134">By selecting customize, you can resize and reorder tiles.</span></span> <span data-ttu-id="efed5-135">tooresize csempére, válassza hello hello környezetfüggő menüből, új méret, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="efed5-135">tooresize a tile, select hello new size from hello contextual menu, as shown in hello following image.</span></span>

![Automatikus oszlopszélesség csempe](./media/azure-portal-dashboards/resize-tile.png)

<span data-ttu-id="efed5-137">Vagy ha hello csempe bármilyen méretű támogatja, húzva hello alsó jobb sarkában szükséges toohello méretét.</span><span class="sxs-lookup"><span data-stu-id="efed5-137">Or, if hello tile supports any size, you can drag hello bottom right-hand corner toohello desired size.</span></span>

![Automatikus oszlopszélesség csempe](./media/azure-portal-dashboards/resize-corner.png)

<span data-ttu-id="efed5-139">Csempék átméretezése, után hello irányítópult megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="efed5-139">After resizing tiles, view hello dashboard.</span></span>

![csempéje](./media/azure-portal-dashboards/view-tile.png)

<span data-ttu-id="efed5-141">Miután befejezte a testreszabása egy irányítópultot, egyszerűen válassza hello **végzett Testreszabás** tooexit testreszabási módban, vagy kattintson a jobb gombbal, és válassza ki **végzett Testreszabás** hello helyi menüből.</span><span class="sxs-lookup"><span data-stu-id="efed5-141">Once you are finished customizing a dashboard, simply select hello **Done customizing** tooexit customize mode or right-click and select **Done customizing** from hello context menu.</span></span>

## <a name="publish-a-dashboard-and-manage-access-control"></a><span data-ttu-id="efed5-142">Irányítópult közzétételét és kezelését a hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="efed5-142">Publish a dashboard and manage access control</span></span>
<span data-ttu-id="efed5-143">Amikor létrehoz egy irányítópultot, saját alapértelmezés szerint azt jelenti, hogy hello csak ő is.</span><span class="sxs-lookup"><span data-stu-id="efed5-143">When you create a dashboard, it is private by default, which means you are hello only person who can see it.</span></span>  <span data-ttu-id="efed5-144">toomake azt látható tooothers hello használata **megosztás** mellett megjelenő gombra hello más irányítópult parancsok.</span><span class="sxs-lookup"><span data-stu-id="efed5-144">toomake it visible tooothers, use hello **Share** button that appears alongside hello other dashboard commands.</span></span>

![Irányítópult megosztása](./media/azure-portal-dashboards/share-dashboard.png)

<span data-ttu-id="efed5-146">Az irányítópult toobe előfizetésbe és erőforráscsoportba csoport közzétett toochoose kell adnia.</span><span class="sxs-lookup"><span data-stu-id="efed5-146">You are asked toochoose a subscription and resource group for your dashboard toobe published to.</span></span> <span data-ttu-id="efed5-147">tooseamlessly Irányítópultok integrálása hello ökoszisztéma, azt korábban megosztott irányítópultok, megvalósítva Azure-erőforrások (tehát nem lehet megosztani, írja be az e-mail címet).</span><span class="sxs-lookup"><span data-stu-id="efed5-147">tooseamlessly integrate dashboards into hello ecosystem, we've implemented shared dashboards as Azure resources (so you can't share by typing an email address).</span></span>  <span data-ttu-id="efed5-148">Hello csempék hello portálon többsége által megjelenített adatok eléréséhez toohello irányadók [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="efed5-148">Access toohello information displayed by most of hello tiles in hello portal are governed by [Azure Role Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="efed5-149">Access control szempontból megosztott irányítópultok ugyanazok a virtuális gép vagy egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="efed5-149">From an access control perspective, shared dashboards are no different from a virtual machine or a storage account.</span></span>  

<span data-ttu-id="efed5-150">Tegyük fel, Azure-előfizetéssel rendelkezik, és a csoport tagjai hozzárendelt hello szerepkörök **tulajdonos**, **közreműködő**, vagy **olvasó** hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="efed5-150">Let's say you have an Azure subscription and members of your team have been assigned hello roles of **owner**, **contributor**, or **reader** of hello subscription.</span></span>  <span data-ttu-id="efed5-151">Tulajdonos vagy közreműködő szerepkörrel rendelkező személyek felhasználók képes toolist, nézet, létrehozása, módosítása, vagy törölje az adott előfizetésen belül irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="efed5-151">Users who are owners or contributors are able toolist, view, create, modify, or delete dashboards within that subscription.</span></span>  <span data-ttu-id="efed5-152">Felhasználók, akik olvasók képes toolist és nézet irányítópultokat, amelyek nem, de módosíthatja vagy törölheti őket is.</span><span class="sxs-lookup"><span data-stu-id="efed5-152">Users who are readers are able toolist and view dashboards, but cannot modify or delete them.</span></span>  <span data-ttu-id="efed5-153">Olvasási joggal rendelkező felhasználók képes toomake helyi szerkesztést tooa megosztott irányítópult, azonban nem tudja toopublish e módosítások hátsó toohello server találhatók.</span><span class="sxs-lookup"><span data-stu-id="efed5-153">Users with reader access are able toomake local edits tooa shared dashboard, but are not able toopublish those changes back toohello server.</span></span>  <span data-ttu-id="efed5-154">Azonban tudják hello irányítópult saját használatra titkos másolatát.</span><span class="sxs-lookup"><span data-stu-id="efed5-154">However, they can make a private copy of hello dashboard for their own use.</span></span>  <span data-ttu-id="efed5-155">Mivel egyes csempék hello irányítópult mindig, saját alapján megfelelnek hello erőforrások hozzáférés-vezérlési szabályok kényszerítéséhez.</span><span class="sxs-lookup"><span data-stu-id="efed5-155">As always, individual tiles on hello dashboard enforce their own access control rules based on hello resources they correspond to.</span></span>  

<span data-ttu-id="efed5-156">Kényelmi célokat szolgál, hello portal erőforráscsoportban irányítópultok helyétől minta felé hívott élmény útmutatók a közzétételi **irányítópultok**.</span><span class="sxs-lookup"><span data-stu-id="efed5-156">For convenience, hello portal's publishing experience guides you towards a pattern where you place dashboards in a resource group called **dashboards**.</span></span>  

![Irányítópult közzététele](./media/azure-portal-dashboards/publish-dashboard.png)

<span data-ttu-id="efed5-158">Másik lehetőségként toopublish irányítópult tooa adott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="efed5-158">You can also choose toopublish a dashboard tooa particular resource group.</span></span>  <span data-ttu-id="efed5-159">Az irányítópult hello hozzáférés-vezérlés hello hozzáférés-vezérlés hello erőforráscsoport megegyezik.</span><span class="sxs-lookup"><span data-stu-id="efed5-159">hello access control for that dashboard matches hello access control for hello resource group.</span></span>  <span data-ttu-id="efed5-160">Felhasználók által kezelhető az erőforráscsoport hello erőforrások hozzáférés toohello irányítópultok is.</span><span class="sxs-lookup"><span data-stu-id="efed5-160">Users that can manage hello resources in that resource group also have access toohello dashboards.</span></span>

![Irányítópult tooresource csoport közzététele](./media/azure-portal-dashboards/publish-to-resource-group.png)

<span data-ttu-id="efed5-162">Az irányítópult közzététele után hello **megosztás + hozzáférés** Vezérlőpulton frissítse, és azt hello közzétett irányítópulton, beleértve a hivatkozás toomanage felhasználói hozzáférés toohello irányítópult információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="efed5-162">After your dashboard is published, hello **Sharing + access** control pane will refresh and show you information about hello published dashboard, including a link toomanage user access toohello dashboard.</span></span>  <span data-ttu-id="efed5-163">Ez a hivatkozás indít hello szabványos szerepköralapú hozzáférés-vezérlés használt panel toomanage hozzáférést az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="efed5-163">This link launches hello standard Role Based Access Control blade used toomanage access for any Azure resource.</span></span>  <span data-ttu-id="efed5-164">Mindig kaphat vissza toothis nézet kijelölése **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="efed5-164">You can always get back toothis view by selecting **Share**.</span></span>

![hozzáférés-vezérlés kezelése](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a><span data-ttu-id="efed5-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="efed5-166">Next steps</span></span>
* <span data-ttu-id="efed5-167">toomanage erőforrások, lásd: [kezelése Azure-erőforrások portálon keresztül](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="efed5-167">toomanage resources, see [Manage Azure resources through portal](../azure-resource-manager/resource-group-portal.md).</span></span>
* <span data-ttu-id="efed5-168">toodeploy erőforrások, lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](../azure-resource-manager/resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="efed5-168">toodeploy resources, see [Deploy resources with Resource Manager templates and Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).</span></span>

