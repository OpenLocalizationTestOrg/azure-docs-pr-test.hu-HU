---
title: "Az Azure Active Directory B2C: Gyorsműveletek konfigurációs |} Microsoft Docs"
description: "Adja meg a regisztráció és bejelentkezés az Azure Active Directory B2C által védett alkalmazások Gyorsműveletek fiókkal rendelkező felhasználók számára."
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
ms.openlocfilehash: b32e81494b8c84799485f154ae43ad30af394caa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a><span data-ttu-id="d2913-103">Az Azure Active Directory B2C: Regisztráció és bejelentkezés adhat Gyorsműveletek fiókkal rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="d2913-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with QQ accounts</span></span>

> [!NOTE]
> <span data-ttu-id="d2913-104">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="d2913-104">This feature is in preview.</span></span>
> 

## <a name="create-a-qq-application"></a><span data-ttu-id="d2913-105">Gyorsműveletek-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2913-105">Create a QQ application</span></span>

<span data-ttu-id="d2913-106">Gyorsműveletek az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként használatához szüksége Gyorsműveletek-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="d2913-106">To use QQ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a QQ application and supply it with the right parameters.</span></span> <span data-ttu-id="d2913-107">Ehhez Gyorsműveletek fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="d2913-107">You need a QQ account to do this.</span></span> <span data-ttu-id="d2913-108">Ha még nincs fiókja, akkor kaphat egyenként [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span><span class="sxs-lookup"><span data-stu-id="d2913-108">If you don’t have one, you can get one at [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span></span>

### <a name="register-for-the-qq-developer-program"></a><span data-ttu-id="d2913-109">Regisztrálja a Gyorsműveletek fejlesztői programhoz</span><span class="sxs-lookup"><span data-stu-id="d2913-109">Register for the QQ developer program</span></span>

1. <span data-ttu-id="d2913-110">Lépjen a [Gyorsműveletek fejlesztői portálján](http://open.qq.com) és jelentkezzen be a Gyorsműveletek fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d2913-110">Go to the [QQ developer portal](http://open.qq.com) and sign in with your QQ account credentials.</span></span>
2. <span data-ttu-id="d2913-111">Lépjen a bejelentkezést követően a [http://open.qq.com/reg](http://open.qq.com/reg) egy fejlesztő el.</span><span class="sxs-lookup"><span data-stu-id="d2913-111">After signing in, go to [http://open.qq.com/reg](http://open.qq.com/reg) to register yourself as a developer.</span></span>
3. <span data-ttu-id="d2913-112">A menüben válasszon ki**个人**(az egyéni fejlesztői).</span><span class="sxs-lookup"><span data-stu-id="d2913-112">In the menu, select **个人** (individual developer).</span></span>
4. <span data-ttu-id="d2913-113">Adja meg a szükséges adatokat az űrlapon, és kattintson a**下一步**(Tovább).</span><span class="sxs-lookup"><span data-stu-id="d2913-113">Enter the required information into the form and click **下一步** (next step).</span></span>
5. <span data-ttu-id="d2913-114">Végezze el az e-mailek ellenőrzési folyamata.</span><span class="sxs-lookup"><span data-stu-id="d2913-114">Complete the email verification process.</span></span>

> [!NOTE]
> <span data-ttu-id="d2913-115">Jóváhagyásra váró fejlesztőként a regisztrálás után néhány napot várni kell.</span><span class="sxs-lookup"><span data-stu-id="d2913-115">You will need to wait a few days to be approved after registering as a developer.</span></span> 

### <a name="register-a-qq-application"></a><span data-ttu-id="d2913-116">Egy Gyorsműveletek alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d2913-116">Register a QQ application</span></span>

1. <span data-ttu-id="d2913-117">Ugrás a [https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span><span class="sxs-lookup"><span data-stu-id="d2913-117">Go to [https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span></span>
2. <span data-ttu-id="d2913-118">Kattintson a**应用管理**(Alkalmazáskezelés).</span><span class="sxs-lookup"><span data-stu-id="d2913-118">Click on **应用管理** (app management).</span></span>
3. <span data-ttu-id="d2913-119">Kattintson a**创建应用**(az alkalmazás létrehozása).</span><span class="sxs-lookup"><span data-stu-id="d2913-119">Click on **创建应用** (create app).</span></span>
4. <span data-ttu-id="d2913-120">Adja meg a szükséges alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d2913-120">Enter the necessary app information.</span></span>
5. <span data-ttu-id="d2913-121">Kattintson a**创建应用**(az alkalmazás létrehozása).</span><span class="sxs-lookup"><span data-stu-id="d2913-121">Click on **创建应用** (create app).</span></span>
6. <span data-ttu-id="d2913-122">Adja meg a szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="d2913-122">Enter the required information.</span></span>
7. <span data-ttu-id="d2913-123">Az a**授权回调域**(visszahívási URL-címe) mezőbe írja be `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="d2913-123">For the **授权回调域** (callback URL) field, enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="d2913-124">Például ha a `tenant_name` van contoso.onmicrosoft.com, állítsa be az URL-címet kell `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="d2913-124">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
8. <span data-ttu-id="d2913-125">Kattintson a**创建应用**(az alkalmazás létrehozása).</span><span class="sxs-lookup"><span data-stu-id="d2913-125">Click on **创建应用** (create app).</span></span>
9. <span data-ttu-id="d2913-126">A jóváhagyó lapon kattintson a**应用管理**(Alkalmazáskezelés) az alkalmazás felügyeleti lapra való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="d2913-126">On the confirmation page, click on **应用管理** (app management) to return to the app management page.</span></span>
10. <span data-ttu-id="d2913-127">Kattintson a**查看**(megtekintése) mellett az imént létrehozott alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d2913-127">Click on **查看** (view) next to the app you just created.</span></span>
11. <span data-ttu-id="d2913-128">Kattintson a**修改**(Szerkesztés).</span><span class="sxs-lookup"><span data-stu-id="d2913-128">Click on **修改** (edit).</span></span>
12. <span data-ttu-id="d2913-129">A lap tetején, másolja a **Alkalmazásazonosító** és **ALKALMAZÁSKULCS**.</span><span class="sxs-lookup"><span data-stu-id="d2913-129">From the top of the page, copy the **APP ID** and **APP KEY**.</span></span>

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="d2913-130">Gyorsműveletek az Ön bérlőjében identitás-szolgáltatóként konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d2913-130">Configure QQ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="d2913-131">Az alábbi lépéseket követve [lépjen a B2C funkciók panelje](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="d2913-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="d2913-132">Kattintson a B2C funkciók panelje **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="d2913-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="d2913-133">A panel tetején kattintson a **+Add** (+Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d2913-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="d2913-134">Adjon meg egy rövid **neve** a az identitás-szolgáltató konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="d2913-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="d2913-135">Írja be például a "Gyorsműveletek".</span><span class="sxs-lookup"><span data-stu-id="d2913-135">For example, enter "QQ".</span></span>
5. <span data-ttu-id="d2913-136">Kattintson a **identitás szolgáltatótípus**, jelölje be **Gyorsműveletek**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2913-136">Click **Identity provider type**, select **QQ**, and click **OK**.</span></span>
6. <span data-ttu-id="d2913-137">Kattintson a **az identitásszolgáltató beállítása**</span><span class="sxs-lookup"><span data-stu-id="d2913-137">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="d2913-138">Adja meg a **Alkalmazáskulcs** korábban kimásolt a **ügyfél-azonosító**.</span><span class="sxs-lookup"><span data-stu-id="d2913-138">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="d2913-139">Adja meg a **alkalmazás titkos kulcs** korábban kimásolt a **Ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="d2913-139">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="d2913-140">Kattintson a **OK** majd **létrehozása** a Gyorsműveletek konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d2913-140">Click **OK** and then click **Create** to save your QQ configuration.</span></span>

