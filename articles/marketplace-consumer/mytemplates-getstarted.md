---
title: "aaaGet a Magánsablonok használatába |} Microsoft Docs"
description: "Hozzáadása, kezelése és megosztása magánsablonok hello Azure-portálon, a hello Azure CLI vagy a PowerShell használatával."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.openlocfilehash: 1fe2c6422f62a98f7ae9ba5c61b9639d993f0bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-private-templates-on-hello-azure-portal"></a><span data-ttu-id="48ff7-103">Ismerkedés az Azure portál hello Magánsablonok</span><span class="sxs-lookup"><span data-stu-id="48ff7-103">Get started with private Templates on hello Azure Portal</span></span>
<span data-ttu-id="48ff7-104">Egy [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sablon olyan deklaratív sablonok használt toodefine a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="48ff7-104">An [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) template is a declarative template used toodefine your deployment.</span></span> <span data-ttu-id="48ff7-105">Hello erőforrások toodeploy megoldás határozza meg, és adja meg a paramétereket és változókat, amelyek lehetővé teszik a különböző környezetekhez tooinput értékeket.</span><span class="sxs-lookup"><span data-stu-id="48ff7-105">You can define hello resources toodeploy for a solution, and specify parameters and variables that enable you tooinput values for different environments.</span></span> <span data-ttu-id="48ff7-106">hello sablon JSON és áll kifejezés, amelynek segítségével az üzemelő példány tooconstruct értékeit.</span><span class="sxs-lookup"><span data-stu-id="48ff7-106">hello template consists of JSON and expressions which you can use tooconstruct values for your deployment.</span></span>

<span data-ttu-id="48ff7-107">Hello új használható **sablonok** hello képesség [Azure Portal](https://portal.azure.com) együtt hello **Microsoft.Gallery** erőforrás-szolgáltató a hello [ Az Azure piactér](https://azure.microsoft.com/marketplace/) tooenable felhasználók toocreate, kezelését, és saját könyvtárukból származó magánsablonok telepítését.</span><span class="sxs-lookup"><span data-stu-id="48ff7-107">You can use hello new **Templates** capability in hello [Azure Portal](https://portal.azure.com) along with hello **Microsoft.Gallery** resource provider as an extension of hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable users toocreate, manage and deploy private templates from a personal library.</span></span>

<span data-ttu-id="48ff7-108">Ez a dokumentum útmutatást nyújt a hozzáadása, kezelése és oszthat **sablon** használatával hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="48ff7-108">This document walks you through adding, managing and sharing a private **Template** using hello Azure Portal.</span></span>

## <a name="guidance"></a><span data-ttu-id="48ff7-109">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="48ff7-109">Guidance</span></span>
<span data-ttu-id="48ff7-110">hello következő javaslatok segítségével teljes körű kihasználása **sablonok** a megoldásaival való munka során:</span><span class="sxs-lookup"><span data-stu-id="48ff7-110">hello following suggestions will help you take full advantage of **Templates** when working with your solutions:</span></span>

* <span data-ttu-id="48ff7-111">A **sablonok** beágyazott erőforrások, amelyek egy Resource Manager-sablont, illetve további metaadatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="48ff7-111">A **Template** is an encapsulating resource that contains an Resource Manager template and additional metadata.</span></span> <span data-ttu-id="48ff7-112">Az hasonlóan viselkednek, mint hello piactér tooan elemére.</span><span class="sxs-lookup"><span data-stu-id="48ff7-112">It behaves very similarly tooan item in hello Marketplace.</span></span> <span data-ttu-id="48ff7-113">hello legfontosabb különbség, hogy a személyes elemet, megakadályozását toohello piactér elemei.</span><span class="sxs-lookup"><span data-stu-id="48ff7-113">hello key difference is that it is a private item as opposed toohello public Marketplace items.</span></span>
* <span data-ttu-id="48ff7-114">Hello **sablonok** könyvtár hasznos segítséget nyújt a felhasználóknak, akik toocustomize a telepítések.</span><span class="sxs-lookup"><span data-stu-id="48ff7-114">hello **Templates** library works well for users who need toocustomize their deployments.</span></span>
* <span data-ttu-id="48ff7-115">A **sablonok** egyszerű Azure-beli tárházat biztosítanak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="48ff7-115">**Templates** work well for users who need a simple repository within Azure.</span></span>
* <span data-ttu-id="48ff7-116">Kezdje egy meglévő Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="48ff7-116">Start with an existing Resource Manager template.</span></span> <span data-ttu-id="48ff7-117">Keresse meg a kívánt sablont a [GitHubon](https://github.com/Azure/azure-quickstart-templates), vagy [exportálja a sablont](../azure-resource-manager/resource-manager-export-template.md) egy meglévő erőforráscsoportból.</span><span class="sxs-lookup"><span data-stu-id="48ff7-117">Find templates in [github](https://github.com/Azure/azure-quickstart-templates) or [Export template](../azure-resource-manager/resource-manager-export-template.md) from an existing resource group.</span></span>
* <span data-ttu-id="48ff7-118">**Sablonok** kapcsolt toohello-felhasználó, aki közzéteszi őket.</span><span class="sxs-lookup"><span data-stu-id="48ff7-118">**Templates** are tied toohello user who publishes them.</span></span> <span data-ttu-id="48ff7-119">hello közzétevő neve látható tooeveryone, aki rendelkezik olvasási jogosultsággal tooit.</span><span class="sxs-lookup"><span data-stu-id="48ff7-119">hello publisher name is visible tooeveryone who has read access tooit.</span></span>
* <span data-ttu-id="48ff7-120">A **sablonok** a Resource Managerhez tartozó erőforrások, amelyeket közzététel után nem lehet átnevezni.</span><span class="sxs-lookup"><span data-stu-id="48ff7-120">**Templates** are Resource Manager resources and cannot be renamed once published.</span></span>

## <a name="add-a-template-resource"></a><span data-ttu-id="48ff7-121">Sablonerőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48ff7-121">Add a Template resource</span></span>
<span data-ttu-id="48ff7-122">Két módon toocreate van egy **sablon** erőforrás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="48ff7-122">There are two ways toocreate a **Template** resource in hello Azure portal.</span></span>

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a><span data-ttu-id="48ff7-123">1. módszer: Új sablonerőforrás létrehozása már futó erőforráscsoportból</span><span class="sxs-lookup"><span data-stu-id="48ff7-123">Method 1 : Create a new Template resource from a running resource group</span></span>
1. <span data-ttu-id="48ff7-124">Keresse meg a tooan meglévő erőforráscsoportot hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="48ff7-124">Navigate tooan existing resource group on hello Azure Portal.</span></span> <span data-ttu-id="48ff7-125">A **Beállítások** menüben válassza a **Sablon exportálása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="48ff7-125">Select **Export template** in **Settings**.</span></span>
2. <span data-ttu-id="48ff7-126">Ha hello Resource Manager sablon exportálása, a hello **sablon mentése** gomb toosave azt toohello **sablonok** tárház.</span><span class="sxs-lookup"><span data-stu-id="48ff7-126">Once hello Resource Manager template is exported, use hello **Save Template** button toosave it toohello **Templates** repository.</span></span> <span data-ttu-id="48ff7-127">A Sablon exportálása funkcióról részletes leírást [itt](../azure-resource-manager/resource-manager-export-template.md) talál.</span><span class="sxs-lookup"><span data-stu-id="48ff7-127">Find complete details for Export template [here](../azure-resource-manager/resource-manager-export-template.md).</span></span>
   <br /><br /><span data-ttu-id="48ff7-128">
   ![Erőforráscsoport exportálása](media/rg-export-portal1.PNG)</span><span class="sxs-lookup"><span data-stu-id="48ff7-128">
![Resource group export](media/rg-export-portal1.PNG)</span></span>  <br />
3. <span data-ttu-id="48ff7-129">Jelölje be hello **tooTemplate mentése** parancsgombra.</span><span class="sxs-lookup"><span data-stu-id="48ff7-129">Select hello **Save tooTemplate** command button.</span></span>
   <br /><br />
4. <span data-ttu-id="48ff7-130">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="48ff7-130">Enter hello following information:</span></span>
   
   * <span data-ttu-id="48ff7-131">Név – hello sablonobjektum neve (Megjegyzés: az Azure Resource Manager-alapú neve.</span><span class="sxs-lookup"><span data-stu-id="48ff7-131">Name – Name of hello template object (NOTE: This is an Azure Resource Manager based name.</span></span> <span data-ttu-id="48ff7-132">Így minden vonatkozó elnevezési korlátozás érvényes rá, és létrehozását követően nem módosítható).</span><span class="sxs-lookup"><span data-stu-id="48ff7-132">All naming restrictions apply and it cannot be changed once created).</span></span>
   * <span data-ttu-id="48ff7-133">Leírás – hello sablon rövid ismertetése.</span><span class="sxs-lookup"><span data-stu-id="48ff7-133">Description – Quick summary about hello template.</span></span>
     
     ![Sablon mentése](media/save-template-portal1.PNG)  <br />
5. <span data-ttu-id="48ff7-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="48ff7-135">Click **Save**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="48ff7-136">hello sablon exportálása panelen értesítések jelennek meg hello exportált Resource Manager sablon hibákkal rendelkezik, de továbbra is meg fogja tudni toosave a Resource Manager sablon toohello sablonok.</span><span class="sxs-lookup"><span data-stu-id="48ff7-136">hello Export template blade shows notifications when hello exported Resource Manager template has errors, but you will still be able toosave this Resource Manager template toohello Templates.</span></span> <span data-ttu-id="48ff7-137">Győződjön meg arról, hogy ellenőrizze, és ki a Resource Manager sablon kapcsolatos problémák megoldása ismételt üzembe helyezéssel hello Resource Manager sablon exportálása előtt.</span><span class="sxs-lookup"><span data-stu-id="48ff7-137">Ensure that you check and fix any Resource Manager template issues before redeploying hello exported Resource Manager template.</span></span>
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a><span data-ttu-id="48ff7-138">2. módszer: Új sablonerőforrás hozzáadása tallózással</span><span class="sxs-lookup"><span data-stu-id="48ff7-138">Method 2 : Add a new Template resource from browse</span></span>
<span data-ttu-id="48ff7-139">Azt is megteheti egy új **sablon** használatával hello + Hozzáadás parancsgomb az alapoktól **Tallózás > sablonok**.</span><span class="sxs-lookup"><span data-stu-id="48ff7-139">You can also add a new **Template** from scratch using hello +Add command button in **Browse > Templates**.</span></span> <span data-ttu-id="48ff7-140">Szüksége lesz tooprovide nevét, leírását és a Resource Manager-sablon JSON hello.</span><span class="sxs-lookup"><span data-stu-id="48ff7-140">You will need tooprovide a Name, Description and hello Resource Manager template JSON.</span></span>

![Sablon hozzáadása](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> <span data-ttu-id="48ff7-142">A Microsoft.Gallery egy bérlőalapú Azure-erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="48ff7-142">Microsoft.Gallery is a Tenant based Azure resource provider.</span></span> <span data-ttu-id="48ff7-143">hello sablonerőforrás az toohello kapcsolt felhasználó, aki létrehozta.</span><span class="sxs-lookup"><span data-stu-id="48ff7-143">hello Template resource is tied toohello user who created it.</span></span> <span data-ttu-id="48ff7-144">A feltételekhez tooany előfizetéshez nincs.</span><span class="sxs-lookup"><span data-stu-id="48ff7-144">It is not tied tooany specific subscription.</span></span> <span data-ttu-id="48ff7-145">Előfizetés csak a sablon telepítésekor kiválasztott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="48ff7-145">A subscription needs toobe chosen only when deploying a Template.</span></span>
> 
> 

## <a name="view-template-resources"></a><span data-ttu-id="48ff7-146">Sablonerőforrás megtekintése</span><span class="sxs-lookup"><span data-stu-id="48ff7-146">View Template resources</span></span>
<span data-ttu-id="48ff7-147">Minden **sablonok** elérhető tooyou látható a következő **Tallózás > sablonok**.</span><span class="sxs-lookup"><span data-stu-id="48ff7-147">All **Templates** available tooyou can be seen at **Browse > Templates**.</span></span> <span data-ttu-id="48ff7-148">Itt az Ön által létrehozott **sablonok**, valamint az Önnel különböző szintű engedélyekkel megosztott sablonok egyaránt megjelennek.</span><span class="sxs-lookup"><span data-stu-id="48ff7-148">This includes **Templates** you have created as well as ones that have been shared with you with varying levels of permissions.</span></span> <span data-ttu-id="48ff7-149">További részletek a hello [hozzáférés-vezérlés](#access-control-for-a-tenant-resource-provider) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="48ff7-149">More details in hello [access control](#access-control-for-a-tenant-resource-provider) section below.</span></span>

![Sablon megtekintése](media/view-template-portal1.PNG)  <br />

<span data-ttu-id="48ff7-151">Hello részleteit megtekintheti a **sablon** hello lista elemére kattintva.</span><span class="sxs-lookup"><span data-stu-id="48ff7-151">You can view hello details of a **Template** by clicking into an item in hello list.</span></span>

![Sablon megtekintése](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a><span data-ttu-id="48ff7-153">Sablonerőforrás módosítása</span><span class="sxs-lookup"><span data-stu-id="48ff7-153">Edit a Template resource</span></span>
<span data-ttu-id="48ff7-154">Hello szerkesztési folyamatának is kezdeményezhető a **sablon** hello Tallózás listában hello elemre jobb gombbal kattint rá vagy hello Szerkesztés parancsgombot kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="48ff7-154">You can initiate hello edit flow for a **Template** by right clicking hello item on hello Browse list or by choosing hello Edit command button.</span></span>

![Sablon szerkesztése](media/edit-template-portal1a.PNG)  <br />

<span data-ttu-id="48ff7-156">Szerkesztheti a hello leírást vagy a Resource Manager-sablon szövegét.</span><span class="sxs-lookup"><span data-stu-id="48ff7-156">You can edit hello description or Resource Manager template text.</span></span> <span data-ttu-id="48ff7-157">Hello neve nem szerkeszthető, mert ez az erőforrás-kezelő erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="48ff7-157">You cannot edit hello name since it is an Resource Manager resource name.</span></span> <span data-ttu-id="48ff7-158">Hello Resource Manager-sablon JSON szerkesztésekor Microsoft ellenőrzi, hogy-e érvényes JSON tooensure.</span><span class="sxs-lookup"><span data-stu-id="48ff7-158">When you edit hello Resource Manager template JSON we will validate tooensure that it is valid JSON.</span></span> <span data-ttu-id="48ff7-159">Válasszon **OK** , majd **mentése** toosave a módosított sablon.</span><span class="sxs-lookup"><span data-stu-id="48ff7-159">Choose **OK** and then **Save** toosave your updated template.</span></span>

![Sablon szerkesztése](media/edit-template-portal2a.PNG)  <br />

<span data-ttu-id="48ff7-161">Egyszer hello **sablon** mentett megerősítési értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="48ff7-161">Once hello **Template** is saved you will see a confirmation notification.</span></span>

![Sablon szerkesztése](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a><span data-ttu-id="48ff7-163">Sablonerőforrás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="48ff7-163">Deploy a Template resource</span></span>
<span data-ttu-id="48ff7-164">Bármely **sablon** üzembe helyezhető, amelyhez **olvasási** engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="48ff7-164">You can deploy any **Template** that you have **Read** permissions on.</span></span> <span data-ttu-id="48ff7-165">hello üzembe helyezési folyamat elindítja hello standard Azure-sablon üzembe helyezési paneljét.</span><span class="sxs-lookup"><span data-stu-id="48ff7-165">hello deployment flow launches hello standard Azure Template deployment blade.</span></span> <span data-ttu-id="48ff7-166">Töltse ki a hello Resource Manager sablon paraméterek tooproceed hello telepítési hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="48ff7-166">Fill out hello values for hello Resource Manager template parameters tooproceed with hello deployment.</span></span>

![Sablon üzembe helyezése](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a><span data-ttu-id="48ff7-168">Sablonerőforrás megosztása</span><span class="sxs-lookup"><span data-stu-id="48ff7-168">Share a Template resource</span></span>
<span data-ttu-id="48ff7-169">A **sablonerőforrást** megoszthatja kollégáival.</span><span class="sxs-lookup"><span data-stu-id="48ff7-169">A **Template** resource can be shared with your peers.</span></span> <span data-ttu-id="48ff7-170">Megosztás hasonlóan működik, hasonlóképpen túl[szerepkör-hozzárendelés az Azure-erőforrásoknál](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="48ff7-170">Sharing behaves similarly too[role assignment for any resource on Azure](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="48ff7-171">Hello **sablon** tulajdonos tooother kommunikáló felhasználók számára is biztosít engedélyeket a sablonerőforrás.</span><span class="sxs-lookup"><span data-stu-id="48ff7-171">hello **Template** owner provides permissions tooother users who can interact with a Template resource.</span></span> <span data-ttu-id="48ff7-172">hello személyek vagy csoportok személyek hello megosztott **sablon** pedig képes toosee hello Resource Manager-sablon és a hozzá tartozó katalógusbeli tulajdonságok is.</span><span class="sxs-lookup"><span data-stu-id="48ff7-172">hello person or group of people you share hello **Template** with will be able toosee hello Resource Manager template and its gallery properties.</span></span>

### <a name="access-control-for-hello-microsoftgallery-resources"></a><span data-ttu-id="48ff7-173">Hello Microsoft.Gallery erőforrások hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="48ff7-173">Access control for hello Microsoft.Gallery resources</span></span>
| <span data-ttu-id="48ff7-174">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="48ff7-174">Role</span></span> | <span data-ttu-id="48ff7-175">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="48ff7-175">Permissions</span></span> |
| --- | --- |
| <span data-ttu-id="48ff7-176">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="48ff7-176">Owner</span></span> |<span data-ttu-id="48ff7-177">Lehetővé teszi, hogy a megosztás beleértve hello sablonerőforrás teljes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="48ff7-177">Allows full control on hello Template resource including Share</span></span> |
| <span data-ttu-id="48ff7-178">Olvasó</span><span class="sxs-lookup"><span data-stu-id="48ff7-178">Reader</span></span> |<span data-ttu-id="48ff7-179">Lehetővé teszi az olvasási és Execute(Deploy) hello sablonerőforrás</span><span class="sxs-lookup"><span data-stu-id="48ff7-179">Allows Read and Execute(Deploy) on hello Template resource</span></span> |
| <span data-ttu-id="48ff7-180">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="48ff7-180">Contributor</span></span> |<span data-ttu-id="48ff7-181">Lehetővé teszi az hello sablonerőforrás szerkesztését és törlését.</span><span class="sxs-lookup"><span data-stu-id="48ff7-181">Allows Edit and Delete permission on hello Template resource.</span></span> <span data-ttu-id="48ff7-182">Felhasználó nem hello sablon megosztása másokkal</span><span class="sxs-lookup"><span data-stu-id="48ff7-182">User cannot Share hello Template with others</span></span> |

<span data-ttu-id="48ff7-183">Válassza ki **megosztás** hello Tallózás elem jobb gombbal kattintva vagy az adott elem hello megtekintési paneljén.</span><span class="sxs-lookup"><span data-stu-id="48ff7-183">Select **Share** on hello browse item by right clicking or on hello view blade of a specific item.</span></span> <span data-ttu-id="48ff7-184">Ekkor elindul a Megosztás folyamat.</span><span class="sxs-lookup"><span data-stu-id="48ff7-184">This launches a Share experience.</span></span>

![Sablon megosztása](media/share-template-portal1a.png)  <br />

 <span data-ttu-id="48ff7-186">Válasszon egy szerepkör és egy felhasználó vagy csoport tooprovide hozzáférés tooa adott **sablon**.</span><span class="sxs-lookup"><span data-stu-id="48ff7-186">You can now choose a role and a user or group tooprovide access tooa particular **Template**.</span></span> <span data-ttu-id="48ff7-187">hello szerepkörök választhatók: tulajdonos, olvasó és közreműködő.</span><span class="sxs-lookup"><span data-stu-id="48ff7-187">hello available roles are Owner, Reader and Contributor.</span></span> <span data-ttu-id="48ff7-188">További részletek a hello [hozzáférés-vezérlés](#access-control-for-a-tenant-resource-provider) fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="48ff7-188">More details in hello [access control](#access-control-for-a-tenant-resource-provider) section above.</span></span>

![Sablon megosztása](media/share-template-portal2b.png)  <br />

![Sablon megosztása](media/share-template-portal3b.png)  <br />

<span data-ttu-id="48ff7-191">Kattintson a **Kijelölés**, majd az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="48ff7-191">Click **Select** and **Ok**.</span></span> <span data-ttu-id="48ff7-192">Most már megtekintheti hello felhasználók vagy csoportok hozzáadott toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="48ff7-192">You can now see hello users or groups you added toohello resource.</span></span>

![Sablon megosztása](media/share-template-portal4b.png)  <br />

> [!NOTE]
> <span data-ttu-id="48ff7-194">A sablon csak meg lehet osztani a felhasználók és csoportok hello azonos Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="48ff7-194">A Template can only be shared with users and groups in hello same Azure Active Directory tenant.</span></span> <span data-ttu-id="48ff7-195">Ha meg megosztani a sablont, amely nincs az Ön bérelt szolgáltatásának e-mail címmel, meghívót küldi hello felhasználói toojoin hello bérlői vendégként kéri.</span><span class="sxs-lookup"><span data-stu-id="48ff7-195">If you share a Template with an email address that is not in your tenant, an invitation will be sent asking hello user toojoin hello tenant as a guest.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="48ff7-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48ff7-196">Next steps</span></span>
* <span data-ttu-id="48ff7-197">toolearn Resource Manager-sablonok létrehozásával kapcsolatban lásd: [sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="48ff7-197">toolearn about creating Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="48ff7-198">a Resource Manager-sablonokban használható toounderstand hello függvények lásd [sablonfüggvények](../azure-resource-manager/resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="48ff7-198">toounderstand hello functions you can use in an Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md)</span></span>
* <span data-ttu-id="48ff7-199">A sablonok kialakításával kapcsolatos útmutatásért lásd: [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md) (Azure Resource Manager-sablonok tervezésének ajánlott eljárásai)</span><span class="sxs-lookup"><span data-stu-id="48ff7-199">For guidance on designing your templates, see [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md)</span></span>

