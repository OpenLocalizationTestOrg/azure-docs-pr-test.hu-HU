---
title: "Az Azure Active Directory B2C: Microsoft-fiók konfigurációja |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers az Azure Active Directory B2C által védett alkalmazások a Microsoft-fiókkal rendelkező."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: bec4777f003c459030f68c35b24f0e4bcddf84ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-microsoft-accounts"></a>Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Microsoft-fiókkal rendelkező adja meg.
## <a name="create-a-microsoft-account-application"></a>Microsoft-fiók alkalmazás létrehozása
toouse Microsoft fiók az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként, kell toocreate egy Microsoft-fiók alkalmazást, és adja meg azt a hello megfelelő paraméterekkel. A Microsoft-fiók toodo ez szükséges. Ha még nincs fiókja, beszerezheti a [https://www.live.com/](https://www.live.com/).

1. Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be Microsoft-fiók hitelesítő adataival.
2. Kattintson a **hozzáadhat egy alkalmazást**.
   
    ![Microsoft fiók – egy új alkalmazás felvétele](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Adjon meg egy **neve** az alkalmazás, és kattintson **alkalmazás létrehozása**.
   
    ![Microsoft-fiók - alkalmazás neve](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Másolja a hello értékének **alkalmazásazonosító**. Szüksége lesz rájuk tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának.
   
    ![Microsoft-fiók – alkalmazásazonosító](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Kattintson a **Hozzáadás platform** válassza **webes**.
   
    ![Microsoft fiók - platform hozzáadása](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-fiók - webalkalmazás](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **átirányítási URI-azonosítók** mező. Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).
   
    ![Microsoft-fiók - átirányítási URL-cím](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Kattintson a **új jelszó létrehozása** alatt hello **alkalmazás titkos kulcsok** szakasz. Másolja a hello új jelszót a képernyőn jelenik meg. Szüksége lesz rájuk tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának. Ez a jelszó nem egy fontos biztonsági hitelesítő adatok.
   
    ![Microsoft fiók – új jelszó létrehozása](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-fiók – új jelszó](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Hello jelölőnégyzetet, amely szerint **Live SDK támogatási** alatt hello **speciális beállítások** szakasz. Kattintson a **Save** (Mentés) gombra.
   
    ![Microsoft-fiók - Live SDK-támogatás](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>A bérlő az identitás-szolgáltatóként Microsoft-fiók konfigurálása
1. Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.
2. A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.
3. Kattintson a **+ Hozzáadás** hello panel hello tetején.
4. Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz. Írja be például a "MSA".
5. Kattintson a **identitás szolgáltatótípus**, jelölje be **Microsoft-fiók**, és kattintson a **OK**.
6. Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello alkalmazás azonosítóját és jelszavát, amely hello korábban létrehozott Microsoft-fiók alkalmazás.
7. Kattintson a **OK** majd **létrehozása** toosave a Microsoft-fiók konfigurációja.

