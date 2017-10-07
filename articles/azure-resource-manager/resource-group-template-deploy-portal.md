---
title: "az Azure portál toodeploy aaaUse Azure-erőforrások |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager toodeploy az erőforrások használatára."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 5a5217f94c8dfc0c1ebd613903ea3dcbe1197bfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a><span data-ttu-id="f5898-103">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="f5898-103">Deploy resources with Resource Manager templates and Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5898-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5898-104">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="f5898-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f5898-105">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="f5898-106">Portál</span><span class="sxs-lookup"><span data-stu-id="f5898-106">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="f5898-107">REST API</span><span class="sxs-lookup"><span data-stu-id="f5898-107">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="f5898-108">Ez a témakör bemutatja, hogyan toouse hello [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) toodeploy az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="f5898-108">This topic shows how toouse hello [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) toodeploy your Azure resources.</span></span> <span data-ttu-id="f5898-109">az erőforrások kezelése toolearn lásd [kezelése Azure-erőforrások portálon keresztül](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-109">toolearn about managing your resources, see [Manage Azure resources through portal](resource-group-portal.md).</span></span>

<span data-ttu-id="f5898-110">Nem minden szolgáltatás jelenleg hello portálon vagy az erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="f5898-110">Currently, not every service supports hello portal or Resource Manager.</span></span> <span data-ttu-id="f5898-111">Ezek a szolgáltatások toouse hello kell [klasszikus portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="f5898-111">For those services, you need toouse hello [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="f5898-112">Minden szolgáltatás hello állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="f5898-112">For hello status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="f5898-113">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5898-113">Create resource group</span></span>
1. <span data-ttu-id="f5898-114">egy üres erőforráscsoporthoz toocreate kiválasztása **új** > **felügyeleti** > **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="f5898-114">toocreate an empty resource group, select **New** > **Management** > **Resource Group**.</span></span>
   
    ![üres erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. <span data-ttu-id="f5898-116">Adjon neki egy nevet és egy helyet, és szükség esetén válasszon egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="f5898-116">Give it a name and location, and, if necessary, select a subscription.</span></span> <span data-ttu-id="f5898-117">Egy hely tooprovide hello erőforráscsoport kell, mivel hello erőforráscsoport hello erőforrások metaadatokat tárol.</span><span class="sxs-lookup"><span data-stu-id="f5898-117">You need tooprovide a location for hello resource group because hello resource group stores metadata about hello resources.</span></span> <span data-ttu-id="f5898-118">Megfelelőségi okokból érdemes lehet, hogy a metaadatokat tároló toospecify.</span><span class="sxs-lookup"><span data-stu-id="f5898-118">For compliance reasons, you may want toospecify where that metadata is stored.</span></span> <span data-ttu-id="f5898-119">Általában javasoljuk, hogy megadja a helyét, amelyben az erőforrások többségét tárolva lesz.</span><span class="sxs-lookup"><span data-stu-id="f5898-119">In general, we recommend that you specify a location where most of your resources will reside.</span></span> <span data-ttu-id="f5898-120">Hello segítségével azonos helyen egyszerűbbé teheti a sablont.</span><span class="sxs-lookup"><span data-stu-id="f5898-120">Using hello same location can simplify your template.</span></span>
   
    ![csoport értékeket](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a><span data-ttu-id="f5898-122">Erőforrások a piactér telepítése</span><span class="sxs-lookup"><span data-stu-id="f5898-122">Deploy resources from Marketplace</span></span>
<span data-ttu-id="f5898-123">Miután létrehozott egy erőforráscsoport, a piactér hello erőforrások tooit telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f5898-123">After you create a resource group, you can deploy resources tooit from hello Marketplace.</span></span> <span data-ttu-id="f5898-124">hello piactér előre definiált megoldást nyújt a gyakori forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="f5898-124">hello Marketplace provides pre-defined solutions for common scenarios.</span></span>

1. <span data-ttu-id="f5898-125">a központi telepítés toostart válassza ki **új** és hello típus erőforrás toodeploy szeretné.</span><span class="sxs-lookup"><span data-stu-id="f5898-125">toostart a deployment, select **New** and hello type of resource you would like toodeploy.</span></span> <span data-ttu-id="f5898-126">Ezután keresse meg a hello erőforrás hello adott verziójához toodeploy szeretné.</span><span class="sxs-lookup"><span data-stu-id="f5898-126">Then, look for hello particular version of hello resource you would like toodeploy.</span></span>
   
    ![erőforrás telepítése](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. <span data-ttu-id="f5898-128">Ha nem látja azt szeretné, hogy toodeploy hello adott megoldás, kereshet hello piactér azt.</span><span class="sxs-lookup"><span data-stu-id="f5898-128">If you do not see hello particular solution you would like toodeploy, you can search hello Marketplace for it.</span></span>
   
    ![a piactéren](./media/resource-group-template-deploy-portal/search-resource.png)
3. <span data-ttu-id="f5898-130">Attól függően, hogy a kiválasztott erőforrás hello típusát központi telepítés előtt vonatkozó tulajdonságok tooset gyűjteménye van.</span><span class="sxs-lookup"><span data-stu-id="f5898-130">Depending on hello type of selected resource, you have a collection of relevant properties tooset before deployment.</span></span> <span data-ttu-id="f5898-131">Ezek a lehetőségek nem itt látható, mivel azok erőforrástípus függően változhat.</span><span class="sxs-lookup"><span data-stu-id="f5898-131">Those options are not shown here, as they vary based on resource type.</span></span> <span data-ttu-id="f5898-132">Az összes ki kell választania a célként megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="f5898-132">For all types, you must select a destination resource group.</span></span> <span data-ttu-id="f5898-133">hello alábbi képen látható, hogyan toocreate egy webalkalmazást, és telepítse azt toohello létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f5898-133">hello following image shows how toocreate a web app and deploy it toohello resource group you created.</span></span>
   
    ![Erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    <span data-ttu-id="f5898-135">Másik megoldásként megadhatja, hogy toocreate erőforráscsoport az erőforrások telepítése során.</span><span class="sxs-lookup"><span data-stu-id="f5898-135">Alternatively, you can decide toocreate a resource group when deploying your resources.</span></span> <span data-ttu-id="f5898-136">Válassza ki **hozzon létre új** , és nevezze el hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f5898-136">Select **Create new** and give hello resource group a name.</span></span>
   
    ![Új erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-new-group.png)
4. <span data-ttu-id="f5898-138">A telepítés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="f5898-138">Your deployment begins.</span></span> <span data-ttu-id="f5898-139">hello telepítési eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="f5898-139">hello deployment could take a few minutes.</span></span> <span data-ttu-id="f5898-140">Amikor hello telepítés véget ért, megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="f5898-140">When hello deployment has finished, you see a notification.</span></span>
   
    ![értesítési nézet](./media/resource-group-template-deploy-portal/view-notification.png)
5. <span data-ttu-id="f5898-142">Az erőforrások való telepítése után hello használatával adhat hozzá további erőforrásokat toohello erőforráscsoport **Hozzáadás** hello erőforráscsoport panel parancs.</span><span class="sxs-lookup"><span data-stu-id="f5898-142">After deploying your resources, you can add more resources toohello resource group by using hello **Add** command on hello resource group blade.</span></span>
   
    ![erőforrás hozzáadása](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a><span data-ttu-id="f5898-144">Az egyéni sablont az erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="f5898-144">Deploy resources from custom template</span></span>
<span data-ttu-id="f5898-145">Ha szeretné, hogy a központi telepítés tooexecute, de nem használható hello sablonok hello piactér, létrehozhat egy egyéni sablont, amely meghatározza a megoldás hello infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="f5898-145">If you want tooexecute a deployment but not use any of hello templates in hello Marketplace, you can create a customized template that defines hello infrastructure for your solution.</span></span> <span data-ttu-id="f5898-146">toolearn sablonok létrehozásával kapcsolatban lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-146">toolearn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

1. <span data-ttu-id="f5898-147">toodeploy hello portálon keresztül egyéni sablont válasszon **új**, és kezdeni a keresést a **sablon-üzembehelyezés** amíg hello lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="f5898-147">toodeploy a customized template through hello portal, select **New**, and start searching for **Template Deployment** until you can select it from hello options.</span></span>
   
    ![keresési sablon-üzembehelyezés](./media/resource-group-template-deploy-portal/search-template.png)
2. <span data-ttu-id="f5898-149">Válassza ki **sablon-üzembehelyezés** hello elérhető erőforrások.</span><span class="sxs-lookup"><span data-stu-id="f5898-149">Select **Template Deployment** from hello available resources.</span></span>
   
    ![Válassza ki a sablon telepítése](./media/resource-group-template-deploy-portal/select-template.png)
3. <span data-ttu-id="f5898-151">Miután elindította hello sablon-üzembehelyezés, nyissa meg a hello üres sablont, amelyet a testreszabása.</span><span class="sxs-lookup"><span data-stu-id="f5898-151">After launching hello template deployment, open hello blank template that is available for customizing.</span></span>
   
    ![Sablon létrehozása](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    <span data-ttu-id="f5898-153">Hello szerkesztő adja hozzá a kívánt toodeploy hello erőforrások definiáló hello JSON szintaxis.</span><span class="sxs-lookup"><span data-stu-id="f5898-153">In hello editor, add hello JSON syntax that defines hello resources you want toodeploy.</span></span> <span data-ttu-id="f5898-154">Válassza ki **mentése** végzett.</span><span class="sxs-lookup"><span data-stu-id="f5898-154">Select **Save** when done.</span></span> <span data-ttu-id="f5898-155">A JSON-szintaxis hello írása útmutatóért lásd: [Resource Manager sablonokhoz](resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-155">For guidance on writing hello JSON syntax, see [Resource Manager template walkthrough](resource-manager-template-walkthrough.md).</span></span>
   
    ![sablon szerkesztése](./media/resource-group-template-deploy-portal/edit-template.png)
4. <span data-ttu-id="f5898-157">Vagy már meglévő sablon választhat hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="f5898-157">Or, you can select a pre-existing template from hello [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="f5898-158">Ezek a sablonok hello Közösség közzé.</span><span class="sxs-lookup"><span data-stu-id="f5898-158">These templates are contributed by hello community.</span></span> <span data-ttu-id="f5898-159">Számos gyakori helyzetek terjed ki, és egy hasonló toowhat toodeploy próbált-sablont hozzá valaki.</span><span class="sxs-lookup"><span data-stu-id="f5898-159">They cover many common scenarios, and someone may have added a template that is similar toowhat you are trying toodeploy.</span></span> <span data-ttu-id="f5898-160">Kereshet hello sablonok toofind, amelyet a forgatókönyvének megfelelő.</span><span class="sxs-lookup"><span data-stu-id="f5898-160">You can search hello templates toofind something that matches your scenario.</span></span>
   
    ![Válassza ki a következő gyorsindítási sablonon](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    <span data-ttu-id="f5898-162">Hello kijelölt sablon hello szerkesztőben tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="f5898-162">You can view hello selected template in hello editor.</span></span>
5. <span data-ttu-id="f5898-163">Miután megadta az összes hello más értékek, válassza ki a **létrehozása** toodeploy hello sablont.</span><span class="sxs-lookup"><span data-stu-id="f5898-163">After providing all hello other values, select **Create** toodeploy hello template.</span></span> 
   
    ![sablon üzembe helyezése](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-tooyour-account"></a><span data-ttu-id="f5898-165">Mentett tooyour fiók sablonból erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="f5898-165">Deploy resources from a template saved tooyour account</span></span>
<span data-ttu-id="f5898-166">hello portál lehetővé teszi, hogy a sablon tooyour Azure-fiók toosave, és telepítse újra később.</span><span class="sxs-lookup"><span data-stu-id="f5898-166">hello portal enables you toosave a template tooyour Azure account, and redeploy it later.</span></span> <span data-ttu-id="f5898-167">További információ ezen sablonok, mentett használata [Ismerkedés az Azure-portálon hello magánsablonok](../marketplace-consumer/mytemplates-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-167">For more information about working with these saved templates, [Get started with private templates on hello Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span></span>

1. <span data-ttu-id="f5898-168">toofind a mentett sablonok kiválasztása **Tallózás** > **sablonok**.</span><span class="sxs-lookup"><span data-stu-id="f5898-168">toofind your saved templates, select **Browse** > **Templates**.</span></span>
   
    ![Keresse meg a sablonok](./media/resource-group-template-deploy-portal/browse-templates.png)
2. <span data-ttu-id="f5898-170">A sablonok tooyour fiók mentett hello listájából válassza ki egy kívánja toowork a hello.</span><span class="sxs-lookup"><span data-stu-id="f5898-170">From hello list of templates saved tooyour account, select hello one you wish toowork on.</span></span>
   
    ![mentett sablonok](./media/resource-group-template-deploy-portal/saved-templates.png)
3. <span data-ttu-id="f5898-172">Válassza ki **telepítés** tooredeploy ez mentette a sablont.</span><span class="sxs-lookup"><span data-stu-id="f5898-172">Select **Deploy** tooredeploy this saved template.</span></span>
   
    ![mentett sablon üzembe helyezése](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a><span data-ttu-id="f5898-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5898-174">Next steps</span></span>
* <span data-ttu-id="f5898-175">tooview naplókat, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-175">tooview audit logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="f5898-176">telepítési hibák tootroubleshoot, lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-176">tootroubleshoot deployment errors, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="f5898-177">tooretrieve egy sablon, egy központi telepítés vagy az erőforráscsoportot, lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-177">tooretrieve a template from a deployment or resource group, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="f5898-178">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="f5898-178">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

