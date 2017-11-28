---
title: Az Azure Active Directory B2C |} Microsoft Docs
description: "Hogyan hozhat létre, a sign-Close-Up/sign-a webalkalmazás, a profil szerkesztése és a jelszó alaphelyzetbe állítása az Azure Active Directory B2C használatával."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 3144ced01b524abb035dc1c6f0cdf764bec46804
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a><span data-ttu-id="64466-103">Egy ASP.NET-webalkalmazás létrehozása az Azure Active Directory B2C regisztráció, bejelentkezés, profil szerkesztése és a jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="64466-103">Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset</span></span>

<span data-ttu-id="64466-104">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="64466-104">This tutorial shows you how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="64466-105">Az Azure AD B2C szervezetiidentitás-szolgáltatások hozzáadása a webalkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="64466-105">Add Azure AD B2C identity features to your web app</span></span>
> * <span data-ttu-id="64466-106">A webes alkalmazás regisztrálása az Azure AD B2C-címtárban</span><span class="sxs-lookup"><span data-stu-id="64466-106">Register your web app in your Azure AD B2C directory</span></span>
> * <span data-ttu-id="64466-107">Egy felhasználó sign-Close-Up/sign-in, a profil szerkesztése és a jelszó-visszaállítási házirend a webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="64466-107">Create a user sign-up/sign-in, profile edit, and password reset policy for your web app</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64466-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="64466-108">Prerequisites</span></span>

- <span data-ttu-id="64466-109">A B2C-bérlő csatlakoztatni kell egy Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="64466-109">You must connect your B2C Tenant to an Azure account.</span></span> <span data-ttu-id="64466-110">Létrehozhat egy ingyenes Azure-fiók [Itt](https://azure.microsoft.com/en-us/).</span><span class="sxs-lookup"><span data-stu-id="64466-110">You can create a free Azure account [here](https://azure.microsoft.com/en-us/).</span></span>
- <span data-ttu-id="64466-111">Szüksége [Microsoft Visual Studio](https://www.visualstudio.com/) vagy megtekintéséhez és módosításához a mintakódot hasonló programok.</span><span class="sxs-lookup"><span data-stu-id="64466-111">You need [Microsoft Visual Studio](https://www.visualstudio.com/) or a similar program to view and modify the sample code.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="64466-112">Azure AD B2C címtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="64466-112">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="64466-113">Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="64466-113">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="64466-114">A könyvtár egy olyan tároló, a felhasználók, alkalmazások, csoportok és több.</span><span class="sxs-lookup"><span data-stu-id="64466-114">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="64466-115">Ha egy már nem rendelkezik, ez az útmutató a folytatás előtt hozzon létre egy B2C-címtárat.</span><span class="sxs-lookup"><span data-stu-id="64466-115">If you don't have one already, create a B2C directory before you continue in this guide.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> <span data-ttu-id="64466-116">Az Azure-előfizetéshez a B2C-bérlő csatlakoztatni kell.</span><span class="sxs-lookup"><span data-stu-id="64466-116">You need to connect the B2C Tenant to your Azure subscription.</span></span> <span data-ttu-id="64466-117">Kiválasztása után **létrehozása**, jelölje be a **kapcsolat egy meglévő Azure AD B2C bérlő számára az Azure-előfizetésem** lehetőséget, majd a a **Azure AD B2C bérlő** legördülő listán, válassza ki a a bérlői szeretne hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="64466-117">After selecting **Create**, select the **Link an existing Azure AD B2C Tenant to my Azure subscription** option, and then in the **Azure AD B2C Tenant** drop down, select the tenant you want to associate.</span></span>

## <a name="create-and-register-an-application"></a><span data-ttu-id="64466-118">Hozzon létre, és egy alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="64466-118">Create and register an application</span></span>

<span data-ttu-id="64466-119">Ezt követően kell létrehozni és regisztrálni az alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="64466-119">Next, you need to create and register the app in your B2C directory.</span></span> <span data-ttu-id="64466-120">Ez az Azure AD B2C-vel történő alkalmazása biztonságos kommunikációhoz szükséges információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="64466-120">This provides information that Azure AD B2C needs to securely communicate with your app.</span></span> 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

<span data-ttu-id="64466-121">Amikor elkészült, hogy az API-k és a natív alkalmazás az alkalmazás beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="64466-121">When you are done, you will have both an API and a native application in your application settings.</span></span>

## <a name="create-policies-on-your-b2c-tenant"></a><span data-ttu-id="64466-122">A B2C-bérlő a házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="64466-122">Create policies on your B2C tenant</span></span>

<span data-ttu-id="64466-123">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="64466-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="64466-124">Ebben a kódmintában három identitásélményt tartalmaz: **előfizetési & bejelentkezési**, **profil szerkesztése**, és **jelszó-változtatási**.</span><span class="sxs-lookup"><span data-stu-id="64466-124">This code sample contains three identity experiences: **sign-up & sign-in**, **profile edit**, and **password reset**.</span></span>  <span data-ttu-id="64466-125">Mindkettőhöz létre kell hoznia egy szabályzatot a [szabályzatok áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md) leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="64466-125">You need to create one policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="64466-126">-Házirendet ne feledje, válassza ki a megjelenítendő név attribútum vagy jogcímet, és másolja le a későbbi használatra a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="64466-126">For each policy, be sure to select the Display name attribute or claim, and to copy down the name of your policy for later use.</span></span>

### <a name="add-your-identity-providers"></a><span data-ttu-id="64466-127">Az identitás-szolgáltatóktól hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64466-127">Add your identity providers</span></span>

<span data-ttu-id="64466-128">Válassza ki a beállítások **identitás-szolgáltatóktól** , és válassza a felhasználónév előfizetési vagy e-mailt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="64466-128">From your settings, select **Identity Providers** and choose Username signup or Email signup.</span></span>

### <a name="create-a-sign-up-and-sign-in-policy"></a><span data-ttu-id="64466-129">Regisztráció és bejelentkezés házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="64466-129">Create a sign-up and sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a><span data-ttu-id="64466-130">A Profilszerkesztési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="64466-130">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a><span data-ttu-id="64466-131">A jelszó-visszaállítási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="64466-131">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

<span data-ttu-id="64466-132">A házirendek létrehozása után készen áll az alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="64466-132">After you create your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="64466-133">A mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="64466-133">Download the sample code</span></span>

<span data-ttu-id="64466-134">Az oktatóanyag kódjának kezelése a [GitHubon](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) történik.</span><span class="sxs-lookup"><span data-stu-id="64466-134">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="64466-135">A minta klónozásához futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="64466-135">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="64466-136">Miután letöltötte a mintakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="64466-136">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="64466-137">A megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="64466-137">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="64466-138">`TaskWebApp`a rendszer az MVC webalkalmazás, amely a felhasználó kommunikál.</span><span class="sxs-lookup"><span data-stu-id="64466-138">`TaskWebApp` is the MVC web application that the user interacts with.</span></span> <span data-ttu-id="64466-139">A `TaskService` az alkalmazás webes API háttérszolgáltatása, amely tárolja a felhasználók feladatlistáit.</span><span class="sxs-lookup"><span data-stu-id="64466-139">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="64466-140">Ez a cikk csak a `TaskWebApp` alkalmazást ismerteti.</span><span class="sxs-lookup"><span data-stu-id="64466-140">This article will only discuss the `TaskWebApp` application.</span></span> <span data-ttu-id="64466-141">Megtudhatja, hogyan hozhat létre `TaskService` Azure AD B2C segítségével, lásd: [a .NET webes api-oktatóanyag](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="64466-141">To learn how to build `TaskService` using Azure AD B2C, see [our .NET web api tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="update-code-to-use-your-tenant-and-policies"></a><span data-ttu-id="64466-142">A bérlői és a házirendek kód frissítése</span><span class="sxs-lookup"><span data-stu-id="64466-142">Update code to use your tenant and policies</span></span>

<span data-ttu-id="64466-143">A minta úgy van konfigurálva, hogy a bemutató bérlőnk házirendjeit és ügyfél-azonosítóját használja.</span><span class="sxs-lookup"><span data-stu-id="64466-143">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="64466-144">Csatlakoztassa a saját bérlőt, nyissa meg a szükséges `web.config` a a `TaskWebApp` projektre, és cserélje le a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="64466-144">To connect it to your own tenant, you need to open `web.config` in the `TaskWebApp` project and replace the following values:</span></span>

* <span data-ttu-id="64466-145">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="64466-145">`ida:Tenant` with your tenant name</span></span>
* <span data-ttu-id="64466-146">Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója</span><span class="sxs-lookup"><span data-stu-id="64466-146">`ida:ClientId` with your web app application ID</span></span>
* <span data-ttu-id="64466-147">Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa</span><span class="sxs-lookup"><span data-stu-id="64466-147">`ida:ClientSecret` with your web app secret key</span></span>
* <span data-ttu-id="64466-148">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="64466-148">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
* <span data-ttu-id="64466-149">Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve</span><span class="sxs-lookup"><span data-stu-id="64466-149">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
* <span data-ttu-id="64466-150">Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve</span><span class="sxs-lookup"><span data-stu-id="64466-150">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>

## <a name="launch-the-app"></a><span data-ttu-id="64466-151">Indítsa el az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="64466-151">Launch the app</span></span>
<span data-ttu-id="64466-152">A Visual studióban, indítsa el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="64466-152">From within Visual Studio, launch the app.</span></span> <span data-ttu-id="64466-153">A feladatlista lapon keresse meg és jegyezze fel, az URL-címe: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...</span><span class="sxs-lookup"><span data-stu-id="64466-153">Navigate to the To-Do List tab, and note the URl is: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*&client_id=*YourclientID*.....</span></span>

<span data-ttu-id="64466-154">Iratkozzon fel az alkalmazást az e-mail cím vagy a felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="64466-154">Sign up for the app by using your email address or user name.</span></span> <span data-ttu-id="64466-155">Jelentkezzen ki, majd jelentkezzen be újra, és szerkesztheti a profilját, vagy a jelszó alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="64466-155">Sign out, then sign in again and edit the profile or reset the password.</span></span> <span data-ttu-id="64466-156">Kijelentkezés és bejelentkezés másik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="64466-156">Sign out and sign in as a different user.</span></span> 

## <a name="add-social-idps"></a><span data-ttu-id="64466-157">Közösségi IDPs hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64466-157">Add social IDPs</span></span>

<span data-ttu-id="64466-158">Jelenleg az alkalmazás támogatja a csak a regisztráció és bejelentkezés felhasználói **helyi fiókok**; fiókok a B2C-címtárban tárolja, amely a felhasználónév és jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="64466-158">Currently, the app supports only user sign-up and sign-in by using **local accounts**; accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="64466-159">Az Azure AD B2C segítségével támogathatja más **identitás-szolgáltatóktól** (IDPs) bármely, a kód módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="64466-159">By using Azure AD B2C, you can add support for other **identity providers** (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="64466-160">Közösségi IDPs hozzáadása az alkalmazáshoz, először ezek a cikkek részletes utasításait követve.</span><span class="sxs-lookup"><span data-stu-id="64466-160">To add social IDPs to your app, begin by following the detailed instructions in these articles.</span></span> <span data-ttu-id="64466-161">Minden egyes IDP, amelyeket támogatni kíván meg kell, hogy a rendszer az alkalmazás regisztrálása és szerezzen be egy ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="64466-161">For each IDP you want to support, you need to register an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="64466-162">Az IDP Facebook beállítása</span><span class="sxs-lookup"><span data-stu-id="64466-162">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="64466-163">Az IDP Google beállítása</span><span class="sxs-lookup"><span data-stu-id="64466-163">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="64466-164">Az IDP Amazon beállítása</span><span class="sxs-lookup"><span data-stu-id="64466-164">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="64466-165">Az IDP LinkedIn beállítása</span><span class="sxs-lookup"><span data-stu-id="64466-165">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="64466-166">Miután hozzáadta az identitás-szolgáltatóktól B2C-címtárban, módosítsa a három szabályzat tartalmazza az új IDPs mindegyikének leírtak szerint a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="64466-166">After you add the identity providers to your B2C directory, edit each of your three policies to include the new IDPs, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="64466-167">A házirend mentése után futtassa újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="64466-167">After you save your policies, run the app again.</span></span>  <span data-ttu-id="64466-168">A bejelentkezési hozzáadott új IDPs kell megjelennie, és minden a személyazonosság-előfizetési beállítások észlel.</span><span class="sxs-lookup"><span data-stu-id="64466-168">You should see the new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="64466-169">A házirendek kísérletezhet, és figyelje meg a mintaalkalmazás hatással.</span><span class="sxs-lookup"><span data-stu-id="64466-169">You can experiment with your policies and observe the effect on your sample app.</span></span> <span data-ttu-id="64466-170">Vegye fel vagy távolítsa el a IDPs, kezelheti az alkalmazás jogcímét, vagy módosítsa a regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="64466-170">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="64466-171">Kísérletet, amíg meg nem látja hogyan házirendeket, a hitelesítési kérelmek és OWIN alkalmazássá.</span><span class="sxs-lookup"><span data-stu-id="64466-171">Experiment until you can see how policies, authentication requests, and OWIN tie together.</span></span>

## <a name="sample-code-walkthrough"></a><span data-ttu-id="64466-172">A minta kód forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="64466-172">Sample code walkthrough</span></span>
<span data-ttu-id="64466-173">Az alábbi szakaszok bemutatják, hogyan példakód alkalmazás van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="64466-173">The following sections show you how the sample application code is configured.</span></span> <span data-ttu-id="64466-174">Akkor lehet, hogy ezzel útmutatóként a jövőbeli fejlesztés.</span><span class="sxs-lookup"><span data-stu-id="64466-174">You may use this as a guide in your future app development.</span></span>

### <a name="add-authentication-support"></a><span data-ttu-id="64466-175">Vegye fel a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="64466-175">Add authentication support</span></span>

<span data-ttu-id="64466-176">Most már konfigurálhatja az alkalmazás az Azure AD B2C használatához.</span><span class="sxs-lookup"><span data-stu-id="64466-176">You can now configure your app to use Azure AD B2C.</span></span> <span data-ttu-id="64466-177">Az alkalmazást úgy, hogy az OpenID Connect hitelesítési kérelmeket küld az Azure AD B2C kommunikál.</span><span class="sxs-lookup"><span data-stu-id="64466-177">Your app communicates with Azure AD B2C by sending OpenID Connect authentication requests.</span></span> <span data-ttu-id="64466-178">A kérelmek előírják a felhasználói élmény, a házirend hajtható végre szeretné az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="64466-178">The requests dictate the user experience your app wants to execute by specifying the policy.</span></span> <span data-ttu-id="64466-179">A Microsoft OWIN könyvtár segítségével ezeket a kérelmeket küldeni, házirendek hajtható végre, kezelheti a felhasználói munkamenetek és még sok más.</span><span class="sxs-lookup"><span data-stu-id="64466-179">You can use Microsoft's OWIN library to send these requests, execute policies, manage user sessions, and more.</span></span>

#### <a name="install-owin"></a><span data-ttu-id="64466-180">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="64466-180">Install OWIN</span></span>

<span data-ttu-id="64466-181">Első lépésként hozzá az OWIN köztes NuGet-csomagok a projekthez a Visual Studio Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="64466-181">To begin, add the OWIN middleware NuGet packages to the project by using the Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a><span data-ttu-id="64466-182">OWIN indítási osztály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64466-182">Add an OWIN startup class</span></span>

<span data-ttu-id="64466-183">Adjon hozzá egy OWIN indítási osztályt a `Startup.cs` nevű API-hoz.</span><span class="sxs-lookup"><span data-stu-id="64466-183">Add an OWIN startup class to the API called `Startup.cs`.</span></span>  <span data-ttu-id="64466-184">Kattintson a jobb gombbal a projektre, válassza az **Add** (Hozzáadás), majd a **New Item** (Új elem) lehetőséget, és keresse meg az OWIN elemet.</span><span class="sxs-lookup"><span data-stu-id="64466-184">Right-click on the project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="64466-185">Az OWIN közbenső szoftver meghívja a `Configuration(…)` metódust az alkalmazás indulásakor.</span><span class="sxs-lookup"><span data-stu-id="64466-185">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="64466-186">A mintánkban módosítottuk az osztálydeklarációt `public partial class Startup` értékre, és implementáltuk az `App_Start\Startup.Auth.cs` fájlban lévő osztály másik részét.</span><span class="sxs-lookup"><span data-stu-id="64466-186">In our sample, we changed the class declaration to `public partial class Startup` and implemented the other part of the class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="64466-187">A `Configuration` metóduson belül hozzáadtunk egy hívást a `ConfigureAuth` metódusra, amely a `Startup.Auth.cs` fájlban van definiálva.</span><span class="sxs-lookup"><span data-stu-id="64466-187">Inside the `Configuration` method, we added a call to `ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="64466-188">A módosítás után a `Startup.cs` a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="64466-188">After the modifications, `Startup.cs` looks like the following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-the-authentication-middleware"></a><span data-ttu-id="64466-189">A hitelesítési köztes konfigurálása</span><span class="sxs-lookup"><span data-stu-id="64466-189">Configure the authentication middleware</span></span>

<span data-ttu-id="64466-190">Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítását a `ConfigureAuth(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="64466-190">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="64466-191">Megadja a paraméterek `OpenIdConnectAuthenticationOptions` kommunikálni az Azure AD B2C-koordináták az alkalmazások kiszolgálására.</span><span class="sxs-lookup"><span data-stu-id="64466-191">The parameters you provide in `OpenIdConnectAuthenticationOptions` serve as coordinates for your app to communicate with Azure AD B2C.</span></span> <span data-ttu-id="64466-192">Ha nem adja meg a paraméterek, az alapértelmezett értéket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="64466-192">If you do not specify certain parameters, it will use the default value.</span></span> <span data-ttu-id="64466-193">Például, hogy ne adja meg a `ResponseType` minta, ezért a alapértelmezett érték `code id_token` minden kimenő kérelem Azure AD B2C használni fogják.</span><span class="sxs-lookup"><span data-stu-id="64466-193">For example, we do not specify the `ResponseType` in the sample, so the default value `code id_token` will be used in each outgoing request to Azure AD B2C.</span></span>

<span data-ttu-id="64466-194">Szükség cookie-hitelesítés beállítása.</span><span class="sxs-lookup"><span data-stu-id="64466-194">You also need to set up cookie authentication.</span></span> <span data-ttu-id="64466-195">Az OpenID Connect köztes cookie-kat használ a felhasználói munkameneteket, többek között.</span><span class="sxs-lookup"><span data-stu-id="64466-195">The OpenID Connect middleware uses cookies to maintain user sessions, among other things.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure the OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate the metadata address using the tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify the callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify the claims to validate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement the "Notification" methods...
}
```

<span data-ttu-id="64466-196">A `OpenIdConnectAuthenticationOptions` újabb meghatároztuk visszahívási funkciók az OpenID Connect köztes által kapott az adott értesítésekhez.</span><span class="sxs-lookup"><span data-stu-id="64466-196">In `OpenIdConnectAuthenticationOptions` above, we define a set of callback functions for specific notifications that are received by the OpenID Connect middleware.</span></span> <span data-ttu-id="64466-197">Ezek közül a viselkedésmódok meghatározása egy `OpenIdConnectAuthenticationNotifications` objektumra, és tárolja azokat a `Notifications` változó.</span><span class="sxs-lookup"><span data-stu-id="64466-197">These behaviors are defined using a `OpenIdConnectAuthenticationNotifications` object and stored into the `Notifications` variable.</span></span> <span data-ttu-id="64466-198">A minta a három különböző visszahívások attól függően, hogy az esemény azt meg.</span><span class="sxs-lookup"><span data-stu-id="64466-198">In our sample, we define three different callbacks depending on the event.</span></span>

### <a name="using-different-policies"></a><span data-ttu-id="64466-199">Eltérő házirendek használatával</span><span class="sxs-lookup"><span data-stu-id="64466-199">Using different policies</span></span>

<span data-ttu-id="64466-200">A `RedirectToIdentityProvider` értesítési aktiválódik, amikor egy kérelem Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="64466-200">The `RedirectToIdentityProvider` notification is triggered whenever a request is made to Azure AD B2C.</span></span> <span data-ttu-id="64466-201">A visszahívási függvény `OnRedirectToIdentityProvider`, rendszer ellenőrzi a kimenő hívásban, ha azt szeretné, hogy egy másik házirend.</span><span class="sxs-lookup"><span data-stu-id="64466-201">In the callback function `OnRedirectToIdentityProvider`, we check in the outgoing call if we want to use a different policy.</span></span> <span data-ttu-id="64466-202">Ahhoz, hogy ne a jelszó alaphelyzetbe állítása, vagy szerkesztheti a profilját, például a jelszó-visszaállítási házirend az alapértelmezett "Regisztráció vagy bejelentkezés" házirend helyett használja a megfelelő házirendet kell.</span><span class="sxs-lookup"><span data-stu-id="64466-202">In order to do a password reset or edit a profile, you need to use the corresponding policy such as the password reset policy instead of the default "Sign-up or Sign-in" policy.</span></span>

<span data-ttu-id="64466-203">A mintában, amikor a felhasználó szeretné a jelszó alaphelyzetbe állítása, vagy szerkesztheti a profilját, azt a házirend hozzáadása azt előnyben részesítik a OWIN környezetben való használatát.</span><span class="sxs-lookup"><span data-stu-id="64466-203">In our sample, when a user wants to reset the password or edit the profile, we add the policy we prefer to use into the OWIN context.</span></span> <span data-ttu-id="64466-204">Amely a következő módon hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="64466-204">That can be done by doing the following:</span></span>

```CSharp
    // Let the middleware know you are trying to use the edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

<span data-ttu-id="64466-205">A visszahívási függvény is létrehozható és `OnRedirectToIdentityProvider` a következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="64466-205">And you can implement the callback function `OnRedirectToIdentityProvider` by doing the following:</span></span>

```CSharp
/*
*  On each call to Azure AD B2C, check if a policy (e.g. the profile edit or password reset policy) has been specified in the OWIN context.
*  If so, use that policy when making the call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a><span data-ttu-id="64466-206">Hitelesítési kódok kezelése</span><span class="sxs-lookup"><span data-stu-id="64466-206">Handling authorization codes</span></span>

<span data-ttu-id="64466-207">A `AuthorizationCodeReceived` értesítési lesz kiváltva, ha az engedélyezési kód érkezik.</span><span class="sxs-lookup"><span data-stu-id="64466-207">The `AuthorizationCodeReceived` notification is triggered when an authorization code is received.</span></span> <span data-ttu-id="64466-208">Az OpenID Connect köztes nem támogatja a hozzáférési jogkivonatok cserélő kódokat.</span><span class="sxs-lookup"><span data-stu-id="64466-208">The OpenID Connect middleware does not support exchanging codes for access tokens.</span></span> <span data-ttu-id="64466-209">Manuálisan továbbíthatja a jogkivonat a visszahívási függvény a kódját.</span><span class="sxs-lookup"><span data-stu-id="64466-209">You can manually exchange the code for the token in a callback function.</span></span> <span data-ttu-id="64466-210">További információkért tekintse meg a [dokumentáció](active-directory-b2c-devquickstarts-web-api-dotnet.md) , amely ismerteti, hogyan.</span><span class="sxs-lookup"><span data-stu-id="64466-210">For more information, please look at the [documentation](active-directory-b2c-devquickstarts-web-api-dotnet.md) that explains how.</span></span>

### <a name="handling-errors"></a><span data-ttu-id="64466-211">Hibák kezelése</span><span class="sxs-lookup"><span data-stu-id="64466-211">Handling errors</span></span>

<span data-ttu-id="64466-212">A `AuthenticationFailed` értesítési lesz kiváltva, ha a hitelesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="64466-212">The `AuthenticationFailed` notification is triggered when authentication fails.</span></span> <span data-ttu-id="64466-213">A visszahívási metódus képes kezelni a hibákat, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="64466-213">In its callback method, you can handle the errors as you wish.</span></span> <span data-ttu-id="64466-214">Azonban hozzá kell adnia egy ellenőrizze a hiba kódja `AADB2C90118`.</span><span class="sxs-lookup"><span data-stu-id="64466-214">You should however add a check for the error code `AADB2C90118`.</span></span> <span data-ttu-id="64466-215">A "Regisztráció vagy bejelentkezés" házirend végrehajtása közben a felhasználó rendelkezik-e választhat egy **elfelejtette a jelszavát?** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="64466-215">During the execution of the "Sign-up or Sign-in" policy, the user has the opportunity to select a **Forgot your password?** link.</span></span> <span data-ttu-id="64466-216">Ebben az esetben az Azure AD B2C elküldi az alkalmazás adott hiba mely azt jelzi, hogy az alkalmazás egy kérelmet, használja helyette: a jelszó-visszaállítási házirend győződjön.</span><span class="sxs-lookup"><span data-stu-id="64466-216">In this event, Azure AD B2C sends your app that error code indicating that your app should make a request using the password reset policy instead.</span></span>

```CSharp
/*
* Catch any failures received by the authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle the error code that Azure AD B2C throws when trying to reset a password from the login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-to-azure-ad"></a><span data-ttu-id="64466-217">Az Azure AD hitelesítési kérések küldése</span><span class="sxs-lookup"><span data-stu-id="64466-217">Send authentication requests to Azure AD</span></span>

<span data-ttu-id="64466-218">Az alkalmazás megfelelően konfigurálva van az Azure AD B2C az OpenID Connect hitelesítési protokoll használatával kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="64466-218">Your app is now properly configured to communicate with Azure AD B2C by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="64466-219">OWIN kezeli a hitelesítési üzenetek létrehozásával, az ellenőrzése az Azure AD B2C származó jogkivonatokat és a felhasználói munkamenet megtartásával részleteit.</span><span class="sxs-lookup"><span data-stu-id="64466-219">OWIN manages the details of crafting authentication messages, validating tokens from Azure AD B2C, and maintaining user session.</span></span> <span data-ttu-id="64466-220">Most már minden felhasználói folyamat kezdeményezéséhez.</span><span class="sxs-lookup"><span data-stu-id="64466-220">All that remains is to initiate each user's flow.</span></span>

<span data-ttu-id="64466-221">Amikor a felhasználó megadja **Sign up/bejelentkezési**, **profilszerkesztés**, vagy **jelszó-átállítási** web app alkalmazásban a társított művelet indították el a `Controllers\AccountController.cs`:</span><span class="sxs-lookup"><span data-stu-id="64466-221">When a user selects **Sign up/Sign in**, **Edit profile**, or **Reset password** in the web app, the associated action is invoked in `Controllers\AccountController.cs`:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign up or sign in
*/
public void SignUpSignIn()
{
    // Use the default policy to process the sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting to edit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let the middleware know you are trying to use the edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set the page to redirect to after editing the profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting to reset a password
*/
public void ResetPassword()
{
    // Let the middleware know you are trying to use the reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set the page to redirect to after changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

<span data-ttu-id="64466-222">OWIN segítségével jelentkezzen ki a felhasználó az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="64466-222">You can also use OWIN to sign out the user from the app.</span></span> <span data-ttu-id="64466-223">A `Controllers\AccountController.cs` vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="64466-223">In `Controllers\AccountController.cs` we have:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign out
*/
public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

<span data-ttu-id="64466-224">Explicit módon hívja meg a házirend, akkor is használhatnak a `[Authorize]` a tartományvezérlőket-címke, amely végrehajtja a házirend, ha a felhasználó nem jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="64466-224">In addition to explicitly invoking a policy, you can use a `[Authorize]` tag in your controllers that executes a policy if the user is not signed in.</span></span> <span data-ttu-id="64466-225">Nyissa meg `Controllers\HomeController.cs` , és adja hozzá a `[Authorize]` címkén belül, hogy a jogcímek vezérlő.</span><span class="sxs-lookup"><span data-stu-id="64466-225">Open `Controllers\HomeController.cs` and add the `[Authorize]` tag to the claims controller.</span></span>  <span data-ttu-id="64466-226">Az utolsó házirend állították be, mikor OWIN választja ki a `[Authorize]` címke talált.</span><span class="sxs-lookup"><span data-stu-id="64466-226">OWIN selects the last policy configured when the `[Authorize]` tag is hit.</span></span>

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a><span data-ttu-id="64466-227">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="64466-227">Display user information</span></span>

<span data-ttu-id="64466-228">Amikor a felhasználók hitelesítésére OpenID Connect használatával, ha az Azure AD B2C azonosítója jogkivonatot ad vissza, amely tartalmazza az alkalmazás **jogcímek**.</span><span class="sxs-lookup"><span data-stu-id="64466-228">When you authenticate users by using OpenID Connect, Azure AD B2C returns an ID token to the app that contains **claims**.</span></span> <span data-ttu-id="64466-229">Ezek a helyességi feltételek a felhasználóról.</span><span class="sxs-lookup"><span data-stu-id="64466-229">These are assertions about the user.</span></span> <span data-ttu-id="64466-230">Az alkalmazás személyre szabása jogcímek használata.</span><span class="sxs-lookup"><span data-stu-id="64466-230">You can use claims to personalize your app.</span></span>

<span data-ttu-id="64466-231">Nyissa meg az `Controllers\HomeController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="64466-231">Open the `Controllers\HomeController.cs` file.</span></span> <span data-ttu-id="64466-232">Felhasználói jogcímeket, a vezérlők keresztül érheti el a `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.</span><span class="sxs-lookup"><span data-stu-id="64466-232">You can access user claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

<span data-ttu-id="64466-233">Bármilyen jogcímet, amely az alkalmazás fogad ugyanúgy érheti el.</span><span class="sxs-lookup"><span data-stu-id="64466-233">You can access any claim that your application receives in the same way.</span></span>  <span data-ttu-id="64466-234">Az alkalmazás kap minden jogcím listája érhető el, a a **jogcímek** lap.</span><span class="sxs-lookup"><span data-stu-id="64466-234">A list of all the claims the app receives is available for you on the **Claims** page.</span></span>
