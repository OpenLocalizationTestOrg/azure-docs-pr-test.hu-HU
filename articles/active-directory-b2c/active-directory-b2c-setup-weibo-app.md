---
title: "Az Azure Active Directory B2C: Weibo konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés az Azure Active Directory B2C által védett alkalmazások Weibo fiókkal rendelkező felhasználók számára."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 00c5d3781455c80b33bdbb4c872ae354531baf3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a><span data-ttu-id="6cf73-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés adhat Weibo fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="6cf73-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Weibo accounts</span></span>

> [!NOTE]
> <span data-ttu-id="6cf73-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="6cf73-104">This feature is in preview.</span></span> <span data-ttu-id="6cf73-105">Ne használja az identitásszolgáltató az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6cf73-105">Do not use this identity provider in your production environment.</span></span>
> 

## <a name="create-a-weibo-application"></a><span data-ttu-id="6cf73-106">Weibo-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6cf73-106">Create a Weibo application</span></span>

<span data-ttu-id="6cf73-107">Az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként Weibo használatához szüksége Weibo-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="6cf73-107">To use Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Weibo application and supply it with the right parameters.</span></span> <span data-ttu-id="6cf73-108">Ehhez Weibo fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="6cf73-108">You need a Weibo account to do this.</span></span> <span data-ttu-id="6cf73-109">Ha még nincs fiókja, akkor kaphat egyenként [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span><span class="sxs-lookup"><span data-stu-id="6cf73-109">If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span></span>

### <a name="register-for-the-weibo-developer-program"></a><span data-ttu-id="6cf73-110">Regisztrálja a Weibo fejlesztői programhoz</span><span class="sxs-lookup"><span data-stu-id="6cf73-110">Register for the Weibo developer program</span></span>

1. <span data-ttu-id="6cf73-111">Lépjen a [Weibo fejlesztői portálján](http://open.weibo.com/) és jelentkezzen be a Weibo fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="6cf73-111">Go to the [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.</span></span>
2. <span data-ttu-id="6cf73-112">Történő bejelentkezés után kattintson a jobb felső sarokban a megjelenített név.</span><span class="sxs-lookup"><span data-stu-id="6cf73-112">After signing in, click on your display name in the top-right corner.</span></span>
3. <span data-ttu-id="6cf73-113">A legördülő listában jelölje ki**编辑开发者信息**(fejlesztői adatainak szerkesztése).</span><span class="sxs-lookup"><span data-stu-id="6cf73-113">In the dropdown, select **编辑开发者信息** (edit developer information).</span></span>
4. <span data-ttu-id="6cf73-114">Adja meg a szükséges adatokat az űrlapon, és kattintson a**提交**(küldje el).</span><span class="sxs-lookup"><span data-stu-id="6cf73-114">Enter the required information into the form and click **提交** (submit).</span></span>
5. <span data-ttu-id="6cf73-115">Végezze el az e-mailek ellenőrzési folyamata.</span><span class="sxs-lookup"><span data-stu-id="6cf73-115">Complete the email verification process.</span></span>
6. <span data-ttu-id="6cf73-116">Lépjen a [identitás ellenőrzése lapon](http://open.weibo.com/developers/identity/edit).</span><span class="sxs-lookup"><span data-stu-id="6cf73-116">Go to the [identity verification page](http://open.weibo.com/developers/identity/edit).</span></span>
7. <span data-ttu-id="6cf73-117">Adja meg a szükséges adatokat az űrlapon, és kattintson a**提交**(küldje el).</span><span class="sxs-lookup"><span data-stu-id="6cf73-117">Enter the required information into the form and click **提交** (submit).</span></span>

### <a name="register-a-weibo-application"></a><span data-ttu-id="6cf73-118">Egy Weibo alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6cf73-118">Register a Weibo application</span></span>

1. <span data-ttu-id="6cf73-119">Lépjen a [új Weibo regisztrációs oldalra](http://open.weibo.com/apps/new).</span><span class="sxs-lookup"><span data-stu-id="6cf73-119">Go to the [new Weibo app registration page](http://open.weibo.com/apps/new).</span></span>
2. <span data-ttu-id="6cf73-120">Adja meg a szükséges alkalmazás adatokat.</span><span class="sxs-lookup"><span data-stu-id="6cf73-120">Enter the necessary application information.</span></span>
3. <span data-ttu-id="6cf73-121">Kattintson a**创建**(létrehozása).</span><span class="sxs-lookup"><span data-stu-id="6cf73-121">Click **创建** (create).</span></span>
4. <span data-ttu-id="6cf73-122">Értékeinek másolására **Alkalmazáskulcs** és **alkalmazás titkos kulcs**.</span><span class="sxs-lookup"><span data-stu-id="6cf73-122">Copy the values of **App Key** and **App Secret**.</span></span> <span data-ttu-id="6cf73-123">Ezt később lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6cf73-123">You will need this later.</span></span>
5. <span data-ttu-id="6cf73-124">Töltse fel a szükséges fényképek, és adja meg a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="6cf73-124">Upload the required photos and enter the necessary information.</span></span>
6. <span data-ttu-id="6cf73-125">Kattintson a**保存以上信息**(Mentés).</span><span class="sxs-lookup"><span data-stu-id="6cf73-125">Click **保存以上信息** (save).</span></span>
7. <span data-ttu-id="6cf73-126">Kattintson a**高级信息**(speciális információk).</span><span class="sxs-lookup"><span data-stu-id="6cf73-126">Click **高级信息** (advanced information).</span></span>
8. <span data-ttu-id="6cf73-127">Kattintson a**编辑**(Szerkesztés) mező mellett a OAuth2.0**授权设置**(átirányítási URL-cím).</span><span class="sxs-lookup"><span data-stu-id="6cf73-127">Click **编辑** (edit) next to the field for OAuth2.0 **授权设置** (redirect URL).</span></span>
9. <span data-ttu-id="6cf73-128">Adja meg `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` a OAuth2.0**授权设置**(átirányítási URL-cím).</span><span class="sxs-lookup"><span data-stu-id="6cf73-128">Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL).</span></span> <span data-ttu-id="6cf73-129">Például ha a `tenant_name` van contoso.onmicrosoft.com, állítsa be az URL-címet kell `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="6cf73-129">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
10. <span data-ttu-id="6cf73-130">Kattintson a**提交**(küldje el).</span><span class="sxs-lookup"><span data-stu-id="6cf73-130">Click **提交** (submit).</span></span>  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="6cf73-131">A bérlő az identitás-szolgáltatóként Weibo konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6cf73-131">Configure Weibo as an identity provider in your tenant</span></span>
1. <span data-ttu-id="6cf73-132">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6cf73-132">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="6cf73-133">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="6cf73-133">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="6cf73-134">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6cf73-134">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="6cf73-135">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="6cf73-135">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="6cf73-136">Írja be például a "Weibo".</span><span class="sxs-lookup"><span data-stu-id="6cf73-136">For example, enter "Weibo".</span></span>
5. <span data-ttu-id="6cf73-137">Kattintson a **identitás szolgáltatótípus**, jelölje be **Weibo**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cf73-137">Click **Identity provider type**, select **Weibo**, and click **OK**.</span></span>
6. <span data-ttu-id="6cf73-138">Kattintson a **az identitásszolgáltató beállítása**</span><span class="sxs-lookup"><span data-stu-id="6cf73-138">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="6cf73-139">Adja meg a **Alkalmazáskulcs** korábban kimásolt a **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="6cf73-139">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="6cf73-140">Adja meg a **alkalmazás titkos kulcs** korábban kimásolt a **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="6cf73-140">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="6cf73-141">Kattintson a **OK** majd **létrehozása** a Weibo konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="6cf73-141">Click **OK** and then click **Create** to save your Weibo configuration.</span></span>

