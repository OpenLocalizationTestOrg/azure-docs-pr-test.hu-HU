---
title: Active Directory B2C aaaAzure |} Microsoft Docs
description: "Hogyan toobuild rendelkező sign-Close-Up/sign-in webes alkalmazás profilját, szerkesztése és a jelszó alaphelyzetbe állítása az Azure Active Directory B2C használatával."
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
ms.openlocfilehash: 187f99a8dd50d212de4f0517f552cdbbe5a8edf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a><span data-ttu-id="d94b5-103">Egy ASP.NET-webalkalmazás létrehozása az Azure Active Directory B2C regisztráció, bejelentkezés, profil szerkesztése és a jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="d94b5-103">Create an ASP.NET web app with Azure Active Directory B2C sign-up, sign-in, profile edit, and password reset</span></span>

<span data-ttu-id="d94b5-104">Ez az oktatóanyag a következőket mutatja be:</span><span class="sxs-lookup"><span data-stu-id="d94b5-104">This tutorial shows you how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d94b5-105">Az Azure AD B2C identitáskezelési funkciókat tooyour webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d94b5-105">Add Azure AD B2C identity features tooyour web app</span></span>
> * <span data-ttu-id="d94b5-106">A webes alkalmazás regisztrálása az Azure AD B2C-címtárban</span><span class="sxs-lookup"><span data-stu-id="d94b5-106">Register your web app in your Azure AD B2C directory</span></span>
> * <span data-ttu-id="d94b5-107">Egy felhasználó sign-Close-Up/sign-in, a profil szerkesztése és a jelszó-visszaállítási házirend a webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d94b5-107">Create a user sign-up/sign-in, profile edit, and password reset policy for your web app</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d94b5-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d94b5-108">Prerequisites</span></span>

- <span data-ttu-id="d94b5-109">A B2C-bérlő tooan Azure-fiókot kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="d94b5-109">You must connect your B2C Tenant tooan Azure account.</span></span> <span data-ttu-id="d94b5-110">Létrehozhat egy ingyenes Azure-fiók [Itt](https://azure.microsoft.com/en-us/).</span><span class="sxs-lookup"><span data-stu-id="d94b5-110">You can create a free Azure account [here](https://azure.microsoft.com/en-us/).</span></span>
- <span data-ttu-id="d94b5-111">Szüksége [Microsoft Visual Studio](https://www.visualstudio.com/) vagy egy hasonló tooview programot, és módosítsa a hello mintakód.</span><span class="sxs-lookup"><span data-stu-id="d94b5-111">You need [Microsoft Visual Studio](https://www.visualstudio.com/) or a similar program tooview and modify hello sample code.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="d94b5-112">Azure AD B2C címtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="d94b5-112">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="d94b5-113">Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="d94b5-113">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="d94b5-114">A könyvtár egy olyan tároló, a felhasználók, alkalmazások, csoportok és több.</span><span class="sxs-lookup"><span data-stu-id="d94b5-114">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="d94b5-115">Ha egy már nem rendelkezik, ez az útmutató a folytatás előtt hozzon létre egy B2C-címtárat.</span><span class="sxs-lookup"><span data-stu-id="d94b5-115">If you don't have one already, create a B2C directory before you continue in this guide.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> <span data-ttu-id="d94b5-116">Tooconnect hello B2C-bérlő tooyour Azure-előfizetésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d94b5-116">You need tooconnect hello B2C Tenant tooyour Azure subscription.</span></span> <span data-ttu-id="d94b5-117">Kiválasztása után **létrehozása**, jelölje be hello **kapcsolat egy meglévő Azure AD B2C bérlő toomy Azure-előfizetés** lehetőséget, majd a hello **Azure AD B2C bérlő** legördülő listán, válassza ki azt szeretné, hogy tooassociate hello bérlő.</span><span class="sxs-lookup"><span data-stu-id="d94b5-117">After selecting **Create**, select hello **Link an existing Azure AD B2C Tenant toomy Azure subscription** option, and then in hello **Azure AD B2C Tenant** drop down, select hello tenant you want tooassociate.</span></span>

## <a name="create-and-register-an-application"></a><span data-ttu-id="d94b5-118">Hozzon létre, és egy alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d94b5-118">Create and register an application</span></span>

<span data-ttu-id="d94b5-119">A következő toocreate kell, és regisztrálja a hello alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="d94b5-119">Next, you need toocreate and register hello app in your B2C directory.</span></span> <span data-ttu-id="d94b5-120">Ez biztosítja, hogy az Azure AD B2C szükséges toosecurely információkat az alkalmazás kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="d94b5-120">This provides information that Azure AD B2C needs toosecurely communicate with your app.</span></span> 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

<span data-ttu-id="d94b5-121">Amikor elkészült, hogy az API-k és a natív alkalmazás az alkalmazás beállításaiban.</span><span class="sxs-lookup"><span data-stu-id="d94b5-121">When you are done, you will have both an API and a native application in your application settings.</span></span>

## <a name="create-policies-on-your-b2c-tenant"></a><span data-ttu-id="d94b5-122">A B2C-bérlő a házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d94b5-122">Create policies on your B2C tenant</span></span>

<span data-ttu-id="d94b5-123">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="d94b5-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="d94b5-124">Ebben a kódmintában három identitásélményt tartalmaz: **előfizetési & bejelentkezési**, **profil szerkesztése**, és **jelszó-változtatási**.</span><span class="sxs-lookup"><span data-stu-id="d94b5-124">This code sample contains three identity experiences: **sign-up & sign-in**, **profile edit**, and **password reset**.</span></span>  <span data-ttu-id="d94b5-125">Szüksége van a toocreate egy házirendet az egyes típusok hello leírtak [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="d94b5-125">You need toocreate one policy of each type, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="d94b5-126">Minden házirend esetében meg arról, hogy tooselect hello megjelenítési név attribútum vagy a jogcím és a toocopy hello nevet a házirend a későbbi felhasználásra vonatkozó lennie.</span><span class="sxs-lookup"><span data-stu-id="d94b5-126">For each policy, be sure tooselect hello Display name attribute or claim, and toocopy down hello name of your policy for later use.</span></span>

### <a name="add-your-identity-providers"></a><span data-ttu-id="d94b5-127">Az identitás-szolgáltatóktól hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d94b5-127">Add your identity providers</span></span>

<span data-ttu-id="d94b5-128">Válassza ki a beállítások **identitás-szolgáltatóktól** , és válassza a felhasználónév előfizetési vagy e-mailt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d94b5-128">From your settings, select **Identity Providers** and choose Username signup or Email signup.</span></span>

### <a name="create-a-sign-up-and-sign-in-policy"></a><span data-ttu-id="d94b5-129">Regisztráció és bejelentkezés házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="d94b5-129">Create a sign-up and sign-in policy</span></span>

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a><span data-ttu-id="d94b5-130">A Profilszerkesztési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="d94b5-130">Create a profile editing policy</span></span>

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a><span data-ttu-id="d94b5-131">A jelszó-visszaállítási házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="d94b5-131">Create a password reset policy</span></span>

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

<span data-ttu-id="d94b5-132">A házirendek létrehozása után készen áll a toobuild Ön az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d94b5-132">After you create your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="d94b5-133">Hello mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="d94b5-133">Download hello sample code</span></span>

<span data-ttu-id="d94b5-134">az oktatóanyag kódjának hello fenntartott [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="d94b5-134">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="d94b5-135">Hello minta klónozhat futtatásával:</span><span class="sxs-lookup"><span data-stu-id="d94b5-135">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="d94b5-136">Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="d94b5-136">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="d94b5-137">hello megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="d94b5-137">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="d94b5-138">`TaskWebApp`hello felhasználói hello MVC webalkalmazás kommunikál.</span><span class="sxs-lookup"><span data-stu-id="d94b5-138">`TaskWebApp` is hello MVC web application that hello user interacts with.</span></span> <span data-ttu-id="d94b5-139">`TaskService`minden felhasználói feladatlistát tároló hello app webes API van.</span><span class="sxs-lookup"><span data-stu-id="d94b5-139">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="d94b5-140">Ez a cikk csak ismertetik hello `TaskWebApp` alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d94b5-140">This article will only discuss hello `TaskWebApp` application.</span></span> <span data-ttu-id="d94b5-141">toolearn hogyan toobuild `TaskService` Azure AD B2C segítségével, lásd: [a .NET webes api-oktatóanyag](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="d94b5-141">toolearn how toobuild `TaskService` using Azure AD B2C, see [our .NET web api tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="update-code-toouse-your-tenant-and-policies"></a><span data-ttu-id="d94b5-142">A bérlői és a házirendek kód toouse frissítése</span><span class="sxs-lookup"><span data-stu-id="d94b5-142">Update code toouse your tenant and policies</span></span>

<span data-ttu-id="d94b5-143">A minta rendszer konfigurált toouse hello házirendek és az ügyfél-Azonosítóját a bemutató bérlő.</span><span class="sxs-lookup"><span data-stu-id="d94b5-143">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="d94b5-144">tooconnect azt tooyour saját bérlőt, tooopen kell `web.config` a hello `TaskWebApp` projektre, és cserélje le a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="d94b5-144">tooconnect it tooyour own tenant, you need tooopen `web.config` in hello `TaskWebApp` project and replace hello following values:</span></span>

* <span data-ttu-id="d94b5-145">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="d94b5-145">`ida:Tenant` with your tenant name</span></span>
* <span data-ttu-id="d94b5-146">Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója</span><span class="sxs-lookup"><span data-stu-id="d94b5-146">`ida:ClientId` with your web app application ID</span></span>
* <span data-ttu-id="d94b5-147">Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa</span><span class="sxs-lookup"><span data-stu-id="d94b5-147">`ida:ClientSecret` with your web app secret key</span></span>
* <span data-ttu-id="d94b5-148">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="d94b5-148">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
* <span data-ttu-id="d94b5-149">Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve</span><span class="sxs-lookup"><span data-stu-id="d94b5-149">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
* <span data-ttu-id="d94b5-150">Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve</span><span class="sxs-lookup"><span data-stu-id="d94b5-150">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>

## <a name="launch-hello-app"></a><span data-ttu-id="d94b5-151">Hello alkalmazás indítása</span><span class="sxs-lookup"><span data-stu-id="d94b5-151">Launch hello app</span></span>
<span data-ttu-id="d94b5-152">A Visual studióban, indítsa el a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d94b5-152">From within Visual Studio, launch hello app.</span></span> <span data-ttu-id="d94b5-153">Keresse meg a toohello feladatlista fülre, majd a Megjegyzés hello URL-címe: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...</span><span class="sxs-lookup"><span data-stu-id="d94b5-153">Navigate toohello To-Do List tab, and note hello URl is: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*&client_id=*YourclientID*.....</span></span>

<span data-ttu-id="d94b5-154">Iratkozzon fel az e-mail cím vagy a felhasználó neve hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d94b5-154">Sign up for hello app by using your email address or user name.</span></span> <span data-ttu-id="d94b5-155">Jelentkezzen ki, majd jelentkezzen be újból hello-profil szerkesztése vagy hello jelszó alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="d94b5-155">Sign out, then sign in again and edit hello profile or reset hello password.</span></span> <span data-ttu-id="d94b5-156">Kijelentkezés és bejelentkezés másik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="d94b5-156">Sign out and sign in as a different user.</span></span> 

## <a name="add-social-idps"></a><span data-ttu-id="d94b5-157">Közösségi IDPs hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d94b5-157">Add social IDPs</span></span>

<span data-ttu-id="d94b5-158">Jelenleg hello alkalmazás támogatja a csak a regisztráció és bejelentkezés felhasználói **helyi fiókok**; fiókok a B2C-címtárban tárolja, amely a felhasználónév és jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="d94b5-158">Currently, hello app supports only user sign-up and sign-in by using **local accounts**; accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="d94b5-159">Az Azure AD B2C segítségével támogathatja más **identitás-szolgáltatóktól** (IDPs) bármely, a kód módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d94b5-159">By using Azure AD B2C, you can add support for other **identity providers** (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="d94b5-160">tooadd közösségi IDPs tooyour alkalmazást, és kezdje az alábbi hello részletes utasításokat a cikkeiben.</span><span class="sxs-lookup"><span data-stu-id="d94b5-160">tooadd social IDPs tooyour app, begin by following hello detailed instructions in these articles.</span></span> <span data-ttu-id="d94b5-161">Az egyes IDP meg toosupport, egy alkalmazás tooregister kell, hogy a rendszer, és szerezzen be egy ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d94b5-161">For each IDP you want toosupport, you need tooregister an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="d94b5-162">Az IDP Facebook beállítása</span><span class="sxs-lookup"><span data-stu-id="d94b5-162">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="d94b5-163">Az IDP Google beállítása</span><span class="sxs-lookup"><span data-stu-id="d94b5-163">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="d94b5-164">Az IDP Amazon beállítása</span><span class="sxs-lookup"><span data-stu-id="d94b5-164">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="d94b5-165">Az IDP LinkedIn beállítása</span><span class="sxs-lookup"><span data-stu-id="d94b5-165">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="d94b5-166">Miután hozzáadta a hello identity providers tooyour B2C-címtárban, Szerkesztés minden, a három házirend tooinclude hello új IDPs hello leírtak [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="d94b5-166">After you add hello identity providers tooyour B2C directory, edit each of your three policies tooinclude hello new IDPs, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="d94b5-167">A házirend mentése után futtassa újra a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d94b5-167">After you save your policies, run hello app again.</span></span>  <span data-ttu-id="d94b5-168">Új IDPs fel, mert a bejelentkezési és regisztrációs beállítások a identitással kapcsolatos műveletet mindegyikében hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="d94b5-168">You should see hello new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="d94b5-169">A házirendek kísérletezhet, és figyelje meg a mintaalkalmazás hello hatással.</span><span class="sxs-lookup"><span data-stu-id="d94b5-169">You can experiment with your policies and observe hello effect on your sample app.</span></span> <span data-ttu-id="d94b5-170">Vegye fel vagy távolítsa el a IDPs, kezelheti az alkalmazás jogcímét, vagy módosítsa a regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="d94b5-170">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="d94b5-171">Kísérletet, amíg meg nem látja hogyan házirendeket, a hitelesítési kérelmek és OWIN alkalmazássá.</span><span class="sxs-lookup"><span data-stu-id="d94b5-171">Experiment until you can see how policies, authentication requests, and OWIN tie together.</span></span>

## <a name="sample-code-walkthrough"></a><span data-ttu-id="d94b5-172">A minta kód forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d94b5-172">Sample code walkthrough</span></span>
<span data-ttu-id="d94b5-173">hello a következő szakaszok bemutatják, hogyan hello mintakód alkalmazás van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="d94b5-173">hello following sections show you how hello sample application code is configured.</span></span> <span data-ttu-id="d94b5-174">Akkor lehet, hogy ezzel útmutatóként a jövőbeli fejlesztés.</span><span class="sxs-lookup"><span data-stu-id="d94b5-174">You may use this as a guide in your future app development.</span></span>

### <a name="add-authentication-support"></a><span data-ttu-id="d94b5-175">Vegye fel a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="d94b5-175">Add authentication support</span></span>

<span data-ttu-id="d94b5-176">Most már konfigurálhatja az alkalmazás toouse az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d94b5-176">You can now configure your app toouse Azure AD B2C.</span></span> <span data-ttu-id="d94b5-177">Az alkalmazást úgy, hogy az OpenID Connect hitelesítési kérelmeket küld az Azure AD B2C kommunikál.</span><span class="sxs-lookup"><span data-stu-id="d94b5-177">Your app communicates with Azure AD B2C by sending OpenID Connect authentication requests.</span></span> <span data-ttu-id="d94b5-178">hello kérelmek előírják hello felhasználói élményt az alkalmazás tooexecute szeretne hello házirend.</span><span class="sxs-lookup"><span data-stu-id="d94b5-178">hello requests dictate hello user experience your app wants tooexecute by specifying hello policy.</span></span> <span data-ttu-id="d94b5-179">Használja a Microsoft OWIN könyvtár toosend ezeket a kérelmeket, házirendek hajtható végre, kezelheti a felhasználói munkamenetek és még sok más.</span><span class="sxs-lookup"><span data-stu-id="d94b5-179">You can use Microsoft's OWIN library toosend these requests, execute policies, manage user sessions, and more.</span></span>

#### <a name="install-owin"></a><span data-ttu-id="d94b5-180">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="d94b5-180">Install OWIN</span></span>

<span data-ttu-id="d94b5-181">toobegin, hello OWIN köztes NuGet csomagjainak toohello projekt hozzáadása hello Visual Studio Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="d94b5-181">toobegin, add hello OWIN middleware NuGet packages toohello project by using hello Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a><span data-ttu-id="d94b5-182">OWIN indítási osztály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d94b5-182">Add an OWIN startup class</span></span>

<span data-ttu-id="d94b5-183">Adja hozzá egy OWIN indítási osztály toohello API hívása `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="d94b5-183">Add an OWIN startup class toohello API called `Startup.cs`.</span></span>  <span data-ttu-id="d94b5-184">Kattintson a jobb gombbal hello projektre, válassza a **Hozzáadás** és **új elem**, majd keresse meg az owin ELEMET.</span><span class="sxs-lookup"><span data-stu-id="d94b5-184">Right-click on hello project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="d94b5-185">hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="d94b5-185">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="d94b5-186">A mintában szereplő változtattuk hello osztálydeklaráció túl`public partial class Startup` és megvalósított hello osztály a többi részét hello `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="d94b5-186">In our sample, we changed hello class declaration too`public partial class Startup` and implemented hello other part of hello class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="d94b5-187">Belső hello `Configuration` módszer, hívása túl hozzáadott`ConfigureAuth`, amelyhez definiálva van `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="d94b5-187">Inside hello `Configuration` method, we added a call too`ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="d94b5-188">Hello módosítások után `Startup.cs` tűnik hello következő:</span><span class="sxs-lookup"><span data-stu-id="d94b5-188">After hello modifications, `Startup.cs` looks like hello following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-hello-authentication-middleware"></a><span data-ttu-id="d94b5-189">Hello hitelesítési köztes konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d94b5-189">Configure hello authentication middleware</span></span>

<span data-ttu-id="d94b5-190">Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="d94b5-190">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="d94b5-191">Megadja a paraméterek hello `OpenIdConnectAuthenticationOptions` szolgáljanak, az Azure AD B2C app toocommunicate koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="d94b5-191">hello parameters you provide in `OpenIdConnectAuthenticationOptions` serve as coordinates for your app toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="d94b5-192">Ha nem adja meg a paraméterek, hello alapértelmezett értéket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d94b5-192">If you do not specify certain parameters, it will use hello default value.</span></span> <span data-ttu-id="d94b5-193">Például, hogy ne adjon meg hello `ResponseType` hello mintában, így hello az alapértelmezett érték `code id_token` minden kimenő kérelem tooAzure AD B2C használni fogják.</span><span class="sxs-lookup"><span data-stu-id="d94b5-193">For example, we do not specify hello `ResponseType` in hello sample, so hello default value `code id_token` will be used in each outgoing request tooAzure AD B2C.</span></span>

<span data-ttu-id="d94b5-194">Meg kell készíteni, hitelesítési cookie-k tooset is.</span><span class="sxs-lookup"><span data-stu-id="d94b5-194">You also need tooset up cookie authentication.</span></span> <span data-ttu-id="d94b5-195">hello OpenID Connect köztes használ cookie-k toomaintain felhasználói munkamenetek, többek között.</span><span class="sxs-lookup"><span data-stu-id="d94b5-195">hello OpenID Connect middleware uses cookies toomaintain user sessions, among other things.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure hello OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate hello metadata address using hello tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify hello callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify hello claims toovalidate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement hello "Notification" methods...
}
```

<span data-ttu-id="d94b5-196">A `OpenIdConnectAuthenticationOptions` újabb meghatároztuk visszahívási funkciók hello OpenID Connect köztes által kapott az adott értesítésekhez.</span><span class="sxs-lookup"><span data-stu-id="d94b5-196">In `OpenIdConnectAuthenticationOptions` above, we define a set of callback functions for specific notifications that are received by hello OpenID Connect middleware.</span></span> <span data-ttu-id="d94b5-197">Ezek közül a viselkedésmódok meghatározása egy `OpenIdConnectAuthenticationNotifications` objektumra, és hello eltárolni `Notifications` változó.</span><span class="sxs-lookup"><span data-stu-id="d94b5-197">These behaviors are defined using a `OpenIdConnectAuthenticationNotifications` object and stored into hello `Notifications` variable.</span></span> <span data-ttu-id="d94b5-198">A mintában szereplő meghatároztuk három különböző visszahívások attól függően, hogy hello esemény.</span><span class="sxs-lookup"><span data-stu-id="d94b5-198">In our sample, we define three different callbacks depending on hello event.</span></span>

### <a name="using-different-policies"></a><span data-ttu-id="d94b5-199">Eltérő házirendek használatával</span><span class="sxs-lookup"><span data-stu-id="d94b5-199">Using different policies</span></span>

<span data-ttu-id="d94b5-200">Hello `RedirectToIdentityProvider` értesítési aktiválódik, amikor a kérelem tooAzure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d94b5-200">hello `RedirectToIdentityProvider` notification is triggered whenever a request is made tooAzure AD B2C.</span></span> <span data-ttu-id="d94b5-201">A visszahívási függvény hello `OnRedirectToIdentityProvider`, ellenőrizzük a kimenő hívás, ha azt szeretné, hogy toouse hello másik szabályt.</span><span class="sxs-lookup"><span data-stu-id="d94b5-201">In hello callback function `OnRedirectToIdentityProvider`, we check in hello outgoing call if we want toouse a different policy.</span></span> <span data-ttu-id="d94b5-202">Rendelés toodo jelszó alaphelyzetbe állítása, vagy szerkesztheti a profilját kell toouse hello megfelelő házirendet például hello jelszó-visszaállítási házirend hello alapértelmezett "Regisztráció vagy bejelentkezés" házirend helyett.</span><span class="sxs-lookup"><span data-stu-id="d94b5-202">In order toodo a password reset or edit a profile, you need toouse hello corresponding policy such as hello password reset policy instead of hello default "Sign-up or Sign-in" policy.</span></span>

<span data-ttu-id="d94b5-203">A mintában, amikor a felhasználó szeretne tooreset hello jelszó vagy hello-profil szerkesztése azt hello házirend hozzáadása azt toouse inkább hello OWIN összefüggésben.</span><span class="sxs-lookup"><span data-stu-id="d94b5-203">In our sample, when a user wants tooreset hello password or edit hello profile, we add hello policy we prefer toouse into hello OWIN context.</span></span> <span data-ttu-id="d94b5-204">Amely végezhető hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="d94b5-204">That can be done by doing hello following:</span></span>

```CSharp
    // Let hello middleware know you are trying toouse hello edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

<span data-ttu-id="d94b5-205">Megvalósíthat hello visszahívási függvény és `OnRedirectToIdentityProvider` hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="d94b5-205">And you can implement hello callback function `OnRedirectToIdentityProvider` by doing hello following:</span></span>

```CSharp
/*
*  On each call tooAzure AD B2C, check if a policy (e.g. hello profile edit or password reset policy) has been specified in hello OWIN context.
*  If so, use that policy when making hello call. Also, don't request a code (since it won't be needed).
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

### <a name="handling-authorization-codes"></a><span data-ttu-id="d94b5-206">Hitelesítési kódok kezelése</span><span class="sxs-lookup"><span data-stu-id="d94b5-206">Handling authorization codes</span></span>

<span data-ttu-id="d94b5-207">Hello `AuthorizationCodeReceived` értesítési lesz kiváltva, ha az engedélyezési kód érkezik.</span><span class="sxs-lookup"><span data-stu-id="d94b5-207">hello `AuthorizationCodeReceived` notification is triggered when an authorization code is received.</span></span> <span data-ttu-id="d94b5-208">hello OpenID Connect köztes nem támogatja a hozzáférési jogkivonatok cserélő kódokat.</span><span class="sxs-lookup"><span data-stu-id="d94b5-208">hello OpenID Connect middleware does not support exchanging codes for access tokens.</span></span> <span data-ttu-id="d94b5-209">Manuálisan továbbíthatja az hello token a visszahívási függvény hello kódját.</span><span class="sxs-lookup"><span data-stu-id="d94b5-209">You can manually exchange hello code for hello token in a callback function.</span></span> <span data-ttu-id="d94b5-210">További információkért tekintse meg hello [dokumentáció](active-directory-b2c-devquickstarts-web-api-dotnet.md) , amely ismerteti, hogyan.</span><span class="sxs-lookup"><span data-stu-id="d94b5-210">For more information, please look at hello [documentation](active-directory-b2c-devquickstarts-web-api-dotnet.md) that explains how.</span></span>

### <a name="handling-errors"></a><span data-ttu-id="d94b5-211">Hibák kezelése</span><span class="sxs-lookup"><span data-stu-id="d94b5-211">Handling errors</span></span>

<span data-ttu-id="d94b5-212">Hello `AuthenticationFailed` értesítési lesz kiváltva, ha a hitelesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="d94b5-212">hello `AuthenticationFailed` notification is triggered when authentication fails.</span></span> <span data-ttu-id="d94b5-213">A visszahívási metódus hello hibák képes kezelni, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="d94b5-213">In its callback method, you can handle hello errors as you wish.</span></span> <span data-ttu-id="d94b5-214">Azonban hozzá kell adnia egy ellenőrzést hello hibakód `AADB2C90118`.</span><span class="sxs-lookup"><span data-stu-id="d94b5-214">You should however add a check for hello error code `AADB2C90118`.</span></span> <span data-ttu-id="d94b5-215">Hello végrehajtásakor hello "Regisztráció vagy bejelentkezés" házirend, hello felhasználó rendelkezik-e hello lehetőség tooselect egy **elfelejtette a jelszavát?** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d94b5-215">During hello execution of hello "Sign-up or Sign-in" policy, hello user has hello opportunity tooselect a **Forgot your password?** link.</span></span> <span data-ttu-id="d94b5-216">Ebben az esetben az Azure AD B2C elküldi az alkalmazás adott hiba mely azt jelzi, hogy az alkalmazás egy kérelmet, használja helyette: hello jelszó-visszaállítási házirend győződjön.</span><span class="sxs-lookup"><span data-stu-id="d94b5-216">In this event, Azure AD B2C sends your app that error code indicating that your app should make a request using hello password reset policy instead.</span></span>

```CSharp
/*
* Catch any failures received by hello authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle hello error code that Azure AD B2C throws when trying tooreset a password from hello login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If hello user clicked hello reset password link, redirect toohello reset password route
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

### <a name="send-authentication-requests-tooazure-ad"></a><span data-ttu-id="d94b5-217">Hitelesítési kérelmek tooAzure AD küldése</span><span class="sxs-lookup"><span data-stu-id="d94b5-217">Send authentication requests tooAzure AD</span></span>

<span data-ttu-id="d94b5-218">Az alkalmazás már Azure AD B2C megfelelően konfigurált toocommunicate hello OpenID Connect hitelesítési protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="d94b5-218">Your app is now properly configured toocommunicate with Azure AD B2C by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="d94b5-219">OWIN kezeli a hitelesítési üzenetek létrehozásával, az ellenőrzése az Azure AD B2C származó jogkivonatokat és a felhasználói munkamenet megtartásával hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="d94b5-219">OWIN manages hello details of crafting authentication messages, validating tokens from Azure AD B2C, and maintaining user session.</span></span> <span data-ttu-id="d94b5-220">Az marad tooinitiate van minden egyes felhasználói folyamat.</span><span class="sxs-lookup"><span data-stu-id="d94b5-220">All that remains is tooinitiate each user's flow.</span></span>

<span data-ttu-id="d94b5-221">Amikor a felhasználó megadja **Sign up/bejelentkezési**, **profilszerkesztés**, vagy **jelszó-átállítási** hello web app alkalmazásban társított hello művelet indították el a `Controllers\AccountController.cs`:</span><span class="sxs-lookup"><span data-stu-id="d94b5-221">When a user selects **Sign up/Sign in**, **Edit profile**, or **Reset password** in hello web app, hello associated action is invoked in `Controllers\AccountController.cs`:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign up or sign in
*/
public void SignUpSignIn()
{
    // Use hello default policy tooprocess hello sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting tooedit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let hello middleware know you are trying toouse hello edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set hello page tooredirect tooafter editing hello profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting tooreset a password
*/
public void ResetPassword()
{
    // Let hello middleware know you are trying toouse hello reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set hello page tooredirect tooafter changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

<span data-ttu-id="d94b5-222">Kimenő hello felhasználói hello alkalmazás OWIN toosign is használható.</span><span class="sxs-lookup"><span data-stu-id="d94b5-222">You can also use OWIN toosign out hello user from hello app.</span></span> <span data-ttu-id="d94b5-223">A `Controllers\AccountController.cs` vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="d94b5-223">In `Controllers\AccountController.cs` we have:</span></span>

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign out
*/
public void SignOut()
{
    // toosign out hello user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

<span data-ttu-id="d94b5-224">Továbbá tooexplicitly meghívása egy házirendet, használhat egy `[Authorize]` a tartományvezérlőket-címke, amely végrehajtja a házirendet, ha hello felhasználó nem jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="d94b5-224">In addition tooexplicitly invoking a policy, you can use a `[Authorize]` tag in your controllers that executes a policy if hello user is not signed in.</span></span> <span data-ttu-id="d94b5-225">Nyissa meg `Controllers\HomeController.cs` , és adja hozzá a hello `[Authorize]` címke toohello jogcím-vezérlő.</span><span class="sxs-lookup"><span data-stu-id="d94b5-225">Open `Controllers\HomeController.cs` and add hello `[Authorize]` tag toohello claims controller.</span></span>  <span data-ttu-id="d94b5-226">OWIN kiválasztja hello utolsó házirend konfigurálását, ha hello `[Authorize]` címke talált.</span><span class="sxs-lookup"><span data-stu-id="d94b5-226">OWIN selects hello last policy configured when hello `[Authorize]` tag is hit.</span></span>

```CSharp
// Controllers\HomeController.cs

// You can use hello Authorize decorator tooexecute a certain policy if hello user is not already signed into hello app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a><span data-ttu-id="d94b5-227">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="d94b5-227">Display user information</span></span>

<span data-ttu-id="d94b5-228">Amikor a felhasználók hitelesítésére OpenID Connect használatával, az Azure AD B2C adja vissza egy azonosító token toohello alkalmazást tartalmazó **jogcímek**.</span><span class="sxs-lookup"><span data-stu-id="d94b5-228">When you authenticate users by using OpenID Connect, Azure AD B2C returns an ID token toohello app that contains **claims**.</span></span> <span data-ttu-id="d94b5-229">Ezek a helyességi feltételek hello felhasználóról.</span><span class="sxs-lookup"><span data-stu-id="d94b5-229">These are assertions about hello user.</span></span> <span data-ttu-id="d94b5-230">Jogcímek toopersonalize használhatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d94b5-230">You can use claims toopersonalize your app.</span></span>

<span data-ttu-id="d94b5-231">Nyissa meg hello `Controllers\HomeController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d94b5-231">Open hello `Controllers\HomeController.cs` file.</span></span> <span data-ttu-id="d94b5-232">Felhasználói jogcímek érheti el, a vezérlők keresztül hello `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.</span><span class="sxs-lookup"><span data-stu-id="d94b5-232">You can access user claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

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

<span data-ttu-id="d94b5-233">Érheti el bármilyen jogcímet, hogy az alkalmazás fogadása hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="d94b5-233">You can access any claim that your application receives in hello same way.</span></span>  <span data-ttu-id="d94b5-234">Hello app kap minden hello jogcím listája megtalálható az Ön hello **jogcímek** lap.</span><span class="sxs-lookup"><span data-stu-id="d94b5-234">A list of all hello claims hello app receives is available for you on hello **Claims** page.</span></span>
