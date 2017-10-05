---
title: "Az Azure Active Directory v2.0 Android-alkalmazás |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre Android-alkalmazás, mely aláírja a felhasználók személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókok és a Graph API hívások harmadik féltől származó könyvtárak használatával."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: c0a5a818c61f7af7ff04bf890b54e8364f3b21b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a><span data-ttu-id="ac331-103">Bejelentkezés hozzáadása egy külső könyvtár használatával Graph API-t használ a v2.0-végpontra Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ac331-103">Add sign-in to an Android app using a third-party library with Graph API using the v2.0 endpoint</span></span>
<span data-ttu-id="ac331-104">A Microsoft identitásplatformja nyílt szabványokat, többek között OAuth2-t és OpenID Connectet használ.</span><span class="sxs-lookup"><span data-stu-id="ac331-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="ac331-105">A fejlesztők a függvénytárat, hogy integrálni szeretne a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="ac331-105">Developers can use any library they want to integrate with our services.</span></span> <span data-ttu-id="ac331-106">Segítségével a fejlesztők a platformot használja a többi könyvtárak, azt korábban írt bemutatják, hogyan lehet kapcsolódni a Microsoft identity platform külső szalagtárak konfigurálása a jelen szoftverhez hasonló néhány forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="ac331-106">To help developers use our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure third-party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="ac331-107">A legtöbb tárak, amelyek megvalósítják az [a RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) csatlakozni tud-e a Microsoft identity platform.</span><span class="sxs-lookup"><span data-stu-id="ac331-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect to the Microsoft identity platform.</span></span>

<span data-ttu-id="ac331-108">Ez a forgatókönyv hoz létre az alkalmazással felhasználók jelentkezzen be a szervezet és majd keresse meg a magukat a szervezetek a Graph API használatával.</span><span class="sxs-lookup"><span data-stu-id="ac331-108">With the application that this walkthrough creates, users can sign in to their organization and then search for themselves in their organization by using the Graph API.</span></span>

<span data-ttu-id="ac331-109">Ha most ismerkedik az OAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem célszerű Önnek.</span><span class="sxs-lookup"><span data-stu-id="ac331-109">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make sense to you.</span></span> <span data-ttu-id="ac331-110">Azt javasoljuk, hogy olvassa el [2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md) a háttérben.</span><span class="sxs-lookup"><span data-stu-id="ac331-110">We recommend that you read [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="ac331-111">Egyes szolgáltatások, amelyek rendelkeznek egy kifejezést a OAuth2 vagy az OpenID Connect szabványok, például a feltételes hozzáférés és a Csoportházirend kezelése Intune-ban, a platform kell használni a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú.</span><span class="sxs-lookup"><span data-stu-id="ac331-111">Some features of our platform that do have an expression in the OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you to use our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="ac331-112">A v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók.</span><span class="sxs-lookup"><span data-stu-id="ac331-112">The v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="ac331-113">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="ac331-113">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-the-code-from-github"></a><span data-ttu-id="ac331-114">Töltse le a kódot a Githubról</span><span class="sxs-lookup"><span data-stu-id="ac331-114">Download the code from GitHub</span></span>
<span data-ttu-id="ac331-115">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span><span class="sxs-lookup"><span data-stu-id="ac331-115">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span></span>  <span data-ttu-id="ac331-116">Követéséhez is [töltse le az alkalmazás vázát egy .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="ac331-116">To follow along, you can  [download the app's skeleton as a .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

<span data-ttu-id="ac331-117">A minta csak is tölthetik le, és rögtön használatba:</span><span class="sxs-lookup"><span data-stu-id="ac331-117">You can also just download the sample and get started right away:</span></span>

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="ac331-118">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="ac331-118">Register an app</span></span>
<span data-ttu-id="ac331-119">Hozzon létre egy új alkalmazást a [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy részletes kövesse a [egy alkalmazás regisztrálása a v2.0-végponttal](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="ac331-119">Create a new app at the [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow the detailed steps at [How to register an app with the v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="ac331-120">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="ac331-120">Make sure to:</span></span>

* <span data-ttu-id="ac331-121">Másolás a **alkalmazásazonosító** , amely hozzá van rendelve az alkalmazás mivel hamarosan lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="ac331-121">Copy the **Application Id** that's assigned to your app because you'll need it soon.</span></span>
* <span data-ttu-id="ac331-122">Adja hozzá a **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="ac331-122">Add the **Mobile** platform for your app.</span></span>

> <span data-ttu-id="ac331-123">Megjegyzés: Az alkalmazásregisztrációs portálra biztosít egy **átirányítási URI-** érték.</span><span class="sxs-lookup"><span data-stu-id="ac331-123">Note: The Application registration portal provides a **Redirect URI** value.</span></span> <span data-ttu-id="ac331-124">Azonban ez a példa kell használnia az alapértelmezett érték `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span><span class="sxs-lookup"><span data-stu-id="ac331-124">However, in this example you must use the default value of `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>
> 
> 

## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a><span data-ttu-id="ac331-125">Töltse le a NXOAuth2 külső könyvtárban, és egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="ac331-125">Download the NXOAuth2 third-party library and create a workspace</span></span>
<span data-ttu-id="ac331-126">A forgatókönyv a Githubból, amelyek alapján a Google OpenID Connect kódját OAuth2 könyvtár OIDCAndroidLib fogja használni.</span><span class="sxs-lookup"><span data-stu-id="ac331-126">For this walkthrough, you will use the OIDCAndroidLib from GitHub, which is an OAuth2 library based on the OpenID Connect code of Google.</span></span> <span data-ttu-id="ac331-127">A natív alkalmazásprofil valósítja meg, és a felhasználó az engedélyezési végpont támogatja.</span><span class="sxs-lookup"><span data-stu-id="ac331-127">It implements the native application profile and supports the authorization endpoint of the user.</span></span> <span data-ttu-id="ac331-128">Az alábbiakban az összes, a Microsoft identitásplatformmal együttműködve integrálni kell.</span><span class="sxs-lookup"><span data-stu-id="ac331-128">These are all the things that you'll need to integrate with the Microsoft identity platform.</span></span>

<span data-ttu-id="ac331-129">A számítógépre a OIDCAndroidLib tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="ac331-129">Clone the OIDCAndroidLib repo to your computer.</span></span>

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a><span data-ttu-id="ac331-131">Az Android Studio környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="ac331-131">Set up your Android Studio environment</span></span>
1. <span data-ttu-id="ac331-132">Hozzon létre egy új Android Studio-projektet, és fogadja el az alapértelmezett beállításokat a varázslóban.</span><span class="sxs-lookup"><span data-stu-id="ac331-132">Create a new Android Studio project and accept the defaults in the wizard.</span></span>
   
    ![Az Android Studio új projekt létrehozása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Cél Android-eszközök](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Adjon hozzá olyan tevékenységet Mobile](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. <span data-ttu-id="ac331-136">A projekt modulok beállításához helyezze át a klónozott tárház a projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="ac331-136">To set up your project modules, move the cloned repo to the project location.</span></span> <span data-ttu-id="ac331-137">A projekt is létrehozhat, és majd klónozza a projekt helyére a közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="ac331-137">You can also create the project and then clone it directly to the project location.</span></span>
   
    ![Projekt modulok](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. <span data-ttu-id="ac331-139">Nyissa meg a projekt modulok beállításait, a helyi menü használatával vagy a Ctrl + Alt + Maj + S parancsikon használatával.</span><span class="sxs-lookup"><span data-stu-id="ac331-139">Open the project modules settings by using the context menu or by using the Ctrl+Alt+Maj+S shortcut.</span></span>
   
    ![Modulok Projektbeállítások](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. <span data-ttu-id="ac331-141">Távolítsa el az alapértelmezett app modul, mivel csak a tároló Projektbeállítások.</span><span class="sxs-lookup"><span data-stu-id="ac331-141">Remove the default app module because you only want the project container settings.</span></span>
   
    ![Az alapértelmezett app modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. <span data-ttu-id="ac331-143">Modulok importálása a klónozott tárház az aktuális projektben.</span><span class="sxs-lookup"><span data-stu-id="ac331-143">Import modules from the cloned repo to the current project.</span></span>
   
    <span data-ttu-id="ac331-144">![Projekt importálása a gradle-lel](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![új modul lap](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span><span class="sxs-lookup"><span data-stu-id="ac331-144">![Import gradle project](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Create new module page](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span></span>
6. <span data-ttu-id="ac331-145">Ismételje meg ezeket a lépéseket a `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="ac331-145">Repeat these steps for the `oidlib-sample` module.</span></span>
7. <span data-ttu-id="ac331-146">A oidclib függőségek ellenőrzése a `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="ac331-146">Check the oidclib dependencies on the `oidlib-sample` module.</span></span>
   
    ![a oidlib-mintavételi modul oidclib függőségek](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. <span data-ttu-id="ac331-148">Kattintson a **OK** és várjon, amíg a gradle-szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="ac331-148">Click **OK** and wait for gradle sync.</span></span>
   
    <span data-ttu-id="ac331-149">A settings.gradle hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="ac331-149">Your settings.gradle should look like:</span></span>
   
    ![Képernyőkép a settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. <span data-ttu-id="ac331-151">Hozza létre a minta alkalmazását, és győződjön meg arról, hogy megfelelően fut a minta.</span><span class="sxs-lookup"><span data-stu-id="ac331-151">Build the sample app to make sure that the sample running correctly.</span></span>
   
    <span data-ttu-id="ac331-152">Sem lesz még használhatja az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="ac331-152">You won't be able to use this with Azure Active Directory yet.</span></span> <span data-ttu-id="ac331-153">Egyes végpontok először konfigurálni kell.</span><span class="sxs-lookup"><span data-stu-id="ac331-153">We'll need to configure some endpoints first.</span></span> <span data-ttu-id="ac331-154">Ez azért így az Android Studio problémák nem rendelkezik, először a mintaalkalmazás testreszabása előtt.</span><span class="sxs-lookup"><span data-stu-id="ac331-154">This is to ensure you don't have an Android Studio issues before we start customizing the sample app.</span></span>
10. <span data-ttu-id="ac331-155">Létrehozása és futtatása `oidlib-sample` Android Studio céljaként.</span><span class="sxs-lookup"><span data-stu-id="ac331-155">Build and run `oidlib-sample` as the target in Android Studio.</span></span>
    
    ![A minta oidlib build folyamatban](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. <span data-ttu-id="ac331-157">Törölje a `app ` címtárhoz, amely hagyták, ha a modul a projektből eltávolította, mert a biztonsági Android Studio nem törli.</span><span class="sxs-lookup"><span data-stu-id="ac331-157">Delete the `app ` directory that was left when you removed the module from the project because Android Studio doesn't delete it for safety.</span></span>
    
    ![Az alkalmazás könyvtárát tartalmazó fájlstruktúra](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. <span data-ttu-id="ac331-159">Nyissa meg a **szerkesztése konfigurációk** menüből, hogy távolítsa el a futtatási konfigurációját, hogy a modul a projekt való eltávolításakor is balra.</span><span class="sxs-lookup"><span data-stu-id="ac331-159">Open the **Edit Configurations** menu to remove the run configuration that was also left when you removed the module from the project.</span></span>
    
    <span data-ttu-id="ac331-160">![Konfigurációk menü szerkesztése](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![konfigurációs alkalmazás futtatása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span><span class="sxs-lookup"><span data-stu-id="ac331-160">![Edit configurations menu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
![Run configuration of app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span></span>

## <a name="configure-the-endpoints-of-the-sample"></a><span data-ttu-id="ac331-161">A minta végpontok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac331-161">Configure the endpoints of the sample</span></span>
<span data-ttu-id="ac331-162">Most, hogy a `oidlib-sample` fut-e., most szerkesztése egyes végpontok a működéséhez az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="ac331-162">Now that you have the `oidlib-sample` running successfully, let's edit some endpoints to get this working with Azure Active Directory.</span></span>

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a><span data-ttu-id="ac331-163">Az ügyfél konfigurálása a oidc_clientconf.xml fájl szerkesztésével</span><span class="sxs-lookup"><span data-stu-id="ac331-163">Configure your client by editing the oidc_clientconf.xml file</span></span>
1. <span data-ttu-id="ac331-164">Mivel csak a szolgáltatáshitelesítést egy token és a Graph API hívása OAuth2 adatfolyamok használ, állítsa be az ügyfelet az OAuth2 csak.</span><span class="sxs-lookup"><span data-stu-id="ac331-164">Because you are using OAuth2 flows only to get a token and call the Graph API, set the client to do OAuth2 only.</span></span> <span data-ttu-id="ac331-165">Egy újabb példa OIDC származnak.</span><span class="sxs-lookup"><span data-stu-id="ac331-165">OIDC will come in a later example.</span></span>
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. <span data-ttu-id="ac331-166">Az ügyfél-azonosító, hogy a regisztrációs portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac331-166">Configure your client ID that you received from the registration portal.</span></span>
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. <span data-ttu-id="ac331-167">Konfigurálja az átirányítási URI-t a egyet az alábbi.</span><span class="sxs-lookup"><span data-stu-id="ac331-167">Configure your redirect URI with the one below.</span></span>
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. <span data-ttu-id="ac331-168">A hatókörök, amelyekre szüksége van ahhoz, hogy hozzáférhessen a Graph API konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ac331-168">Configure your scopes that you need in order to access the Graph API.</span></span>
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

<span data-ttu-id="ac331-169">A `User.Read` értéket `oidc_scopes` lehetővé teszi a felhasználó által aláírt az alapvető profil olvasása.</span><span class="sxs-lookup"><span data-stu-id="ac331-169">The `User.Read` value in `oidc_scopes` allows you to read the basic profile the signed in user.</span></span>
<span data-ttu-id="ac331-170">Ön tudhat meg többet a rendelkezésre álló összes hatókör [Microsoft Graph-engedélyhatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="ac331-170">You can learn more about all the available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="ac331-171">Ha azt szeretné magyarázatokat kapcsolatos `openid` vagy `offline_access` , az OpenID Connect hatókörök, lásd: [2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="ac331-171">If you'd like explanations about `openid` or `offline_access` as scopes in OpenID Connect, see [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md).</span></span>

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a><span data-ttu-id="ac331-172">A oidc_endpoints.xml fájl szerkesztésével ügyfél végpontok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac331-172">Configure your client endpoints by editing the oidc_endpoints.xml file</span></span>
* <span data-ttu-id="ac331-173">Nyissa meg a `oidc_endpoints.xml` fájlt, és hajtsa végre a következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="ac331-173">Open the `oidc_endpoints.xml` file and make the following changes:</span></span>
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

<span data-ttu-id="ac331-174">Ezeket a végpontokat kell soha ne módosuljanak OAuth2 protokoll használatakor.</span><span class="sxs-lookup"><span data-stu-id="ac331-174">These endpoints should never change if you are using OAuth2 as your protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="ac331-175">A végpontokat a `userInfoEndpoint` és `revocationEndpoint` jelenleg nem támogatottak az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="ac331-175">The endpoints for `userInfoEndpoint` and `revocationEndpoint` are currently not supported by Azure Active Directory.</span></span> <span data-ttu-id="ac331-176">Ha nem adja meg ezeket az alapértelmezett example.com értékkel, fog emlékezteti, hogy nincsenek elérhető minta :-)</span><span class="sxs-lookup"><span data-stu-id="ac331-176">If you leave these with the default example.com value, you will be reminded that they are not available in the sample :-)</span></span>
> 
> 

## <a name="configure-a-graph-api-call"></a><span data-ttu-id="ac331-177">A Graph API-hívás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ac331-177">Configure a Graph API call</span></span>
* <span data-ttu-id="ac331-178">Nyissa meg a `HomeActivity.java` fájlt, és hajtsa végre a következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="ac331-178">Open the `HomeActivity.java` file and make the following changes:</span></span>
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

<span data-ttu-id="ac331-179">Egy egyszerű Graph API-hívás itt az adatait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ac331-179">Here a simple Graph API call returns our information.</span></span>

<span data-ttu-id="ac331-180">Most már az összes módosítást, végre kell hajtani.</span><span class="sxs-lookup"><span data-stu-id="ac331-180">Those are all the changes that you need to do.</span></span> <span data-ttu-id="ac331-181">Futtassa a `oidlib-sample` alkalmazáshoz, majd kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ac331-181">Run the `oidlib-sample` application, and click **Sign in**.</span></span>

<span data-ttu-id="ac331-182">Már sikeresen hitelesítése után válassza ki a **védett erőforrás kérelem** gombra kattintva tesztelheti a Graph API hívása.</span><span class="sxs-lookup"><span data-stu-id="ac331-182">After you've successfully authenticated, select the **Request Protected Resource** button to test your call to the Graph API.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="ac331-183">A termék biztonsági frissítések beszerzése</span><span class="sxs-lookup"><span data-stu-id="ac331-183">Get security updates for our product</span></span>
<span data-ttu-id="ac331-184">Javasoljuk, hogy a biztonsági események szóló értesítések lekérése látogasson el a [biztonsági TechCenter](https://technet.microsoft.com/security/dd252948) és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="ac331-184">We encourage you to get notifications about security incidents by visiting the [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

