---
title: "aaaAdd új felhasználók tooAzure Active Directory |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd új felhasználók az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 6ca413c84a7a5238a30fd26fc751d687d827b24a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-new-users-tooazure-active-directory"></a>Gyors üzembe helyezés: Új felhasználók tooAzure Active Directory hozzáadása
Ez a cikk azt ismerteti, hogyan tooadd a szervezet hello Azure Active Directory (Azure AD) az új felhasználói hello Azure-portál használatával egyszerre, vagy a helyszíni Windows Server AD-felhasználó szinkronizálásával fiók adatait. 

## <a name="add-cloud-based-users"></a>Adja hozzá a felhőalapú felhasználók
1. Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **Azure Active Directory** , majd **felhasználók és csoportok**.
3. A hello **felhasználók és csoportok** panelen válassza **minden felhasználó**, majd válassza ki **új felhasználó**.
   ![Hello hozzáadása paranccsal](./media/add-users-azure-active-directory/add-user.png)
4. Adja meg például a részleteit hello felhasználói **neve** és **felhasználónév**. hello tartomány hello felhasználónév részét kell hello kezdeti alapértelmezett tartomány neve "[neve].onmicrosoft.com", vagy egy ellenőrzött és érvényes, nem összevont [egyéni tartománynév](add-custom-domain.md) például "contoso.com".
5. Másolat vagy más módon Megjegyzés hello által létrehozott felhasználói jelszó, így megadhatja azt toohello felhasználói a folyamat befejezése után.
6. Igény szerint megnyitásához, és a hello hello adatok **profil** panelen, hello **csoportok** panel vagy hello **Directory szerepkör** hello felhasználói paneljét. A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).
7. A hello **felhasználói** panelen válassza **létrehozása**.
8. Biztonságosan elosztani a generált hello jelszó toohello új felhasználó, így hello felhasználói bejelentkezés.

> [!TIP]
> Is szinkronizálhatja a helyszíni Windows Server AD a felhasználói fiók adatait. A Microsoft identity megoldások span a helyszíni és felhőalapú képességek, létrehozása egy felhasználói azonosítót a hitelesítéshez és engedélyezéshez tooall erőforrások helyétől függetlenül. A hibrid identitás nevezzük. [Az Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) lehet használt toointegrate a helyszíni címtárak és az Azure Active Directory hibrid identitáskezelési környezetekben. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazások a felhasználók közös identitás tooprovide. 

## <a name="delete-users-from-azure-ad"></a>Azure ad-felhasználók törlése
1. Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **felhasználók és csoportok**.
3. A hello **felhasználók és csoportok** panelen, jelölje be hello felhasználói toodelete hello listából. 
4. A kiválasztott felhasználó hello hello panelen válassza ki a **áttekintése**, majd a hello parancssávon válassza **törlése**.
   ![Hello hozzáadása paranccsal](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Részletek 
* [Külső felhasználó hozzáadásához](active-directory-users-create-external-azure-portal.md)

* [Egy felhasználó tooa szerepkör hozzárendelése az Azure AD-ben](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Következő lépések
A gyors üzembe helyezés, hogy megtanulta, hogyan tooadd új felhasználók tooAzure prémium szintű AD. 

Hello hivatkozás toocreate új felhasználó követően az Azure AD hello Azure-portálon is használhatja.

> [!div class="nextstepaction"]
> [Felhasználók tooAzure AD hozzá](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
