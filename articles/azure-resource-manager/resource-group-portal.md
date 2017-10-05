---
title: "Azure-erőforrások kezeléséhez Azure portál használatával |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager segítségével kezelheti az erőforrásokat. Bemutatja, hogyan irányítópultok erőforrások figyelésére használható."
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
ms.openlocfilehash: 7a94fd5065de93384460e851627a9813d439956b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="manage-azure-resources-through-portal"></a><span data-ttu-id="19dda-104">Azure-portálon keresztül erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="19dda-104">Manage Azure resources through portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="19dda-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="19dda-105">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="19dda-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="19dda-106">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="19dda-107">Portál</span><span class="sxs-lookup"><span data-stu-id="19dda-107">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="19dda-108">REST API</span><span class="sxs-lookup"><span data-stu-id="19dda-108">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="19dda-109">Ez a témakör bemutatja, hogyan használja a [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="19dda-109">This topic shows how to use the [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) to manage your Azure resources.</span></span> <span data-ttu-id="19dda-110">A portálon keresztül erőforrások telepítésével kapcsolatos további tudnivalókért lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-110">To learn about deploying resources through the portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>

<span data-ttu-id="19dda-111">Jelenleg nem minden szolgáltatás támogatja, a portál vagy az erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="19dda-111">Currently, not every service supports the portal or Resource Manager.</span></span> <span data-ttu-id="19dda-112">Ezek a szolgáltatások kell használnia a [klasszikus portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="19dda-112">For those services, you need to use the [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="19dda-113">Minden szolgáltatás állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="19dda-113">For the status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="manage-resource-groups"></a><span data-ttu-id="19dda-114">Erőforrás-csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="19dda-114">Manage resource groups</span></span>

<span data-ttu-id="19dda-115">Egy erőforráscsoport egy olyan tároló, amely egy Azure megoldás kapcsolódó erőforrásokat tárol.</span><span class="sxs-lookup"><span data-stu-id="19dda-115">A resource group is a container that holds related resources for an Azure solution.</span></span> <span data-ttu-id="19dda-116">Az erőforráscsoport tartalmazhatja a megoldás összes erőforrását, vagy csak azokat az erőforrásokat, amelyeket Ön egy csoportként szeretne kezelni.</span><span class="sxs-lookup"><span data-stu-id="19dda-116">The resource group can include all the resources for the solution, or only those resources that you want to manage as a group.</span></span> <span data-ttu-id="19dda-117">A szervezet számára legideálisabb elosztás alapján eldöntheti, hogyan szeretné elosztani az erőforrásokat az erőforráscsoportok között.</span><span class="sxs-lookup"><span data-stu-id="19dda-117">You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization.</span></span> <span data-ttu-id="19dda-118">Általában adja hozzá az erőforrásokat, amelyek ugyanabban az erőforráscsoportban az azonos életciklussal megoszthatja, így könnyen központi telepítése, frissítése és csoportként törölje őket.</span><span class="sxs-lookup"><span data-stu-id="19dda-118">Generally, add resources that share the same lifecycle to the same resource group so you can easily deploy, update, and delete them as a group.</span></span> 

<span data-ttu-id="19dda-119">Az erőforráscsoport erőforrásokra vonatkozó metaadatokat tárol.</span><span class="sxs-lookup"><span data-stu-id="19dda-119">The resource group stores metadata about the resources.</span></span> <span data-ttu-id="19dda-120">Ezért ha az erőforráscsoport számára megad egy helyet, akkor a metaadatok tárolási helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="19dda-120">Therefore, when you specify a location for the resource group, you are specifying where that metadata is stored.</span></span> <span data-ttu-id="19dda-121">Megfelelőségi okokból szükség lehet arra, hogy az adatokat egy adott régióban tárolja.</span><span class="sxs-lookup"><span data-stu-id="19dda-121">For compliance reasons, you may need to ensure that your data is stored in a particular region.</span></span>

1. <span data-ttu-id="19dda-122">Az előfizetés összes erőforráscsoport megtekintéséhez válasszon **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="19dda-122">To see all the resource groups in your subscription, select **Resource groups**.</span></span>
   
    ![Keresse meg az erőforráscsoport-sablonok](./media/resource-group-portal/browse-groups.png)
2. <span data-ttu-id="19dda-124">Hozzon létre egy üres erőforráscsoportot, válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="19dda-124">To create an empty resource group, select **Add**.</span></span>
   
    ![csoport hozzáadása](./media/resource-group-portal/add-resource-group.png)
3. <span data-ttu-id="19dda-126">Adjon meg nevet és az új erőforráscsoporthoz tartozó helyet.</span><span class="sxs-lookup"><span data-stu-id="19dda-126">Provide a name and location for the new resource group.</span></span> <span data-ttu-id="19dda-127">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="19dda-127">Select **Create**.</span></span>
   
    ![Erőforráscsoport létrehozása](./media/resource-group-portal/create-empty-group.png)
4. <span data-ttu-id="19dda-129">Válassza ki szeretne **frissítése** a legutóbb létrehozott erőforráscsoport megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="19dda-129">You may need to select **Refresh** to see the recently created resource group.</span></span>
   
    ![erőforrás-csoport frissítése](./media/resource-group-portal/refresh-resource-groups.png)
5. <span data-ttu-id="19dda-131">Az erőforráscsoportok megjelenített információk testreszabásához jelölje be **oszlopok**.</span><span class="sxs-lookup"><span data-stu-id="19dda-131">To customize the information displayed for your resource groups, select **Columns**.</span></span>
   
    ![oszlop testreszabása](./media/resource-group-portal/select-columns.png)
6. <span data-ttu-id="19dda-133">A hozzáadása, és válasszon egy oszlopot válasszon ki **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="19dda-133">Select the columns to add, and then select **Update**.</span></span>
   
    ![oszlopok hozzáadása](./media/resource-group-portal/add-columns.png)
7. <span data-ttu-id="19dda-135">Az új erőforráscsoportba erőforrások telepítésével kapcsolatos további tudnivalókért lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-135">To learn about deploying resources to your new resource group, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
8. <span data-ttu-id="19dda-136">A gyors elérés érdekében az erőforráscsoporthoz a panel rögzítheti az irányítópulton való rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="19dda-136">For quick access to a resource group, you can pin the blade to your dashboard.</span></span>
   
    ![PIN-kód erőforráscsoport](./media/resource-group-portal/pin-group.png)
9. <span data-ttu-id="19dda-138">Az irányítópult az erőforráscsoportot és az erőforrások jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="19dda-138">The dashboard displays the resource group and its resources.</span></span> <span data-ttu-id="19dda-139">Kiválaszthatja az erőforráscsoportok vagy az erőforrásokat a navigáljon.</span><span class="sxs-lookup"><span data-stu-id="19dda-139">You can select either the resource groups or any of its resources to navigate to the item.</span></span>
   
    ![PIN-kód erőforráscsoport](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a><span data-ttu-id="19dda-141">Címke erőforrások</span><span class="sxs-lookup"><span data-stu-id="19dda-141">Tag resources</span></span>
<span data-ttu-id="19dda-142">A címkékkel erőforráscsoportok és erőforrásokhoz, hogy logikusan rendszerezhesse az Ön eszközeit.</span><span class="sxs-lookup"><span data-stu-id="19dda-142">You can apply tags to resource groups and resources to logically organize your assets.</span></span> <span data-ttu-id="19dda-143">A címkék használatáról információkért lásd: [az Azure-erőforrások rendszerezése címkék használatával](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-143">For information about working with tags, see [Using tags to organize your Azure resources](resource-group-using-tags.md).</span></span>

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a><span data-ttu-id="19dda-144">Erőforrások megfigyelése</span><span class="sxs-lookup"><span data-stu-id="19dda-144">Monitor resources</span></span>
<span data-ttu-id="19dda-145">Amikor kiválaszt egy erőforrást, az erőforrás panel alapértelmezett diagramjait és az adott erőforrástípus figyelési táblák mutatja be.</span><span class="sxs-lookup"><span data-stu-id="19dda-145">When you select a resource, the resource blade presents default graphs and tables for monitoring that resource type.</span></span>

1. <span data-ttu-id="19dda-146">Válasszon ki egy erőforrást és értesítés a **figyelés** szakasz.</span><span class="sxs-lookup"><span data-stu-id="19dda-146">Select a resource and notice the **Monitoring** section.</span></span> <span data-ttu-id="19dda-147">Ez magában foglalja a diagramok, amelyek kapcsolódnak az erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="19dda-147">It includes graphs that are relevant to the resource type.</span></span> <span data-ttu-id="19dda-148">A következő kép bemutatja az alapértelmezett figyelési a tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="19dda-148">The following image shows the default monitoring data for a storage account.</span></span>
   
    ![figyelési megjelenítése](./media/resource-group-portal/show-monitoring.png)
2. <span data-ttu-id="19dda-150">Egy részében találhatja; ehhez válassza a három ponttal (…) a szakasz fenti rögzítheti az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="19dda-150">You can pin a section of the blade to your dashboard by selecting the ellipsis (...) above the section.</span></span> <span data-ttu-id="19dda-151">A szakasz a panel mérete testreszabásával, vagy távolítsa el teljesen.</span><span class="sxs-lookup"><span data-stu-id="19dda-151">You can also customize the size the section in the blade or remove it completely.</span></span> <span data-ttu-id="19dda-152">A következő kép bemutatja, hogyan PIN-kód, testre szabhatja, vagy távolítsa el a Processzor és memória szakaszban.</span><span class="sxs-lookup"><span data-stu-id="19dda-152">The following image shows how to pin, customize, or remove the CPU and Memory section.</span></span>
   
    ![PIN-kód szakasz](./media/resource-group-portal/pin-cpu-section.png)
3. <span data-ttu-id="19dda-154">Miután a szakasz az irányítópulton, látni fogja az összegzés az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="19dda-154">After pinning the section to the dashboard, you will see the summary on the dashboard.</span></span> <span data-ttu-id="19dda-155">És lehetőséget azonnal viszi az adatokkal kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="19dda-155">And, selecting it immediately takes you to more details about the data.</span></span>
   
    ![Irányítópult nézet](./media/resource-group-portal/view-startboard.png)
4. <span data-ttu-id="19dda-157">A portálon keresztül figyelheti az adatok teljesen testreszabásához nyissa meg az alapértelmezett irányítópultot, és válassza ki **új irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="19dda-157">To completely customize the data you monitor through the portal, navigate to your default dashboard, and select **New dashboard**.</span></span>
   
    ![irányítópult](./media/resource-group-portal/dashboard.png)
5. <span data-ttu-id="19dda-159">Nevezze el az új irányítópult és csempék húzza az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="19dda-159">Give your new dashboard a name and drag tiles onto the dashboard.</span></span> <span data-ttu-id="19dda-160">A csempék különböző beállítások szerint vannak szűrve.</span><span class="sxs-lookup"><span data-stu-id="19dda-160">The tiles are filtered by different options.</span></span>
   
    ![irányítópult](./media/resource-group-portal/create-dashboard.png)
   
     <span data-ttu-id="19dda-162">Irányítópultok kezelésével kapcsolatos információkért lásd: [létrehozása és az Azure portálon irányítópultok megosztása](../azure-portal/azure-portal-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-162">To learn about working with dashboards, see [Creating and sharing dashboards in the Azure portal](../azure-portal/azure-portal-dashboards.md).</span></span>

## <a name="manage-resources"></a><span data-ttu-id="19dda-163">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="19dda-163">Manage resources</span></span>
<span data-ttu-id="19dda-164">Az erőforrás panelen tekintse meg a beállítások kezelését az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="19dda-164">In the blade for a resource, you see the options for managing the resource.</span></span> <span data-ttu-id="19dda-165">A portál megjelenít, hogy adott erőforrástípusra vonatkozó beállítások.</span><span class="sxs-lookup"><span data-stu-id="19dda-165">The portal presents management options for that particular resource type.</span></span> <span data-ttu-id="19dda-166">A parancsok és a bal oldalon az erőforrás panel tetején megjelenik.</span><span class="sxs-lookup"><span data-stu-id="19dda-166">You see the management commands across the top of the resource blade and on the left side.</span></span>

![Erőforrások kezelése](./media/resource-group-portal/manage-resources.png)

<span data-ttu-id="19dda-168">Ezek a beállítások műveletek, például a indítása és leállítása a virtuális gép vagy újrakonfigurálása a virtuális gép tulajdonságai között végezheti el.</span><span class="sxs-lookup"><span data-stu-id="19dda-168">From these options, you can perform operations such as starting and stopping a virtual machine, or reconfiguring the properties of the virtual machine.</span></span>

## <a name="move-resources"></a><span data-ttu-id="19dda-169">Erőforrások áthelyezése</span><span class="sxs-lookup"><span data-stu-id="19dda-169">Move resources</span></span>
<span data-ttu-id="19dda-170">Ha az erőforrások áthelyezése egy másik erőforráscsoportban vagy egy másik előfizetés van szüksége, tekintse meg [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-170">If you need to move resources to another resource group or another subscription, see [Move resources to new resource group or subscription](resource-group-move-resources.md).</span></span>

## <a name="lock-resources"></a><span data-ttu-id="19dda-171">Erőforrások zárolása</span><span class="sxs-lookup"><span data-stu-id="19dda-171">Lock resources</span></span>
<span data-ttu-id="19dda-172">Zárolhatja egy előfizetés, erőforráscsoportból vagy erőforrás véletlen törlése vagy a kritikus erőforrásokat módosítása a munkahely más felhasználóinak megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="19dda-172">You can lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="19dda-173">További információ: [Erőforrások zárolása az Azure Resource Manager eszközzel](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-173">For more information, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a><span data-ttu-id="19dda-174">Az előfizetés és a költségek megtekintése</span><span class="sxs-lookup"><span data-stu-id="19dda-174">View your subscription and costs</span></span>
<span data-ttu-id="19dda-175">Az előfizetés és az összegzett költségeinek kapcsolatos információk is megtekinthetők az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="19dda-175">You can view information about your subscription and the rolled-up costs for all your resources.</span></span> <span data-ttu-id="19dda-176">Válassza ki **előfizetések** és meg szeretné tekinteni az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="19dda-176">Select **Subscriptions** and the subscription you want to see.</span></span> <span data-ttu-id="19dda-177">Válasszon egy előfizetés csak lehet.</span><span class="sxs-lookup"><span data-stu-id="19dda-177">You might only have one subscription to select.</span></span>

![előfizetést](./media/resource-group-portal/select-subscription.png)

<span data-ttu-id="19dda-179">Az előfizetés panel belül megjelenik egy írási sebesség.</span><span class="sxs-lookup"><span data-stu-id="19dda-179">Within the subscription blade, you see a burn rate.</span></span>

![Írás gyakorisága](./media/resource-group-portal/burn-rate.png)

<span data-ttu-id="19dda-181">És erőforrástípusok szerint költségeket.</span><span class="sxs-lookup"><span data-stu-id="19dda-181">And, a breakdown of costs by resource type.</span></span>

![Erőforrás költség](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a><span data-ttu-id="19dda-183">Sablon exportálása</span><span class="sxs-lookup"><span data-stu-id="19dda-183">Export template</span></span>
<span data-ttu-id="19dda-184">Miután beállította az erőforráscsoport, érdemes lehet a Resource Manager-sablon az erőforráscsoport megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="19dda-184">After setting up your resource group, you may want to view the Resource Manager template for the resource group.</span></span> <span data-ttu-id="19dda-185">A sablon exportálása két előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="19dda-185">Exporting the template offers two benefits:</span></span>

1. <span data-ttu-id="19dda-186">Könnyen automatizálható a későbbi központi telepítés alkalmával a megoldás, mert a sablon tartalmazza a teljes infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="19dda-186">You can easily automate future deployments of the solution because the template contains all the complete infrastructure.</span></span>
2. <span data-ttu-id="19dda-187">Válhat ismeri a sablon szintaxisát, a JavaScript Object Notation (JSON) a megoldás jelölő megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="19dda-187">You can become familiar with template syntax by looking at the JavaScript Object Notation (JSON) that represents your solution.</span></span>

<span data-ttu-id="19dda-188">Lépésenkénti útmutatásért lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-188">For step-by-step guidance, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

## <a name="delete-resource-group-or-resources"></a><span data-ttu-id="19dda-189">Erőforráscsoport és erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="19dda-189">Delete resource group or resources</span></span>
<span data-ttu-id="19dda-190">Erőforráscsoport törlésekor törlődnek a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="19dda-190">Deleting a resource group deletes all the resources contained within it.</span></span> <span data-ttu-id="19dda-191">Törölheti az egyes erőforrások erőforráscsoporton belül is.</span><span class="sxs-lookup"><span data-stu-id="19dda-191">You can also delete individual resources within a resource group.</span></span> <span data-ttu-id="19dda-192">Szeretné körültekintően járjon el egy erőforráscsoport törlésekor, mert lehetséges, hogy nem erőforrások egyéb erőforráscsoportok, amelyek kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="19dda-192">You want to exercise caution when you delete a resource group because there might be resources in other resource groups that are linked to it.</span></span> <span data-ttu-id="19dda-193">Erőforrás-kezelő nem törli a társított erőforrásokat, de azok nem működnek megfelelően nélkül szükséges erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="19dda-193">Resource Manager does not delete linked resources, but they may not operate correctly without the expected resources.</span></span>

![Csoport törlése](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a><span data-ttu-id="19dda-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19dda-195">Next steps</span></span>
* <span data-ttu-id="19dda-196">Tevékenységi naplóit megtekintéséhez lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-196">To view activity logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="19dda-197">A központi telepítés részleteinek megtekintéséhez lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-197">To view details about a deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="19dda-198">Az erőforrásoknak a portálon keresztül történő központi telepítéséhez lásd: [erőforrások a Resource Manager-sablonok és az Azure-portál telepítése](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-198">To deploy resources through the portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
* <span data-ttu-id="19dda-199">Erőforrások elérésének kezelésében, tekintse meg [az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése a szerepkör-hozzárendelések segítségével](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="19dda-199">To manage access to resources, see [Use role assignments to manage access to your Azure subscription resources](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="19dda-200">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="19dda-200">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

