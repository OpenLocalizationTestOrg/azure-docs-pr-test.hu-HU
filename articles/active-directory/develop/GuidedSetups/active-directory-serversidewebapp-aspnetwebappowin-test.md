---
title: "aaaAzure AD v2 ASP.NET Web Server bevezetés - teszt |} Microsoft Docs"
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
ms.openlocfilehash: 99c7525b9146605142180962fc2a61b3c953c064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a><span data-ttu-id="e5df1-103">Tesztelheti a kódját</span><span class="sxs-lookup"><span data-stu-id="e5df1-103">Test your code</span></span>

<span data-ttu-id="e5df1-104">Nyomja le az `F5` toorun a projektre a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="e5df1-104">Press `F5` toorun your project in Visual Studio.</span></span> <span data-ttu-id="e5df1-105">hello böngésző megnyitja és közvetlen, túl*http://localhost: {port}* ahol láthatja hello *jelentkezzen be Microsoft* gombra.</span><span class="sxs-lookup"><span data-stu-id="e5df1-105">hello browser will open and direct you too*http://localhost:{port}* where you’ll see hello *Sign in with Microsoft* button.</span></span> <span data-ttu-id="e5df1-106">Lépjen tovább, és kattintson rá a toosign.</span><span class="sxs-lookup"><span data-stu-id="e5df1-106">Go ahead and click it toosign in.</span></span>

<span data-ttu-id="e5df1-107">Ha most készen áll a tootest, munkahelyi vagy iskolai (az Azure Active Directory), vagy egy személyes (live.com, outlook.com) toosign a fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="e5df1-107">When you're ready tootest, use a work or school (Azure Active Directory), or a personal (live.com, outlook.com) account toosign in.</span></span> 

![Jelentkezzen be Microsoft-böngészőablakot](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Jelentkezzen be Microsoft-böngészőablakot](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a><span data-ttu-id="e5df1-110">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="e5df1-110">Expected results</span></span>
<span data-ttu-id="e5df1-111">Bejelentkezéskor a hello felhasználói párt átirányított toohello kezdőlap a webhely, amely hello hello Microsoft alkalmazásregisztrációs portálra az alkalmazás regisztrációs adatait a megadott HTTPS URL-címet.</span><span class="sxs-lookup"><span data-stu-id="e5df1-111">After sign-in, hello user is redirected toohello home page of your web site which is hello HTTPS URL specified in your application registration information in hello Microsoft Application Registration Portal.</span></span> <span data-ttu-id="e5df1-112">Ezen a lapon látható most *Hello {felhasználó}* és egy hivatkozás toosign kibővített, és a hivatkozás toosee hello felhasználói jogcímek – amely egy hivatkozás toohello engedélyezés vezérlő korábban létrehozott-e.</span><span class="sxs-lookup"><span data-stu-id="e5df1-112">This page now shows *Hello {User}* and a link toosign-out, and a link toosee hello user’s claims – which is a link toohello Authorize controller created earlier.</span></span>

### <a name="see-users-claims"></a><span data-ttu-id="e5df1-113">Tekintse meg a felhasználói jogcímek</span><span class="sxs-lookup"><span data-stu-id="e5df1-113">See user's claims</span></span>
<span data-ttu-id="e5df1-114">Válassza ki a hello hivatkozás toosee hello felhasználói jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="e5df1-114">Select hello hyperlink toosee hello user's claims.</span></span> <span data-ttu-id="e5df1-115">A részletes útmutatást toohello tartományvezérlő, és tekintse meg, hogy csak hitelesített elérhető toousers.</span><span class="sxs-lookup"><span data-stu-id="e5df1-115">This leads you toohello controller and view that is only available toousers that are authenticated.</span></span>

#### <a name="expected-results"></a><span data-ttu-id="e5df1-116">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="e5df1-116">Expected results</span></span>
 <span data-ttu-id="e5df1-117">Hello alaptulajdonságait hello bejelentkezett felhasználó tartalmazó táblát kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e5df1-117">You should see a table containing hello basic properties of hello logged user:</span></span>

| <span data-ttu-id="e5df1-118">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e5df1-118">Property</span></span> | <span data-ttu-id="e5df1-119">Érték</span><span class="sxs-lookup"><span data-stu-id="e5df1-119">Value</span></span> | <span data-ttu-id="e5df1-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="e5df1-120">Description</span></span>|
|---|---|---|
| <span data-ttu-id="e5df1-121">Név</span><span class="sxs-lookup"><span data-stu-id="e5df1-121">Name</span></span> | <span data-ttu-id="e5df1-122">{Felhasználó teljes neve}</span><span class="sxs-lookup"><span data-stu-id="e5df1-122">{User Full Name}</span></span> | <span data-ttu-id="e5df1-123">hello felhasználói vezeték- és keresztneve</span><span class="sxs-lookup"><span data-stu-id="e5df1-123">hello user’s first and last name</span></span>
|<span data-ttu-id="e5df1-124">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="e5df1-124">Username</span></span> | <span>user@domain.com</span>| <span data-ttu-id="e5df1-125">hello felhasználónév használt tooidentify hello bejelentkezett felhasználó</span><span class="sxs-lookup"><span data-stu-id="e5df1-125">hello username used tooidentify hello logged user</span></span>
| <span data-ttu-id="e5df1-126">Tárgy</span><span class="sxs-lookup"><span data-stu-id="e5df1-126">Subject</span></span>| <span data-ttu-id="e5df1-127">{Tulajdonos}</span><span class="sxs-lookup"><span data-stu-id="e5df1-127">{Subject}</span></span>|<span data-ttu-id="e5df1-128">Egy karakterlánc toouniquely hello felhasználói bejelentkezési hello weben azonosításához.</span><span class="sxs-lookup"><span data-stu-id="e5df1-128">A string toouniquely identify hello user logon across hello web</span></span>|
| <span data-ttu-id="e5df1-129">Bérlőazonosító</span><span class="sxs-lookup"><span data-stu-id="e5df1-129">Tenant ID</span></span>| <span data-ttu-id="e5df1-130">{Guid}</span><span class="sxs-lookup"><span data-stu-id="e5df1-130">{Guid}</span></span>| <span data-ttu-id="e5df1-131">A *guid* toouniquely hello felhasználó Azure Active Directory szervezetet képviseljék.</span><span class="sxs-lookup"><span data-stu-id="e5df1-131">A *guid* toouniquely represent hello user’s Azure Active Directory organization.</span></span>|

<span data-ttu-id="e5df1-132">Ezenkívül megjelenik egy táblázatot, beleértve az összes felhasználó szerepel a hitelesítési kérelmet.</span><span class="sxs-lookup"><span data-stu-id="e5df1-132">In addition, you will see a table including all user claims included in authentication request.</span></span> <span data-ttu-id="e5df1-133">Egy azonosító Token és magyarázat minden jogcím listáját lásd: a [cikk](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista a jogcímek a lexikális elem azonosítója").</span><span class="sxs-lookup"><span data-stu-id="e5df1-133">For a list of all claims in an Id Token and explanation please see this [article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "List of Claims in Id Token").</span></span>


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a><span data-ttu-id="e5df1-134">Egy metódust, amelynek elérésekor teszt egy *[engedélyezés]* attribútum (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="e5df1-134">Test accessing a method that has an *[Authorize]* attribute (Optional)</span></span>
<span data-ttu-id="e5df1-135">Ebben a lépésben tesztelni elérése során hello hitelesített vezérlő névtelen felhasználóként:</span><span class="sxs-lookup"><span data-stu-id="e5df1-135">In this step, you will test accessing hello Authenticated controller as an anonymous user:</span></span><br/>
<span data-ttu-id="e5df1-136">Válassza ki a hello hivatkozás toosign kibővített hello felhasználó- és kijelentkezési folyamat teljes hello.</span><span class="sxs-lookup"><span data-stu-id="e5df1-136">Select hello link toosign-out hello user and complete hello sign-out process.</span></span><br/>
<span data-ttu-id="e5df1-137">A böngészőben, a parancssorba írja be a http://localhost: {port} / hitelesített tooaccess a tartományvezérlő által védett hello `[Authorize]` attribútum</span><span class="sxs-lookup"><span data-stu-id="e5df1-137">Now in your browser, type http://localhost:{port}/authenticated tooaccess your controller that is protected with hello `[Authorize]` attribute</span></span>

#### <a name="expected-results"></a><span data-ttu-id="e5df1-138">Kívánt eredmény elérése érdekében</span><span class="sxs-lookup"><span data-stu-id="e5df1-138">Expected results</span></span>
<span data-ttu-id="e5df1-139">Nincs szükség tooauthenticate toosee hello nézet hello kérdés kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="e5df1-139">You should receive hello prompt requiring you tooauthenticate toosee hello view.</span></span>

## <a name="additional-information"></a><span data-ttu-id="e5df1-140">További információ</span><span class="sxs-lookup"><span data-stu-id="e5df1-140">Additional information</span></span>

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a><span data-ttu-id="e5df1-141">A teljes webhelyet védelme</span><span class="sxs-lookup"><span data-stu-id="e5df1-141">Protect your entire web site</span></span>
<span data-ttu-id="e5df1-142">tooprotect a teljes webhelyet hello hozzáadása `AuthorizeAttribute` túl`GlobalFilters` a `Global.asax` `Application_Start` módszert:</span><span class="sxs-lookup"><span data-stu-id="e5df1-142">tooprotect your entire web site, add hello `AuthorizeAttribute` too`GlobalFilters` in `Global.asax` `Application_Start` method:</span></span>

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> <span data-ttu-id="e5df1-143">**Hogyan tooyour alkalmazásban csak egy szervezet toosign toorestrict felhasználók**</span><span class="sxs-lookup"><span data-stu-id="e5df1-143">**How toorestrict users from only one organization toosign in tooyour application**</span></span>

> <span data-ttu-id="e5df1-144">Alapértelmezés szerint személyes fiókok (például outlook.com, live.com, és egyéb), valamint a vállalat vagy szervezet, amely az Azure Active Directoryval integrált munkahelyi és iskolai fiókok is bejelentkezhet tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e5df1-144">By default, personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory can sign-in tooyour application.</span></span> 

> <span data-ttu-id="e5df1-145">Ha azt szeretné, hogy az alkalmazás tooaccept bejelentkezések csak egy Azure Active Directory szervezeti, cserélje le a hello `Tenant` paramétere *web.config* a `Common` toohello bérlő neve hello szervezet – példa *contoso.onmicrosoft.com*. Ezt követően módosíthatja a hello `ValidateIssuer` argumentumának a *OWIN indítási osztály* túl`true`.</span><span class="sxs-lookup"><span data-stu-id="e5df1-145">If you want your application tooaccept sign-ins from only one Azure Active Directory organization, replace hello `Tenant` parameter in *web.config* from `Common` toohello tenant name of hello organization – example, *contoso.onmicrosoft.com*. After that, change hello `ValidateIssuer` argument in your *OWIN Startup class* too`true`.</span></span>

> <span data-ttu-id="e5df1-146">tooallow felhasználók közül csak bizonyos szervezetekkel meg `ValidateIssuer` tootrue és -felhasználási hello `ValidIssuers` paraméter toospecify szervezetek listáját.</span><span class="sxs-lookup"><span data-stu-id="e5df1-146">tooallow users from only a list of specific organizations, set `ValidateIssuer` tootrue and use hello `ValidIssuers` parameter toospecify a list of organizations.</span></span>

> <span data-ttu-id="e5df1-147">Egy másik lehetőség van egyéni módszer tooimplement toovalidate hello kiállítók IssuerValidator paraméter használatával.</span><span class="sxs-lookup"><span data-stu-id="e5df1-147">Another option is tooimplement a custom method toovalidate hello issuers using IssuerValidator parameter.</span></span> <span data-ttu-id="e5df1-148">További információ `TokenValidationParameters`, lásd: [ez](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-cikk") MSDN-cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="e5df1-148">For more information about `TokenValidationParameters`, please see [this](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN article") MSDN article.</span></span>

