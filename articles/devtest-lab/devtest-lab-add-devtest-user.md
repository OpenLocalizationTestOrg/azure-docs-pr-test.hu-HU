---
title: "aaaAdd tulajdonosa és a felhasználók a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban hello Azure-portálon vagy a PowerShell használatával"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: 2a98f5fe1efbd7c23e0d97f58f47c37462aed3b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Adja hozzá a tulajdonosok és a felhasználók a Azure DevTest Labs szolgáltatásban
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Hozzáférés az Azure DevTest Labs vezérli [átruházásához hozzáférés-vezérlés (RBAC)](../active-directory/role-based-access-control-what-is.md). Szerepalapú használ, akkor is elkülönítse azon belül a csapat a *szerepkörök* ahol számára biztosítson hozzáférést szükséges toousers tooperform csak hello mennyisége a munkájukat. Közül három ezeket az RBAC-szerepkörök *tulajdonos*, *DevTest Labs felhasználói*, és *közreműködő*. Ebből a cikkből megismerheti milyen műveletek végezhetők az egyes hello három fő RBAC szerepkört. Ott, megismerheti, hogyan tooadd felhasználók tooa lab - mindkét keresztül hello portál és a PowerShell-parancsfájlt, és hogyan tooadd felhasználók hello előfizetés szintjéről.

## <a name="actions-that-can-be-performed-in-each-role"></a>Az egyes szerepkörökhöz végrehajtható műveletek
Az a felhasználó hozzárendelheti három fő szerepkörök állnak rendelkezésre:

* Tulajdonos
* DevTest Labs felhasználó
* Közreműködő

hello következő táblázat bemutatja, hogy ezek a szerepkörök a felhasználók által végrehajtható műveletek hello:

| **A szerepet betöltő felhasználók műveleteket hajthat végre.** | **DevTest Labs felhasználó** | **Tulajdonos** | **Közreműködő** |
| --- | --- | --- | --- |
| **Labor feladatok** | | | |
| Felhasználók tooa labor hozzáadása |Nem |Igen |Nem |
| Költség-beállítások frissítése |Nem |Igen |Igen |
| **Alapszintű VM-feladatok** | | | |
| Hozzáadhat és eltávolíthat egyéni lemezképek |Nem |Igen |Igen |
| Hozzáadása, módosítása és törlése képletek |Igen |Igen |Igen |
| Engedélyezett Azure piactéren elérhető rendszerkép |Nem |Igen |Igen |
| **VM-feladatok** | | | |
| Virtuális gépek létrehozása |Igen |Igen |Igen |
| Indítás, Leállítás, és törölje a virtuális gépek |Csak virtuális gépek hello felhasználó által létrehozott |Igen |Igen |
| Virtuális gép házirendek frissítése |Nem |Igen |Igen |
| Az adatlemezek és a virtuális gépek hozzáadása |Csak virtuális gépek hello felhasználó által létrehozott |Igen |Igen |
| **Összetevő-feladatok** | | | |
| Hozzáadhat és eltávolíthat összetevő adattárak |Nem |Igen |Igen |
| Az összetevők alkalmazása |Igen |Igen |Igen |

> [!NOTE]
> Amikor egy felhasználó létrehoz egy virtuális Gépet, a felhasználónak lesz automatikusan hozzárendelve toohello **tulajdonos** létrehozott virtuális gép hello szerepe.
> 
> 

## <a name="add-an-owner-or-user-at-hello-lab-level"></a>Tulajdonos vagy felhasználó hozzáadása hello labor szinten
Tulajdonosa és a felhasználók hello Azure-portálon keresztül hello labor szintjén adhatók meg. Ez magában foglalja a külső felhasználók érvényes [Microsoft-fiók (msa-t)](devtest-lab-faq.md#what-is-a-microsoft-account).
a lépéseket követve hello ismerteti egy tulajdonos vagy a felhasználó tooa labor hozzáadása az Azure DevTest Labs hello folyamatot:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
3. Labs hello listában jelölje ki hello kívánt labor.
4. Hello labor paneljén válassza **konfigurációs**. 
5. A hello **konfigurációs** panelen válassza **felhasználók**.
6. A hello **felhasználók** panelen válassza **+ Hozzáadás**.
   
    ![Felhasználó hozzáadása](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. A hello **Szerepkörválasztás** panelen, jelölje be hello kívánt szerepkört. a szakasz hello [szerepkörönként végrehajtott műveletekből](#actions-that-can-be-performed-in-each-role) listák hello különböző hello tulajdonos, DevTest felhasználói és közreműködő szerepkört felhasználók által végrehajtható műveleteket.
8. A hello **felhasználók hozzáadása az** panelen hello e-mail címet, vagy azt szeretné, hogy a megadott hello szerepkör tooadd hello felhasználó nevét adja meg. Hello a felhasználó nem található, egy hibaüzenet ismerteti, hogy ha hello probléma. Hello felhasználó található, ha, hogy a felhasználó szerepel és van kiválasztva. 
9. Válassza ki **válasszon**.
10. Válassza ki **OK** tooclose hello **hozzáférés hozzáadása** panelen.
11. Amikor visszatér toohello **felhasználók** panelen hello felhasználó hozzá lett adva.  

## <a name="add-an-external-user-tooa-lab-using-powershell"></a>Adja hozzá egy külső felhasználó tooa labor PowerShell használatával
Ezenkívül hello Azure-portálon a felhasználók tooadding, hozzáadhat egy külső felhasználó tooyour, amikor egy PowerShell-parancsfájlt használ. Hello a következő példában egyszerűen módosítsa hello paraméterértékeket a hello **értékek toochange** megjegyzés.
Hello le `subscriptionId`, `labResourceGroup`, és `labName` hello labor panel az Azure-portálon hello értékeit.

> [!NOTE]
> hello mintaparancsfájl azt feltételezi, hogy adott hello megadott felhasználó hozzá lett adva, a Vendég toohello Active Directory, és sikertelen lesz, ha a rendszer hello eset. egy felhasználó nem szerepel a hello Active Directory tooa labor tooadd hello Azure portál tooassign hello felhasználói tooa szerepkört használjon, hello szakaszban ismertetett módon [egy tulajdonos vagy a felhasználó hozzáadása hello labor szinten](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role tooa lab
    # Ensure that guest users can be added toohello Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values toochange
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select hello Azure subscription that contains hello lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve hello user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create hello role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-hello-subscription-level"></a>Tulajdonos vagy felhasználó hozzáadása hello előfizetés szintjén
A hatókör toochild szülőhatókörben az Azure-ban Azure engedélyek továbbítja. Ezért az Azure-előfizetéssel, amely tartalmazza a labs tulajdonosai a program automatikusan e labs tulajdonosai. Emellett saját hello virtuális gépek és egyéb erőforrások hello labor felhasználók és hello Azure DevTest Labs szolgáltatás által létrehozott. 

Hello adhat hozzá további tulajdonosok tooa labor keresztül hello labor panel [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040). Azonban hello hozzáadott, a tulajdonos felügyeleti hatóköre sokkal szűkebb hello előfizetés tulajdonosa hatókör-nál. Például hello hozzáadott tulajdonosok nem rendelkezik teljes körű hozzáférési toosome hello erőforrások hello DevTest Labs szolgáltatás által létrehozott hello előfizetésben. 

tooadd egy tulajdonos tooan Azure-előfizetésre, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Válassza ki **több szolgáltatások**, majd válassza ki **előfizetések** hello listából.
3. Válassza ki a kívánt hello előfizetést.
4. Válassza ki **hozzáférés** ikonra. 
   
    ![Használó felhasználók](./media/devtest-lab-add-devtest-user/access-users.png)
5. A hello **felhasználók** panelen válassza **Hozzáadás**.
   
    ![Felhasználó hozzáadása](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. A hello **Szerepkörválasztás** panelen válasszon ki **tulajdonos**.
7. A hello **felhasználók hozzáadása** panelen adja meg az üdvözlő e-mail cím vagy név hello felhasználó tulajdonos tooadd kívánt. Ha hello a felhasználó nem található, hello probléma ismertető hibaüzenetet kap. Hello felhasználó található, ha a felhasználó megtalálható-e e hello **felhasználói** szövegmezőben.
8. Hello található felhasználónév kiválasztása.
9. Válassza ki **válasszon**.
10. Válassza ki **OK** tooclose hello **hozzáférés hozzáadása** panelen.
11. Amikor visszatér toohello **felhasználók** panelen hello felhasználó tulajdonos hozzáadva. Ez a felhasználó már létrehozott az előfizetéshez tartozó bármely labs tulajdonosa, és így képes tooperform tulajdonos feladatok. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

