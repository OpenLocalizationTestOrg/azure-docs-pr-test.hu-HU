---
title: "Az Azure Active Directory B2C: Gyorsműveletek konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés tooconsumers Gyorsműveletek fiókokhoz az Azure Active Directory B2C által védett alkalmazások."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 896d6221e01d15de1652a5717cf1f65619101e0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-qq-accounts"></a><span data-ttu-id="c296d-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés tooconsumers Gyorsműveletek fiókok adja meg.</span><span class="sxs-lookup"><span data-stu-id="c296d-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with QQ accounts</span></span>

> [!NOTE]
> <span data-ttu-id="c296d-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="c296d-104">This feature is in preview.</span></span>
> 

## <a name="create-a-qq-application"></a><span data-ttu-id="c296d-105">Gyorsműveletek-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c296d-105">Create a QQ application</span></span>

<span data-ttu-id="c296d-106">Gyorsműveletek toouse identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Gyorsműveletek alkalmazást kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="c296d-106">toouse QQ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a QQ application and supply it with hello right parameters.</span></span> <span data-ttu-id="c296d-107">Egy Gyorsműveletek fiók toodo ez szükséges.</span><span class="sxs-lookup"><span data-stu-id="c296d-107">You need a QQ account toodo this.</span></span> <span data-ttu-id="c296d-108">Ha még nincs fiókja, akkor kaphat egyenként [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span><span class="sxs-lookup"><span data-stu-id="c296d-108">If you don’t have one, you can get one at [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span></span>

### <a name="register-for-hello-qq-developer-program"></a><span data-ttu-id="c296d-109">Gyorsműveletek fejlesztőprogrambeli hello regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c296d-109">Register for hello QQ developer program</span></span>

1. <span data-ttu-id="c296d-110">Nyissa meg toohello [Gyorsműveletek fejlesztői portálján](http://open.qq.com) és jelentkezzen be a Gyorsműveletek fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="c296d-110">Go toohello [QQ developer portal](http://open.qq.com) and sign in with your QQ account credentials.</span></span>
2. <span data-ttu-id="c296d-111">Történő bejelentkezés után nyissa meg túl[http://open.qq.com/reg](http://open.qq.com/reg) tooregister fejlesztőként saját maga.</span><span class="sxs-lookup"><span data-stu-id="c296d-111">After signing in, go too[http://open.qq.com/reg](http://open.qq.com/reg) tooregister yourself as a developer.</span></span>
3. <span data-ttu-id="c296d-112">Hello menüben válasszon ki**个人**(az egyéni fejlesztői).</span><span class="sxs-lookup"><span data-stu-id="c296d-112">In hello menu, select **个人** (individual developer).</span></span>
4. <span data-ttu-id="c296d-113">Adja meg a szükséges hello adatokat hello formába, és kattintson a**下一步**(Tovább).</span><span class="sxs-lookup"><span data-stu-id="c296d-113">Enter hello required information into hello form and click **下一步** (next step).</span></span>
5. <span data-ttu-id="c296d-114">Hello e-mail ellenőrzési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="c296d-114">Complete hello email verification process.</span></span>

> [!NOTE]
> <span data-ttu-id="c296d-115">Szüksége lesz toowait néhány napon toobe jóváhagyott fejlesztőként a regisztrálás után.</span><span class="sxs-lookup"><span data-stu-id="c296d-115">You will need toowait a few days toobe approved after registering as a developer.</span></span> 

### <a name="register-a-qq-application"></a><span data-ttu-id="c296d-116">Egy Gyorsműveletek alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c296d-116">Register a QQ application</span></span>

1. <span data-ttu-id="c296d-117">Nyissa meg túl[https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span><span class="sxs-lookup"><span data-stu-id="c296d-117">Go too[https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span></span>
2. <span data-ttu-id="c296d-118">Kattintson a**应用管理**(Alkalmazáskezelés).</span><span class="sxs-lookup"><span data-stu-id="c296d-118">Click on **应用管理** (app management).</span></span>
3. <span data-ttu-id="c296d-119">Kattintson a**创建应用**(az alkalmazás létrehozása).</span><span class="sxs-lookup"><span data-stu-id="c296d-119">Click on **创建应用** (create app).</span></span>
4. <span data-ttu-id="c296d-120">Adja meg a hello szükséges alkalmazásadatokat.</span><span class="sxs-lookup"><span data-stu-id="c296d-120">Enter hello necessary app information.</span></span>
5. <span data-ttu-id="c296d-121">Kattintson a**创建应用**(az alkalmazás létrehozása).</span><span class="sxs-lookup"><span data-stu-id="c296d-121">Click on **创建应用** (create app).</span></span>
6. <span data-ttu-id="c296d-122">Adja meg a hello szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="c296d-122">Enter hello required information.</span></span>
7. <span data-ttu-id="c296d-123">A hello**授权回调域**(visszahívási URL-címe) mezőbe írja be `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="c296d-123">For hello **授权回调域** (callback URL) field, enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="c296d-124">Például ha a `tenant_name` contoso.onmicrosoft.com, set hello URL-cím toobe van `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="c296d-124">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
8. <span data-ttu-id="c296d-125">Kattintson a**创建应用**(az alkalmazás létrehozása).</span><span class="sxs-lookup"><span data-stu-id="c296d-125">Click on **创建应用** (create app).</span></span>
9. <span data-ttu-id="c296d-126">A hello megerősítési oldalán kattintson a**应用管理**(Alkalmazáskezelés) tooreturn toohello alkalmazás kezelése lapján.</span><span class="sxs-lookup"><span data-stu-id="c296d-126">On hello confirmation page, click on **应用管理** (app management) tooreturn toohello app management page.</span></span>
10. <span data-ttu-id="c296d-127">Kattintson a**查看**(Nézet) imént létrehozott következő toohello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c296d-127">Click on **查看** (view) next toohello app you just created.</span></span>
11. <span data-ttu-id="c296d-128">Kattintson a**修改**(Szerkesztés).</span><span class="sxs-lookup"><span data-stu-id="c296d-128">Click on **修改** (edit).</span></span>
12. <span data-ttu-id="c296d-129">Hello hello oldal tetejére, másolja a hello **Alkalmazásazonosító** és **ALKALMAZÁSKULCS**.</span><span class="sxs-lookup"><span data-stu-id="c296d-129">From hello top of hello page, copy hello **APP ID** and **APP KEY**.</span></span>

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="c296d-130">Gyorsműveletek az Ön bérlőjében identitás-szolgáltatóként konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c296d-130">Configure QQ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="c296d-131">Kövesse az alábbi lépéseket túl[keresse meg a toohello B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c296d-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="c296d-132">A hello B2C funkciók paneljére, kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="c296d-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="c296d-133">Kattintson a **+ Hozzáadás** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="c296d-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="c296d-134">Adjon meg egy rövid **neve** hello identitás szolgáltató a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="c296d-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="c296d-135">Írja be például a "Gyorsműveletek".</span><span class="sxs-lookup"><span data-stu-id="c296d-135">For example, enter "QQ".</span></span>
5. <span data-ttu-id="c296d-136">Kattintson a **identitás szolgáltatótípus**, jelölje be **Gyorsműveletek**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c296d-136">Click **Identity provider type**, select **QQ**, and click **OK**.</span></span>
6. <span data-ttu-id="c296d-137">Kattintson a **az identitásszolgáltató beállítása**</span><span class="sxs-lookup"><span data-stu-id="c296d-137">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="c296d-138">Adja meg a hello **Alkalmazáskulcs** korábban kimásolt hello mint **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="c296d-138">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="c296d-139">Adja meg a hello **alkalmazás titkos kulcs** korábban kimásolt hello mint **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="c296d-139">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="c296d-140">Kattintson a **OK** majd **létrehozása** toosave Gyorsműveletek konfigurációjáról.</span><span class="sxs-lookup"><span data-stu-id="c296d-140">Click **OK** and then click **Create** toosave your QQ configuration.</span></span>

