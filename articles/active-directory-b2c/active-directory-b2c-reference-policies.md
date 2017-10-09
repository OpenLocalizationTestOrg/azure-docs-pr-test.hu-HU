---
title: "Az Azure Active Directory B2C: Beépített házirendek |} Microsoft Docs"
description: "A témakör az Azure Active Directory B2C hello bővíthető szabályzat-keretrendszert, és hogyan toocreate különféle házirend"
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
ms.openlocfilehash: 24bb85eba30f888c6ea7d0401e05235e8eb6ea79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-built-in-policies"></a><span data-ttu-id="bf5b2-103">Az Azure Active Directory B2C: Beépített házirendek</span><span class="sxs-lookup"><span data-stu-id="bf5b2-103">Azure Active Directory B2C: Built-in policies</span></span>


<span data-ttu-id="bf5b2-104">az Azure Active Directory (Azure AD) B2C hello bővíthető szabályzat-keretrendszernek hello core erőssége hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-104">hello extensible policy framework of Azure Active Directory (Azure AD) B2C is hello core strength of hello service.</span></span> <span data-ttu-id="bf5b2-105">Például csak a teljes leírása fogyasztói identitással kapcsolatos műveletet házirendek regisztráció, bejelentkezés és profil szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-105">Policies fully describe consumer identity experiences such as sign-up, sign-in, or profile editing.</span></span> <span data-ttu-id="bf5b2-106">Például a regisztrációs szabályzatban lehetővé teszi a toocontrol viselkedés úgy konfigurálja a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="bf5b2-106">For instance, a sign-up policy allows you toocontrol behaviors by configuring hello following settings:</span></span>

* <span data-ttu-id="bf5b2-107">Fiók típusa (közösségi, például a Facebookhoz) és helyi fiókok például az e-mail címeket, hogy a fogyasztók hello alkalmazás feliratkozott toosign használhatja</span><span class="sxs-lookup"><span data-stu-id="bf5b2-107">Account types (social accounts such as Facebook or local accounts such as email addresses) that consumers can use toosign up for hello application</span></span>
* <span data-ttu-id="bf5b2-108">Attribútumok (például utónév, irányítószámát és cipőméreten) toobe gyűjtött hello felhasználói regisztráció során</span><span class="sxs-lookup"><span data-stu-id="bf5b2-108">Attributes (for example, first name, postal code, and shoe size) toobe collected from hello consumer during sign-up</span></span>
* <span data-ttu-id="bf5b2-109">Az Azure többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="bf5b2-109">Use of Azure Multi-Factor Authentication</span></span>
* <span data-ttu-id="bf5b2-110">hello megjelenését és működését minden bejelentkezési lap</span><span class="sxs-lookup"><span data-stu-id="bf5b2-110">hello look and feel of all sign-up pages</span></span>
* <span data-ttu-id="bf5b2-111">Információ (amely akkor jelentkezik, mint a jogcím egy jogkivonatba), amely az alkalmazás hello kap hello házirend futtatása után</span><span class="sxs-lookup"><span data-stu-id="bf5b2-111">Information (which manifests as claims in a token) that hello application receives when hello policy run finishes</span></span>

<span data-ttu-id="bf5b2-112">Hozzon létre több, különböző típusú házirendet az Ön bérlőjében, és felhasználja az alkalmazásokban, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-112">You can create multiple policies of different types in your tenant and use them in your applications as needed.</span></span> <span data-ttu-id="bf5b2-113">Házirendek a különböző alkalmazások felhasználhatók.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-113">Policies can be reused across applications.</span></span> <span data-ttu-id="bf5b2-114">Erre a rugalmasságra lehetővé teszi a fejlesztők toodefine, és módosítsa a fogyasztói identitással kapcsolatos műveletet, a minimális vagy nincs módosítások tootheir kód.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-114">This flexibility enables developers toodefine and modify consumer identity experiences with minimal or no changes tootheir code.</span></span>

<span data-ttu-id="bf5b2-115">Házirendek állnak rendelkezésre egy egyszerű fejlesztői felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-115">Policies are available for use via a simple developer interface.</span></span> <span data-ttu-id="bf5b2-116">Az alkalmazás elindítja egy házirendet a szabványos HTTP-hitelesítési kérelmek (házirend paraméter benyújtása hello kérelem) használatával, és megkapja a testreszabott jogkivonatot válaszként.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-116">Your application triggers a policy by using a standard HTTP authentication request (passing a policy parameter in hello request) and receives a customized token as response.</span></span> <span data-ttu-id="bf5b2-117">Hello egyetlen különbség a között, amelyek aktiválják az előfizetési házirend és invoke egy bejelentkezési házirend kérelmeket például hello "p" lekérdezésparaméter karakterlánca használt hello házirend neve:</span><span class="sxs-lookup"><span data-stu-id="bf5b2-117">For example, hello only difference between requests that invoke a sign-up policy and requests that invoke a sign-in policy is hello policy name that's used in hello "p" query string parameter:</span></span>

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

<span data-ttu-id="bf5b2-118">Hello szabályzat-keretrendszernek kapcsolatos további információkért lásd: [ebben a blogbejegyzésben kapcsolatban az Azure AD B2C a hello nagyvállalati mobilitással és biztonsággal foglalkozó blogban](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf5b2-118">For more information about hello policy framework, see [this blog post about Azure AD B2C on hello Enterprise Mobility and Security Blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).</span></span>

## <a name="create-a-sign-up-or-sign-in-policy"></a><span data-ttu-id="bf5b2-119">Regisztráció vagy bejelentkezés házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf5b2-119">Create a sign-up or sign-in policy</span></span>

<span data-ttu-id="bf5b2-120">Ez a házirend mindkét fogyasztói előfizetési & bejelentkezési élmény egyetlen konfigurációval kezeli.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-120">This policy handles both consumer sign-up & sign-in experiences with a single configuration.</span></span> <span data-ttu-id="bf5b2-121">A fogyasztók hello megfelelő elérési utat (regisztráció vagy bejelentkezés) hello környezettől függően le vannak vezetett.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-121">Consumers are led down hello right path (sign-up or sign-in) depending on hello context.</span></span> <span data-ttu-id="bf5b2-122">Azt is bemutatja hello tartalmát jogkivonatokat, amelyek hello alkalmazás sikeres regisztrációra vagy bejelentkezések fog kapni.  A kód a minta hello regisztráció vagy bejelentkezés házirend [érhető el itt](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="bf5b2-122">It also describes hello contents of tokens that hello application will receive upon successful sign-ups or sign-ins.  A code sample for hello sign-up or sign-in policy is [available here](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>  <span data-ttu-id="bf5b2-123">Ajánlott maszkolandó, hogy a regisztrációs szabályzatban, és a bejelentkezési házirend keresztül használja ezt a házirendet is.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-123">It is recommened that you use this policy over a sign-up policy and sign-in policy.</span></span>  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-policy"></a><span data-ttu-id="bf5b2-124">Előfizetési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf5b2-124">Create a sign-up policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-policy"></a><span data-ttu-id="bf5b2-125">Bejelentkezési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf5b2-125">Create a sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-policy"></a><span data-ttu-id="bf5b2-126">A Profilszerkesztési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf5b2-126">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-policy"></a><span data-ttu-id="bf5b2-127">A jelszó-visszaállítási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf5b2-127">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="frequently-asked-questions"></a><span data-ttu-id="bf5b2-128">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bf5b2-128">Frequently asked questions</span></span>

### <a name="how-do-i-link-a-sign-up-or-sign-in-policy-with-a-password-reset-policy"></a><span data-ttu-id="bf5b2-129">Hogyan hivatkozásra a jelszó-visszaállítási házirend regisztráció vagy bejelentkezés házirenddel?</span><span class="sxs-lookup"><span data-stu-id="bf5b2-129">How do I link a sign-up or sign-in policy with a password reset policy?</span></span>
<span data-ttu-id="bf5b2-130">A regisztráció vagy bejelentkezés házirendje (helyi fiókoknál) hoz létre, ha megjelenik egy **elfelejtette jelszó?** hello hello tapasztalat első oldalán lévő hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-130">When you create a sign-up or sign-in policy (with local accounts), you see a **Forgot password?** link on hello first page of hello experience.</span></span> <span data-ttu-id="bf5b2-131">Ez a hivatkozás nem automatikusan eseményindító jelszó alaphelyzetbe állítása házirend.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-131">Clicking this link doesn't automatically trigger a password reset policy.</span></span> 

<span data-ttu-id="bf5b2-132">Ehelyett hello hibakód  **`AADB2C90118`**  tooyour app adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-132">Instead, hello error code **`AADB2C90118`** is returned tooyour app.</span></span> <span data-ttu-id="bf5b2-133">Az alkalmazás a hibakóddal kell toohandle figyelőn egy adott jelszó-visszaállítási házirend.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-133">Your app needs toohandle this error code by invoking a specific password reset policy.</span></span> <span data-ttu-id="bf5b2-134">További információkért lásd: a [minta azt mutatja be a hello módszert is, amely a linking házirendek](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span><span class="sxs-lookup"><span data-stu-id="bf5b2-134">For more information, see a [sample that demonstrates hello approach of linking policies](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).</span></span>

### <a name="should-i-use-a-sign-up-or-sign-in-policy-or-a-sign-up-policy-and-a-sign-in-policy"></a><span data-ttu-id="bf5b2-135">A regisztráció vagy bejelentkezés házirend vagy a regisztrációs szabályzatban, és a bejelentkezési házirend érdemes használni?</span><span class="sxs-lookup"><span data-stu-id="bf5b2-135">Should I use a sign-up or sign-in policy or a sign-up policy and a sign-in policy?</span></span>
<span data-ttu-id="bf5b2-136">Azt javasoljuk, hogy a regisztrációs szabályzatban, és a bejelentkezési házirend keresztül használja a regisztráció vagy bejelentkezés házirend.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-136">We recommend that you use a sign-up or sign-in policy over a sign-up policy and a sign-in policy.</span></span>  

<span data-ttu-id="bf5b2-137">hello regisztráció vagy bejelentkezés házirend hello bejelentkezési házirend-nál több képességekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-137">hello sign-up or sign-in policy has more capabilities than hello sign-in policy.</span></span> <span data-ttu-id="bf5b2-138">Lehetővé teszi a toouse lap felhasználói felületének testreszabása és a honosításhoz jobb támogatást.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-138">It also enables you toouse page UI customization and has better support for localization.</span></span> 

<span data-ttu-id="bf5b2-139">hello bejelentkezési házirend ajánlott toolocalize a házirendek nem szükséges, ha csak kisebb testreszabási lehetőségek szükséges branding, és szeretné a jelszó alaphelyzetbe állítása azt beépítve.</span><span class="sxs-lookup"><span data-stu-id="bf5b2-139">hello sign-in policy is recommended if you don't need toolocalize your policies, only need minor customization capabilities for branding, and want password reset built into it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf5b2-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf5b2-140">Next steps</span></span>
* [<span data-ttu-id="bf5b2-141">Token, munkamenet és egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bf5b2-141">Token, session, and single sign-on configuration</span></span>](active-directory-b2c-token-session-sso.md)
* [<span data-ttu-id="bf5b2-142">Tiltsa le a felhasználói regisztráció során e-mail ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="bf5b2-142">Disable email verification during consumer sign-up</span></span>](active-directory-b2c-reference-disable-ev.md)

