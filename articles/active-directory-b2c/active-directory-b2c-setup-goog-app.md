---
title: "Az Azure Active Directory B2C: Google + konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés Google + az Azure Active Directory B2C által védett alkalmazások fiókkal rendelkező felhasználók számára."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ab73e5c79742ab548733f5712dee1e28461db9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a><span data-ttu-id="7510e-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés biztosítása Google + fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="7510e-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Google+ accounts</span></span>
## <a name="create-a-google-application"></a><span data-ttu-id="7510e-104">Google +-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7510e-104">Create a Google+ application</span></span>
<span data-ttu-id="7510e-105">Használatához Google + az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként kell Google +-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="7510e-105">To use Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Google+ application and supply it with the right parameters.</span></span> <span data-ttu-id="7510e-106">Google +-fiók szükséges ehhez.</span><span class="sxs-lookup"><span data-stu-id="7510e-106">You need a Google+ account to do this.</span></span> <span data-ttu-id="7510e-107">Ha még nincs fiókja, beszerezheti a [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span><span class="sxs-lookup"><span data-stu-id="7510e-107">If you don’t have one, you can get it at [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span></span>

1. <span data-ttu-id="7510e-108">Lépjen a [Google fejlesztői konzol](https://console.developers.google.com/) , és jelentkezzen be Google + fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="7510e-108">Go to the [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2. <span data-ttu-id="7510e-109">Kattintson a **Create project**, adjon meg egy **projekt neve**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7510e-109">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>
   
    ![Google + - első lépések](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + – új projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. <span data-ttu-id="7510e-112">Kattintson a **API Manager** majd **hitelesítő adatok** a bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="7510e-112">Click **API Manager** and then click **Credentials** in the left navigation.</span></span>
4. <span data-ttu-id="7510e-113">Kattintson a **OAuth-hozzájárulás képernyő** fülre az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="7510e-113">Click the **OAuth consent screen** tab at the top.</span></span>
   
    ![Google + - hitelesítő adatok](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. <span data-ttu-id="7510e-115">Válassza ki vagy adjon meg érvényes **E-mail cím**, adjon meg egy **Terméknév**, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7510e-115">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. <span data-ttu-id="7510e-117">Kattintson a **új hitelesítő adatok** majd **OAuth-Ügyfélazonosító**.</span><span class="sxs-lookup"><span data-stu-id="7510e-117">Click **New credentials** and then choose **OAuth client ID**.</span></span>
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. <span data-ttu-id="7510e-119">A **alkalmazástípus**, jelölje be **webes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7510e-119">Under **Application type**, select **Web application**.</span></span>
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. <span data-ttu-id="7510e-121">Adjon meg egy **neve** adja meg az alkalmazás `https://login.microsoftonline.com` a a **JavaScript engedélyezett eredeteket** mező, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a a **jogosult átirányítási URI-azonosítók** a mező.</span><span class="sxs-lookup"><span data-stu-id="7510e-121">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in the **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized redirect URIs** field.</span></span> <span data-ttu-id="7510e-122">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="7510e-122">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="7510e-123">A **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="7510e-123">The **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="7510e-124">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7510e-124">Click **Create**.</span></span>
   
    ![Google + - ügyfél-azonosító létrehozása](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. <span data-ttu-id="7510e-126">Értékeinek másolására **ügyfél-azonosító** és **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="7510e-126">Copy the values of **Client ID** and **Client secret**.</span></span> <span data-ttu-id="7510e-127">Mindkettő konfigurálásához Google +-bérlőben identitás-szolgáltatóként kell.</span><span class="sxs-lookup"><span data-stu-id="7510e-127">You will need both of them to configure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="7510e-128">**Titkos ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="7510e-128">**Client secret** is an important security credential.</span></span>
   
    ![Google + - ügyfélkulcs](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="7510e-130">Google + identitás-szolgáltatóként az Ön bérelt szolgáltatásának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7510e-130">Configure Google+ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="7510e-131">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7510e-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="7510e-132">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="7510e-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="7510e-133">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7510e-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="7510e-134">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="7510e-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="7510e-135">Adja meg például "G +".</span><span class="sxs-lookup"><span data-stu-id="7510e-135">For example, enter "G+".</span></span>
5. <span data-ttu-id="7510e-136">Kattintson a **identitás szolgáltatótípus**, jelölje be **Google**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="7510e-136">Click **Identity provider type**, select **Google**, and click **OK**.</span></span>
6. <span data-ttu-id="7510e-137">Kattintson a **az identitásszolgáltató beállítása** , és adja meg az ügyfél-azonosító és a Google +-alkalmazás, amely a korábban létrehozott titkos ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="7510e-137">Click **Set up this identity provider** and enter the client ID and client secret of the Google+ application that you created earlier.</span></span>
7. <span data-ttu-id="7510e-138">Kattintson a **OK** majd **létrehozása** a Google + konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="7510e-138">Click **OK** and then click **Create** to save your Google+ configuration.</span></span>

