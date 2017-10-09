---
title: "Az Azure Active Directory B2C: WeChat konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers WeChat fiókokhoz az Azure Active Directory B2C által védett alkalmazások."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 92cc3579d818d2379a503ccc695138b33a34466d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-wechat-accounts"></a><span data-ttu-id="ac5b2-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers WeChat fiókok adja meg.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with WeChat accounts</span></span>

> [!NOTE]
> <span data-ttu-id="ac5b2-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-104">This feature is in preview.</span></span>
> 

## <a name="create-a-wechat-application"></a><span data-ttu-id="ac5b2-105">WeChat-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac5b2-105">Create a WeChat application</span></span>

<span data-ttu-id="ac5b2-106">toouse WeChat identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy WeChat alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-106">toouse WeChat as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a WeChat application and supply it with hello right parameters.</span></span> <span data-ttu-id="ac5b2-107">Egy WeChat fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-107">You need a WeChat account toodo this.</span></span> <span data-ttu-id="ac5b2-108">Ha még nincs fiókja, egyet a mobilalkalmazások egyikével regisztrál vagy a Gyorsműveletek szám használatával kaphat.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-108">If you don’t have one, you can get one by signing up through one of their mobile apps or by using your QQ number.</span></span> <span data-ttu-id="ac5b2-109">Ezt követően a hello WeChat fejlesztőprogrambeli regisztrált fiók lekérése.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-109">After that, get your account registered with hello WeChat developer program.</span></span> <span data-ttu-id="ac5b2-110">További információt talál [Itt](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span><span class="sxs-lookup"><span data-stu-id="ac5b2-110">You can find more information [here](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span></span>

### <a name="register-a-wechat-application"></a><span data-ttu-id="ac5b2-111">Egy WeChat alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="ac5b2-111">Register a WeChat application</span></span>

1. <span data-ttu-id="ac5b2-112">Nyissa meg túl[https://open.weixin.qq.com/](https://open.weixin.qq.com/) , és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-112">Go too[https://open.weixin.qq.com/](https://open.weixin.qq.com/) and log in.</span></span>
2. <span data-ttu-id="ac5b2-113">Kattintson a**管理中心**(felügyeleti központ).</span><span class="sxs-lookup"><span data-stu-id="ac5b2-113">Click on **管理中心** (management center).</span></span>
3. <span data-ttu-id="ac5b2-114">Hajtsa végre a hello szükséges lépéseket tooregister egy új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-114">Follow hello necessary steps tooregister a new application.</span></span>
4. <span data-ttu-id="ac5b2-115">A**授权回调域**(visszahívási URL-cím), adja meg `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-115">For **授权回调域** (callback URL), enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="ac5b2-116">Például ha a `tenant_name` contoso.onmicrosoft.com, set hello URL-cím toobe van `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-116">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
5. <span data-ttu-id="ac5b2-117">Keresés és a példány hello **Alkalmazásazonosító** és **ALKALMAZÁSKULCS**.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-117">Find and copy hello **APP ID** and **APP KEY**.</span></span> <span data-ttu-id="ac5b2-118">Ezekre később kell.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-118">You will need these later.</span></span>

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="ac5b2-119">A bérlő az identitás-szolgáltatóként WeChat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac5b2-119">Configure WeChat as an identity provider in your tenant</span></span>
1. <span data-ttu-id="ac5b2-120">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-120">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="ac5b2-121">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-121">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="ac5b2-122">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-122">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="ac5b2-123">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-123">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="ac5b2-124">Írja be például a "WeChat".</span><span class="sxs-lookup"><span data-stu-id="ac5b2-124">For example, enter "WeChat".</span></span>
5. <span data-ttu-id="ac5b2-125">Kattintson a **identitás szolgáltatótípus**, jelölje be **WeChat**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-125">Click **Identity provider type**, select **WeChat**, and click **OK**.</span></span>
6. <span data-ttu-id="ac5b2-126">Kattintson a **az identitásszolgáltató beállítása**</span><span class="sxs-lookup"><span data-stu-id="ac5b2-126">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="ac5b2-127">Adja meg a hello **Alkalmazáskulcs** korábban kimásolt hello mint **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-127">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="ac5b2-128">Adja meg a hello **alkalmazás titkos kulcs** korábban kimásolt hello mint **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-128">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="ac5b2-129">Kattintson a **OK** majd **létrehozása** toosave WeChat konfigurációjáról.</span><span class="sxs-lookup"><span data-stu-id="ac5b2-129">Click **OK** and then click **Create** toosave your WeChat configuration.</span></span>

