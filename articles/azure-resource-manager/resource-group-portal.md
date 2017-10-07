---
title: "az Azure portál toomanage aaaUse Azure-erőforrások |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager toomanage az erőforrások használatára. Bemutatja, hogyan toowork irányítópultok toomonitor erőforrásokkal."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 0c89a197a31c5b6dd03ba457cb4d1fdf9f6d00f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-resources-through-portal"></a><span data-ttu-id="dd021-104">Azure-portálon keresztül erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="dd021-104">Manage Azure resources through portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dd021-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd021-105">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="dd021-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="dd021-106">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="dd021-107">Portál</span><span class="sxs-lookup"><span data-stu-id="dd021-107">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="dd021-108">REST API</span><span class="sxs-lookup"><span data-stu-id="dd021-108">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="dd021-109">Ez a témakör bemutatja, hogyan toouse hello [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) toomanage az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="dd021-109">This topic shows how toouse hello [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) toomanage your Azure resources.</span></span> <span data-ttu-id="dd021-110">toolearn hello portálon keresztül erőforrások telepítésével kapcsolatban lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-110">toolearn about deploying resources through hello portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>

<span data-ttu-id="dd021-111">Nem minden szolgáltatás jelenleg hello portálon vagy az erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="dd021-111">Currently, not every service supports hello portal or Resource Manager.</span></span> <span data-ttu-id="dd021-112">Ezek a szolgáltatások toouse hello kell [klasszikus portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="dd021-112">For those services, you need toouse hello [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="dd021-113">Minden szolgáltatás hello állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="dd021-113">For hello status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="manage-resource-groups"></a><span data-ttu-id="dd021-114">Erőforrás-csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="dd021-114">Manage resource groups</span></span>

<span data-ttu-id="dd021-115">Egy erőforráscsoport egy olyan tároló, amely egy Azure megoldás kapcsolódó erőforrásokat tárol.</span><span class="sxs-lookup"><span data-stu-id="dd021-115">A resource group is a container that holds related resources for an Azure solution.</span></span> <span data-ttu-id="dd021-116">hello erőforráscsoport összes hello erőforrások hello megoldás, és csak azokat az erőforrásokat, amelyet az egy csoportként toomanage tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="dd021-116">hello resource group can include all hello resources for hello solution, or only those resources that you want toomanage as a group.</span></span> <span data-ttu-id="dd021-117">Úgy dönt, hogy hogyan tooallocate erőforrások tooresource csoportok alapján hasznossá hello a legtöbb, a szervezet számára legjobb.</span><span class="sxs-lookup"><span data-stu-id="dd021-117">You decide how you want tooallocate resources tooresource groups based on what makes hello most sense for your organization.</span></span> <span data-ttu-id="dd021-118">Általában a megosztás hello erőforrásokat adjon hozzá azonos életciklus toohello azonos erőforráscsoport, így könnyen központi telepítése, frissítése és csoportként törölje őket.</span><span class="sxs-lookup"><span data-stu-id="dd021-118">Generally, add resources that share hello same lifecycle toohello same resource group so you can easily deploy, update, and delete them as a group.</span></span> 

<span data-ttu-id="dd021-119">hello erőforráscsoport hello erőforrások metaadatokat tárol.</span><span class="sxs-lookup"><span data-stu-id="dd021-119">hello resource group stores metadata about hello resources.</span></span> <span data-ttu-id="dd021-120">Ezért amikor hello erőforrásnak a helyét adja meg, meg, hogy a metaadatokat tároló.</span><span class="sxs-lookup"><span data-stu-id="dd021-120">Therefore, when you specify a location for hello resource group, you are specifying where that metadata is stored.</span></span> <span data-ttu-id="dd021-121">Megfelelőségi okokból, szükség lehet a tooensure, amely az adott tárolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="dd021-121">For compliance reasons, you may need tooensure that your data is stored in a particular region.</span></span>

1. <span data-ttu-id="dd021-122">toosee az előfizetésében szereplő összes hello erőforráscsoport kiválasztása **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="dd021-122">toosee all hello resource groups in your subscription, select **Resource groups**.</span></span>
   
    ![Keresse meg az erőforráscsoport-sablonok](./media/resource-group-portal/browse-groups.png)
2. <span data-ttu-id="dd021-124">egy üres erőforráscsoporthoz toocreate kiválasztása **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="dd021-124">toocreate an empty resource group, select **Add**.</span></span>
   
    ![csoport hozzáadása](./media/resource-group-portal/add-resource-group.png)
3. <span data-ttu-id="dd021-126">Adjon meg egy új erőforráscsoportot hello helyét és típusát.</span><span class="sxs-lookup"><span data-stu-id="dd021-126">Provide a name and location for hello new resource group.</span></span> <span data-ttu-id="dd021-127">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dd021-127">Select **Create**.</span></span>
   
    ![Erőforráscsoport létrehozása](./media/resource-group-portal/create-empty-group.png)
4. <span data-ttu-id="dd021-129">Előfordulhat, hogy tooselect **frissítése** toosee hello nemrég létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="dd021-129">You may need tooselect **Refresh** toosee hello recently created resource group.</span></span>
   
    ![erőforrás-csoport frissítése](./media/resource-group-portal/refresh-resource-groups.png)
5. <span data-ttu-id="dd021-131">Az erőforráscsoportok megjelenő toocustomize hello információ kiválasztása **oszlopok**.</span><span class="sxs-lookup"><span data-stu-id="dd021-131">toocustomize hello information displayed for your resource groups, select **Columns**.</span></span>
   
    ![oszlop testreszabása](./media/resource-group-portal/select-columns.png)
6. <span data-ttu-id="dd021-133">Hello oszlopok tooadd, majd válassza ki és **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="dd021-133">Select hello columns tooadd, and then select **Update**.</span></span>
   
    ![oszlopok hozzáadása](./media/resource-group-portal/add-columns.png)
7. <span data-ttu-id="dd021-135">toolearn erőforrások tooyour új erőforráscsoportot, telepítésével kapcsolatban lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-135">toolearn about deploying resources tooyour new resource group, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
8. <span data-ttu-id="dd021-136">A gyors hozzáférés tooa erőforráscsoport PIN-kód hello panel tooyour irányítópult.</span><span class="sxs-lookup"><span data-stu-id="dd021-136">For quick access tooa resource group, you can pin hello blade tooyour dashboard.</span></span>
   
    ![PIN-kód erőforráscsoport](./media/resource-group-portal/pin-group.png)
9. <span data-ttu-id="dd021-138">hello irányítópult hello erőforráscsoport és az erőforrások jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="dd021-138">hello dashboard displays hello resource group and its resources.</span></span> <span data-ttu-id="dd021-139">Kiválaszthatja a hello erőforráscsoportok vagy az erőforrások toonavigate toohello elemek bármelyikét.</span><span class="sxs-lookup"><span data-stu-id="dd021-139">You can select either hello resource groups or any of its resources toonavigate toohello item.</span></span>
   
    ![PIN-kód erőforráscsoport](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a><span data-ttu-id="dd021-141">Címke erőforrások</span><span class="sxs-lookup"><span data-stu-id="dd021-141">Tag resources</span></span>
<span data-ttu-id="dd021-142">Címkék tooresource csoportok alkalmazhat, és erőforrások toologically rendezze az eszközök.</span><span class="sxs-lookup"><span data-stu-id="dd021-142">You can apply tags tooresource groups and resources toologically organize your assets.</span></span> <span data-ttu-id="dd021-143">A címkék használatáról információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-143">For information about working with tags, see [Using tags tooorganize your Azure resources](resource-group-using-tags.md).</span></span>

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a><span data-ttu-id="dd021-144">Erőforrások megfigyelése</span><span class="sxs-lookup"><span data-stu-id="dd021-144">Monitor resources</span></span>
<span data-ttu-id="dd021-145">Amikor kiválaszt egy erőforrást, hello erőforráspanelen alapértelmezett diagramjait és az adott erőforrástípus figyelési táblák mutatja be.</span><span class="sxs-lookup"><span data-stu-id="dd021-145">When you select a resource, hello resource blade presents default graphs and tables for monitoring that resource type.</span></span>

1. <span data-ttu-id="dd021-146">Válassza ki az erőforrás és a hirdetmény hello **figyelés** szakasz.</span><span class="sxs-lookup"><span data-stu-id="dd021-146">Select a resource and notice hello **Monitoring** section.</span></span> <span data-ttu-id="dd021-147">Ez magában foglalja, amelyek megfelelő toohello erőforrástípus diagramjait.</span><span class="sxs-lookup"><span data-stu-id="dd021-147">It includes graphs that are relevant toohello resource type.</span></span> <span data-ttu-id="dd021-148">hello következő kép bemutatja hello alapértelmezett figyelési adatok tárfiókok esetén.</span><span class="sxs-lookup"><span data-stu-id="dd021-148">hello following image shows hello default monitoring data for a storage account.</span></span>
   
    ![figyelési megjelenítése](./media/resource-group-portal/show-monitoring.png)
2. <span data-ttu-id="dd021-150">Hello panel tooyour irányítópult szakasz rögzíthető hello három ponttal (…) hello szakasz fölé kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="dd021-150">You can pin a section of hello blade tooyour dashboard by selecting hello ellipsis (...) above hello section.</span></span> <span data-ttu-id="dd021-151">Hello mérete hello szakasz hello panelen testreszabásával, vagy távolítsa el teljesen.</span><span class="sxs-lookup"><span data-stu-id="dd021-151">You can also customize hello size hello section in hello blade or remove it completely.</span></span> <span data-ttu-id="dd021-152">hello következő kép bemutatja, hogyan toopin, testre szabhatja, vagy távolítsa el a hello CPU és memória szakasz.</span><span class="sxs-lookup"><span data-stu-id="dd021-152">hello following image shows how toopin, customize, or remove hello CPU and Memory section.</span></span>
   
    ![PIN-kód szakasz](./media/resource-group-portal/pin-cpu-section.png)
3. <span data-ttu-id="dd021-154">Miután hello szakasz toohello irányítópult, látni fogja az összefoglaló hello hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="dd021-154">After pinning hello section toohello dashboard, you will see hello summary on hello dashboard.</span></span> <span data-ttu-id="dd021-155">És lehetőséget azonnal viszi toomore hello adatok részleteit.</span><span class="sxs-lookup"><span data-stu-id="dd021-155">And, selecting it immediately takes you toomore details about hello data.</span></span>
   
    ![Irányítópult nézet](./media/resource-group-portal/view-startboard.png)
4. <span data-ttu-id="dd021-157">toocompletely testreszabása hello adatok hello portálon keresztül figyelheti, keresse meg a tooyour alapértelmezett irányítópultot, és válassza ki **új irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="dd021-157">toocompletely customize hello data you monitor through hello portal, navigate tooyour default dashboard, and select **New dashboard**.</span></span>
   
    ![irányítópult](./media/resource-group-portal/dashboard.png)
5. <span data-ttu-id="dd021-159">Nevezze el az új irányítópult és csempék húzza hello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="dd021-159">Give your new dashboard a name and drag tiles onto hello dashboard.</span></span> <span data-ttu-id="dd021-160">hello csempék különböző beállítások szerint vannak szűrve.</span><span class="sxs-lookup"><span data-stu-id="dd021-160">hello tiles are filtered by different options.</span></span>
   
    ![irányítópult](./media/resource-group-portal/create-dashboard.png)
   
     <span data-ttu-id="dd021-162">toolearn irányítópultok, használatával kapcsolatban lásd: [létrehozása és az Azure-portálon hello irányítópultok megosztása](../azure-portal/azure-portal-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-162">toolearn about working with dashboards, see [Creating and sharing dashboards in hello Azure portal](../azure-portal/azure-portal-dashboards.md).</span></span>

## <a name="manage-resources"></a><span data-ttu-id="dd021-163">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="dd021-163">Manage resources</span></span>
<span data-ttu-id="dd021-164">Erőforrás hello panelen látható hello beállítások kezeléséhez hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="dd021-164">In hello blade for a resource, you see hello options for managing hello resource.</span></span> <span data-ttu-id="dd021-165">hello portál megjeleníti a felügyeleti beállításokat az adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="dd021-165">hello portal presents management options for that particular resource type.</span></span> <span data-ttu-id="dd021-166">Hello parancsok hello erőforráspanelen és hello bal oldalán hello tetején megjelenik.</span><span class="sxs-lookup"><span data-stu-id="dd021-166">You see hello management commands across hello top of hello resource blade and on hello left side.</span></span>

![Erőforrások kezelése](./media/resource-group-portal/manage-resources.png)

<span data-ttu-id="dd021-168">Ezek a beállítások műveletek, például a indítása és leállítása a virtuális gép vagy hello virtuális gép tulajdonságainak hello újrakonfigurálása végezheti el.</span><span class="sxs-lookup"><span data-stu-id="dd021-168">From these options, you can perform operations such as starting and stopping a virtual machine, or reconfiguring hello properties of hello virtual machine.</span></span>

## <a name="move-resources"></a><span data-ttu-id="dd021-169">Erőforrások áthelyezése</span><span class="sxs-lookup"><span data-stu-id="dd021-169">Move resources</span></span>
<span data-ttu-id="dd021-170">Ha toomove erőforrások tooanother erőforráscsoport vagy egy másik előfizetés van szüksége, tekintse meg [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-170">If you need toomove resources tooanother resource group or another subscription, see [Move resources toonew resource group or subscription](resource-group-move-resources.md).</span></span>

## <a name="lock-resources"></a><span data-ttu-id="dd021-171">Erőforrások zárolása</span><span class="sxs-lookup"><span data-stu-id="dd021-171">Lock resources</span></span>
<span data-ttu-id="dd021-172">Zárolhatja előfizetést, erőforráscsoporthoz vagy erőforrás tooprevent más felhasználók véletlen törlése vagy a kritikus erőforrásokat módosítása a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="dd021-172">You can lock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="dd021-173">További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-173">For more information, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a><span data-ttu-id="dd021-174">Az előfizetés és a költségek megtekintése</span><span class="sxs-lookup"><span data-stu-id="dd021-174">View your subscription and costs</span></span>
<span data-ttu-id="dd021-175">Az erőforrások előfizetését és hello összegzett költségeinek kapcsolatos információk is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="dd021-175">You can view information about your subscription and hello rolled-up costs for all your resources.</span></span> <span data-ttu-id="dd021-176">Válassza ki **előfizetések** és azt szeretné, hogy toosee hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="dd021-176">Select **Subscriptions** and hello subscription you want toosee.</span></span> <span data-ttu-id="dd021-177">Csak előfordulhat, hogy egy előfizetés tooselect.</span><span class="sxs-lookup"><span data-stu-id="dd021-177">You might only have one subscription tooselect.</span></span>

![előfizetést](./media/resource-group-portal/select-subscription.png)

<span data-ttu-id="dd021-179">Belül hello előfizetés panelen megjelenik egy írási sebesség.</span><span class="sxs-lookup"><span data-stu-id="dd021-179">Within hello subscription blade, you see a burn rate.</span></span>

![Írás gyakorisága](./media/resource-group-portal/burn-rate.png)

<span data-ttu-id="dd021-181">És erőforrástípusok szerint költségeket.</span><span class="sxs-lookup"><span data-stu-id="dd021-181">And, a breakdown of costs by resource type.</span></span>

![Erőforrás költség](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a><span data-ttu-id="dd021-183">Sablon exportálása</span><span class="sxs-lookup"><span data-stu-id="dd021-183">Export template</span></span>
<span data-ttu-id="dd021-184">Miután beállította az erőforráscsoport, érdemes lehet az tooview hello Resource Manager-sablon hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="dd021-184">After setting up your resource group, you may want tooview hello Resource Manager template for hello resource group.</span></span> <span data-ttu-id="dd021-185">Exportáló hello sablon két előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="dd021-185">Exporting hello template offers two benefits:</span></span>

1. <span data-ttu-id="dd021-186">Könnyen automatizálható a későbbi központi telepítés alkalmával hello megoldás, mert hello a sablon tartalmazza az összes hello teljes infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="dd021-186">You can easily automate future deployments of hello solution because hello template contains all hello complete infrastructure.</span></span>
2. <span data-ttu-id="dd021-187">Válhat ismeri a sablon szintaxisát megtekintésével hello JavaScript Object Notation (JSON), amely a megoldás jelöli.</span><span class="sxs-lookup"><span data-stu-id="dd021-187">You can become familiar with template syntax by looking at hello JavaScript Object Notation (JSON) that represents your solution.</span></span>

<span data-ttu-id="dd021-188">Lépésenkénti útmutatásért lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-188">For step-by-step guidance, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

## <a name="delete-resource-group-or-resources"></a><span data-ttu-id="dd021-189">Erőforráscsoport és erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="dd021-189">Delete resource group or resources</span></span>
<span data-ttu-id="dd021-190">Erőforráscsoport törlésekor az abban szereplő összes hello erőforrást.</span><span class="sxs-lookup"><span data-stu-id="dd021-190">Deleting a resource group deletes all hello resources contained within it.</span></span> <span data-ttu-id="dd021-191">Törölheti az egyes erőforrások erőforráscsoporton belül is.</span><span class="sxs-lookup"><span data-stu-id="dd021-191">You can also delete individual resources within a resource group.</span></span> <span data-ttu-id="dd021-192">Erőforráscsoport törlésekor, mert lehetséges, hogy nem erőforrásokat, amelyek csatolt tooit egyéb erőforráscsoportok kívánt tooexercise járjon el.</span><span class="sxs-lookup"><span data-stu-id="dd021-192">You want tooexercise caution when you delete a resource group because there might be resources in other resource groups that are linked tooit.</span></span> <span data-ttu-id="dd021-193">Erőforrás-kezelő nem törli a társított erőforrásokat, de azok nem működnek megfelelően várt hello erőforrások nélkül.</span><span class="sxs-lookup"><span data-stu-id="dd021-193">Resource Manager does not delete linked resources, but they may not operate correctly without hello expected resources.</span></span>

![Csoport törlése](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a><span data-ttu-id="dd021-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd021-195">Next steps</span></span>
* <span data-ttu-id="dd021-196">tooview tevékenységi naplóit, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-196">tooview activity logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="dd021-197">a központi telepítés tooview részleteit lásd [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-197">tooview details about a deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="dd021-198">hello portálon keresztül toodeploy erőforrások lásd [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-198">toodeploy resources through hello portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
* <span data-ttu-id="dd021-199">toomanage hozzáférés tooresources, lásd: [szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-199">toomanage access tooresources, see [Use role assignments toomanage access tooyour Azure subscription resources](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="dd021-200">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="dd021-200">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

