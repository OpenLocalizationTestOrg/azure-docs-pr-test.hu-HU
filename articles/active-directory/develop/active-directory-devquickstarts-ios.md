---
title: "Az iOS-alkalmazásoknak az Azure AD integrálása |} Microsoft Docs"
description: "Hogyan hozhat létre, amely az Azure AD bejelentkezési és a hívások Azure AD számára az iOS-alkalmazás OAuth használatával védett API-k."
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
ms.openlocfilehash: 57f465df99ac234466459b8031f61805d8334b59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a><span data-ttu-id="90f57-103">Az Azure AD integrálása az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="90f57-103">Integrate Azure AD into an iOS app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="90f57-104">Az új az előzetes kiadás kipróbálásához [fejlesztői portálján](https://identity.microsoft.com/Docs/iOS) , amely segít, amelyekből megismerheti az Azure Active Directoryval csak néhány perc múlva!</span><span class="sxs-lookup"><span data-stu-id="90f57-104">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure Active Directory in just a few minutes!</span></span>  <span data-ttu-id="90f57-105">A fejlesztői portálján végigvezeti a regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.</span><span class="sxs-lookup"><span data-stu-id="90f57-105">The developer portal guides you through the process of registering an app and integrating Azure AD into your code.</span></span>  <span data-ttu-id="90f57-106">Amikor elkészült, akkor kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók számára a bérlő és a háttérkiszolgáló fogadni és-ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="90f57-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a backend that can accept tokens and perform validation.</span></span> 
> 
> 

<span data-ttu-id="90f57-107">Azure Active Directory (Azure AD) biztosítja az Active Directory Authentication Library, vagy az adal-t, iOS-ügyfelek számára, amelyek védett erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="90f57-107">Azure Active Directory (Azure AD) provides the Active Directory Authentication Library, or ADAL, for iOS clients that need to access protected resources.</span></span> <span data-ttu-id="90f57-108">ADAL egyszerűbben alkalmazása használja-e a hozzáférési tokenek beszerzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="90f57-108">ADAL simplifies the process that your app uses to obtain access tokens.</span></span> <span data-ttu-id="90f57-109">Annak bemutatásához, hogyan könnyen van, a cikkben azt Objective C feladatlista alkalmazás létrehozásához, amely:</span><span class="sxs-lookup"><span data-stu-id="90f57-109">To demonstrate how easy it is, in this article we build an Objective C To-Do List application that:</span></span>

* <span data-ttu-id="90f57-110">Lekérdezi az Azure AD Graph API felület meghívásakor használatával jogkivonatainak hozzáférni a [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="90f57-110">Gets access tokens for calling the Azure AD Graph API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="90f57-111">Egy könyvtárat a felhasználók egy adott aliasnév keres.</span><span class="sxs-lookup"><span data-stu-id="90f57-111">Searches a directory for users with a given alias.</span></span>

<span data-ttu-id="90f57-112">A teljes működő alkalmazás létrehozásához kell:</span><span class="sxs-lookup"><span data-stu-id="90f57-112">To build the complete working application, you need to:</span></span>

1. <span data-ttu-id="90f57-113">Az alkalmazás regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="90f57-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="90f57-114">Telepítse és konfigurálja az adal-t.</span><span class="sxs-lookup"><span data-stu-id="90f57-114">Install and configure ADAL.</span></span>
3. <span data-ttu-id="90f57-115">Adal-t használó tokenek lekérni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f57-115">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="90f57-116">A kezdéshez [töltse le az alkalmazás vázat](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="90f57-116">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span> <span data-ttu-id="90f57-117">Akkor is, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="90f57-117">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="90f57-118">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="90f57-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>


> [!TIP]
> <span data-ttu-id="90f57-119">Az új az előzetes kiadás kipróbálásához [fejlesztői portálján](https://identity.microsoft.com/Docs/iOS) , amely segít, amelyekből megismerheti az Azure AD csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="90f57-119">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="90f57-120">A fejlesztői portálján végigvezeti a regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.</span><span class="sxs-lookup"><span data-stu-id="90f57-120">The developer portal guides you through the process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="90f57-121">Ha végzett, konfigurálnia kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók az Ön bérlőjében, és egy háttérhálózatként, hogy fogadni, és -ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="90f57-121">When you’re finished, you'll have a simple application that can authenticate users in your tenant, and a back end that can accept tokens and perform validation.</span></span> 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a><span data-ttu-id="90f57-122">1. Határozza meg, milyen az átirányítási URI megadása iOS-hez</span><span class="sxs-lookup"><span data-stu-id="90f57-122">1. Determine what your redirect URI is for iOS</span></span>
<span data-ttu-id="90f57-123">Biztonságosan indítsa el az alkalmazások bizonyos SSO forgatókönyvekben, létre kell hoznia egy *átirányítási URI* adott formátumban.</span><span class="sxs-lookup"><span data-stu-id="90f57-123">To securely start your applications in certain SSO scenarios, you must create a *redirect URI* in a particular format.</span></span> <span data-ttu-id="90f57-124">Egy átirányítási URI-t annak biztosítására szolgál, hogy a jogkivonatok térjen vissza a megfelelő alkalmazás feltett számukra.</span><span class="sxs-lookup"><span data-stu-id="90f57-124">A redirect URI is used to ensure that the tokens return to the correct application that asked for them.</span></span>


<span data-ttu-id="90f57-125">Az iOS egy átirányítási URI formátuma:</span><span class="sxs-lookup"><span data-stu-id="90f57-125">The iOS format for a redirect URI is:</span></span>

```
<app-scheme>://<bundle-id>
```

* <span data-ttu-id="90f57-126">**alkalmazás-séma** -ez regisztrálva van az XCode-projektjéhez.</span><span class="sxs-lookup"><span data-stu-id="90f57-126">**app-scheme** - This is registered in your XCode project.</span></span> <span data-ttu-id="90f57-127">Fontos, hogy milyen más alkalmazások hívhatják meg.</span><span class="sxs-lookup"><span data-stu-id="90f57-127">It is how other applications can call you.</span></span> <span data-ttu-id="90f57-128">Ez az Info.plist -> található URL-cím típusú -> URL-cím azonosítója.</span><span class="sxs-lookup"><span data-stu-id="90f57-128">You can find this under Info.plist -> URL types -> URL Identifier.</span></span> <span data-ttu-id="90f57-129">Ha még nem rendelkezik legalább egy konfigurálni kell létrehoznia egyet.</span><span class="sxs-lookup"><span data-stu-id="90f57-129">You should create one if you don't already have one or more configured.</span></span>
* <span data-ttu-id="90f57-130">**Csomagazonosító** -Ez az "azonosító" alatt található csomagazonosítót megszünteti a projektbeállításokat, az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="90f57-130">**bundle-id** - This is the Bundle Identifier found under "identity" un your project settings in XCode.</span></span>

<span data-ttu-id="90f57-131">A kód gyors üzembe helyezési példa: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span><span class="sxs-lookup"><span data-stu-id="90f57-131">An example for this QuickStart code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span></span>

## <a name="2-register-the-directorysearcher-application"></a><span data-ttu-id="90f57-132">2. A DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="90f57-132">2. Register the DirectorySearcher application</span></span>
<span data-ttu-id="90f57-133">Az alkalmazás beállítása a jogkivonatok lekérésére, először kell regisztrálni az Azure AD-bérlő, és adja meg azt az engedélyt az Azure AD Graph API eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="90f57-133">To set up your app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="90f57-134">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="90f57-134">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="90f57-135">A felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="90f57-135">On the top bar, click your account.</span></span> <span data-ttu-id="90f57-136">Az a **Directory** menüben válassza ki, hol szeretné az alkalmazás regisztrálása az Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="90f57-136">Under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>
3. <span data-ttu-id="90f57-137">Kattintson a **több szolgáltatások** a bal oldali navigációs ablaktáblán, és válassza ki azt a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="90f57-137">Click **More Services** in the leftmost navigation pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="90f57-138">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="90f57-138">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="90f57-139">Kövesse az utasításokat, hozzon létre egy új **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="90f57-139">Follow the prompts to create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="90f57-140">A **neve** , az alkalmazás írja le az alkalmazást a végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="90f57-140">The **Name** of the application describes your application to end users.</span></span>
  * <span data-ttu-id="90f57-141">A **átirányítási Uri-** protokollt és a karakterlánc kombinációját, amely az Azure AD token válaszok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="90f57-141">The **Redirect Uri** is a scheme and string combination that Azure AD uses to return token responses.</span></span>  <span data-ttu-id="90f57-142">Adjon meg egy értéket, amely az alkalmazásra vonatkoznak, és a korábbi átirányítási URI információkon alapul.</span><span class="sxs-lookup"><span data-stu-id="90f57-142">Enter a value that is specific to your application and is based on the previous redirect URI information.</span></span>
6. <span data-ttu-id="90f57-143">A regisztráció befejezését követően az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="90f57-143">After you've completed the registration, Azure AD assigns your app a unique application ID.</span></span>  <span data-ttu-id="90f57-144">Ez az érték kell a következő szakaszokban lévő, másolja az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="90f57-144">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="90f57-145">A a **beállítások** lapon jelölje be **szükséges engedélyek** majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="90f57-145">From the **Settings** page, select **Required Permissions** and then select **Add**.</span></span> <span data-ttu-id="90f57-146">Válassza ki **Microsoft Graph** , az API-t, majd adja hozzá a **címtáradatok olvasása** engedélyt a **delegált engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="90f57-146">Select **Microsoft Graph** as the API, and then add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="90f57-147">Az alkalmazás a felhasználók számára az Azure AD Graph API lekérdezni állít be.</span><span class="sxs-lookup"><span data-stu-id="90f57-147">This sets up your application to query the Azure AD Graph API for users.</span></span>

## <a name="3-install-and-configure-adal"></a><span data-ttu-id="90f57-148">3. Telepítse és konfigurálja az adal-t</span><span class="sxs-lookup"><span data-stu-id="90f57-148">3. Install and configure ADAL</span></span>
<span data-ttu-id="90f57-149">Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="90f57-149">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="90f57-150">Az adal-t az Azure AD kommunikálni meg kell adnia azt bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="90f57-150">For ADAL to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

1. <span data-ttu-id="90f57-151">Kezdje az adal-t hozzáadása a DirectorySearcher projekt CocoaPods segítségével.</span><span class="sxs-lookup"><span data-stu-id="90f57-151">Begin by adding ADAL to the DirectorySearcher project by using CocoaPods.</span></span>

    ```
    $ vi Podfile
    ```
2. <span data-ttu-id="90f57-152">Adja hozzá a következőt a pod-fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="90f57-152">Add the following to this podfile:</span></span>

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. <span data-ttu-id="90f57-153">Most töltse be a podfile CocoaPods segítségével.</span><span class="sxs-lookup"><span data-stu-id="90f57-153">Now load the podfile by using CocoaPods.</span></span> <span data-ttu-id="90f57-154">Ebben a lépésben létrehoz egy új XCode-munkaterületet, hogy betöltve.</span><span class="sxs-lookup"><span data-stu-id="90f57-154">This step creates a new XCode workspace that you load.</span></span>

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. <span data-ttu-id="90f57-155">A gyors üzembe helyezés projektben nyissa meg a plist-fájlt `settings.plist`.</span><span class="sxs-lookup"><span data-stu-id="90f57-155">In the QuickStart project, open the plist file `settings.plist`.</span></span>  <span data-ttu-id="90f57-156">Cserélje le az értékeket az elemek a szakaszban az Azure-portálon megadott értékeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="90f57-156">Replace the values of the elements in the section to reflect the values that you entered in the Azure portal.</span></span> <span data-ttu-id="90f57-157">A kód hivatkozik ezeket az értékeket, minden alkalommal adal-t.</span><span class="sxs-lookup"><span data-stu-id="90f57-157">Your code references these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="90f57-158">A `tenant` a tartományt az Azure AD-bérlőn, például a contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="90f57-158">The `tenant` is the domain of your Azure AD tenant, for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="90f57-159">A `clientId` a portálról másolt az alkalmazás ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="90f57-159">The `clientId` is the client ID of your application that you copied from the portal.</span></span>
  * <span data-ttu-id="90f57-160">A `redirectUri` az átirányítási URL-cím regisztrált a portálon.</span><span class="sxs-lookup"><span data-stu-id="90f57-160">The `redirectUri` is the redirect URL that you registered in the portal.</span></span>

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="90f57-161">4.    Adal-t használó tokenek lekérni az Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f57-161">4.    Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="90f57-162">Az alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja a completionBlock `+(void) getToken : `, és a többi adal-t.</span><span class="sxs-lookup"><span data-stu-id="90f57-162">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does the rest.</span></span>  

1. <span data-ttu-id="90f57-163">Az a `QuickStart` projektben nyissa meg `GraphAPICaller.m` , és keresse meg a `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` Megjegyzés felső részén.</span><span class="sxs-lookup"><span data-stu-id="90f57-163">In the `QuickStart` project, open `GraphAPICaller.m` and locate the `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near the top.</span></span>  <span data-ttu-id="90f57-164">Ez az adott át adal-t a koordináták a CompletionBlock, az Azure ad-val, és módon gyorsítótárazza a jogkivonatok keresztül.</span><span class="sxs-lookup"><span data-stu-id="90f57-164">This is where you pass ADAL the coordinates through a CompletionBlock, to communicate with Azure AD, and tell it how to cache tokens.</span></span>

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
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
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

2. <span data-ttu-id="90f57-165">Most létre kell a token használata a grafikonon felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="90f57-165">Now we need to use this token to search for users in the graph.</span></span> <span data-ttu-id="90f57-166">Keresés a `// TODO: implement SearchUsersList` megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="90f57-166">Find the `// TODO: implement SearchUsersList` comment.</span></span> <span data-ttu-id="90f57-167">Ez a módszer az Azure AD Graph API lekérdezéshez GET kérés teszi a felhasználók számára, amelyek egyszerű felhasználónév a megadott keresési kifejezés kezdődik.</span><span class="sxs-lookup"><span data-stu-id="90f57-167">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="90f57-168">Az Azure AD Graph API lekérdezni, meg kell adnia egy access_token a a `Authorization` a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="90f57-168">To query the Azure AD Graph API, you need to include an access_token in the `Authorization` header of the request.</span></span> <span data-ttu-id="90f57-169">Ez az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="90f57-169">This is where ADAL comes in.</span></span>

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

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
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


3. <span data-ttu-id="90f57-170">Ha az alkalmazás kísérel meg jogkivonat meghívásával `getToken(...)`, adal-t próbálja elküldeni a jogkivonatot a felhasználói hitelesítő adatok kérése nélkül.</span><span class="sxs-lookup"><span data-stu-id="90f57-170">When your app requests a token by calling `getToken(...)`, ADAL attempts to return a token without asking the user for credentials.</span></span>  <span data-ttu-id="90f57-171">ADAL határozza meg, hogy a felhasználó bejelentkezhet a szolgáltatáshitelesítést egy token kell-e, ha azt fogja bejelentkezésnél párbeszédpanel megjelenítése, a felhasználói hitelesítő adatok összegyűjtése és sikeres hitelesítés után térjen vissza a jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="90f57-171">If ADAL determines that the user needs to sign in to get a token, it will display a dialog box for sign-in, collect the user's credentials, and then return a token after successful authentication.</span></span>  <span data-ttu-id="90f57-172">Ha az adal-t nem tud visszatérni jogkivonat bármilyen okból, jelez egy `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="90f57-172">If ADAL is not able to return a token for any reason, it throws an `AdalException`.</span></span>

> [!Note] 
> <span data-ttu-id="90f57-173">A `AuthenticationResult` objektum tartalmaz egy `tokenCacheStoreItem` objektum, amely a céghez, amely csak az alkalmazás akkor is használható.</span><span class="sxs-lookup"><span data-stu-id="90f57-173">The `AuthenticationResult` object contains a `tokenCacheStoreItem` object that can be used to collect the information that your app may need.</span></span> <span data-ttu-id="90f57-174">A gyorsindító `tokenCacheStoreItem` határozza meg, ha a hitelesítés már megtörtént.</span><span class="sxs-lookup"><span data-stu-id="90f57-174">In the QuickStart, `tokenCacheStoreItem` is used to determine if authentication is already done.</span></span>
>
>

## <a name="5-build-and-run-the-application"></a><span data-ttu-id="90f57-175">5. Az alkalmazás fordítása és futtatása</span><span class="sxs-lookup"><span data-stu-id="90f57-175">5. Build and run the application</span></span>
<span data-ttu-id="90f57-176">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f57-176">Congratulations!</span></span> <span data-ttu-id="90f57-177">Most már rendelkezik egy működő iOS-alkalmazást, amely hitelesíti a felhasználókat, biztonságos webes API-k hívására OAuth 2.0-s, és a felhasználó alapszintű adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="90f57-177">You now have a working iOS application that can authenticate users, securely call Web APIs by using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="90f57-178">Ha még nem tette meg, most már az egyes felhasználóival a bérlő feltölti idő.</span><span class="sxs-lookup"><span data-stu-id="90f57-178">If you haven't already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="90f57-179">Indítsa el a gyorsindító alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="90f57-179">Start your QuickStart app, and then sign in with one of those users.</span></span>  <span data-ttu-id="90f57-180">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="90f57-180">Search for other users based on their UPN.</span></span>  <span data-ttu-id="90f57-181">Zárja be az alkalmazást, majd indítsa el újból.</span><span class="sxs-lookup"><span data-stu-id="90f57-181">Close the app, and then start it again.</span></span>  <span data-ttu-id="90f57-182">Figyelje meg, hogy a felhasználói munkamenet érintetlen marad.</span><span class="sxs-lookup"><span data-stu-id="90f57-182">Notice that the user's session remains intact.</span></span>

<span data-ttu-id="90f57-183">Az adal TÁRAT megkönnyíti, hogy átfogó mindezeket a közös identitás funkciókat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="90f57-183">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="90f57-184">Azt gondoskodik a dirty munkát meg, mint a gyorsítótár-felügyelet OAuth protokoll támogatása, a felhasználó a felhasználói felületen való bejelentkezéshez, bemutató, és a jogkivonatok frissítése lejárt.</span><span class="sxs-lookup"><span data-stu-id="90f57-184">It takes care of all the dirty work for you, like cache management, OAuth protocol support, presenting the user with a UI to sign in, and refreshing expired tokens.</span></span>  <span data-ttu-id="90f57-185">Biztosan tudni, hogy szüksége egy egyetlen API-hívással `getToken`.</span><span class="sxs-lookup"><span data-stu-id="90f57-185">All you really need to know is a single API call, `getToken`.</span></span>

<span data-ttu-id="90f57-186">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként a megadott [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="90f57-186">For reference, the completed sample (without your configuration values) is provided on [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="90f57-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90f57-187">Next steps</span></span>
<span data-ttu-id="90f57-188">Most már továbbléphet további helyzeteket is.</span><span class="sxs-lookup"><span data-stu-id="90f57-188">You can now move on to additional scenarios.</span></span>  <span data-ttu-id="90f57-189">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="90f57-189">You may want to try:</span></span>

* [<span data-ttu-id="90f57-190">Egy Node.JS webes API-t az Azure ad-vel biztonságos</span><span class="sxs-lookup"><span data-stu-id="90f57-190">Secure a Node.JS Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)
* <span data-ttu-id="90f57-191">Ismerje meg, [adal-t használó IOS alkalmazások közötti SSO engedélyezése](active-directory-sso-ios.md)</span><span class="sxs-lookup"><span data-stu-id="90f57-191">Learn [how to enable cross-app SSO on iOS using ADAL](active-directory-sso-ios.md)</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

