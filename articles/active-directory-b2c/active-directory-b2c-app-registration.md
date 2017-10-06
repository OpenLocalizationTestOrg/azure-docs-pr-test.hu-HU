---
title: "Azure Active Directory B2C: Alkalmazás regisztrációja | Microsoft Docs"
description: "Hogyan tooregister az alkalmazás Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: bd58e123751db387d6c8f16bd010291ba698b1a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a><span data-ttu-id="50285-103">Azure Active Directory B2C: Az alkalmazás regisztrációja</span><span class="sxs-lookup"><span data-stu-id="50285-103">Azure Active Directory B2C: Register your application</span></span>

<span data-ttu-id="50285-104">Ez a gyors útmutató pár percben összefoglalja egy alkalmazás regisztrálásának folyamatát egy Microsoft Azure Active Directory (Azure AD) B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="50285-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="50285-105">Amikor végzett, az alkalmazás regisztrálva van hello Azure B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="50285-105">When you're finished, your application is registered for use in hello Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50285-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="50285-106">Prerequisites</span></span>

<span data-ttu-id="50285-107">a felhasználói regisztrációt és bejelentkezést elfogadó alkalmazás toobuild, először tooregister hello alkalmazás Azure Active Directory B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="50285-107">toobuild an application that accepts consumer sign-up and sign-in, you first need tooregister hello application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="50285-108">Című rész lépéseit hello segítségével könnyebben nyerhet saját bérlőt [az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="50285-108">Get your own tenant by using hello steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="50285-109">Az Azure AD B2C hello panel az hello Azure-portálon létrehozott alkalmazások felügyelni a hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="50285-109">Applications created from hello Azure AD B2C blade in hello Azure portal must be managed from hello same location.</span></span> <span data-ttu-id="50285-110">Ha manuálisan szerkeszti a PowerShell vagy egy másik portálon hello B2C alkalmazások, nem támogatott lesz, és nem működik együtt az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="50285-110">If you edit hello B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="50285-111">A részleteket a hello [hibát jelzett az alkalmazások](#faulted-apps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="50285-111">See details in hello [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-toob2c-settings"></a><span data-ttu-id="50285-112">Keresse meg a tooB2C beállítások</span><span class="sxs-lookup"><span data-stu-id="50285-112">Navigate tooB2C settings</span></span>

<span data-ttu-id="50285-113">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) , hello hello B2C bérlő globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="50285-113">Log in toohello [Azure portal](https://portal.azure.com/) as hello Global Administrator of hello B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="50285-114">Válassza ki a következő lépések regisztrál hello alkalmazás típusától függően:</span><span class="sxs-lookup"><span data-stu-id="50285-114">Choose next steps based on hello application type you are registering:</span></span>

* [<span data-ttu-id="50285-115">Webalkalmazás regisztrációja</span><span class="sxs-lookup"><span data-stu-id="50285-115">Register a web application</span></span>](#register-a-web-app)
* [<span data-ttu-id="50285-116">Webes API regisztrálása</span><span class="sxs-lookup"><span data-stu-id="50285-116">Register a web API</span></span>](#register-a-web-api)
* [<span data-ttu-id="50285-117">Mobil vagy natív alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="50285-117">Register a mobile or native application</span></span>](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a><span data-ttu-id="50285-118">Webalkalmazás regisztrációja</span><span class="sxs-lookup"><span data-stu-id="50285-118">Register a web app</span></span>

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

<span data-ttu-id="50285-119">Ha webalkalmazása az Azure AD B2C által védett webes API-t hív meg, kövesse a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="50285-119">If your web application calls a web API secured by Azure AD B2C, perform these steps:</span></span>
   1. <span data-ttu-id="50285-120">Hozzon létre egy alkalmazás titkos kulcs is toohello **kulcsok** panelen, majd kattintson a hello **készítése kulcs** gombra.</span><span class="sxs-lookup"><span data-stu-id="50285-120">Create an application secret by going toohello **Keys** blade and clicking hello **Generate Key** button.</span></span> <span data-ttu-id="50285-121">Jegyezze fel a hello **Alkalmazáskulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="50285-121">Make note of hello **App key** value.</span></span> <span data-ttu-id="50285-122">Az alkalmazás kódjában hello alkalmazás titkos kulcsként hello érték használata.</span><span class="sxs-lookup"><span data-stu-id="50285-122">You use hello value as hello application secret in your application's code.</span></span>
   2. <span data-ttu-id="50285-123">Kattintson az **API-hozzáférés**, majd a **Hozzáadás**, lehetőségre, és válassza ki webes API-ját és a hatóköröket (engedélyeket).</span><span class="sxs-lookup"><span data-stu-id="50285-123">Click **API Access**, click **Add**, and select your web API and scopes (permissions).</span></span>

> [!NOTE]
> <span data-ttu-id="50285-124">Az **Application Secret** (Alkalmazástitok) fontos biztonsági hitelesítő adat, amelynek megfelelő biztonságáról gondoskodni kell.</span><span class="sxs-lookup"><span data-stu-id="50285-124">An **Application Secret** is an important security credential, and should be secured appropriately.</span></span>
> 

[<span data-ttu-id="50285-125">Túl Jump**további lépések**</span><span class="sxs-lookup"><span data-stu-id="50285-125">Jump too**next steps**</span></span>](#next-steps)

## <a name="register-a-web-api"></a><span data-ttu-id="50285-126">Webes API regisztrálása</span><span class="sxs-lookup"><span data-stu-id="50285-126">Register a web API</span></span>

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

<span data-ttu-id="50285-127">Kattintson a **hatókörök közzétett** tooadd több hatóköröket annak megfelelően szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="50285-127">Click **Published scopes** tooadd more scopes as necessary.</span></span> <span data-ttu-id="50285-128">Alapértelmezés szerint hello "user_impersonation" hatókörben van definiálva.</span><span class="sxs-lookup"><span data-stu-id="50285-128">By default, hello "user_impersonation" scope is defined.</span></span> <span data-ttu-id="50285-129">hello user_impersonation hatókör az api ad más alkalmazások hello képességét tooaccess hello bejelentkezett felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="50285-129">hello user_impersonation scope gives other applications hello ability tooaccess this api on behalf of hello signed-in user.</span></span> <span data-ttu-id="50285-130">Ha kívánja, hello user_impersonation hatókör távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="50285-130">If you wish, hello user_impersonation scope can be removed.</span></span>

[<span data-ttu-id="50285-131">Túl Jump**további lépések**</span><span class="sxs-lookup"><span data-stu-id="50285-131">Jump too**next steps**</span></span>](#next-steps)

## <a name="register-a-mobile-or-native-app"></a><span data-ttu-id="50285-132">Mobil vagy natív alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="50285-132">Register a mobile or native app</span></span>

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[<span data-ttu-id="50285-133">Túl Jump**további lépések**</span><span class="sxs-lookup"><span data-stu-id="50285-133">Jump too**next steps**</span></span>](#next-steps)

## <a name="limitations"></a><span data-ttu-id="50285-134">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="50285-134">Limitations</span></span>

### <a name="choosing-a-web-app-or-api-reply-url"></a><span data-ttu-id="50285-135">Webalkalmazás vagy API-válasz URL-címének kiválasztása</span><span class="sxs-lookup"><span data-stu-id="50285-135">Choosing a web app or api reply URL</span></span>

<span data-ttu-id="50285-136">Alkalmazások regisztrált az Azure AD B2C jelenleg korlátozott korlátozott tooa értékhalmazt válasz URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="50285-136">Currently, apps that are registered with Azure AD B2C are restricted tooa limited set of reply URL values.</span></span> <span data-ttu-id="50285-137">válasz URL-címet a webalkalmazások és szolgáltatások hello sémát kell kezdődnie hello `https`, és minden válasz URL-cím értékek közös egyetlen DNS-tartományt.</span><span class="sxs-lookup"><span data-stu-id="50285-137">hello reply URL for web apps and services must begin with hello scheme `https`, and all reply URL values must share a single DNS domain.</span></span> <span data-ttu-id="50285-138">Például nem regisztrálhat olyan webalkalmazást, amely az alábbi URL-címek egyikével rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="50285-138">For example, you cannot register a web app that has one of these reply URLs:</span></span>

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="50285-139">hello regisztrációs rendszer összehasonlítja a hello meglévő válasz URL-cím toohello DNS-neve hello válasz URL-címet ad hozzá hello teljes DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="50285-139">hello registration system compares hello whole DNS name of hello existing reply URL toohello DNS name of hello reply URL that you are adding.</span></span> <span data-ttu-id="50285-140">hello kérelem tooadd hello DNS-név sikertelen lesz, ha hello a következő feltételek valamelyike teljesül:</span><span class="sxs-lookup"><span data-stu-id="50285-140">hello request tooadd hello DNS name fails if either of hello following conditions is true:</span></span>

* <span data-ttu-id="50285-141">hello új válasz URL-CÍMEN hello teljes DNS-neve nem egyezik meg a DNS-neve hello hello meglévő válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="50285-141">hello whole DNS name of hello new reply URL does not match hello DNS name of hello existing reply URL.</span></span>
* <span data-ttu-id="50285-142">teljes DNS-neve hello hello új válasz URL-CÍMEN altartománya nem hello meglévő válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="50285-142">hello whole DNS name of hello new reply URL is not a subdomain of hello existing reply URL.</span></span>

<span data-ttu-id="50285-143">Például ha hello app van a válasz URL-címe:</span><span class="sxs-lookup"><span data-stu-id="50285-143">For example, if hello app has this reply URL:</span></span>

`https://login.contoso.com`

<span data-ttu-id="50285-144">Hozzáadhat tooit ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="50285-144">You can add tooit, like this:</span></span>

`https://login.contoso.com/new`

<span data-ttu-id="50285-145">Ebben az esetben hello DNS-név pontosan megegyezik.</span><span class="sxs-lookup"><span data-stu-id="50285-145">In this case, hello DNS name matches exactly.</span></span> <span data-ttu-id="50285-146">Esetleg a következőt teheti meg:</span><span class="sxs-lookup"><span data-stu-id="50285-146">Or, you can do this:</span></span>

`https://new.login.contoso.com`

<span data-ttu-id="50285-147">Ebben az esetben hivatkozott login.contoso.com a tooa DNS altartományában. Ha azt szeretné, hogy egy alkalmazás, amely rendelkezik bejelentkezési-east.contoso.com és a bejelentkezési-west.contoso.com, válasz URL-címek toohave, hozzá kell adnia az válasz URL-címeket az itt megadott sorrendben:</span><span class="sxs-lookup"><span data-stu-id="50285-147">In this case, you're referring tooa DNS subdomain of login.contoso.com. If you want toohave an app that has login-east.contoso.com and login-west.contoso.com as reply URLs, you must add those reply URLs in this order:</span></span>

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

<span data-ttu-id="50285-148">Hello utóbbi két adhat hozzá, mivel azok hello első válasz URL-CÍMEN, az altartományok contoso.com.</span><span class="sxs-lookup"><span data-stu-id="50285-148">You can add hello latter two because they are subdomains of hello first reply URL, contoso.com.</span></span>

### <a name="choosing-a-native-app-redirect-uri"></a><span data-ttu-id="50285-149">Natív alkalmazás átirányítási URI-jának kiválasztása</span><span class="sxs-lookup"><span data-stu-id="50285-149">Choosing a native app redirect URI</span></span>

<span data-ttu-id="50285-150">A mobil-/natív alkalmazások átirányítási URI-jának kiválasztásakor két fontos szempontot kell megfontolnia:</span><span class="sxs-lookup"><span data-stu-id="50285-150">There are two important considerations when choosing a redirect URI for mobile/native applications:</span></span>

* <span data-ttu-id="50285-151">**Egyedi**: hello sémája hello átirányítási URI-t minden egyes alkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="50285-151">**Unique**: hello scheme of hello redirect URI should be unique for every application.</span></span> <span data-ttu-id="50285-152">Ebben a példában (com.onmicrosoft.contoso.appname://redirect/path) a com.onmicrosoft.contoso.appname hello sémát használjuk.</span><span class="sxs-lookup"><span data-stu-id="50285-152">In our example (com.onmicrosoft.contoso.appname://redirect/path), we use com.onmicrosoft.contoso.appname as hello scheme.</span></span> <span data-ttu-id="50285-153">Javasoljuk, hogy ezt a mintát kövesse.</span><span class="sxs-lookup"><span data-stu-id="50285-153">We recommend following this pattern.</span></span> <span data-ttu-id="50285-154">Ha a két alkalmazás megosztása hello azonos sémáját, hello felhasználónál "Válasszon app" párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="50285-154">If two applications share hello same scheme, hello user sees a "choose app" dialog.</span></span> <span data-ttu-id="50285-155">Hello felhasználó esetén a megfelelő választás, ha sikertelen hello bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="50285-155">If hello user makes an incorrect choice, hello login fails.</span></span>
* <span data-ttu-id="50285-156">**Teljes**: Az átirányítási URI-nak sémával és elérési útvonallal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="50285-156">**Complete**: Redirect URI must have a scheme and a path.</span></span> <span data-ttu-id="50285-157">hello elérési utat tartalmaznia kell legalább egy perjel után hello tartomány (például //contoso/ works és //contoso sikertelen).</span><span class="sxs-lookup"><span data-stu-id="50285-157">hello path must contain at least one forward slash after hello domain (for example, //contoso/ works and //contoso fails).</span></span>

<span data-ttu-id="50285-158">Gondoskodjon arról, hogy nincsenek különleges karakterek, például aláhúzásjelet hello az átirányítási URI-címe.</span><span class="sxs-lookup"><span data-stu-id="50285-158">Ensure there are no special characters like underscores in hello redirect uri.</span></span>

### <a name="faulted-apps"></a><span data-ttu-id="50285-159">Hibás alkalmazások</span><span class="sxs-lookup"><span data-stu-id="50285-159">Faulted apps</span></span>

<span data-ttu-id="50285-160">A B2C-alkalmazások NEM szerkeszthetők:</span><span class="sxs-lookup"><span data-stu-id="50285-160">B2C applications should NOT be edited:</span></span>

* <span data-ttu-id="50285-161">Egyéb alkalmazáskezelési portálokon, például a [klasszikus Azure portálon](https://manage.windowsazure.com/) vagy az [alkalmazásregisztrációs portálon](https://apps.dev.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="50285-161">On other application management portals such as the [Azure classic portal](https://manage.windowsazure.com/) & the [Application Registration Portal](https://apps.dev.microsoft.com/).</span></span>
* <span data-ttu-id="50285-162">Graph API vagy PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="50285-162">Using Graph API or PowerShell</span></span>

<span data-ttu-id="50285-163">Ha hello B2C alkalmazás szerkesztése a fent leírt módon, majd próbálja tooedit azt újra hello Azure AD B2C funkciók panelére hello Azure-portálon válik, a hibás alkalmazást és az alkalmazás már nem használható az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="50285-163">If you edit hello B2C application as described above and try tooedit it again in hello Azure AD B2C features blade on hello Azure portal, it becomes a faulted app, and your application is no longer usable with Azure AD B2C.</span></span> <span data-ttu-id="50285-164">Toodelete hello alkalmazást, és hozza létre újra.</span><span class="sxs-lookup"><span data-stu-id="50285-164">You have toodelete hello application and create it again.</span></span>

<span data-ttu-id="50285-165">toodelete hello alkalmazást, és lépjen toohello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/) és hello alkalmazás törlése.</span><span class="sxs-lookup"><span data-stu-id="50285-165">toodelete hello app, go toohello [Application Registration Portal](https://apps.dev.microsoft.com/) and delete hello application there.</span></span> <span data-ttu-id="50285-166">Ahhoz, hogy hello alkalmazás toobe látható kell hello alkalmazás tulajdonosának toobe hello (és nem csak egy hello Bérlői rendszergazda).</span><span class="sxs-lookup"><span data-stu-id="50285-166">In order for hello application toobe visible, you need toobe hello owner of hello application (and not just an admin of hello tenant).</span></span>

## <a name="next-steps"></a><span data-ttu-id="50285-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50285-167">Next steps</span></span>

<span data-ttu-id="50285-168">Most, hogy az Azure AD B2C-vel regisztrált egy alkalmazást, akkor fejezheti be egyik [a gyors üzembe helyezési oktatóanyag](active-directory-b2c-overview.md#get-started) tooget lépéseivel.</span><span class="sxs-lookup"><span data-stu-id="50285-168">Now that you have an application registered with Azure AD B2C, you can complete one of [our quick-start tutorials](active-directory-b2c-overview.md#get-started) tooget up and running.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="50285-169">ASP.NET-webalkalmazás létrehozása regisztrációval, bejelentkezéssel és új jelszó kérésével</span><span class="sxs-lookup"><span data-stu-id="50285-169">Create an ASP.NET web app with sign-up, sign-in, and password reset</span></span>](active-directory-b2c-devquickstarts-web-dotnet-susi.md)