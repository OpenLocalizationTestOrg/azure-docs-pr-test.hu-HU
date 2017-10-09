---
title: "az iOS-alkalmazások az Azure AD aaaIntegrate |} Microsoft Docs"
description: "Hogyan toobuild, amely az Azure AD bejelentkezési és a hívások Azure AD számára az iOS-alkalmazás OAuth használatával védett API-k."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 6e05745b2b2b122995dcba896ab0f2ed32509e3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a><span data-ttu-id="767a2-103">Az Azure AD integrálása az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="767a2-103">Integrate Azure AD into an iOS app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="767a2-104">Próbálja meg az új hello előnézete [fejlesztői portálján](https://identity.microsoft.com/Docs/iOS) , amely segít, amelyekből megismerheti az Azure Active Directoryval csak néhány perc múlva!</span><span class="sxs-lookup"><span data-stu-id="767a2-104">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure Active Directory in just a few minutes!</span></span>  <span data-ttu-id="767a2-105">hello fejlesztői portálján végigvezeti hello regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.</span><span class="sxs-lookup"><span data-stu-id="767a2-105">hello developer portal guides you through hello process of registering an app and integrating Azure AD into your code.</span></span>  <span data-ttu-id="767a2-106">Amikor elkészült, akkor kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók számára a bérlő és a háttérkiszolgáló fogadni és-ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="767a2-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a backend that can accept tokens and perform validation.</span></span> 
> 
> 

<span data-ttu-id="767a2-107">Azure Active Directory (Azure AD) biztosít hello Active Directory Authentication Library vagy adal-t, iOS-ügyfelek számára, amelyek tooaccess védett erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="767a2-107">Azure Active Directory (Azure AD) provides hello Active Directory Authentication Library, or ADAL, for iOS clients that need tooaccess protected resources.</span></span> <span data-ttu-id="767a2-108">Adal-t, hogy alkalmazása használja-e tooobtain hozzáférési jogkivonatok hello folyamat egyszerűbbé teszi.</span><span class="sxs-lookup"><span data-stu-id="767a2-108">ADAL simplifies hello process that your app uses tooobtain access tokens.</span></span> <span data-ttu-id="767a2-109">milyen egyszerűen van, ez a cikk toodemonstrate Objective C feladatlista alkalmazás létrehozásához azt:</span><span class="sxs-lookup"><span data-stu-id="767a2-109">toodemonstrate how easy it is, in this article we build an Objective C To-Do List application that:</span></span>

* <span data-ttu-id="767a2-110">Lekérdezi hozzáférési jogkivonatainak hello Azure AD Graph API felület meghívásakor hello segítségével [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="767a2-110">Gets access tokens for calling hello Azure AD Graph API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="767a2-111">Egy könyvtárat a felhasználók egy adott aliasnév keres.</span><span class="sxs-lookup"><span data-stu-id="767a2-111">Searches a directory for users with a given alias.</span></span>

<span data-ttu-id="767a2-112">toobuild hello teljes működő alkalmazást kell:</span><span class="sxs-lookup"><span data-stu-id="767a2-112">toobuild hello complete working application, you need to:</span></span>

1. <span data-ttu-id="767a2-113">Az alkalmazás regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="767a2-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="767a2-114">Telepítse és konfigurálja az adal-t.</span><span class="sxs-lookup"><span data-stu-id="767a2-114">Install and configure ADAL.</span></span>
3. <span data-ttu-id="767a2-115">Használja az Azure AD ADAL tooget jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="767a2-115">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="767a2-116">elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="767a2-116">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span> <span data-ttu-id="767a2-117">Akkor is, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="767a2-117">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="767a2-118">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="767a2-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>


> [!TIP]
> <span data-ttu-id="767a2-119">Próbálja meg az új hello előnézete [fejlesztői portálján](https://identity.microsoft.com/Docs/iOS) , amely segít, amelyekből megismerheti az Azure AD csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="767a2-119">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="767a2-120">hello fejlesztői portálján végigvezeti hello regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.</span><span class="sxs-lookup"><span data-stu-id="767a2-120">hello developer portal guides you through hello process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="767a2-121">Ha végzett, konfigurálnia kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók az Ön bérlőjében, és egy háttérhálózatként, hogy fogadni, és -ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="767a2-121">When you’re finished, you'll have a simple application that can authenticate users in your tenant, and a back end that can accept tokens and perform validation.</span></span> 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a><span data-ttu-id="767a2-122">1. Határozza meg, milyen az átirányítási URI megadása iOS-hez</span><span class="sxs-lookup"><span data-stu-id="767a2-122">1. Determine what your redirect URI is for iOS</span></span>
<span data-ttu-id="767a2-123">toosecurely bizonyos SSO forgatókönyvekben indítsa el az alkalmazásokat, és létre kell hoznia egy *átirányítási URI* adott formátumban.</span><span class="sxs-lookup"><span data-stu-id="767a2-123">toosecurely start your applications in certain SSO scenarios, you must create a *redirect URI* in a particular format.</span></span> <span data-ttu-id="767a2-124">Egy átirányítási URI megadása, amelyek jogkivonatok visszatérési toohello megfelelő alkalmazás számukra feltett hello használt tooensure.</span><span class="sxs-lookup"><span data-stu-id="767a2-124">A redirect URI is used tooensure that hello tokens return toohello correct application that asked for them.</span></span>


<span data-ttu-id="767a2-125">hello iOS egy átirányítási URI formátuma:</span><span class="sxs-lookup"><span data-stu-id="767a2-125">hello iOS format for a redirect URI is:</span></span>

```
<app-scheme>://<bundle-id>
```

* <span data-ttu-id="767a2-126">**alkalmazás-séma** -ez regisztrálva van az XCode-projektjéhez.</span><span class="sxs-lookup"><span data-stu-id="767a2-126">**app-scheme** - This is registered in your XCode project.</span></span> <span data-ttu-id="767a2-127">Fontos, hogy milyen más alkalmazások hívhatják meg.</span><span class="sxs-lookup"><span data-stu-id="767a2-127">It is how other applications can call you.</span></span> <span data-ttu-id="767a2-128">Ez az Info.plist -> található URL-cím típusú -> URL-cím azonosítója.</span><span class="sxs-lookup"><span data-stu-id="767a2-128">You can find this under Info.plist -> URL types -> URL Identifier.</span></span> <span data-ttu-id="767a2-129">Ha még nem rendelkezik legalább egy konfigurálni kell létrehoznia egyet.</span><span class="sxs-lookup"><span data-stu-id="767a2-129">You should create one if you don't already have one or more configured.</span></span>
* <span data-ttu-id="767a2-130">**Csomagazonosító** -Ez az hello Bundle Identifier "azonosító" alatt található megszünteti a projektbeállításokat, az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="767a2-130">**bundle-id** - This is hello Bundle Identifier found under "identity" un your project settings in XCode.</span></span>

<span data-ttu-id="767a2-131">A kód gyors üzembe helyezési példa: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span><span class="sxs-lookup"><span data-stu-id="767a2-131">An example for this QuickStart code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span></span>

## <a name="2-register-hello-directorysearcher-application"></a><span data-ttu-id="767a2-132">2. Hello DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="767a2-132">2. Register hello DirectorySearcher application</span></span>
<span data-ttu-id="767a2-133">tooset be az alkalmazás tooget jogkivonatokat, először tooregister azt az Azure ad bérlői, és adja meg az engedélyt tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="767a2-133">tooset up your app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="767a2-134">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="767a2-134">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="767a2-135">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="767a2-135">On hello top bar, click your account.</span></span> <span data-ttu-id="767a2-136">A hello **Directory** menüben válassza ki a kívánt tooregister hello Active Directory-bérlőt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="767a2-136">Under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="767a2-137">Kattintson a **több szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="767a2-137">Click **More Services** in hello leftmost navigation pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="767a2-138">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="767a2-138">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="767a2-139">Hajtsa végre a hello kérni fogja az új toocreate **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="767a2-139">Follow hello prompts toocreate a new **Native Client Application**.</span></span>
  * <span data-ttu-id="767a2-140">Hello **neve** hello az alkalmazás az alkalmazás tooend felhasználók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="767a2-140">hello **Name** of hello application describes your application tooend users.</span></span>
  * <span data-ttu-id="767a2-141">Hello **átirányítási Uri-** , hogy az Azure AD által használt tooreturn token válaszok protokollt és a karakterlánc kombinációját.</span><span class="sxs-lookup"><span data-stu-id="767a2-141">hello **Redirect Uri** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span>  <span data-ttu-id="767a2-142">Adjon meg egy értéket, amely adott tooyour alkalmazás, és hello előző átirányítási URI információkon alapul.</span><span class="sxs-lookup"><span data-stu-id="767a2-142">Enter a value that is specific tooyour application and is based on hello previous redirect URI information.</span></span>
6. <span data-ttu-id="767a2-143">Hello regisztráció befejezését követően az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="767a2-143">After you've completed hello registration, Azure AD assigns your app a unique application ID.</span></span>  <span data-ttu-id="767a2-144">Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="767a2-144">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="767a2-145">A hello **beállítások** lapon jelölje be **szükséges engedélyek** , és válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="767a2-145">From hello **Settings** page, select **Required Permissions** and then select **Add**.</span></span> <span data-ttu-id="767a2-146">Válassza ki **Microsoft Graph** hello API-t, majd adja hozzá a hello **címtáradatok olvasása** engedélyt a **delegált engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="767a2-146">Select **Microsoft Graph** as hello API, and then add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="767a2-147">Az alkalmazás tooquery hello Azure AD Graph API a felhasználók állít be.</span><span class="sxs-lookup"><span data-stu-id="767a2-147">This sets up your application tooquery hello Azure AD Graph API for users.</span></span>

## <a name="3-install-and-configure-adal"></a><span data-ttu-id="767a2-148">3. Telepítse és konfigurálja az adal-t</span><span class="sxs-lookup"><span data-stu-id="767a2-148">3. Install and configure ADAL</span></span>
<span data-ttu-id="767a2-149">Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="767a2-149">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="767a2-150">Az Azure ad-val ADAL toocommunicate tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="767a2-150">For ADAL toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

1. <span data-ttu-id="767a2-151">Kezdje az ADAL toohello DirectorySearcher projekt hozzáadása a CocoaPods segítségével.</span><span class="sxs-lookup"><span data-stu-id="767a2-151">Begin by adding ADAL toohello DirectorySearcher project by using CocoaPods.</span></span>

    ```
    $ vi Podfile
    ```
2. <span data-ttu-id="767a2-152">Adja hozzá a következő toothis podfile hello:</span><span class="sxs-lookup"><span data-stu-id="767a2-152">Add hello following toothis podfile:</span></span>

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. <span data-ttu-id="767a2-153">Most töltse be hello podfile CocoaPods segítségével.</span><span class="sxs-lookup"><span data-stu-id="767a2-153">Now load hello podfile by using CocoaPods.</span></span> <span data-ttu-id="767a2-154">Ebben a lépésben létrehoz egy új XCode-munkaterületet, hogy betöltve.</span><span class="sxs-lookup"><span data-stu-id="767a2-154">This step creates a new XCode workspace that you load.</span></span>

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. <span data-ttu-id="767a2-155">Hello Gyorsútmutató-projekt, nyissa meg hello plist-fájlt `settings.plist`.</span><span class="sxs-lookup"><span data-stu-id="767a2-155">In hello QuickStart project, open hello plist file `settings.plist`.</span></span>  <span data-ttu-id="767a2-156">Hello értékek hello elemeinek hello szakasz tooreflect hello értékek hello Azure-portálon megadott helyett.</span><span class="sxs-lookup"><span data-stu-id="767a2-156">Replace hello values of hello elements in hello section tooreflect hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="767a2-157">A kód hivatkozik ezeket az értékeket, minden alkalommal adal-t.</span><span class="sxs-lookup"><span data-stu-id="767a2-157">Your code references these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="767a2-158">Hello `tenant` hello tartománya, akkor az Azure AD-bérlőn, például a contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="767a2-158">hello `tenant` is hello domain of your Azure AD tenant, for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="767a2-159">Hello `clientId` hello ügyfél-azonosító az alkalmazás hello portálról másolt.</span><span class="sxs-lookup"><span data-stu-id="767a2-159">hello `clientId` is hello client ID of your application that you copied from hello portal.</span></span>
  * <span data-ttu-id="767a2-160">Hello `redirectUri` hello átirányítási URL-cím hello portálon regisztrált.</span><span class="sxs-lookup"><span data-stu-id="767a2-160">hello `redirectUri` is hello redirect URL that you registered in hello portal.</span></span>

## <a name="4----use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="767a2-161">4.    Az Azure AD ADAL tooget jogkivonatok használata</span><span class="sxs-lookup"><span data-stu-id="767a2-161">4.    Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="767a2-162">hello alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja a completionBlock `+(void) getToken : `, és ADAL rest hello.</span><span class="sxs-lookup"><span data-stu-id="767a2-162">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does hello rest.</span></span>  

1. <span data-ttu-id="767a2-163">A hello `QuickStart` projektben nyissa meg `GraphAPICaller.m` , és keresse meg a hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` Megjegyzés hello felső közelében.</span><span class="sxs-lookup"><span data-stu-id="767a2-163">In hello `QuickStart` project, open `GraphAPICaller.m` and locate hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near hello top.</span></span>  <span data-ttu-id="767a2-164">Ez az adott ADAL hello koordináták CompletionBlock, az Azure ad-val, toocommunicate keresztül továbbítja, és mondja el hogyan toocache jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="767a2-164">This is where you pass ADAL hello coordinates through a CompletionBlock, toocommunicate with Azure AD, and tell it how toocache tokens.</span></span>

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy toodisplay hello correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. <span data-ttu-id="767a2-165">Most létre kell toouse a token toosearch hello graph lévő felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="767a2-165">Now we need toouse this token toosearch for users in hello graph.</span></span> <span data-ttu-id="767a2-166">Hello található `// TODO: implement SearchUsersList` megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="767a2-166">Find hello `// TODO: implement SearchUsersList` comment.</span></span> <span data-ttu-id="767a2-167">Ez a módszer egy GET kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="767a2-167">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="767a2-168">tooquery hello Azure AD Graph API-t kell tooinclude egy access_token a hello `Authorization` hello kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="767a2-168">tooquery hello Azure AD Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request.</span></span> <span data-ttu-id="767a2-169">Ez az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="767a2-169">This is where ADAL comes in.</span></span>

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab hello JSON node at hello top tooget our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by hello key name being "value". It really is hello name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

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
         }];

    }

    ```


3. <span data-ttu-id="767a2-170">Ha az alkalmazás kísérel meg jogkivonat meghívásával `getToken(...)`, ADAL tooreturn jogkivonat kísérletek hello felhasználói hitelesítő adatok kérése nélkül.</span><span class="sxs-lookup"><span data-stu-id="767a2-170">When your app requests a token by calling `getToken(...)`, ADAL attempts tooreturn a token without asking hello user for credentials.</span></span>  <span data-ttu-id="767a2-171">Ha ADAL hello felhasználó van szüksége a tooget jogkivonat toosign, azt fogja bejelentkezésnél párbeszédpanel megjelenítése, hello felhasználói hitelesítő adatok összegyűjtése és sikeres hitelesítés után térjen vissza a jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="767a2-171">If ADAL determines that hello user needs toosign in tooget a token, it will display a dialog box for sign-in, collect hello user's credentials, and then return a token after successful authentication.</span></span>  <span data-ttu-id="767a2-172">Ha az adal TÁRAT nem képes tooreturn jogkivonat bármilyen okból, jelez egy `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="767a2-172">If ADAL is not able tooreturn a token for any reason, it throws an `AdalException`.</span></span>

> [!Note] 
> <span data-ttu-id="767a2-173">Hello `AuthenticationResult` objektum tartalmaz egy `tokenCacheStoreItem` objektum, amely lehet, hogy az alkalmazás esetleg használt toocollect hello információkat.</span><span class="sxs-lookup"><span data-stu-id="767a2-173">hello `AuthenticationResult` object contains a `tokenCacheStoreItem` object that can be used toocollect hello information that your app may need.</span></span> <span data-ttu-id="767a2-174">A gyors üzembe helyezés, hello `tokenCacheStoreItem` használt toodetermine akkor, ha a hitelesítés már megtörtént.</span><span class="sxs-lookup"><span data-stu-id="767a2-174">In hello QuickStart, `tokenCacheStoreItem` is used toodetermine if authentication is already done.</span></span>
>
>

## <a name="5-build-and-run-hello-application"></a><span data-ttu-id="767a2-175">5. Hozza létre és hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="767a2-175">5. Build and run hello application</span></span>
<span data-ttu-id="767a2-176">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="767a2-176">Congratulations!</span></span> <span data-ttu-id="767a2-177">Most már rendelkezik egy működő iOS-alkalmazást, amely hitelesíti a felhasználókat, biztonságos webes API-k hívására OAuth 2.0-s, és alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="767a2-177">You now have a working iOS application that can authenticate users, securely call Web APIs by using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="767a2-178">Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.</span><span class="sxs-lookup"><span data-stu-id="767a2-178">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="767a2-179">Indítsa el a gyorsindító alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="767a2-179">Start your QuickStart app, and then sign in with one of those users.</span></span>  <span data-ttu-id="767a2-180">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="767a2-180">Search for other users based on their UPN.</span></span>  <span data-ttu-id="767a2-181">Hello-alkalmazások bezárása, majd indítsa el újból.</span><span class="sxs-lookup"><span data-stu-id="767a2-181">Close hello app, and then start it again.</span></span>  <span data-ttu-id="767a2-182">Figyelje meg, hogy hello felhasználói munkamenet érintetlen marad.</span><span class="sxs-lookup"><span data-stu-id="767a2-182">Notice that hello user's session remains intact.</span></span>

<span data-ttu-id="767a2-183">ADAL teszi, hogy könnyen tooincorporate mindezeket a funkciókat az alkalmazásba közös identitás.</span><span class="sxs-lookup"><span data-stu-id="767a2-183">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="767a2-184">Az gondoskodik összes hello dirty munkahelyi meg, mint a gyorsítótár-felügyelet OAuth protokoll támogatása, és a felhasználói felület toosign a hello felhasználói bemutató, és lejárt jogkivonatok frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="767a2-184">It takes care of all hello dirty work for you, like cache management, OAuth protocol support, presenting hello user with a UI toosign in, and refreshing expired tokens.</span></span>  <span data-ttu-id="767a2-185">Valóban tooknow szüksége egy egyetlen API-hívással `getToken`.</span><span class="sxs-lookup"><span data-stu-id="767a2-185">All you really need tooknow is a single API call, `getToken`.</span></span>

<span data-ttu-id="767a2-186">A megadott befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="767a2-186">For reference, hello completed sample (without your configuration values) is provided on [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="767a2-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="767a2-187">Next steps</span></span>
<span data-ttu-id="767a2-188">Most már továbbléphet tooadditional forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="767a2-188">You can now move on tooadditional scenarios.</span></span>  <span data-ttu-id="767a2-189">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="767a2-189">You may want tootry:</span></span>

* [<span data-ttu-id="767a2-190">Egy Node.JS webes API-t az Azure ad-vel biztonságos</span><span class="sxs-lookup"><span data-stu-id="767a2-190">Secure a Node.JS Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)
* <span data-ttu-id="767a2-191">Ismerje meg, [hogyan tooenable adal-t használó IOS alkalmazások közötti SSO](active-directory-sso-ios.md)</span><span class="sxs-lookup"><span data-stu-id="767a2-191">Learn [how tooenable cross-app SSO on iOS using ADAL](active-directory-sso-ios.md)</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

