---
title: "Felhasználó törlése az Azure Active Directoryban olyan könyvtárból |} Microsoft Docs"
description: "Ismerteti az Azure Active Directory a felhasználók és az adatok törlése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bd1c9ccc-2d80-42bf-82cc-db37bb1a1d67
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: 19a4d2c37c785b3b56a2e0e1b6797ec56dad9835
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="delete-a-user-from-a-directory-in-azure-active-directory"></a>A felhasználó töröl egy könyvtárat az Azure Active Directoryban
Ez a cikk azt ismerteti, hogyan felhasználó törlése az Azure Active Directory (Azure AD) olyan könyvtárból. A szervezetben új felhasználók hozzáadásával kapcsolatos további információkért lásd: [új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-azure-portal.md).

## <a name="to-delete-a-user"></a>Felhasználó törlése
1. Jelentkezzen be [az Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.

   ![Nyitó felhasználók kezelése](./media/active-directory-users-delete-user-azure-portal/create-users-user-management.png)
3. Az a **felhasználók és csoportok** panelen válassza **felhasználók**.

   ![A felhasználók panel megnyitása](./media/active-directory-users-delete-user-azure-portal/create-users-open-users-blade.png)
4. A **Felhasználók és csoportok – Felhasználók** panelen válasszon ki egy felhasználót a listából.
5. A kiválasztott felhasználó paneljén válassza **áttekintése**, majd a parancssávon válassza **törlése**.

    ![A Delete parancs kiválasztása](./media/active-directory-users-delete-user-azure-portal/create-users-delete-command.png)

## <a name="next-steps"></a>Következő lépések
* [Új felhasználók hozzáadása az Azure Active Directoryhoz](active-directory-users-create-azure-portal.md)
* [Az Azure Active Directoryban a felhasználó jelszavának visszaállítása](active-directory-users-reset-password-azure-portal.md)
* [Felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök](active-directory-users-assign-role-azure-portal.md)
* [Adja hozzá, vagy egy Azure Active Directoryban lévő felhasználói profil adatainak módosítása](active-directory-users-work-info-azure-portal.md)
* [A felhasználó töröl egy könyvtárat az Azure Active Directoryban](active-directory-users-profile-azure-portal.md)
