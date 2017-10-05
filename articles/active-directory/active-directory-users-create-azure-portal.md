---
title: "Új felhasználók hozzáadása az Azure Active Directoryhoz | Microsoft Docs"
description: "Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat, vagy hogyan módosíthatja a felhasználói adatokat az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: bfe0c556d94d50207a23d2e3984371fb602e9406
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-new-users-to-azure-active-directory"></a>Új felhasználók hozzáadása az Azure Active Directoryhoz
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-users-create-azure-portal.md)
> * [klasszikus Azure portál](active-directory-create-users.md)
>
>

Ez a cikk azt ismerteti, hogyan vehet fel új felhasználókat a szervezet az Azure Active Directory (Azure AD). 

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.

   ![Nyitó felhasználók és csoportok](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. Az a **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **Hozzáadás**.

   ![Az Add parancs kiválasztása](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. Adja meg a felhasználó, például a **neve** és **felhasználónév**. A tartomány a felhasználónév részét kell lennie, a kezdeti alapértelmezett tartomány neve "foo.onmicrosoft.com" tartomány nevét, vagy egy nem összevont ellenőrzött tartomány nevét, például "contoso.com".
5. Másolja, vagy ellenkező esetben vegye figyelembe a létrehozott felhasználói jelszót, így megadhatja azt a felhasználó számára a folyamat befejezése után.
6. Másik lehetőségként megnyithatja és töltse ki a **profil** panelen a **csoportok** panelen, vagy a **címtár szerepkörrel** panel a felhasználó. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).
7. Az a **felhasználói** panelen válassza **létrehozása**.
8. Az új felhasználó számára létrehozott jelszót biztonságos terjesztése, hogy a felhasználói bejelentkezés.

### <a name="next-steps"></a>Következő lépések
* [Külső felhasználó hozzáadásához](active-directory-users-create-external-azure-portal.md)
* [A felhasználó jelszavát az új Azure-portálon](active-directory-users-reset-password-azure-portal.md)
* [A felhasználó munkahelyi adatainak módosítása](active-directory-users-work-info-azure-portal.md)
* [Felhasználói profilok kezelése](active-directory-users-profile-azure-portal.md)
* [Felhasználó törlése az Azure AD-ben](active-directory-users-delete-user-azure-portal.md)
* [Felhasználó hozzárendelése egy szerepkörhöz az Azure AD-ben](active-directory-users-assign-role-azure-portal.md)
