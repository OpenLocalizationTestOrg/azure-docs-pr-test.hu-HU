---
title: "IOS alkalmazás - Azure AD B2C segítségével tokenbeolvasás |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan Azure Active Directory B2C AppAuth használ a felhasználói identitások kezelésére, és hitelesíti a felhasználókat egy iOS-alkalmazás létrehozásához."
services: active-directory-b2c
documentationcenter: ios
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: ebec5d910b8987dcc8155cd4ead00f87d219941c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a><span data-ttu-id="17213-103">Az Azure AD B2C: Bejelentkezés használata iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="17213-103">Azure AD B2C: Sign-in using an iOS application</span></span>

<span data-ttu-id="17213-104">A Microsoft identitásplatformja nyílt szabványokat, többek között OAuth2-t és OpenID Connectet használ.</span><span class="sxs-lookup"><span data-stu-id="17213-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="17213-105">További fejlesztői választott egy megnyitott szabványos protokollt használó kínál, a szalagtár szolgáltatások integrálása kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="17213-105">Using an open standard protocol offers more developer choice when selecting a library to integrate with our services.</span></span> <span data-ttu-id="17213-106">Ez a forgatókönyv, míg mások például úgy, hogy segítséget nyújtanak a fejlesztők az alkalmazások a Microsoft Identity platform-hez mellékelt.</span><span class="sxs-lookup"><span data-stu-id="17213-106">We've provided this walkthrough and others like it to aid developers with writing applications that connect to the Microsoft Identity platform.</span></span> <span data-ttu-id="17213-107">A legtöbb tárak, amelyek megvalósítják az [a RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) kapcsolódik a Microsoft Identity platformra.</span><span class="sxs-lookup"><span data-stu-id="17213-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) are able to connect to the Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="17213-108">A Microsoft nem biztosít a külső szalagtárak javítások, és nem végrehajtva a tárak áttekintése.</span><span class="sxs-lookup"><span data-stu-id="17213-108">Microsoft does not provide fixes for third-party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="17213-109">Ez a minta, amely tesztelték AppAuth nevű alapszintű forgatókönyvekben kompatibilisek-e az Azure AD B2C külső szalagtárat használ.</span><span class="sxs-lookup"><span data-stu-id="17213-109">This sample is using a third-party library called AppAuth that has been tested for compatibility in basic scenarios with the Azure AD B2C.</span></span> <span data-ttu-id="17213-110">A könyvtár nyílt forráskódú projekt problémákat és funkciókra vonatkozó kérések legyenek irányítva.</span><span class="sxs-lookup"><span data-stu-id="17213-110">Issues and feature requests should be directed to the library's open-source project.</span></span> <span data-ttu-id="17213-111">További információkért tekintse meg [ezt a cikket](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span><span class="sxs-lookup"><span data-stu-id="17213-111">For more information, see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span></span>
>
>

<span data-ttu-id="17213-112">Ha most ismerkedik az OAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem célszerű sok Önnek.</span><span class="sxs-lookup"><span data-stu-id="17213-112">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make much sense to you.</span></span> <span data-ttu-id="17213-113">Ebben az esetben javasoljuk, hogy olvassa el [a protokoll áttekintését, amelyet itt talál](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="17213-113">We recommend you look at a brief [overview of the protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="17213-114">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="17213-114">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="17213-115">Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="17213-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="17213-116">A könyvtár egy olyan tároló, a felhasználók, alkalmazások, csoportok és több.</span><span class="sxs-lookup"><span data-stu-id="17213-116">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="17213-117">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="17213-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="17213-118">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="17213-118">Create an application</span></span>
<span data-ttu-id="17213-119">A következő lépésben hozzon létre egy alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="17213-119">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="17213-120">Az alkalmazás regisztrálása biztosít az alkalmazás biztonságos kommunikációhoz szükséges információkat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="17213-120">The app registration gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="17213-121">A mobilalkalmazások létrehozásához hajtsa végre az [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="17213-121">To create a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="17213-122">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="17213-122">Be sure to:</span></span>

* <span data-ttu-id="17213-123">Tartalmaznak egy **natív ügyfél** az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="17213-123">Include a **Native client** in the application.</span></span>
* <span data-ttu-id="17213-124">Másolja az alkalmazáshoz rendelt **alkalmazásazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="17213-124">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="17213-125">A GUID később szüksége.</span><span class="sxs-lookup"><span data-stu-id="17213-125">You need this GUID later.</span></span>
* <span data-ttu-id="17213-126">Állítson be egy **átirányítási URI-** egy egyéni séma (például com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="17213-126">Set up a **Redirect URI** with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="17213-127">Ezt az URI később szüksége.</span><span class="sxs-lookup"><span data-stu-id="17213-127">You need this URI later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="17213-128">Szabályzatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="17213-128">Create your policies</span></span>
<span data-ttu-id="17213-129">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="17213-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="17213-130">Az alkalmazás tartalmaz egy identitás-élmény: egy kombinált bejelentkezési és regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="17213-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="17213-131">A házirend létrehozásához lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="17213-131">Create this policy as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="17213-132">A szabályzat létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="17213-132">When you create the policy, be sure to:</span></span>

* <span data-ttu-id="17213-133">A **regisztrációs attribútumokat**, válassza ki az attribútum **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="17213-133">Under **Sign-up attributes**, select the attribute **Display name**.</span></span>  <span data-ttu-id="17213-134">Kiválaszthatja, valamint egyéb attribútumai.</span><span class="sxs-lookup"><span data-stu-id="17213-134">You can select other attributes as well.</span></span>
* <span data-ttu-id="17213-135">A **alkalmazási jogcímet**, válassza ki a jogcímeket **megjelenített név** és **felhasználó Objektumazonosító**.</span><span class="sxs-lookup"><span data-stu-id="17213-135">Under **Application claims**, select the claims **Display name** and **User's Object ID**.</span></span> <span data-ttu-id="17213-136">Kiválaszthatja, hogy más jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="17213-136">You can select other claims as well.</span></span>
* <span data-ttu-id="17213-137">Az egyes házirendek létrehozása után másolja a házirend **nevét**.</span><span class="sxs-lookup"><span data-stu-id="17213-137">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="17213-138">A házirend neve a következő előtaggal van `b2c_1_` amikor menteni a házirendet.</span><span class="sxs-lookup"><span data-stu-id="17213-138">Your policy name is prefixed with `b2c_1_` when you save the policy.</span></span>  <span data-ttu-id="17213-139">A házirend nevére később szüksége.</span><span class="sxs-lookup"><span data-stu-id="17213-139">You need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="17213-140">A szabályzat létrehozását követően készen áll az alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="17213-140">After you have created your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="17213-141">A mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="17213-141">Download the sample code</span></span>
<span data-ttu-id="17213-142">Egy Azure AD B2C AppAuth használó minta adtunk [a Githubon](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="17213-142">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="17213-143">Töltse le a kódot, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="17213-143">You can download the code and run it.</span></span> <span data-ttu-id="17213-144">A saját Azure AD B2C bérlő használatához kövesse az utasításokat a [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="17213-144">To use your own Azure AD B2C tenant, follow the instructions in the [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="17213-145">Ez a minta információs útmutatásai szerint hozta létre a [iOS AppAuth-projektre a Githubon](https://github.com/openid/AppAuth-iOS).</span><span class="sxs-lookup"><span data-stu-id="17213-145">This sample was created by following the README instructions by the [iOS AppAuth project on GitHub](https://github.com/openid/AppAuth-iOS).</span></span> <span data-ttu-id="17213-146">További részletek a minta és a szalagtár működése lásd a AppAuth Readme fájlt a Githubon.</span><span class="sxs-lookup"><span data-stu-id="17213-146">For more details on how the sample and the library work, reference the AppAuth README on GitHub.</span></span>

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a><span data-ttu-id="17213-147">Az alkalmazás az Azure AD B2C használata AppAuth módosítása</span><span class="sxs-lookup"><span data-stu-id="17213-147">Modifying your app to use Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="17213-148">AppAuth támogatja az iOS 7-es vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="17213-148">AppAuth supports iOS 7 and above.</span></span>  <span data-ttu-id="17213-149">Azonban Google közösségi bejelentkezések támogatásához SFSafariViewController szükséges ehhez a iOS 9-es vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="17213-149">However, to support social logins on Google, SFSafariViewController is needed which requires iOS 9 or higher.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="17213-150">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="17213-150">Configuration</span></span>

<span data-ttu-id="17213-151">Az Azure AD B2C kommunikációs konfigurálható az engedélyezési végpont és a token-végpont URI-azonosítók megadásával.</span><span class="sxs-lookup"><span data-stu-id="17213-151">You can configure communication with Azure AD B2C by specifying both the authorization endpoint and token endpoint URIs.</span></span>  <span data-ttu-id="17213-152">Az URI-k létrehozásához szüksége van a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="17213-152">To generate these URIs, you need the following information:</span></span>
* <span data-ttu-id="17213-153">Bérlő azonosítója (például contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="17213-153">Tenant ID (for example, contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="17213-154">Házirend neve (például B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="17213-154">Policy name (for example, B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="17213-155">A token-végpont URI generálhatók azáltal, hogy a bérlő\_azonosítója és a házirend\_neve a következő URL-cím:</span><span class="sxs-lookup"><span data-stu-id="17213-155">The token endpoint URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

<span data-ttu-id="17213-156">Az engedélyezési végpont URI generálhatók azáltal, hogy a bérlő\_azonosítója és a házirend\_neve a következő URL-cím:</span><span class="sxs-lookup"><span data-stu-id="17213-156">The authorization endpoint URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

<span data-ttu-id="17213-157">Futtassa a következő kódot a AuthorizationServiceConfiguration objektum létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="17213-157">Run the following code to create your AuthorizationServiceConfiguration object:</span></span>

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a><span data-ttu-id="17213-158">Engedélyezése</span><span class="sxs-lookup"><span data-stu-id="17213-158">Authorizing</span></span>

<span data-ttu-id="17213-159">Konfigurálja, vagy egy engedélyezési szolgáltatás konfigurációjának beolvasása után egy engedélyezési-kérelem lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="17213-159">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="17213-160">A kérelem létrehozásához szüksége van a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="17213-160">To create the request, you need the following information:</span></span>  
* <span data-ttu-id="17213-161">Ügyfél-azonosító (például 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="17213-161">Client ID (for example, 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="17213-162">Átirányítási URI egy egyéni séma (például com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span><span class="sxs-lookup"><span data-stu-id="17213-162">Redirect URI with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span></span>

<span data-ttu-id="17213-163">Mindkét elemeket kell mentett Ha Ön volt [regisztrálja az alkalmazást](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="17213-163">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

<span data-ttu-id="17213-164">Az alkalmazás kezelni az egyéni séma URI-ra való beállításához módosítania a Info.pList URL-sémákat listája:</span><span class="sxs-lookup"><span data-stu-id="17213-164">To set up your application to handle the redirect to the URI with the custom scheme, you need to update the list of 'URL Schemes' in your Info.pList:</span></span>
* <span data-ttu-id="17213-165">Nyissa meg az Info.pList-ben.</span><span class="sxs-lookup"><span data-stu-id="17213-165">Open Info.pList.</span></span>
* <span data-ttu-id="17213-166">Például a "Csomag az operációs rendszer típusa Code" sor mutat, majd kattintson a \+ szimbólum.</span><span class="sxs-lookup"><span data-stu-id="17213-166">Hover over a row like 'Bundle OS Type Code' and click the \+ symbol.</span></span>
* <span data-ttu-id="17213-167">Nevezze át az új sor "URL-cím types".</span><span class="sxs-lookup"><span data-stu-id="17213-167">Rename the new row 'URL types'.</span></span>
* <span data-ttu-id="17213-168">Kattintson a "URL-cím types" balra mutató nyílra a fa megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="17213-168">Click the arrow to the left of 'URL types' to open the tree.</span></span>
* <span data-ttu-id="17213-169">Kattintson a balra mutató nyílra "elem 0' a fa megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="17213-169">Click the arrow to the left of 'Item 0' to open the tree.</span></span>
* <span data-ttu-id="17213-170">Nevezze át az első elem "URL-sémákat" 0 elem alatt.</span><span class="sxs-lookup"><span data-stu-id="17213-170">Rename first item underneath Item 0 to 'URL Schemes'.</span></span>
* <span data-ttu-id="17213-171">Kattintson a "URL-sémákat" nyissa meg a fa balra mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="17213-171">Click the arrow to the left of 'URL Schemes' to open the tree.</span></span>
* <span data-ttu-id="17213-172">Az "Érték" oszlop nincs egy üres mező balra "elem 0'"URL-sémákat"alá.</span><span class="sxs-lookup"><span data-stu-id="17213-172">In the 'Value' column, there is a blank field to the left of 'Item 0' underneath 'URL Schemes'.</span></span>  <span data-ttu-id="17213-173">Adja meg az értéket az alkalmazás egyedi séma.</span><span class="sxs-lookup"><span data-stu-id="17213-173">Set the value to your application's unique scheme.</span></span>  <span data-ttu-id="17213-174">Az értéket meg kell egyeznie a rendszer a OIDAuthorizationRequest objektum létrehozásakor használt redirectURL.</span><span class="sxs-lookup"><span data-stu-id="17213-174">The value must match the scheme used in redirectURL when creating the OIDAuthorizationRequest object.</span></span>  <span data-ttu-id="17213-175">A mintában a rendszer "com.onmicrosoft.fabrikamb2c.exampleapp" használtuk.</span><span class="sxs-lookup"><span data-stu-id="17213-175">In our sample, we used the scheme 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span></span>

<span data-ttu-id="17213-176">Tekintse meg a [AppAuth útmutató](https://openid.github.io/AppAuth-iOS/) való fejezze be a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="17213-176">Refer to the [AppAuth guide](https://openid.github.io/AppAuth-iOS/) on how to complete the rest of the process.</span></span> <span data-ttu-id="17213-177">Ha egy működő alkalmazást gyorsan megismerkedhet van szüksége, tekintse meg [a minta](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="17213-177">If you need to quickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="17213-178">Kövesse a [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) a saját Azure AD B2C konfigurációs megadását.</span><span class="sxs-lookup"><span data-stu-id="17213-178">Follow the steps in the [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) to enter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="17213-179">Azt mindig nyitva a visszajelzések és tanácsok!</span><span class="sxs-lookup"><span data-stu-id="17213-179">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="17213-180">Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="17213-180">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="17213-181">A szolgáltatás kéréseket, hozzáadhatja őket a [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="17213-181">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
