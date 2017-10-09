---
title: "Az Azure Active Directory B2C: Weibo konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Weibo fiókokhoz az Azure Active Directory B2C által védett alkalmazások."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: c0648620f318046c1d9d24feb92a0c5f19c6a91a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-weibo-accounts"></a>Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Weibo fiókok adja meg.

> [!NOTE]
> A funkció jelenleg előzetes verzió. Ne használja az identitásszolgáltató az éles környezetben.
> 

## <a name="create-a-weibo-application"></a>Weibo-alkalmazás létrehozása

toouse Weibo identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Weibo alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel. Egy Weibo fiók toodo ez szükséges. Ha még nincs fiókja, akkor kaphat egyenként [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-hello-weibo-developer-program"></a>Hello Weibo fejlesztőprogrambeli regisztrálása

1. Nyissa meg toohello [Weibo fejlesztői portálján](http://open.weibo.com/) és jelentkezzen be a Weibo fiók hitelesítő adataival.
2. Történő bejelentkezés után kattintson a jobb felső sarokban hello megjelenítendő neve.
3. Hello legördülő menüben válassza ki**编辑开发者信息**(fejlesztői adatainak szerkesztése).
4. Adja meg a szükséges hello adatokat hello formába, és kattintson a**提交**(küldje el).
5. Hello e-mail ellenőrzési folyamat befejezéséhez.
6. Nyissa meg toohello [identitás ellenőrzése lapon](http://open.weibo.com/developers/identity/edit).
7. Adja meg a szükséges hello adatokat hello formába, és kattintson a**提交**(küldje el).

### <a name="register-a-weibo-application"></a>Egy Weibo alkalmazás regisztrálása

1. Nyissa meg toohello [új Weibo regisztrációs oldalra](http://open.weibo.com/apps/new).
2. Hello szükséges alkalmazással kapcsolatos adatok megadása
3. Kattintson a**创建**(létrehozása).
4. Hello értékeit másolja **Alkalmazáskulcs** és **alkalmazás titkos kulcs**. Ezt később lesz szüksége.
5. Töltse fel a szükséges hello fényképek, és írja be a hello szükséges információkat.
6. Kattintson a**保存以上信息**(Mentés).
7. Kattintson a**高级信息**(speciális információk).
8. Kattintson a**编辑**(Szerkesztés) következő toohello mező OAuth2.0**授权设置**(átirányítási URL-cím).
9. Adja meg `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` a OAuth2.0**授权设置**(átirányítási URL-cím). Például ha a `tenant_name` contoso.onmicrosoft.com, set hello URL-cím toobe van `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Kattintson a**提交**(küldje el).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>A bérlő az identitás-szolgáltatóként Weibo konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "Weibo".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Weibo**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása**
7. Adja meg a hello **Alkalmazáskulcs** korábban kimásolt hello mint **ügyfél-azonosító**.
8. Adja meg a hello **alkalmazás titkos kulcs** korábban kimásolt hello mint **Ügyfélkulcs**.
9. Kattintson a **OK** majd **létrehozása** toosave Weibo konfigurációjáról.

