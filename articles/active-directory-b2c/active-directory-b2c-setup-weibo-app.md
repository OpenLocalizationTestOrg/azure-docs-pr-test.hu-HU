---
title: "Az Azure Active Directory B2C: Weibo konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Weibo fiókokhoz az Azure Active Directory B2C által védett alkalmazások."
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
ms.openlocfilehash: c0648620f318046c1d9d24feb92a0c5f19c6a91a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-weibo-accounts"></a><span data-ttu-id="b6f4f-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Weibo fiókok adja meg.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Weibo accounts</span></span>

> [!NOTE]
> <span data-ttu-id="b6f4f-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-104">This feature is in preview.</span></span> <span data-ttu-id="b6f4f-105">Ne használja az identitásszolgáltató az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-105">Do not use this identity provider in your production environment.</span></span>
> 

## <a name="create-a-weibo-application"></a><span data-ttu-id="b6f4f-106">Weibo-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6f4f-106">Create a Weibo application</span></span>

<span data-ttu-id="b6f4f-107">toouse Weibo identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Weibo alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-107">toouse Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Weibo application and supply it with hello right parameters.</span></span> <span data-ttu-id="b6f4f-108">Egy Weibo fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-108">You need a Weibo account toodo this.</span></span> <span data-ttu-id="b6f4f-109">Ha még nincs fiókja, akkor kaphat egyenként [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-109">If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span></span>

### <a name="register-for-hello-weibo-developer-program"></a><span data-ttu-id="b6f4f-110">Hello Weibo fejlesztőprogrambeli regisztrálása</span><span class="sxs-lookup"><span data-stu-id="b6f4f-110">Register for hello Weibo developer program</span></span>

1. <span data-ttu-id="b6f4f-111">Nyissa meg toohello [Weibo fejlesztői portálján](http://open.weibo.com/) és jelentkezzen be a Weibo fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-111">Go toohello [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.</span></span>
2. <span data-ttu-id="b6f4f-112">Történő bejelentkezés után kattintson a jobb felső sarokban hello megjelenítendő neve.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-112">After signing in, click on your display name in hello top-right corner.</span></span>
3. <span data-ttu-id="b6f4f-113">Hello legördülő menüben válassza ki**编辑开发者信息**(fejlesztői adatainak szerkesztése).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-113">In hello dropdown, select **编辑开发者信息** (edit developer information).</span></span>
4. <span data-ttu-id="b6f4f-114">Adja meg a szükséges hello adatokat hello formába, és kattintson a**提交**(küldje el).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-114">Enter hello required information into hello form and click **提交** (submit).</span></span>
5. <span data-ttu-id="b6f4f-115">Hello e-mail ellenőrzési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-115">Complete hello email verification process.</span></span>
6. <span data-ttu-id="b6f4f-116">Nyissa meg toohello [identitás ellenőrzése lapon](http://open.weibo.com/developers/identity/edit).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-116">Go toohello [identity verification page](http://open.weibo.com/developers/identity/edit).</span></span>
7. <span data-ttu-id="b6f4f-117">Adja meg a szükséges hello adatokat hello formába, és kattintson a**提交**(küldje el).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-117">Enter hello required information into hello form and click **提交** (submit).</span></span>

### <a name="register-a-weibo-application"></a><span data-ttu-id="b6f4f-118">Egy Weibo alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="b6f4f-118">Register a Weibo application</span></span>

1. <span data-ttu-id="b6f4f-119">Nyissa meg toohello [új Weibo regisztrációs oldalra](http://open.weibo.com/apps/new).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-119">Go toohello [new Weibo app registration page](http://open.weibo.com/apps/new).</span></span>
2. <span data-ttu-id="b6f4f-120">Hello szükséges alkalmazással kapcsolatos adatok megadása</span><span class="sxs-lookup"><span data-stu-id="b6f4f-120">Enter hello necessary application information.</span></span>
3. <span data-ttu-id="b6f4f-121">Kattintson a**创建**(létrehozása).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-121">Click **创建** (create).</span></span>
4. <span data-ttu-id="b6f4f-122">Hello értékeit másolja **Alkalmazáskulcs** és **alkalmazás titkos kulcs**.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-122">Copy hello values of **App Key** and **App Secret**.</span></span> <span data-ttu-id="b6f4f-123">Ezt később lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-123">You will need this later.</span></span>
5. <span data-ttu-id="b6f4f-124">Töltse fel a szükséges hello fényképek, és írja be a hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-124">Upload hello required photos and enter hello necessary information.</span></span>
6. <span data-ttu-id="b6f4f-125">Kattintson a**保存以上信息**(Mentés).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-125">Click **保存以上信息** (save).</span></span>
7. <span data-ttu-id="b6f4f-126">Kattintson a**高级信息**(speciális információk).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-126">Click **高级信息** (advanced information).</span></span>
8. <span data-ttu-id="b6f4f-127">Kattintson a**编辑**(Szerkesztés) következő toohello mező OAuth2.0**授权设置**(átirányítási URL-cím).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-127">Click **编辑** (edit) next toohello field for OAuth2.0 **授权设置** (redirect URL).</span></span>
9. <span data-ttu-id="b6f4f-128">Adja meg `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` a OAuth2.0**授权设置**(átirányítási URL-cím).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-128">Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL).</span></span> <span data-ttu-id="b6f4f-129">Például ha a `tenant_name` contoso.onmicrosoft.com, set hello URL-cím toobe van `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-129">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
10. <span data-ttu-id="b6f4f-130">Kattintson a**提交**(küldje el).</span><span class="sxs-lookup"><span data-stu-id="b6f4f-130">Click **提交** (submit).</span></span>  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="b6f4f-131">A bérlő az identitás-szolgáltatóként Weibo konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b6f4f-131">Configure Weibo as an identity provider in your tenant</span></span>
1. <span data-ttu-id="b6f4f-132">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-132">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="b6f4f-133">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-133">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="b6f4f-134">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-134">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="b6f4f-135">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-135">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="b6f4f-136">Írja be például a "Weibo".</span><span class="sxs-lookup"><span data-stu-id="b6f4f-136">For example, enter "Weibo".</span></span>
5. <span data-ttu-id="b6f4f-137">Kattintson a **identitás szolgáltatótípus**, jelölje be **Weibo**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-137">Click **Identity provider type**, select **Weibo**, and click **OK**.</span></span>
6. <span data-ttu-id="b6f4f-138">Kattintson a **az identitásszolgáltató beállítása**</span><span class="sxs-lookup"><span data-stu-id="b6f4f-138">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="b6f4f-139">Adja meg a hello **Alkalmazáskulcs** korábban kimásolt hello mint **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-139">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="b6f4f-140">Adja meg a hello **alkalmazás titkos kulcs** korábban kimásolt hello mint **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-140">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="b6f4f-141">Kattintson a **OK** majd **létrehozása** toosave Weibo konfigurációjáról.</span><span class="sxs-lookup"><span data-stu-id="b6f4f-141">Click **OK** and then click **Create** toosave your Weibo configuration.</span></span>

