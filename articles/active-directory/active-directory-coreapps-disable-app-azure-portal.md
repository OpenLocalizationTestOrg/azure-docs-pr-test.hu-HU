---
title: "aaaDisable felhasználói bejelentkezések Azure Active Directory vállalati alkalmazás |} Microsoft Docs"
description: "Hogyan toodisable egy vállalati alkalmazás, hogy a felhasználók nem lehetséges, hogy jelentkezzen be az Azure Active Directoryban tooit"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 4c560b59359d433b0852a7606cc2cc0204866234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Tiltsa le a felhasználói bejelentkezéseket a vállalati alkalmazás Azure Active Directoryban
Könnyen toodisable egy vállalati alkalmazás, hogy a felhasználók nem lehetséges, hogy jelentkezzen be az Azure Active Directory (Azure AD) tooit. Rendelkeznie kell hello megfelelő engedélyek toomanage hello vállalati alkalmazást, és hello címtár globális rendszergazdának kell lennie.

## <a name="how-do-i-disable-user-sign-ins"></a>Hogyan tiltsa le a felhasználói bejelentkezéseket?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.
3. A hello **Azure Active Directory** -  ***directoryname*** (Ez azt jelenti, hogy az Azure AD hello panelen kezelt hello könyvtár) panelen válassza ki **vállalatialkalmazások**.

    ![Vállalati alkalmazások megnyitásakor](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. A hello **vállalati alkalmazások** panelen válassza **összes alkalmazás**. Kezelheti a hello alkalmazások listájának megtekintéséhez.
5. A hello **vállalati alkalmazások – összes alkalmazás** panelen, jelöljön ki egy alkalmazást.
6. A hello ***appname*** (Ez azt jelenti, hogy hello panelen kiválasztott alkalmazás hello hello címben hello nevű) panelen válassza ki **tulajdonságok**.

    ![Minden alkalmazások parancs kiválasztásával hello](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. A hello ***appname*** - **tulajdonságok** panelen válassza **nem** a **toosign lévő felhasználók számára engedélyezett?**.
8. Jelölje be hello **mentése** parancsot.

## <a name="next-steps"></a>Következő lépések
* [Összes csoport megtekintéshez](active-directory-groups-view-azure-portal.md)
* [Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)
* [Egy felhasználó vagy csoport-hozzárendelés eltávolítása a vállalati alkalmazások](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Hello nevének módosítása vagy egy vállalati alkalmazás embléma](active-directory-coreapps-change-app-logo-user-azure-portal.md)
