---
title: "Bejelentkezés hozzáadása egy iOS-alkalmazást az Azure AD v2.0-végponttól |} Microsoft Docs"
description: "Egy iOS-alkalmazást, amely képes bejelentkeztetni a felhasználókat, és mindkét személyes Microsoft-fiók létrehozása és a munkahelyi vagy iskolai fiókok külső könyvtárak használatával."
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: fd3603c0-42f7-438c-87b5-a52d20d6344b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: cf1455dc3d55ea3581195f7a315556d134c23a26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a><span data-ttu-id="d537a-103">Bejelentkezés hozzáadása egy iOS-alkalmazást használ egy külső könyvtár Graph API-t használ a v2.0-végpontra</span><span class="sxs-lookup"><span data-stu-id="d537a-103">Add sign-in to an iOS app using a third-party library with Graph API using the v2.0 endpoint</span></span>
<span data-ttu-id="d537a-104">A Microsoft identitásplatformja nyílt szabványokat, többek között OAuth2-t és OpenID Connectet használ.</span><span class="sxs-lookup"><span data-stu-id="d537a-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="d537a-105">A fejlesztők a függvénytárat, hogy integrálni szeretne a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="d537a-105">Developers can use any library they want to integrate with our services.</span></span> <span data-ttu-id="d537a-106">Segítségével a fejlesztők a platformot használja a többi könyvtárak, azt korábban írt bemutatják, hogyan lehet kapcsolódni a Microsoft identity platform külső szalagtárak konfigurálása a jelen szoftverhez hasonló néhány forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="d537a-106">To help developers use our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure third-party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="d537a-107">A legtöbb tárak, amelyek megvalósítják az [a RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) csatlakozni tud-e a Microsoft identity platform.</span><span class="sxs-lookup"><span data-stu-id="d537a-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect to the Microsoft identity platform.</span></span>

<span data-ttu-id="d537a-108">Ez a forgatókönyv hoz létre az alkalmazással felhasználók jelentkezzen be a szervezet és majd keresse meg a többi a szervezetek a Graph API használatával.</span><span class="sxs-lookup"><span data-stu-id="d537a-108">With the application that this walkthrough creates, users can sign in to their organization and then search for others in their organization by using the Graph API.</span></span>

<span data-ttu-id="d537a-109">Ha most ismerkedik az OAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem célszerű Önnek.</span><span class="sxs-lookup"><span data-stu-id="d537a-109">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make sense to you.</span></span> <span data-ttu-id="d537a-110">Azt javasoljuk, hogy olvassa el [v2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md) a háttérben.</span><span class="sxs-lookup"><span data-stu-id="d537a-110">We recommend that you read  [v2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="d537a-111">Egyes szolgáltatások, amelyek rendelkeznek egy kifejezést a OAuth2 vagy az OpenID Connect szabványok, például a feltételes hozzáférés és a Csoportházirend kezelése Intune-ban, a platform kell használni a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú.</span><span class="sxs-lookup"><span data-stu-id="d537a-111">Some features of our platform that do have an expression in the OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you to use our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="d537a-112">A v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók.</span><span class="sxs-lookup"><span data-stu-id="d537a-112">The v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="d537a-113">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d537a-113">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-code-from-github"></a><span data-ttu-id="d537a-114">Töltse le a kód a Githubról</span><span class="sxs-lookup"><span data-stu-id="d537a-114">Download code from GitHub</span></span>
<span data-ttu-id="d537a-115">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span><span class="sxs-lookup"><span data-stu-id="d537a-115">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span></span>  <span data-ttu-id="d537a-116">Követéséhez is [töltse le az alkalmazás vázát egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="d537a-116">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

<span data-ttu-id="d537a-117">A minta csak is tölthetik le, és rögtön használatba:</span><span class="sxs-lookup"><span data-stu-id="d537a-117">You can also just download the sample and get started right away:</span></span>

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="d537a-118">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="d537a-118">Register an app</span></span>
<span data-ttu-id="d537a-119">Hozzon létre egy új alkalmazást a [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy részletes kövesse a [egy alkalmazás regisztrálása a v2.0-végponttal](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="d537a-119">Create a new app at the [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow the detailed steps at  [How to register an app with the v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="d537a-120">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="d537a-120">Make sure to:</span></span>

* <span data-ttu-id="d537a-121">Másolás a **alkalmazásazonosító** , amely hozzá van rendelve az alkalmazás mivel hamarosan lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="d537a-121">Copy the **Application Id** that's assigned to your app because you'll need it soon.</span></span>
* <span data-ttu-id="d537a-122">Adja hozzá a **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="d537a-122">Add the **Mobile** platform for your app.</span></span>
* <span data-ttu-id="d537a-123">Másolás a **átirányítási URI-** a portálról.</span><span class="sxs-lookup"><span data-stu-id="d537a-123">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="d537a-124">Az alapértelmezett értéket kell használnia `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="d537a-124">You must use the default value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a><span data-ttu-id="d537a-125">Töltse le a külső NXOAuth2 könyvtárban, és egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="d537a-125">Download the third-party NXOAuth2 library and create a workspace</span></span>
<span data-ttu-id="d537a-126">A forgatókönyv a Githubból, amely a Mac OS X és az iOS rendszerhez (Cocoa és Cocoa touch) OAuth2 könyvtár OAuth2Client fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d537a-126">For this walkthrough, you will use the OAuth2Client from GitHub, which is an OAuth2 library for Mac OS X and iOS (Cocoa and Cocoa touch).</span></span> <span data-ttu-id="d537a-127">A kódtár az OAuth2 specifikációinak 10-es tervezetén alapul.</span><span class="sxs-lookup"><span data-stu-id="d537a-127">This library is based on draft 10 of the OAuth2 spec.</span></span> <span data-ttu-id="d537a-128">A natív alkalmazásprofil valósítja meg, és a felhasználó az engedélyezési végpont támogatja.</span><span class="sxs-lookup"><span data-stu-id="d537a-128">It implements the native application profile and supports the authorization endpoint of the user.</span></span> <span data-ttu-id="d537a-129">Az alábbiakban összes integrálhatja a Microsoft identitásplatformmal együttműködve kell.</span><span class="sxs-lookup"><span data-stu-id="d537a-129">These are all the things you'll need to integrate with the Microsoft identity platform.</span></span>

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a><span data-ttu-id="d537a-130">A könyvtár hozzáadása a projekthez a CocoaPods segítségével</span><span class="sxs-lookup"><span data-stu-id="d537a-130">Add the library to your project by using CocoaPods</span></span>
<span data-ttu-id="d537a-131">A CocoaPods egy Xcode-projektekhez készült függőségkezelő.</span><span class="sxs-lookup"><span data-stu-id="d537a-131">CocoaPods is a dependency manager for Xcode projects.</span></span> <span data-ttu-id="d537a-132">Automatikusan kezeli a korábbi telepítési lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d537a-132">It manages the previous installation steps automatically.</span></span>

```
$ vi Podfile
```
1. <span data-ttu-id="d537a-133">Adja hozzá a következőt a pod-fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="d537a-133">Add the following to this podfile:</span></span>
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. <span data-ttu-id="d537a-134">CocoaPods segítségével töltse be a podfile.</span><span class="sxs-lookup"><span data-stu-id="d537a-134">Load the podfile by using CocoaPods.</span></span> <span data-ttu-id="d537a-135">Ezzel létrehoz egy új Xcode-munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="d537a-135">This will create a new Xcode workspace that you will load.</span></span>
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a><span data-ttu-id="d537a-136">Megismerkedhet a projekt felépítése</span><span class="sxs-lookup"><span data-stu-id="d537a-136">Explore the structure of the project</span></span>
<span data-ttu-id="d537a-137">A projekt a vázat be van állítva az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="d537a-137">The following structure is set up for our project in the skeleton:</span></span>

* <span data-ttu-id="d537a-138">Egy nézet egy egyszerű keresés</span><span class="sxs-lookup"><span data-stu-id="d537a-138">A Master View with a UPN Search</span></span>
* <span data-ttu-id="d537a-139">A részletes nézet a kiválasztott felhasználóval kapcsolatos adatok</span><span class="sxs-lookup"><span data-stu-id="d537a-139">A Detail View for the data about the selected user</span></span>
* <span data-ttu-id="d537a-140">Ha egy felhasználó bejelentkezhessen a az alkalmazás lekérdezni a diagram bejelentkezési nézet</span><span class="sxs-lookup"><span data-stu-id="d537a-140">A Login View where a user can sign in to the app to query the graph</span></span>

<span data-ttu-id="d537a-141">A vázat a különböző fájlok azt helyezi át, a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="d537a-141">We will move to various files in the skeleton to add authentication.</span></span> <span data-ttu-id="d537a-142">A kód, például a visual code más részei identitás nem vonatkoznak, de megadta-e meg.</span><span class="sxs-lookup"><span data-stu-id="d537a-142">Other parts of the code, such as the visual code, do not pertain to identity but are provided for you.</span></span>

## <a name="set-up-the-settingsplst-file-in-the-library"></a><span data-ttu-id="d537a-143">Állítsa be a settings.plst fájl a könyvtárban</span><span class="sxs-lookup"><span data-stu-id="d537a-143">Set up the settings.plst file in the library</span></span>
* <span data-ttu-id="d537a-144">A gyors üzembe helyezés projektben nyissa meg a `settings.plist` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d537a-144">In the QuickStart project, open the `settings.plist` file.</span></span> <span data-ttu-id="d537a-145">Cserélje le az értékeket az elemek a szakaszban az Azure portálon használt értékeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d537a-145">Replace the values of the elements in the section to reflect the values that you used in the Azure portal.</span></span> <span data-ttu-id="d537a-146">A kód minden alkalommal az Active Directory Authentication Library hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d537a-146">Your code will reference these values whenever it uses the Active Directory Authentication Library.</span></span>
  * <span data-ttu-id="d537a-147">A `clientId` a portálról másolt az alkalmazás ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="d537a-147">The `clientId` is the client ID of your application that you copied from the portal.</span></span>
  * <span data-ttu-id="d537a-148">A `redirectUri` a portálon megadott átirányítási URL-cím.</span><span class="sxs-lookup"><span data-stu-id="d537a-148">The `redirectUri` is the redirect URL that the portal provided.</span></span>

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a><span data-ttu-id="d537a-149">Állítsa be a LoginViewController a NXOAuth2Client szalagtár</span><span class="sxs-lookup"><span data-stu-id="d537a-149">Set up the NXOAuth2Client library in your LoginViewController</span></span>
<span data-ttu-id="d537a-150">A NXOAuth2Client tartalomtárát néhány érték első beállítása.</span><span class="sxs-lookup"><span data-stu-id="d537a-150">The NXOAuth2Client library requires some values to get set up.</span></span> <span data-ttu-id="d537a-151">Miután elvégezte ezt a feladatot, a megszerzett jogkivonat segítségével a Graph API hívása.</span><span class="sxs-lookup"><span data-stu-id="d537a-151">After you complete that task, you can use the acquired token to call the Graph API.</span></span> <span data-ttu-id="d537a-152">Mivel `LoginView` lesz nevű bármikor kell hitelesíteni, érdemes, amelyre a konfigurációs értékeket a fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="d537a-152">Because `LoginView` will be called any time we need to authenticate, it makes sense to put configuration values in to that file.</span></span>

* <span data-ttu-id="d537a-153">Adjunk néhány értéket, hogy a `LoginViewController.m` fájlt beállítani a környezetet a hitelesítéshez és engedélyezéshez.</span><span class="sxs-lookup"><span data-stu-id="d537a-153">Let's add some values to the  `LoginViewController.m` file to set the context for authentication and authorization.</span></span> <span data-ttu-id="d537a-154">Az értékek részleteit kövesse a kódot.</span><span class="sxs-lookup"><span data-stu-id="d537a-154">Details about the values follow the code.</span></span>
  
    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

<span data-ttu-id="d537a-155">Vizsgáljuk meg a kódot részleteit.</span><span class="sxs-lookup"><span data-stu-id="d537a-155">Let's look at details about the code.</span></span>

<span data-ttu-id="d537a-156">A rendszer az első karakterlánc `scopes`.</span><span class="sxs-lookup"><span data-stu-id="d537a-156">The first string is for `scopes`.</span></span>  <span data-ttu-id="d537a-157">A `User.Read` érték lehetővé teszi, hogy olvassa a bejelentkezett felhasználó alapvető profiladataihoz.</span><span class="sxs-lookup"><span data-stu-id="d537a-157">The `User.Read` value allows you to read the basic profile of the signed in user.</span></span>

<span data-ttu-id="d537a-158">Ön tudhat meg többet a rendelkezésre álló összes hatókör [Microsoft Graph-engedélyhatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="d537a-158">You can learn more about all the available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="d537a-159">A `authURL`, `loginURL`, `bhh`, és `tokenURL`, a korábban megadott értékeket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d537a-159">For `authURL`, `loginURL`, `bhh`, and `tokenURL`, you should use the values provided previously.</span></span> <span data-ttu-id="d537a-160">Ha használja a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú, azt lekérik a ezeket az adatokat, a metaadat-végpontjához használatával.</span><span class="sxs-lookup"><span data-stu-id="d537a-160">If you use the open source Microsoft Azure Identity Libraries, we pull this data down for you by using our metadata endpoint.</span></span> <span data-ttu-id="d537a-161">A nehezét, azaz az értékek kinyerését mi végezzük Ön helyett.</span><span class="sxs-lookup"><span data-stu-id="d537a-161">We've done the hard work of extracting these values for you.</span></span>

<span data-ttu-id="d537a-162">A `keychain` érték azt a tárolót adja meg, amelyet az NXOAuth2Client kódtár a jogkivonatok tárolására szolgáló kulcslánc létrehozásához fog használni.</span><span class="sxs-lookup"><span data-stu-id="d537a-162">The `keychain` value is the container that the NXOAuth2Client library will use to create a keychain to store your tokens.</span></span> <span data-ttu-id="d537a-163">Ha szeretné beolvasni az egyszeri bejelentkezés (SSO) számos alkalmazások között, adja meg az azonos kulcslánc minden, az alkalmazások, és kérheti, hogy kulcslánc használatát az Xcode jogosultságok.</span><span class="sxs-lookup"><span data-stu-id="d537a-163">If you'd like to get single sign-on (SSO) across numerous apps, you can specify the same keychain in each of your applications and request the use of that keychain in your Xcode entitlements.</span></span> <span data-ttu-id="d537a-164">Ennek a magyarázatát, az Apple-dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="d537a-164">This is explained in the Apple documentation.</span></span>

<span data-ttu-id="d537a-165">Ezeket az értékeket a többi is használhassa a kódtárat, és hozzon létre, ha a környezet értékeket hajthat helyeken.</span><span class="sxs-lookup"><span data-stu-id="d537a-165">The rest of these values are required to use the library and create places for you to carry values to the context.</span></span>

### <a name="create-a-url-cache"></a><span data-ttu-id="d537a-166">Egy URL-gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="d537a-166">Create a URL cache</span></span>
<span data-ttu-id="d537a-167">Belül `(void)viewDidLoad()`, amely mindig neve után a nézetben be van töltve, a következő kódot primes jelek használata a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="d537a-167">Inside `(void)viewDidLoad()`, which is always called after the view is loaded, the following code primes a cache for our use.</span></span>

<span data-ttu-id="d537a-168">Adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d537a-168">Add the following code:</span></span>

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a><span data-ttu-id="d537a-169">A bejelentkezés a webes nézet létrehozása</span><span class="sxs-lookup"><span data-stu-id="d537a-169">Create a WebView for sign-in</span></span>
<span data-ttu-id="d537a-170">A webes nézet a felhasználótól például SMS szöveges üzenet további tényező (Ha be van állítva), vagy hiba üzeneteket visszaküldeni, a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="d537a-170">A WebView can prompt the user for additional factors like SMS text message (if configured) or return error messages to the user.</span></span> <span data-ttu-id="d537a-171">Itt lesznek állítva a webes nézet össze, és jegyezze később a visszahívásokat, amely fordul elő a webes nézet az identitás szolgáltatással kezelni a kódot.</span><span class="sxs-lookup"><span data-stu-id="d537a-171">Here you'll set up the WebView and then later write the code to handle the callbacks that will happen in the WebView from the identity services.</span></span>

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a><span data-ttu-id="d537a-172">Írja felül a webnézet metódusait a hitelesítés kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d537a-172">Override the WebView methods to handle authentication</span></span>
<span data-ttu-id="d537a-173">Kérje meg a webes nézet, mi történik, ha a felhasználó nem jelentkezhet be a korábban bemutatott, illessze be az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="d537a-173">To tell the WebView what happens when a user needs to sign in as discussed previously, you can paste the following code.</span></span>

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a><span data-ttu-id="d537a-174">Írja meg az OAuth2-kérés eredményét kezelő kódot.</span><span class="sxs-lookup"><span data-stu-id="d537a-174">Write code to handle the result of the OAuth2 request</span></span>
<span data-ttu-id="d537a-175">A következő kódot fogja kezelni a redirectURL, amely a webes nézet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d537a-175">The following code will handle the redirectURL that returns from the WebView.</span></span> <span data-ttu-id="d537a-176">Ha a hitelesítés nem volt sikeres, a kódot újra próbálkozik.</span><span class="sxs-lookup"><span data-stu-id="d537a-176">If authentication wasn't successful, the code will try again.</span></span> <span data-ttu-id="d537a-177">Eközben a könyvtárban fogja biztosítani a hiba, amelyet a konzolon látható elemek vagy aszinkron módon kezeli.</span><span class="sxs-lookup"><span data-stu-id="d537a-177">Meanwhile, the library will provide the error that you can see in the console or handle asynchronously.</span></span>

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a><span data-ttu-id="d537a-178">A (néven fióktároló) OAuth-környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="d537a-178">Set up the OAuth Context (called account store)</span></span>
<span data-ttu-id="d537a-179">Itt hívása `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` minden egyes szolgáltatás, amely az alkalmazás által érhetik el a megosztott fiók áruházban.</span><span class="sxs-lookup"><span data-stu-id="d537a-179">Here you can call `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` on the shared account store for each service that you want the application to be able to access.</span></span> <span data-ttu-id="d537a-180">A fiók típusát: egy bizonyos szolgáltatás azonosítóként használt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d537a-180">The account type is a string that is used as an identifier for a certain service.</span></span> <span data-ttu-id="d537a-181">A Graph API-val érik el, mert a kód hivatkozik rá `"myGraphService"`.</span><span class="sxs-lookup"><span data-stu-id="d537a-181">Because you are accessing the Graph API, the code refers to it as `"myGraphService"`.</span></span> <span data-ttu-id="d537a-182">Majd állítsa be, amely jelzi, ha bármilyen változás a jogkivonatok megfigyelő.</span><span class="sxs-lookup"><span data-stu-id="d537a-182">You then set up an observer that will tell you when anything changes with the token.</span></span> <span data-ttu-id="d537a-183">Miután megkapta a jogkivonatot, visszatérési a felhasználó biztonsági a `masterView`.</span><span class="sxs-lookup"><span data-stu-id="d537a-183">After you get the token, you return the user back to the `masterView`.</span></span>

```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a><span data-ttu-id="d537a-184">Állítsa be a nézet kikeresheti és megjelenítheti a felhasználóknak a Graph API-n</span><span class="sxs-lookup"><span data-stu-id="d537a-184">Set up the Master View to search and display the users from the Graph API</span></span>
<span data-ttu-id="d537a-185">A fő-vezérlő (MVC) alkalmazást, amely a visszaadott adatok megjelennek a rács túlmutat a jelen útmutató, és sok online oktatóprogramok bemutatják, hogyan hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="d537a-185">A Master-View-Controller (MVC) app that displays the returned data in the grid is beyond the scope of this walkthrough, and many online tutorials explain how to build one.</span></span> <span data-ttu-id="d537a-186">Ez a kód az üres fájl van.</span><span class="sxs-lookup"><span data-stu-id="d537a-186">All this code is in the skeleton file.</span></span> <span data-ttu-id="d537a-187">Azonban kell néhány dolgot az MVC alkalmazás foglalkozik:</span><span class="sxs-lookup"><span data-stu-id="d537a-187">However, you do need to deal with a few things in this MVC application:</span></span>

* <span data-ttu-id="d537a-188">Ha egy felhasználó valamit a keresőmezőbe INTERCEPT</span><span class="sxs-lookup"><span data-stu-id="d537a-188">Intercept when a user types something in the search field</span></span>
* <span data-ttu-id="d537a-189">Az adatok objektum biztosítanak a MasterView vissza a, így azt jelenítheti meg az eredményeket a rács</span><span class="sxs-lookup"><span data-stu-id="d537a-189">Provide an object of data back to the MasterView so it can display the results in the grid</span></span>

<span data-ttu-id="d537a-190">Igazolnia kell végeznie ezeket az alábbi.</span><span class="sxs-lookup"><span data-stu-id="d537a-190">We'll do those below.</span></span>

### <a name="add-a-check-to-see-if-youre-logged-in"></a><span data-ttu-id="d537a-191">Ellenőrzi, hogy ha jelentkezett be hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d537a-191">Add a check to see if you're logged in</span></span>
<span data-ttu-id="d537a-192">Az alkalmazás a felhasználó nem jelentkezett be, ha kevés teszi, hogy ellenőrizze, hogy már jogkivonat a gyorsítótárban való intelligens.</span><span class="sxs-lookup"><span data-stu-id="d537a-192">The application does little if the user is not signed in, so it's smart to check if there is already a token in the cache.</span></span> <span data-ttu-id="d537a-193">Ha nem, akkor jelentkezhet be a felhasználó a LoginView átirányítja.</span><span class="sxs-lookup"><span data-stu-id="d537a-193">If not, you redirect to the LoginView for the user to sign in.</span></span> <span data-ttu-id="d537a-194">Emlékezzen vissza, ha az ajánlott műveletek nézet betöltésekor módja használatára a `viewDidLoad()` módszer, amely Apple velünk.</span><span class="sxs-lookup"><span data-stu-id="d537a-194">If you recall, the best way to do actions when a view loads is to use the `viewDidLoad()` method that Apple provides us.</span></span>

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a><span data-ttu-id="d537a-195">A tábla nézet frissítése adatainak fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="d537a-195">Update the Table View when data is received</span></span>
<span data-ttu-id="d537a-196">A Graph API adatokat ad vissza, ha szeretné megjeleníteni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d537a-196">When the Graph API returns data, you need to display the data.</span></span> <span data-ttu-id="d537a-197">Az egyszerűség Ez a tábla frissítéséhez a kódot.</span><span class="sxs-lookup"><span data-stu-id="d537a-197">For simplicity, here is all the code to update the table.</span></span> <span data-ttu-id="d537a-198">Csak az MVC bolierplate kódjában illessze be a megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="d537a-198">You can just paste the right values in your MVC boilerplate code.</span></span>

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a><span data-ttu-id="d537a-199">A Graph API hívható, ha valaki a keresőmezőt hardvermódosításainak</span><span class="sxs-lookup"><span data-stu-id="d537a-199">Provide a way to call the Graph API when someone types in the search field</span></span>
<span data-ttu-id="d537a-200">Ha egy felhasználó egy keresést a lista, a Graph API shove, amely kell.</span><span class="sxs-lookup"><span data-stu-id="d537a-200">When a user types a search in the box, you need to shove that over to the Graph API.</span></span> <span data-ttu-id="d537a-201">A `GraphAPICaller` osztály alábbi kódjában fog létrehozni, amely elválasztja a bemutató a keresési funkciót.</span><span class="sxs-lookup"><span data-stu-id="d537a-201">The `GraphAPICaller` class, which you will build in the following code, separates the lookup functionality from the presentation.</span></span> <span data-ttu-id="d537a-202">Most ideje lefuttatni a kódot, amely a Graph API-hírcsatornák a keresési karaktereket.</span><span class="sxs-lookup"><span data-stu-id="d537a-202">For now, let's write the code that feeds any search characters to the Graph API.</span></span> <span data-ttu-id="d537a-203">A Microsoft ehhez a hívott metódus megadásával `lookupInGraph`, így tovább is azt a keresendő karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="d537a-203">We do this by providing a method called `lookupInGraph`, which takes the string that we want to search for.</span></span>

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a><span data-ttu-id="d537a-204">A Graph API eléréséhez egy segítőosztály írása</span><span class="sxs-lookup"><span data-stu-id="d537a-204">Write a Helper class to access the Graph API</span></span>
<span data-ttu-id="d537a-205">Ez az alkalmazás alapszolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="d537a-205">This is the core of our application.</span></span> <span data-ttu-id="d537a-206">A többi lett beszúrni a kódot az alapértelmezett MVC mintában az Apple-től, mivel Itt írhat kódot lekérdezni a diagram a felhasználó típusokkal, és térjen vissza az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d537a-206">Whereas the rest was inserting code in the default MVC pattern from Apple, here you write code to query the graph as the user types and then return that data.</span></span> <span data-ttu-id="d537a-207">A kód itt látható, és részletesen ismerteti az azt követő.</span><span class="sxs-lookup"><span data-stu-id="d537a-207">Here's the code, and a detailed explanation follows it.</span></span>

### <a name="create-a-new-objective-c-header-file"></a><span data-ttu-id="d537a-208">Hozzon létre egy új Objective C-fejléc fájlt</span><span class="sxs-lookup"><span data-stu-id="d537a-208">Create a new Objective C header file</span></span>
<span data-ttu-id="d537a-209">A fájl neve `GraphAPICaller.h`, és adja hozzá a következő kódot.</span><span class="sxs-lookup"><span data-stu-id="d537a-209">Name the file `GraphAPICaller.h`, and add the following code.</span></span>

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

<span data-ttu-id="d537a-210">Itt láthatja, hogy a megadott metódus lép egy karakterláncot, és egy completionBlock adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d537a-210">Here you see that a specified method takes a string and returns a completionBlock.</span></span> <span data-ttu-id="d537a-211">A completionBlock, akkor előfordulhat, hogy rendelkezik kitalál, frissíteni fogja a tábla azzal, hogy biztosít egy objektum ki van töltve adatok valós idejű, a felhasználó keresi.</span><span class="sxs-lookup"><span data-stu-id="d537a-211">This completionBlock, as you may have guessed, will update the table by providing an object with populated data in real time as the user searches.</span></span>

### <a name="create-a-new-objective-c-file"></a><span data-ttu-id="d537a-212">Hozzon létre egy új Objective C-fájlt</span><span class="sxs-lookup"><span data-stu-id="d537a-212">Create a new Objective C file</span></span>
<span data-ttu-id="d537a-213">A fájl neve `GraphAPICaller.m`, és adja hozzá a következő metódust.</span><span class="sxs-lookup"><span data-stu-id="d537a-213">Name the file `GraphAPICaller.m`, and add the following method.</span></span>

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

<span data-ttu-id="d537a-214">Ezzel a módszerrel részletesen lépjen.</span><span class="sxs-lookup"><span data-stu-id="d537a-214">Let's go through this method in detail.</span></span>

<span data-ttu-id="d537a-215">Ez a kód legfontosabb van a `NXOAuth2Request`, metódust a paramétereket, amelyek már meghatározta a settings.plist fájlban.</span><span class="sxs-lookup"><span data-stu-id="d537a-215">The core of this code is in the `NXOAuth2Request`, method which takes the parameters that you've already defined in the settings.plist file.</span></span>

<span data-ttu-id="d537a-216">Az első lépés a megfelelő Graph API-hívás összeállításához.</span><span class="sxs-lookup"><span data-stu-id="d537a-216">The first step is to construct the right Graph API call.</span></span> <span data-ttu-id="d537a-217">Mivel a hívott `/users`, megadja, hogy a verziót és a Graph API erőforrás hozzáfűzésével.</span><span class="sxs-lookup"><span data-stu-id="d537a-217">Because you are calling `/users`, you specify that by appending it to the Graph API resource along with the version.</span></span> <span data-ttu-id="d537a-218">Az így kell állítania egy külső beállítások fájlba, mivel ezek módosíthatja az API-t fejlődésének.</span><span class="sxs-lookup"><span data-stu-id="d537a-218">It makes sense to put these in an external settings file because these can change as the API evolves.</span></span>

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

<span data-ttu-id="d537a-219">Ezután meg kell adnia a paramétereket, akkor is nyújt a Graph API-hívásnak.</span><span class="sxs-lookup"><span data-stu-id="d537a-219">Next, you need to specify parameters that you will also provide to the Graph API call.</span></span> <span data-ttu-id="d537a-220">Az *nagyon fontos* , hogy nem helyezett a paraméterek az erőforrás-végpont mert, amely az összes nem-URI megfelelő karaktereket futásidőben van törlődik.</span><span class="sxs-lookup"><span data-stu-id="d537a-220">It is *very important* that you do not put the parameters in the resource endpoint because that is scrubbed for all non-URI conforming characters at runtime.</span></span> <span data-ttu-id="d537a-221">Az összes lekérdezés kód meg kell adni a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d537a-221">All query code must be provided in the parameters.</span></span>

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

<span data-ttu-id="d537a-222">Bizonyára észrevette, hogy meghívja a `convertParamsToDictionary` módszer, amely még nem írt.</span><span class="sxs-lookup"><span data-stu-id="d537a-222">You might notice this calls a `convertParamsToDictionary` method that you haven't written yet.</span></span> <span data-ttu-id="d537a-223">Tekintsük át ezzel a fájl végén:</span><span class="sxs-lookup"><span data-stu-id="d537a-223">Let's do so now at the end of the file:</span></span>

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
<span data-ttu-id="d537a-224">A következő most használja az `NXOAuth2Request` módszer segítségével adatokat vissza JSON formátumban API.</span><span class="sxs-lookup"><span data-stu-id="d537a-224">Next, let's use the `NXOAuth2Request` method to get data back from the API in JSON format.</span></span>

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

<span data-ttu-id="d537a-225">Végezetül nézzük hogyan visszatér az adatokat a MasterViewController.</span><span class="sxs-lookup"><span data-stu-id="d537a-225">Finally, let's look at how you return the data to the MasterViewController.</span></span> <span data-ttu-id="d537a-226">Az adatokat adja vissza, mert a szerializált, és kell deszerializálni és betöltött olyan objektum, amely a MainViewController felhasználhat.</span><span class="sxs-lookup"><span data-stu-id="d537a-226">The data returns as serialized and needs to be deserialized and loaded in an object that the MainViewController can consume.</span></span> <span data-ttu-id="d537a-227">Erre a célra a vázat tartalmaz egy `User.m/h` fájlt, amely a felhasználó-objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d537a-227">For this purpose, the skeleton has a `User.m/h` file that creates a User object.</span></span> <span data-ttu-id="d537a-228">Feltölti az adott felhasználói objektum, a graph származó információkkal.</span><span class="sxs-lookup"><span data-stu-id="d537a-228">You populate that User object with information from the graph.</span></span>

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a><span data-ttu-id="d537a-229">A minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="d537a-229">Run the sample</span></span>
<span data-ttu-id="d537a-230">Ha követte a forgatókönyv az alkalmazás most már működik együtt vagy használt a vázat.</span><span class="sxs-lookup"><span data-stu-id="d537a-230">If you've used the skeleton or followed along with the walkthrough your application should now run.</span></span> <span data-ttu-id="d537a-231">Indítsa el a szimulátor, és kattintson a **bejelentkezés** használni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d537a-231">Start the simulator and click **Sign in** to use the application.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="d537a-232">A termék biztonsági frissítések beszerzése</span><span class="sxs-lookup"><span data-stu-id="d537a-232">Get security updates for our product</span></span>
<span data-ttu-id="d537a-233">Javasoljuk, hogy kérjen értesítést a bekövetkező biztonsági incidensekről látogasson el a [biztonsági TechCenter](https://technet.microsoft.com/security/dd252948) és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="d537a-233">We encourage you to get notifications of when security incidents occur by visiting the [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

