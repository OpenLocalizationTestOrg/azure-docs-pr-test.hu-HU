---
title: "IOS alkalmazás - Azure AD B2C segítségével tokenbeolvasás |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocreate egy iOS-alkalmazást, amely Azure Active Directory B2C toomanage felhasználói identitások AppAuth használ, valamint a felhasználók hitelesítésére."
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
ms.openlocfilehash: e7cbe2de6e9ae3d45448cdd36292c457a0ef4887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a><span data-ttu-id="14fb9-103">Az Azure AD B2C: Bejelentkezés használata iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="14fb9-103">Azure AD B2C: Sign-in using an iOS application</span></span>

<span data-ttu-id="14fb9-104">hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja.</span><span class="sxs-lookup"><span data-stu-id="14fb9-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="14fb9-105">További fejlesztői választott egy megnyitott szabványos protokollt használó kínál, a szalagtár toointegrate a szolgáltatások kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="14fb9-105">Using an open standard protocol offers more developer choice when selecting a library toointegrate with our services.</span></span> <span data-ttu-id="14fb9-106">Nyújtunk a forgatókönyv, míg mások például azt tooaid fejlesztők toohello Microsoft Identity platform csatlakozó alkalmazások írásához.</span><span class="sxs-lookup"><span data-stu-id="14fb9-106">We've provided this walkthrough and others like it tooaid developers with writing applications that connect toohello Microsoft Identity platform.</span></span> <span data-ttu-id="14fb9-107">A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) képes tooconnect toohello Microsoft Identity platform vannak.</span><span class="sxs-lookup"><span data-stu-id="14fb9-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) are able tooconnect toohello Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="14fb9-108">A Microsoft nem biztosít a külső szalagtárak javítások, és nem végrehajtva a tárak áttekintése.</span><span class="sxs-lookup"><span data-stu-id="14fb9-108">Microsoft does not provide fixes for third-party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="14fb9-109">Ez a minta, amely tesztelték AppAuth nevű alapszintű forgatókönyvekben kompatibilisek-e az Azure AD B2C hello külső szalagtárat használ.</span><span class="sxs-lookup"><span data-stu-id="14fb9-109">This sample is using a third-party library called AppAuth that has been tested for compatibility in basic scenarios with hello Azure AD B2C.</span></span> <span data-ttu-id="14fb9-110">Problémákat és funkciókra vonatkozó kérések irányított toohello könyvtár nyílt forráskódú projekt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="14fb9-110">Issues and feature requests should be directed toohello library's open-source project.</span></span> <span data-ttu-id="14fb9-111">További információkért tekintse meg [ezt a cikket](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span><span class="sxs-lookup"><span data-stu-id="14fb9-111">For more information, see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span></span>
>
>

<span data-ttu-id="14fb9-112">Ha új tooOAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem tehetik sok logika tooyou.</span><span class="sxs-lookup"><span data-stu-id="14fb9-112">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make much sense tooyou.</span></span> <span data-ttu-id="14fb9-113">Javasoljuk, hogy megnézi rövid [azt korábban itt dokumentált lehetőségektől hello protokoll – áttekintés](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="14fb9-113">We recommend you look at a brief [overview of hello protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="14fb9-114">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="14fb9-114">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="14fb9-115">Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="14fb9-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="14fb9-116">A könyvtár egy olyan tároló, a felhasználók, alkalmazások, csoportok és több.</span><span class="sxs-lookup"><span data-stu-id="14fb9-116">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="14fb9-117">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="14fb9-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="14fb9-118">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="14fb9-118">Create an application</span></span>
<span data-ttu-id="14fb9-119">A következő lépésben toocreate egy alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="14fb9-119">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="14fb9-120">hello app regisztrációs biztosítja, hogy kell-e az alkalmazással biztonságosan toocommunicate az Azure AD-információkat.</span><span class="sxs-lookup"><span data-stu-id="14fb9-120">hello app registration gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="14fb9-121">Kövesse a mobilalkalmazások toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="14fb9-121">toocreate a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="14fb9-122">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="14fb9-122">Be sure to:</span></span>

* <span data-ttu-id="14fb9-123">Tartalmaznak egy **natív ügyfél** hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="14fb9-123">Include a **Native client** in hello application.</span></span>
* <span data-ttu-id="14fb9-124">Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="14fb9-124">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="14fb9-125">A GUID később szüksége.</span><span class="sxs-lookup"><span data-stu-id="14fb9-125">You need this GUID later.</span></span>
* <span data-ttu-id="14fb9-126">Állítson be egy **átirányítási URI-** egy egyéni séma (például com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="14fb9-126">Set up a **Redirect URI** with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="14fb9-127">Ezt az URI később szüksége.</span><span class="sxs-lookup"><span data-stu-id="14fb9-127">You need this URI later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="14fb9-128">Szabályzatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="14fb9-128">Create your policies</span></span>
<span data-ttu-id="14fb9-129">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="14fb9-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="14fb9-130">Az alkalmazás tartalmaz egy identitás-élmény: egy kombinált bejelentkezési és regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="14fb9-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="14fb9-131">A házirend létrehozásához lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="14fb9-131">Create this policy as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="14fb9-132">Hello házirend létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="14fb9-132">When you create hello policy, be sure to:</span></span>

* <span data-ttu-id="14fb9-133">A **regisztrációs attribútumokat**, jelölje be hello attribútum **megjelenített név**.</span><span class="sxs-lookup"><span data-stu-id="14fb9-133">Under **Sign-up attributes**, select hello attribute **Display name**.</span></span>  <span data-ttu-id="14fb9-134">Kiválaszthatja, valamint egyéb attribútumai.</span><span class="sxs-lookup"><span data-stu-id="14fb9-134">You can select other attributes as well.</span></span>
* <span data-ttu-id="14fb9-135">A **alkalmazási jogcímet**, hello jogcímek kiválasztására **megjelenített név** és **felhasználó Objektumazonosító**.</span><span class="sxs-lookup"><span data-stu-id="14fb9-135">Under **Application claims**, select hello claims **Display name** and **User's Object ID**.</span></span> <span data-ttu-id="14fb9-136">Kiválaszthatja, hogy más jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="14fb9-136">You can select other claims as well.</span></span>
* <span data-ttu-id="14fb9-137">Másolás hello **neve** egyes házirendek létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="14fb9-137">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="14fb9-138">A házirend neve a következő előtaggal van `b2c_1_` hello házirend mentésekor.</span><span class="sxs-lookup"><span data-stu-id="14fb9-138">Your policy name is prefixed with `b2c_1_` when you save hello policy.</span></span>  <span data-ttu-id="14fb9-139">Később szüksége hello házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="14fb9-139">You need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="14fb9-140">Miután létrehozta a házirendeket, készen áll a toobuild Ön az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="14fb9-140">After you have created your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="14fb9-141">Hello mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="14fb9-141">Download hello sample code</span></span>
<span data-ttu-id="14fb9-142">Egy Azure AD B2C AppAuth használó minta adtunk [a Githubon](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="14fb9-142">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="14fb9-143">Töltse le a hello kódot, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="14fb9-143">You can download hello code and run it.</span></span> <span data-ttu-id="14fb9-144">a bérlői a saját Azure AD B2C toouse, hello hello utasításait követve [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="14fb9-144">toouse your own Azure AD B2C tenant, follow hello instructions in hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="14fb9-145">Ez a minta hozta létre hello információs utasításai szerint hello [iOS AppAuth-projektre a Githubon](https://github.com/openid/AppAuth-iOS).</span><span class="sxs-lookup"><span data-stu-id="14fb9-145">This sample was created by following hello README instructions by hello [iOS AppAuth project on GitHub](https://github.com/openid/AppAuth-iOS).</span></span> <span data-ttu-id="14fb9-146">Hello mintát és hello szalagtár működése a további részletekért lásd hello AppAuth fontos a Githubon.</span><span class="sxs-lookup"><span data-stu-id="14fb9-146">For more details on how hello sample and hello library work, reference hello AppAuth README on GitHub.</span></span>

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a><span data-ttu-id="14fb9-147">Az alkalmazás toouse Azure AD B2C-vel rendelkező AppAuth módosítása</span><span class="sxs-lookup"><span data-stu-id="14fb9-147">Modifying your app toouse Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="14fb9-148">AppAuth támogatja az iOS 7-es vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="14fb9-148">AppAuth supports iOS 7 and above.</span></span>  <span data-ttu-id="14fb9-149">Azonban azt toosupport közösségi bejelentkezések a Google, SFSafariViewController van szükség, amelyhez iOS 9-es vagy újabb rendszer szükséges.</span><span class="sxs-lookup"><span data-stu-id="14fb9-149">However, toosupport social logins on Google, SFSafariViewController is needed which requires iOS 9 or higher.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="14fb9-150">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="14fb9-150">Configuration</span></span>

<span data-ttu-id="14fb9-151">Az Azure AD B2C kommunikációs megadva hello engedélyezési végpont és a token-végpont URI-azonosítók konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="14fb9-151">You can configure communication with Azure AD B2C by specifying both hello authorization endpoint and token endpoint URIs.</span></span>  <span data-ttu-id="14fb9-152">toogenerate az URI-k kell hello a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="14fb9-152">toogenerate these URIs, you need hello following information:</span></span>
* <span data-ttu-id="14fb9-153">Bérlő azonosítója (például contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="14fb9-153">Tenant ID (for example, contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="14fb9-154">Házirend neve (például B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="14fb9-154">Policy name (for example, B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="14fb9-155">hello token-végpont URI generálhatók tartományvezérlőkkel történő lecserélésével hello bérlői\_azonosítója és hello házirend\_neve a következő URL-cím hello:</span><span class="sxs-lookup"><span data-stu-id="14fb9-155">hello token endpoint URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

<span data-ttu-id="14fb9-156">hello engedélyezési végpont URI generálhatók tartományvezérlőkkel történő lecserélésével hello bérlői\_azonosítója és hello házirend\_neve a következő URL-cím hello:</span><span class="sxs-lookup"><span data-stu-id="14fb9-156">hello authorization endpoint URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

<span data-ttu-id="14fb9-157">Futtassa a következő kód toocreate hello a AuthorizationServiceConfiguration objektum:</span><span class="sxs-lookup"><span data-stu-id="14fb9-157">Run hello following code toocreate your AuthorizationServiceConfiguration object:</span></span>

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready tooperform hello auth request...
```

### <a name="authorizing"></a><span data-ttu-id="14fb9-158">Engedélyezése</span><span class="sxs-lookup"><span data-stu-id="14fb9-158">Authorizing</span></span>

<span data-ttu-id="14fb9-159">Konfigurálja, vagy egy engedélyezési szolgáltatás konfigurációjának beolvasása után egy engedélyezési-kérelem lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="14fb9-159">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="14fb9-160">kérelem toocreate hello, a következő információk hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="14fb9-160">toocreate hello request, you need hello following information:</span></span>  
* <span data-ttu-id="14fb9-161">Ügyfél-azonosító (például 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="14fb9-161">Client ID (for example, 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="14fb9-162">Átirányítási URI egy egyéni séma (például com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span><span class="sxs-lookup"><span data-stu-id="14fb9-162">Redirect URI with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span></span>

<span data-ttu-id="14fb9-163">Mindkét elemeket kell mentett Ha Ön volt [regisztrálja az alkalmazást](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="14fb9-163">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

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

<span data-ttu-id="14fb9-164">tooset be az alkalmazás toohandle hello átirányítási toohello URI hello egyéni séma, szükséges "URL-sémákat" tooupdate hello listája az Info.pList-ben:</span><span class="sxs-lookup"><span data-stu-id="14fb9-164">tooset up your application toohandle hello redirect toohello URI with hello custom scheme, you need tooupdate hello list of 'URL Schemes' in your Info.pList:</span></span>
* <span data-ttu-id="14fb9-165">Nyissa meg az Info.pList-ben.</span><span class="sxs-lookup"><span data-stu-id="14fb9-165">Open Info.pList.</span></span>
* <span data-ttu-id="14fb9-166">Például a "Csomag az operációs rendszer típusa Code" sor mutat, majd kattintson a hello \+ szimbólum.</span><span class="sxs-lookup"><span data-stu-id="14fb9-166">Hover over a row like 'Bundle OS Type Code' and click hello \+ symbol.</span></span>
* <span data-ttu-id="14fb9-167">Nevezze át a hello új sor "URL-cím types".</span><span class="sxs-lookup"><span data-stu-id="14fb9-167">Rename hello new row 'URL types'.</span></span>
* <span data-ttu-id="14fb9-168">Kattintson a hello toohello balra nyíl "URL-cím types" tooopen hello fa.</span><span class="sxs-lookup"><span data-stu-id="14fb9-168">Click hello arrow toohello left of 'URL types' tooopen hello tree.</span></span>
* <span data-ttu-id="14fb9-169">Kattintson a hello toohello balra nyíl tooopen hello fa "Konfigurációelem-0".</span><span class="sxs-lookup"><span data-stu-id="14fb9-169">Click hello arrow toohello left of 'Item 0' tooopen hello tree.</span></span>
* <span data-ttu-id="14fb9-170">Nevezze át az első elem alatt elem 0 too'URL rendszerek.</span><span class="sxs-lookup"><span data-stu-id="14fb9-170">Rename first item underneath Item 0 too'URL Schemes'.</span></span>
* <span data-ttu-id="14fb9-171">Kattintson a hello toohello balra nyíl "URL-sémákat" tooopen hello fa.</span><span class="sxs-lookup"><span data-stu-id="14fb9-171">Click hello arrow toohello left of 'URL Schemes' tooopen hello tree.</span></span>
* <span data-ttu-id="14fb9-172">Van egy üres mező toohello bal oldalán hello "Érték" oszlop, "0 elem" "URL-sémákat" alatt.</span><span class="sxs-lookup"><span data-stu-id="14fb9-172">In hello 'Value' column, there is a blank field toohello left of 'Item 0' underneath 'URL Schemes'.</span></span>  <span data-ttu-id="14fb9-173">Állítsa be a hello érték tooyour alkalmazás egyedi séma.</span><span class="sxs-lookup"><span data-stu-id="14fb9-173">Set hello value tooyour application's unique scheme.</span></span>  <span data-ttu-id="14fb9-174">hello értéket meg kell egyeznie a redirectURL hello OIDAuthorizationRequest objektum létrehozásakor használt hello séma.</span><span class="sxs-lookup"><span data-stu-id="14fb9-174">hello value must match hello scheme used in redirectURL when creating hello OIDAuthorizationRequest object.</span></span>  <span data-ttu-id="14fb9-175">A mintában szereplő hello séma "com.onmicrosoft.fabrikamb2c.exampleapp" használtuk.</span><span class="sxs-lookup"><span data-stu-id="14fb9-175">In our sample, we used hello scheme 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span></span>

<span data-ttu-id="14fb9-176">Tekintse meg a toohello [AppAuth útmutató](https://openid.github.io/AppAuth-iOS/) a hogyan toocomplete hello ki hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="14fb9-176">Refer toohello [AppAuth guide](https://openid.github.io/AppAuth-iOS/) on how toocomplete hello rest of hello process.</span></span> <span data-ttu-id="14fb9-177">Ha tooquickly kell egy működő alkalmazást az első lépései, tekintse meg [a minta](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="14fb9-177">If you need tooquickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="14fb9-178">Hello kövesse hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter a saját Azure AD B2C-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="14fb9-178">Follow hello steps in hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="14fb9-179">Azt a rendszer mindig megnyitott toofeedback és javaslatok!</span><span class="sxs-lookup"><span data-stu-id="14fb9-179">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="14fb9-180">Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="14fb9-180">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="14fb9-181">A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="14fb9-181">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
