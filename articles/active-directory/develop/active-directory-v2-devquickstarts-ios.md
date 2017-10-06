---
title: "az Azure AD v2.0-végponttól aaaAdd bejelentkezési tooan iOS alkalmazás használatával hello |} Microsoft Docs"
description: "Hogyan toobuild egy iOS-alkalmazást, amely jelentkezik be mindkét személyes Microsoft-fiókkal rendelkező felhasználók és a munkahelyi vagy iskolai fiókok külső könyvtárak használatával."
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
ms.openlocfilehash: a384062e6e4bd398a2b12318800728e627e05c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-ios-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a><span data-ttu-id="4f193-103">Egy külső könyvtár használata a Graph API segítségével hello v2.0-végponttól bejelentkezési tooan iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4f193-103">Add sign-in tooan iOS app using a third-party library with Graph API using hello v2.0 endpoint</span></span>
<span data-ttu-id="4f193-104">hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja.</span><span class="sxs-lookup"><span data-stu-id="4f193-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="4f193-105">A fejlesztők a kívánják a szolgáltatások toointegrate függvénytárat.</span><span class="sxs-lookup"><span data-stu-id="4f193-105">Developers can use any library they want toointegrate with our services.</span></span> <span data-ttu-id="4f193-106">toohelp fejlesztők más könyvtárakkal a platformot használ, azt korábban írt néhány forgatókönyvek például a egy toodemonstrate hogyan tooconfigure külső szalagtárak tooconnect toohello Microsoft identitásplatformmal.</span><span class="sxs-lookup"><span data-stu-id="4f193-106">toohelp developers use our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure third-party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="4f193-107">A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) toohello Microsoft identitásplatformmal kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="4f193-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect toohello Microsoft identity platform.</span></span>

<span data-ttu-id="4f193-108">Ez a forgatókönyv létrehozó hello alkalmazást, a felhasználók tootheir szervezet bejelentkezhet és majd keresse meg a többi a szervezetek hello Graph API segítségével.</span><span class="sxs-lookup"><span data-stu-id="4f193-108">With hello application that this walkthrough creates, users can sign in tootheir organization and then search for others in their organization by using hello Graph API.</span></span>

<span data-ttu-id="4f193-109">Ha új tooOAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem tehetik logika tooyou.</span><span class="sxs-lookup"><span data-stu-id="4f193-109">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make sense tooyou.</span></span> <span data-ttu-id="4f193-110">Azt javasoljuk, hogy olvassa el [v2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md) a háttérben.</span><span class="sxs-lookup"><span data-stu-id="4f193-110">We recommend that you read  [v2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="4f193-111">Egyes szolgáltatások, amelyek rendelkeznek egy kifejezés hello OAuth2 vagy OpenID Connect szabványok, például a feltételes hozzáférés és a Csoportházirend kezelése Intune-ban, a platform szükség akkor toouse a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú.</span><span class="sxs-lookup"><span data-stu-id="4f193-111">Some features of our platform that do have an expression in hello OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you toouse our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="4f193-112">hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók.</span><span class="sxs-lookup"><span data-stu-id="4f193-112">hello v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="4f193-113">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="4f193-113">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-code-from-github"></a><span data-ttu-id="4f193-114">Töltse le a kód a Githubról</span><span class="sxs-lookup"><span data-stu-id="4f193-114">Download code from GitHub</span></span>
<span data-ttu-id="4f193-115">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span><span class="sxs-lookup"><span data-stu-id="4f193-115">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span></span>  <span data-ttu-id="4f193-116">toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="4f193-116">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

<span data-ttu-id="4f193-117">Csak is letöltheti hello mintát, és rögtön használatba:</span><span class="sxs-lookup"><span data-stu-id="4f193-117">You can also just download hello sample and get started right away:</span></span>

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="4f193-118">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="4f193-118">Register an app</span></span>
<span data-ttu-id="4f193-119">Hozzon létre egy új alkalmazást hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a részletes lépésekről hello [hogyan tooregister egy alkalmazást a v2.0-végponttól hello](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="4f193-119">Create a new app at hello [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow hello detailed steps at  [How tooregister an app with hello v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="4f193-120">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="4f193-120">Make sure to:</span></span>

* <span data-ttu-id="4f193-121">Másolás hello **alkalmazásazonosító** , amely hozzárendelt tooyour app, mert hamarosan kell.</span><span class="sxs-lookup"><span data-stu-id="4f193-121">Copy hello **Application Id** that's assigned tooyour app because you'll need it soon.</span></span>
* <span data-ttu-id="4f193-122">Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="4f193-122">Add hello **Mobile** platform for your app.</span></span>
* <span data-ttu-id="4f193-123">Másolás hello **átirányítási URI-** hello portálról.</span><span class="sxs-lookup"><span data-stu-id="4f193-123">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="4f193-124">Hello alapértelmezett értékét kell használnia `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="4f193-124">You must use hello default value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="download-hello-third-party-nxoauth2-library-and-create-a-workspace"></a><span data-ttu-id="4f193-125">Töltse le a hello külső NXOAuth2 könyvtárban, és egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f193-125">Download hello third-party NXOAuth2 library and create a workspace</span></span>
<span data-ttu-id="4f193-126">A forgatókönyv a Githubból, amely a Mac OS X és az iOS rendszerhez (Cocoa és Cocoa touch) OAuth2 könyvtár OAuth2Client hello fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4f193-126">For this walkthrough, you will use hello OAuth2Client from GitHub, which is an OAuth2 library for Mac OS X and iOS (Cocoa and Cocoa touch).</span></span> <span data-ttu-id="4f193-127">Ezt a szalagtárat vázlat 10 a hello OAuth2 spec alapul. Hello natív alkalmazásprofil valósítja meg, és támogatja a hello engedélyezési végpont hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4f193-127">This library is based on draft 10 of hello OAuth2 spec. It implements hello native application profile and supports hello authorization endpoint of hello user.</span></span> <span data-ttu-id="4f193-128">Az alábbiakban összes hello hello Microsoft identitásplatformmal együttműködve toointegrate lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="4f193-128">These are all hello things you'll need toointegrate with hello Microsoft identity platform.</span></span>

### <a name="add-hello-library-tooyour-project-by-using-cocoapods"></a><span data-ttu-id="4f193-129">Hello könyvtár tooyour projekt hozzáadása a CocoaPods segítségével</span><span class="sxs-lookup"><span data-stu-id="4f193-129">Add hello library tooyour project by using CocoaPods</span></span>
<span data-ttu-id="4f193-130">A CocoaPods egy Xcode-projektekhez készült függőségkezelő.</span><span class="sxs-lookup"><span data-stu-id="4f193-130">CocoaPods is a dependency manager for Xcode projects.</span></span> <span data-ttu-id="4f193-131">Hello korábbi telepítési lépéseket automatikusan kezeli.</span><span class="sxs-lookup"><span data-stu-id="4f193-131">It manages hello previous installation steps automatically.</span></span>

```
$ vi Podfile
```
1. <span data-ttu-id="4f193-132">Adja hozzá a következő toothis podfile hello:</span><span class="sxs-lookup"><span data-stu-id="4f193-132">Add hello following toothis podfile:</span></span>
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. <span data-ttu-id="4f193-133">Hello podfile CocoaPods segítségével töltse be.</span><span class="sxs-lookup"><span data-stu-id="4f193-133">Load hello podfile by using CocoaPods.</span></span> <span data-ttu-id="4f193-134">Ezzel létrehoz egy új Xcode-munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="4f193-134">This will create a new Xcode workspace that you will load.</span></span>
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-hello-structure-of-hello-project"></a><span data-ttu-id="4f193-135">Fedezze fel hello projekt hello szerkezete</span><span class="sxs-lookup"><span data-stu-id="4f193-135">Explore hello structure of hello project</span></span>
<span data-ttu-id="4f193-136">a következő struktúra hello a projektre a hello vázat be van állítva:</span><span class="sxs-lookup"><span data-stu-id="4f193-136">hello following structure is set up for our project in hello skeleton:</span></span>

* <span data-ttu-id="4f193-137">Egy nézet egy egyszerű keresés</span><span class="sxs-lookup"><span data-stu-id="4f193-137">A Master View with a UPN Search</span></span>
* <span data-ttu-id="4f193-138">A részletes nézet hello adatok hello kiválasztott felhasználóról</span><span class="sxs-lookup"><span data-stu-id="4f193-138">A Detail View for hello data about hello selected user</span></span>
* <span data-ttu-id="4f193-139">Ha egy felhasználó bejelentkezhessen a toohello app tooquery hello graph bejelentkezési nézet</span><span class="sxs-lookup"><span data-stu-id="4f193-139">A Login View where a user can sign in toohello app tooquery hello graph</span></span>

<span data-ttu-id="4f193-140">Hello üres tooadd hitelesítési toovarious fájlok áthelyezi azt.</span><span class="sxs-lookup"><span data-stu-id="4f193-140">We will move toovarious files in hello skeleton tooadd authentication.</span></span> <span data-ttu-id="4f193-141">Más részei hello kódok, például hello visual kód, nem vonatkoznak a tooidentity, de megadta-e meg.</span><span class="sxs-lookup"><span data-stu-id="4f193-141">Other parts of hello code, such as hello visual code, do not pertain tooidentity but are provided for you.</span></span>

## <a name="set-up-hello-settingsplst-file-in-hello-library"></a><span data-ttu-id="4f193-142">Hello settings.plst fájl a könyvtárban hello beállítása</span><span class="sxs-lookup"><span data-stu-id="4f193-142">Set up hello settings.plst file in hello library</span></span>
* <span data-ttu-id="4f193-143">Hello Gyorsútmutató-projekt, nyissa meg hello `settings.plist` fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f193-143">In hello QuickStart project, open hello `settings.plist` file.</span></span> <span data-ttu-id="4f193-144">Cserélje le a hello szakasz tooreflect hello értékek hello Azure-portálon a használt hello elemeinek hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="4f193-144">Replace hello values of hello elements in hello section tooreflect hello values that you used in hello Azure portal.</span></span> <span data-ttu-id="4f193-145">A kód minden alkalommal hello Active Directory Authentication Library hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="4f193-145">Your code will reference these values whenever it uses hello Active Directory Authentication Library.</span></span>
  * <span data-ttu-id="4f193-146">Hello `clientId` hello ügyfél-azonosító az alkalmazás hello portálról másolt.</span><span class="sxs-lookup"><span data-stu-id="4f193-146">hello `clientId` is hello client ID of your application that you copied from hello portal.</span></span>
  * <span data-ttu-id="4f193-147">Hello `redirectUri` hello átirányítási URL-CÍMÉT, hogy a megadott hello portál van.</span><span class="sxs-lookup"><span data-stu-id="4f193-147">hello `redirectUri` is hello redirect URL that hello portal provided.</span></span>

## <a name="set-up-hello-nxoauth2client-library-in-your-loginviewcontroller"></a><span data-ttu-id="4f193-148">A LoginViewController NXOAuth2Client szalagtár hello beállítása</span><span class="sxs-lookup"><span data-stu-id="4f193-148">Set up hello NXOAuth2Client library in your LoginViewController</span></span>
<span data-ttu-id="4f193-149">hello NXOAuth2Client tartalomtárát egyes értékek tooget beállítása.</span><span class="sxs-lookup"><span data-stu-id="4f193-149">hello NXOAuth2Client library requires some values tooget set up.</span></span> <span data-ttu-id="4f193-150">Miután elvégezte ezt a feladatot, hello szerzett be token toocall hello Graph API-t is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4f193-150">After you complete that task, you can use hello acquired token toocall hello Graph API.</span></span> <span data-ttu-id="4f193-151">Mivel `LoginView` fogja meghívni bármikor tooauthenticate szükséges, az így tooput konfigurációs értékeket toothat fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f193-151">Because `LoginView` will be called any time we need tooauthenticate, it makes sense tooput configuration values in toothat file.</span></span>

* <span data-ttu-id="4f193-152">Adjunk néhány értékek toohello `LoginViewController.m` fájl tooset hello környezetben a hitelesítéshez és engedélyezéshez.</span><span class="sxs-lookup"><span data-stu-id="4f193-152">Let's add some values toohello  `LoginViewController.m` file tooset hello context for authentication and authorization.</span></span> <span data-ttu-id="4f193-153">Hello értékek részleteit hello kód kövesse.</span><span class="sxs-lookup"><span data-stu-id="4f193-153">Details about hello values follow hello code.</span></span>
  
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

<span data-ttu-id="4f193-154">Nézzük hello kód részleteit.</span><span class="sxs-lookup"><span data-stu-id="4f193-154">Let's look at details about hello code.</span></span>

<span data-ttu-id="4f193-155">hello első karakterlánca a `scopes`.</span><span class="sxs-lookup"><span data-stu-id="4f193-155">hello first string is for `scopes`.</span></span>  <span data-ttu-id="4f193-156">Hello `User.Read` érték tooread hello alapvető profiladataihoz a bejelentkezett felhasználó hello lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="4f193-156">hello `User.Read` value allows you tooread hello basic profile of hello signed in user.</span></span>

<span data-ttu-id="4f193-157">Minden hello elérhető hatókörök kapcsolatos részletesebb [Microsoft Graph-engedélyhatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="4f193-157">You can learn more about all hello available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="4f193-158">A `authURL`, `loginURL`, `bhh`, és `tokenURL`, korábban megadott hello értékek kell használnia.</span><span class="sxs-lookup"><span data-stu-id="4f193-158">For `authURL`, `loginURL`, `bhh`, and `tokenURL`, you should use hello values provided previously.</span></span> <span data-ttu-id="4f193-159">Ha hello nyílt forráskódú Microsoft Azure identitáskezelési-tárakat használ, azt lekérik a ezeket az adatokat, a metaadat-végpontjához használatával.</span><span class="sxs-lookup"><span data-stu-id="4f193-159">If you use hello open source Microsoft Azure Identity Libraries, we pull this data down for you by using our metadata endpoint.</span></span> <span data-ttu-id="4f193-160">A Microsoft régebben már kötöttek hello rögzített munkáját, ezek az értékek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4f193-160">We've done hello hard work of extracting these values for you.</span></span>

<span data-ttu-id="4f193-161">Hello `keychain` hello tároló, amely NXOAuth2Client könyvtár hello fogja használni a kulcslánc-toostore toocreate a tokenek értéke.</span><span class="sxs-lookup"><span data-stu-id="4f193-161">hello `keychain` value is hello container that hello NXOAuth2Client library will use toocreate a keychain toostore your tokens.</span></span> <span data-ttu-id="4f193-162">Ha szeretné tooget egyszeri bejelentkezés (SSO) számos alkalmazások között, megadhat hello azonos kulcslánc minden, az alkalmazások, és kérje, hogy az Xcode jogosultságok a kulcslánc hello használata.</span><span class="sxs-lookup"><span data-stu-id="4f193-162">If you'd like tooget single sign-on (SSO) across numerous apps, you can specify hello same keychain in each of your applications and request hello use of that keychain in your Xcode entitlements.</span></span> <span data-ttu-id="4f193-163">Ennek a magyarázatát a hello Apple dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="4f193-163">This is explained in hello Apple documentation.</span></span>

<span data-ttu-id="4f193-164">Ezeket az értékeket hello rest szükséges toouse hello könyvtár és helyen hozza létre toocarry értékek toohello környezetben.</span><span class="sxs-lookup"><span data-stu-id="4f193-164">hello rest of these values are required toouse hello library and create places for you toocarry values toohello context.</span></span>

### <a name="create-a-url-cache"></a><span data-ttu-id="4f193-165">Egy URL-gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f193-165">Create a URL cache</span></span>
<span data-ttu-id="4f193-166">Belül `(void)viewDidLoad()`, amely mindig neve után hello nézet be van töltve, hello alábbira primes jelek használata a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="4f193-166">Inside `(void)viewDidLoad()`, which is always called after hello view is loaded, hello following code primes a cache for our use.</span></span>

<span data-ttu-id="4f193-167">Adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="4f193-167">Add hello following code:</span></span>

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

### <a name="create-a-webview-for-sign-in"></a><span data-ttu-id="4f193-168">A bejelentkezés a webes nézet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f193-168">Create a WebView for sign-in</span></span>
<span data-ttu-id="4f193-169">A webes nézet hello felhasználónak további tényező van például SMS szöveges üzenet (Ha be van állítva), vagy hiba üzenetek toohello felhasználói adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4f193-169">A WebView can prompt hello user for additional factors like SMS text message (if configured) or return error messages toohello user.</span></span> <span data-ttu-id="4f193-170">Itt fogja beállított hello webes nézet és majd később írási hello kód toohandle hello visszahívások történjen hello webes nézet hello identitás szolgáltatásokból.</span><span class="sxs-lookup"><span data-stu-id="4f193-170">Here you'll set up hello WebView and then later write hello code toohandle hello callbacks that will happen in hello WebView from hello identity services.</span></span>

```objc
-(void)requestOAuth2Access {
    //toosign in tooMicrosoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate toohello URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-hello-webview-methods-toohandle-authentication"></a><span data-ttu-id="4f193-171">Hello webes nézet módszerek toohandle hitelesítési felülbírálása</span><span class="sxs-lookup"><span data-stu-id="4f193-171">Override hello WebView methods toohandle authentication</span></span>
<span data-ttu-id="4f193-172">tootell hello webes nézet, mi történik, ha a felhasználó számára szükséges toosign a korábban bemutatott, illessze be a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="4f193-172">tootell hello WebView what happens when a user needs toosign in as discussed previously, you can paste hello following code.</span></span>

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get hello auth token from a redirect so we need toohandle that in hello webview.

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

    // hello webview is where all hello communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if hello UIWebView is showing our authorization URL or consent URL, show hello UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read hello Location from hello UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by hello redirect URL we chose toouse from Microsoft APIs
        //continue hello OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-toohandle-hello-result-of-hello-oauth2-request"></a><span data-ttu-id="4f193-173">Kód toohandle hello hello OAuth2 kérelem eredményét írása</span><span class="sxs-lookup"><span data-stu-id="4f193-173">Write code toohandle hello result of hello OAuth2 request</span></span>
<span data-ttu-id="4f193-174">hello alábbira kezelnek hello redirectURL, amely a webes nézet hello ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4f193-174">hello following code will handle hello redirectURL that returns from hello WebView.</span></span> <span data-ttu-id="4f193-175">Ha a hitelesítés nem volt sikeres, hello kódot újra próbálkozik.</span><span class="sxs-lookup"><span data-stu-id="4f193-175">If authentication wasn't successful, hello code will try again.</span></span> <span data-ttu-id="4f193-176">Eközben hello könyvtár hello hiba hello konzolon látható elemek vagy aszinkron módon kezelésére ad meg.</span><span class="sxs-lookup"><span data-stu-id="4f193-176">Meanwhile, hello library will provide hello error that you can see in hello console or handle asynchronously.</span></span>

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse hello response for success or failure
     if (accessResult)
    //if success, complete hello OAuth2 flow by handling hello redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-hello-oauth-context-called-account-store"></a><span data-ttu-id="4f193-177">Hello (néven fióktároló) OAuth környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="4f193-177">Set up hello OAuth Context (called account store)</span></span>
<span data-ttu-id="4f193-178">Itt hívása `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` a hello megosztott fióktároló az egyes szolgáltatásokhoz, amelyet az hello alkalmazás toobe képes tooaccess.</span><span class="sxs-lookup"><span data-stu-id="4f193-178">Here you can call `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` on hello shared account store for each service that you want hello application toobe able tooaccess.</span></span> <span data-ttu-id="4f193-179">hello fiók típusa: egy bizonyos szolgáltatás azonosítóként használt karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4f193-179">hello account type is a string that is used as an identifier for a certain service.</span></span> <span data-ttu-id="4f193-180">Hello Graph API-val érik el, mert hello kód hivatkozik, tooit `"myGraphService"`.</span><span class="sxs-lookup"><span data-stu-id="4f193-180">Because you are accessing hello Graph API, hello code refers tooit as `"myGraphService"`.</span></span> <span data-ttu-id="4f193-181">Majd állítsa be, amely jelzi, ha bármilyen változás hello jogkivonatok megfigyelő.</span><span class="sxs-lookup"><span data-stu-id="4f193-181">You then set up an observer that will tell you when anything changes with hello token.</span></span> <span data-ttu-id="4f193-182">Miután hello jogkivonat beszerzéséhez hello felhasználói hátsó toohello vissza `masterView`.</span><span class="sxs-lookup"><span data-stu-id="4f193-182">After you get hello token, you return hello user back toohello `masterView`.</span></span>

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

## <a name="set-up-hello-master-view-toosearch-and-display-hello-users-from-hello-graph-api"></a><span data-ttu-id="4f193-183">Nézet toosearch hello beállítása és a Graph API hello hello felhasználók megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4f193-183">Set up hello Master View toosearch and display hello users from hello Graph API</span></span>
<span data-ttu-id="4f193-184">Egy Master-vezérlő (MVC) alkalmazást, amely megjeleníti a visszaadott adatok hello rácsban hello nem ez a forgatókönyv hello terjed, és sok online oktatóanyagok azt ismertetik, hogyan toobuild egyet.</span><span class="sxs-lookup"><span data-stu-id="4f193-184">A Master-View-Controller (MVC) app that displays hello returned data in hello grid is beyond hello scope of this walkthrough, and many online tutorials explain how toobuild one.</span></span> <span data-ttu-id="4f193-185">Ez a kód hello üres fájl van.</span><span class="sxs-lookup"><span data-stu-id="4f193-185">All this code is in hello skeleton file.</span></span> <span data-ttu-id="4f193-186">Az MVC alkalmazás néhány dolgot a toodeal szüksége:</span><span class="sxs-lookup"><span data-stu-id="4f193-186">However, you do need toodeal with a few things in this MVC application:</span></span>

* <span data-ttu-id="4f193-187">Ha egy felhasználó egy keresőmezőt hello INTERCEPT</span><span class="sxs-lookup"><span data-stu-id="4f193-187">Intercept when a user types something in hello search field</span></span>
* <span data-ttu-id="4f193-188">Adatok hátsó toohello MasterView objektum biztosítanak, így hello eredmények hello rácsban megjeleníti</span><span class="sxs-lookup"><span data-stu-id="4f193-188">Provide an object of data back toohello MasterView so it can display hello results in hello grid</span></span>

<span data-ttu-id="4f193-189">Igazolnia kell végeznie ezeket az alábbi.</span><span class="sxs-lookup"><span data-stu-id="4f193-189">We'll do those below.</span></span>

### <a name="add-a-check-toosee-if-youre-logged-in"></a><span data-ttu-id="4f193-190">Adja hozzá a jelölőnégyzet toosee, ha jelentkezett be</span><span class="sxs-lookup"><span data-stu-id="4f193-190">Add a check toosee if you're logged in</span></span>
<span data-ttu-id="4f193-191">hello alkalmazás hello felhasználó nem jelentkezett be, ha kevés teszi, hogy az intelligens toocheck Ha már van egy token hello gyorsítótárában.</span><span class="sxs-lookup"><span data-stu-id="4f193-191">hello application does little if hello user is not signed in, so it's smart toocheck if there is already a token in hello cache.</span></span> <span data-ttu-id="4f193-192">Ha nem, a hello felhasználói toosign LoginView toohello irányítja át a.</span><span class="sxs-lookup"><span data-stu-id="4f193-192">If not, you redirect toohello LoginView for hello user toosign in.</span></span> <span data-ttu-id="4f193-193">Emlékezzen vissza, ha hello legjobb módja toodo műveletek nézet betöltésekor-e toouse hello `viewDidLoad()` módszer, amely Apple velünk.</span><span class="sxs-lookup"><span data-stu-id="4f193-193">If you recall, hello best way toodo actions when a view loads is toouse hello `viewDidLoad()` method that Apple provides us.</span></span>

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

### <a name="update-hello-table-view-when-data-is-received"></a><span data-ttu-id="4f193-194">Frissítés hello tábla nézet adatainak fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="4f193-194">Update hello Table View when data is received</span></span>
<span data-ttu-id="4f193-195">A Graph API hello adatait jeleníti meg, úgy kell toodisplay hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f193-195">When hello Graph API returns data, you need toodisplay hello data.</span></span> <span data-ttu-id="4f193-196">Az egyszerűség kedvéért itt található összes hello kód tooupdate hello tábla.</span><span class="sxs-lookup"><span data-stu-id="4f193-196">For simplicity, here is all hello code tooupdate hello table.</span></span> <span data-ttu-id="4f193-197">Csak az MVC bolierplate kódjában illessze be hello megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="4f193-197">You can just paste hello right values in your MVC boilerplate code.</span></span>

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


    // Configure hello cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-toocall-hello-graph-api-when-someone-types-in-hello-search-field"></a><span data-ttu-id="4f193-198">Adjon meg egy módon toocall hello Graph API-val, ha valaki hello keresési mező</span><span class="sxs-lookup"><span data-stu-id="4f193-198">Provide a way toocall hello Graph API when someone types in hello search field</span></span>
<span data-ttu-id="4f193-199">Ha egy felhasználó egy keresési hello mezőben, tooshove kell, a Graph API toohello keresztül.</span><span class="sxs-lookup"><span data-stu-id="4f193-199">When a user types a search in hello box, you need tooshove that over toohello Graph API.</span></span> <span data-ttu-id="4f193-200">Hello `GraphAPICaller` osztályt, amely a következő kód hello fog létrehozni, hello bemutató elválasztja hello keresési funkciót.</span><span class="sxs-lookup"><span data-stu-id="4f193-200">hello `GraphAPICaller` class, which you will build in hello following code, separates hello lookup functionality from hello presentation.</span></span> <span data-ttu-id="4f193-201">Most ideje lefuttatni bármely keresési karakterek toohello Graph API-hírcsatornák hello kódját.</span><span class="sxs-lookup"><span data-stu-id="4f193-201">For now, let's write hello code that feeds any search characters toohello Graph API.</span></span> <span data-ttu-id="4f193-202">A Microsoft ehhez a hívott metódus megadásával `lookupInGraph`, így tovább is, amely azt szeretnénk, ha a toosearch hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4f193-202">We do this by providing a method called `lookupInGraph`, which takes hello string that we want toosearch for.</span></span>

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

## <a name="write-a-helper-class-tooaccess-hello-graph-api"></a><span data-ttu-id="4f193-203">Írás a segítő osztály tooaccess hello Graph API-val</span><span class="sxs-lookup"><span data-stu-id="4f193-203">Write a Helper class tooaccess hello Graph API</span></span>
<span data-ttu-id="4f193-204">Ez a hello mag az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4f193-204">This is hello core of our application.</span></span> <span data-ttu-id="4f193-205">Mivel hello rest lett beszúrva kód hello alapértelmezett MVC mintában az Apple-től, itt írt kódot tooquery hello graph hello felhasználói meg kell adnia, és térjen vissza az adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f193-205">Whereas hello rest was inserting code in hello default MVC pattern from Apple, here you write code tooquery hello graph as hello user types and then return that data.</span></span> <span data-ttu-id="4f193-206">Hello kódot ide, és részletesen ismerteti az azt követő.</span><span class="sxs-lookup"><span data-stu-id="4f193-206">Here's hello code, and a detailed explanation follows it.</span></span>

### <a name="create-a-new-objective-c-header-file"></a><span data-ttu-id="4f193-207">Hozzon létre egy új Objective C-fejléc fájlt</span><span class="sxs-lookup"><span data-stu-id="4f193-207">Create a new Objective C header file</span></span>
<span data-ttu-id="4f193-208">Nevű hello fájl `GraphAPICaller.h`, és adja hozzá a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="4f193-208">Name hello file `GraphAPICaller.h`, and add hello following code.</span></span>

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

<span data-ttu-id="4f193-209">Itt láthatja, hogy a megadott metódus lép egy karakterláncot, és egy completionBlock adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4f193-209">Here you see that a specified method takes a string and returns a completionBlock.</span></span> <span data-ttu-id="4f193-210">A completionBlock, akkor előfordulhat, hogy rendelkezik kitalál, frissíteni fogja hello tábla azáltal objektum feltöltött adatok valós idejű, a felhasználó keresések hello.</span><span class="sxs-lookup"><span data-stu-id="4f193-210">This completionBlock, as you may have guessed, will update hello table by providing an object with populated data in real time as hello user searches.</span></span>

### <a name="create-a-new-objective-c-file"></a><span data-ttu-id="4f193-211">Hozzon létre egy új Objective C-fájlt</span><span class="sxs-lookup"><span data-stu-id="4f193-211">Create a new Objective C file</span></span>
<span data-ttu-id="4f193-212">Nevű hello fájl `GraphAPICaller.m`, és adja hozzá a következő metódus hello.</span><span class="sxs-lookup"><span data-stu-id="4f193-212">Name hello file `GraphAPICaller.m`, and add hello following method.</span></span>

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
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
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

<span data-ttu-id="4f193-213">Ezzel a módszerrel részletesen lépjen.</span><span class="sxs-lookup"><span data-stu-id="4f193-213">Let's go through this method in detail.</span></span>

<span data-ttu-id="4f193-214">Ez a kód hello mag van hello `NXOAuth2Request`, metódust hello paramétereket, amelyek már megadott hello settings.plist fájlban.</span><span class="sxs-lookup"><span data-stu-id="4f193-214">hello core of this code is in hello `NXOAuth2Request`, method which takes hello parameters that you've already defined in hello settings.plist file.</span></span>

<span data-ttu-id="4f193-215">hello első lépéseként tooconstruct hello jobb Graph API-hívás, amely.</span><span class="sxs-lookup"><span data-stu-id="4f193-215">hello first step is tooconstruct hello right Graph API call.</span></span> <span data-ttu-id="4f193-216">Mivel a hívott `/users`, megadja, hogy toohello Graph API erőforrás hello verziójával együtt hozzáfűzésével.</span><span class="sxs-lookup"><span data-stu-id="4f193-216">Because you are calling `/users`, you specify that by appending it toohello Graph API resource along with hello version.</span></span> <span data-ttu-id="4f193-217">Teszi logika tooput egy külső beállítások fájlba, mert ezek hello API fejlődésének módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="4f193-217">It makes sense tooput these in an external settings file because these can change as hello API evolves.</span></span>

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

<span data-ttu-id="4f193-218">A következő lépésben toospecify paraméterek toohello Graph API-hívások is megadja.</span><span class="sxs-lookup"><span data-stu-id="4f193-218">Next, you need toospecify parameters that you will also provide toohello Graph API call.</span></span> <span data-ttu-id="4f193-219">Az *nagyon fontos* , hogy nem helyezett hello paraméterek hello erőforrás végpont mert, amely az összes nem-URI megfelelő karaktereket futásidőben van törlődik.</span><span class="sxs-lookup"><span data-stu-id="4f193-219">It is *very important* that you do not put hello parameters in hello resource endpoint because that is scrubbed for all non-URI conforming characters at runtime.</span></span> <span data-ttu-id="4f193-220">Az összes lekérdezés kód hello paramétereket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4f193-220">All query code must be provided in hello parameters.</span></span>

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

<span data-ttu-id="4f193-221">Bizonyára észrevette, hogy meghívja a `convertParamsToDictionary` módszer, amely még nem írt.</span><span class="sxs-lookup"><span data-stu-id="4f193-221">You might notice this calls a `convertParamsToDictionary` method that you haven't written yet.</span></span> <span data-ttu-id="4f193-222">Tekintsük át most hello fájl hello végén:</span><span class="sxs-lookup"><span data-stu-id="4f193-222">Let's do so now at hello end of hello file:</span></span>

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
<span data-ttu-id="4f193-223">A következő most használja az hello `NXOAuth2Request` metódus tooget adatok biztonsági hello API JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="4f193-223">Next, let's use hello `NXOAuth2Request` method tooget data back from hello API in JSON format.</span></span>

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
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

<span data-ttu-id="4f193-224">Végezetül nézzük hogyan hello adatok toohello MasterViewController adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4f193-224">Finally, let's look at how you return hello data toohello MasterViewController.</span></span> <span data-ttu-id="4f193-225">hello adatokat adja vissza, mert a szerializált kell deszerializálni toobe és töltve az objektum az adott hello MainViewController felhasználhat.</span><span class="sxs-lookup"><span data-stu-id="4f193-225">hello data returns as serialized and needs toobe deserialized and loaded in an object that hello MainViewController can consume.</span></span> <span data-ttu-id="4f193-226">Erre a célra hello vázat tartalmaz egy `User.m/h` fájlt, amely a felhasználó-objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f193-226">For this purpose, hello skeleton has a `User.m/h` file that creates a User object.</span></span> <span data-ttu-id="4f193-227">Feltölti felhasználói objektum hello graph származó információkkal.</span><span class="sxs-lookup"><span data-stu-id="4f193-227">You populate that User object with information from hello graph.</span></span>

```objc
                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
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


## <a name="run-hello-sample"></a><span data-ttu-id="4f193-228">Hello minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="4f193-228">Run hello sample</span></span>
<span data-ttu-id="4f193-229">Ha hello vázat használt vagy együtt hello forgatókönyv követni az alkalmazás most már működik.</span><span class="sxs-lookup"><span data-stu-id="4f193-229">If you've used hello skeleton or followed along with hello walkthrough your application should now run.</span></span> <span data-ttu-id="4f193-230">Hello szimulátorban történő elindításához, és kattintson a **bejelentkezés** toouse hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4f193-230">Start hello simulator and click **Sign in** toouse hello application.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="4f193-231">A termék biztonsági frissítések beszerzése</span><span class="sxs-lookup"><span data-stu-id="4f193-231">Get security updates for our product</span></span>
<span data-ttu-id="4f193-232">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről hello felkeresésével [biztonsági TechCenter](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="4f193-232">We encourage you tooget notifications of when security incidents occur by visiting hello [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

