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
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-linkedin-accounts"></a><span data-ttu-id="cb67e-103">Az Azure Active Directory B2C: Adja meg a regisztráció és bejelentkezés tooconsumers LinkedIn-fiókok</span><span class="sxs-lookup"><span data-stu-id="cb67e-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with LinkedIn accounts</span></span>
## <a name="create-a-linkedin-application"></a><span data-ttu-id="cb67e-104">LinkedIn-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb67e-104">Create a LinkedIn application</span></span>
<span data-ttu-id="cb67e-105">toouse LinkedIn identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, kell toocreate LinkedIn-alkalmazás, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="cb67e-105">toouse LinkedIn as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a LinkedIn application and supply it with hello right parameters.</span></span> <span data-ttu-id="cb67e-106">A LinkedIn fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb67e-106">You need a LinkedIn account toodo this.</span></span> <span data-ttu-id="cb67e-107">Ha még nincs fiókja, beszerezheti a [https://www.linkedin.com/](https://www.linkedin.com/).</span><span class="sxs-lookup"><span data-stu-id="cb67e-107">If you don’t have one, you can get it at [https://www.linkedin.com/](https://www.linkedin.com/).</span></span>

1. <span data-ttu-id="cb67e-108">Nyissa meg toohello [LinkedIn fejlesztők webhely](https://www.developer.linkedin.com/) és jelentkezzen be a LinkedIn-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="cb67e-108">Go toohello [LinkedIn Developers website](https://www.developer.linkedin.com/) and sign in with your LinkedIn account credentials.</span></span>
2. <span data-ttu-id="cb67e-109">Kattintson a **saját alkalmazások** a felső menüsávban hello, és kattintson a **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cb67e-109">Click **My Apps** in hello top menu bar and then click **Create Application**.</span></span>
   
    ![LinkedIn - új alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. <span data-ttu-id="cb67e-111">A hello **hozzon létre egy új alkalmazást** alkotnak, töltse ki hello vonatkozó információkat (**vállalatnév**, **neve**, **leírás**, **Embléma URL-címe**, **alkalmazás használata**, **webhely URL-címe**, **üzleti E-mail** és **Munkahelyitelefon**).</span><span class="sxs-lookup"><span data-stu-id="cb67e-111">In hello **Create a New Application** form, fill in hello relevant information (**Company Name**, **Name**, **Description**, **Application Logo URL**, **Application Use**, **Website URL**, **Business Email** and **Business Phone**).</span></span>
4. <span data-ttu-id="cb67e-112">Elfogadom toohello **LinkedIn API használati** kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="cb67e-112">Agree toohello **LinkedIn API Terms of Use** and click **Submit**.</span></span>
   
    ![LinkedIn - alkalmazás regisztrálása](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. <span data-ttu-id="cb67e-114">Hello értékeit másolja **ügyfél-azonosító** és **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="cb67e-114">Copy hello values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="cb67e-115">(Megtalálja azokat a **hitelesítési kulcsokat**.) Szüksége lesz mindkettő tooconfigure LinkedIn-bérlőben identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="cb67e-115">(You can find them under **Authentication Keys**.) You will need both of them tooconfigure LinkedIn as an identity provider in your tenant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cb67e-116">**Titkos Ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="cb67e-116">**Client Secret** is an important security credential.</span></span>
   > 
   > 
6. <span data-ttu-id="cb67e-117">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **átirányítási URL-t engedélyezett** mező (alatt **OAuth 2.0**).</span><span class="sxs-lookup"><span data-stu-id="cb67e-117">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized Redirect URLs** field (under **OAuth 2.0**).</span></span> <span data-ttu-id="cb67e-118">Cserélje le **{tenant}** a bérlő nevű (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="cb67e-118">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="cb67e-119">Kattintson a **Hozzáadás**, és kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="cb67e-119">Click **Add**, and then click **Update**.</span></span> <span data-ttu-id="cb67e-120">Hello **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cb67e-120">hello **{tenant}** value is case-sensitive.</span></span>
   
    ![LinkedIn - telepítő alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="cb67e-122">A bérlő az identitás-szolgáltatóként LinkedIn konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cb67e-122">Configure LinkedIn as an identity provider in your tenant</span></span>
1. <span data-ttu-id="cb67e-123">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="cb67e-123">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="cb67e-124">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="cb67e-124">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="cb67e-125">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="cb67e-125">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="cb67e-126">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="cb67e-126">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="cb67e-127">Írja be például a "LI".</span><span class="sxs-lookup"><span data-stu-id="cb67e-127">For example, enter "LI".</span></span>
5. <span data-ttu-id="cb67e-128">Kattintson a **identitás szolgáltatótípus**, jelölje be **LinkedIn**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb67e-128">Click **Identity provider type**, select **LinkedIn**, and click **OK**.</span></span>
6. <span data-ttu-id="cb67e-129">Kattintson a **az identitásszolgáltató beállítása** , és írja be a hello azonosító és az ügyfél titkos ügyfélkulcs az hello korábban létrehozott LinkedIn-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cb67e-129">Click **Set up this identity provider** and enter hello client ID and client secret of hello LinkedIn application that you created earlier.</span></span>
7. <span data-ttu-id="cb67e-130">Kattintson a **OK** majd **létrehozása** toosave LinkedIn-konfigurációjáról.</span><span class="sxs-lookup"><span data-stu-id="cb67e-130">Click **OK** and then click **Create** toosave your LinkedIn configuration.</span></span>

