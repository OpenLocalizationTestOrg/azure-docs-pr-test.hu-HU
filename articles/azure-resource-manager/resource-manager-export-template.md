---
title: "Azure Resource Manager-sablonok exportálása | Microsoft Docs"
description: "Az Azure Resource Manager használatával sablonokat exportálhat létező erőforráscsoportokból."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: 1801ef47e5b182e0bcd5b23970a2999633b4a852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a><span data-ttu-id="d6b18-103">Azure Resource Manager-sablonok exportálása létező erőforrásokból</span><span class="sxs-lookup"><span data-stu-id="d6b18-103">Export an Azure Resource Manager template from existing resources</span></span>
<span data-ttu-id="d6b18-104">Ebből a cikkből megtudja, hogyan exportálhatja az előfizetéshez tartozó meglévő erőforrások Resource Manager-sablonjait.</span><span class="sxs-lookup"><span data-stu-id="d6b18-104">In this article, you learn how to export a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="d6b18-105">A létrejött sablonnal jobban megértheti a sablon szintaxisát.</span><span class="sxs-lookup"><span data-stu-id="d6b18-105">You can use that generated template to gain a better understanding of template syntax.</span></span>

<span data-ttu-id="d6b18-106">Kétféleképpen exportálhat sablont:</span><span class="sxs-lookup"><span data-stu-id="d6b18-106">There are two ways to export a template:</span></span>

* <span data-ttu-id="d6b18-107">Exportálhatja az **üzembe helyezéshez is használt tényleges sablont**.</span><span class="sxs-lookup"><span data-stu-id="d6b18-107">You can export the **actual template used for deployment**.</span></span> <span data-ttu-id="d6b18-108">Ebben az esetben az exportált sablon pontosan úgy tartalmazza a különböző paramétereket és változókat, ahogy azok az eredeti sablonban szerepeltek.</span><span class="sxs-lookup"><span data-stu-id="d6b18-108">The exported template includes all the parameters and variables exactly as they appeared in the original template.</span></span> <span data-ttu-id="d6b18-109">Ez a megközelítés akkor lehet hasznos, ha a portálon keresztül helyezte üzembe az erőforrásokat, és látni szeretné a sablont ezen erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d6b18-109">This approach is helpful when you deployed resources through the portal, and want to see the template to create those resources.</span></span> <span data-ttu-id="d6b18-110">Ez a sablon azonnal használható.</span><span class="sxs-lookup"><span data-stu-id="d6b18-110">This template is readily usable.</span></span> 
* <span data-ttu-id="d6b18-111">A másik megoldás, hogy úgy exportálja a **létrejött sablont, hogy az az erőforráscsoport aktuális állapotát tükrözze**.</span><span class="sxs-lookup"><span data-stu-id="d6b18-111">You can export a **generated template that represents the current state of the resource group**.</span></span> <span data-ttu-id="d6b18-112">Ebben az esetben az exportált sablon nem az üzembe helyezéshez használt sablonon alapul.</span><span class="sxs-lookup"><span data-stu-id="d6b18-112">The exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="d6b18-113">A rendszer ehelyett új sablont hoz létre az erőforráscsoport aktuális állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="d6b18-113">Instead, it creates a template that is a snapshot of the resource group.</span></span> <span data-ttu-id="d6b18-114">Az exportált sablon számos nem módosítható értéket tartalmaz, és valószínűleg kevesebb paraméter található benne, mint amennyit általában használni szokott.</span><span class="sxs-lookup"><span data-stu-id="d6b18-114">The exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="d6b18-115">Ez a megközelítés akkor lehet hasznos, ha az üzembe helyezés után módosította az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d6b18-115">This approach is useful when you have modified the resource group after deployment.</span></span> <span data-ttu-id="d6b18-116">Ez a sablon általában módosításokat igényel, mielőtt használható lenne.</span><span class="sxs-lookup"><span data-stu-id="d6b18-116">This template usually requires modifications before it is usable.</span></span>

<span data-ttu-id="d6b18-117">Ebben a témakörben mind a két megoldást bemutatjuk a portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d6b18-117">This topic shows both approaches through the portal.</span></span>

## <a name="deploy-resources"></a><span data-ttu-id="d6b18-118">Erőforrások üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d6b18-118">Deploy resources</span></span>
<span data-ttu-id="d6b18-119">Először helyezzünk üzembe erőforrásokat az Azure-ra, amelyeket aztán sablonként exportálhat.</span><span class="sxs-lookup"><span data-stu-id="d6b18-119">Let's start by deploying resources to Azure that you can use for exporting as a template.</span></span> <span data-ttu-id="d6b18-120">Ha már rendelkezik olyan erőforráscsoporttal az előfizetésében, amelyet sablonként szeretne exportálni, kihagyhatja ezt a szakaszt.</span><span class="sxs-lookup"><span data-stu-id="d6b18-120">If you already have a resource group in your subscription that you want to export to a template, you can skip this section.</span></span> <span data-ttu-id="d6b18-121">A cikk többi része feltételezi, hogy üzembe helyezte az ebben a szakaszban látható webalkalmazást és SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="d6b18-121">The remainder of this article assumes you have deployed the web app and SQL database solution shown in this section.</span></span> <span data-ttu-id="d6b18-122">Ha másik megoldást használ, a lépések kissé eltérők lehetnek, de a sablonok exportálásának lépései azonosak.</span><span class="sxs-lookup"><span data-stu-id="d6b18-122">If you use a different solution, your experience might be a little different, but the steps to export a template are the same.</span></span> 

1. <span data-ttu-id="d6b18-123">Az [Azure Portalon](https://portal.azure.com) válassza az **Új** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-123">In the [Azure portal](https://portal.azure.com), select **New**.</span></span>
   
      ![új kiválasztása](./media/resource-manager-export-template/new.png)
2. <span data-ttu-id="d6b18-125">Keressen a **webalkalmazás + SQL** kifejezésre, és válassza ki azt az elérhető lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="d6b18-125">Search for **web app + SQL** and select it from the available options.</span></span>
   
      ![webalkalmazás és SQL keresése](./media/resource-manager-export-template/webapp-sql.png)

3. <span data-ttu-id="d6b18-127">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b18-127">Select **Create**.</span></span>

      ![létrehozás kiválasztása](./media/resource-manager-export-template/create.png)

4. <span data-ttu-id="d6b18-129">Adja meg a webalkalmazás és az SQL-adatbázis szükséges értékeit.</span><span class="sxs-lookup"><span data-stu-id="d6b18-129">Provide the required values for the web app and SQL database.</span></span> <span data-ttu-id="d6b18-130">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b18-130">Select **Create**.</span></span>

      ![web és SQL érték megadása](./media/resource-manager-export-template/provide-web-values.png)

<span data-ttu-id="d6b18-132">Az üzembe helyezés egy percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d6b18-132">The deployment may take a minute.</span></span> <span data-ttu-id="d6b18-133">Az üzembe helyezés befejezése után az előfizetése már tartalmazza a megoldást.</span><span class="sxs-lookup"><span data-stu-id="d6b18-133">After the deployment finishes, your subscription contains the solution.</span></span>

## <a name="view-template-from-deployment-history"></a><span data-ttu-id="d6b18-134">Sablon megtekintése az üzembehelyezési előzményekből</span><span class="sxs-lookup"><span data-stu-id="d6b18-134">View template from deployment history</span></span>
1. <span data-ttu-id="d6b18-135">Nyissa meg az új erőforráscsoport paneljét.</span><span class="sxs-lookup"><span data-stu-id="d6b18-135">Go to the resource group blade for your new resource group.</span></span> <span data-ttu-id="d6b18-136">Figyelje meg, hogy a panelen a legutóbbi üzembe helyezés részletes adatai láthatók.</span><span class="sxs-lookup"><span data-stu-id="d6b18-136">Notice that the blade shows the result of the last deployment.</span></span> <span data-ttu-id="d6b18-137">Kattintson erre a hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d6b18-137">Select this link.</span></span>
   
      ![erőforráscsoport panel](./media/resource-manager-export-template/select-deployment.png)
2. <span data-ttu-id="d6b18-139">Megjelennek a csoport üzembe helyezési előzményei.</span><span class="sxs-lookup"><span data-stu-id="d6b18-139">You see a history of deployments for the group.</span></span> <span data-ttu-id="d6b18-140">Az Ön esetében valószínűleg csak egyetlen üzembe helyezés látható a panelen.</span><span class="sxs-lookup"><span data-stu-id="d6b18-140">In your case, the blade probably lists only one deployment.</span></span> <span data-ttu-id="d6b18-141">Válassza ki ezt a telepítést.</span><span class="sxs-lookup"><span data-stu-id="d6b18-141">Select this deployment.</span></span>
   
     ![legutóbbi telepítés](./media/resource-manager-export-template/select-history.png)
3. <span data-ttu-id="d6b18-143">A panelen megjelenik az üzembe helyezés összegzése.</span><span class="sxs-lookup"><span data-stu-id="d6b18-143">The blade displays a summary of the deployment.</span></span> <span data-ttu-id="d6b18-144">Az összegzés tartalmazza a telepítés, valamint annak műveleteinek állapotát, és a paraméterek számára megadott értékeket.</span><span class="sxs-lookup"><span data-stu-id="d6b18-144">The summary includes the status of the deployment and its operations and the values that you provided for parameters.</span></span> <span data-ttu-id="d6b18-145">Az üzembe helyezéshez használt sablon megtekintéséhez válassza a **Sablon megtekintése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-145">To see the template that you used for the deployment, select **View template**.</span></span>
   
     ![telepítés összegzésének megtekintése](./media/resource-manager-export-template/view-template.png)
4. <span data-ttu-id="d6b18-147">A Resource Manager az alábbi hét fájlt kéri le:</span><span class="sxs-lookup"><span data-stu-id="d6b18-147">Resource Manager retrieves the following seven files for you:</span></span>
   
   1. <span data-ttu-id="d6b18-148">**Sablon** – A megoldás infrastruktúráját meghatározó sablon.</span><span class="sxs-lookup"><span data-stu-id="d6b18-148">**Template** - The template that defines the infrastructure for your solution.</span></span> <span data-ttu-id="d6b18-149">A tárfiók a portálon keresztül történő létrehozásakor a Resource Manager egy sablon használatával telepítette azt, és elmentette ezt a sablont későbbi felhasználás céljából.</span><span class="sxs-lookup"><span data-stu-id="d6b18-149">When you created the storage account through the portal, Resource Manager used a template to deploy it and saved that template for future reference.</span></span>
   2. <span data-ttu-id="d6b18-150">**Paraméterek** – Az értékek az üzembe helyezés során történő megadásához szükséges paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="d6b18-150">**Parameters** - A parameter file that you can use to pass in values during deployment.</span></span> <span data-ttu-id="d6b18-151">Ez tartalmazza az első üzembe helyezés során megadott értékeket.</span><span class="sxs-lookup"><span data-stu-id="d6b18-151">It contains the values that you provided during the first deployment.</span></span> <span data-ttu-id="d6b18-152">Ezek bármelyike módosítható a sablon újbóli telepítése során.</span><span class="sxs-lookup"><span data-stu-id="d6b18-152">You can change any of these values when you redeploy the template.</span></span>
   3. <span data-ttu-id="d6b18-153">**CLI** – A sablon üzembe helyezéséhez használható Azure CLI-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="d6b18-153">**CLI** - An Azure command-line-interface (CLI) script file that you can use to deploy the template.</span></span>
   3. <span data-ttu-id="d6b18-154">**CLI 2.0** – A sablon üzembe helyezéséhez használható Azure CLI-szkriptfájl.</span><span class="sxs-lookup"><span data-stu-id="d6b18-154">**CLI 2.0** - An Azure command-line-interface (CLI) script file that you can use to deploy the template.</span></span>
   4. <span data-ttu-id="d6b18-155">**PowerShell** – A sablon üzembe helyezéséhez használható Azure PowerShell-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="d6b18-155">**PowerShell** - An Azure PowerShell script file that you can use to deploy the template.</span></span>
   5. <span data-ttu-id="d6b18-156">**.NET** – A sablon üzembe helyezéséhez használható .NET-osztály.</span><span class="sxs-lookup"><span data-stu-id="d6b18-156">**.NET** - A .NET class that you can use to deploy the template.</span></span>
   6. <span data-ttu-id="d6b18-157">**Ruby** – A sablon üzembe helyezéséhez használható Ruby-osztály.</span><span class="sxs-lookup"><span data-stu-id="d6b18-157">**Ruby** - A Ruby class that you can use to deploy the template.</span></span>
      
      <span data-ttu-id="d6b18-158">A fájlok a panelen található hivatkozásokon keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d6b18-158">The files are available through links across the blade.</span></span> <span data-ttu-id="d6b18-159">Alapértelmezés szerint a panelben a sablon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d6b18-159">By default, the blade displays the template.</span></span>
      
       ![sablon megtekintése](./media/resource-manager-export-template/see-template.png)
      
<span data-ttu-id="d6b18-161">Ez maga a sablon, amelyet a webalkalmazás és az SQL-adatbázis létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="d6b18-161">This template is the actual template used to create your web app and SQL database.</span></span> <span data-ttu-id="d6b18-162">Figyelje meg, hogy a benne szereplő paraméterek különböző értékek megadását is lehetővé teszik az üzembe helyezés során.</span><span class="sxs-lookup"><span data-stu-id="d6b18-162">Notice it contains parameters that enable you to provide different values during deployment.</span></span> <span data-ttu-id="d6b18-163">A sablonok struktúrájával kapcsolatos további információk: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d6b18-163">To learn more about the structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

## <a name="export-the-template-from-resource-group"></a><span data-ttu-id="d6b18-164">Az erőforráscsoport sablonjának exportálása</span><span class="sxs-lookup"><span data-stu-id="d6b18-164">Export the template from resource group</span></span>
<span data-ttu-id="d6b18-165">Ha manuálisan módosította az erőforrásokat, vagy több üzemelő példányban adott hozzá erőforrásokat, az üzembe helyezés előzményeiből a sablonok lekérése nem tükrözi az erőforráscsoport aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="d6b18-165">If you have manually changed your resources or added resources in multiple deployments, retrieving a template from the deployment history does not reflect the current state of the resource group.</span></span> <span data-ttu-id="d6b18-166">Ez a szakasz bemutatja, hogyan exportálhat az erőforráscsoport aktuális állapotát tükröző sablont.</span><span class="sxs-lookup"><span data-stu-id="d6b18-166">This section shows you how to export a template that reflects the current state of the resource group.</span></span> 

> [!NOTE]
> <span data-ttu-id="d6b18-167">Több mint 200 erőforrással rendelkező erőforráscsoport esetében nem exportálhat sablont.</span><span class="sxs-lookup"><span data-stu-id="d6b18-167">You cannot export a template for a resource group that has more than 200 resources.</span></span>
> 
> 

1. <span data-ttu-id="d6b18-168">Az egyes erőforráscsoportok sablonjának megtekintéséhez válassza az **Automation-szkript** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-168">To view the template for a resource group, select **Automation script**.</span></span>
   
      ![erőforráscsoportok exportálása](./media/resource-manager-export-template/select-automation.png)
   
     <span data-ttu-id="d6b18-170">A Resource Manager kiértékeli az erőforráscsoportban lévő erőforrásokat, és létrehoz egy sablont ezekhez az erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d6b18-170">Resource Manager evaluates the resources in the resource group, and generates a template for those resources.</span></span> <span data-ttu-id="d6b18-171">A sablonexportálási funkciót nem támogatja az összes erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="d6b18-171">Not all resource types support the export template function.</span></span> <span data-ttu-id="d6b18-172">Előfordulhat, hogy egy hibaüzenet jelenik meg, amely arról tájékoztatja, hogy az exportálás során probléma merült fel.</span><span class="sxs-lookup"><span data-stu-id="d6b18-172">You may see an error stating that there is a problem with the export.</span></span> <span data-ttu-id="d6b18-173">Ezeket a hibákat [Az exportálással kapcsolatos problémák megoldása](#fix-export-issues) című részben szereplő információk segítségével oldhatja meg.</span><span class="sxs-lookup"><span data-stu-id="d6b18-173">You learn how to handle those issues in the [Fix export issues](#fix-export-issues) section.</span></span>
2. <span data-ttu-id="d6b18-174">Ismét megjelenik a megoldás újbóli üzembe helyezéséhez használható hat fájl.</span><span class="sxs-lookup"><span data-stu-id="d6b18-174">You again see the six files that you can use to redeploy the solution.</span></span> <span data-ttu-id="d6b18-175">De ezúttal a sablon némileg eltérően jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d6b18-175">However, this time the template is a little different.</span></span> <span data-ttu-id="d6b18-176">Figyelje meg, hogy a létrejött sablon kevesebb paramétert tartalmaz, mint az előző szakasz sablonja.</span><span class="sxs-lookup"><span data-stu-id="d6b18-176">Notice that the generated template contains fewer parameters than the template in previous section.</span></span> <span data-ttu-id="d6b18-177">Ezenkívül sok érték (például a hely és a termékváltozat) paraméterértékek fogadása helyett nem módosíthatóként jelenik meg ebben a sablonban.</span><span class="sxs-lookup"><span data-stu-id="d6b18-177">Also, many of the values (like location and SKU values) are hard-coded in this template rather than accepting a parameter value.</span></span> <span data-ttu-id="d6b18-178">A sablon újbóli használata előtt érdemes lehet szerkeszteni a sablont, hogy jobban kihasználja a paraméterekben rejlő lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="d6b18-178">Before reusing this template, you might want to edit the template to make better use of parameters.</span></span> 
   
3. <span data-ttu-id="d6b18-179">Van néhány további lehetőség is arra, hogy továbbra is ezzel a sablonnal dolgozzon.</span><span class="sxs-lookup"><span data-stu-id="d6b18-179">You have a couple of options for continuing to work with this template.</span></span> <span data-ttu-id="d6b18-180">Letöltheti a sablont, és dolgozhat rajta helyben egy JSON-szerkesztővel.</span><span class="sxs-lookup"><span data-stu-id="d6b18-180">You can either download the template and work on it locally with a JSON editor.</span></span> <span data-ttu-id="d6b18-181">Mentheti a sablont a saját tárába, és a portálon keresztül dolgozhat rajta.</span><span class="sxs-lookup"><span data-stu-id="d6b18-181">Or, you can save the template to your library and work on it through the portal.</span></span>
   
     <span data-ttu-id="d6b18-182">Ha járatos egy JSON-szerkesztő, például a [VS Code](https://code.visualstudio.com/) vagy a [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) használatában, érdemes a sablont helyileg letölteni, és az adott szerkesztőt használni.</span><span class="sxs-lookup"><span data-stu-id="d6b18-182">If you are comfortable using a JSON editor like [VS Code](https://code.visualstudio.com/) or [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), you might prefer downloading the template locally and using that editor.</span></span> <span data-ttu-id="d6b18-183">Ha helyben kíván dolgozni, válassza a **Letöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-183">To work locally, select **Download**.</span></span>
   
      ![sablon letöltése](./media/resource-manager-export-template/download-template.png)
   
     <span data-ttu-id="d6b18-185">Ha nem állította be a JSON-szerkesztőt, inkább a portálon keresztül szerkessze a sablont.</span><span class="sxs-lookup"><span data-stu-id="d6b18-185">If you are not set up with a JSON editor, you might prefer editing the template through the portal.</span></span> <span data-ttu-id="d6b18-186">A témakör további részében azt feltételezzük, hogy mentette a sablont a saját tárába a portálon.</span><span class="sxs-lookup"><span data-stu-id="d6b18-186">The remainder of this topic assumes you have saved the template to your library in the portal.</span></span> <span data-ttu-id="d6b18-187">Azonban ugyanazon szintaxismódosításokat végezheti el a sablonon a JSON-szerkesztőben helyileg és a portálon keresztül egyaránt.</span><span class="sxs-lookup"><span data-stu-id="d6b18-187">However, you make the same syntax changes to the template whether working locally with a JSON editor or through the portal.</span></span> <span data-ttu-id="d6b18-188">Ha a portálon keresztül dolgozna, válassza a **Hozzáadás a dokumentumtárhoz** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-188">To work through the portal, select **Add to library**.</span></span>
   
      ![hozzáadás a dokumentumtárhoz](./media/resource-manager-export-template/add-to-library.png)
   
     <span data-ttu-id="d6b18-190">Egy sablonnak a dokumentumtárhoz való hozzáadásakor adja meg a sablon nevét és leírását.</span><span class="sxs-lookup"><span data-stu-id="d6b18-190">When adding a template to the library, give the template a name and description.</span></span> <span data-ttu-id="d6b18-191">Ezt követően válassza a **Mentés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-191">Then, select **Save**.</span></span>
   
     ![sablon értékeinek megadása](./media/resource-manager-export-template/save-library-template.png)
4. <span data-ttu-id="d6b18-193">Egy, a saját tárába mentett sablon megtekintéséhez válassza a **További szolgáltatások** lehetőséget, írja be a **Sablonok** szöveget az eredmények szűréséhez, majd válassza a **Sablonok** elemet.</span><span class="sxs-lookup"><span data-stu-id="d6b18-193">To view a template saved in your library, select **More services**, type **Templates** to filter results, select **Templates**.</span></span>
   
      ![sablonok keresése](./media/resource-manager-export-template/find-templates.png)
5. <span data-ttu-id="d6b18-195">Válassza mentett nevű sablont.</span><span class="sxs-lookup"><span data-stu-id="d6b18-195">Select the template with the name you saved.</span></span>
   
      ![sablon kiválasztása](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-the-template"></a><span data-ttu-id="d6b18-197">A sablon testreszabása</span><span class="sxs-lookup"><span data-stu-id="d6b18-197">Customize the template</span></span>
<span data-ttu-id="d6b18-198">Az exportált sablon jól működik, ha ugyanazt a webalkalmazást és SQL-adatbázist szeretné létrehozni mindegyik üzemelő példányhoz.</span><span class="sxs-lookup"><span data-stu-id="d6b18-198">The exported template works fine if you want to create the same web app and SQL database for every deployment.</span></span> <span data-ttu-id="d6b18-199">A Resource Manager által nyújtott lehetőségek azonban lehetővé teszik, hogy a sablonokat rugalmasabb módon helyezze üzembe.</span><span class="sxs-lookup"><span data-stu-id="d6b18-199">However, Resource Manager provides options so that you can deploy templates with a lot more flexibility.</span></span> <span data-ttu-id="d6b18-200">Ez a cikk bemutatja, hogyan adhat paramétereket az adatbázis rendszergazdai nevéhez és jelszavához.</span><span class="sxs-lookup"><span data-stu-id="d6b18-200">This article shows you how to add parameters for the database administrator name and password.</span></span> <span data-ttu-id="d6b18-201">Ugyanezzel a módszerrel láthatja el további rugalmassággal a sablon más értékeit.</span><span class="sxs-lookup"><span data-stu-id="d6b18-201">You can use this same approach to add more flexibility for other values in the template.</span></span>

1. <span data-ttu-id="d6b18-202">A sablon testreszabásához válassza a **Szerkesztés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-202">To customize the template, select **Edit**.</span></span>
   
     ![sablon megjelenítése](./media/resource-manager-export-template/select-edit.png)
2. <span data-ttu-id="d6b18-204">Válassza ki a sablont.</span><span class="sxs-lookup"><span data-stu-id="d6b18-204">Select the template.</span></span>
   
     ![sablon szerkesztése](./media/resource-manager-export-template/select-added-template.png)
3. <span data-ttu-id="d6b18-206">A telepítés során meghatározni kívánt értékek megadásához adja a következő két paramétert a sablon **paraméterek** szakaszához:</span><span class="sxs-lookup"><span data-stu-id="d6b18-206">To be able to pass the values that you might want to specify during deployment, add the following two parameters to the **parameters** section in the template:</span></span>

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. <span data-ttu-id="d6b18-207">Az új paraméterek használatához cserélje le az SQL-kiszolgáló definícióját az **erőforrások** szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d6b18-207">To use the new parameters, replace the SQL server definition in the **resources** section.</span></span> <span data-ttu-id="d6b18-208">Figyelje meg, hogy az **administratorLogin** és az **administratorLoginPassword** most paraméterértékeket használ.</span><span class="sxs-lookup"><span data-stu-id="d6b18-208">Notice that **administratorLogin** and **administratorLoginPassword** now use parameter values.</span></span>

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. <span data-ttu-id="d6b18-209">Amikor befejezte a sablon szerkesztését, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6b18-209">Select **OK** when you are done editing the template.</span></span>
7. <span data-ttu-id="d6b18-210">Kattintson a **Mentés** gombra a sablon módosításainak mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6b18-210">Select **Save** to save the changes to the template.</span></span>
   
     ![sablon mentése](./media/resource-manager-export-template/save-template.png)
8. <span data-ttu-id="d6b18-212">A frissített sablon újbóli üzembe helyezéséhez válassza a **Telepítés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d6b18-212">To redeploy the updated template, select **Deploy**.</span></span>
   
     ![sablon üzembe helyezése](./media/resource-manager-export-template/redeploy-template.png)
9. <span data-ttu-id="d6b18-214">Adja meg a paraméterértékeket, és válassza ki azt az erőforráscsoportot, amelybe az erőforrásokat üzembe kívánja helyezni.</span><span class="sxs-lookup"><span data-stu-id="d6b18-214">Provide parameter values, and select a resource group to deploy the resources to.</span></span>


## <a name="fix-export-issues"></a><span data-ttu-id="d6b18-215">Az exportálással kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="d6b18-215">Fix export issues</span></span>
<span data-ttu-id="d6b18-216">A sablonexportálási funkciót nem támogatja az összes erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="d6b18-216">Not all resource types support the export template function.</span></span> <span data-ttu-id="d6b18-217">A probléma megoldásához adja hozzá ismét manuálisan a hiányzó erőforrásokat a sablonhoz.</span><span class="sxs-lookup"><span data-stu-id="d6b18-217">To resolve this issue, manually add the missing resources back into your template.</span></span> <span data-ttu-id="d6b18-218">A hibaüzenetben szerepelnek a nem exportálható erőforrástípusok.</span><span class="sxs-lookup"><span data-stu-id="d6b18-218">The error message includes the resource types that cannot be exported.</span></span> <span data-ttu-id="d6b18-219">Ezt az erőforrástípust a [Sablonreferenciában](/azure/templates/) találja.</span><span class="sxs-lookup"><span data-stu-id="d6b18-219">Find that resource type in [Template reference](/azure/templates/).</span></span> <span data-ttu-id="d6b18-220">Ha például manuálisan szeretne hozzáadni egy virtuális hálózati átjárót, lásd a [Microsoft.Network/virtualNetworkGateways sablonreferenciát](/azure/templates/microsoft.network/virtualnetworkgateways).</span><span class="sxs-lookup"><span data-stu-id="d6b18-220">For example, to manually add a virtual network gateway, see [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways).</span></span>

> [!NOTE]
> <span data-ttu-id="d6b18-221">Az exportálási hibák csak akkor lépnek fel, ha az erőforráscsoportból, és nem az üzembe helyezési előzmények közül végez exportálást.</span><span class="sxs-lookup"><span data-stu-id="d6b18-221">You only encounter export issues when exporting from a resource group rather than from your deployment history.</span></span> <span data-ttu-id="d6b18-222">Ha a legutóbbi üzembe helyezés pontosan tükrözi az erőforráscsoport aktuális állapotát, érdemes az erőforráscsoport helyett az üzembe helyezési előzmények közül elvégezni a sablon exportálását.</span><span class="sxs-lookup"><span data-stu-id="d6b18-222">If your last deployment accurately represents the current state of the resource group, you should export the template from the deployment history rather than from the resource group.</span></span> <span data-ttu-id="d6b18-223">Csak akkor exportáljon az erőforráscsoportból, ha olyan módosításokat végzett rajta, amelyeket nem lehet egyetlen sablonnal definiálni.</span><span class="sxs-lookup"><span data-stu-id="d6b18-223">Only export from a resource group when you have made changes to the resource group that are not defined in a single template.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d6b18-224">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6b18-224">Next steps</span></span>
<span data-ttu-id="d6b18-225">Megtanulta, hogyan exportálhat sablonokat a portálon létrehozott erőforrásokból.</span><span class="sxs-lookup"><span data-stu-id="d6b18-225">You have learned how to export a template from resources that you created in the portal.</span></span>

* <span data-ttu-id="d6b18-226">Sablont a [PowerShell](resource-group-template-deploy.md), az [Azure parancssori felülete](resource-group-template-deploy-cli.md) vagy a [REST API](resource-group-template-deploy-rest.md) használatával helyezhet üzembe.</span><span class="sxs-lookup"><span data-stu-id="d6b18-226">You can deploy a template through [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md), or [REST API](resource-group-template-deploy-rest.md).</span></span>
* <span data-ttu-id="d6b18-227">A sablonok PowerShellen keresztül történő exportálásával kapcsolatos információk: [Az Azure PowerShell használata Azure Resource Managerrel](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="d6b18-227">To see how to export a template through PowerShell, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="d6b18-228">A sablonok az Azure parancssori felületen keresztül történő exportálásával kapcsolatos információk: [A Mac, Linux és Windows eszközökhöz készült Azure CLI használata az Azure Resource Manager eszközzel](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="d6b18-228">To see how to export a template through Azure CLI, see [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>

