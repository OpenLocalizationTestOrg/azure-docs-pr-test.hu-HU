---
title: "Hozzáférés-vezérlés aaaRole alapú hello Azure portálon |} Microsoft Docs"
description: "Ismerkedés a hozzáférés-kezelés a szerepköralapú hozzáférés-vezérlés az Azure portál hello. Szerepkör hozzárendelések tooassign engedélyek tooyour erőforrások használatára."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b87e00089b0fc93fb212b318330a6f22bfbf59e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-access-tooyour-azure-subscription-resources"></a>Szerepköralapú hozzáférés-vezérlés toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához
> [!div class="op_single_selector"]
> * [Hozzáférés kezelése felhasználó vagy csoport alapján](role-based-access-control-manage-assignments.md)
> * [Hozzáférés kezelése erőforrás alapján](role-based-access-control-configure.md)

Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz. Az RBAC használata, biztosíthat csak hello mértékű hozzáférést a felhasználóknak frissíteniük kell tooperform a munkájukat. Ez a cikk segít, amelyekből megismerheti az RBAC a hello Azure-portálon. Ha további részleteket szeretne arról, hogy hogyan segít az RBAC a hozzáférések kezelésében, tekintse meg [a szerepköralapú hozzáférés-vezérlést](role-based-access-control-what-is.md) ismertető szakaszt.

Minden előfizetésen belül biztosíthat a szerepkör-hozzárendelések too2000 fel. 

## <a name="view-access"></a>Hozzáférés megtekintése
Láthatja, hogy hozzáférési tooa erőforrás, erőforráscsoportból vagy előfizetés fő paneljén kik hello [Azure-portálon](https://portal.azure.com). Például azt szeretnénk, toosee rendelkező hozzáférés tooone erőforráscsoporthoz:

1. Válassza ki **erőforráscsoportok** hello bal oldali navigációs sávján hello.  
    ![Erőforráscsoportok – ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Hello hello erőforráscsoport nevét válassza hello **erőforráscsoportok** panelen.
3. Válassza ki **hozzáférés-vezérlés (IAM)** hello bal oldali menüből.  
4. hello Access control panel összes felhasználók, csoportok és alkalmazások, amely rendelkezik hozzáférési toohello erőforráscsoport sorolja fel.  
   
    ![Képernyőfelvétel a felhasználók panelről – örökölt vagy hozzárendelt hozzáférés](./media/role-based-access-control-configure/view-access.png)

Figyelje meg, hogy egyes szerepkörök hatóköre túl**ehhez az erőforráshoz** vannak **örökölt** azt egy másik hatókörben. Hozzáférés hozzárendelt kifejezetten toohello erőforráscsoport vagy egy hozzárendelés toohello szülő előfizetés öröklődik.

> [!NOTE]
> Hagyományos előfizetés rendszergazdák és a társadminisztrátoroknak minősülnek tulajdonosoknak hello előfizetés hello új RBAC-modellben.

## <a name="add-access"></a>Hozzáférés felvétele
Megadja a hozzáférés hello erőforrás, erőforráscsoportból vagy hello szerepkör-hozzárendelés hello hatókörében van.

1. Válassza ki **Hozzáadás** hello Access control panelen.  
2. Jelölje be hello szerepkör, hogy kívánja-e a hello tooassign **Szerepkörválasztás** panelen.
3. Válassza ki a hello felhasználó, csoport vagy alkalmazást a címtárában, amely toogrant elérésére kívánja. A megjelenített nevek, e-mail címekre és objektumazonosítókra hello directory kereshet.  
   
    ![Felhasználók hozzáadása panel – keresés – képernyőfelvétel](./media/role-based-access-control-configure/grant-access2.png)
4. Válassza ki **OK** toocreate hello hozzárendelés. Hello **felhasználó felvétele** előugró ablak nyomon követi a hello folyamatban van.  
    ![Felhasználó felvétele folyamatjelző sáv – képernyőfelvétel](./media/role-based-access-control-configure/addinguser_popup.png)

Sikerült a szerepkör-hozzárendelés felvett, az megjelenik hello **felhasználók** panelen.

## <a name="remove-access"></a>Hozzáférés megszüntetése
1. A kurzorral rámutat hello nevét, amelyet az tooremove hello hozzárendelés. A jelölőnégyzet következő toohello neve.
2. Hello jelölőnégyzetek tooselect használja egy vagy több szerepkör-hozzárendelések.
2. Válassza az **Eltávolítás** lehetőséget.  
3. Válassza ki **Igen** tooconfirm hello eltávolítása.

Az örökölt hozzárendeléseket nem lehet eltávolítani. Ha tooremove örökölt hozzárendelés van szüksége, meg kell toodo hello a hatókör, ahol a hello szerepkör-hozzárendelés létrehozása történt. A hello **hatókör** oszlop, a következő túl**örökölt** egy hivatkozás, amely toohello erőforrások ahol ezt a szerepkört rendelték. Lépjen az ott szereplő erőforrásra toohello tooremove hello szerepkör-hozzárendelés.

![Felhasználók panel – az örökölt hozzáférés letiltja az eltávolítás gombot – képernyőfelvétel](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-toomanage-access"></a>Más eszközök toomanage hozzáférés
Szerepkörök hozzárendelése, és hozzáférés az Azure RBAC-parancsokkal hello Azure-portálon kívül eszközök kezelése.  Hajtsa végre a hello hivatkozások toolearn hello előfeltételeivel kapcsolatos további, valamint Ismerkedés a hello Azure RBAC-parancsokkal.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Azure parancssori felület](role-based-access-control-manage-access-azure-cli.md)
* [REST API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Következő lépések
* [Jelentés létrehozása a hozzáférés-módosítások előzményeiről](role-based-access-control-access-change-history-report.md)
* Lásd: hello [beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)
* Saját [egyéni szerepkörök az Azure RBAC-ben](role-based-access-control-custom-roles.md)

