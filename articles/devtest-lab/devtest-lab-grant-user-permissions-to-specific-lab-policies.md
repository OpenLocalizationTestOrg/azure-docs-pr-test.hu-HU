---
title: "aaaGrant felhasználói engedélyek toospecific labor házirendeket |} Microsoft Docs"
description: "Ismerje meg, hogyan toogrant felhasználói engedélyek toospecific labor házirendeket a DevTest Labs szolgáltatásban minden egyes felhasználói igények alapján"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 35647ab837243188f06566cdf365b67fe33a3865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="grant-user-permissions-toospecific-lab-policies"></a>Felhasználói engedélyek toospecific labor házirendek
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan toouse PowerShell toogrant felhasználók engedélyek tooa adott labor házirend. Ily módon engedélyek alkalmazhatók minden felhasználói igények alapján. Például érdemes toogrant egy adott felhasználói hello képességét toochange hello VM házirend-beállításokat, de nem hello házirendek költsége.

## <a name="policies-as-resources"></a>Erőforrásként házirendek
Hello leírtaknak megfelelően [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) RBAC a cikkben lehetővé teszi, hogy az Azure-erőforrások részletes hozzáféréskezelést. Az RBAC használata, feladatokat elkülönítse a DevOps munkacsoporton belül, és hozzáférési toousers csak hello összegét adja meg, hogy be kell tooperform a feladatokat a.

A DevTest Labs szolgáltatásban, a házirend, amely lehetővé teszi a hello RBAC művelet erőforrástípus **Microsoft.DevTestLab/labs/policySets/policies/**. Minden tesztkörnyezeti házirend hello házirend erőforrástípus az erőforráshoz, és hozzárendelhető hatókör tooan RBAC szerepkörként.

Például a rendelés toogrant felhasználók olvasási/írási engedéllyel toohello **engedélyezett Virtuálisgép-méretek** házirend, akkor létre egy egyéni biztonsági szerepkört, amely kompatibilis a hello **Microsoft.DevTestLab/labs/policySets/policies/** * művelet, és rendelje hozzá hello megfelelő felhasználók toothis egyéni szerepkör hatóköre hello **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

További információ az egyéni szerepkörök az RBAC, toolearn, lásd: hello [egyéni szerepkörök hozzáférés-vezérlés](../active-directory/role-based-access-control-custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>PowerShell-lel labor egyéni szerepkör létrehozása
A lépések sorrendben tooget, szüksége lesz a következő cikket, amely alapján meghatározható tooread hello hogyan tooinstall és Azure PowerShell-parancsmagok hello konfigurálása: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Azure PowerShell-parancsmagok hello beállítása után hello a következő feladatokat végezheti el:

* Egy erőforrás-szolgáltató minden hello műveletek/műveletek listázása
* Egy adott szerepkörben műveletek listája:
* Egyéni szerepkör létrehozása

a következő PowerShell-parancsfájl hello példákat szemlélteti tooperform ezeket a feladatokat:

    ‘List all hello operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-tooa-user-for-a-specific-policy-using-custom-roles"></a>Engedélyek tooa felhasználói egy adott házirend segítségével egyéni szerepkörök hozzárendelése
Miután meghatározta a egyéni szerepkörök, hozzárendelheti azokat toousers. A sorrend tooassign egy egyéni biztonsági szerepkört tooa felhasználói, be kell szereznie hello **ObjectId** képviselő, hogy a felhasználó. használó, hello toodo **Get-AzureRmADUser** parancsmag.

A következő példa hello, hello **ObjectId** a hello *SomeUser* felhasználói 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Ha elvégezte a hello **ObjectId** hello felhasználói és egyéni szerepkör nevét, akkor is lehet rendelni a hello szerepkör toohello felhasználó **New-AzureRmRoleAssignment** parancsmagot:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Hello előző példában hello **AllowedVmSizesInLab** házirendet használja. Következő szabályzatok hello bármelyikét használhatja:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések
Egyszer Ön már biztosított felhasználói engedélyek toospecific labor házirendek, az alábbiakban néhány következő lépések tooconsider:

* [Biztonságos hozzáférés tooa labor](devtest-lab-add-devtest-user.md).
* [Laborházirendek megadása](devtest-lab-set-lab-policy.md).
* [Laborsablon létrehozása](devtest-lab-create-template.md).
* [Egyéni összetevők létrehozása a virtuális géphez](devtest-lab-artifact-author.md).
* [Adja hozzá a virtuális gép és az összetevők tooa labor](devtest-lab-add-vm-with-artifacts.md).

