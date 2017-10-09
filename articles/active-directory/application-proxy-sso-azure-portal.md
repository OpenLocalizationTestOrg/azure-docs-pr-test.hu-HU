---
title: "az Azure AD alkalmazásproxy aaaSingle bejelentkezés tooapps |} Microsoft Docs"
description: "Kapcsolja be az egyszeri bejelentkezés az Azure AD alkalmazásproxy hello Azure-portálon a közzétett helyszíni alkalmazásokhoz."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5ff288d36163b74215677d9e34c93c985ac33d54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Az egyszeri bejelentkezés az alkalmazásproxy vaulting jelszó

Az Azure Active Directory Alkalmazásproxyjával segít a helyszíni alkalmazások közzététele, hogy a távoli alkalmazottak biztonságosan érhetik el azokat, túl által a termelékenység növelése. Hello Azure-portálon, a is beállíthat egyszeri bejelentkezés (SSO) toothese alkalmazásokat. A felhasználók csak az Azure ad-val tooauthenticate kell, és anélkül toosign újra a vállalati alkalmazás eléréséhez.

Alkalmazásproxy támogatja több [az egyszeri bejelentkezési módok](application-proxy-sso-overview.md). Jelszó alapú bejelentkezés készült alkalmazásokat, amelyek egy felhasználónév/jelszó kombináció használják a hitelesítéshez. Konfigurálásakor jelszó alapú bejelentkezés az alkalmazáshoz, a felhasználók toosign egyszer toohello a helyszíni alkalmazások rendelkeznek. Ezt követően az Azure Active Directory hello bejelentkezési adatait tárolja, és automatikusan biztosítja azt toohello alkalmazás amikor a felhasználók távolról el. 

Kell már rendelkezik közzétett és tesztelni az alkalmazást az alkalmazásproxy. Ha nem, kövesse hello [alkalmazások közzététele az Azure AD-alkalmazásproxy használatával](application-proxy-publish-azure-portal.md) majd térjen vissza ide. 

## <a name="set-up-password-vaulting-for-your-application"></a>Az alkalmazás vaulting jelszó beállítása

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) rendszergazdaként.
2. Válassza ki **Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás**.
3. Hello listájából válassza ki, amelyet az egyszeri bejelentkezési modellel mentése tooset hello alkalmazást.  
4. Válassza ki **egyszeri bejelentkezés**.

   ![Válassza ki az egyszeri bejelentkezés](./media/application-proxy-sso-azure-portal/select-sso.png)

5. Hello SSO módot, válassza a **jelszóalapú bejelentkezés**.
6. Bejelentkezési URL-címhez hello meg hello URL-címet hello lap ahol felhasználók adja meg a felhasználónevet és jelszót toosign alkalmazásban tooyour hello vállalati hálózaton kívülről. Lehet, hogy a külső URL-cím hello app proxyn keresztül történő közzétételekor létrehozott hello. 

   ![Válassza a jelszó alapú bejelentkezés, és írja be az URL-cím](./media/application-proxy-sso-azure-portal/password-sso.png)

7. Kattintson a **Mentés** gombra.

<!-- Need toorepro?
7. hello page should tell you that a sign-in form was successfully detected at hello provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow hello instructions toopoint out where hello sign-in credentials go. 
-->

## <a name="test-your-app"></a>Az alkalmazás tesztelése

Nyissa meg a távelérés tooyour alkalmazáshoz konfigurált tooexternal URL-CÍMÉT. Jelentkezzen be a hitelesítő adatokat az adott alkalmazáshoz (vagy egy olyan fiókot, amely hozzáféréssel rendelkező beállítása hello hitelesítő adatait). Amikor bejelentkezik sikeresen tudja tooleave hello alkalmazás, és térjen vissza a hitelesítő adatok ismételt beírása nélkül. 

## <a name="next-steps"></a>Következő lépések

- Olvassa el, egyéb módon tooimplement [egyszeri bejelentkezéshez az alkalmazásproxy](application-proxy-sso-overview.md)
- További tudnivalók [távolról az Azure AD alkalmazásproxy alkalmazásokhoz fér hozzá biztonsági szempontjai](application-proxy-security-considerations.md)