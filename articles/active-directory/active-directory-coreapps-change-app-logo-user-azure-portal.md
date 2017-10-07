---
title: "aaaChange hello nevét vagy egy vállalati alkalmazás Azure Active Directoryban embléma |} Microsoft Docs"
description: "Hogyan toochange hello nevét vagy az Azure Active Directoryban egyéni vállalati alkalmazások embléma"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d01303ce-e6cb-4f3b-a4d6-ec29dfd68146
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: b660e1f0f6c7ffd626e17e2e3399e7169f6e8bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-name-or-logo-of-an-enterprise-app-in-azure-active-directory"></a>Hello nevének módosítása vagy egy vállalati alkalmazás Azure Active Directoryban embléma
Egyszerű toochange hello nevét vagy egy egyéni vállalati alkalmazás Azure Active Directory (Azure AD) embléma. Rendelkeznie kell hello megfelelő engedélyek toomake ezeket a módosításokat, és be kell hello hello egyéni alkalmazás hozta létre.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Hogyan változtathatom meg egy vállalati alkalmazás nevét vagy embléma?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.
3. A hello **Azure Active Directory - *directoryname***  (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalati alkalmazások**.

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)
4. A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**. Látni fogja a kezelhető hello alkalmazások listáját.
5. A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.
6. A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **tulajdonságok**.

    ![Hello Tulajdonságok parancs kiválasztása](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)
7. A hello ***appname*** **-tulajdonságok** panelen, tallózással keresse meg a fájl toouse, mint egy új embléma, vagy hello alkalmazás neve, vagy mindkettőt szerkesztése.

    ![Hello app embléma vagy nameproperties parancs módosítása](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)
8. Jelölje be hello **mentése** parancsot.

## <a name="next-steps"></a>Következő lépések
* [Összes saját csoportok](active-directory-groups-view-azure-portal.md)
* [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)
* [Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás](active-directory-coreapps-disable-app-azure-portal.md)
