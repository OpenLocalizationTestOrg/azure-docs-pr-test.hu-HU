---
title: "Az Azure Active Directory B2C: Facebook konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers az Azure Active Directory B2C által védett alkalmazások Facebook-fiókkal rendelkező."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 4c3b79c7248bd1e789bf33fc467abb27d0170edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-facebook-accounts"></a>Az Azure Active Directory B2C: Tooconsumers regisztráció és bejelentkezés Facebook-fiókkal rendelkező adja meg.
## <a name="create-a-facebook-application"></a>Facebook-alkalmazás létrehozása
toouse Facebook identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, kell toocreate Facebook-alkalmazás, és adja meg azt a hello megfelelő paraméterekkel. A Facebook-fiókkal toodo ez szükséges. Ha még nincs fiókja, beszerezheti a [https://www.facebook.com/](https://www.facebook.com/).

1. Nyissa meg toohello [Facebook-fejlesztőknek](https://developers.facebook.com/) webhelyet, és jelentkezzen be a Facebook-fiók hitelesítő adatait.
2. Ha még nem tette meg, Facebook-fejlesztőként kell tooregister. toodo, kattintson **regisztrálása** (a hello jobb felső sarkában hello oldalon), fogadja el a Facebook a házirendeket, és hello regisztráció végrehajtásához.
3. Kattintson a **saját alkalmazások** majd **fel egy új alkalmazást**. 
4. Hello formában adja meg a **megjelenített név** és egy érvényes **kapcsolattartó E-mail**.
5. Kattintson a **App ID Azonosítójának létrehozása**. Ez tooaccept Facebook platform házirendek megkövetelik, és az online biztonsági ellenőrzés.
6. Hello bal oldali oszlopban, kattintson a **beállítások** majd **alapvető** Ha nincs bejelölve.
7. Válassza ki a **kategória**. 
8. Kattintson a **+ Hozzáadás Platform** válassza **webhely**.
   
    ![Facebook - beállítások](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - beállítások - webhely](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Adja meg `https://login.microsoftonline.com/` a hello **webhely URL-címe** mezőben, majd kattintson **módosítások mentése** hello lap hello alján.
   
    ![Facebook - webhely URL-címe](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Másolja a hello értékének **Alkalmazásazonosító**. Kattintson a **megjelenítése** , és másolja a hello értékének **alkalmazás titkos kulcs**. Szüksége lesz mindkettő tooconfigure Facebook-bérlőben identitás-szolgáltatóként. **Alkalmazás titkos kulcs** van egy fontos biztonsági hitelesítő adatok.
   
    ![Facebook - Alkalmazásazonosító & alkalmazás titkos kulcs](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Kattintson a **+ Hozzáadás termék** a bal oldali navigációs hello és majd hello **Set Up** gombra kattint, a **Facebook bejelentkezési**.
   
    ![Facebook - Facebook-bejelentkezés](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Kattintson a **beállítások** hello jobb nav alatt a **Facebook-bejelentkezés**

    ![Facebook - Facebook bejelentkezési beállítások](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **érvényes OAuth-alapú átirányítási URI-azonosítók** hello mezőjét **OAuth ügyfélbeállítások** szakasz. Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com). Kattintson a **módosítások mentése** hello lap hello alján.
    
    ![Facebook - OAuth átirányítási URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. toomake a Facebook-alkalmazás az Azure AD B2C tárolóhálózatnak kell toomake nyilvánosan elérhetővé. Ehhez kattintson **App felülvizsgálati** a bal oldali navigációs hello és indításának hello kapcsoló hello oldal hello tetején túl**Igen** elemre kattintva **megerősítése**.
    
    ![Facebook - alkalmazást nyilvános](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurálja a Facebook-bérlőben identitás-szolgáltatóként
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "Facebook".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Facebook**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello app ID és az alkalmazás titkos kulcs (a korábban létrehozott Facebook-alkalmazás hello) a hello **ügyfél-azonosító** és **ügyfélkulcs**mezők kulcsattribútumokkal.
7. Kattintson a **OK**, és kattintson a **létrehozása** toosave a Facebook-konfigurációt.

> [!NOTE]
> Hozzáadása egy **identitásszolgáltató** tooyour bérlő nem módosítja a meglévő szabályzatokat. Ne felejtse el tooupdate a házirendek újonnan létrehozott hello identitásszolgáltató-ot.
>