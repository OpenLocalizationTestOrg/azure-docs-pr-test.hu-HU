---
title: "Azure-erőforrások telepítése Azure portál használatával |} Microsoft Docs"
description: "Azure-portál és az Azure Resource Manager segítségével az erőforrások telepítése."
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
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a><span data-ttu-id="cc2e6-103">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="cc2e6-103">Deploy resources with Resource Manager templates and Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cc2e6-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cc2e6-104">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="cc2e6-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cc2e6-105">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="cc2e6-106">Portál</span><span class="sxs-lookup"><span data-stu-id="cc2e6-106">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="cc2e6-107">REST API</span><span class="sxs-lookup"><span data-stu-id="cc2e6-107">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="cc2e6-108">Ez a témakör bemutatja, hogyan használja a [Azure-portálon](https://portal.azure.com) rendelkező [Azure Resource Manager](resource-group-overview.md) központi telepítése az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-108">This topic shows how to use the [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) to deploy your Azure resources.</span></span> <span data-ttu-id="cc2e6-109">Az erőforrások kezelésével kapcsolatos információkért lásd: [kezelése Azure-erőforrások portálon keresztül](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-109">To learn about managing your resources, see [Manage Azure resources through portal](resource-group-portal.md).</span></span>

<span data-ttu-id="cc2e6-110">Jelenleg nem minden szolgáltatás támogatja, a portál vagy az erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-110">Currently, not every service supports the portal or Resource Manager.</span></span> <span data-ttu-id="cc2e6-111">Ezek a szolgáltatások kell használnia a [klasszikus portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-111">For those services, you need to use the [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="cc2e6-112">Minden szolgáltatás állapotát, lásd: [az Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-112">For the status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="cc2e6-113">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2e6-113">Create resource group</span></span>
1. <span data-ttu-id="cc2e6-114">Hozzon létre egy üres erőforráscsoportot, válassza ki **új** > **felügyeleti** > **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-114">To create an empty resource group, select **New** > **Management** > **Resource Group**.</span></span>
   
    ![üres erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. <span data-ttu-id="cc2e6-116">Adjon neki egy nevet és egy helyet, és szükség esetén válasszon egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-116">Give it a name and location, and, if necessary, select a subscription.</span></span> <span data-ttu-id="cc2e6-117">Meg kell adnia az erőforrásnak a helyét, mert az erőforráscsoport erőforrásokra vonatkozó metaadatokat tárol.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-117">You need to provide a location for the resource group because the resource group stores metadata about the resources.</span></span> <span data-ttu-id="cc2e6-118">Megfelelőségi okokból érdemes lehet, hogy metaadatai tárolási helyének meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-118">For compliance reasons, you may want to specify where that metadata is stored.</span></span> <span data-ttu-id="cc2e6-119">Általában javasoljuk, hogy megadja a helyét, amelyben az erőforrások többségét tárolva lesz.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-119">In general, we recommend that you specify a location where most of your resources will reside.</span></span> <span data-ttu-id="cc2e6-120">Ugyanazon a helyen használatával egyszerűbbé teheti a sablont.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-120">Using the same location can simplify your template.</span></span>
   
    ![csoport értékeket](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a><span data-ttu-id="cc2e6-122">Erőforrások a piactér telepítése</span><span class="sxs-lookup"><span data-stu-id="cc2e6-122">Deploy resources from Marketplace</span></span>
<span data-ttu-id="cc2e6-123">Miután létrehozott egy erőforráscsoport, a piactérről erőforrások telepítheti azt.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-123">After you create a resource group, you can deploy resources to it from the Marketplace.</span></span> <span data-ttu-id="cc2e6-124">A piactér előre definiált megoldást nyújt a gyakori forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-124">The Marketplace provides pre-defined solutions for common scenarios.</span></span>

1. <span data-ttu-id="cc2e6-125">A telepítés elindításához válassza ki a **új** és a telepíteni kívánt erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-125">To start a deployment, select **New** and the type of resource you would like to deploy.</span></span> <span data-ttu-id="cc2e6-126">Ezután keresse meg az adott verzióját szeretné telepíteni az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-126">Then, look for the particular version of the resource you would like to deploy.</span></span>
   
    ![erőforrás telepítése](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. <span data-ttu-id="cc2e6-128">Ha nem látja az adott megoldás szeretne telepíteni, akkor is kereshet a piactér azt.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-128">If you do not see the particular solution you would like to deploy, you can search the Marketplace for it.</span></span>
   
    ![a piactéren](./media/resource-group-template-deploy-portal/search-resource.png)
3. <span data-ttu-id="cc2e6-130">Attól függően, hogy a kiválasztott erőforrás típusát hogy egy központi telepítés előtt állítsa be a megfelelő tulajdonságok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-130">Depending on the type of selected resource, you have a collection of relevant properties to set before deployment.</span></span> <span data-ttu-id="cc2e6-131">Ezek a lehetőségek nem itt látható, mivel azok erőforrástípus függően változhat.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-131">Those options are not shown here, as they vary based on resource type.</span></span> <span data-ttu-id="cc2e6-132">Az összes ki kell választania a célként megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-132">For all types, you must select a destination resource group.</span></span> <span data-ttu-id="cc2e6-133">A következő kép bemutatja, hogyan hozzon létre egy webalkalmazást, és telepítse azt a létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-133">The following image shows how to create a web app and deploy it to the resource group you created.</span></span>
   
    ![Erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    <span data-ttu-id="cc2e6-135">Azt is megteheti Ha dönt, hogy hozzon létre egy erőforráscsoportot az erőforrások telepítése során.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-135">Alternatively, you can decide to create a resource group when deploying your resources.</span></span> <span data-ttu-id="cc2e6-136">Válassza ki **hozzon létre új** , és nevezze el az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-136">Select **Create new** and give the resource group a name.</span></span>
   
    ![Új erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-new-group.png)
4. <span data-ttu-id="cc2e6-138">A telepítés megkezdése.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-138">Your deployment begins.</span></span> <span data-ttu-id="cc2e6-139">A központi telepítés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-139">The deployment could take a few minutes.</span></span> <span data-ttu-id="cc2e6-140">A telepítés befejezését, megjelenik egy értesítés.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-140">When the deployment has finished, you see a notification.</span></span>
   
    ![értesítési nézet](./media/resource-group-template-deploy-portal/view-notification.png)
5. <span data-ttu-id="cc2e6-142">Az erőforrások való telepítése után adhat hozzá további erőforrásokat az erőforráscsoport segítségével a **Hozzáadás** parancs az erőforráscsoport panel.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-142">After deploying your resources, you can add more resources to the resource group by using the **Add** command on the resource group blade.</span></span>
   
    ![erőforrás hozzáadása](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a><span data-ttu-id="cc2e6-144">Az egyéni sablont az erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="cc2e6-144">Deploy resources from custom template</span></span>
<span data-ttu-id="cc2e6-145">Ha szeretné a központi telepítés hajtható végre, de nem használja a sablonok a piactéren, létrehozhat egy egyéni sablont, amely meghatározza a megoldás infrastruktúráját.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-145">If you want to execute a deployment but not use any of the templates in the Marketplace, you can create a customized template that defines the infrastructure for your solution.</span></span> <span data-ttu-id="cc2e6-146">Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-146">To learn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

1. <span data-ttu-id="cc2e6-147">Egy egyéni sablon a portálon keresztül történő üzembe helyezéséhez válassza **új**, és kezdeni a keresést a **sablon-üzembehelyezés** mindaddig, amíg a lehetőségek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-147">To deploy a customized template through the portal, select **New**, and start searching for **Template Deployment** until you can select it from the options.</span></span>
   
    ![keresési sablon-üzembehelyezés](./media/resource-group-template-deploy-portal/search-template.png)
2. <span data-ttu-id="cc2e6-149">Válassza ki **sablon-üzembehelyezés** elérhető erőforrások.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-149">Select **Template Deployment** from the available resources.</span></span>
   
    ![Válassza ki a sablon telepítése](./media/resource-group-template-deploy-portal/select-template.png)
3. <span data-ttu-id="cc2e6-151">Miután elindította a sablon-üzembehelyezés, nyissa meg az üres sablon testreszabása érhetők el.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-151">After launching the template deployment, open the blank template that is available for customizing.</span></span>
   
    ![Sablon létrehozása](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    <span data-ttu-id="cc2e6-153">A szerkesztő vegye fel a JSON-szintaxis, amely meghatározza a telepíteni kívánt erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-153">In the editor, add the JSON syntax that defines the resources you want to deploy.</span></span> <span data-ttu-id="cc2e6-154">Válassza ki **mentése** végzett.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-154">Select **Save** when done.</span></span> <span data-ttu-id="cc2e6-155">A JSON-szintaxis írásáról útmutatóért lásd: [Resource Manager sablonokhoz](resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-155">For guidance on writing the JSON syntax, see [Resource Manager template walkthrough](resource-manager-template-walkthrough.md).</span></span>
   
    ![sablon szerkesztése](./media/resource-group-template-deploy-portal/edit-template.png)
4. <span data-ttu-id="cc2e6-157">Vagy választhat egy már meglévő sablon, a [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-157">Or, you can select a pre-existing template from the [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="cc2e6-158">Ezeket a sablonokat a Közösség által közzé.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-158">These templates are contributed by the community.</span></span> <span data-ttu-id="cc2e6-159">Számos gyakori helyzetek terjed ki, és valaki hozzáadta a sablont, amely hasonló mi próbál telepíteni.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-159">They cover many common scenarios, and someone may have added a template that is similar to what you are trying to deploy.</span></span> <span data-ttu-id="cc2e6-160">A sablonok található, amelyet a forgatókönyvének megfelelő kereshet.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-160">You can search the templates to find something that matches your scenario.</span></span>
   
    ![Válassza ki a következő gyorsindítási sablonon](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    <span data-ttu-id="cc2e6-162">A kiválasztott sablont a szerkesztőben tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-162">You can view the selected template in the editor.</span></span>
5. <span data-ttu-id="cc2e6-163">Miután megadta a többi érték, válassza ki a **létrehozása** a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-163">After providing all the other values, select **Create** to deploy the template.</span></span> 
   
    ![sablon üzembe helyezése](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a><span data-ttu-id="cc2e6-165">Egy sablonból fiókjába erőforrások telepítése</span><span class="sxs-lookup"><span data-stu-id="cc2e6-165">Deploy resources from a template saved to your account</span></span>
<span data-ttu-id="cc2e6-166">A portál lehetővé teszi egy sablon mentése az Azure-fiókjával, és telepítse újra később.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-166">The portal enables you to save a template to your Azure account, and redeploy it later.</span></span> <span data-ttu-id="cc2e6-167">További információ ezen sablonok, mentett használata [Ismerkedés az Azure portálon magánsablonok](../marketplace-consumer/mytemplates-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-167">For more information about working with these saved templates, [Get started with private templates on the Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span></span>

1. <span data-ttu-id="cc2e6-168">A mentett sablonok találhatók, válassza ki a **Tallózás** > **sablonok**.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-168">To find your saved templates, select **Browse** > **Templates**.</span></span>
   
    ![Keresse meg a sablonok](./media/resource-group-template-deploy-portal/browse-templates.png)
2. <span data-ttu-id="cc2e6-170">Fiókjába sablonok listájának megtekintéséhez jelölje ki a használni kívánt egy.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-170">From the list of templates saved to your account, select the one you wish to work on.</span></span>
   
    ![mentett sablonok](./media/resource-group-template-deploy-portal/saved-templates.png)
3. <span data-ttu-id="cc2e6-172">Válassza ki **telepítés** újratelepíteni ezt a mentett sablont.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-172">Select **Deploy** to redeploy this saved template.</span></span>
   
    ![mentett sablon üzembe helyezése](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a><span data-ttu-id="cc2e6-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc2e6-174">Next steps</span></span>
* <span data-ttu-id="cc2e6-175">Naplók megtekintése: [naplózási műveletek a Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-175">To view audit logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="cc2e6-176">Telepítési hibák elhárításához lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-176">To troubleshoot deployment errors, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="cc2e6-177">Egy központi telepítés vagy az erőforráscsoport a sablon lekéréséhez lásd: [Azure Resource Manager sablon exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="cc2e6-177">To retrieve a template from a deployment or resource group, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="cc2e6-178">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="cc2e6-178">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

