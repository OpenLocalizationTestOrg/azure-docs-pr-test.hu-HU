---
title: "Az Azure Active Directory B2C: Twitter konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Twitter-fiókok az Azure Active Directory B2C által védett alkalmazások."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 275c5c73fd5e8e5075e77fee942cbc1b5e1586cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-twitter-accounts"></a>Az Azure Active Directory B2C: Adja meg a regisztráció és bejelentkezés tooconsumers, a Twitter-fiókokkal

> [!NOTE]
> A funkció jelenleg előzetes verzió.
> 

## <a name="create-a-twitter-application"></a>Twitter-alkalmazás létrehozása
az Azure Active Directory (Azure AD) B2C-ben identitás-szolgáltatóként toouse Twitter, toocreate egy Twitter-alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel. Egy Twitter fejlesztői fiók toodo ez szükséges. Ha még nincs fiókja, beszerezheti a [https://dev.twitter.com/](https://dev.twitter.com/).

1. Nyissa meg toohello [fejlesztői webhely Twitter](https://dev.twitter.com/) és jelentkezzen be a hitelesítő adatait.
2. Kattintson a **alkalmazásaimat** alatt **eszközök & támogatási** , majd **új alkalmazás létrehozása**. 
3. Hello képernyőn adjon meg egy értéket hello **neve**, **leírás**, és **webhely**.
4. A hello **visszahívási URL-cím**, adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Győződjön meg arról, hogy tooreplace **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).
5. Ellenőrizze hello mezőben tooagree toohello **fejlesztői megállapodás** kattintson **az Twitter-alkalmazás létrehozása**.
6. Hello alkalmazás létrehozása után kattintson **kulcsok és a hozzáférési jogkivonatok**.
7. Másolja a hello értékének **kulcsa** és **felhasználói titok**. Szüksége lesz mindkettő tooconfigure Twitter-bérlőben identitás-szolgáltatóként.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Twitter-bérlőben identitás-szolgáltatóként konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Adja meg például "Twitter".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Twitter**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello Twitter **kulcsa** a hello **ügyfél-azonosító** és a hello Twitter **felhasználói titok**a hello **ügyfélkulcs**.
7. Kattintson a **OK**, és kattintson a **létrehozása** toosave a Twitter-konfigurációt.

