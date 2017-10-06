---
title: "a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban aaaAssign |} Microsoft Docs"
description: "Hogyan tooselect egy vállalati alkalmazás tooassign egy felhasználó vagy csoport tooit az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 86c11f19892b9c947a5331677c17759178ed2806
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-or-group-tooan-enterprise-app-in-azure-active-directory"></a>Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások az Azure Active Directoryban
Fontos, hogy könnyen tooassign egy felhasználó vagy egy csoport tooyour vállalati alkalmazások az Azure Active Directory (Azure AD). Rendelkeznie kell hello megfelelő engedélyek toomanage hello vállalati alkalmazást, és hello címtár globális rendszergazdának kell lennie.

## <a name="how-do-i-assign-user-access-tooan-enterprise-app"></a>Hogyan oszthatok ki a felhasználói hozzáférés tooan vállalati alkalmazást?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, Azure Active Directory hello mezőben adja meg, és válassza **Enter**.
3. A hello **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalati alkalmazások**.

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**. Látni fogja a kezelhető hello alkalmazások listáját.
5. A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.
6. A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **felhasználók és csoportok**.

    ![Minden alkalmazások parancs kiválasztásával hello](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. A hello ***appname*** **-felhasználó & csoport-hozzárendelés** panelen, jelölje be hello **Hozzáadás** parancsot.
8. A hello **hozzáadása hozzárendelés** panelen válassza **felhasználók és csoportok**.

    ![Egy felhasználó vagy csoport toohello alkalmazás hozzárendelése](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. A hello **felhasználók és csoportok** panelen, jelölje be egy vagy több felhasználók vagy csoportok hello a listában, és válassza a hello **válasszon** hello hello panel alsó részén gombra.
10. A hello **hozzáadása hozzárendelés** panelen válassza **szerepkör**. Ezután a hello **Szerepkörválasztás** panelen, jelölje ki a szerepkör tooapply toohello kijelölt felhasználók vagy csoportok, és válassza ki hello **OK** hello hello panel alsó részén gombra.
11. A hello **hozzáadása hozzárendelés** panelen, jelölje be hello **hozzárendelése** hello hello panel alsó részén gombra. hello hozzárendelt felhasználók vagy csoportok hello kijelölt szerepkör vállalati alkalmazás által meghatározott hello engedélyekkel rendelkezik.

## <a name="next-steps"></a>Következő lépések
* [Összes saját csoportok](active-directory-groups-view-azure-portal.md)
* [Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás](active-directory-coreapps-disable-app-azure-portal.md)
* [Hello nevének módosítása vagy egy vállalati alkalmazás embléma](active-directory-coreapps-change-app-logo-user-azure-portal.md)
