---
title: "Az Azure Active Directory B2C: LinkedIn konfigurációs |} Microsoft Docs"
description: "Regisztráció és bejelentkezés adhat az Azure Active Directory B2C által védett alkalmazások LinkedIn fiókkal rendelkező felhasználók"
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
ms.openlocfilehash: 1a6c4b19261aa34e668554ccad2b6340cddf9bf5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a><span data-ttu-id="77d04-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés adhat LinkedIn-fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="77d04-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with LinkedIn accounts</span></span>
## <a name="create-a-linkedin-application"></a><span data-ttu-id="77d04-104">LinkedIn-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="77d04-104">Create a LinkedIn application</span></span>
<span data-ttu-id="77d04-105">Az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként LinkedIn használatához szüksége LinkedIn-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="77d04-105">To use LinkedIn as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a LinkedIn application and supply it with the right parameters.</span></span> <span data-ttu-id="77d04-106">Ehhez LinkedIn-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="77d04-106">You need a LinkedIn account to do this.</span></span> <span data-ttu-id="77d04-107">Ha még nincs fiókja, beszerezheti a [https://www.linkedin.com/](https://www.linkedin.com/).</span><span class="sxs-lookup"><span data-stu-id="77d04-107">If you don’t have one, you can get it at [https://www.linkedin.com/](https://www.linkedin.com/).</span></span>

1. <span data-ttu-id="77d04-108">Lépjen a [LinkedIn fejlesztők webhely](https://www.developer.linkedin.com/) és jelentkezzen be a LinkedIn-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="77d04-108">Go to the [LinkedIn Developers website](https://www.developer.linkedin.com/) and sign in with your LinkedIn account credentials.</span></span>
2. <span data-ttu-id="77d04-109">Kattintson a **saját alkalmazások** a felső menüsávban majd **alkalmazás létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="77d04-109">Click **My Apps** in the top menu bar and then click **Create Application**.</span></span>
   
    ![LinkedIn - új alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. <span data-ttu-id="77d04-111">Az a **hozzon létre egy új alkalmazást** alkotnak, töltse ki a megfelelő információkat (**vállalatnév**, **neve**, **leírás**, **embléma URL-címe**, **alkalmazás használata**, **webhely URL-címe**, **üzleti E-mail** és **munkahelyi telefon**).</span><span class="sxs-lookup"><span data-stu-id="77d04-111">In the **Create a New Application** form, fill in the relevant information (**Company Name**, **Name**, **Description**, **Application Logo URL**, **Application Use**, **Website URL**, **Business Email** and **Business Phone**).</span></span>
4. <span data-ttu-id="77d04-112">Vállalja, hogy a **LinkedIn API használati** kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="77d04-112">Agree to the **LinkedIn API Terms of Use** and click **Submit**.</span></span>
   
    ![LinkedIn - alkalmazás regisztrálása](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. <span data-ttu-id="77d04-114">Értékeinek másolására **ügyfél-azonosító** és **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="77d04-114">Copy the values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="77d04-115">(Megtalálja azokat a **hitelesítési kulcsokat**.) Mindkettő konfigurálásához LinkedIn-bérlőben identitás-szolgáltatóként kell.</span><span class="sxs-lookup"><span data-stu-id="77d04-115">(You can find them under **Authentication Keys**.) You will need both of them to configure LinkedIn as an identity provider in your tenant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="77d04-116">**Titkos Ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="77d04-116">**Client Secret** is an important security credential.</span></span>
   > 
   > 
6. <span data-ttu-id="77d04-117">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a a **átirányítási URL-t engedélyezett** mező (alatt **OAuth 2.0**).</span><span class="sxs-lookup"><span data-stu-id="77d04-117">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized Redirect URLs** field (under **OAuth 2.0**).</span></span> <span data-ttu-id="77d04-118">Cserélje le **{tenant}** a bérlő nevű (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="77d04-118">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="77d04-119">Kattintson a **Hozzáadás**, és kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="77d04-119">Click **Add**, and then click **Update**.</span></span> <span data-ttu-id="77d04-120">A **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="77d04-120">The **{tenant}** value is case-sensitive.</span></span>
   
    ![LinkedIn - telepítő alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="77d04-122">A bérlő az identitás-szolgáltatóként LinkedIn konfigurálása</span><span class="sxs-lookup"><span data-stu-id="77d04-122">Configure LinkedIn as an identity provider in your tenant</span></span>
1. <span data-ttu-id="77d04-123">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="77d04-123">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="77d04-124">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="77d04-124">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="77d04-125">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="77d04-125">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="77d04-126">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="77d04-126">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="77d04-127">Írja be például a "LI".</span><span class="sxs-lookup"><span data-stu-id="77d04-127">For example, enter "LI".</span></span>
5. <span data-ttu-id="77d04-128">Kattintson a **identitás szolgáltatótípus**, jelölje be **LinkedIn**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="77d04-128">Click **Identity provider type**, select **LinkedIn**, and click **OK**.</span></span>
6. <span data-ttu-id="77d04-129">Kattintson a **az identitásszolgáltató beállítása** , és adja meg az ügyfél-azonosító és a korábban létrehozott LinkedIn alkalmazás titkos ügyfélkulcsot.</span><span class="sxs-lookup"><span data-stu-id="77d04-129">Click **Set up this identity provider** and enter the client ID and client secret of the LinkedIn application that you created earlier.</span></span>
7. <span data-ttu-id="77d04-130">Kattintson a **OK** majd **létrehozása** a LinkedIn-konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="77d04-130">Click **OK** and then click **Create** to save your LinkedIn configuration.</span></span>

