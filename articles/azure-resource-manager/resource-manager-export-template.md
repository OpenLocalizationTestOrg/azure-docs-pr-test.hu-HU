---
title: Azure Resource Manager-sablon aaaExport |} Microsoft Docs
description: "Használja az Azure Resource Manager tooexport egy sablon, egy meglévő erőforráscsoportot."
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
ms.openlocfilehash: 94daa4812da2fec705044ca31c8e74e6d59bd53f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a><span data-ttu-id="d77db-103">Azure Resource Manager-sablonok exportálása létező erőforrásokból</span><span class="sxs-lookup"><span data-stu-id="d77db-103">Export an Azure Resource Manager template from existing resources</span></span>
<span data-ttu-id="d77db-104">Ebből a cikkből megismerheti, hogyan tooexport a Resource Manager-sablon a meglévő erőforrást az előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="d77db-104">In this article, you learn how tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="d77db-105">A létrehozott sablon toogain sablon szintaxisáról jobb megértése is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d77db-105">You can use that generated template toogain a better understanding of template syntax.</span></span>

<span data-ttu-id="d77db-106">Nincsenek két módon tooexport sablont:</span><span class="sxs-lookup"><span data-stu-id="d77db-106">There are two ways tooexport a template:</span></span>

* <span data-ttu-id="d77db-107">Exportálhatja a hello **tényleges központi telepítéshez használt sablon**.</span><span class="sxs-lookup"><span data-stu-id="d77db-107">You can export hello **actual template used for deployment**.</span></span> <span data-ttu-id="d77db-108">hello exportált sablon tartalmaz minden hello paraméterek és változók pontosan úgy, mint ahogy az eredeti sablon hello.</span><span class="sxs-lookup"><span data-stu-id="d77db-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="d77db-109">Ez a módszer akkor hasznos, ha az erőforrások hello portálon keresztül telepített, és szeretné toosee hello sablon toocreate ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d77db-109">This approach is helpful when you deployed resources through hello portal, and want toosee hello template toocreate those resources.</span></span> <span data-ttu-id="d77db-110">Ez a sablon azonnal használható.</span><span class="sxs-lookup"><span data-stu-id="d77db-110">This template is readily usable.</span></span> 
* <span data-ttu-id="d77db-111">Exportálhatja egy **létrehozott sablont, amely hello hello erőforráscsoport aktuális állapotát jeleníti meg**.</span><span class="sxs-lookup"><span data-stu-id="d77db-111">You can export a **generated template that represents hello current state of hello resource group**.</span></span> <span data-ttu-id="d77db-112">hello exportált sablon nem alapul, amely a központi telepítéshez használt sablonokat.</span><span class="sxs-lookup"><span data-stu-id="d77db-112">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="d77db-113">Ehelyett az alkalmazás létrehozza a sablont, amely hello erőforráscsoportban található egy pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="d77db-113">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="d77db-114">hello exportált sablonjának sok változtatható értékek és valószínűleg nem annyi megadott paraméterek általában határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="d77db-114">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="d77db-115">Ezt a módszert akkor hasznos, ha a telepítés utáni hello erőforráscsoport módosította.</span><span class="sxs-lookup"><span data-stu-id="d77db-115">This approach is useful when you have modified hello resource group after deployment.</span></span> <span data-ttu-id="d77db-116">Ez a sablon általában módosításokat igényel, mielőtt használható lenne.</span><span class="sxs-lookup"><span data-stu-id="d77db-116">This template usually requires modifications before it is usable.</span></span>

<span data-ttu-id="d77db-117">Ez a témakör bemutatja a mindkét megközelítés hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d77db-117">This topic shows both approaches through hello portal.</span></span>

## <a name="deploy-resources"></a><span data-ttu-id="d77db-118">Erőforrások üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="d77db-118">Deploy resources</span></span>
<span data-ttu-id="d77db-119">Kezdjük használható sablonként exportálási erőforrások tooAzure telepítésével.</span><span class="sxs-lookup"><span data-stu-id="d77db-119">Let's start by deploying resources tooAzure that you can use for exporting as a template.</span></span> <span data-ttu-id="d77db-120">Ha már van erőforráscsoport, amelyet az tooexport tooa sablon előfizetését, ez a szakasz kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="d77db-120">If you already have a resource group in your subscription that you want tooexport tooa template, you can skip this section.</span></span> <span data-ttu-id="d77db-121">hello a cikk hátralévő része azt feltételezi, hogy telepítette hello web app és az SQL-adatbázis megoldás ebben a szakaszban látható.</span><span class="sxs-lookup"><span data-stu-id="d77db-121">hello remainder of this article assumes you have deployed hello web app and SQL database solution shown in this section.</span></span> <span data-ttu-id="d77db-122">Ha egy másik megoldás használja, a felhasználói élmény kissé eltérőek lehetnek, de tooexport sablon vannak hello lépéseket hello azonos.</span><span class="sxs-lookup"><span data-stu-id="d77db-122">If you use a different solution, your experience might be a little different, but hello steps tooexport a template are hello same.</span></span> 

1. <span data-ttu-id="d77db-123">A hello [Azure-portálon](https://portal.azure.com), jelölje be **új**.</span><span class="sxs-lookup"><span data-stu-id="d77db-123">In hello [Azure portal](https://portal.azure.com), select **New**.</span></span>
   
      ![új kiválasztása](./media/resource-manager-export-template/new.png)
2. <span data-ttu-id="d77db-125">Keresse meg **webes alkalmazás + SQL** , és jelölje ki a hello rendelkezésre álló lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="d77db-125">Search for **web app + SQL** and select it from hello available options.</span></span>
   
      ![webalkalmazás és SQL keresése](./media/resource-manager-export-template/webapp-sql.png)

3. <span data-ttu-id="d77db-127">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d77db-127">Select **Create**.</span></span>

      ![létrehozás kiválasztása](./media/resource-manager-export-template/create.png)

4. <span data-ttu-id="d77db-129">Adja meg szükség hello hello web app és az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d77db-129">Provide hello required values for hello web app and SQL database.</span></span> <span data-ttu-id="d77db-130">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d77db-130">Select **Create**.</span></span>

      ![web és SQL érték megadása](./media/resource-manager-export-template/provide-web-values.png)

<span data-ttu-id="d77db-132">hello telepítési egy percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d77db-132">hello deployment may take a minute.</span></span> <span data-ttu-id="d77db-133">Hello központi telepítés befejezése után az előfizetése tartalmazza majd hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="d77db-133">After hello deployment finishes, your subscription contains hello solution.</span></span>

## <a name="view-template-from-deployment-history"></a><span data-ttu-id="d77db-134">Sablon megtekintése az üzembehelyezési előzményekből</span><span class="sxs-lookup"><span data-stu-id="d77db-134">View template from deployment history</span></span>
1. <span data-ttu-id="d77db-135">Nyissa meg toohello erőforráscsoport panel az új erőforráscsoport számára.</span><span class="sxs-lookup"><span data-stu-id="d77db-135">Go toohello resource group blade for your new resource group.</span></span> <span data-ttu-id="d77db-136">Figyelje meg, hogy hello hello utolsó telepítési hello eredménye megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d77db-136">Notice that hello blade shows hello result of hello last deployment.</span></span> <span data-ttu-id="d77db-137">Kattintson erre a hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d77db-137">Select this link.</span></span>
   
      ![erőforráscsoport panel](./media/resource-manager-export-template/select-deployment.png)
2. <span data-ttu-id="d77db-139">Megjelenik egy hello csoport telepítéseinek előzményei.</span><span class="sxs-lookup"><span data-stu-id="d77db-139">You see a history of deployments for hello group.</span></span> <span data-ttu-id="d77db-140">Az Ön hello panel valószínűleg csak egy központi telepítési sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="d77db-140">In your case, hello blade probably lists only one deployment.</span></span> <span data-ttu-id="d77db-141">Válassza ki ezt a telepítést.</span><span class="sxs-lookup"><span data-stu-id="d77db-141">Select this deployment.</span></span>
   
     ![legutóbbi telepítés](./media/resource-manager-export-template/select-history.png)
3. <span data-ttu-id="d77db-143">hello panel hello telepítés összegzését jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d77db-143">hello blade displays a summary of hello deployment.</span></span> <span data-ttu-id="d77db-144">összefoglaló hello hello hello központi telepítés állapotát, valamint műveleteket és hello paramétereinek megadott értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d77db-144">hello summary includes hello status of hello deployment and its operations and hello values that you provided for parameters.</span></span> <span data-ttu-id="d77db-145">hello központi telepítését, jelölje be a használt toosee hello sablon **sablon megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="d77db-145">toosee hello template that you used for hello deployment, select **View template**.</span></span>
   
     ![telepítés összegzésének megtekintése](./media/resource-manager-export-template/view-template.png)
4. <span data-ttu-id="d77db-147">Erőforrás-kezelő kéri le a következő hét fájlok meg hello:</span><span class="sxs-lookup"><span data-stu-id="d77db-147">Resource Manager retrieves hello following seven files for you:</span></span>
   
   1. <span data-ttu-id="d77db-148">**Sablon** -hello sablont, amely meghatározza a megoldás hello infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="d77db-148">**Template** - hello template that defines hello infrastructure for your solution.</span></span> <span data-ttu-id="d77db-149">Hello portálon keresztül hello storage-fiók létrehozásakor a Resource Manager egy sablon toodeploy használt, és elmentette ezt a sablont későbbi felhasználás céljából.</span><span class="sxs-lookup"><span data-stu-id="d77db-149">When you created hello storage account through hello portal, Resource Manager used a template toodeploy it and saved that template for future reference.</span></span>
   2. <span data-ttu-id="d77db-150">**Paraméterek** -egy paraméterfájl használható toopass értékek a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="d77db-150">**Parameters** - A parameter file that you can use toopass in values during deployment.</span></span> <span data-ttu-id="d77db-151">Hello első telepítés során megadott hello értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d77db-151">It contains hello values that you provided during hello first deployment.</span></span> <span data-ttu-id="d77db-152">Módosíthatja a következő értékek hello sablon újratelepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d77db-152">You can change any of these values when you redeploy hello template.</span></span>
   3. <span data-ttu-id="d77db-153">**Parancssori felület** -Azure parancssori felület (CLI) parancsfájl toodeploy hello sablon használható.</span><span class="sxs-lookup"><span data-stu-id="d77db-153">**CLI** - An Azure command-line-interface (CLI) script file that you can use toodeploy hello template.</span></span>
   3. <span data-ttu-id="d77db-154">**Parancssori felület 2.0** -Azure parancssori felület (CLI) parancsfájl toodeploy hello sablon használható.</span><span class="sxs-lookup"><span data-stu-id="d77db-154">**CLI 2.0** - An Azure command-line-interface (CLI) script file that you can use toodeploy hello template.</span></span>
   4. <span data-ttu-id="d77db-155">**PowerShell** -toodeploy hello sablon használható az Azure PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="d77db-155">**PowerShell** - An Azure PowerShell script file that you can use toodeploy hello template.</span></span>
   5. <span data-ttu-id="d77db-156">**.NET** -toodeploy hello sablon használhatja A .NET-osztályt.</span><span class="sxs-lookup"><span data-stu-id="d77db-156">**.NET** - A .NET class that you can use toodeploy hello template.</span></span>
   6. <span data-ttu-id="d77db-157">**Ruby** – használható toodeploy hello sablon, A Ruby osztály.</span><span class="sxs-lookup"><span data-stu-id="d77db-157">**Ruby** - A Ruby class that you can use toodeploy hello template.</span></span>
      
      <span data-ttu-id="d77db-158">hello fájlok található hivatkozásokon keresztül érhetők hello panel között.</span><span class="sxs-lookup"><span data-stu-id="d77db-158">hello files are available through links across hello blade.</span></span> <span data-ttu-id="d77db-159">Alapértelmezés szerint hello panel hello sablon jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d77db-159">By default, hello blade displays hello template.</span></span>
      
       ![sablon megtekintése](./media/resource-manager-export-template/see-template.png)
      
<span data-ttu-id="d77db-161">Ez a sablon hello tényleges sablont toocreate használni, a web app és az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d77db-161">This template is hello actual template used toocreate your web app and SQL database.</span></span> <span data-ttu-id="d77db-162">Figyelje meg, amelyek lehetővé teszik a telepítés során eltérő értékeket tooprovide paramétert tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d77db-162">Notice it contains parameters that enable you tooprovide different values during deployment.</span></span> <span data-ttu-id="d77db-163">toolearn hello a sablonok struktúrájával kapcsolatos, kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d77db-163">toolearn more about hello structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

## <a name="export-hello-template-from-resource-group"></a><span data-ttu-id="d77db-164">Erőforráscsoport hello-sablonok exportálása</span><span class="sxs-lookup"><span data-stu-id="d77db-164">Export hello template from resource group</span></span>
<span data-ttu-id="d77db-165">Ha Ön rendelkezik manuálisan az erőforrások új és módosított erőforrások több telepítések esetén, egy sablon lekérése hello üzembe helyezési előzményeket nem tükrözi hello hello erőforráscsoport aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="d77db-165">If you have manually changed your resources or added resources in multiple deployments, retrieving a template from hello deployment history does not reflect hello current state of hello resource group.</span></span> <span data-ttu-id="d77db-166">Ez a szakasz bemutatja, hogyan tooexport a sablont, amely tükrözi hello hello erőforráscsoport aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="d77db-166">This section shows you how tooexport a template that reflects hello current state of hello resource group.</span></span> 

> [!NOTE]
> <span data-ttu-id="d77db-167">Több mint 200 erőforrással rendelkező erőforráscsoport esetében nem exportálhat sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-167">You cannot export a template for a resource group that has more than 200 resources.</span></span>
> 
> 

1. <span data-ttu-id="d77db-168">az egy erőforráscsoport, jelölje be a tooview hello sablon **automatizálási parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="d77db-168">tooview hello template for a resource group, select **Automation script**.</span></span>
   
      ![erőforráscsoportok exportálása](./media/resource-manager-export-template/select-automation.png)
   
     <span data-ttu-id="d77db-170">Erőforrás-kezelő hello erőforrások hello erőforráscsoportban kiértékeli, és létrehoz egy sablont az erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d77db-170">Resource Manager evaluates hello resources in hello resource group, and generates a template for those resources.</span></span> <span data-ttu-id="d77db-171">Nem minden erőforrástípusok hello sablon függvényt.</span><span class="sxs-lookup"><span data-stu-id="d77db-171">Not all resource types support hello export template function.</span></span> <span data-ttu-id="d77db-172">Arról, hogy nincs-e probléma hello exportálási hiba jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="d77db-172">You may see an error stating that there is a problem with hello export.</span></span> <span data-ttu-id="d77db-173">Megtudhatja, hogyan toohandle azokat problémáinak hello [javítás exportálási problémák](#fix-export-issues) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d77db-173">You learn how toohandle those issues in hello [Fix export issues](#fix-export-issues) section.</span></span>
2. <span data-ttu-id="d77db-174">Ismét megjelenik hello hat fájlok használható tooredeploy hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="d77db-174">You again see hello six files that you can use tooredeploy hello solution.</span></span> <span data-ttu-id="d77db-175">Azonban az idő hello sablon kicsit más.</span><span class="sxs-lookup"><span data-stu-id="d77db-175">However, this time hello template is a little different.</span></span> <span data-ttu-id="d77db-176">Figyelje meg, hogy a létrehozott sablon hello paramétert tartalmaz, kevesebb mint hello sablon az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="d77db-176">Notice that hello generated template contains fewer parameters than hello template in previous section.</span></span> <span data-ttu-id="d77db-177">Hello értékeket (például a hely és SKU értékek) számos is változtatható egy paraméterérték elfogadása helyett ezt a sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-177">Also, many of hello values (like location and SKU values) are hard-coded in this template rather than accepting a parameter value.</span></span> <span data-ttu-id="d77db-178">Mielőtt Ez a sablon újbóli felhasználása, érdemes lehet tooedit hello sablon toomake jobb paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="d77db-178">Before reusing this template, you might want tooedit hello template toomake better use of parameters.</span></span> 
   
3. <span data-ttu-id="d77db-179">Ezzel a sablonnal toowork folyamatos vonatkozó beállítások közül több lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="d77db-179">You have a couple of options for continuing toowork with this template.</span></span> <span data-ttu-id="d77db-180">Hello sablon letöltése, és dolgozhat rajta egy JSON-szerkesztővel helyileg.</span><span class="sxs-lookup"><span data-stu-id="d77db-180">You can either download hello template and work on it locally with a JSON editor.</span></span> <span data-ttu-id="d77db-181">Vagy hello tooyour Sablonkönyvtár mentheti, és dolgozhat rajta hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d77db-181">Or, you can save hello template tooyour library and work on it through hello portal.</span></span>
   
     <span data-ttu-id="d77db-182">Ha részeként például egy JSON-szerkesztő segítségével [Visual STUDIO Code](https://code.visualstudio.com/) vagy [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), előfordulhat, hogy inkább hello sablont helyben letöltését és a szerkesztő használatát.</span><span class="sxs-lookup"><span data-stu-id="d77db-182">If you are comfortable using a JSON editor like [VS Code](https://code.visualstudio.com/) or [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), you might prefer downloading hello template locally and using that editor.</span></span> <span data-ttu-id="d77db-183">toowork helyileg, jelölje be **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="d77db-183">toowork locally, select **Download**.</span></span>
   
      ![sablon letöltése](./media/resource-manager-export-template/download-template.png)
   
     <span data-ttu-id="d77db-185">Ha beállította a JSON-szerkesztővel, előfordulhat, hogy inkább hello portálon keresztül hello sablon szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="d77db-185">If you are not set up with a JSON editor, you might prefer editing hello template through hello portal.</span></span> <span data-ttu-id="d77db-186">hello a témakör további részei azt feltételezi, hogy a mentett hello tooyour Sablonkönyvtár hello portálon.</span><span class="sxs-lookup"><span data-stu-id="d77db-186">hello remainder of this topic assumes you have saved hello template tooyour library in hello portal.</span></span> <span data-ttu-id="d77db-187">Azonban ügyeljen a hello azonos szintaxis toohello sablon vált, hogy helyileg dolgozik, egy JSON-szerkesztő vagy hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d77db-187">However, you make hello same syntax changes toohello template whether working locally with a JSON editor or through hello portal.</span></span> <span data-ttu-id="d77db-188">hello portálon keresztül toowork válasszon **toolibrary hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d77db-188">toowork through hello portal, select **Add toolibrary**.</span></span>
   
      ![toolibrary hozzáadása](./media/resource-manager-export-template/add-to-library.png)
   
     <span data-ttu-id="d77db-190">Egy Sablonkönyvtár toohello hozzáadásakor adjon hello sablon nevét és leírását.</span><span class="sxs-lookup"><span data-stu-id="d77db-190">When adding a template toohello library, give hello template a name and description.</span></span> <span data-ttu-id="d77db-191">Ezt követően válassza a **Mentés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d77db-191">Then, select **Save**.</span></span>
   
     ![sablon értékeinek megadása](./media/resource-manager-export-template/save-library-template.png)
4. <span data-ttu-id="d77db-193">Válasszon egy sablont a könyvtárban menteni tooview **további szolgáltatások**, típus **sablonok** toofilter eredményeket, jelölje be **sablonok**.</span><span class="sxs-lookup"><span data-stu-id="d77db-193">tooview a template saved in your library, select **More services**, type **Templates** toofilter results, select **Templates**.</span></span>
   
      ![sablonok keresése](./media/resource-manager-export-template/find-templates.png)
5. <span data-ttu-id="d77db-195">Válassza ki a mentett hello nevű hello sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-195">Select hello template with hello name you saved.</span></span>
   
      ![sablon kiválasztása](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-hello-template"></a><span data-ttu-id="d77db-197">Hello sablon testreszabása</span><span class="sxs-lookup"><span data-stu-id="d77db-197">Customize hello template</span></span>
<span data-ttu-id="d77db-198">elfogadható, ha azt szeretné, hogy toocreate hello azonos web app és minden üzembe helyezés SQL-adatbázis hello exportált sablon működik.</span><span class="sxs-lookup"><span data-stu-id="d77db-198">hello exported template works fine if you want toocreate hello same web app and SQL database for every deployment.</span></span> <span data-ttu-id="d77db-199">A Resource Manager által nyújtott lehetőségek azonban lehetővé teszik, hogy a sablonokat rugalmasabb módon helyezze üzembe.</span><span class="sxs-lookup"><span data-stu-id="d77db-199">However, Resource Manager provides options so that you can deploy templates with a lot more flexibility.</span></span> <span data-ttu-id="d77db-200">Ez a cikk bemutatja, hogyan hello tooadd paramétereinek adatbázis-rendszergazdai nevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="d77db-200">This article shows you how tooadd parameters for hello database administrator name and password.</span></span> <span data-ttu-id="d77db-201">Használhatja a azonos megközelítés tooadd nagyobb rugalmasságot hello sablon más értékekre.</span><span class="sxs-lookup"><span data-stu-id="d77db-201">You can use this same approach tooadd more flexibility for other values in hello template.</span></span>

1. <span data-ttu-id="d77db-202">toocustomize hello sablon, jelölje be **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="d77db-202">toocustomize hello template, select **Edit**.</span></span>
   
     ![sablon megjelenítése](./media/resource-manager-export-template/select-edit.png)
2. <span data-ttu-id="d77db-204">Válassza ki a hello sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-204">Select hello template.</span></span>
   
     ![sablon szerkesztése](./media/resource-manager-export-template/select-added-template.png)
3. <span data-ttu-id="d77db-206">toobe képes toopass hello értékeket, hogy a telepítés során toospecify érdemes adja hozzá a következő két paraméterek toohello hello **paraméterek** hello sablon szakaszában:</span><span class="sxs-lookup"><span data-stu-id="d77db-206">toobe able toopass hello values that you might want toospecify during deployment, add hello following two parameters toohello **parameters** section in hello template:</span></span>

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. <span data-ttu-id="d77db-207">toouse hello új paramétereket, cserélje le a hello hello SQL server-definíció **erőforrások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d77db-207">toouse hello new parameters, replace hello SQL server definition in hello **resources** section.</span></span> <span data-ttu-id="d77db-208">Figyelje meg, hogy az **administratorLogin** és az **administratorLoginPassword** most paraméterértékeket használ.</span><span class="sxs-lookup"><span data-stu-id="d77db-208">Notice that **administratorLogin** and **administratorLoginPassword** now use parameter values.</span></span>

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

6. <span data-ttu-id="d77db-209">Válassza ki **OK** Ha elkészült a szerkesztési hello sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-209">Select **OK** when you are done editing hello template.</span></span>
7. <span data-ttu-id="d77db-210">Válassza ki **mentése** toosave hello módosítások toohello sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-210">Select **Save** toosave hello changes toohello template.</span></span>
   
     ![sablon mentése](./media/resource-manager-export-template/save-template.png)
8. <span data-ttu-id="d77db-212">tooredeploy frissítése hello sablon, jelölje be **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="d77db-212">tooredeploy hello updated template, select **Deploy**.</span></span>
   
     ![sablon üzembe helyezése](./media/resource-manager-export-template/redeploy-template.png)
9. <span data-ttu-id="d77db-214">Adja meg a paraméter értékét, és válasszon ki egy erőforrás csoport toodeploy hello erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d77db-214">Provide parameter values, and select a resource group toodeploy hello resources to.</span></span>


## <a name="fix-export-issues"></a><span data-ttu-id="d77db-215">Az exportálással kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="d77db-215">Fix export issues</span></span>
<span data-ttu-id="d77db-216">Nem minden erőforrástípusok hello sablon függvényt.</span><span class="sxs-lookup"><span data-stu-id="d77db-216">Not all resource types support hello export template function.</span></span> <span data-ttu-id="d77db-217">a probléma manuálisan tooresolve adja hozzá a hiányzó erőforrást hello újra üzembe a sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-217">tooresolve this issue, manually add hello missing resources back into your template.</span></span> <span data-ttu-id="d77db-218">hello hibaüzenet hello típusok nem exportálhatók tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d77db-218">hello error message includes hello resource types that cannot be exported.</span></span> <span data-ttu-id="d77db-219">Ezt az erőforrástípust a [Sablonreferenciában](/azure/templates/) találja.</span><span class="sxs-lookup"><span data-stu-id="d77db-219">Find that resource type in [Template reference](/azure/templates/).</span></span> <span data-ttu-id="d77db-220">Például toomanually virtuális hálózati átjáró hozzáadása című [Microsoft.Network/virtualNetworkGateways sablonra való hivatkozást](/azure/templates/microsoft.network/virtualnetworkgateways).</span><span class="sxs-lookup"><span data-stu-id="d77db-220">For example, toomanually add a virtual network gateway, see [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways).</span></span>

> [!NOTE]
> <span data-ttu-id="d77db-221">Az exportálási hibák csak akkor lépnek fel, ha az erőforráscsoportból, és nem az üzembe helyezési előzmények közül végez exportálást.</span><span class="sxs-lookup"><span data-stu-id="d77db-221">You only encounter export issues when exporting from a resource group rather than from your deployment history.</span></span> <span data-ttu-id="d77db-222">Ha a legutóbbi telepítés pontosan hello hello erőforráscsoport aktuális állapotát jeleníti meg, exportálnia kell hello sablon hello üzembe helyezési előzményeket és nem hello erőforráscsoportból.</span><span class="sxs-lookup"><span data-stu-id="d77db-222">If your last deployment accurately represents hello current state of hello resource group, you should export hello template from hello deployment history rather than from hello resource group.</span></span> <span data-ttu-id="d77db-223">Ha hajtott végre módosításokat toohello erőforráscsoport egyetlen sablonban nem meghatározott csak exportálja az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="d77db-223">Only export from a resource group when you have made changes toohello resource group that are not defined in a single template.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d77db-224">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d77db-224">Next steps</span></span>
<span data-ttu-id="d77db-225">Megtanulta, hogyan tooexport hello portálon létrehozott erőforrásokból sablont.</span><span class="sxs-lookup"><span data-stu-id="d77db-225">You have learned how tooexport a template from resources that you created in hello portal.</span></span>

* <span data-ttu-id="d77db-226">Sablont a [PowerShell](resource-group-template-deploy.md), az [Azure parancssori felülete](resource-group-template-deploy-cli.md) vagy a [REST API](resource-group-template-deploy-rest.md) használatával helyezhet üzembe.</span><span class="sxs-lookup"><span data-stu-id="d77db-226">You can deploy a template through [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md), or [REST API](resource-group-template-deploy-rest.md).</span></span>
* <span data-ttu-id="d77db-227">Hogyan tooexport egy sablon PowerShell használatával: toosee [az Azure PowerShell használata Azure Resource Managerrel](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="d77db-227">toosee how tooexport a template through PowerShell, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="d77db-228">Hogyan tooexport egy sablont az Azure parancssori felületen keresztül: toosee [használata hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="d77db-228">toosee how tooexport a template through Azure CLI, see [Use hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>

