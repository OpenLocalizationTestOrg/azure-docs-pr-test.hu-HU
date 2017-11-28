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
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-microsoft-accounts"></a><span data-ttu-id="42634-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Microsoft-fiókkal rendelkező adja meg.</span><span class="sxs-lookup"><span data-stu-id="42634-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Microsoft accounts</span></span>
## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="42634-104">Microsoft-fiók alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="42634-104">Create a Microsoft account application</span></span>
<span data-ttu-id="42634-105">toouse Microsoft fiók az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként, kell toocreate egy Microsoft-fiók alkalmazást, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="42634-105">toouse Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Microsoft account application and supply it with hello right parameters.</span></span> <span data-ttu-id="42634-106">A Microsoft-fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="42634-106">You need a Microsoft account toodo this.</span></span> <span data-ttu-id="42634-107">Ha még nincs fiókja, beszerezheti a [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="42634-107">If you don’t have one, you can get it at [https://www.live.com/](https://www.live.com/).</span></span>

1. <span data-ttu-id="42634-108">Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be Microsoft-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="42634-108">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2. <span data-ttu-id="42634-109">Kattintson a **hozzáadhat egy alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="42634-109">Click **Add an app**.</span></span>
   
    ![Microsoft fiók – egy új alkalmazás felvétele](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. <span data-ttu-id="42634-111">Adjon meg egy **neve** az alkalmazás, és kattintson **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="42634-111">Provide a **Name** for your application and click **Create application**.</span></span>
   
    ![Microsoft-fiók - alkalmazás neve](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. <span data-ttu-id="42634-113">Másolja a hello értékének **alkalmazásazonosító**. Szüksége lesz rájuk tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="42634-113">Copy hello value of **Application Id**. You will need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span>
   
    ![Microsoft-fiók – alkalmazásazonosító](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. <span data-ttu-id="42634-115">Kattintson a **Hozzáadás platform** válassza **webes**.</span><span class="sxs-lookup"><span data-stu-id="42634-115">Click on **Add platform** and choose **Web**.</span></span>
   
    ![Microsoft fiók - platform hozzáadása](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-fiók - webalkalmazás](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. <span data-ttu-id="42634-118">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **átirányítási URI-azonosítók** mező.</span><span class="sxs-lookup"><span data-stu-id="42634-118">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Redirect URIs** field.</span></span> <span data-ttu-id="42634-119">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="42634-119">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
   
    ![Microsoft-fiók - átirányítási URL-cím](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. <span data-ttu-id="42634-121">Kattintson a **új jelszó létrehozása** alatt hello **alkalmazás titkos kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="42634-121">Click on **Generate New Password** under hello **Application Secrets** section.</span></span> <span data-ttu-id="42634-122">Másolja a hello új jelszót a képernyőn jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="42634-122">Copy hello new password displayed on screen.</span></span> <span data-ttu-id="42634-123">Szüksége lesz rájuk tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="42634-123">You will need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="42634-124">Ez a jelszó nem egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="42634-124">This password is an important security credential.</span></span>
   
    ![Microsoft fiók – új jelszó létrehozása](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-fiók – új jelszó](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. <span data-ttu-id="42634-127">Hello jelölőnégyzetet, amely szerint **Live SDK támogatási** alatt hello **speciális beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="42634-127">Check hello box that says **Live SDK support** under hello **Advanced Options** section.</span></span> <span data-ttu-id="42634-128">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="42634-128">Click **Save**.</span></span>
   
    ![Microsoft-fiók - Live SDK-támogatás](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="42634-130">A bérlő az identitás-szolgáltatóként Microsoft-fiók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42634-130">Configure Microsoft account as an identity provider in your tenant</span></span>
1. <span data-ttu-id="42634-131">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="42634-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="42634-132">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="42634-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="42634-133">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="42634-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="42634-134">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="42634-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="42634-135">Írja be például a "MSA".</span><span class="sxs-lookup"><span data-stu-id="42634-135">For example, enter "MSA".</span></span>
5. <span data-ttu-id="42634-136">Kattintson a **identitás szolgáltatótípus**, jelölje be **Microsoft-fiók**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="42634-136">Click **Identity provider type**, select **Microsoft account**, and click **OK**.</span></span>
6. <span data-ttu-id="42634-137">Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello alkalmazás azonosítóját és jelszavát, amely hello korábban létrehozott Microsoft-fiók alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="42634-137">Click **Set up this identity provider** and enter hello Application Id and password of hello Microsoft account application that you created earlier.</span></span>
7. <span data-ttu-id="42634-138">Kattintson a **OK** majd **létrehozása** toosave a Microsoft-fiók konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="42634-138">Click **OK** and then click **Create** toosave your Microsoft account configuration.</span></span>

