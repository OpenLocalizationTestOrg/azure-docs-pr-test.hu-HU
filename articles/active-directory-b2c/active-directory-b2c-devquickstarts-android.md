---
title: "Az Azure Active Directory B2C: Tokenbeolvasás Android alkalmazással |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocreate Android-alkalmazás, amely Azure Active Directory B2C toomanage felhasználói identitások AppAuth használ, valamint a felhasználók hitelesítésére."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
ms.openlocfilehash: 0236398673115a34951f035cb1e73e89417abf86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a><span data-ttu-id="6eb93-103">Az Azure AD B2C: Bejelentkezés Android alkalmazás segítségével</span><span class="sxs-lookup"><span data-stu-id="6eb93-103">Azure AD B2C: Sign-in using an Android application</span></span>

<span data-ttu-id="6eb93-104">hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja.</span><span class="sxs-lookup"><span data-stu-id="6eb93-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="6eb93-105">Ez lehetővé teszi a fejlesztők tooleverage függvénytárat, a szolgáltatások toointegrate kívánnak.</span><span class="sxs-lookup"><span data-stu-id="6eb93-105">This allows developers tooleverage any library they wish toointegrate with our services.</span></span> <span data-ttu-id="6eb93-106">tooaid fejlesztők számára a más könyvtárak a platformot használnak, azt korábban írt néhány forgatókönyvek például a egy toodemonstrate hogyan tooconfigure 3. fél szalagtárak tooconnect toohello Microsoft identitásplatformmal.</span><span class="sxs-lookup"><span data-stu-id="6eb93-106">tooaid developers in using our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure 3rd party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="6eb93-107">A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) képes tooconnect toohello Microsoft Identity platform lesz.</span><span class="sxs-lookup"><span data-stu-id="6eb93-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) will be able tooconnect toohello Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="6eb93-108">A Microsoft nem biztosít azoknak a 3. fél szalagtárak, és nem végrehajtva a tárak áttekintése.</span><span class="sxs-lookup"><span data-stu-id="6eb93-108">Microsoft does not provide fixes for 3rd party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="6eb93-109">Ez a minta, amely tesztelték AppAuth nevű alapszintű forgatókönyvekben hello Azure AD B2C-vel való kompatibilitás érdekében 3. fél szalagtárat használ.</span><span class="sxs-lookup"><span data-stu-id="6eb93-109">This sample is using a 3rd party library called AppAuth that has been tested for compatibility in basic scenarios with hello Azure AD B2C.</span></span> <span data-ttu-id="6eb93-110">Problémákat és funkciókra vonatkozó kérések irányított toohello könyvtár nyílt forráskódú projekt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6eb93-110">Issues and feature requests should be directed toohello library's open-source project.</span></span> <span data-ttu-id="6eb93-111">Ellenőrizze a [Ez a cikk](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) további információt.</span><span class="sxs-lookup"><span data-stu-id="6eb93-111">Please see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) for more information.</span></span>  
>
>

<span data-ttu-id="6eb93-112">Ha új tooOAuth2 vagy az OpenID Connect még a minta konfigurálási nem tehetik sok logika tooyou.</span><span class="sxs-lookup"><span data-stu-id="6eb93-112">If you're new tooOAuth2 or OpenID Connect much of this sample configuration may not make much sense tooyou.</span></span> <span data-ttu-id="6eb93-113">Javasoljuk, hogy megnézi rövid [azt korábban itt dokumentált lehetőségektől hello protokoll – áttekintés](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="6eb93-113">We recommend you look at a brief [overview of hello protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="6eb93-114">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="6eb93-114">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="6eb93-115">Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="6eb93-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="6eb93-116">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és más elemeket.</span><span class="sxs-lookup"><span data-stu-id="6eb93-116">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="6eb93-117">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="6eb93-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="6eb93-118">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb93-118">Create an application</span></span>

<span data-ttu-id="6eb93-119">A következő lépésben toocreate egy alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="6eb93-119">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="6eb93-120">Ez biztosítja, hogy kell-e az alkalmazással biztonságosan toocommunicate az Azure AD-információkat.</span><span class="sxs-lookup"><span data-stu-id="6eb93-120">This gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="6eb93-121">Kövesse a mobilalkalmazások toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="6eb93-121">toocreate a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="6eb93-122">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="6eb93-122">Be sure to:</span></span>

* <span data-ttu-id="6eb93-123">Tartalmaznak egy **Native Client** hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6eb93-123">Include a **Native Client** in hello application.</span></span>
* <span data-ttu-id="6eb93-124">Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6eb93-124">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="6eb93-125">Ezt később lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6eb93-125">You will need this later.</span></span>
* <span data-ttu-id="6eb93-126">Állítson be egy natív ügyfél **átirányítási URI-** (pl. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="6eb93-126">Set up a native client **Redirect URI** (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="6eb93-127">Később erre is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="6eb93-127">You will also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="6eb93-128">Házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb93-128">Create your policies</span></span>

<span data-ttu-id="6eb93-129">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="6eb93-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="6eb93-130">Az alkalmazás tartalmaz egy identitás-élmény: egy kombinált bejelentkezési és regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="6eb93-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="6eb93-131">Szüksége toocreate ezt a házirendet, lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="6eb93-131">You need toocreate this policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="6eb93-132">Hello házirend létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="6eb93-132">When you create hello policy, be sure to:</span></span>

* <span data-ttu-id="6eb93-133">Válassza ki a hello **megjelenített név** a szabályzatot az előfizetési attribútumaként.</span><span class="sxs-lookup"><span data-stu-id="6eb93-133">Choose hello **Display name** as a sign-up attribute in your policy.</span></span>
* <span data-ttu-id="6eb93-134">Válassza ki a hello **megjelenített név** és **Objektumazonosító** minden házirend alkalmazási jogcímet.</span><span class="sxs-lookup"><span data-stu-id="6eb93-134">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="6eb93-135">Kiválaszthat egyéb jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="6eb93-135">You can choose other claims as well.</span></span>
* <span data-ttu-id="6eb93-136">Másolás hello **neve** egyes házirendek létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="6eb93-136">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="6eb93-137">Hello előtag nem rendelkezhet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="6eb93-137">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="6eb93-138">Hello házirendnév később lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="6eb93-138">You'll need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="6eb93-139">Miután létrehozta a házirendeket, készen áll a toobuild Ön az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6eb93-139">After you have created your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="6eb93-140">Hello mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="6eb93-140">Download hello sample code</span></span>

<span data-ttu-id="6eb93-141">Egy Azure AD B2C AppAuth használó minta adtunk [a Githubon](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="6eb93-141">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="6eb93-142">Töltse le a hello kódot, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="6eb93-142">You can download hello code and run it.</span></span> <span data-ttu-id="6eb93-143">Akkor is gyorsan megismerkedhet a saját alkalmazás a saját Azure AD B2C konfigurációval hello hello utasításait követve [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="6eb93-143">You can quickly get started with your own app using your own Azure AD B2C configuration by following hello instructions in hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="6eb93-144">hello minta olyan módosítás által biztosított hello minta [AppAuth](https://openid.github.io/AppAuth-Android/).</span><span class="sxs-lookup"><span data-stu-id="6eb93-144">hello sample is a modification of hello sample provided by [AppAuth](https://openid.github.io/AppAuth-Android/).</span></span> <span data-ttu-id="6eb93-145">Látogasson el a lap toolearn AppAuth és funkcióival kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="6eb93-145">Please visit their page toolearn more about AppAuth and its features.</span></span>

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a><span data-ttu-id="6eb93-146">Az alkalmazás toouse Azure AD B2C-vel rendelkező AppAuth módosítása</span><span class="sxs-lookup"><span data-stu-id="6eb93-146">Modifying your app toouse Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="6eb93-147">AppAuth támogatja az Android API 16 (Jellybean) vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="6eb93-147">AppAuth supports Android API 16 (Jellybean) and above.</span></span> <span data-ttu-id="6eb93-148">Azt javasoljuk, API 23 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="6eb93-148">We recommend using API 23 and above.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="6eb93-149">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="6eb93-149">Configuration</span></span>

<span data-ttu-id="6eb93-150">Az Azure AD B2C kommunikációs megadását hello derítenie URI vagy megadva hello engedélyezési végpont és a token-végpont URI-azonosítók konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="6eb93-150">You can configure communication with Azure AD B2C by either specifying hello discovery URI or by specifying both hello authorization endpoint and token endpoint URIs.</span></span> <span data-ttu-id="6eb93-151">Mindkét esetben szüksége lesz a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6eb93-151">In either case, you will need hello following information:</span></span>

* <span data-ttu-id="6eb93-152">Bérlő azonosítója (pl. contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="6eb93-152">Tenant ID (e.g. contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="6eb93-153">Házirend neve (pl. B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="6eb93-153">Policy name (e.g. B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="6eb93-154">Ha úgy dönt, tooautomatically hello engedélyezési és a token-végpont URI-azonosítók felderítése, a hello felderítési URI toofetch információk megadása szükséges.</span><span class="sxs-lookup"><span data-stu-id="6eb93-154">If you choose tooautomatically discover hello authorization and token endpoint URIs, you will need toofetch information from hello discovery URI.</span></span> <span data-ttu-id="6eb93-155">hello felderítési URI generálhatók hello bérlői lecserélésével\_azonosítója és hello házirend\_neve a következő URL-cím hello:</span><span class="sxs-lookup"><span data-stu-id="6eb93-155">hello discovery URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

<span data-ttu-id="6eb93-156">Ezután hello engedélyezési és a token-végpont URI-k megszerzésére és AuthorizationServiceConfiguration objektumot létrehozni hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6eb93-156">You can then acquire hello authorization and token endpoint URIs and create an AuthorizationServiceConfiguration object by running hello following:</span></span>

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed tooretrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed tooauthorization...
        }
      }
  });
```

<span data-ttu-id="6eb93-157">Felderítési tooobtain hello engedélyezési és a token-végpont URI-k helyett azt is megadhatja azokat explicit módon történő lecserélésével hello bérlői\_azonosítója és hello házirend\_neve hello URL-ben az alábbi:</span><span class="sxs-lookup"><span data-stu-id="6eb93-157">Instead of using discovery tooobtain hello authorization and token endpoint URIs, you can also specify them explicitly by replacing hello Tenant\_ID and hello Policy\_Name in hello URL's below:</span></span>

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

<span data-ttu-id="6eb93-158">Futtassa a következő kód toocreate hello a AuthorizationServiceConfiguration objektum:</span><span class="sxs-lookup"><span data-stu-id="6eb93-158">Run hello following code toocreate your AuthorizationServiceConfiguration object:</span></span>

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform hello auth request...
```

### <a name="authorizing"></a><span data-ttu-id="6eb93-159">Engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6eb93-159">Authorizing</span></span>

<span data-ttu-id="6eb93-160">Konfigurálja, vagy egy engedélyezési szolgáltatás konfigurációjának beolvasása után egy engedélyezési-kérelem lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6eb93-160">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="6eb93-161">kérelem toocreate hello, szüksége lesz a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6eb93-161">toocreate hello request, you will need hello following information:</span></span>

* <span data-ttu-id="6eb93-162">Ügyfél-azonosító (pl. 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="6eb93-162">Client ID (e.g. 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="6eb93-163">Átirányítási URI egy egyéni séma (pl. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span><span class="sxs-lookup"><span data-stu-id="6eb93-163">Redirect URI with a custom scheme (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span></span>

<span data-ttu-id="6eb93-164">Mindkét elemeket kell mentett Ha Ön volt [regisztrálja az alkalmazást](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="6eb93-164">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

<span data-ttu-id="6eb93-165">Tekintse meg a toohello [AppAuth útmutató](https://openid.github.io/AppAuth-Android/) a hogyan toocomplete hello ki hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="6eb93-165">Please refer toohello [AppAuth guide](https://openid.github.io/AppAuth-Android/) on how toocomplete hello rest of hello process.</span></span> <span data-ttu-id="6eb93-166">Ha tooquickly kell egy működő alkalmazást az első lépései, tekintse meg [a minta](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="6eb93-166">If you need tooquickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="6eb93-167">Hello kövesse hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter a saját Azure AD B2C-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="6eb93-167">Follow hello steps in hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="6eb93-168">Azt a rendszer mindig megnyitott toofeedback és javaslatok!</span><span class="sxs-lookup"><span data-stu-id="6eb93-168">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="6eb93-169">Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="6eb93-169">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="6eb93-170">A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="6eb93-170">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

