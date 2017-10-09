---
title: Active Directory B2C aaaAzure |} Microsoft Docs
description: "Hogyan toobuild Windows asztali alkalmazások, amely magában foglalja a bejelentkezési, regisztrációs profil és felügyeleti Azure Active Directory B2C használatával."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9da14362-8216-4485-960e-af17cd5ba3bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: f22b0299ff74bfba2f3fea88f006da609859dda5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a><span data-ttu-id="3fa67-103">Az Azure AD B2C: A Windows asztali alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="3fa67-103">Azure AD B2C: Build a Windows desktop app</span></span>
<span data-ttu-id="3fa67-104">Azure Active Directory (Azure AD) B2C használatával néhány néhány lépés elvégzésével hatékony önkiszolgáló identitáskezelési felügyeleti funkciók tooyour egy asztali alkalmazás adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="3fa67-104">By using Azure Active Directory (Azure AD) B2C, you can add powerful self-service identity management features tooyour desktop app in a few short steps.</span></span> <span data-ttu-id="3fa67-105">Ez a cikk bemutatja, hogyan toocreate egy .NET Windows megjelenítési alaprendszer (WPF) "Feladatlista" alkalmazást, amely tartalmazza a felhasználói regisztráció, bejelentkezés és profilkezelést.</span><span class="sxs-lookup"><span data-stu-id="3fa67-105">This article will show you how toocreate a .NET Windows Presentation Foundation (WPF) "to-do list" app that includes user sign-up, sign-in, and profile management.</span></span> <span data-ttu-id="3fa67-106">hello app támogatni fogja előfizetési, és jelentkezzen be egy felhasználónevet vagy e-mailek használatával.</span><span class="sxs-lookup"><span data-stu-id="3fa67-106">hello app will include support for sign-up and sign-in by using a user name or email.</span></span> <span data-ttu-id="3fa67-107">Azt is támogatni fogja előfizetési, és jelentkezzen be például a Facebookhoz és a Google közösségi fiókokkal.</span><span class="sxs-lookup"><span data-stu-id="3fa67-107">It will also include support for sign-up and sign-in by using social accounts such as Facebook and Google.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="3fa67-108">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="3fa67-108">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="3fa67-109">Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="3fa67-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="3fa67-110">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást.</span><span class="sxs-lookup"><span data-stu-id="3fa67-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="3fa67-111">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="3fa67-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="3fa67-112">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fa67-112">Create an application</span></span>
<span data-ttu-id="3fa67-113">A következő lépésben toocreate egy alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="3fa67-113">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="3fa67-114">Ez biztosítja, hogy az igények toosecurely kommunikálni az alkalmazást az Azure AD-információkat.</span><span class="sxs-lookup"><span data-stu-id="3fa67-114">This gives Azure AD information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="3fa67-115">hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="3fa67-115">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span>  <span data-ttu-id="3fa67-116">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="3fa67-116">Be sure to:</span></span>

* <span data-ttu-id="3fa67-117">Tartalmaznak egy **natív ügyfél** hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3fa67-117">Include a **native client** in hello application.</span></span>
* <span data-ttu-id="3fa67-118">Másolás hello **átirányítási URI-** `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="3fa67-118">Copy hello **Redirect URI** `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="3fa67-119">A fenti hello alapértelmezett URL-.</span><span class="sxs-lookup"><span data-stu-id="3fa67-119">It is hello default URL for this code sample.</span></span>
* <span data-ttu-id="3fa67-120">Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3fa67-120">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="3fa67-121">Erre később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="3fa67-121">You will need it later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="3fa67-122">Szabályzatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fa67-122">Create your policies</span></span>
<span data-ttu-id="3fa67-123">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="3fa67-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3fa67-124">Ebben a kódmintában három identitásélményt tartalmaz: regisztráció, bejelentkezés és profilszerkesztés.</span><span class="sxs-lookup"><span data-stu-id="3fa67-124">This code sample contains three identity experiences: sign up, sign in, and edit profile.</span></span> <span data-ttu-id="3fa67-125">Kell toocreate egy házirendet az egyes, lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="3fa67-125">You need toocreate a policy for each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="3fa67-126">Hello három házirend létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="3fa67-126">When you create hello three policies, be sure to:</span></span>

* <span data-ttu-id="3fa67-127">Válasszon **előfizetési Felhasználóazonosító** vagy **regisztrációs E-mail** hello Identitásszolgáltatók paneljén található.</span><span class="sxs-lookup"><span data-stu-id="3fa67-127">Choose either **User ID sign-up** or **Email sign-up** in hello identity providers blade.</span></span>
* <span data-ttu-id="3fa67-128">A regisztrációs házirendben válassza ki a **megjelenítendő nevet** és az egyéb regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="3fa67-128">Choose **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="3fa67-129">Alkalmazási jogcímnek minden házirend esetén válassza ki a **megjelenítendő nevet** és az **objektumazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="3fa67-129">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="3fa67-130">Kiválaszthat egyéb jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="3fa67-130">You can choose other claims as well.</span></span>
* <span data-ttu-id="3fa67-131">Másolás hello **neve** egyes házirendek létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="3fa67-131">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="3fa67-132">Hello előtag nem rendelkezhet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="3fa67-132">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="3fa67-133">A házirendek nevére később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="3fa67-133">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="3fa67-134">Miután sikeresen létrehozott hello három olyan szabályzatot, készen áll a toobuild még az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3fa67-134">After you have successfully created hello three policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="3fa67-135">Hello kód letöltése</span><span class="sxs-lookup"><span data-stu-id="3fa67-135">Download hello code</span></span>
<span data-ttu-id="3fa67-136">Ez az oktatóanyag kód hello [kezelése a Githubon történik](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="3fa67-136">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span></span> <span data-ttu-id="3fa67-137">Ugrás toobuild hello mintát, ha Ön is [töltse le a projektvázát tartalmazó .zip-fájlt](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="3fa67-137">toobuild hello sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span></span> <span data-ttu-id="3fa67-138">Hello vázat klónozhatja is:</span><span class="sxs-lookup"><span data-stu-id="3fa67-138">You can also clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

<span data-ttu-id="3fa67-139">befejeződött hello alkalmazás is van [elérhető .zip-fájlként](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) vagy hello `complete` hello ága adott adattár.</span><span class="sxs-lookup"><span data-stu-id="3fa67-139">hello completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) or on hello `complete` branch of hello same repository.</span></span>

<span data-ttu-id="3fa67-140">Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="3fa67-140">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="3fa67-141">Hello `TaskClient` projekt hello WPF asztali alkalmazás felhasználói hello kommunikál.</span><span class="sxs-lookup"><span data-stu-id="3fa67-141">hello `TaskClient` project is hello WPF desktop application that hello user interacts with.</span></span> <span data-ttu-id="3fa67-142">Ez az oktatóanyag céljából hello egy háttér-feladat webes API-t, minden felhasználói feladatlistát tároló, Azure-ban üzemeltetett meghívja.</span><span class="sxs-lookup"><span data-stu-id="3fa67-142">For hello purposes of this tutorial, it calls a back-end task web API, hosted in Azure, that stores each user's to-do list.</span></span>  <span data-ttu-id="3fa67-143">Nem kell toobuild hello webes API, már meg futnak.</span><span class="sxs-lookup"><span data-stu-id="3fa67-143">You do not need toobuild hello web API, we already have it running for you.</span></span>

<span data-ttu-id="3fa67-144">webes API-k biztonságosan hitelesíti a kérelmeket az Azure AD B2C segítségével hogyan toolearn tekintse meg a [webes API-k első lépések a cikk](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="3fa67-144">toolearn how a web API securely authenticates requests by using Azure AD B2C, check out the [web API getting started article](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="execute-policies"></a><span data-ttu-id="3fa67-145">Házirendek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="3fa67-145">Execute policies</span></span>
<span data-ttu-id="3fa67-146">Az alkalmazás kommunikál az Azure AD B2C hello házirend tooexecute kívánják hello HTTP-kérelem részeként megadott hitelesítési üzenetek küldésével.</span><span class="sxs-lookup"><span data-stu-id="3fa67-146">Your app communicates with Azure AD B2C by sending authentication messages that specify hello policy they want tooexecute as part of hello HTTP request.</span></span> <span data-ttu-id="3fa67-147">Az asztali alkalmazások .NET, használhatja a hello tekintse meg a Microsoft hitelesítési könyvtár (MSAL) toosend OAuth 2.0 hitelesítési üzeneteket, végrehajtási házirendek és a jogkivonatok lekérésére, a hívás web API-k.</span><span class="sxs-lookup"><span data-stu-id="3fa67-147">For .NET desktop applications, you can use hello preview Microsoft Authentication Library (MSAL) toosend OAuth 2.0 authentication messages, execute policies, and get tokens that call web APIs.</span></span>

### <a name="install-msal"></a><span data-ttu-id="3fa67-148">MSAL telepítése</span><span class="sxs-lookup"><span data-stu-id="3fa67-148">Install MSAL</span></span>
<span data-ttu-id="3fa67-149">Adja hozzá a MSAL toohello `TaskClient` projektben hello Visual Studio Csomagkezelő konzol segítségével.</span><span class="sxs-lookup"><span data-stu-id="3fa67-149">Add MSAL toohello `TaskClient` project by using hello Visual Studio Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a><span data-ttu-id="3fa67-150">Adja meg a B2C-re vonatkozó információkat</span><span class="sxs-lookup"><span data-stu-id="3fa67-150">Enter your B2C details</span></span>
<span data-ttu-id="3fa67-151">Nyissa meg hello fájl `Globals.cs` , és cserélje hello tulajdonság értékek mindegyike saját.</span><span class="sxs-lookup"><span data-stu-id="3fa67-151">Open hello file `Globals.cs` and replace each of hello property values with your own.</span></span> <span data-ttu-id="3fa67-152">Ez az osztály a teljes használható `TaskClient` tooreference általánosan használt értékeit.</span><span class="sxs-lookup"><span data-stu-id="3fa67-152">This class is used throughout `TaskClient` tooreference commonly used values.</span></span>

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-hello-publicclientapplication"></a><span data-ttu-id="3fa67-153">Hello PublicClientApplication létrehozása</span><span class="sxs-lookup"><span data-stu-id="3fa67-153">Create hello PublicClientApplication</span></span>
<span data-ttu-id="3fa67-154">hello MSAL elsődleges osztálya: `PublicClientApplication`.</span><span class="sxs-lookup"><span data-stu-id="3fa67-154">hello primary class of MSAL is `PublicClientApplication`.</span></span> <span data-ttu-id="3fa67-155">Ez az osztály az alkalmazás az Azure AD B2C hello rendszer jelöli.</span><span class="sxs-lookup"><span data-stu-id="3fa67-155">This class represents your application in hello Azure AD B2C system.</span></span> <span data-ttu-id="3fa67-156">Ha hello app initalizes hozható létre példánya `PublicClientApplication` a `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="3fa67-156">When hello app initalizes, create an instance of `PublicClientApplication` in `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="3fa67-157">Ez hello időszak alatt is használható.</span><span class="sxs-lookup"><span data-stu-id="3fa67-157">This can be used throughout hello window.</span></span>

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens toopersist when hello user closes hello app,
        // we've extended hello MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a><span data-ttu-id="3fa67-158">A regisztrációs folyamat kezdeményezéséhez</span><span class="sxs-lookup"><span data-stu-id="3fa67-158">Initiate a sign-up flow</span></span>
<span data-ttu-id="3fa67-159">Amikor egy felhasználó toosigns mentése mellett dönt, érdemes a regisztrációs folyamat által létrehozott hello regisztrációs szabályzatban használt tooinitiate.</span><span class="sxs-lookup"><span data-stu-id="3fa67-159">When a user opts toosigns up, you want tooinitiate a sign-up flow that uses hello sign-up policy you created.</span></span> <span data-ttu-id="3fa67-160">MSAL használatával egyszerűen meghívja a `pca.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="3fa67-160">By using MSAL, you just call `pca.AcquireTokenAsync(...)`.</span></span> <span data-ttu-id="3fa67-161">paraméterek túl át hello`AcquireTokenAsync(...)` határozza meg, melyik jogkivonatot kap, hello hitelesítési kérelmet, és több használt hello házirend.</span><span class="sxs-lookup"><span data-stu-id="3fa67-161">hello parameters you pass too`AcquireTokenAsync(...)` determine which token you receive, hello policy used in hello authentication request, and more.</span></span>

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use hello app's clientId here as hello scope parameter, indicating that
        // you want a token toohello your app's backend web API (represented by
        // hello cloud hosted task API).  Use hello UiOptions.ForceLogin flag to
        // indicate tooMSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in hello app that hello user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When hello request completes successfully, you can get user
        // information from hello AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After hello sign up successfully completes, display hello user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of hello policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a><span data-ttu-id="3fa67-162">A bejelentkezési folyamat kezdeményezéséhez</span><span class="sxs-lookup"><span data-stu-id="3fa67-162">Initiate a sign-in flow</span></span>
<span data-ttu-id="3fa67-163">A bejelentkezési folyamat a hello is kezdeményezhető azonos módon, hogy a regisztrációs folyamat elindítása.</span><span class="sxs-lookup"><span data-stu-id="3fa67-163">You can initiate a sign-in flow in hello same way that you initiate a sign-up flow.</span></span> <span data-ttu-id="3fa67-164">Amikor egy felhasználó bejelentkezik, ellenőrizze a hello azonos hívás tooMSAL, ezúttal a bejelentkezési házirend használatával:</span><span class="sxs-lookup"><span data-stu-id="3fa67-164">When a user signs in, make hello same call tooMSAL, this time by using your sign-in policy:</span></span>

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a><span data-ttu-id="3fa67-165">Egy profil szerkesztése folyamat kezdeményezéséhez</span><span class="sxs-lookup"><span data-stu-id="3fa67-165">Initiate an edit-profile flow</span></span>
<span data-ttu-id="3fa67-166">Ebben az esetben akkor is végre lehet hajtani egy profil szerkesztése házirend hello azonos módon:</span><span class="sxs-lookup"><span data-stu-id="3fa67-166">Again, you can execute an edit-profile policy in hello same fashion:</span></span>

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

<span data-ttu-id="3fa67-167">A fenti esetek mindegyikét olyan, MSAL vagy ad vissza egy jogkivonatot a `AuthenticationResult` vagy kivételt jelez.</span><span class="sxs-lookup"><span data-stu-id="3fa67-167">In all of these cases, MSAL either returns a token in `AuthenticationResult` or throws an exception.</span></span> <span data-ttu-id="3fa67-168">Minden alkalommal, amikor a jogkivonat beszerzése MSAL, hello használható `AuthenticationResult.User` tooupdate hello felhasználói adatok hello alkalmazás, például a felhasználói felület hello objektum.</span><span class="sxs-lookup"><span data-stu-id="3fa67-168">Each time you get a token from MSAL, you can use hello `AuthenticationResult.User` object tooupdate hello user data in hello app, such as hello UI.</span></span> <span data-ttu-id="3fa67-169">ADAL is gyorsítótárak hello token használható hello alkalmazás többi részével.</span><span class="sxs-lookup"><span data-stu-id="3fa67-169">ADAL also caches hello token for use in other parts of hello application.</span></span>

### <a name="check-for-tokens-on-app-start"></a><span data-ttu-id="3fa67-170">Ellenőrizze a jogkivonatok az alkalmazás indítása</span><span class="sxs-lookup"><span data-stu-id="3fa67-170">Check for tokens on app start</span></span>
<span data-ttu-id="3fa67-171">MSAL tookeep követése hello felhasználói bejelentkezési állapot is használható.</span><span class="sxs-lookup"><span data-stu-id="3fa67-171">You can also use MSAL tookeep track of hello user's sign-in state.</span></span>  <span data-ttu-id="3fa67-172">Az alkalmazáshoz, az azt szeretnénk, hello felhasználói tooremain jelentkezik be, akkor is, hello-alkalmazások bezárása, és nyissa meg újra.</span><span class="sxs-lookup"><span data-stu-id="3fa67-172">In this app, we want hello user tooremain signed in even after they close hello app & re-open it.</span></span>  <span data-ttu-id="3fa67-173">Vissza a hello belül `OnInitialized` bírálja felül, MSAL által használható `AcquireTokenSilent` metódus toocheck gyorsítótárazott jogkivonatokat:</span><span class="sxs-lookup"><span data-stu-id="3fa67-173">Back inside hello `OnInitialized` override, use MSAL's `AcquireTokenSilent` method toocheck for cached tokens:</span></span>

```C#
AuthenticationResult result = null;
try
{
    // If hello user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in hello cache.  Proceed without calling hello tooDo list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-hello-task-api"></a><span data-ttu-id="3fa67-174">Hello feladat API hívása</span><span class="sxs-lookup"><span data-stu-id="3fa67-174">Call hello task API</span></span>
<span data-ttu-id="3fa67-175">Most már használta MSAL tooexecute házirendek és a jogkivonatok lekérésére.</span><span class="sxs-lookup"><span data-stu-id="3fa67-175">You have now used MSAL tooexecute policies and get tokens.</span></span>  <span data-ttu-id="3fa67-176">Ha azt szeretné, hogy egy ezek jogkivonatok toocall hello feladat API toouse, újra használhatja MSAL tartozó `AcquireTokenSilent` metódus toocheck gyorsítótárazott jogkivonatokat:</span><span class="sxs-lookup"><span data-stu-id="3fa67-176">When you want toouse one these tokens toocall hello task API, you can again use MSAL's `AcquireTokenSilent` method toocheck for cached tokens:</span></span>

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want toocheck for a cached token, independent of whatever policy was used tooacquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent tooindicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch hello exception and show hello user a message.
    catch (MsalException ex)
    {
        // There is no access token in hello cache, so prompt hello user toosign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

<span data-ttu-id="3fa67-177">Ha túl az hívás hello`AcquireTokenSilentAsync(...)` sikeres lesz, és egy jogkivonat található hello gyorsítótár hello token toohello is hozzáadhat `Authorization` hello HTTP-kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="3fa67-177">When hello call too`AcquireTokenSilentAsync(...)` succeeds and a token is found in hello cache, you can add hello token toohello `Authorization` header of hello HTTP request.</span></span> <span data-ttu-id="3fa67-178">hello feladat webes API-t fogja használni a fejléc tooauthenticate hello kérelem tooread hello felhasználói feladatlistát:</span><span class="sxs-lookup"><span data-stu-id="3fa67-178">hello task web API will use this header tooauthenticate hello request tooread hello user's to-do list:</span></span>

```C#
    ...
    // Once hello token has been returned by MSAL, add it toohello http authorization header, before making hello call tooaccess hello tooDo list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call hello tooDo list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-hello-user-out"></a><span data-ttu-id="3fa67-179">Bejelentkezési hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="3fa67-179">Sign hello user out</span></span>
<span data-ttu-id="3fa67-180">Végezetül MSAL használhatja a felhasználói munkamenet hello alkalmazással, ha hello felhasználó tooend **Kijelentkezés**.  MSAL használatakor mindez törlésével az összes hello jogkivonatok hello token gyorsítótárból:</span><span class="sxs-lookup"><span data-stu-id="3fa67-180">Finally, you can use MSAL tooend a user's session with hello app when hello user selects **Sign out**.  When using MSAL, this is accomplished by clearing all of hello tokens from hello token cache:</span></span>

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of hello user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in hello browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update hello UI tooshow hello user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-hello-sample-app"></a><span data-ttu-id="3fa67-181">Hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="3fa67-181">Run hello sample app</span></span>
<span data-ttu-id="3fa67-182">Végezetül készítse el és hello minta futtatásához.</span><span class="sxs-lookup"><span data-stu-id="3fa67-182">Finally, build and run hello sample.</span></span>  <span data-ttu-id="3fa67-183">Iratkozzon fel egy e-mail cím vagy a felhasználó nevét a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3fa67-183">Sign up for hello app by using an email address or user name.</span></span> <span data-ttu-id="3fa67-184">Jelentkezzen ki, és újból be kell jelentkeznie, hello azonos felhasználói.</span><span class="sxs-lookup"><span data-stu-id="3fa67-184">Sign out and sign back in as hello same user.</span></span> <span data-ttu-id="3fa67-185">A felhasználói profil szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="3fa67-185">Edit that user's profile.</span></span> <span data-ttu-id="3fa67-186">Jelentkezzen ki, és jelentkezzen egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="3fa67-186">Sign out and sign up by using a different user.</span></span>

## <a name="add-social-idps"></a><span data-ttu-id="3fa67-187">Közösségi IDPs hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3fa67-187">Add social IDPs</span></span>
<span data-ttu-id="3fa67-188">Jelenleg hello alkalmazás támogatja-e a csak felhasználói regisztráció és bejelentkezés használó **helyi fiókok**.</span><span class="sxs-lookup"><span data-stu-id="3fa67-188">Currently, hello app supports only user sign-up and sign-in that use **local accounts**.</span></span> <span data-ttu-id="3fa67-189">Ezek a fiókok a B2C-címtárban tárolja, amely a felhasználónév és jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="3fa67-189">These are accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="3fa67-190">Az Azure AD B2C segítségével támogathatja az egyéb identitás-szolgáltatóktól (IDPs) bármely, a kód módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="3fa67-190">By using Azure AD B2C, you can add support for other identity providers (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="3fa67-191">tooadd közösségi IDPs tooyour alkalmazást, és kezdje az alábbi hello részletes utasításokat a cikkeiben.</span><span class="sxs-lookup"><span data-stu-id="3fa67-191">tooadd social IDPs tooyour app, begin by following hello detailed instructions in these articles.</span></span> <span data-ttu-id="3fa67-192">Az egyes IDP meg toosupport, egy alkalmazás tooregister kell, hogy a rendszer, és szerezzen be egy ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3fa67-192">For each IDP you want toosupport, you need tooregister an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="3fa67-193">Az IDP Facebook beállítása</span><span class="sxs-lookup"><span data-stu-id="3fa67-193">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="3fa67-194">Az IDP Google beállítása</span><span class="sxs-lookup"><span data-stu-id="3fa67-194">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="3fa67-195">Az IDP Amazon beállítása</span><span class="sxs-lookup"><span data-stu-id="3fa67-195">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="3fa67-196">Az IDP LinkedIn beállítása</span><span class="sxs-lookup"><span data-stu-id="3fa67-196">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="3fa67-197">Miután hello identity providers tooyour B2C directory, a tooinclude hello új IDPs, mint a három szabályzat hello leírása tooedit kell [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3fa67-197">After you add hello identity providers tooyour B2C directory, you need tooedit each of your three policies tooinclude hello new IDPs, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3fa67-198">A házirend mentése után futtassa újra a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3fa67-198">After you save your policies, run hello app again.</span></span> <span data-ttu-id="3fa67-199">Új IDPs fel, mert a bejelentkezési és regisztrációs beállítások a identitással kapcsolatos műveletet mindegyikében hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="3fa67-199">You should see hello new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="3fa67-200">A házirendek kísérletezhet, és figyelje meg a mintaalkalmazás hello hatással.</span><span class="sxs-lookup"><span data-stu-id="3fa67-200">You can experiment with your policies and observe hello effects on your sample app.</span></span> <span data-ttu-id="3fa67-201">Vegye fel vagy távolítsa el a IDPs, kezelheti az alkalmazás jogcímét, vagy módosítsa a regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="3fa67-201">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="3fa67-202">Kísérletet, amíg meg nem látja hogyan házirendeket, a hitelesítési kérelmek és MSAL alkalmazássá.</span><span class="sxs-lookup"><span data-stu-id="3fa67-202">Experiment until you can see how policies, authentication requests, and MSAL tie together.</span></span>

<span data-ttu-id="3fa67-203">Referenciaként hello befejeződött minta [egy .zip-fájlban is](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="3fa67-203">For reference, hello completed sample [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="3fa67-204">Ezenfelül a GitHubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="3fa67-204">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
