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
# <a name="migrate-automation-account-and-resources"></a>Automation-fiók és erőforrások áttelepítése
Az Automation-fiókok és a kapcsolódó erőforrások (pl. eszközök, a runbookok, modulok, stb.), amely létrehozta a hello Azure-portálon, és szeretné, hogy egy erőforrás toomigrate csoport tooanother, vagy egy előfizetés tooanother, a Ez a könnyen elvégezhető Hello [erőforrások áthelyezése](../azure-resource-manager/resource-group-move-resources.md) hello Azure-portálon szolgáltatásával. Azonban ez a művelet folytatása előtt először tekintse át a következő hello [erőforrások áthelyezése előtt ellenőrzőlista](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) és adott tooAutomation emellett hello listán.   

1. hello cél előfizetésbe/erőforráscsoportba hello forrásaként ugyanabban a régióban kell lennie.  Tehát, Automation-fiók nem lehet áthelyezni régiók között.
2. Erőforrások (pl. a runbookok, feladatok, stb.) áthelyezésekor hello forrás csoport és a célcsoport hello írásvédett hello művelet hello időtartama. Írási és törlési műveletek blokkolják hello csoportok hello áthelyezés befejezéséig.  
3. Vannak forgatókönyvek vagy változók, amelyik egy erőforrás vagy előfizetés-azonosító a hello meglévő előfizetés toobe áttelepítés befejezése után frissíteni kell.   

> [!NOTE]
> Ez a funkció nem támogatja a mozgóátlag klasszikus automation erőforrásokat.
>
>

## <a name="toomove-hello-automation-account-using-hello-portal"></a>toomove hello Automation-fiók hello portál használatával
1. Az Automation-fiók kattintson **áthelyezése** hello panel hello tetején.<br> ![Elem áthelyezése](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. A hello **erőforrások áthelyezése** panelen, vegye figyelembe, hogy erőforrásokat kapcsolódó tooboth azt mutatja be, az Automation-fiók és az erőforrás (ok) ból.  Jelölje be hello **előfizetés** és **erőforráscsoport** hello legördülő listák, vagy válassza hello beállítás **hozzon létre egy új erőforráscsoportot** , és írja be egy új erőforráscsoport neve a a megadott hello mező.  
3. Tekintse meg és jelölje be hello jelölőnégyzet tooacknowledge meg *eszközeinek és parancsfájljainak lesz megértése szükséges frissített toobe toouse új erőforrás-azonosítók erőforrások után* , majd **OK**.<br> ![Helyezze át az erőforrásokat panel](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Ez a művelet eltarthat néhány percig toocomplete.  A **értesítések**, akkor számára jelenik meg, hogy megtörténik - érvényesítési, az áttelepítés állapotot, majd végül amikor elkészült.     

## <a name="toomove-hello-automation-account-using-powershell"></a>toomove hello Automation-fiók PowerShell használatával
toomove meglévő Automation erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello **Get-AzureRmResource** parancsmag tooget hello adott Automation-fiókot, majd **Move-AzureRmResource** Helyezze át a parancsmag tooperform hello.

hello első példa bemutatja, hogyan toomove egy automatizálási fiókot tooa új erőforráscsoportot.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

Hello fent Kódpélda végrehajtása után ez a művelet tooperform kívánt felszólító tooverify fogja.  Miután rákattintott **Igen** , és teszi lehetővé a parancsfájl tooproceed hello, addig nem kap belőle értesítéseket közben hello áttelepítést hajt végre.  

toomove tooa új előfizetés, hello értéket tartalmazza *DestinationSubscriptionId* paraméter.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Hello előző példában a kért tooconfirm hello áthelyezés fogja.  

## <a name="next-steps"></a>Következő lépések
* Az erőforrások áthelyezése toonew erőforráscsoportba vagy előfizetésbe kapcsolatos további információkért lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md)
* Szerepköralapú hozzáférés-vezérlés az Azure Automationben kapcsolatos további információkért tekintse meg túl[szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).
* az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal toolearn lásd [Azure PowerShell használata a Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* az előfizetés kezelésének portál funkciókkal kapcsolatos toolearn lásd [hello Azure Portal toomanage erőforrásokat használó](../azure-resource-manager/resource-group-portal.md).
