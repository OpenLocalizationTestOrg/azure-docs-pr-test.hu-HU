---
title: "aaaMigrate Automation-fiók és a források |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toomove egy automatizálási fiókot az Azure Automation és a kapcsolódó erőforrásokat egy előfizetés tooanother a."
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
ms.openlocfilehash: 201c9091cd2d78d7ea407c1e5fb27f366bb4fa8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-automation-account-and-resources"></a><span data-ttu-id="5ded9-103">Automation-fiók és erőforrások áttelepítése</span><span class="sxs-lookup"><span data-stu-id="5ded9-103">Migrate Automation Account and resources</span></span>
<span data-ttu-id="5ded9-104">Az Automation-fiókok és a kapcsolódó erőforrások (pl. eszközök, a runbookok, modulok, stb.), amely létrehozta a hello Azure-portálon, és szeretné, hogy egy erőforrás toomigrate csoport tooanother, vagy egy előfizetés tooanother, a Ez a könnyen elvégezhető Hello [erőforrások áthelyezése](../azure-resource-manager/resource-group-move-resources.md) hello Azure-portálon szolgáltatásával.</span><span class="sxs-lookup"><span data-stu-id="5ded9-104">For Automation accounts and its associated resources (i.e. assets, runbooks, modules, etc.) that you have created in hello Azure portal and want toomigrate from one resource group tooanother or from one subscription tooanother, you can accomplish this easily with hello [move resources](../azure-resource-manager/resource-group-move-resources.md) feature available in hello Azure portal.</span></span> <span data-ttu-id="5ded9-105">Azonban ez a művelet folytatása előtt először tekintse át a következő hello [erőforrások áthelyezése előtt ellenőrzőlista](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) és adott tooAutomation emellett hello listán.</span><span class="sxs-lookup"><span data-stu-id="5ded9-105">However, before proceeding with this action, you should first review hello following [checklist before moving resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) and additionally, hello list below specific tooAutomation.</span></span>   

1. <span data-ttu-id="5ded9-106">hello cél előfizetésbe/erőforráscsoportba hello forrásaként ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5ded9-106">hello destination subscription/resource group must be in same region as hello source.</span></span>  <span data-ttu-id="5ded9-107">Tehát, Automation-fiók nem lehet áthelyezni régiók között.</span><span class="sxs-lookup"><span data-stu-id="5ded9-107">Meaning, Automation accounts cannot be moved across regions.</span></span>
2. <span data-ttu-id="5ded9-108">Erőforrások (pl. a runbookok, feladatok, stb.) áthelyezésekor hello forrás csoport és a célcsoport hello írásvédett hello művelet hello időtartama.</span><span class="sxs-lookup"><span data-stu-id="5ded9-108">When moving resources (e.g. runbooks, jobs, etc.), both hello source group and hello target group are locked for hello duration of hello operation.</span></span> <span data-ttu-id="5ded9-109">Írási és törlési műveletek blokkolják hello csoportok hello áthelyezés befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="5ded9-109">Write and delete operations are blocked on hello groups until hello move completes.</span></span>  
3. <span data-ttu-id="5ded9-110">Vannak forgatókönyvek vagy változók, amelyik egy erőforrás vagy előfizetés-azonosító a hello meglévő előfizetés toobe áttelepítés befejezése után frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="5ded9-110">Any runbooks or variables which reference a resource or subscription ID from hello existing subscription will need toobe updated after migration is completed.</span></span>   

> [!NOTE]
> <span data-ttu-id="5ded9-111">Ez a funkció nem támogatja a mozgóátlag klasszikus automation erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5ded9-111">This feature does not support moving Classic automation resources.</span></span>
>
>

## <a name="toomove-hello-automation-account-using-hello-portal"></a><span data-ttu-id="5ded9-112">toomove hello Automation-fiók hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="5ded9-112">toomove hello Automation Account using hello portal</span></span>
1. <span data-ttu-id="5ded9-113">Az Automation-fiók kattintson **áthelyezése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="5ded9-113">From your Automation account, click **Move** at hello top of hello blade.</span></span><br> <span data-ttu-id="5ded9-114">![Elem áthelyezése](media/automation-migrate-account-subscription/automation-menu-move.png)</span><span class="sxs-lookup"><span data-stu-id="5ded9-114">![Move option](media/automation-migrate-account-subscription/automation-menu-move.png)</span></span><br>
2. <span data-ttu-id="5ded9-115">A hello **erőforrások áthelyezése** panelen, vegye figyelembe, hogy erőforrásokat kapcsolódó tooboth azt mutatja be, az Automation-fiók és az erőforrás (ok) ból.</span><span class="sxs-lookup"><span data-stu-id="5ded9-115">On hello **Move resources** blade, note that it presents resources related tooboth your Automation account and your resource group(s).</span></span>  <span data-ttu-id="5ded9-116">Jelölje be hello **előfizetés** és **erőforráscsoport** hello legördülő listák, vagy válassza hello beállítás **hozzon létre egy új erőforráscsoportot** , és írja be egy új erőforráscsoport neve a a megadott hello mező.</span><span class="sxs-lookup"><span data-stu-id="5ded9-116">Select hello **subscription** and **resource group** from hello drop-down lists, or select hello option **create a new resource group** and enter a new resource group name in hello field provided.</span></span>  
3. <span data-ttu-id="5ded9-117">Tekintse meg és jelölje be hello jelölőnégyzet tooacknowledge meg *eszközeinek és parancsfájljainak lesz megértése szükséges frissített toobe toouse új erőforrás-azonosítók erőforrások után* , majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ded9-117">Review and select hello checkbox tooacknowledge you *understand tools and scripts will need toobe updated toouse new resource IDs after resources have been moved* and then click **OK**.</span></span><br> <span data-ttu-id="5ded9-118">![Helyezze át az erőforrásokat panel](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span><span class="sxs-lookup"><span data-stu-id="5ded9-118">![Move Resources Blade](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span></span><br>   

<span data-ttu-id="5ded9-119">Ez a művelet eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5ded9-119">This action will take several minutes toocomplete.</span></span>  <span data-ttu-id="5ded9-120">A **értesítések**, akkor számára jelenik meg, hogy megtörténik - érvényesítési, az áttelepítés állapotot, majd végül amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="5ded9-120">In **Notifications**, you will be presented with a status of each action that takes place - validation, migration, and then finally when it is completed.</span></span>     

## <a name="toomove-hello-automation-account-using-powershell"></a><span data-ttu-id="5ded9-121">toomove hello Automation-fiók PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5ded9-121">toomove hello Automation Account using PowerShell</span></span>
<span data-ttu-id="5ded9-122">toomove meglévő Automation erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello **Get-AzureRmResource** parancsmag tooget hello adott Automation-fiókot, majd **Move-AzureRmResource** Helyezze át a parancsmag tooperform hello.</span><span class="sxs-lookup"><span data-stu-id="5ded9-122">toomove existing Automation resources tooanother resource group or subscription, use hello  **Get-AzureRmResource** cmdlet tooget hello specific Automation account and then **Move-AzureRmResource** cmdlet tooperform hello move.</span></span>

<span data-ttu-id="5ded9-123">hello első példa bemutatja, hogyan toomove egy automatizálási fiókot tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5ded9-123">hello first example shows how toomove an Automation account tooa new resource group.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

<span data-ttu-id="5ded9-124">Hello fent Kódpélda végrehajtása után ez a művelet tooperform kívánt felszólító tooverify fogja.</span><span class="sxs-lookup"><span data-stu-id="5ded9-124">After you execute hello above code example, you will be prompted tooverify you want tooperform this action.</span></span>  <span data-ttu-id="5ded9-125">Miután rákattintott **Igen** , és teszi lehetővé a parancsfájl tooproceed hello, addig nem kap belőle értesítéseket közben hello áttelepítést hajt végre.</span><span class="sxs-lookup"><span data-stu-id="5ded9-125">Once you click **Yes** and allow hello script tooproceed, you will not receive any notifications while it's performing hello migration.</span></span>  

<span data-ttu-id="5ded9-126">toomove tooa új előfizetés, hello értéket tartalmazza *DestinationSubscriptionId* paraméter.</span><span class="sxs-lookup"><span data-stu-id="5ded9-126">toomove tooa new subscription, include a value for hello *DestinationSubscriptionId* parameter.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

<span data-ttu-id="5ded9-127">Hello előző példában a kért tooconfirm hello áthelyezés fogja.</span><span class="sxs-lookup"><span data-stu-id="5ded9-127">As with hello previous example, you will be prompted tooconfirm hello move.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="5ded9-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ded9-128">Next steps</span></span>
* <span data-ttu-id="5ded9-129">Az erőforrások áthelyezése toonew erőforráscsoportba vagy előfizetésbe kapcsolatos további információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="5ded9-129">For more information about moving resources toonew resource group or subscription, see [Move  resources toonew resource group or subscription](../azure-resource-manager/resource-group-move-resources.md)</span></span>
* <span data-ttu-id="5ded9-130">Szerepköralapú hozzáférés-vezérlés az Azure Automationben kapcsolatos további információkért tekintse meg túl[szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5ded9-130">For more information about Role-based Access Control in Azure Automation, refer too[Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="5ded9-131">az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal toolearn lásd [Azure PowerShell használata a Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="5ded9-131">toolearn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="5ded9-132">az előfizetés kezelésének portál funkciókkal kapcsolatos toolearn lásd [hello Azure Portal toomanage erőforrásokat használó](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5ded9-132">toolearn about portal features for managing your subscription, see [Using hello Azure Portal toomanage resources](../azure-resource-manager/resource-group-portal.md).</span></span>
