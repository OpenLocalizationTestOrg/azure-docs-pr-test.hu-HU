---
title: "Bevezetés a magánsablonok használatába | Microsoft Docs"
description: "Magánsablonok hozzáadása, kezelése és megosztása az Azure portál, az Azure parancssori felülete, illetve a PowerShell segítségével."
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
ms.openlocfilehash: 01657619cbe579c6818a790cc3ab95a33936a565
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a><span data-ttu-id="2c0aa-103">Bevezetés a magánsablonok használatába az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="2c0aa-103">Get started with private Templates on the Azure Portal</span></span>
<span data-ttu-id="2c0aa-104">Az [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sablonjai olyan deklaratív sablonok, amelyek az üzemelő példány definiálására használatosak.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-104">An [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) template is a declarative template used to define your deployment.</span></span> <span data-ttu-id="2c0aa-105">Meghatározhatja az adott megoldáshoz üzembe helyezendő erőforrásokat, valamint megadhatja azokat a paramétereket és változókat, amelyek segítségével beviheti a különböző környezetekhez tartozó értékeket.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-105">You can define the resources to deploy for a solution, and specify parameters and variables that enable you to input values for different environments.</span></span> <span data-ttu-id="2c0aa-106">A sablon JSON-okból és kifejezésekből áll, amelyek segítségével kialakíthatja az üzemelő példány értékeit.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-106">The template consists of JSON and expressions which you can use to construct values for your deployment.</span></span>

<span data-ttu-id="2c0aa-107">Az [Azure Portal](https://portal.azure.com) új **Sablonok** funkciója, valamint a **Microsoft.Gallery** erőforrás-szolgáltató az [Azure Piactér](https://azure.microsoft.com/marketplace/) bővítményeként használható, amelynek segítségével a felhasználók saját könyvtárukból származó magánsablonokat hozhatnak létre, kezelhetnek és helyezhetnek üzembe.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-107">You can use the new **Templates** capability in the [Azure Portal](https://portal.azure.com) along with the **Microsoft.Gallery** resource provider as an extension of the [Azure Marketplace](https://azure.microsoft.com/marketplace/) to enable users to create, manage and deploy private templates from a personal library.</span></span>

<span data-ttu-id="2c0aa-108">Ebben a dokumentumban bemutatjuk, hogyan adhat hozzá, kezelhet és oszthat meg **magánsablonokat** az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-108">This document walks you through adding, managing and sharing a private **Template** using the Azure Portal.</span></span>

## <a name="guidance"></a><span data-ttu-id="2c0aa-109">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="2c0aa-109">Guidance</span></span>
<span data-ttu-id="2c0aa-110">A következő javaslatok segítségével teljes mértékben kihasználhatja a **sablonok** előnyeit a megoldásaival való munkavégzés során.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-110">The following suggestions will help you take full advantage of **Templates** when working with your solutions:</span></span>

* <span data-ttu-id="2c0aa-111">A **sablonok** beágyazott erőforrások, amelyek egy Resource Manager-sablont, illetve további metaadatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-111">A **Template** is an encapsulating resource that contains an Resource Manager template and additional metadata.</span></span> <span data-ttu-id="2c0aa-112">Hasonlóan viselkednek, mint a Piactér elemei.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-112">It behaves very similarly to an item in the Marketplace.</span></span> <span data-ttu-id="2c0aa-113">A legfontosabb különbség, hogy a Piactér elemei nyilvánosak, míg ezek a sablonok privát felhasználásra szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-113">The key difference is that it is a private item as opposed to the public Marketplace items.</span></span>
* <span data-ttu-id="2c0aa-114">A **Sablonok** könyvtár hasznos segítséget nyújt a felhasználóknak üzemelő példányaik testre szabásában.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-114">The **Templates** library works well for users who need to customize their deployments.</span></span>
* <span data-ttu-id="2c0aa-115">A **sablonok** egyszerű Azure-beli tárházat biztosítanak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-115">**Templates** work well for users who need a simple repository within Azure.</span></span>
* <span data-ttu-id="2c0aa-116">Kezdje egy meglévő Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-116">Start with an existing Resource Manager template.</span></span> <span data-ttu-id="2c0aa-117">Keresse meg a kívánt sablont a [GitHubon](https://github.com/Azure/azure-quickstart-templates), vagy [exportálja a sablont](../azure-resource-manager/resource-manager-export-template.md) egy meglévő erőforráscsoportból.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-117">Find templates in [github](https://github.com/Azure/azure-quickstart-templates) or [Export template](../azure-resource-manager/resource-manager-export-template.md) from an existing resource group.</span></span>
* <span data-ttu-id="2c0aa-118">A **sablonok** ahhoz a felhasználóhoz kötődnek, aki közzéteszi őket.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-118">**Templates** are tied to the user who publishes them.</span></span> <span data-ttu-id="2c0aa-119">Az olvasási hozzáféréssel rendelkezők szabadon megtekinthetik a közzétevő nevét.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-119">The publisher name is visible to everyone who has read access to it.</span></span>
* <span data-ttu-id="2c0aa-120">A **sablonok** a Resource Managerhez tartozó erőforrások, amelyeket közzététel után nem lehet átnevezni.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-120">**Templates** are Resource Manager resources and cannot be renamed once published.</span></span>

## <a name="add-a-template-resource"></a><span data-ttu-id="2c0aa-121">Sablonerőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2c0aa-121">Add a Template resource</span></span>
<span data-ttu-id="2c0aa-122">Az Azure Portalon két módszer áll rendelkezésre **sablonerőforrás** létrehozására.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-122">There are two ways to create a **Template** resource in the Azure portal.</span></span>

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a><span data-ttu-id="2c0aa-123">1. módszer: Új sablonerőforrás létrehozása már futó erőforráscsoportból</span><span class="sxs-lookup"><span data-stu-id="2c0aa-123">Method 1 : Create a new Template resource from a running resource group</span></span>
1. <span data-ttu-id="2c0aa-124">Nyisson meg egy meglévő erőforráscsoportot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-124">Navigate to an existing resource group on the Azure Portal.</span></span> <span data-ttu-id="2c0aa-125">A **Beállítások** menüben válassza a **Sablon exportálása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-125">Select **Export template** in **Settings**.</span></span>
2. <span data-ttu-id="2c0aa-126">A Resource Manager-sablon exportálását követően használja a **Sablon mentése** gombot az exportált elemnek a **Sablonok** tárházba mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-126">Once the Resource Manager template is exported, use the **Save Template** button to save it to the **Templates** repository.</span></span> <span data-ttu-id="2c0aa-127">A Sablon exportálása funkcióról részletes leírást [itt](../azure-resource-manager/resource-manager-export-template.md) talál.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-127">Find complete details for Export template [here](../azure-resource-manager/resource-manager-export-template.md).</span></span>
   <br /><br /><span data-ttu-id="2c0aa-128">
   ![Erőforráscsoport exportálása](media/rg-export-portal1.PNG)</span><span class="sxs-lookup"><span data-stu-id="2c0aa-128">
![Resource group export](media/rg-export-portal1.PNG)</span></span>  <br />
3. <span data-ttu-id="2c0aa-129">Kattintson a **Save to Template** (Mentés sablonba) parancsgombra.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-129">Select the **Save to Template** command button.</span></span>
   <br /><br />
4. <span data-ttu-id="2c0aa-130">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="2c0aa-130">Enter the following information:</span></span>
   
   * <span data-ttu-id="2c0aa-131">Név – A sablonobjektum neve (MEGJEGYZÉS: ez a név az Azure Resource Manageren alapul.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-131">Name – Name of the template object (NOTE: This is an Azure Resource Manager based name.</span></span> <span data-ttu-id="2c0aa-132">Így minden vonatkozó elnevezési korlátozás érvényes rá, és létrehozását követően nem módosítható).</span><span class="sxs-lookup"><span data-stu-id="2c0aa-132">All naming restrictions apply and it cannot be changed once created).</span></span>
   * <span data-ttu-id="2c0aa-133">Leírás – A sablon rövid ismertetése.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-133">Description – Quick summary about the template.</span></span>
     
     ![Sablon mentése](media/save-template-portal1.PNG)  <br />
5. <span data-ttu-id="2c0aa-135">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-135">Click **Save**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2c0aa-136">A Sablon exportálása panelen értesítések jelennek meg, ha a rendszer hibát talál az exportált Resource Manager-sablonban. A Resource Manager-sablon azonban ebben az esetben is menthető a Sablonok tárházba.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-136">The Export template blade shows notifications when the exported Resource Manager template has errors, but you will still be able to save this Resource Manager template to the Templates.</span></span> <span data-ttu-id="2c0aa-137">Az exportált Resource Manager-sablon ismételt üzembe helyezése előtt ellenőrizze, és szükség esetén javítsa ki a Resource Manager-sablonnal kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-137">Ensure that you check and fix any Resource Manager template issues before redeploying the exported Resource Manager template.</span></span>
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a><span data-ttu-id="2c0aa-138">2. módszer: Új sablonerőforrás hozzáadása tallózással</span><span class="sxs-lookup"><span data-stu-id="2c0aa-138">Method 2 : Add a new Template resource from browse</span></span>
<span data-ttu-id="2c0aa-139">Beállításokat még nem tartalmazó **sablont** is hozzáadhat. Ehhez kattintson a **Tallózás > Sablonok** menüpontban elérhető +Hozzáadás parancsgombra.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-139">You can also add a new **Template** from scratch using the +Add command button in **Browse > Templates**.</span></span> <span data-ttu-id="2c0aa-140">Meg kell adnia a nevet, a leírást és a Resource Manager-sablon JSON-ját.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-140">You will need to provide a Name, Description and the Resource Manager template JSON.</span></span>

![Sablon hozzáadása](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> <span data-ttu-id="2c0aa-142">A Microsoft.Gallery egy bérlőalapú Azure-erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-142">Microsoft.Gallery is a Tenant based Azure resource provider.</span></span> <span data-ttu-id="2c0aa-143">A sablonerőforrás kötődik ahhoz a felhasználóhoz kötődik, aki létrehozta.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-143">The Template resource is tied to the user who created it.</span></span> <span data-ttu-id="2c0aa-144">Nem kötődik azonban előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-144">It is not tied to any specific subscription.</span></span> <span data-ttu-id="2c0aa-145">Előfizetést csak a sablon üzembe helyezésekor kell választani.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-145">A subscription needs to be chosen only when deploying a Template.</span></span>
> 
> 

## <a name="view-template-resources"></a><span data-ttu-id="2c0aa-146">Sablonerőforrás megtekintése</span><span class="sxs-lookup"><span data-stu-id="2c0aa-146">View Template resources</span></span>
<span data-ttu-id="2c0aa-147">A **Tallózás > Sablonok** menüben az összes elérhető **sablon** megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-147">All **Templates** available to you can be seen at **Browse > Templates**.</span></span> <span data-ttu-id="2c0aa-148">Itt az Ön által létrehozott **sablonok**, valamint az Önnel különböző szintű engedélyekkel megosztott sablonok egyaránt megjelennek.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-148">This includes **Templates** you have created as well as ones that have been shared with you with varying levels of permissions.</span></span> <span data-ttu-id="2c0aa-149">További részletek a [hozzáférés-vezérlésre](#access-control-for-a-tenant-resource-provider) vonatkozó alábbi szakaszban olvashatók.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-149">More details in the [access control](#access-control-for-a-tenant-resource-provider) section below.</span></span>

![Sablon megtekintése](media/view-template-portal1.PNG)  <br />

<span data-ttu-id="2c0aa-151">A **sablon** részletes adatainak megtekintéséhez kattintson a lista kívánt elemére.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-151">You can view the details of a **Template** by clicking into an item in the list.</span></span>

![Sablon megtekintése](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a><span data-ttu-id="2c0aa-153">Sablonerőforrás módosítása</span><span class="sxs-lookup"><span data-stu-id="2c0aa-153">Edit a Template resource</span></span>
<span data-ttu-id="2c0aa-154">A **sablon** szerkesztési folyamatának elindításához kattintson jobb gombbal a kívánt elemre a Tallózás listában, vagy válassza a Szerkesztés parancsgombot.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-154">You can initiate the edit flow for a **Template** by right clicking the item on the Browse list or by choosing the Edit command button.</span></span>

![Sablon szerkesztése](media/edit-template-portal1a.PNG)  <br />

<span data-ttu-id="2c0aa-156">Módosíthatja a leírást vagy a Resource Manager-sablon szövegét.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-156">You can edit the description or Resource Manager template text.</span></span> <span data-ttu-id="2c0aa-157">A nevet nem változtathatja meg, mivel az a Resource Manager-erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-157">You cannot edit the name since it is an Resource Manager resource name.</span></span> <span data-ttu-id="2c0aa-158">Ha átírja a Resource Manager-sablon JSON-ját, a Microsoft ellenőrzi, hogy érvényes JSON maradt-e.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-158">When you edit the Resource Manager template JSON we will validate to ensure that it is valid JSON.</span></span> <span data-ttu-id="2c0aa-159">A módosított sablon mentéséhez kattintson az **OK**, majd a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-159">Choose **OK** and then **Save** to save your updated template.</span></span>

![Sablon szerkesztése](media/edit-template-portal2a.PNG)  <br />

<span data-ttu-id="2c0aa-161">A **sablon** mentését követően megerősítési értesítés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-161">Once the **Template** is saved you will see a confirmation notification.</span></span>

![Sablon szerkesztése](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a><span data-ttu-id="2c0aa-163">Sablonerőforrás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="2c0aa-163">Deploy a Template resource</span></span>
<span data-ttu-id="2c0aa-164">Bármely **sablon** üzembe helyezhető, amelyhez **olvasási** engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-164">You can deploy any **Template** that you have **Read** permissions on.</span></span> <span data-ttu-id="2c0aa-165">Az üzembe helyezési folyamat elindítja az Azure standard sablon-üzembe helyezési paneljét.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-165">The deployment flow launches the standard Azure Template deployment blade.</span></span> <span data-ttu-id="2c0aa-166">Az üzembe helyezés folytatásához adja meg a Resource Manager-sablon paramétereihez tartozó értékeket.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-166">Fill out the values for the Resource Manager template parameters to proceed with the deployment.</span></span>

![Sablon üzembe helyezése](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a><span data-ttu-id="2c0aa-168">Sablonerőforrás megosztása</span><span class="sxs-lookup"><span data-stu-id="2c0aa-168">Share a Template resource</span></span>
<span data-ttu-id="2c0aa-169">A **sablonerőforrást** megoszthatja kollégáival.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-169">A **Template** resource can be shared with your peers.</span></span> <span data-ttu-id="2c0aa-170">A megosztás hasonlóan működik, mint [a szerepkör-hozzárendelés az Azure-erőforrásoknál](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="2c0aa-170">Sharing behaves similarly to [role assignment for any resource on Azure](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="2c0aa-171">A **sablon** tulajdonosa engedélyt ad a többi felhasználónak, akik így műveleteket végezhetnek a sablonerőforrással.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-171">The **Template** owner provides permissions to other users who can interact with a Template resource.</span></span> <span data-ttu-id="2c0aa-172">Azok a személyek vagy csoportok, akikkel megosztja a **sablont**, jogosulttá válnak a Resource Manager-sablon és a hozzá tartozó katalógusbeli tulajdonságok megtekintésére.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-172">The person or group of people you share the **Template** with will be able to see the Resource Manager template and its gallery properties.</span></span>

### <a name="access-control-for-the-microsoftgallery-resources"></a><span data-ttu-id="2c0aa-173">A Microsoft.Gallery erőforrások hozzáférés-vezérlése</span><span class="sxs-lookup"><span data-stu-id="2c0aa-173">Access control for the Microsoft.Gallery resources</span></span>
| <span data-ttu-id="2c0aa-174">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="2c0aa-174">Role</span></span> | <span data-ttu-id="2c0aa-175">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="2c0aa-175">Permissions</span></span> |
| --- | --- |
| <span data-ttu-id="2c0aa-176">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="2c0aa-176">Owner</span></span> |<span data-ttu-id="2c0aa-177">Lehetővé teszi a sablonerőforrás teljes körű ellenőrzését, ideértve a megosztást is</span><span class="sxs-lookup"><span data-stu-id="2c0aa-177">Allows full control on the Template resource including Share</span></span> |
| <span data-ttu-id="2c0aa-178">Olvasó</span><span class="sxs-lookup"><span data-stu-id="2c0aa-178">Reader</span></span> |<span data-ttu-id="2c0aa-179">Lehetővé teszi a sablonerőforrás olvasását és futtatását (üzembe helyezését)</span><span class="sxs-lookup"><span data-stu-id="2c0aa-179">Allows Read and Execute(Deploy) on the Template resource</span></span> |
| <span data-ttu-id="2c0aa-180">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="2c0aa-180">Contributor</span></span> |<span data-ttu-id="2c0aa-181">Lehetővé teszi a sablonerőforrás szerkesztését és törlését.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-181">Allows Edit and Delete permission on the Template resource.</span></span> <span data-ttu-id="2c0aa-182">A felhasználó nem oszthatja meg másokkal a sablont</span><span class="sxs-lookup"><span data-stu-id="2c0aa-182">User cannot Share the Template with others</span></span> |

<span data-ttu-id="2c0aa-183">Kattintson jobb gombbal a kívánt elemre, és válassza a **Megosztás** lehetőséget, vagy kattintson erre a lehetőségre az adott elem megtekintési paneljén.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-183">Select **Share** on the browse item by right clicking or on the view blade of a specific item.</span></span> <span data-ttu-id="2c0aa-184">Ekkor elindul a Megosztás folyamat.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-184">This launches a Share experience.</span></span>

![Sablon megosztása](media/share-template-portal1a.png)  <br />

 <span data-ttu-id="2c0aa-186">A **sablonhoz** való hozzáférés biztosítása érdekében válasszon egy felhasználót vagy csoportot, majd adja meg a kívánt szerepkört.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-186">You can now choose a role and a user or group to provide access to a particular **Template**.</span></span> <span data-ttu-id="2c0aa-187">A következő szerepkörök választhatók: Tulajdonos, Olvasó és Közreműködő.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-187">The available roles are Owner, Reader and Contributor.</span></span> <span data-ttu-id="2c0aa-188">További részletek a [hozzáférés-vezérlésre](#access-control-for-a-tenant-resource-provider) vonatkozó fenti szakaszban olvashatók.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-188">More details in the [access control](#access-control-for-a-tenant-resource-provider) section above.</span></span>

![Sablon megosztása](media/share-template-portal2b.png)  <br />

![Sablon megosztása](media/share-template-portal3b.png)  <br />

<span data-ttu-id="2c0aa-191">Kattintson a **Kijelölés**, majd az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-191">Click **Select** and **Ok**.</span></span> <span data-ttu-id="2c0aa-192">A megjelenő képernyőn láthatja az erőforráshoz adott felhasználókat és csoportokat.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-192">You can now see the users or groups you added to the resource.</span></span>

![Sablon megosztása](media/share-template-portal4b.png)  <br />

> [!NOTE]
> <span data-ttu-id="2c0aa-194">A sablont kizárólag az azonos Azure Active Directory-bérlő alá tartozó felhasználókkal és csoportokkal oszthatja meg.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-194">A Template can only be shared with users and groups in the same Azure Active Directory tenant.</span></span> <span data-ttu-id="2c0aa-195">Ha olyan e-mail-címmel rendelkező felhasználóval próbálja meg megosztani a sablont, aki nem az Ön bérlőjéhez tartozik, a rendszer meghívót küld a felhasználónak, amelyben felkéri, hogy csatlakozzon vendégként az Ön bérlőjéhez.</span><span class="sxs-lookup"><span data-stu-id="2c0aa-195">If you share a Template with an email address that is not in your tenant, an invitation will be sent asking the user to join the tenant as a guest.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2c0aa-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c0aa-196">Next steps</span></span>
* <span data-ttu-id="2c0aa-197">A Resource Manager-sablonok létrehozásával kapcsolatos további információk: [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md) (Sablonok készítése)</span><span class="sxs-lookup"><span data-stu-id="2c0aa-197">To learn about creating Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="2c0aa-198">A Resource Manager-sablonokban használható függvények ismertetése: [Template functions](../azure-resource-manager/resource-group-template-functions.md) (Sablonfüggvények)</span><span class="sxs-lookup"><span data-stu-id="2c0aa-198">To understand the functions you can use in an Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md)</span></span>
* <span data-ttu-id="2c0aa-199">A sablonok kialakításával kapcsolatos útmutatásért lásd: [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md) (Azure Resource Manager-sablonok tervezésének ajánlott eljárásai)</span><span class="sxs-lookup"><span data-stu-id="2c0aa-199">For guidance on designing your templates, see [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md)</span></span>

