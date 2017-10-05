---
title: "A csoportok a csoport beletartozik az Azure Active Directoryban kezelése |} Microsoft Docs"
description: "Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Megtudhatja, hogyan kezelheti az adott tagságok."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08e04a6590176c4084ca47b4bd6cbb22500eca2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Kezelheti az Azure Active Directory-bérlőhöz tartozik egy csoport mely csoportok
Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Megtudhatja, hogyan kezelheti az adott tagságok.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Hogyan találhatom meg a csoportok a csoport egy tagja?
1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. Az a **felhasználók és csoportok** panelen válassza **összes csoport**.

   ![A csoportok panel megnyitása](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. Az a **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.
5. Az a **csoport - *groupname***  panelen válassza **csoporttagságok**.

   ![A csoport tagságát panel megnyitása](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. A csoport tagja másik csoportnak, hozzáadja a a **Group - csoporttagságok** panelen válassza a **Hozzáadás** parancsot.
7. Válasszon csoportot a **csoport kijelölése** panelt, és válassza a **válasszon** gomb a panel alján. Egyszerre csak egy csoporthoz adhat hozzá a csoporthoz. A **felhasználói** mezőben szűrése a képernyőt a megfelelő felhasználó vagy eszköz nevek a bejegyzést. Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.

   ![A csoporttagság hozzáadása](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. A csoport eltávolításához egy másik csoport tagjaként a a **Group - csoporttagságok** panelen válasszon ki egy csoportot.
9. Az a ***groupname*** panelen válassza a **eltávolítása** parancsot, és erősítse meg választását a parancssorba.

   ![Távolítsa el a tagság parancs](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Amikor befejezte a csoport csoporttagság módosítása, válassza ki a **mentése**.

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Hozzon létre egy új csoportot és tagok hozzáadása](active-directory-groups-create-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagjai kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
