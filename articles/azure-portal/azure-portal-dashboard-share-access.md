---
title: "aaaShare Azure portál irányítópultok RBAC használatával |} Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan tooshare egy irányítópult hello szerepköralapú hozzáférés-vezérlés használatával Azure-portálon."
services: azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: b12f9f8582596ee14aa8bfdfb4772cc139e3bf45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Azure irányítópultok megosztása a szerepköralapú hozzáférés-vezérlés használatával
Miután egy irányítópultot, tegye közzé, és megoszthatják más felhasználókkal a szervezetében. Engedélyezni másoknak tooview az Azure használatával irányítópult [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md). Egy felhasználót vagy csoportot a felhasználók tooa szerepkör hozzárendelése, és ez a szerepkör határozza meg, hogy azoknak a felhasználóknak megtekintheti és módosíthatja a közzétett irányítópulton hello. 

Az összes közzétett irányítópultok Azure-erőforrások, mint kerülnek végrehajtásra, ami azt jelenti, hogy az előfizetés kezelhető elemeiként létezik, és egy erőforráscsoport található.  Egy hozzáférés-vezérlő szempontjából irányítópultok ugyanazok, mint más erőforrások, például egy virtuális gép vagy egy tárfiókot.

> [!TIP]
> Egyes csempék hello irányítópult kényszerítése a saját megjelenő hello erőforrások alapuló hozzáférés-vezérlési követelményeinek.  Ezért tervezhet meg egy irányítópultot, amely meg van osztva körben közben továbbra is az egyes csempék hello adatainak védelméhez.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Irányítópultok ismertetése hozzáférés-vezérlés
A szerepköralapú hozzáférés-vezérlés (RBAC), a felhasználók tooroles hatókör három különböző szinteken hozzárendelhet:

* előfizetést
* erőforráscsoport
* Erőforrás

hello engedélyeknek le toohello erőforrás előfizetés öröklődnek. hello közzétett irányítópulton erőforrás. Ezért előfordulhat, hogy már felhasználóira tooroles hello előfizetés amely hello közzétett irányítópult is működik. 

Íme egy példa.  Tegyük fel, Azure-előfizetéssel rendelkezik, és a csoport tagjai különféle hozzárendelt hello szerepkörök **tulajdonos**, **közreműködő**, vagy **olvasó** hello az előfizetéshez. Felhasználókat, akik a tulajdonos vagy közreműködő szerepkörrel rendelkező személyek is képes toolist, nézet, létrehozása, módosítása vagy törlése irányítópultok hello előfizetésen belül.  Felhasználók, akik olvasók képes toolist és nézet irányítópultokat, amelyek nem, de módosíthatja vagy törölheti őket is.  Olvasási joggal rendelkező felhasználók is képes toomake helyi szerkesztést tooa közzétett irányítópulton (például, amikor egy probléma), azonban nem tudja toopublish e módosítások hátsó toohello kiszolgáló.  Rendelkeznek hello beállítás toomake hello irányítópult titkos másolatát maguknak

Azonban engedélyek toohello tartalmazó erőforráscsoportot több irányítópultok vagy tooan egyéni irányítópult is rendelheti. Dönthet például úgy, hogy a felhasználók egy csoportjánál kell korlátozott engedélyekkel hello előfizetés, de nagyobb tooa adott irányítópulton keresztül. Az irányítópult ezeket felhasználók tooa szerepkört rendel. 

## <a name="publish-dashboard"></a>Irányítópult közzététele
Tegyük fel, amelyet tooshare rendelkező felhasználók csoportja az előfizetésében irányítópult beállítása után. hello lépéseket jelzik a tárolási kezelők nevű egyéni csoport, de nevet adhat a csoport függetlenül szeretné. További információ az Active Directory-csoport létrehozása és felhasználók toothat csoport hozzáadása: [csoportkezelés az Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

1. Hello irányítópulton, válassza ki a **megosztás**.
   
     ![Válasszon megosztást](./media/azure-portal-dashboard-share-access/select-share.png)
2. Hozzáférés engedélyezése előtt közzé kell tennie hello irányítópult. Alapértelmezés szerint a hello irányítópult lesz közzétett tooa erőforráscsoportot **irányítópultok**. Válassza ki **közzététele**.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Az irányítópult már közzé van téve. Ha hello előfizetésből örökölt hello engedélyeket alkalmasak, nem kell toodo semmi további. A munkahely más felhasználóinak képes tooaccess kell, és a hello irányítópult előfizetés szintű szerepkör alapján. Azonban ebben az oktatóanyagban most is kiadhatjuk egy csoportnak, hogy az irányítópult felhasználók tooa szerepkör.

## <a name="assign-access-tooa-dashboard"></a>Hozzáférés tooa irányítópult hozzárendelése
1. Miután közzétette hello irányítópultot, válassza ki a **felhasználók kezelése**.
   
     ![felhasználók kezelése](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Látni fogja, amely már hozzá vannak rendelve egy szerepkört ehhez az irányítópulthoz a meglévő felhasználók listáját. A meglévő felhasználók listáját az alábbi képen hello eltérő lesz. Nagy valószínűséggel hello hozzárendelések hello előfizetés öröklődnek. tooadd egy új felhasználót vagy csoportot, válassza ki a **Hozzáadás**.
   
     ![felhasználó hozzáadása](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Válassza ki azt szeretné, hogy toogrant hello engedélyek jelölő hello szerepkör. Ehhez a példához válassza ki a **közreműködő**.
   
     ![szerepkör kiválasztása](./media/azure-portal-dashboard-share-access/select-role.png)
4. Válassza ki a hello felhasználót vagy csoportot, hogy kívánja-e tooassign toohello szerepkör. Ha nem látja hello felhasználó vagy csoport hello listájában keresi, hello keresőmező használata. Az Active Directoryban létrehozott hello csoportok függ a rendelkezésre álló csoportok listájából.
   
     ![felhasználó kiválasztása](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Ha befejezte a felhasználók vagy csoportok hozzáadása, válassza ki a **OK**. 
6. hello új hozzárendelés kerül toohello azoknak a felhasználóknak. Figyelje meg, hogy a **hozzáférés** jelenik meg, **hozzárendelt** helyett **örökölt**.
   
     ![rendelt szerepkörök](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Következő lépések
* A szerepkörök listájáért lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).
* erőforrások kezelése toolearn lásd [kezelése Azure-erőforrások portálon keresztül](resource-group-portal.md).

