---
title: "Fejlesztői fiókok hitelesítéséhez az Azure Active Directory B2C - Azure API Management használatával |} Microsoft Docs"
description: "Tudnivalók a felhasználók hitelesítése az Azure Active Directory B2C segítségével az API Management."
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
ms.openlocfilehash: eb7deb1a79d9db9ac5cfbea69b8d3c564eb55577
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-authorize-developer-accounts-by-using-azure-active-directory-b2c-in-azure-api-management"></a><span data-ttu-id="50507-103">Hogyan szeretné engedélyekkel felruházni fejlesztői fiókok Azure Active Directory B2C segítségével az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="50507-103">How to authorize developer accounts by using Azure Active Directory B2C in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="50507-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="50507-104">Overview</span></span>
<span data-ttu-id="50507-105">Az Azure Active Directory B2C egy felhőbeli identitáskezelő megoldás a felhasználók felé néző webes és mobilalkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="50507-105">Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications.</span></span> <span data-ttu-id="50507-106">A fejlesztői portálján hozzáférés kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="50507-106">You can use it to manage access to your developer portal.</span></span> <span data-ttu-id="50507-107">Ez az útmutató bemutatja, a által előírt konfiguráció szerint az Azure Active Directory B2C-vel integrálni az API Management szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="50507-107">This guide shows you the configuration that's required in your API Management service to integrate with Azure Active Directory B2C.</span></span> <span data-ttu-id="50507-108">A fejlesztői portálhoz való hozzáférés engedélyezése a klasszikus Azure Active Directory használatával kapcsolatos információkért lásd: [hogyan szeretné engedélyekkel felruházni fejlesztői fiókok Azure Active Directory használatával].</span><span class="sxs-lookup"><span data-stu-id="50507-108">For information about enabling access to the developer portal by using classic Azure Active Directory, see [How to authorize developer accounts using Azure Active Directory].</span></span>

> [!NOTE]
> <span data-ttu-id="50507-109">Ez az útmutató lépéseinek végrehajtásához először az alkalmazás létrehozása az Azure Active Directory B2C-bérlő kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="50507-109">To complete the steps in this guide, you must first have an Azure Active Directory B2C tenant to create an application in.</span></span> <span data-ttu-id="50507-110">Emellett kell készen áll a regisztráció és bejelentkezés házirendet.</span><span class="sxs-lookup"><span data-stu-id="50507-110">Also, you need to have signup and signin policies ready.</span></span> <span data-ttu-id="50507-111">További információkért lásd: [Azure Active Directory B2C áttekintése].</span><span class="sxs-lookup"><span data-stu-id="50507-111">For more information, see [Azure Active Directory B2C overview].</span></span>

## <a name="authorize-developer-accounts-by-using-azure-active-directory-b2c"></a><span data-ttu-id="50507-112">Azure Active Directory B2C segítségével hitelesíthetők a fejlesztői fiókok</span><span class="sxs-lookup"><span data-stu-id="50507-112">Authorize developer accounts by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="50507-113">A kezdéshez kattintson **Publisher portal** az API Management szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="50507-113">To get started, click **Publisher portal** in the Azure portal for your API Management service.</span></span> <span data-ttu-id="50507-114">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="50507-114">This takes you to the API Management publisher portal.</span></span>

   ![Közzétevő portál][api-management-management-console]

   > [!NOTE]
   > <span data-ttu-id="50507-116">Ha még nem hozott az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a a [Ismerkedés az Azure API Management oktatóanyag][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="50507-116">If you haven't yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management tutorial][Get started with Azure API Management].</span></span>

2. <span data-ttu-id="50507-117">Az a **API Management** menüben kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="50507-117">On the **API Management** menu, click **Security**.</span></span> <span data-ttu-id="50507-118">Az a **identitások** lapra, majd **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="50507-118">On the **Identities** tab, choose **Azure Active Directory B2C**.</span></span>

  ![Külső identitások 1][api-management-howto-aad-b2c-security-tab]

3. <span data-ttu-id="50507-120">Jegyezze fel a **átirányítási URL-cím** és Azure Active Directory B2C kapcsoljon át az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="50507-120">Make a note of the **Redirect URL** and switch over to Azure Active Directory B2C in the Azure portal.</span></span>

  ![Külső identitások 2][api-management-howto-aad-b2c-security-tab-reply-url]

4. <span data-ttu-id="50507-122">Kattintson a **alkalmazások** gombra.</span><span class="sxs-lookup"><span data-stu-id="50507-122">Click the **Applications** button.</span></span>

  ![Egy új alkalmazás 1 regisztrálása][api-management-howto-aad-b2c-portal-menu]

5. <span data-ttu-id="50507-124">Kattintson a **Hozzáadás** gombra kattintva hozzon létre egy új Azure Active Directory B2C-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="50507-124">Click the **Add** button to create a new Azure Active Directory B2C application.</span></span>

  ![Egy új alkalmazás 2 regisztrálása][api-management-howto-aad-b2c-add-button]

6. <span data-ttu-id="50507-126">Az a **új alkalmazás** panelen adjon meg egy nevet az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="50507-126">In the **New application** blade, enter a name for the application.</span></span> <span data-ttu-id="50507-127">Válasszon **Igen** alatt **Web App/Web API**, és válassza a **Igen** alatt **implicit engedélyezési folyamat engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="50507-127">Choose **Yes** under **Web App/Web API**, and choose **Yes** under **Allow implicit flow**.</span></span> <span data-ttu-id="50507-128">Másolja a **átirányítási URL-cím** a a **Azure Active Directory B2C** szakasza a **identitások** a közzétevő portál lapot, és illessze be azt a **válasz URL-CÍMEN** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="50507-128">Then copy the **Redirect URL** from the **Azure Active Directory B2C** section of the **Identities** tab in the publisher portal, and paste it into the **Reply URL** text box.</span></span>

  ![Egy új alkalmazás 3 regisztrálása][api-management-howto-aad-b2c-app-details]

7. <span data-ttu-id="50507-130">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="50507-130">Click the **Create** button.</span></span> <span data-ttu-id="50507-131">Ha az alkalmazás létrehozása az megjelenik a **alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="50507-131">When the application is created, it appears in the **Applications** blade.</span></span> <span data-ttu-id="50507-132">Kattintson a részletek megtekintéséhez az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="50507-132">Click the application name to see its details.</span></span>

  ![Egy új alkalmazás 4 regisztrálása][api-management-howto-aad-b2c-app-created]

8. <span data-ttu-id="50507-134">Az a **tulajdonságok** panelen, a Másolás a **Alkalmazásazonosító** a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="50507-134">From the **Properties** blade, copy the **Application ID** to the clipboard.</span></span>

  ![1 alkalmazás azonosítója][api-management-howto-aad-b2c-app-id]

9. <span data-ttu-id="50507-136">Lépjen vissza a közzétevő portálon, és illessze be az azonosító a **ügyfél-azonosító** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="50507-136">Switch back to the publisher portal and paste the ID into the **Client Id** text box.</span></span>

  ![Kérelem azonosítója 2][api-management-howto-aad-b2c-client-id]

10. <span data-ttu-id="50507-138">Váltson vissza az Azure-portálon, kattintson a **kulcsok** gombra, majd **kulcs**.</span><span class="sxs-lookup"><span data-stu-id="50507-138">Switch back to the Azure portal, click the **Keys** button, and then click **Generate key**.</span></span> <span data-ttu-id="50507-139">Kattintson a **mentése** a konfiguráció mentéséhez, majd megjeleníti a **Alkalmazáskulcs**.</span><span class="sxs-lookup"><span data-stu-id="50507-139">Click **Save** to save the configuration and display the **App key**.</span></span> <span data-ttu-id="50507-140">Másolja a vágólapra a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="50507-140">Copy the key to the clipboard.</span></span>

  ![1 alkalmazás kulcs][api-management-howto-aad-b2c-app-key]

11. <span data-ttu-id="50507-142">Váltás a közzétevő portal, majd illessze be a kulcs a **Ügyfélkulcs** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="50507-142">Switch back to the publisher portal and paste the key into the **Client Secret** text box.</span></span>

  ![Alkalmazáskulcs 2][api-management-howto-aad-b2c-client-secret]

12. <span data-ttu-id="50507-144">Adja meg a tartomány nevét az Azure Active Directory B2C-bérlő **engedélyezett bérlői**.</span><span class="sxs-lookup"><span data-stu-id="50507-144">Specify the domain name of the Azure Active Directory B2C tenant in **Allowed Tenant**.</span></span>

  ![Engedélyezett bérlői][api-management-howto-aad-b2c-allowed-tenant]

13. <span data-ttu-id="50507-146">Adja meg a **előfizetési házirend** és **Signin házirend**.</span><span class="sxs-lookup"><span data-stu-id="50507-146">Specify the **Signup Policy** and **Signin Policy**.</span></span> <span data-ttu-id="50507-147">Szükség esetén is megadhatja a **profil szerkesztése házirend** és **jelszó-visszaállítási házirend**.</span><span class="sxs-lookup"><span data-stu-id="50507-147">Optionally, you can also provide the **Profile Editing Policy** and **Password Reset Policy**.</span></span>

  ![Házirendek][api-management-howto-aad-b2c-policies]

  > [!NOTE]
  > <span data-ttu-id="50507-149">A házirendekkel kapcsolatos további információkért lásd: [Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek].</span><span class="sxs-lookup"><span data-stu-id="50507-149">For more information on policies, see [Azure Active Directory B2C: Extensible policy framework].</span></span>

14. <span data-ttu-id="50507-150">A megadott kívánt beállításait, kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="50507-150">After you've specified the desired configuration, click **Save**.</span></span>

  <span data-ttu-id="50507-151">Menti a módosításokat, miután a fejlesztők fog tudni új fiókok létrehozását, és jelentkezzen be a fejlesztői portálhoz Azure Active Directory B2C használatával.</span><span class="sxs-lookup"><span data-stu-id="50507-151">After the changes are saved, developers will be able to create new accounts and sign in to the developer portal by using Azure Active Directory B2C.</span></span>

## <a name="sign-up-for-a-developer-account-by-using-azure-active-directory-b2c"></a><span data-ttu-id="50507-152">Azure Active Directory B2C segítségével egy developer-fiók regisztrálása</span><span class="sxs-lookup"><span data-stu-id="50507-152">Sign up for a developer account by using Azure Active Directory B2C</span></span>

1. <span data-ttu-id="50507-153">Az Azure Active Directory B2C segítségével egy developer-fiók regisztrálása, nyisson meg egy új böngészőablakot, majd keresse meg a fejlesztői portálhoz.</span><span class="sxs-lookup"><span data-stu-id="50507-153">To sign up for a developer account by using Azure Active Directory B2C, open a new browser window and go to the developer portal.</span></span> <span data-ttu-id="50507-154">Kattintson a **regisztráljon** gombra.</span><span class="sxs-lookup"><span data-stu-id="50507-154">Click the **Sign up** button.</span></span>

   ![Fejlesztői portálján 1][api-management-howto-aad-b2c-dev-portal]

2. <span data-ttu-id="50507-156">Regisztrálni kívánt **Azure Active Directory B2C**.</span><span class="sxs-lookup"><span data-stu-id="50507-156">Choose to sign up with **Azure Active Directory B2C**.</span></span>

   ![Fejlesztői portálján 2][api-management-howto-aad-b2c-dev-portal-b2c-button]

3. <span data-ttu-id="50507-158">A program átirányítja az előfizetési házirend, az előzőekben konfigurált.</span><span class="sxs-lookup"><span data-stu-id="50507-158">You're redirected to the signup policy that you configured in the previous section.</span></span> <span data-ttu-id="50507-159">Válassza ki az e-mail címet vagy egy meglévő közösségi fiók használatával regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="50507-159">Choose to sign up by using your email address or one of your existing social accounts.</span></span>

   > [!NOTE]
   > <span data-ttu-id="50507-160">Ha az Azure Active Directory B2C jelenleg az egyetlen lehetséges, hogy engedélyezve van a **identitások** lapon publisher portálon meg fogja átirányítani az előfizetési házirend közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="50507-160">If Azure Active Directory B2C is the only option that's enabled on the **Identities** tab in the publisher portal, you'll be redirected to the signup policy directly.</span></span>

   ![Fejlesztői portál][api-management-howto-aad-b2c-dev-portal-b2c-options]

   <span data-ttu-id="50507-162">Ha befejeződött a regisztráció, a program átirányítja vissza a fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="50507-162">When the signup is complete, you're redirected back to the developer portal.</span></span> <span data-ttu-id="50507-163">Most bejelentkeztetjük a fejlesztői portálhoz az API Management szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="50507-163">You're now signed in to the developer portal for your API Management service instance.</span></span>

    ![Regisztráció kész][api-management-registration-complete]

## <a name="next-steps"></a><span data-ttu-id="50507-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50507-165">Next steps</span></span>

*  <span data-ttu-id="50507-166">[Azure Active Directory B2C áttekintése]</span><span class="sxs-lookup"><span data-stu-id="50507-166">[Azure Active Directory B2C overview]</span></span>
*  <span data-ttu-id="50507-167">[Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek]</span><span class="sxs-lookup"><span data-stu-id="50507-167">[Azure Active Directory B2C: Extensible policy framework]</span></span>
*  <span data-ttu-id="50507-168">[Az Azure Active Directory B2C identitás-szolgáltatóként a Microsoft-fiók használata]</span><span class="sxs-lookup"><span data-stu-id="50507-168">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="50507-169">[A Google-fiók használata az Azure Active Directory B2C identitás-szolgáltatóként]</span><span class="sxs-lookup"><span data-stu-id="50507-169">[Use a Google account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="50507-170">[Az Azure Active Directory B2C identitás-szolgáltatóként LinkedIn fiók használata]</span><span class="sxs-lookup"><span data-stu-id="50507-170">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]</span></span>
*  <span data-ttu-id="50507-171">[Az Azure Active Directory B2C identitás-szolgáltatóként Facebook fiók használata]</span><span class="sxs-lookup"><span data-stu-id="50507-171">[Use a Facebook account as an identity provider in Azure Active Directory B2C]</span></span>




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

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing the Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph
<span data-ttu-id="50507-172">[Azure Active Directory B2C áttekintése]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview</span><span class="sxs-lookup"><span data-stu-id="50507-172">[Azure Active Directory B2C overview]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview</span></span>
<span data-ttu-id="50507-173">[hogyan szeretné engedélyekkel felruházni fejlesztői fiókok Azure Active Directory használatával]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad</span><span class="sxs-lookup"><span data-stu-id="50507-173">[How to authorize developer accounts using Azure Active Directory]: https://docs.microsoft.com/azure/api-management/api-management-howto-aad</span></span>
<span data-ttu-id="50507-174">[Azure Active Directory B2C: bővíthető szabályzat-keretrendszernek]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies</span><span class="sxs-lookup"><span data-stu-id="50507-174">[Azure Active Directory B2C: Extensible policy framework]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies</span></span>
<span data-ttu-id="50507-175">[Az Azure Active Directory B2C identitás-szolgáltatóként a Microsoft-fiók használata]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app</span><span class="sxs-lookup"><span data-stu-id="50507-175">[Use a Microsoft account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app</span></span>
<span data-ttu-id="50507-176">[A Google-fiók használata az Azure Active Directory B2C identitás-szolgáltatóként]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app</span><span class="sxs-lookup"><span data-stu-id="50507-176">[Use a Google account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-goog-app</span></span>
<span data-ttu-id="50507-177">[Az Azure Active Directory B2C identitás-szolgáltatóként Facebook fiók használata]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app</span><span class="sxs-lookup"><span data-stu-id="50507-177">[Use a Facebook account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-fb-app</span></span>
<span data-ttu-id="50507-178">[Az Azure Active Directory B2C identitás-szolgáltatóként LinkedIn fiók használata]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app</span><span class="sxs-lookup"><span data-stu-id="50507-178">[Use a LinkedIn account as an identity provider in Azure Active Directory B2C]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-li-app</span></span>

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Log in to the Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account
