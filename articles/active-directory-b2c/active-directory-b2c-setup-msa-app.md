---
title: "Az Azure Active Directory B2C: Microsoft-fiók konfigurációja |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés az Azure Active Directory B2C által védett alkalmazások a Microsoft-fiókkal rendelkező felhasználók számára."
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
ms.openlocfilehash: 59879dc0b3fc1d7af3e2a1f67f1701f451de9126
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a><span data-ttu-id="744b9-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés adhat Microsoft-fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="744b9-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Microsoft accounts</span></span>
## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="744b9-104">Microsoft-fiók alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="744b9-104">Create a Microsoft account application</span></span>
<span data-ttu-id="744b9-105">Az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként a Microsoft-fiók használatához szüksége hozzon létre egy Microsoft-fiók alkalmazást, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="744b9-105">To use Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Microsoft account application and supply it with the right parameters.</span></span> <span data-ttu-id="744b9-106">Ehhez a Microsoft-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="744b9-106">You need a Microsoft account to do this.</span></span> <span data-ttu-id="744b9-107">Ha még nincs fiókja, beszerezheti a [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="744b9-107">If you don’t have one, you can get it at [https://www.live.com/](https://www.live.com/).</span></span>

1. <span data-ttu-id="744b9-108">Lépjen a [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be Microsoft-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="744b9-108">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2. <span data-ttu-id="744b9-109">Kattintson a **hozzáadhat egy alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="744b9-109">Click **Add an app**.</span></span>
   
    ![Microsoft fiók – egy új alkalmazás felvétele](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. <span data-ttu-id="744b9-111">Adjon meg egy **neve** az alkalmazás, és kattintson **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="744b9-111">Provide a **Name** for your application and click **Create application**.</span></span>
   
    ![Microsoft-fiók - alkalmazás neve](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. <span data-ttu-id="744b9-113">Másolja a értékének **alkalmazásazonosító**. Szüksége lesz rájuk Microsoft-fiókok konfigurálása a bérlő az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="744b9-113">Copy the value of **Application Id**. You will need it to configure Microsoft account as an identity provider in your tenant.</span></span>
   
    ![Microsoft-fiók – alkalmazásazonosító](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. <span data-ttu-id="744b9-115">Kattintson a **Hozzáadás platform** válassza **webes**.</span><span class="sxs-lookup"><span data-stu-id="744b9-115">Click on **Add platform** and choose **Web**.</span></span>
   
    ![Microsoft fiók - platform hozzáadása](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-fiók - webalkalmazás](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. <span data-ttu-id="744b9-118">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a a **átirányítási URI-azonosítók** mező.</span><span class="sxs-lookup"><span data-stu-id="744b9-118">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Redirect URIs** field.</span></span> <span data-ttu-id="744b9-119">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="744b9-119">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
   
    ![Microsoft-fiók - átirányítási URL-cím](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. <span data-ttu-id="744b9-121">Kattintson a **új jelszó létrehozása** alatt a **alkalmazás titkos kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="744b9-121">Click on **Generate New Password** under the **Application Secrets** section.</span></span> <span data-ttu-id="744b9-122">Másolja az új jelszót a képernyőn jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="744b9-122">Copy the new password displayed on screen.</span></span> <span data-ttu-id="744b9-123">Szüksége lesz rájuk Microsoft-fiókok konfigurálása a bérlő az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="744b9-123">You will need it to configure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="744b9-124">Ez a jelszó nem egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="744b9-124">This password is an important security credential.</span></span>
   
    ![Microsoft fiók – új jelszó létrehozása](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-fiók – új jelszó](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. <span data-ttu-id="744b9-127">Jelölje be a jelölőnégyzetet, amely szerint **Live SDK támogatási** alatt a **speciális beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="744b9-127">Check the box that says **Live SDK support** under the **Advanced Options** section.</span></span> <span data-ttu-id="744b9-128">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="744b9-128">Click **Save**.</span></span>
   
    ![Microsoft-fiók - Live SDK-támogatás](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="744b9-130">A bérlő az identitás-szolgáltatóként Microsoft-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="744b9-130">Configure Microsoft account as an identity provider in your tenant</span></span>
1. <span data-ttu-id="744b9-131">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="744b9-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="744b9-132">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="744b9-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="744b9-133">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="744b9-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="744b9-134">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="744b9-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="744b9-135">Írja be például a "MSA".</span><span class="sxs-lookup"><span data-stu-id="744b9-135">For example, enter "MSA".</span></span>
5. <span data-ttu-id="744b9-136">Kattintson a **identitás szolgáltatótípus**, jelölje be **Microsoft-fiók**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="744b9-136">Click **Identity provider type**, select **Microsoft account**, and click **OK**.</span></span>
6. <span data-ttu-id="744b9-137">Kattintson a **az identitásszolgáltató beállítása** , és írja be az alkalmazás-azonosító és jelszó korábban létrehozott Microsoft-fiók alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="744b9-137">Click **Set up this identity provider** and enter the Application Id and password of the Microsoft account application that you created earlier.</span></span>
7. <span data-ttu-id="744b9-138">Kattintson a **OK** majd **létrehozása** menteni a Microsoft-fiók konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="744b9-138">Click **OK** and then click **Create** to save your Microsoft account configuration.</span></span>

