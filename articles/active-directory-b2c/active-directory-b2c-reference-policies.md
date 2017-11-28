---
title: "Az Azure Active Directory B2C: Beépített házirendek |} Microsoft Docs"
description: "A témakör az Azure Active Directory B2C bővíthető szabályzat-keretrendszert és különféle házirend létrehozása"
services: active-directory-b2c
documentationcenter: 
author: sama
manager: mbaldwin
editor: PatAltimore
ms.assetid: 0d453e72-7f70-4aa2-953d-938d2814d5a9
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: sama
ms.openlocfilehash: daad3af089afdf76b930053728bb11a5cf4c2a92
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a><span data-ttu-id="f25b8-103">Az Azure Active Directory B2C: Beépített házirendek</span><span class="sxs-lookup"><span data-stu-id="f25b8-103">Azure Active Directory B2C: Built-in policies</span></span>


<span data-ttu-id="f25b8-104">Az Azure Active Directory (Azure AD) B2C bővíthető szabályzat-keretrendszernek a szolgáltatás alapvető erősségével.</span><span class="sxs-lookup"><span data-stu-id="f25b8-104">The extensible policy framework of Azure Active Directory (Azure AD) B2C is the core strength of the service.</span></span> <span data-ttu-id="f25b8-105">Például csak a teljes leírása fogyasztói identitással kapcsolatos műveletet házirendek regisztráció, bejelentkezés és profil szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="f25b8-105">Policies fully describe consumer identity experiences such as sign-up, sign-in, or profile editing.</span></span> <span data-ttu-id="f25b8-106">Például a regisztrációs szabályzatban teszi viselkedés szabályozására, a következő beállítások konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="f25b8-106">For instance, a sign-up policy allows you to control behaviors by configuring the following settings:</span></span>

* <span data-ttu-id="f25b8-107">Fióktípus (közösségi, például a Facebookhoz) és helyi fiókok például az e-mail címeket, amelyek fogyasztókat iratkozzon fel az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="f25b8-107">Account types (social accounts such as Facebook or local accounts such as email addresses) that consumers can use to sign up for the application</span></span>
* <span data-ttu-id="f25b8-108">Attribútumok (például utónév, irányítószámát és cipőméreten) kell lennie az ügyféltől összegyűjtött regisztráció során</span><span class="sxs-lookup"><span data-stu-id="f25b8-108">Attributes (for example, first name, postal code, and shoe size) to be collected from the consumer during sign-up</span></span>
* <span data-ttu-id="f25b8-109">Az Azure többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="f25b8-109">Use of Azure Multi-Factor Authentication</span></span>
* <span data-ttu-id="f25b8-110">Az összes regisztrációs lap megjelenését és működését</span><span class="sxs-lookup"><span data-stu-id="f25b8-110">The look and feel of all sign-up pages</span></span>
* <span data-ttu-id="f25b8-111">Információ (amely akkor jelentkezik, mint a jogcím egy jogkivonatba), hogy az alkalmazás esetén a házirendet kap futtatása befejeződött</span><span class="sxs-lookup"><span data-stu-id="f25b8-111">Information (which manifests as claims in a token) that the application receives when the policy run finishes</span></span>

<span data-ttu-id="f25b8-112">Hozzon létre több, különböző típusú házirendet az Ön bérlőjében, és felhasználja az alkalmazásokban, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="f25b8-112">You can create multiple policies of different types in your tenant and use them in your applications as needed.</span></span> <span data-ttu-id="f25b8-113">Házirendek a különböző alkalmazások felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="f25b8-113">Policies can be reused across applications.</span></span> <span data-ttu-id="f25b8-114">Erre a rugalmasságra segítségével a fejlesztők határozza meg, és módosítsa a fogyasztói identitással kapcsolatos műveletet, a minimális vagy a kód nem módosultak.</span><span class="sxs-lookup"><span data-stu-id="f25b8-114">This flexibility enables developers to define and modify consumer identity experiences with minimal or no changes to their code.</span></span>

<span data-ttu-id="f25b8-115">Házirendek állnak rendelkezésre egy egyszerű fejlesztői felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="f25b8-115">Policies are available for use via a simple developer interface.</span></span> <span data-ttu-id="f25b8-116">Az alkalmazás elindítja egy házirendet a szabványos HTTP-hitelesítési kérelmek (házirend paraméter átadja a kérésben) használatával, és megkapja a testreszabott jogkivonatot válaszként.</span><span class="sxs-lookup"><span data-stu-id="f25b8-116">Your application triggers a policy by using a standard HTTP authentication request (passing a policy parameter in the request) and receives a customized token as response.</span></span> <span data-ttu-id="f25b8-117">Például az egyetlen különbség, hogy a regisztrációs szabályzatban meghívása és invoke egy bejelentkezési házirend kérelmeket között a házirend nevére, amely a "p" lekérdezési karakterlánc-paraméter szerepel:</span><span class="sxs-lookup"><span data-stu-id="f25b8-117">For example, the only difference between requests that invoke a sign-up policy and requests that invoke a sign-in policy is the policy name that's used in the "p" query string parameter:</span></span>

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

<span data-ttu-id="f25b8-118">A szabályzat-keretrendszernek kapcsolatos további információkért lásd: [ebben a blogbejegyzésben kapcsolatban az Azure AD B2C meg a nagyvállalati mobilitással és biztonsággal foglalkozó blogban](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span><span class="sxs-lookup"><span data-stu-id="f25b8-118">For more information about the policy framework, see [this blog post about Azure AD B2C on the Enterprise Mobility and Security Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span></span>

## <a name="create-a-sign-up-or-sign-in-policy"></a><span data-ttu-id="f25b8-119">Regisztráció vagy bejelentkezés házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="f25b8-119">Create a sign-up or sign-in policy</span></span>

<span data-ttu-id="f25b8-120">Ez a házirend mindkét fogyasztói előfizetési & bejelentkezési élmény egyetlen konfigurációval kezeli.</span><span class="sxs-lookup"><span data-stu-id="f25b8-120">This policy handles both consumer sign-up & sign-in experiences with a single configuration.</span></span> <span data-ttu-id="f25b8-121">Fogyasztók le a megfelelő elérési utat (regisztráció vagy bejelentkezés) attól függően, hogy a helyi rendszer vezetett.</span><span class="sxs-lookup"><span data-stu-id="f25b8-121">Consumers are led down the right path (sign-up or sign-in) depending on the context.</span></span> <span data-ttu-id="f25b8-122">Azt is bemutatja a jogkivonatokat, amelyek az alkalmazás sikeres regisztrációra vagy bejelentkezések kap tartalmát.  A kód a minta a regisztráció vagy bejelentkezés házirend [érhető el itt](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="f25b8-122">It also describes the contents of tokens that the application will receive upon successful sign-ups or sign-ins.  A code sample for the sign-up or sign-in policy is [available here](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>  <span data-ttu-id="f25b8-123">Ajánlott maszkolandó, hogy a regisztrációs szabályzatban, és a bejelentkezési házirend keresztül használja ezt a házirendet is.</span><span class="sxs-lookup"><span data-stu-id="f25b8-123">It is recommened that you use this policy over a sign-up policy and sign-in policy.</span></span>  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a><span data-ttu-id="f25b8-124">Előfizetési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="f25b8-124">Create a sign-up policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a><span data-ttu-id="f25b8-125">Bejelentkezési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="f25b8-125">Create a sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a><span data-ttu-id="f25b8-126">A Profilszerkesztési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="f25b8-126">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a><span data-ttu-id="f25b8-127">A jelszó-visszaállítási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="f25b8-127">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a><span data-ttu-id="f25b8-128">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="f25b8-128">Frequently asked questions</span></span>

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a><span data-ttu-id="f25b8-129">Hogyan hivatkozásra a jelszó-visszaállítási házirend regisztráció vagy bejelentkezés házirenddel?</span><span class="sxs-lookup"><span data-stu-id="f25b8-129">How do I link a sign-up or sign-in policy with a password reset policy?</span></span>
<span data-ttu-id="f25b8-130">A regisztráció vagy bejelentkezés házirendje (helyi fiókoknál) hoz létre, ha megjelenik egy **elfelejtette jelszó?** tapasztalat első oldalán lévő hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f25b8-130">When you create a sign-up or sign-in policy (with local accounts), you see a **Forgot password?** link on the first page of the experience.</span></span> <span data-ttu-id="f25b8-131">Ez a hivatkozás nem automatikusan eseményindító jelszó alaphelyzetbe állítása házirend.</span><span class="sxs-lookup"><span data-stu-id="f25b8-131">Clicking this link doesn't automatically trigger a password reset policy.</span></span> 

<span data-ttu-id="f25b8-132">Ehelyett a hiba kódja  **`AADB2C90118`**  küld vissza az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f25b8-132">Instead, the error code **`AADB2C90118`** is returned to your app.</span></span> <span data-ttu-id="f25b8-133">Ez a hibakód kezelni a megadott jelszó-visszaállítási házirend figyelőn kell az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f25b8-133">Your app needs to handle this error code by invoking a specific password reset policy.</span></span> <span data-ttu-id="f25b8-134">További információkért lásd: a [minta azt mutatja be a módszert is, amely a linking házirendek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span><span class="sxs-lookup"><span data-stu-id="f25b8-134">For more information, see a [sample that demonstrates the approach of linking policies](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span></span>

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a><span data-ttu-id="f25b8-135">A regisztráció vagy bejelentkezés házirend vagy a regisztrációs szabályzatban, és a bejelentkezési házirend érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="f25b8-135">Should I use a sign-up or sign-in policy or a sign-up policy and a sign-in policy?</span></span>
<span data-ttu-id="f25b8-136">Azt javasoljuk, hogy a regisztrációs szabályzatban, és a bejelentkezési házirend keresztül használja a regisztráció vagy bejelentkezés házirend.</span><span class="sxs-lookup"><span data-stu-id="f25b8-136">We recommend that you use a sign-up or sign-in policy over a sign-up policy and a sign-in policy.</span></span>  

<span data-ttu-id="f25b8-137">A regisztráció vagy bejelentkezés házirend a bejelentkezési házirend-nál több képességekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f25b8-137">The sign-up or sign-in policy has more capabilities than the sign-in policy.</span></span> <span data-ttu-id="f25b8-138">Lehetővé teszi, hogy lap felhasználói felületének testreszabása és a honosításhoz jobb támogatást.</span><span class="sxs-lookup"><span data-stu-id="f25b8-138">It also enables you to use page UI customization and has better support for localization.</span></span> 

<span data-ttu-id="f25b8-139">A bejelentkezési házirend ajánlott, nem kell szerepelnie a házirendeket, ha csak kisebb testreszabási lehetőségek szükséges branding, és szeretné a jelszó alaphelyzetbe állítása azt beépítve.</span><span class="sxs-lookup"><span data-stu-id="f25b8-139">The sign-in policy is recommended if you don't need to localize your policies, only need minor customization capabilities for branding, and want password reset built into it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f25b8-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f25b8-140">Next steps</span></span>
* [<span data-ttu-id="f25b8-141">Token, munkamenet és egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f25b8-141">Token, session, and single sign-on configuration</span></span>](active-directory-b2c-token-session-sso.md)
* [<span data-ttu-id="f25b8-142">Tiltsa le a felhasználói regisztráció során e-mail ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f25b8-142">Disable email verification during consumer sign-up</span></span>](active-directory-b2c-reference-disable-ev.md)

