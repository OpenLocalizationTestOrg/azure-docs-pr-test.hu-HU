---
title: "Az Azure Active Directory B2C: LinkedIn konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers LinkedIn fiókokhoz az Azure Active Directory B2C által védett alkalmazások"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6c5233ef48b24968fd6383f470b5d8a969a78ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-linkedin-accounts"></a>Az Azure Active Directory B2C: Adja meg a regisztráció és bejelentkezés tooconsumers LinkedIn-fiókok
## <a name="create-a-linkedin-application"></a>LinkedIn-alkalmazás létrehozása
toouse LinkedIn identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, kell toocreate LinkedIn-alkalmazás, és adja meg azt a hello megfelelő paraméterekkel. A LinkedIn fiók toodo ez szükséges. Ha még nincs fiókja, beszerezheti a [https://www.linkedin.com/](https://www.linkedin.com/).

1. Nyissa meg toohello [LinkedIn fejlesztők webhely](https://www.developer.linkedin.com/) és jelentkezzen be a LinkedIn-fiók hitelesítő adataival.
2. Kattintson a **saját alkalmazások** a felső menüsávban hello, és kattintson a **alkalmazás létrehozása**.
   
    ![LinkedIn - új alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. A hello **hozzon létre egy új alkalmazást** alkotnak, töltse ki hello vonatkozó információkat (**vállalatnév**, **neve**, **leírás**, **Embléma URL-címe**, **alkalmazás használata**, **webhely URL-címe**, **üzleti E-mail** és **Munkahelyitelefon**).
4. Elfogadom toohello **LinkedIn API használati** kattintson **Submit**.
   
    ![LinkedIn - alkalmazás regisztrálása](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Hello értékeit másolja **ügyfél-azonosító** és **Ügyfélkulcs**. (Megtalálja azokat a **hitelesítési kulcsokat**.) Szüksége lesz mindkettő tooconfigure LinkedIn-bérlőben identitás-szolgáltatóként.
   
   > [!NOTE]
   > **Titkos Ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.
   > 
   > 
6. Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **átirányítási URL-t engedélyezett** mező (alatt **OAuth 2.0**). Cserélje le **{tenant}** a bérlő nevű (például contoso.onmicrosoft.com). Kattintson a **Hozzáadás**, és kattintson a **frissítés**. Hello **{tenant}** érték kis-és nagybetűket.
   
    ![LinkedIn - telepítő alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>A bérlő az identitás-szolgáltatóként LinkedIn konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "LI".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **LinkedIn**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello azonosító és az ügyfél titkos ügyfélkulcs az hello korábban létrehozott LinkedIn-alkalmazás.
7. Kattintson a **OK** majd **létrehozása** toosave LinkedIn-konfigurációjáról.

