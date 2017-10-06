---
title: "az Azure erőforrás-hozzáférési hozzárendelések aaaView |} Microsoft Docs"
description: "Megtekintéséhez és kezeléséhez minden hello szerepköralapú hozzáférés-vezérlés hozzárendeléseinek bármely felhasználónak vagy csoportnak a hello Azure-portálon"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: ec96c9d4b9e2cb4925949b8bac78767bd564c8ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-hello-azure-portal"></a>A felhasználók és csoportok hello Azure-portálon a hozzáférés-hozzárendelések megtekintése
> [!div class="op_single_selector"]
> * [Hozzáférés kezelése felhasználó vagy csoport alapján](role-based-access-control-manage-assignments.md)
> * [Hozzáférés kezelése erőforrás alapján](role-based-access-control-configure.md)

A szerepköralapú hozzáférés-vezérlés (RBAC) hello Azure Active Directory (Azure AD), kezelheti a hozzáférést tooyour Azure erőforrásokat. 

Rendelni, amelyben a Szerepalapú hozzáférés nem részletes, mert hello engedélyeket korlátozhatja két módja van:

* **Hatókör:** RBAC szerepkör-hozzárendeléseket a rendszer hatókörön belüli tooa előfizetéshez, erőforrás vagy az erőforrás. Egy felhasználó által megadott hozzáférési tooa egyetlen erőforrás nem érhető el bármilyen egyéb erőforrások hello ugyanahhoz az előfizetéshez.
* **Szerepkör:** hello hozzárendelés hello hatókörbe hozzáférés szűkíthető van akár további által szerepkör hozzárendelése. Szerepkörök magas szintű, például a tulajdonos vagy adott, például a virtuális gép olvasó lehet.

Szerepkörök csak rendelhetők hozzá a hello előfizetés, erőforráscsoportból vagy erőforrása, amely hello hatókör hello hozzárendelés belül. De megtekintheti egy adott felhasználó vagy csoport összes hello hozzáférés hozzárendelését egyetlen helyen. Akkor is fel too2000 szerepkör-hozzárendelések minden előfizetésben. 

További információért arról, hogyan túl[szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](role-based-access-control-configure.md).

## <a name="view-access-assignments"></a>Hozzáférés-hozzárendelések megtekintése
toolook mentése hello hozzáférés hozzárendelése egy felhasználót vagy csoportot, indítsa el az Azure Active Directoryban hello [Azure-portálon](http://portal.azure.com).

1. Válassza ki **az Azure Active Directory**. Ha ezt a beállítást nem a navigációs listája látható, válassza ki a **több szolgáltatások** majd görgessen lefelé toofind **Azure Active Directory**.
2. Válassza ki **felhasználók és csoportok**, majd **minden felhasználó** vagy **összes csoport**. Ehhez a példához azt összpontosítani egyéni felhasználók számára is.
    ![Kezelheti a felhasználókat és csoportokat az Azure Active Directory – képernyőkép](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Keresés név vagy felhasználónév hello felhasználó.
4. Válassza ki **Azure-erőforrások** hello felhasználói panelen. Az adott felhasználó összes hello hozzáférés hozzárendelést jelennek meg.

### <a name="read-permissions-tooview-assignments"></a>Olvasási engedéllyel tooview hozzárendelések
Ezen az oldalon csak az hello hozzáférés hozzárendelések, hogy rendelkezik-e engedéllyel tooread. Például A olvasási hozzáférés toosubscription rendelkezik, és toohello Azure-erőforrások panel toocheck nyissa meg a felhasználó-hozzárendeléseket. Az előfizetés A hozzáférési hozzárendeléseinek látható, de nem látja, hogy ő is van a hozzáférés-hozzárendelések az előfizetésben a b kiszolgálóra.

## <a name="delete-access-assignments"></a>Hozzáférés-hozzárendelések törlése
Ezen a panelen törölheti a hozzáférési hozzárendelések közvetlenül hozzárendelt tooa felhasználó vagy csoport. Ha hello hozzáférés hozzárendelése egy szülőcsoportot öröklődött, toogo toohello erőforrás vagy előfizetés szükséges, és nincs hello hozzárendelés kezelése.

1. Az összes hello hozzáférés egy felhasználó vagy csoport hello listáról válassza ki a kívánt toodelete egy hello.
2. Válassza ki **eltávolítása** , majd **Igen** tooconfirm.
    ![Távolítsa el a hozzáférés hozzárendelése – képernyőkép](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="next-steps"></a>Következő lépések

* Ismerkedés a szerepköralapú hozzáférés-vezérlés túl[szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](role-based-access-control-configure.md)
* Lásd: hello [beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)

