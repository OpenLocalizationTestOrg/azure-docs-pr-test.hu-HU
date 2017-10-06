---
title: "egy felhasználó vagy csoport-hozzárendelést egy vállalati alkalmazás Azure Active Directoryban aaaRemove |} Microsoft Docs"
description: "Hogyan tooremove hello férhetnek hozzá a felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: c067ecf59b4dedfe8f848357ca8bd545bdc610eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Egy felhasználó vagy csoport-hozzárendelés eltávolítása egy vállalati alkalmazás Azure Active Directoryban
A felhasználó könnyen tooremove vagy folyik a hozzárendelése az Azure Active Directory (Azure AD) a vállalati alkalmazások hozzáférési tooone csoportot is. Rendelkeznie kell hello megfelelő engedélyek toomanage hello vállalati alkalmazást, és hello címtár globális rendszergazdának kell lennie.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hogyan el egy felhasználó vagy csoport-hozzárendelést?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.
3. A hello **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalati alkalmazások**.

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**. Látni fogja a kezelhető hello alkalmazások listáját.
5. A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.
6. A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **felhasználók és csoportok**.

    ![Felhasználók vagy csoportok kiválasztása](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. A hello ***appname*** **-felhasználó & csoport-hozzárendelés** panelen válasszon egyet a további felhasználókat és csoportokat, és válassza a hello **eltávolítása** parancsot. Erősítse meg a döntést, hello a parancssorból.

    ![Hello eltávolítása paranccsal](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Következő lépések
* [Összes saját csoportok](active-directory-groups-view-azure-portal.md)
* [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)
* [Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás](active-directory-coreapps-disable-app-azure-portal.md)
* [Hello nevének módosítása vagy egy vállalati alkalmazás embléma](active-directory-coreapps-change-app-logo-user-azure-portal.md)
