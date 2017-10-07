---
title: "aaaAuthorize fejlesztői fiókok Azure Active Directory B2C - Azure API Management használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan tooauthorize felhasználók Azure Active Directory B2C segítségével az API Management."
services: api-management
documentationcenter: API Management
author: miaojiang
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 28f7cae53138938dbbc848b4afcbf08b72690e37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a>Hogyan tooauthorize fejlesztői fiókok Azure Active Directory B2C segítségével az Azure API Management
## <a name="overview"></a>Áttekintés
Az Azure Active Directory B2C egy felhőbeli identitáskezelő megoldás a felhasználók felé néző webes és mobilalkalmazásokhoz. Használhatja toomanage hozzáférés tooyour fejlesztői portálján. Ez az útmutató jeleníti meg, Ön által előírt konfiguráció szerint az Azure Active Directory B2C az API Management szolgáltatás toointegrate hello. A klasszikus Azure Active Directory hozzáférési toohello fejlesztői portálján engedélyezésével kapcsolatos információkért lásd: [hogyan tooauthorize fejlesztői fiókok Azure Active Directory].

> [!NOTE]
> toocomplete hello jelen útmutató lépéseit, először rendelkeznie kell egy Azure Active Directory B2C-bérlő toocreate egy alkalmazást. Is kell toohave regisztráció és bejelentkezés házirendek készen áll. További információkért lásd: [Azure Active Directory B2C áttekintése].

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C segítségével hitelesíthetők a fejlesztői fiókok

1. tooget elindítani, kattintson a **Publisher portal** a hello Azure-portálon az API Management szolgáltatás. Ezzel megnyitná toohello API Management publisher portálon.

   ![Közzétevő portál][api-management-management-console]

   > [!NOTE]
   > Ha még nem hozott az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management oktatóanyag][Get started with Azure API Management].

2. A hello **API Management** menüben kattintson a **biztonsági**. A hello **identitások** lapra, majd **Azure Active Directory B2C**.

  ![Külső identitások 1][api-management-howto-aad-b2c-security-tab]

3. Jegyezze fel a hello **átirányítási URL-cím** és Active Directory B2C tooAzure a hello Azure-portálon keresztül.

  ![Külső identitások 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. Kattintson a hello **alkalmazások** gombra.

  ![Egy új alkalmazás 1 regisztrálása][api-management-howto-aad-b2c-portal-menu]

5. Kattintson a hello **Hozzáadás** toocreate egy új Azure Active Directory B2C alkalmazás gombra.

  ![Egy új alkalmazás 2 regisztrálása][api-management-howto-aad-b2c-add-button]

6. A hello **új alkalmazás** panelen adjon meg egy nevet hello alkalmazás. Válasszon **Igen** alatt **Web App/Web API**, és válassza a **Igen** alatt **implicit engedélyezési folyamat engedélyezése**. Ezután másolási hello **átirányítási URL-cím** a hello **Azure Active Directory B2C** hello szakasza **identitások** hello publisher portálon fülre, és illessze be hello **Válasz URL-CÍMEN** szövegmezőben.

  ![Egy új alkalmazás 3 regisztrálása][api-management-howto-aad-b2c-app-details]

7. Kattintson a hello **létrehozása** gombra. Hello alkalmazás létrehozásakor hello megjelenik **alkalmazások** panelen. Hello alkalmazás neve toosee a Részletek gombra.

  ![Egy új alkalmazás 4 regisztrálása][api-management-howto-aad-b2c-app-created]

8. A hello **tulajdonságok** panelen, a Másolás hello **Alkalmazásazonosító** toohello vágólapra.

  ![1 alkalmazás azonosítója][api-management-howto-aad-b2c-app-id]

9. Váltson vissza toohello publisher portálon, és illessze be hello azonosító hello **ügyfél-azonosító** szövegmezőben.

  ![Kérelem azonosítója 2][api-management-howto-aad-b2c-client-id]

10. Váltson vissza toohello Azure-portálon, kattintson a hello **kulcsok** gombra, majd **kulcs**. Kattintson a **mentése** toosave hello konfigurációs és megjelenítési hello **Alkalmazáskulcs**. Hello kulcs toohello vágólapra másolása.

  ![1 alkalmazás kulcs][api-management-howto-aad-b2c-app-key]

11. Kapcsoló hátsó toohello publisher portál és a Beillesztés hello kulcs be hello **Ügyfélkulcs** szövegmezőben.

  ![Alkalmazáskulcs 2][api-management-howto-aad-b2c-client-secret]

12. Adja meg a hello Azure Active Directory B2C-bérlő tartománynevét hello **engedélyezett bérlői**.

  ![Engedélyezett bérlői][api-management-howto-aad-b2c-allowed-tenant]

13. Adja meg a hello **előfizetési házirend** és **Signin házirend**. Szükség esetén is megadhatja hello **profil szerkesztése házirend** és **jelszó-visszaállítási házirend**.

  ![Házirendek][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > A házirendekkel kapcsolatos további információkért lásd: [Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek].

14. A megadott hello szükségeskonfiguráció, kattintson **mentése**.

  Hello módosítások mentése történik meg, miután a fejlesztők képes toocreate lehet új fiókokat, és toohello developer portálon az Azure Active Directory B2C használatával írja alá.

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a>Azure Active Directory B2C segítségével egy developer-fiók regisztrálása

1. egy Azure Active Directory B2C segítségével fejlesztői fiókot toosign nyisson meg egy új böngészőablakot, és folytassa a toohello fejlesztői portálján. Kattintson a hello **regisztráljon** gombra.

   ![Fejlesztői portálján 1][api-management-howto-aad-b2c-dev-portal]

2. Válassza ki a toosign a **Azure Active Directory B2C**.

   ![Fejlesztői portálján 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. Ön átirányított toohello előfizetési házirend hello előző részben konfigurált. Válassza ki a toosign az e-mail címet vagy egy meglévő közösségi fiók használatával.

   > [!NOTE]
   > Ha az Azure Active Directory B2C hello csak beállítás engedélyezve van a hello **identitások** lapon hello publisher portálon lesz átirányított toohello előfizetési házirend közvetlenül.

   ![Fejlesztői portál][api-management-howto-aad-b2c-dev-portal-b2c-options]

   Hello előfizetési befejezését, akkor is átirányított hátsó toohello fejlesztői portálján. Most már bejelentkezett ezen a toohello fejlesztői portálján az API Management szolgáltatáspéldány.

    ![Regisztráció kész][api-management-registration-complete]

## <a name="next-steps"></a>Következő lépések

*  [Azure Active Directory B2C áttekintése]
*  [Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek]
*  [Az Azure Active Directory B2C identitás-szolgáltatóként a Microsoft-fiók használata]
*  [A Google-fiók használata az Azure Active Directory B2C identitás-szolgáltatóként]
*  [Az Azure Active Directory B2C identitás-szolgáltatóként LinkedIn fiók használata]
*  [Az Azure Active Directory B2C identitás-szolgáltatóként Facebook fiók használata]




[api-management-howto-aad-b2c-security-tab]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab.PNG
[api-management-howto-aad-b2c-security-tab-reply-url]: ./media/api-management-howto-aad-b2c/api-management-b2c-security-tab-reply-url.PNG
[api-management-howto-aad-b2c-portal-menu]: ./media/api-management-howto-aad-b2c/api-management-b2c-portal-menu.PNG
[api-management-howto-aad-b2c-add-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-add-button.PNG
[api-management-howto-aad-b2c-app-details]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-details.PNG
[api-management-howto-aad-b2c-app-created]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-created.PNG
[api-management-howto-aad-b2c-app-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-id.PNG
[api-management-howto-aad-b2c-client-id]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-id.PNG
[api-management-howto-aad-b2c-app-key]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key.PNG
[api-management-howto-aad-b2c-app-key-saved]: ./media/api-management-howto-aad-b2c/api-management-b2c-app-key-saved.PNG
[api-management-howto-aad-b2c-client-secret]: ./media/api-management-howto-aad-b2c/api-management-b2c-client-secret.PNG
[api-management-howto-aad-b2c-allowed-tenant]: ./media/api-management-howto-aad-b2c/api-management-b2c-allowed-tenant.PNG
[api-management-howto-aad-b2c-policies]: ./media/api-management-howto-aad-b2c/api-management-b2c-policies.PNG
[api-management-howto-aad-b2c-dev-portal]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-button]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-button.PNG
[api-management-howto-aad-b2c-dev-portal-b2c-options]: ./media/api-management-howto-aad-b2c/api-management-b2c-dev-portal-b2c-options.PNG
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.PNG
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-b2c-security-tab.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
[Azure Active Directory B2C áttekintése]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview
[hogyan tooauthorize fejlesztői fiókok Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad
[Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies
[Az Azure Active Directory B2C identitás-szolgáltatóként a Microsoft-fiók használata]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app
[A Google-fiók használata az Azure Active Directory B2C identitás-szolgáltatóként]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app
[Az Azure Active Directory B2C identitás-szolgáltatóként Facebook fiók használata]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app
[Az Azure Active Directory B2C identitás-szolgáltatóként LinkedIn fiók használata]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
