---
title: "Az Azure Active Directory B2C: Google + konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Google + az Azure Active Directory B2C által védett alkalmazások fiókkal rendelkező."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ef66eb17777acd95b5f4745ed6097c77e37663b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-google-accounts"></a>Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Google + fiókok adja meg.
## <a name="create-a-google-application"></a>Google +-alkalmazás létrehozása
toouse Google + identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Google + alapú alkalmazásba kell, és adja meg azt a hello megfelelő paraméterekkel. A Google + fiók toodo ez szükséges. Ha még nincs fiókja, beszerezheti a [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Nyissa meg toohello [Google fejlesztői konzol](https://console.developers.google.com/) , és jelentkezzen be Google + fiók hitelesítő adataival.
2. Kattintson a **Create project**, adjon meg egy **projekt neve**, és kattintson a **létrehozása**.
   
    ![Google + - első lépések](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + – új projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Kattintson a **API Manager** majd **hitelesítő adatok** a bal oldali navigációs hello.
4. Kattintson a hello **OAuth-hozzájárulás képernyő** hello felső fülre.
   
    ![Google + - hitelesítő adatok](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Válassza ki vagy adjon meg érvényes **E-mail cím**, adjon meg egy **Terméknév**, és kattintson **mentése**.
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Kattintson a **új hitelesítő adatok** majd **OAuth-Ügyfélazonosító**.
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. A **alkalmazástípus**, jelölje be **webes alkalmazás**.
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Adjon meg egy **neve** adja meg az alkalmazás `https://login.microsoftonline.com` a hello **JavaScript engedélyezett eredeteket** mező, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **jogosult átirányítási URI-azonosítók**mező. Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com). Hello **{tenant}** érték kis-és nagybetűket. Kattintson a **Create** (Létrehozás) gombra.
   
    ![Google + - ügyfél-azonosító létrehozása](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Hello értékeit másolja **ügyfél-azonosító** és **ügyfélkulcs**. Szüksége lesz mindkettő tooconfigure Google +-bérlőben identitás-szolgáltatóként. **Titkos ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.
   
    ![Google + - ügyfélkulcs](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Google + identitás-szolgáltatóként az Ön bérelt szolgáltatásának konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Adja meg például "G +".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Google**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello azonosító és az ügyfél ügyfélkulcs a korábban létrehozott Google + alkalmazás hello.
7. Kattintson a **OK** , majd **létrehozása** toosave a Google + konfigurációt.

