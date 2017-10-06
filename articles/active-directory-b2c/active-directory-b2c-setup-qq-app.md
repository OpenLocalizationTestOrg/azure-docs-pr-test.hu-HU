---
title: "Az Azure Active Directory B2C: Gyorsműveletek konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Gyorsműveletek fiókokhoz az Azure Active Directory B2C által védett alkalmazások."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 896d6221e01d15de1652a5717cf1f65619101e0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-qq-accounts"></a>Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Gyorsműveletek fiókok adja meg.

> [!NOTE]
> A funkció jelenleg előzetes verzió.
> 

## <a name="create-a-qq-application"></a>Gyorsműveletek-alkalmazás létrehozása

Gyorsműveletek toouse identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Gyorsműveletek alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel. Egy Gyorsműveletek fiók toodo ez szükséges. Ha még nincs fiókja, akkor kaphat egyenként [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-hello-qq-developer-program"></a>Gyorsműveletek fejlesztőprogrambeli hello regisztrálása

1. Nyissa meg toohello [Gyorsműveletek fejlesztői portálján](http://open.qq.com) és jelentkezzen be a Gyorsműveletek fiók hitelesítő adataival.
2. Történő bejelentkezés után nyissa meg túl[http://open.qq.com/reg](http://open.qq.com/reg) tooregister fejlesztőként saját maga.
3. Hello menüben válasszon ki**个人**(az egyéni fejlesztői).
4. Adja meg a szükséges hello adatokat hello formába, és kattintson a**下一步**(Tovább).
5. Hello e-mail ellenőrzési folyamat befejezéséhez.

> [!NOTE]
> Szüksége lesz toowait néhány napon toobe jóváhagyott fejlesztőként a regisztrálás után. 

### <a name="register-a-qq-application"></a>Egy Gyorsműveletek alkalmazás regisztrálása

1. Nyissa meg túl[https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Kattintson a**应用管理**(Alkalmazáskezelés).
3. Kattintson a**创建应用**(az alkalmazás létrehozása).
4. Adja meg a hello szükséges alkalmazásadatokat.
5. Kattintson a**创建应用**(az alkalmazás létrehozása).
6. Adja meg a hello szükséges adatokat.
7. A hello**授权回调域**(visszahívási URL-címe) mezőbe írja be `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Például ha a `tenant_name` contoso.onmicrosoft.com, set hello URL-cím toobe van `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Kattintson a**创建应用**(az alkalmazás létrehozása).
9. A hello megerősítési oldalán kattintson a**应用管理**(Alkalmazáskezelés) tooreturn toohello alkalmazás kezelése lapján.
10. Kattintson a**查看**(Nézet) imént létrehozott következő toohello alkalmazást.
11. Kattintson a**修改**(Szerkesztés).
12. Hello hello oldal tetejére, másolja a hello **Alkalmazásazonosító** és **ALKALMAZÁSKULCS**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Gyorsműveletek az Ön bérlőjében identitás-szolgáltatóként konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "Gyorsműveletek".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Gyorsműveletek**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása**
7. Adja meg a hello **Alkalmazáskulcs** korábban kimásolt hello mint **ügyfél-azonosító**.
8. Adja meg a hello **alkalmazás titkos kulcs** korábban kimásolt hello mint **Ügyfélkulcs**.
9. Kattintson a **OK** majd **létrehozása** toosave Gyorsműveletek konfigurációjáról.

