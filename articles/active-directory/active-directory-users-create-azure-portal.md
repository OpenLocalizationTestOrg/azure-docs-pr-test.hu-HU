---
title: "aaaAdd új felhasználók tooAzure Active Directory |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd új felhasználók, vagy módosítsa a felhasználói adatokat az Azure Active Directoryban."
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
ms.openlocfilehash: c4a156ea31b81202bb0d0ac224afbfc3f1785532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-tooazure-active-directory"></a>Adja hozzá az új felhasználók tooAzure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-users-create-azure-portal.md)
> * [klasszikus Azure portál](active-directory-create-users.md)
>
>

Ez a cikk azt ismerteti, hogyan tooadd új felhasználók az Ön szervezetében hello Azure Active Directory (Azure AD). 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók és csoportok](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **Hozzáadás**.

   ![Hello hozzáadása paranccsal](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. Adja meg például a részleteit hello felhasználói **neve** és **felhasználónév**. hello tartomány neve hello felhasználónév részét kell hello kezdeti alapértelmezett tartomány neve "foo.onmicrosoft.com" tartomány nevét, vagy egy nem összevont ellenőrzött tartomány nevét, például "contoso.com".
5. Másolat vagy más módon Megjegyzés hello által létrehozott felhasználói jelszó, így megadhatja azt toohello felhasználói a folyamat befejezése után.
6. Igény szerint megnyitásához, és a hello hello adatok **profil** panelen, hello **csoportok** panel vagy hello **Directory szerepkör** hello felhasználói paneljét. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).
7. A hello **felhasználói** panelen válassza **létrehozása**.
8. Biztonságosan elosztani a generált hello jelszó toohello új felhasználó, így hello felhasználói bejelentkezés.

### <a name="next-steps"></a>Következő lépések
* [Külső felhasználó hozzáadásához](active-directory-users-create-external-azure-portal.md)
* [A felhasználó jelszavát hello új Azure-portálon](active-directory-users-reset-password-azure-portal.md)
* [A felhasználó munkahelyi adatainak módosítása](active-directory-users-work-info-azure-portal.md)
* [Felhasználói profilok kezelése](active-directory-users-profile-azure-portal.md)
* [Felhasználó törlése az Azure AD-ben](active-directory-users-delete-user-azure-portal.md)
* [Egy felhasználó tooa szerepkör hozzárendelése az Azure AD-ben](active-directory-users-assign-role-azure-portal.md)
