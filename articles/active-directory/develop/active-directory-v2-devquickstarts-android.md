---
title: "aaaAzure Active Directory v2.0 Android-alkalmazás |} Microsoft Docs"
description: "Android-alkalmazás, mely aláírja a felhasználók személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókok és hívások toobuild hogyan Graph API hello harmadik féltől származó könyvtárak használatával."
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
ms.openlocfilehash: 1dd40bd3bcea28c629abce09abaed66b38774162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-android-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a><span data-ttu-id="7e651-103">Egy külső könyvtár használata a Graph API segítségével hello v2.0-végponttól bejelentkezési tooan Android alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7e651-103">Add sign-in tooan Android app using a third-party library with Graph API using hello v2.0 endpoint</span></span>
<span data-ttu-id="7e651-104">hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja.</span><span class="sxs-lookup"><span data-stu-id="7e651-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="7e651-105">A fejlesztők a kívánják a szolgáltatások toointegrate függvénytárat.</span><span class="sxs-lookup"><span data-stu-id="7e651-105">Developers can use any library they want toointegrate with our services.</span></span> <span data-ttu-id="7e651-106">toohelp fejlesztők más könyvtárakkal a platformot használ, azt korábban írt néhány forgatókönyvek például a egy toodemonstrate hogyan tooconfigure külső szalagtárak tooconnect toohello Microsoft identitásplatformmal.</span><span class="sxs-lookup"><span data-stu-id="7e651-106">toohelp developers use our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure third-party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="7e651-107">A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) toohello Microsoft identitásplatformmal kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="7e651-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect toohello Microsoft identity platform.</span></span>

<span data-ttu-id="7e651-108">Ez a forgatókönyv létrehozó hello alkalmazást, a felhasználók tootheir szervezet bejelentkezhet és majd keresse meg a magukat a szervezetek hello Graph API segítségével.</span><span class="sxs-lookup"><span data-stu-id="7e651-108">With hello application that this walkthrough creates, users can sign in tootheir organization and then search for themselves in their organization by using hello Graph API.</span></span>

<span data-ttu-id="7e651-109">Ha új tooOAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem tehetik logika tooyou.</span><span class="sxs-lookup"><span data-stu-id="7e651-109">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make sense tooyou.</span></span> <span data-ttu-id="7e651-110">Azt javasoljuk, hogy olvassa el [2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md) a háttérben.</span><span class="sxs-lookup"><span data-stu-id="7e651-110">We recommend that you read [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="7e651-111">Egyes szolgáltatások, amelyek rendelkeznek egy kifejezés hello OAuth2 vagy OpenID Connect szabványok, például a feltételes hozzáférés és a Csoportházirend kezelése Intune-ban, a platform szükség akkor toouse a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú.</span><span class="sxs-lookup"><span data-stu-id="7e651-111">Some features of our platform that do have an expression in hello OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you toouse our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="7e651-112">hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók.</span><span class="sxs-lookup"><span data-stu-id="7e651-112">hello v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="7e651-113">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7e651-113">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-hello-code-from-github"></a><span data-ttu-id="7e651-114">A Githubból hello kód letöltése</span><span class="sxs-lookup"><span data-stu-id="7e651-114">Download hello code from GitHub</span></span>
<span data-ttu-id="7e651-115">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span><span class="sxs-lookup"><span data-stu-id="7e651-115">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span></span>  <span data-ttu-id="7e651-116">toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="7e651-116">toofollow along, you can  [download hello app's skeleton as a .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

<span data-ttu-id="7e651-117">Csak is letöltheti hello mintát, és rögtön használatba:</span><span class="sxs-lookup"><span data-stu-id="7e651-117">You can also just download hello sample and get started right away:</span></span>

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="7e651-118">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="7e651-118">Register an app</span></span>
<span data-ttu-id="7e651-119">Hozzon létre egy új alkalmazást hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a részletes lépésekről hello [hogyan tooregister egy alkalmazást a v2.0-végponttól hello](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="7e651-119">Create a new app at hello [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow hello detailed steps at [How tooregister an app with hello v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="7e651-120">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="7e651-120">Make sure to:</span></span>

* <span data-ttu-id="7e651-121">Másolás hello **alkalmazásazonosító** , amely hozzárendelt tooyour app, mert hamarosan kell.</span><span class="sxs-lookup"><span data-stu-id="7e651-121">Copy hello **Application Id** that's assigned tooyour app because you'll need it soon.</span></span>
* <span data-ttu-id="7e651-122">Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="7e651-122">Add hello **Mobile** platform for your app.</span></span>

> <span data-ttu-id="7e651-123">Megjegyzés: hello alkalmazásregisztrációs portálra biztosít egy **átirányítási URI-** érték.</span><span class="sxs-lookup"><span data-stu-id="7e651-123">Note: hello Application registration portal provides a **Redirect URI** value.</span></span> <span data-ttu-id="7e651-124">Azonban ez a példa kell használnia hello alapértelmezett értékének `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span><span class="sxs-lookup"><span data-stu-id="7e651-124">However, in this example you must use hello default value of `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>
> 
> 

## <a name="download-hello-nxoauth2-third-party-library-and-create-a-workspace"></a><span data-ttu-id="7e651-125">Töltse le a hello NXOAuth2 külső könyvtárban, és egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e651-125">Download hello NXOAuth2 third-party library and create a workspace</span></span>
<span data-ttu-id="7e651-126">A forgatókönyv a Githubból, amely alapján hello Google kódját OpenID Connect OAuth2 könyvtár OIDCAndroidLib hello fogja használni.</span><span class="sxs-lookup"><span data-stu-id="7e651-126">For this walkthrough, you will use hello OIDCAndroidLib from GitHub, which is an OAuth2 library based on hello OpenID Connect code of Google.</span></span> <span data-ttu-id="7e651-127">Hello natív alkalmazásprofil valósítja meg, és támogatja a hello engedélyezési végpont hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7e651-127">It implements hello native application profile and supports hello authorization endpoint of hello user.</span></span> <span data-ttu-id="7e651-128">Az alábbiakban összes hello, konfigurálnia kell toointegrate hello Microsoft identitásplatformmal együttműködve.</span><span class="sxs-lookup"><span data-stu-id="7e651-128">These are all hello things that you'll need toointegrate with hello Microsoft identity platform.</span></span>

<span data-ttu-id="7e651-129">Hello OIDCAndroidLib tárház tooyour számítógép klónozása.</span><span class="sxs-lookup"><span data-stu-id="7e651-129">Clone hello OIDCAndroidLib repo tooyour computer.</span></span>

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a><span data-ttu-id="7e651-131">Az Android Studio környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="7e651-131">Set up your Android Studio environment</span></span>
1. <span data-ttu-id="7e651-132">Hozzon létre egy új Android Studio-projektet, és fogadja el hello alapértelmezett hello varázslóban.</span><span class="sxs-lookup"><span data-stu-id="7e651-132">Create a new Android Studio project and accept hello defaults in hello wizard.</span></span>
   
    ![Az Android Studio új projekt létrehozása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Cél Android-eszközök](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Egy tevékenység toomobile hozzáadása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. <span data-ttu-id="7e651-136">tooset be a projekt modulok áthelyezése hello klónozott tárház toohello projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="7e651-136">tooset up your project modules, move hello cloned repo toohello project location.</span></span> <span data-ttu-id="7e651-137">Hello projekt is létrehozhat, és ezután klónozza közvetlenül toohello projekt helyére.</span><span class="sxs-lookup"><span data-stu-id="7e651-137">You can also create hello project and then clone it directly toohello project location.</span></span>
   
    ![Projekt modulok](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. <span data-ttu-id="7e651-139">Nyissa meg a hello modulok Projektbeállítások hello helyi menü használatával vagy hello Ctrl + Alt + Maj + S parancsikon használatával.</span><span class="sxs-lookup"><span data-stu-id="7e651-139">Open hello project modules settings by using hello context menu or by using hello Ctrl+Alt+Maj+S shortcut.</span></span>
   
    ![Modulok Projektbeállítások](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. <span data-ttu-id="7e651-141">Eltávolít hello alapértelmezett app modult, mivel csak hello Projektbeállítások tároló.</span><span class="sxs-lookup"><span data-stu-id="7e651-141">Remove hello default app module because you only want hello project container settings.</span></span>
   
    ![hello alapértelmezett app modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. <span data-ttu-id="7e651-143">Modul importálása hello klónozott tárház toohello aktuális projektben.</span><span class="sxs-lookup"><span data-stu-id="7e651-143">Import modules from hello cloned repo toohello current project.</span></span>
   
    <span data-ttu-id="7e651-144">![Projekt importálása a gradle-lel](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![új modul lap](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span><span class="sxs-lookup"><span data-stu-id="7e651-144">![Import gradle project](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Create new module page](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span></span>
6. <span data-ttu-id="7e651-145">Ismételje meg ezeket a lépéseket hello `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="7e651-145">Repeat these steps for hello `oidlib-sample` module.</span></span>
7. <span data-ttu-id="7e651-146">Hello oidclib függőség a hello ellenőrzése `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="7e651-146">Check hello oidclib dependencies on hello `oidlib-sample` module.</span></span>
   
    ![oidclib függőségek hello oidlib-mintavételi modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. <span data-ttu-id="7e651-148">Kattintson a **OK** és várjon, amíg a gradle-szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="7e651-148">Click **OK** and wait for gradle sync.</span></span>
   
    <span data-ttu-id="7e651-149">A settings.gradle hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="7e651-149">Your settings.gradle should look like:</span></span>
   
    ![Képernyőkép a settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. <span data-ttu-id="7e651-151">Build hello sample app toomake meg arról, hogy megfelelően fut hello minta.</span><span class="sxs-lookup"><span data-stu-id="7e651-151">Build hello sample app toomake sure that hello sample running correctly.</span></span>
   
    <span data-ttu-id="7e651-152">Ön nem fogja tudni toouse Ez az Azure Active Directoryval még.</span><span class="sxs-lookup"><span data-stu-id="7e651-152">You won't be able toouse this with Azure Active Directory yet.</span></span> <span data-ttu-id="7e651-153">Szükség lesz tooconfigure egyes végpontok először.</span><span class="sxs-lookup"><span data-stu-id="7e651-153">We'll need tooconfigure some endpoints first.</span></span> <span data-ttu-id="7e651-154">Ez a tooensure nem rendelkezik az Android Studio problémák előtt először hello mintaalkalmazás testreszabása.</span><span class="sxs-lookup"><span data-stu-id="7e651-154">This is tooensure you don't have an Android Studio issues before we start customizing hello sample app.</span></span>
10. <span data-ttu-id="7e651-155">Létrehozása és futtatása `oidlib-sample` Android Studio hello célként.</span><span class="sxs-lookup"><span data-stu-id="7e651-155">Build and run `oidlib-sample` as hello target in Android Studio.</span></span>
    
    ![A minta oidlib build folyamatban](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. <span data-ttu-id="7e651-157">Törölje a hello `app ` címtárhoz, amely hagyták, amikor hello modul hello projektből eltávolította, mert a biztonsági Android Studio nem törli.</span><span class="sxs-lookup"><span data-stu-id="7e651-157">Delete hello `app ` directory that was left when you removed hello module from hello project because Android Studio doesn't delete it for safety.</span></span>
    
    ![Hello alkalmazáskönyvtárban tartalmazó fájlstruktúra](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. <span data-ttu-id="7e651-159">Nyissa meg hello **szerkesztése konfigurációk** menü tooremove futtatása hello konfigurációs hello modul hello projekt való eltávolításakor is balra.</span><span class="sxs-lookup"><span data-stu-id="7e651-159">Open hello **Edit Configurations** menu tooremove hello run configuration that was also left when you removed hello module from hello project.</span></span>
    
    <span data-ttu-id="7e651-160">![Konfigurációk menü szerkesztése](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![konfigurációs alkalmazás futtatása](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span><span class="sxs-lookup"><span data-stu-id="7e651-160">![Edit configurations menu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
![Run configuration of app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span></span>

## <a name="configure-hello-endpoints-of-hello-sample"></a><span data-ttu-id="7e651-161">Hello minta hello végpontok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e651-161">Configure hello endpoints of hello sample</span></span>
<span data-ttu-id="7e651-162">Most, hogy hello `oidlib-sample` fut-e., most szerkesztése egyes végpontok tooget a munkát az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="7e651-162">Now that you have hello `oidlib-sample` running successfully, let's edit some endpoints tooget this working with Azure Active Directory.</span></span>

### <a name="configure-your-client-by-editing-hello-oidcclientconfxml-file"></a><span data-ttu-id="7e651-163">Az ügyfél konfigurálása hello oidc_clientconf.xml fájl szerkesztésével</span><span class="sxs-lookup"><span data-stu-id="7e651-163">Configure your client by editing hello oidc_clientconf.xml file</span></span>
1. <span data-ttu-id="7e651-164">OAuth2 adatfolyamok csak tooget egy tokent használ, és hello Graph API hívása, mert hello ügyfél toodo OAuth2 csak beállítása</span><span class="sxs-lookup"><span data-stu-id="7e651-164">Because you are using OAuth2 flows only tooget a token and call hello Graph API, set hello client toodo OAuth2 only.</span></span> <span data-ttu-id="7e651-165">Egy újabb példa OIDC származnak.</span><span class="sxs-lookup"><span data-stu-id="7e651-165">OIDC will come in a later example.</span></span>
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. <span data-ttu-id="7e651-166">Az ügyfél-azonosító kapott hello regisztrációs portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e651-166">Configure your client ID that you received from hello registration portal.</span></span>
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. <span data-ttu-id="7e651-167">Konfigurálja az átirányítási URI-t egy hello alatt.</span><span class="sxs-lookup"><span data-stu-id="7e651-167">Configure your redirect URI with hello one below.</span></span>
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. <span data-ttu-id="7e651-168">A hatókörök, hogy meg kell a rendelés tooaccess hello Graph API konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7e651-168">Configure your scopes that you need in order tooaccess hello Graph API.</span></span>
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

<span data-ttu-id="7e651-169">Hello `User.Read` értéket `oidc_scopes` lehetővé teszi, hogy tooread hello alapvető profiladataihoz hello bejelentkezett felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7e651-169">hello `User.Read` value in `oidc_scopes` allows you tooread hello basic profile hello signed in user.</span></span>
<span data-ttu-id="7e651-170">Minden hello elérhető hatókörök kapcsolatos részletesebb [Microsoft Graph-engedélyhatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="7e651-170">You can learn more about all hello available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="7e651-171">Ha azt szeretné magyarázatokat kapcsolatos `openid` vagy `offline_access` , az OpenID Connect hatókörök, lásd: [2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="7e651-171">If you'd like explanations about `openid` or `offline_access` as scopes in OpenID Connect, see [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md).</span></span>

### <a name="configure-your-client-endpoints-by-editing-hello-oidcendpointsxml-file"></a><span data-ttu-id="7e651-172">Az ügyfél-végpontok hello oidc_endpoints.xml fájl szerkesztésével konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e651-172">Configure your client endpoints by editing hello oidc_endpoints.xml file</span></span>
* <span data-ttu-id="7e651-173">Nyissa meg hello `oidc_endpoints.xml` , és győződjön meg a következő módosításokat hello:</span><span class="sxs-lookup"><span data-stu-id="7e651-173">Open hello `oidc_endpoints.xml` file and make hello following changes:</span></span>
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

<span data-ttu-id="7e651-174">Ezeket a végpontokat kell soha ne módosuljanak OAuth2 protokoll használatakor.</span><span class="sxs-lookup"><span data-stu-id="7e651-174">These endpoints should never change if you are using OAuth2 as your protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="7e651-175">a végpontok hello `userInfoEndpoint` és `revocationEndpoint` jelenleg nem támogatottak az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="7e651-175">hello endpoints for `userInfoEndpoint` and `revocationEndpoint` are currently not supported by Azure Active Directory.</span></span> <span data-ttu-id="7e651-176">Ha nem adja meg ezen a hello alapértékként example.com, fog emlékezteti, hogy nincsenek elérhető hello mintában :-)</span><span class="sxs-lookup"><span data-stu-id="7e651-176">If you leave these with hello default example.com value, you will be reminded that they are not available in hello sample :-)</span></span>
> 
> 

## <a name="configure-a-graph-api-call"></a><span data-ttu-id="7e651-177">A Graph API-hívás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e651-177">Configure a Graph API call</span></span>
* <span data-ttu-id="7e651-178">Nyissa meg hello `HomeActivity.java` , és győződjön meg a következő módosításokat hello:</span><span class="sxs-lookup"><span data-stu-id="7e651-178">Open hello `HomeActivity.java` file and make hello following changes:</span></span>
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

<span data-ttu-id="7e651-179">Egy egyszerű Graph API-hívás itt az adatait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7e651-179">Here a simple Graph API call returns our information.</span></span>

<span data-ttu-id="7e651-180">Most már az összes hello módosítását, hogy kell-e toodo.</span><span class="sxs-lookup"><span data-stu-id="7e651-180">Those are all hello changes that you need toodo.</span></span> <span data-ttu-id="7e651-181">Futtassa a hello `oidlib-sample` alkalmazáshoz, majd kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7e651-181">Run hello `oidlib-sample` application, and click **Sign in**.</span></span>

<span data-ttu-id="7e651-182">Már sikeresen hitelesítése után válassza ki a hello **védett erőforrás kérelem** tootest gombra a hívás toohello Graph API-val.</span><span class="sxs-lookup"><span data-stu-id="7e651-182">After you've successfully authenticated, select hello **Request Protected Resource** button tootest your call toohello Graph API.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="7e651-183">A termék biztonsági frissítések beszerzése</span><span class="sxs-lookup"><span data-stu-id="7e651-183">Get security updates for our product</span></span>
<span data-ttu-id="7e651-184">Javasoljuk, biztonsági incidensek tooget értesítések hello felkeresésével [biztonsági TechCenter](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="7e651-184">We encourage you tooget notifications about security incidents by visiting hello [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

