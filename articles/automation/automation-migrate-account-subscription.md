---
title: "Automation-fiók és erőforrások áttelepítése |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Automation és a kapcsolódó erőforrások Automation-fiók áthelyezése egy előfizetés másik."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte
ms.openlocfilehash: 687da15bdaf854254321b59350f47549781676f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-automation-account-and-resources"></a><span data-ttu-id="3030b-103">Automation-fiók és erőforrások áttelepítése</span><span class="sxs-lookup"><span data-stu-id="3030b-103">Migrate Automation Account and resources</span></span>
<span data-ttu-id="3030b-104">Az Automation-fiókok és a kapcsolódó erőforrások (pl. eszközök, a runbookok, modulok, stb.), amely az Azure-portálon létrehozott, és szeretne áttérni az egyik erőforráscsoportból egy másikra, vagy egy előfizetésből az egy másik, ez a könnyen elvégezhető a [erőforrások áthelyezése](../azure-resource-manager/resource-group-move-resources.md) funkció érhető el az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3030b-104">For Automation accounts and its associated resources (i.e. assets, runbooks, modules, etc.) that you have created in the Azure portal and want to migrate from one resource group to another or from one subscription to another, you can accomplish this easily with the [move resources](../azure-resource-manager/resource-group-move-resources.md) feature available in the Azure portal.</span></span> <span data-ttu-id="3030b-105">Azonban ez a művelet folytatása előtt először tekintse át a következő [erőforrások áthelyezése előtt ellenőrzőlista](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) és emellett az alábbi listán az Automation jellemző.</span><span class="sxs-lookup"><span data-stu-id="3030b-105">However, before proceeding with this action, you should first review the following [checklist before moving resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) and additionally, the list below specific to Automation.</span></span>   

1. <span data-ttu-id="3030b-106">A célként megadott előfizetés-erőforráscsoport csoport forrásaként ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3030b-106">The destination subscription/resource group must be in same region as the source.</span></span>  <span data-ttu-id="3030b-107">Tehát, Automation-fiók nem lehet áthelyezni régiók között.</span><span class="sxs-lookup"><span data-stu-id="3030b-107">Meaning, Automation accounts cannot be moved across regions.</span></span>
2. <span data-ttu-id="3030b-108">Amikor helyezi át az erőforrásokat (pl. a runbookok, feladatok, stb.), mind a forrás-csoport és a célcsoport zárolva van a művelet időtartama.</span><span class="sxs-lookup"><span data-stu-id="3030b-108">When moving resources (e.g. runbooks, jobs, etc.), both the source group and the target group are locked for the duration of the operation.</span></span> <span data-ttu-id="3030b-109">Írási és törlési műveletek blokkolják a csoportok csak az áthelyezés befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="3030b-109">Write and delete operations are blocked on the groups until the move completes.</span></span>  
3. <span data-ttu-id="3030b-110">Vannak forgatókönyvek vagy változók, amelyik egy erőforrás vagy előfizetés-azonosító a meglévő előfizetésből kell frissítenie kell az áttelepítés befejezése után.</span><span class="sxs-lookup"><span data-stu-id="3030b-110">Any runbooks or variables which reference a resource or subscription ID from the existing subscription will need to be updated after migration is completed.</span></span>   

> [!NOTE]
> <span data-ttu-id="3030b-111">Ez a funkció nem támogatja a mozgóátlag klasszikus automation erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3030b-111">This feature does not support moving Classic automation resources.</span></span>
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a><span data-ttu-id="3030b-112">A portál használatával Automation-fiók áthelyezése</span><span class="sxs-lookup"><span data-stu-id="3030b-112">To move the Automation Account using the portal</span></span>
1. <span data-ttu-id="3030b-113">Az Automation-fiók kattintson **áthelyezése** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="3030b-113">From your Automation account, click **Move** at the top of the blade.</span></span><br> <span data-ttu-id="3030b-114">![Elem áthelyezése](media/automation-migrate-account-subscription/automation-menu-move.png)</span><span class="sxs-lookup"><span data-stu-id="3030b-114">![Move option](media/automation-migrate-account-subscription/automation-menu-move.png)</span></span><br>
2. <span data-ttu-id="3030b-115">Az a **erőforrások áthelyezése** panelen, vegye figyelembe, hogy azt mutatja be mind az Automation-fiók, és az erőforrás (ok) ból erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="3030b-115">On the **Move resources** blade, note that it presents resources related to both your Automation account and your resource group(s).</span></span>  <span data-ttu-id="3030b-116">Válassza ki a **előfizetés** és **erőforráscsoport** a legördülő listákból, vagy a lehetőséget választja **hozzon létre egy új erőforráscsoportot** , és írja be egy új erőforráscsoport neve mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3030b-116">Select the **subscription** and **resource group** from the drop-down lists, or select the option **create a new resource group** and enter a new resource group name in the field provided.</span></span>  
3. <span data-ttu-id="3030b-117">Tekintse át, és a jelölőnégyzet bejelölésével igazolja, hogy *megérteni az eszközök és parancsfájlok kell frissíteni, hogy új erőforrás-azonosítók használata után erőforrások* , majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="3030b-117">Review and select the checkbox to acknowledge you *understand tools and scripts will need to be updated to use new resource IDs after resources have been moved* and then click **OK**.</span></span><br> <span data-ttu-id="3030b-118">![Helyezze át az erőforrásokat panel](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span><span class="sxs-lookup"><span data-stu-id="3030b-118">![Move Resources Blade](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span></span><br>   

<span data-ttu-id="3030b-119">Ez a művelet befejezéséhez több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="3030b-119">This action will take several minutes to complete.</span></span>  <span data-ttu-id="3030b-120">A **értesítések**, akkor számára jelenik meg, hogy megtörténik - érvényesítési, az áttelepítés állapotot, majd végül amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="3030b-120">In **Notifications**, you will be presented with a status of each action that takes place - validation, migration, and then finally when it is completed.</span></span>     

## <a name="to-move-the-automation-account-using-powershell"></a><span data-ttu-id="3030b-121">A PowerShell használatával Automation-fiók áthelyezése</span><span class="sxs-lookup"><span data-stu-id="3030b-121">To move the Automation Account using PowerShell</span></span>
<span data-ttu-id="3030b-122">Használjon meglévő Automation-erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, a **Get-AzureRmResource** parancsmagot, hogy az adott Automation-fiókot, majd **Move-AzureRmResource** parancsmag az áthelyezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="3030b-122">To move existing Automation resources to another resource group or subscription, use the  **Get-AzureRmResource** cmdlet to get the specific Automation account and then **Move-AzureRmResource** cmdlet to perform the move.</span></span>

<span data-ttu-id="3030b-123">Az első példában Automation-fiók áthelyezése egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3030b-123">The first example shows how to move an Automation account to a new resource group.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

<span data-ttu-id="3030b-124">A fenti példa végrehajtása után a program kéri ellenőrizze akarja-e az adott művelet végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="3030b-124">After you execute the above code example, you will be prompted to verify you want to perform this action.</span></span>  <span data-ttu-id="3030b-125">Miután rákattintott **Igen** és folytatja a parancsfájl, addig nem kap belőle értesítéseket, működik-e az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="3030b-125">Once you click **Yes** and allow the script to proceed, you will not receive any notifications while it's performing the migration.</span></span>  

<span data-ttu-id="3030b-126">Helyezze át az új előfizetés, tartalmazza a értéket a *DestinationSubscriptionId* paraméter.</span><span class="sxs-lookup"><span data-stu-id="3030b-126">To move to a new subscription, include a value for the *DestinationSubscriptionId* parameter.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

<span data-ttu-id="3030b-127">Csakúgy, mint az előző példában kérni fogja az áthelyezés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3030b-127">As with the previous example, you will be prompted to confirm the move.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="3030b-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3030b-128">Next steps</span></span>
* <span data-ttu-id="3030b-129">További információ az erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe: [erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe](../azure-resource-manager/resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="3030b-129">For more information about moving resources to new resource group or subscription, see [Move  resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md)</span></span>
* <span data-ttu-id="3030b-130">Az Azure Automation szerepköralapú hozzáférés-vezérlési funkciójáról további információkért lásd: [Az Azure Automation szerepköralapú hozzáférés-vezérlése](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="3030b-130">For more information about Role-based Access Control in Azure Automation, refer to [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="3030b-131">Az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal kapcsolatban lásd: [Azure PowerShell használata a Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="3030b-131">To learn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="3030b-132">Az előfizetés kezelésének portál funkciókkal kapcsolatos további tudnivalókért lásd: [erőforrások kezelése az Azure portál segítségével](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3030b-132">To learn about portal features for managing your subscription, see [Using the Azure Portal to manage resources](../azure-resource-manager/resource-group-portal.md).</span></span>
