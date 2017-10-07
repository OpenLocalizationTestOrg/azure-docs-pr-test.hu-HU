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
# <a name="how-tooauthorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a><span data-ttu-id="b6fa4-103">Hogyan tooauthorize fejlesztői fiókok Azure Active Directory B2C segítségével az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="b6fa4-103">How tooauthorize developer accounts by using Azure Active Directory B2C in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="b6fa4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b6fa4-104">Overview</span></span>
<span data-ttu-id="b6fa4-105">Az Azure Active Directory B2C egy felhőbeli identitáskezelő megoldás a felhasználók felé néző webes és mobilalkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-105">Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.</span></span> <span data-ttu-id="b6fa4-106">Használhatja toomanage hozzáférés tooyour fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-106">You can use it toomanage access tooyour developer portal.</span></span> <span data-ttu-id="b6fa4-107">Ez az útmutató jeleníti meg, Ön által előírt konfiguráció szerint az Azure Active Directory B2C az API Management szolgáltatás toointegrate hello.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-107">This guide shows you hello configuration that's required in your API Management service toointegrate with Azure Active Directory B2C.</span></span> <span data-ttu-id="b6fa4-108">A klasszikus Azure Active Directory hozzáférési toohello fejlesztői portálján engedélyezésével kapcsolatos információkért lásd: [hogyan tooauthorize fejlesztői fiókok Azure Active Directory].</span><span class="sxs-lookup"><span data-stu-id="b6fa4-108">For information about enabling access toohello developer portal by using classic Azure Active Directory, see [How tooauthorize developer accounts using Azure Active Directory].</span></span>

> [!NOTE]
> <span data-ttu-id="b6fa4-109">toocomplete hello jelen útmutató lépéseit, először rendelkeznie kell egy Azure Active Directory B2C-bérlő toocreate egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-109">toocomplete hello steps in this guide, you must first have an Azure Active Directory B2C tenant toocreate an application in.</span></span> <span data-ttu-id="b6fa4-110">Is kell toohave regisztráció és bejelentkezés házirendek készen áll.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-110">Also, you need toohave signup and signin policies ready.</span></span> <span data-ttu-id="b6fa4-111">További információkért lásd: [Azure Active Directory B2C áttekintése].</span><span class="sxs-lookup"><span data-stu-id="b6fa4-111">For more information, see [Azure Active Directory B2C overview].</span></span>

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a><span data-ttu-id="b6fa4-112">Azure Active Directory B2C segítségével hitelesíthetők a fejlesztői fiókok</span><span class="sxs-lookup"><span data-stu-id="b6fa4-112">Authorize developer accounts by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="b6fa4-113">tooget elindítani, kattintson a **Publisher portal** a hello Azure-portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-113">tooget started, click **Publisher portal** in hello Azure portal for your API Management service.</span></span> <span data-ttu-id="b6fa4-114">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-114">This takes you toohello API Management publisher portal.</span></span>

   ![Közzétevő portál][api-management-management-console]

   > [!NOTE]
   > <span data-ttu-id="b6fa4-116">Ha még nem hozott az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management oktatóanyag][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="b6fa4-116">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management tutorial][Get started with Azure API Management].</span></span>

2. <span data-ttu-id="b6fa4-117">A hello **API Management** menüben kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-117">On hello **API Management** menu, click **Security**.</span></span> <span data-ttu-id="b6fa4-118">A hello **identitások** lapra, majd **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-118">On hello **Identities** tab, choose **Azure Active Directory B2C**.</span></span>

  ![Külső identitások 1][api-management-howto-aad-b2c-security-tab]

3. <span data-ttu-id="b6fa4-120">Jegyezze fel a hello **átirányítási URL-cím** és Active Directory B2C tooAzure a hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-120">Make a note of hello **Redirect URL** and switch over tooAzure Active Directory B2C in hello Azure portal.</span></span>

  ![Külső identitások 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. <span data-ttu-id="b6fa4-122">Kattintson a hello **alkalmazások** gombra.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-122">Click hello **Applications** button.</span></span>

  ![Egy új alkalmazás 1 regisztrálása][api-management-howto-aad-b2c-portal-menu]

5. <span data-ttu-id="b6fa4-124">Kattintson a hello **Hozzáadás** toocreate egy új Azure Active Directory B2C alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-124">Click hello **Add** button toocreate a new Azure Active Directory B2C application.</span></span>

  ![Egy új alkalmazás 2 regisztrálása][api-management-howto-aad-b2c-add-button]

6. <span data-ttu-id="b6fa4-126">A hello **új alkalmazás** panelen adjon meg egy nevet hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-126">In hello **New application** blade, enter a name for hello application.</span></span> <span data-ttu-id="b6fa4-127">Válasszon **Igen** alatt **Web App/Web API**, és válassza a **Igen** alatt **implicit engedélyezési folyamat engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-127">Choose **Yes** under **Web App/Web API**, and choose **Yes** under **Allow implicit flow**.</span></span> <span data-ttu-id="b6fa4-128">Ezután másolási hello **átirányítási URL-cím** a hello **Azure Active Directory B2C** hello szakasza **identitások** hello publisher portálon fülre, és illessze be hello **Válasz URL-CÍMEN** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-128">Then copy hello **Redirect URL** from hello **Azure Active Directory B2C** section of hello **Identities** tab in hello publisher portal, and paste it into hello **Reply URL** text box.</span></span>

  ![Egy új alkalmazás 3 regisztrálása][api-management-howto-aad-b2c-app-details]

7. <span data-ttu-id="b6fa4-130">Kattintson a hello **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-130">Click hello **Create** button.</span></span> <span data-ttu-id="b6fa4-131">Hello alkalmazás létrehozásakor hello megjelenik **alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-131">When hello application is created, it appears in hello **Applications** blade.</span></span> <span data-ttu-id="b6fa4-132">Hello alkalmazás neve toosee a Részletek gombra.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-132">Click hello application name toosee its details.</span></span>

  ![Egy új alkalmazás 4 regisztrálása][api-management-howto-aad-b2c-app-created]

8. <span data-ttu-id="b6fa4-134">A hello **tulajdonságok** panelen, a Másolás hello **Alkalmazásazonosító** toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-134">From hello **Properties** blade, copy hello **Application ID** toohello clipboard.</span></span>

  ![1 alkalmazás azonosítója][api-management-howto-aad-b2c-app-id]

9. <span data-ttu-id="b6fa4-136">Váltson vissza toohello publisher portálon, és illessze be hello azonosító hello **ügyfél-azonosító** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-136">Switch back toohello publisher portal and paste hello ID into hello **Client Id** text box.</span></span>

  ![Kérelem azonosítója 2][api-management-howto-aad-b2c-client-id]

10. <span data-ttu-id="b6fa4-138">Váltson vissza toohello Azure-portálon, kattintson a hello **kulcsok** gombra, majd **kulcs**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-138">Switch back toohello Azure portal, click hello **Keys** button, and then click **Generate key**.</span></span> <span data-ttu-id="b6fa4-139">Kattintson a **mentése** toosave hello konfigurációs és megjelenítési hello **Alkalmazáskulcs**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-139">Click **Save** toosave hello configuration and display hello **App key**.</span></span> <span data-ttu-id="b6fa4-140">Hello kulcs toohello vágólapra másolása.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-140">Copy hello key toohello clipboard.</span></span>

  ![1 alkalmazás kulcs][api-management-howto-aad-b2c-app-key]

11. <span data-ttu-id="b6fa4-142">Kapcsoló hátsó toohello publisher portál és a Beillesztés hello kulcs be hello **Ügyfélkulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-142">Switch back toohello publisher portal and paste hello key into hello **Client Secret** text box.</span></span>

  ![Alkalmazáskulcs 2][api-management-howto-aad-b2c-client-secret]

12. <span data-ttu-id="b6fa4-144">Adja meg a hello Azure Active Directory B2C-bérlő tartománynevét hello **engedélyezett bérlői**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-144">Specify hello domain name of hello Azure Active Directory B2C tenant in **Allowed Tenant**.</span></span>

  ![Engedélyezett bérlői][api-management-howto-aad-b2c-allowed-tenant]

13. <span data-ttu-id="b6fa4-146">Adja meg a hello **előfizetési házirend** és **Signin házirend**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-146">Specify hello **Signup Policy** and **Signin Policy**.</span></span> <span data-ttu-id="b6fa4-147">Szükség esetén is megadhatja hello **profil szerkesztése házirend** és **jelszó-visszaállítási házirend**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-147">Optionally, you can also provide hello **Profile Editing Policy** and **Password Reset Policy**.</span></span>

  ![Házirendek][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > <span data-ttu-id="b6fa4-149">A házirendekkel kapcsolatos további információkért lásd: [Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek].</span><span class="sxs-lookup"><span data-stu-id="b6fa4-149">For more information on policies, see [Azure Active Directory B2C: Extensible policy framework].</span></span>

14. <span data-ttu-id="b6fa4-150">A megadott hello szükségeskonfiguráció, kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-150">After you've specified hello desired configuration, click **Save**.</span></span>

  <span data-ttu-id="b6fa4-151">Hello módosítások mentése történik meg, miután a fejlesztők képes toocreate lehet új fiókokat, és toohello developer portálon az Azure Active Directory B2C használatával írja alá.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-151">After hello changes are saved, developers will be able toocreate new accounts and sign in toohello developer portal by using Azure Active Directory B2C.</span></span>

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a><span data-ttu-id="b6fa4-152">Azure Active Directory B2C segítségével egy developer-fiók regisztrálása</span><span class="sxs-lookup"><span data-stu-id="b6fa4-152">Sign up for a developer account by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="b6fa4-153">egy Azure Active Directory B2C segítségével fejlesztői fiókot toosign nyisson meg egy új böngészőablakot, és folytassa a toohello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-153">toosign up for a developer account by using Azure Active Directory B2C, open a new browser window and go toohello developer portal.</span></span> <span data-ttu-id="b6fa4-154">Kattintson a hello **regisztráljon** gombra.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-154">Click hello **Sign up** button.</span></span>

   ![Fejlesztői portálján 1][api-management-howto-aad-b2c-dev-portal]

2. <span data-ttu-id="b6fa4-156">Válassza ki a toosign a **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-156">Choose toosign up with **Azure Active Directory B2C**.</span></span>

   ![Fejlesztői portálján 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. <span data-ttu-id="b6fa4-158">Ön átirányított toohello előfizetési házirend hello előző részben konfigurált.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-158">You're redirected toohello signup policy that you configured in hello previous section.</span></span> <span data-ttu-id="b6fa4-159">Válassza ki a toosign az e-mail címet vagy egy meglévő közösségi fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-159">Choose toosign up by using your email address or one of your existing social accounts.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b6fa4-160">Ha az Azure Active Directory B2C hello csak beállítás engedélyezve van a hello **identitások** lapon hello publisher portálon lesz átirányított toohello előfizetési házirend közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-160">If Azure Active Directory B2C is hello only option that's enabled on hello **Identities** tab in hello publisher portal, you'll be redirected toohello signup policy directly.</span></span>

   ![Fejlesztői portál][api-management-howto-aad-b2c-dev-portal-b2c-options]

   <span data-ttu-id="b6fa4-162">Hello előfizetési befejezését, akkor is átirányított hátsó toohello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-162">When hello signup is complete, you're redirected back toohello developer portal.</span></span> <span data-ttu-id="b6fa4-163">Most már bejelentkezett ezen a toohello fejlesztői portálján az API Management szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="b6fa4-163">You're now signed in toohello developer portal for your API Management service instance.</span></span>

    ![Regisztráció kész][api-management-registration-complete]

## <a name="next-steps"></a><span data-ttu-id="b6fa4-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6fa4-165">Next steps</span></span>

*  <span data-ttu-id="b6fa4-166">[Azure Active Directory B2C áttekintése]</span><span class="sxs-lookup"><span data-stu-id="b6fa4-166">[Azure Active Directory B2C overview]</span></span>
*  <span data-ttu-id="b6fa4-167">[Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek]</span><span class="sxs-lookup"><span data-stu-id="b6fa4-167">[Azure Active Directory B2C: Extensible policy framework]</span></span>
*  <span data-ttu-id="b6fa4-168">[Az Azure Active Directory B2C identitás-szolgáltatóként a Microsoft-fiók használata]</span><span class="sxs-lookup"><span data-stu-id="b6fa4-168">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="b6fa4-169">[A Google-fiók használata az Azure Active Directory B2C identitás-szolgáltatóként]</span><span class="sxs-lookup"><span data-stu-id="b6fa4-169">[Use a Google account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="b6fa4-170">[Az Azure Active Directory B2C identitás-szolgáltatóként LinkedIn fiók használata]</span><span class="sxs-lookup"><span data-stu-id="b6fa4-170">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="b6fa4-171">[Az Azure Active Directory B2C identitás-szolgáltatóként Facebook fiók használata]</span><span class="sxs-lookup"><span data-stu-id="b6fa4-171">[Use a Facebook account as an identity provider in Azure Active Directory B2C]</span></span>




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
