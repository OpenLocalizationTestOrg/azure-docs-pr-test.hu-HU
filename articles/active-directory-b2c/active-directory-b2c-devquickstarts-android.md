---
title: "Az Azure Active Directory B2C: Tokenbeolvasás Android alkalmazással |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan, amely Azure Active Directory B2C AppAuth használ a felhasználói identitások kezelésére, és hitelesíti a felhasználókat, Android-alkalmazás létrehozásához."
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
ms.openlocfilehash: cd4b8048245be49ea79bcb1b364f2f99c56f8291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a><span data-ttu-id="7f091-103">Az Azure AD B2C: Bejelentkezés Android alkalmazás segítségével</span><span class="sxs-lookup"><span data-stu-id="7f091-103">Azure AD B2C: Sign-in using an Android application</span></span>

<span data-ttu-id="7f091-104">A Microsoft identitásplatformja nyílt szabványokat, többek között OAuth2-t és OpenID Connectet használ.</span><span class="sxs-lookup"><span data-stu-id="7f091-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="7f091-105">Így a fejlesztők bármilyen típusú kódtárat integrálhatnak szolgáltatásainkkal.</span><span class="sxs-lookup"><span data-stu-id="7f091-105">This allows developers to leverage any library they wish to integrate with our services.</span></span> <span data-ttu-id="7f091-106">A fejlesztők támogatási be más könyvtárakkal a platformot, azt korábban írt bemutatják, hogyan lehet kapcsolódni a Microsoft identity platform 3. fél szalagtárak konfigurálása a jelen szoftverhez hasonló néhány forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="7f091-106">To aid developers in using our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure 3rd party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="7f091-107">Az [RFC6749 OAuth2 specifikációt](https://tools.ietf.org/html/rfc6749) használó legtöbb kódtár képes lesz kapcsolódni a Microsoft identitásplatformjához.</span><span class="sxs-lookup"><span data-stu-id="7f091-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) will be able to connect to the Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="7f091-108">A Microsoft nem biztosít azoknak a 3. fél szalagtárak, és nem végrehajtva a tárak áttekintése.</span><span class="sxs-lookup"><span data-stu-id="7f091-108">Microsoft does not provide fixes for 3rd party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="7f091-109">Ez a minta, amely tesztelték AppAuth nevű alapszintű forgatókönyvekben kompatibilisek-e az Azure AD B2C 3. fél szalagtárat használ.</span><span class="sxs-lookup"><span data-stu-id="7f091-109">This sample is using a 3rd party library called AppAuth that has been tested for compatibility in basic scenarios with the Azure AD B2C.</span></span> <span data-ttu-id="7f091-110">A könyvtár nyílt forráskódú projekt problémákat és funkciókra vonatkozó kérések legyenek irányítva.</span><span class="sxs-lookup"><span data-stu-id="7f091-110">Issues and feature requests should be directed to the library's open-source project.</span></span> <span data-ttu-id="7f091-111">Ellenőrizze a [Ez a cikk](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) további információt.</span><span class="sxs-lookup"><span data-stu-id="7f091-111">Please see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) for more information.</span></span>  
>
>

<span data-ttu-id="7f091-112">Ha csak most ismerkedik az OAuth2 vagy az OpenID Connect használatával, előfordulhat, hogy nem fogja tökéletesen érteni a konfigurációs lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7f091-112">If you're new to OAuth2 or OpenID Connect much of this sample configuration may not make much sense to you.</span></span> <span data-ttu-id="7f091-113">Ebben az esetben javasoljuk, hogy olvassa el [a protokoll áttekintését, amelyet itt talál](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="7f091-113">We recommend you look at a brief [overview of the protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="7f091-114">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="7f091-114">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="7f091-115">Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="7f091-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="7f091-116">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és más elemeket.</span><span class="sxs-lookup"><span data-stu-id="7f091-116">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="7f091-117">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.</span><span class="sxs-lookup"><span data-stu-id="7f091-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="7f091-118">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7f091-118">Create an application</span></span>

<span data-ttu-id="7f091-119">A következő lépésben hozzon létre egy alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="7f091-119">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="7f091-120">Ez biztosítja az alkalmazással történő biztonságos kommunikációhoz szükséges információkat az Azure AD számára.</span><span class="sxs-lookup"><span data-stu-id="7f091-120">This gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="7f091-121">A mobilalkalmazások létrehozásához hajtsa végre az [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="7f091-121">To create a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="7f091-122">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="7f091-122">Be sure to:</span></span>

* <span data-ttu-id="7f091-123">Tartalmaznak egy **Native Client** az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7f091-123">Include a **Native Client** in the application.</span></span>
* <span data-ttu-id="7f091-124">Másolja az alkalmazáshoz rendelt **alkalmazásazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="7f091-124">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="7f091-125">Ezt később lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="7f091-125">You will need this later.</span></span>
* <span data-ttu-id="7f091-126">Állítson be egy natív ügyfél **átirányítási URI-** (pl. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="7f091-126">Set up a native client **Redirect URI** (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="7f091-127">Később erre is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="7f091-127">You will also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="7f091-128">Házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="7f091-128">Create your policies</span></span>

<span data-ttu-id="7f091-129">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="7f091-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="7f091-130">Az alkalmazás tartalmaz egy identitás-élmény: egy kombinált bejelentkezési és regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="7f091-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="7f091-131">Szeretné létrehozni ezt a házirendet, lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="7f091-131">You need to create this policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="7f091-132">A szabályzat létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="7f091-132">When you create the policy, be sure to:</span></span>

* <span data-ttu-id="7f091-133">Válassza ki a **megjelenített név** a szabályzatot az előfizetési attribútumaként.</span><span class="sxs-lookup"><span data-stu-id="7f091-133">Choose the **Display name** as a sign-up attribute in your policy.</span></span>
* <span data-ttu-id="7f091-134">Az összes szabályzatban válassza ki a **Megjelenített név** és az **Objektumazonosító** alkalmazási jogcímet.</span><span class="sxs-lookup"><span data-stu-id="7f091-134">Choose the **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="7f091-135">Ezenfelül más jogcímeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="7f091-135">You can choose other claims as well.</span></span>
* <span data-ttu-id="7f091-136">Az egyes házirendek létrehozása után másolja a házirend **nevét**.</span><span class="sxs-lookup"><span data-stu-id="7f091-136">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="7f091-137">A névnek a következő előtaggal kell rendelkeznie: `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="7f091-137">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="7f091-138">A szabályzat nevére később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="7f091-138">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="7f091-139">A szabályzat létrehozását követően készen áll az alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="7f091-139">After you have created your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="7f091-140">A mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="7f091-140">Download the sample code</span></span>

<span data-ttu-id="7f091-141">Egy Azure AD B2C AppAuth használó minta adtunk [a Githubon](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="7f091-141">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="7f091-142">Töltse le a kódot, és futtassa.</span><span class="sxs-lookup"><span data-stu-id="7f091-142">You can download the code and run it.</span></span> <span data-ttu-id="7f091-143">Gyorsan utasításait követve a saját Azure AD B2C konfigurációs használatával saját alkalmazással Ismerkedés a [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="7f091-143">You can quickly get started with your own app using your own Azure AD B2C configuration by following the instructions in the [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="7f091-144">A minta rendszer által biztosított mintafájl módosítását [AppAuth](https://openid.github.io/AppAuth-Android/).</span><span class="sxs-lookup"><span data-stu-id="7f091-144">The sample is a modification of the sample provided by [AppAuth](https://openid.github.io/AppAuth-Android/).</span></span> <span data-ttu-id="7f091-145">Látogasson el a AppAuth és funkcióival kapcsolatos további oldalára.</span><span class="sxs-lookup"><span data-stu-id="7f091-145">Please visit their page to learn more about AppAuth and its features.</span></span>

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a><span data-ttu-id="7f091-146">Az alkalmazás az Azure AD B2C használata AppAuth módosítása</span><span class="sxs-lookup"><span data-stu-id="7f091-146">Modifying your app to use Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="7f091-147">AppAuth támogatja az Android API 16 (Jellybean) vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="7f091-147">AppAuth supports Android API 16 (Jellybean) and above.</span></span> <span data-ttu-id="7f091-148">Azt javasoljuk, API 23 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="7f091-148">We recommend using API 23 and above.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="7f091-149">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7f091-149">Configuration</span></span>

<span data-ttu-id="7f091-150">Az Azure AD B2C kommunikáció vagy a felderítés URI, vagy az engedélyezési végpont és a token-végpont URI-azonosítók megadásával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="7f091-150">You can configure communication with Azure AD B2C by either specifying the discovery URI or by specifying both the authorization endpoint and token endpoint URIs.</span></span> <span data-ttu-id="7f091-151">Mindkét esetben szüksége lesz a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="7f091-151">In either case, you will need the following information:</span></span>

* <span data-ttu-id="7f091-152">Bérlő azonosítója (pl. contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="7f091-152">Tenant ID (e.g. contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="7f091-153">Házirend neve (pl. B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="7f091-153">Policy name (e.g. B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="7f091-154">Ha automatikus észleléséhez, az engedélyezés és a token-végpont URI-azonosítók választja, akkor adatok beolvasása a felderítés URI.</span><span class="sxs-lookup"><span data-stu-id="7f091-154">If you choose to automatically discover the authorization and token endpoint URIs, you will need to fetch information from the discovery URI.</span></span> <span data-ttu-id="7f091-155">A felderítés URI generálhatók azáltal, hogy a bérlő\_azonosítója és a házirend\_az alábbi URL-címben:</span><span class="sxs-lookup"><span data-stu-id="7f091-155">The discovery URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

<span data-ttu-id="7f091-156">Ezután az engedélyezési és a token-végpont URI-k megszerzésére és AuthorizationServiceConfiguration objektumot létrehozni a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7f091-156">You can then acquire the authorization and token endpoint URIs and create an AuthorizationServiceConfiguration object by running the following:</span></span>

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
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

<span data-ttu-id="7f091-157">Felderítési helyett a az engedélyezési és a token-végpont URI-k, azt is megadhatja azokat explicit módon történő lecserélésével a bérlő\_azonosítója és a házirend\_neve az URL-címet az alábbi:</span><span class="sxs-lookup"><span data-stu-id="7f091-157">Instead of using discovery to obtain the authorization and token endpoint URIs, you can also specify them explicitly by replacing the Tenant\_ID and the Policy\_Name in the URL's below:</span></span>

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

<span data-ttu-id="7f091-158">Futtassa a következő kódot a AuthorizationServiceConfiguration objektum létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="7f091-158">Run the following code to create your AuthorizationServiceConfiguration object:</span></span>

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a><span data-ttu-id="7f091-159">Engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7f091-159">Authorizing</span></span>

<span data-ttu-id="7f091-160">Konfigurálja, vagy egy engedélyezési szolgáltatás konfigurációjának beolvasása után egy engedélyezési-kérelem lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="7f091-160">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="7f091-161">A kérelem létrehozásához szüksége lesz a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="7f091-161">To create the request, you will need the following information:</span></span>

* <span data-ttu-id="7f091-162">Ügyfél-azonosító (pl. 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="7f091-162">Client ID (e.g. 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="7f091-163">Átirányítási URI egy egyéni séma (pl. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span><span class="sxs-lookup"><span data-stu-id="7f091-163">Redirect URI with a custom scheme (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span></span>

<span data-ttu-id="7f091-164">Mindkét elemeket kell mentett Ha Ön volt [regisztrálja az alkalmazást](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="7f091-164">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

<span data-ttu-id="7f091-165">Tekintse meg a [AppAuth útmutató](https://openid.github.io/AppAuth-Android/) való fejezze be a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="7f091-165">Please refer to the [AppAuth guide](https://openid.github.io/AppAuth-Android/) on how to complete the rest of the process.</span></span> <span data-ttu-id="7f091-166">Ha egy működő alkalmazást gyorsan megismerkedhet van szüksége, tekintse meg [a minta](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="7f091-166">If you need to quickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="7f091-167">Kövesse a [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) a saját Azure AD B2C konfigurációs megadását.</span><span class="sxs-lookup"><span data-stu-id="7f091-167">Follow the steps in the [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) to enter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="7f091-168">Azt mindig nyitva a visszajelzések és tanácsok!</span><span class="sxs-lookup"><span data-stu-id="7f091-168">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="7f091-169">Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="7f091-169">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="7f091-170">A szolgáltatás kéréseket, hozzáadhatja őket a [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="7f091-170">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

