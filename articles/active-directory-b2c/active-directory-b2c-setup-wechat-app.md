---
title: "Az Azure Active Directory B2C: WeChat konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers WeChat fiókokhoz az Azure Active Directory B2C által védett alkalmazások."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 92cc3579d818d2379a503ccc695138b33a34466d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-wechat-accounts"></a>Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers WeChat fiókok adja meg.

> [!NOTE]
> A funkció jelenleg előzetes verzió.
> 

## <a name="create-a-wechat-application"></a>WeChat-alkalmazás létrehozása

toouse WeChat identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy WeChat alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel. Egy WeChat fiók toodo ez szükséges. Ha még nincs fiókja, egyet a mobilalkalmazások egyikével regisztrál vagy a Gyorsműveletek szám használatával kaphat. Ezt követően a hello WeChat fejlesztőprogrambeli regisztrált fiók lekérése. További információt talál [Itt](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Egy WeChat alkalmazás regisztrálása

1. Nyissa meg túl[https://open.weixin.qq.com/](https://open.weixin.qq.com/) , és jelentkezzen be.
2. Kattintson a**管理中心**(felügyeleti központ).
3. Hajtsa végre a hello szükséges lépéseket tooregister egy új alkalmazást.
4. A**授权回调域**(visszahívási URL-cím), adja meg `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Például ha a `tenant_name` contoso.onmicrosoft.com, set hello URL-cím toobe van `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Keresés és a példány hello **Alkalmazásazonosító** és **ALKALMAZÁSKULCS**. Ezekre később kell.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>A bérlő az identitás-szolgáltatóként WeChat konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "WeChat".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **WeChat**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása**
7. Adja meg a hello **Alkalmazáskulcs** korábban kimásolt hello mint **ügyfél-azonosító**.
8. Adja meg a hello **alkalmazás titkos kulcs** korábban kimásolt hello mint **Ügyfélkulcs**.
9. Kattintson a **OK** majd **létrehozása** toosave WeChat konfigurációjáról.

