---
title: "Szerepköralapú hozzáférés-vezérlést (RBAC) toocontrol aaaAzure hozzáférési jogok toocreate és támogatási kérelmek kezelése |} Microsoft Docs"
description: "Azure szerepköralapú hozzáférés-vezérlés (RBAC) toocontrol hozzáférési jogok toocreate és támogatási kérelmek kezelése"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: c68a699ac870fa6bf371deb8ed0424848f39acf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-based-access-control-rbac-toocontrol-access-rights-toocreate-and-manage-support-requests"></a>Azure szerepköralapú hozzáférés-vezérlés (RBAC) toocontrol hozzáférési jogok toocreate és támogatási kérelmek kezelése

[Szerepköralapú hozzáférés-vezérlés (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) lehetővé teszi a részletes hozzáféréskezelést az Azure-bA.
Támogatja a kérelem létrehozását az Azure-portálon hello [portal.azure.com](https://portal.azure.com), használja az Azure RBAC modell toodefine hozhat létre és kezelhet a támogatási kérelmek számára.
A hozzáférés hello megfelelő RBAC szerepkör toousers, csoportok és alkalmazások egy bizonyos hatókörhöz, amely lehet egy előfizetést, erőforráscsoport vagy egy erőforrás hozzárendelésével.

Vegyünk egy példát: egy erőforrás csoport tulajdonosának olvasási engedély hello előfizetés hatókörben, kezelheti a hello erőforráscsoport, például webhelyekhez, virtuális gépek és alhálózatok tartozó összes hello erőforrást.
Azonban ha toocreate egy támogatási kérést elleni hello virtuálisgép-erőforrást, tapasztal hello a következő hiba

![Előfizetés hiba](./media/create-manage-support-requests-using-access-control/subscription-error.png)

A támogatási kérelem beépülő modulban kell írási engedélye, vagy hello által betöltött szerepkör támogatja a következő hello előfizetés hatókör toobe képes toocreate Microsoft.Support/* műveletet, és a támogatási kérelmek kezelése.

hello a következő cikk ismerteti, hogyan használhatja az Azure egyéni szerepköralapú hozzáférés-vezérlést (RBAC) toocreate és hello Azure-portálon a támogatási kérelmek kezelése.

## <a name="getting-started"></a>Első lépések

A fenti hello példáját lehetővé válik képes toocreate egy támogatási kérést az erőforrás hello előfizetés tulajdonosa által hozzárendelt hello előfizetésben egyéni RBAC szerepkör volt.
[Egyéni RBAC-szerepkörök](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) Azure PowerShell, az Azure parancssori felület (CLI) és a REST API hello segítségével hozhatók létre.

az egy egyéni biztonsági szerepkört a hello műveletek tulajdonság határozza meg, hello Azure üzemeltetése toowhich hello szerepkör engedélyezi a hozzáférést.
egy egyéni biztonsági szerepkört a támogatási kérelem felügyeleti toocreate, hello szerepkörnek rendelkeznie kell a hello művelet Microsoft.Support/*

Íme egy példa egy egyéni biztonsági szerepkört toocreate használja, és kezelheti a kérelmek támogatásához.
Jelenleg ez a szerepkör "Támogatási kérelem közreműködői" nevű, és hogyan irányítjuk toohello egyéni szerepkör ebben a cikkben.

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Hello című rész lépéseit követve [Ez a videó](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn hogyan toocreate egy egyéni biztonsági szerepkört az előfizetés.

## <a name="create-and-manage-support-requests-in-hello-azure-portal"></a>Hozzon létre és hello Azure-portálon a támogatási kérelmek kezelése

Vegyünk egy példát – áll hello tulajdonos előfizetés "Visual Studio MSDN-előfizetés."
Joe a partner, aki egy erőforrás tulajdonosa toosome hello erőforráscsoportok ebben az előfizetésben és rendelkezik-e olvasási engedéllyel toohello előfizetés.
Toogive hozzáférés tooyour társ, Joe, hello képességét toocreate kívánja és hello erőforrások az előfizetéshez tartozó támogatási jegyek kezelése.

1. hello első lépéseként toogo toohello előfizetés és a "Beállítások" felhasználók listájának megtekintéséhez. Kattintson a hello felhasználói Joe hello előfizetés olvasási hozzáféréssel rendelkező, és most rendelje hozzá egy új egyéni biztonsági szerepkört toohim.

    ![Szerepkör hozzáadása](./media/create-manage-support-requests-using-access-control/add-role.png)

2. Kattintson a "Hozzáadás" hello "Felhasználók" panel alatt. Válassza ki a hello egyéni szerepkör "Támogatási kérelem közreműködői" a hello szerepkörök

    ![Támogatási közreműködői szerepkör hozzáadása](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Miután kiválasztotta a hello szerepkör nevét, a "Hozzáadás felhasználók", és írja be a hello Joe e-mail hitelesítő adatokhoz. Kattintson a "Select"

    ![Felhasználók hozzáadása](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Kattintson az "Ok" tooproceed

    ![Hozzáférés hozzáadása](./media/create-manage-support-requests-using-access-control/add-access.png)

5. Ekkor megjelenik a hello felhasználói hello újonnan hozzáadott egyéni szerepkör "Támogatási kérelem közreműködői" hello előfizetésben, amelynek hello tulajdonosa

    ![A hozzáadott felhasználónak](./media/create-manage-support-requests-using-access-control/user-added.png)

    Amikor Joe hello portal bejelentkezik, látja hello előfizetés toowhich ő hozzá lett adva.

7. Joe "Új támogatási kérelem" hello "Súgó és támogatása" paneljéről kattint, és hozhat létre támogatási kérelmek "Visual Studio Ultimate az MSDN"

    ![Új támogatási kérelem](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. Kattintson az "Összes kérelmek támogatásához" Joe hello listája látható a támogatási kérelmek létrehozása ehhez az előfizetéshez ![eset Részletek nézet](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-hello-azure-portal"></a>Távolítsa el a támogatási kérelem hozzáférés hello Azure-portálon

Ugyanúgy, mint lehetséges toogrant hozzáférés tooa felhasználói toocreate és támogatási kérelmek kezelése, akkor lehetséges tooremove hozzáférést, valamint a hello felhasználó számára.
tooremove képességét toocreate hello és támogatási kérelmek kezelése nyissa toohello előfizetés, kattintson a "Beállítások" és kattintson az hello felhasználónak (jelen esetben Joe).
Kattintson a jobb gombbal a hello szerepkör nevét, "Támogatási kérelem közreműködői", és kattintson az "Eltávolítás"

![Támogatási kérelem hozzáférés](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

Ha Joe toohello portálon naplózza, és megpróbál toocreate egy támogatási kérést, ő észlel hello a következő hiba

![Előfizetés hiba – 2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Joe nem jelennek meg kérelmek támogatásához, ha "Az összes kérelmek támogatásához" elemre kattint

![eset adatainak megtekintése – 2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
