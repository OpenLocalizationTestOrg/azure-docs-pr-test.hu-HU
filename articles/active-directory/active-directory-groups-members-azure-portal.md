---
title: a csoport az Azure Active Directoryban aaaManage hello tagok |} Microsoft Docs
description: "Hogyan tooadd vagy a felhasználók és eszközök eltávolítása egy csoportból az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4cb16ee63828003da251423a04736f7174dd4896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Az Azure Active Directory-bérlő felhasználók csoport tagságának kezelésére
Ez a cikk azt ismerteti, hogyan toomanage hello az Azure Active Directory (Azure AD) csoport tagjai.

## <a name="how-do-i-find-hello-members-and-manage-them"></a>Hogyan hello tagok keresse meg és kezelheti azokat?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **összes csoport**.

   ![Nyitó hello csoportok panelen](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. A hello **felhasználók és csoportok – minden csoport** panelen válasszon ki egy csoportot.
5. A hello **csoport - *groupname***  panelen válassza **tagok**.

   ![Nyitó hello tagok panel](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. tooadd tagok toohello csoport, a hello **csoport - tagok** panelen válassza **tagok hozzáadása**.

   ![Tagok parancs hozzáadása](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. A hello **tagok** panelen, válasszon ki egy vagy több felhasználók vagy eszközök tooadd toohello csoport és select hello **válasszon** gomb hello panel tooadd hello alján őket toohello csoport. Hello **felhasználói** szűri hello megjelenítési megfelelő a bejegyzés tooany része egy felhasználó vagy eszköz nevét. Nem helyettesítő karakterek használata engedélyezett, hogy a mezőben.
8. hello csoportból, a hello tooremove tagok **csoport - tagok** panelen válassza ki a megfelelő tag.
9. A hello ***membername*** panelen, jelölje be hello **eltávolítása** parancsot, és erősítse meg választását hello a parancssorból.

   ![Távolítsa el a tagok parancs](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Amikor befejezte a hello csoport tagjainak módosítása, válassza ki a **mentése**.

## <a name="additional-information"></a>További információ
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [Tekintse meg a meglévő csoportok](active-directory-groups-view-azure-portal.md)
* [Hozzon létre egy új csoportot és tagok hozzáadása](active-directory-groups-create-azure-portal.md)
* [Egy csoport beállításainak kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagságát kezelése](active-directory-groups-membership-azure-portal.md)
* [A csoport dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
