---
title: "a felhasználó aaaAssign tooadministrator szerepkörök az Azure Active Directoryban |} Microsoft Docs"
description: "Azt ismerteti, hogyan toochange felhasználói felügyeleti adatokat az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: 8bb6711c2570843962fe075b6026542a4bca525f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-tooadministrator-roles-in-azure-active-directory"></a>A felhasználó az Azure Active Directoryban tooadministrator szerepkörök hozzárendelése
Ez a cikk azt ismerteti, hogyan tooassign egy rendszergazdai szerepkör tooa felhasználó az Azure Active Directory (Azure AD). A szervezetben új felhasználók hozzáadásával kapcsolatos további információkért lásd: [adja hozzá az új felhasználók tooAzure Active Directory](active-directory-users-create-azure-portal.md). A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzárendelheti a szerepkörökhöz toothem.

## <a name="assign-a-role-tooa-user"></a>A szerepkör tooa felhasználók hozzárendelése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. A hello **felhasználók és csoportok** panelen válassza **minden felhasználó**.

   ![Nyitó hello minden felhasználók panel](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. A hello **felhasználók és csoportok - minden felhasználó** panelen válassza ki a megfelelő felhasználói hello listából.
5. A kiválasztott felhasználó hello hello panelen válassza ki a **Directory szerepkör**, és hozzárendelheti a hello hello felhasználói tooa szerepkör **címtár szerepkörrel** lista. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).

      ![Felhasználói tooa szerepkör hozzárendelése](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. Kattintson a **Mentés** gombra.

## <a name="next-steps"></a>Következő lépések
* [Felhasználó hozzáadása](active-directory-users-create-azure-portal.md)
* [A felhasználó jelszavát hello új Azure-portálon](active-directory-users-reset-password-azure-portal.md)
* [A felhasználó munkahelyi adatainak módosítása](active-directory-users-work-info-azure-portal.md)
* [Felhasználói profilok kezelése](active-directory-users-profile-azure-portal.md)
* [Felhasználó törlése az Azure AD-ben](active-directory-users-delete-user-azure-portal.md)
