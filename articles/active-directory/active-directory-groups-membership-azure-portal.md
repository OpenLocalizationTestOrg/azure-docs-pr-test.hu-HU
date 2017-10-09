---
title: a csoport tartozik tooin Azure Active Directory aaaManage hello csoportok |} Microsoft Docs
description: "Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Itt hogyan toomanage adott tagságok."
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
ms.openlocfilehash: d0a0a1967084de0968e1e802559f9cdfd7ca6ae4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-toowhich-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Az Azure Active Directory-bérlőhöz tartozik egy csoport toowhich csoportok kezelése
Csoportok tartalmazhatnak más csoportok az Azure Active Directoryban. Itt hogyan toomanage adott tagságok.

## <a name="how-do-i-find-hello-groups-my-group-is-a-member-of"></a>Hogyan találhatom hello csoportok a csoport egy tagja?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **összes csoport**.

   ![Nyitó hello csoportok panelen](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. A hello **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.
5. A hello **csoport - *groupname***  panelen válassza **csoporttagságok**.

   ![Hello tagságát panel megnyitása](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. tooadd a csoport egy másik csoportot, a hello tagjaként **Group - csoporttagságok** panelen, jelölje be hello **Hozzáadás** parancsot.
7. Jelöljön ki egy csoportot hello **csoport kijelölése** panelen, és jelölje ki hello **válasszon** hello hello panel alsó részén gombra. A csoport tooonly egy csoport egyszerre is hozzáadhat. Hello **felhasználói** szűri hello megjelenítési megfelelő a bejegyzés tooany része egy felhasználó vagy eszköz nevét. Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.

   ![A csoporttagság hozzáadása](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. tooremove a csoport egy másik csoportot, a hello tagjaként **Group - csoporttagságok** panelen válasszon ki egy csoportot.
9. A hello ***groupname*** panelen, jelölje be hello **eltávolítása** parancsot, és erősítse meg választását hello a parancssorból.

   ![Távolítsa el a tagság parancs](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Amikor befejezte a csoport csoporttagság módosítása, válassza ki a **mentése**.

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Hozzon létre egy új csoportot és tagok hozzáadása](active-directory-groups-create-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagjai kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
