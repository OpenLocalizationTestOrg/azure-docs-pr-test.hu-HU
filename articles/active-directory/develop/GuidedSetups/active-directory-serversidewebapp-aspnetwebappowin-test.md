---
title: "Az Azure AD v2 ASP.NET Web Server első lépések – tesztelése |} Microsoft Docs"
description: "A Microsoft bejelentkezés végrehajtási egy ASP.NET-megoldás a hagyományos böngészőalapú webalkalmazás a szabványos OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a><span data-ttu-id="79f0f-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="79f0f-103">Test your code</span></span>

<span data-ttu-id="79f0f-104">Nyomja le az `F5` a projektet a Visual Studio futtatásához.</span><span class="sxs-lookup"><span data-stu-id="79f0f-104">Press `F5` to run your project in Visual Studio.</span></span> <span data-ttu-id="79f0f-105">A böngészőben megnyitja és, hogy közvetlen *http://localhost: {port}* ahol láthatja a *jelentkezzen be Microsoft* gombra.</span><span class="sxs-lookup"><span data-stu-id="79f0f-105">The browser will open and direct you to *http://localhost:{port}* where you’ll see the *Sign in with Microsoft* button.</span></span> <span data-ttu-id="79f0f-106">Lépjen tovább, és kattintson rá a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="79f0f-106">Go ahead and click it to sign in.</span></span>

<span data-ttu-id="79f0f-107">Amikor készen áll a tesztelése, használatával munkahelyi vagy iskolai (az Azure Active Directory) vagy egy személyes (live.com, outlook.com) fiók jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="79f0f-107">When you're ready to test, use a work or school (Azure Active Directory), or a personal (live.com, outlook.com) account to sign in.</span></span> 

![Jelentkezzen be Microsoft-böngészőablakot](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Jelentkezzen be Microsoft-böngészőablakot](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a><span data-ttu-id="79f0f-110">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="79f0f-110">Expected results</span></span>
<span data-ttu-id="79f0f-111">Bejelentkezés, miután a felhasználó a kezdőlapot, a webhely, amely a HTTPS URL-címét, a Microsoft alkalmazás-regisztrálási portál alkalmazás regisztrációs adatait a megadott átirányítási.</span><span class="sxs-lookup"><span data-stu-id="79f0f-111">After sign-in, the user is redirected to the home page of your web site which is the HTTPS URL specified in your application registration information in the Microsoft Application Registration Portal.</span></span> <span data-ttu-id="79f0f-112">Ezen a lapon látható most *Hello {felhasználó}* és kijelentkezési mutató hivatkozást, és egy hivatkozásra kattintva megtekintheti az a felhasználói jogcímek – ami az engedélyezés vezérlő mutató hivatkozást a korábban létrehozott-e.</span><span class="sxs-lookup"><span data-stu-id="79f0f-112">This page now shows *Hello {User}* and a link to sign-out, and a link to see the user’s claims – which is a link to the Authorize controller created earlier.</span></span>

### <a name="see-users-claims"></a><span data-ttu-id="79f0f-113">Tekintse meg a felhasználói jogcímek</span><span class="sxs-lookup"><span data-stu-id="79f0f-113">See user's claims</span></span>
<span data-ttu-id="79f0f-114">Válassza ki a hivatkozásra kattintva tekintse meg a felhasználói jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="79f0f-114">Select the hyperlink to see the user's claims.</span></span> <span data-ttu-id="79f0f-115">A részletes útmutatást a tartományvezérlő, tekintse meg, hogy csak hitelesített felhasználók számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="79f0f-115">This leads you to the controller and view that is only available to users that are authenticated.</span></span>

#### <a name="expected-results"></a><span data-ttu-id="79f0f-116">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="79f0f-116">Expected results</span></span>
 <span data-ttu-id="79f0f-117">A bejelentkezett felhasználó alaptulajdonságait tartalmazó táblát kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="79f0f-117">You should see a table containing the basic properties of the logged user:</span></span>

| <span data-ttu-id="79f0f-118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="79f0f-118">Property</span></span> | <span data-ttu-id="79f0f-119">Érték</span><span class="sxs-lookup"><span data-stu-id="79f0f-119">Value</span></span> | <span data-ttu-id="79f0f-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="79f0f-120">Description</span></span>|
|---|---|---|
| <span data-ttu-id="79f0f-121">Név</span><span class="sxs-lookup"><span data-stu-id="79f0f-121">Name</span></span> | <span data-ttu-id="79f0f-122">{Felhasználó teljes neve}</span><span class="sxs-lookup"><span data-stu-id="79f0f-122">{User Full Name}</span></span> | <span data-ttu-id="79f0f-123">A felhasználó nagyapja vezeték- és keresztneve</span><span class="sxs-lookup"><span data-stu-id="79f0f-123">The user’s first and last name</span></span>
|<span data-ttu-id="79f0f-124">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="79f0f-124">Username</span></span> | <span>user@domain.com</span>| <span data-ttu-id="79f0f-125">A felhasználó azonosítására használt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="79f0f-125">The username used to identify the logged user</span></span>
| <span data-ttu-id="79f0f-126">Tárgy</span><span class="sxs-lookup"><span data-stu-id="79f0f-126">Subject</span></span>| <span data-ttu-id="79f0f-127">{Tulajdonos}</span><span class="sxs-lookup"><span data-stu-id="79f0f-127">{Subject}</span></span>|<span data-ttu-id="79f0f-128">A karakterlánc egyedi azonosítására a weben a felhasználói bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="79f0f-128">A string to uniquely identify the user logon across the web</span></span>|
| <span data-ttu-id="79f0f-129">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="79f0f-129">Tenant ID</span></span>| <span data-ttu-id="79f0f-130">{Guid}</span><span class="sxs-lookup"><span data-stu-id="79f0f-130">{Guid}</span></span>| <span data-ttu-id="79f0f-131">A *guid* a felhasználó Azure Active Directory szervezeti egyedileg ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="79f0f-131">A *guid* to uniquely represent the user’s Azure Active Directory organization.</span></span>|

<span data-ttu-id="79f0f-132">Ezenkívül megjelenik egy táblázatot, beleértve az összes felhasználó szerepel a hitelesítési kérelmet.</span><span class="sxs-lookup"><span data-stu-id="79f0f-132">In addition, you will see a table including all user claims included in authentication request.</span></span> <span data-ttu-id="79f0f-133">Egy azonosító Token és magyarázat minden jogcím listáját lásd: a [cikk](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista a jogcímek a lexikális elem azonosítója").</span><span class="sxs-lookup"><span data-stu-id="79f0f-133">For a list of all claims in an Id Token and explanation please see this [article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "List of Claims in Id Token").</span></span>


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a><span data-ttu-id="79f0f-134">Egy metódust, amelynek elérésekor teszt egy *[engedélyezés]* attribútum (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="79f0f-134">Test accessing a method that has an *[Authorize]* attribute (Optional)</span></span>
<span data-ttu-id="79f0f-135">Ebben a lépésben tesztelni a hitelesített vezérlő névtelen felhasználóként elérésekor:</span><span class="sxs-lookup"><span data-stu-id="79f0f-135">In this step, you will test accessing the Authenticated controller as an anonymous user:</span></span><br/>
<span data-ttu-id="79f0f-136">Válassza ki a Kijelentkezés hivatkozásra a felhasználó, és kijelentkezési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="79f0f-136">Select the link to sign-out the user and complete the sign-out process.</span></span><br/>
<span data-ttu-id="79f0f-137">A böngészőben, a parancssorba írja be a http://localhost: {port} / hitelesített a tartományvezérlő által védett eléréséhez a `[Authorize]` attribútum</span><span class="sxs-lookup"><span data-stu-id="79f0f-137">Now in your browser, type http://localhost:{port}/authenticated to access your controller that is protected with the `[Authorize]` attribute</span></span>

#### <a name="expected-results"></a><span data-ttu-id="79f0f-138">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="79f0f-138">Expected results</span></span>
<span data-ttu-id="79f0f-139">A parancssorba, nincs szükség, hogy a nézet hitelesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="79f0f-139">You should receive the prompt requiring you to authenticate to see the view.</span></span>

## <a name="additional-information"></a><span data-ttu-id="79f0f-140">További információ</span><span class="sxs-lookup"><span data-stu-id="79f0f-140">Additional information</span></span>

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a><span data-ttu-id="79f0f-141">A teljes webhelyet védelme</span><span class="sxs-lookup"><span data-stu-id="79f0f-141">Protect your entire web site</span></span>
<span data-ttu-id="79f0f-142">A teljes webhelyet védelme érdekében vegye fel a `AuthorizeAttribute` való `GlobalFilters` a `Global.asax` `Application_Start` módszert:</span><span class="sxs-lookup"><span data-stu-id="79f0f-142">To protect your entire web site, add the `AuthorizeAttribute` to `GlobalFilters` in `Global.asax` `Application_Start` method:</span></span>

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> <span data-ttu-id="79f0f-143">**Jelentkezzen be az alkalmazás csak egy szervezet a felhasználók korlátozása**</span><span class="sxs-lookup"><span data-stu-id="79f0f-143">**How to restrict users from only one organization to sign in to your application**</span></span>

> <span data-ttu-id="79f0f-144">Alapértelmezés szerint személyes fiókok (például outlook.com, live.com és mások), valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely az Azure Active Directoryval integrált is bejelentkezhet az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="79f0f-144">By default, personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory can sign-in to your application.</span></span> 

> <span data-ttu-id="79f0f-145">Ha azt szeretné, hogy az alkalmazás csak egy Azure Active Directory szervezeti bejelentkezések fogadására, cserélje le a `Tenant` paraméterének *web.config* a `Common` a bérlő neve a szervezet – példa, *contoso.onmicrosoft.com*.</span><span class="sxs-lookup"><span data-stu-id="79f0f-145">If you want your application to accept sign-ins from only one Azure Active Directory organization, replace the `Tenant` parameter in *web.config* from `Common` to the tenant name of the organization – example, *contoso.onmicrosoft.com*.</span></span> <span data-ttu-id="79f0f-146">Ezt követően módosíthatja a `ValidateIssuer` argumentumának a *OWIN indítási osztály* való `true`.</span><span class="sxs-lookup"><span data-stu-id="79f0f-146">After that, change the `ValidateIssuer` argument in your *OWIN Startup class* to `true`.</span></span>

> <span data-ttu-id="79f0f-147">Engedélyezi a felhasználók csak bizonyos szervezetekkel listája, állítsa `ValidateIssuer` true, és használja a `ValidIssuers` paraméter adja meg a szervezetek listáját.</span><span class="sxs-lookup"><span data-stu-id="79f0f-147">To allow users from only a list of specific organizations, set `ValidateIssuer` to true and use the `ValidIssuers` parameter to specify a list of organizations.</span></span>

> <span data-ttu-id="79f0f-148">Egy másik lehetőség, hogy a kiállítók IssuerValidator paraméter használatával érvényesítéséhez egyéni módszer alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="79f0f-148">Another option is to implement a custom method to validate the issuers using IssuerValidator parameter.</span></span> <span data-ttu-id="79f0f-149">További információ `TokenValidationParameters`, lásd: [ez](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-cikk") MSDN-cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="79f0f-149">For more information about `TokenValidationParameters`, please see [this](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN article") MSDN article.</span></span>

