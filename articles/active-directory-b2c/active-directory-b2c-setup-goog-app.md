---
title: "Az Azure Active Directory B2C: Google + konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Google + az Azure Active Directory B2C által védett alkalmazások fiókkal rendelkező."
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
ms.openlocfilehash: 6ef66eb17777acd95b5f4745ed6097c77e37663b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-google-accounts"></a><span data-ttu-id="1f073-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Google + fiókok adja meg.</span><span class="sxs-lookup"><span data-stu-id="1f073-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Google+ accounts</span></span>
## <a name="create-a-google-application"></a><span data-ttu-id="1f073-104">Google +-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f073-104">Create a Google+ application</span></span>
<span data-ttu-id="1f073-105">toouse Google + identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Google + alapú alkalmazásba kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="1f073-105">toouse Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Google+ application and supply it with hello right parameters.</span></span> <span data-ttu-id="1f073-106">A Google + fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="1f073-106">You need a Google+ account toodo this.</span></span> <span data-ttu-id="1f073-107">Ha még nincs fiókja, beszerezheti a [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span><span class="sxs-lookup"><span data-stu-id="1f073-107">If you don’t have one, you can get it at [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span></span>

1. <span data-ttu-id="1f073-108">Nyissa meg toohello [Google fejlesztői konzol](https://console.developers.google.com/) , és jelentkezzen be Google + fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="1f073-108">Go toohello [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2. <span data-ttu-id="1f073-109">Kattintson a **Create project**, adjon meg egy **projekt neve**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1f073-109">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>
   
    ![Google + - első lépések](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + – új projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. <span data-ttu-id="1f073-112">Kattintson a **API Manager** majd **hitelesítő adatok** a bal oldali navigációs hello.</span><span class="sxs-lookup"><span data-stu-id="1f073-112">Click **API Manager** and then click **Credentials** in hello left navigation.</span></span>
4. <span data-ttu-id="1f073-113">Kattintson a hello **OAuth-hozzájárulás képernyő** hello felső fülre.</span><span class="sxs-lookup"><span data-stu-id="1f073-113">Click hello **OAuth consent screen** tab at hello top.</span></span>
   
    ![Google + - hitelesítő adatok](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. <span data-ttu-id="1f073-115">Válassza ki vagy adjon meg érvényes **E-mail cím**, adjon meg egy **Terméknév**, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1f073-115">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. <span data-ttu-id="1f073-117">Kattintson a **új hitelesítő adatok** majd **OAuth-Ügyfélazonosító**.</span><span class="sxs-lookup"><span data-stu-id="1f073-117">Click **New credentials** and then choose **OAuth client ID**.</span></span>
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. <span data-ttu-id="1f073-119">A **alkalmazástípus**, jelölje be **webes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1f073-119">Under **Application type**, select **Web application**.</span></span>
   
    ![Google + - OAuth-hozzájárulás képernyő](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. <span data-ttu-id="1f073-121">Adjon meg egy **neve** adja meg az alkalmazás `https://login.microsoftonline.com` a hello **JavaScript engedélyezett eredeteket** mező, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **jogosult átirányítási URI-azonosítók**mező.</span><span class="sxs-lookup"><span data-stu-id="1f073-121">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in hello **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized redirect URIs** field.</span></span> <span data-ttu-id="1f073-122">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="1f073-122">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="1f073-123">Hello **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="1f073-123">hello **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="1f073-124">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1f073-124">Click **Create**.</span></span>
   
    ![Google + - ügyfél-azonosító létrehozása](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. <span data-ttu-id="1f073-126">Hello értékeit másolja **ügyfél-azonosító** és **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="1f073-126">Copy hello values of **Client ID** and **Client secret**.</span></span> <span data-ttu-id="1f073-127">Szüksége lesz mindkettő tooconfigure Google +-bérlőben identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="1f073-127">You will need both of them tooconfigure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="1f073-128">**Titkos ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="1f073-128">**Client secret** is an important security credential.</span></span>
   
    ![Google + - ügyfélkulcs](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="1f073-130">Google + identitás-szolgáltatóként az Ön bérelt szolgáltatásának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1f073-130">Configure Google+ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="1f073-131">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1f073-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="1f073-132">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="1f073-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="1f073-133">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="1f073-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="1f073-134">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="1f073-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="1f073-135">Adja meg például "G +".</span><span class="sxs-lookup"><span data-stu-id="1f073-135">For example, enter "G+".</span></span>
5. <span data-ttu-id="1f073-136">Kattintson a **identitás szolgáltatótípus**, jelölje be **Google**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f073-136">Click **Identity provider type**, select **Google**, and click **OK**.</span></span>
6. <span data-ttu-id="1f073-137">Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello azonosító és az ügyfél ügyfélkulcs a korábban létrehozott Google + alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="1f073-137">Click **Set up this identity provider** and enter hello client ID and client secret of hello Google+ application that you created earlier.</span></span>
7. <span data-ttu-id="1f073-138">Kattintson a **OK** , majd **létrehozása** toosave a Google + konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="1f073-138">Click **OK** and then click **Create** toosave your Google+ configuration.</span></span>

