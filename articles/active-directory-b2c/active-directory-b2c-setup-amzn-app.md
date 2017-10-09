---
title: "Az Azure Active Directory B2C: Amazon konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers az Azure Active Directory B2C által védett alkalmazások Amazon-fiókkal rendelkező."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 60d7c4b76d9d3e86ed535765329abed07f1e5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-amazon-accounts"></a>Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Amazon-fiókkal rendelkező adja meg.
## <a name="create-an-amazon-application"></a>Az Amazon-alkalmazás létrehozása
toouse Amazon identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate Amazon kérelmet kell, és adja meg azt a hello megfelelő paraméterekkel. Az Amazon-fiókkal toodo ez szükséges. Ha még nincs fiókja, beszerezheti a [http://www.amazon.com/](http://www.amazon.com/).

1. Nyissa meg toohello [Amazon fejlesztői központ](https://login.amazon.com/) és jelentkezzen be az Amazon fiók hitelesítő adatait.
2. Ha még nem tette meg, kattintson a **regisztráció**, hello fejlesztői regisztrációs lépésekkel, és fogadja el a hello házirend.
3. Kattintson a **új alkalmazás regisztrálása**.
   
    ![Egy új alkalmazás hello Amazon webhelyen regisztrálása](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Adja meg az alkalmazással kapcsolatos információk (**neve**, **leírás**, és **adatvédelmi nyilatkozat URL-címe**), és kattintson a **mentése**.
   
    ![Amazon egy új alkalmazás regisztrálásához alkalmazással kapcsolatos adatok megadása](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. A hello **Webbeállításainak** részben, a Másolás hello értékének **ügyfél-azonosító** és **Ügyfélkulcs**. (Tooclick hello kell **megjelenítése titkos** toosee ez gombra.) Mindkettő kell tooconfigure Amazon-bérlőben identitás-szolgáltatóként. Kattintson a **szerkesztése** hello szakasz hello alján. **Titkos Ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.
   
    ![Olyan ügyfél-Azonosítót és titkos Ügyfélkulcs az Amazon: új alkalmazás](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Adja meg `https://login.microsoftonline.com` a hello **engedélyezett JavaScript források** mező és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **visszatérési URL-címek engedélyezett** mező. Cserélje le **{tenant}** a bérlő nevű (például contoso.onmicrosoft.com). Kattintson a **Save** (Mentés) gombra. Hello **{tenant}** érték kis-és nagybetűket.
   
    ![JavaScript források és visszatérési URL-címek megadása az új alkalmazást az Amazon:](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>A bérlő az identitás-szolgáltatóként Amazon konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "Amzn".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Amazon**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello azonosító és az ügyfél titkos ügyfélkulcs az Amazon alkalmazás korábban létrehozott hello.
7. Kattintson a **OK** majd **létrehozása** toosave az Amazon konfigurációját.

