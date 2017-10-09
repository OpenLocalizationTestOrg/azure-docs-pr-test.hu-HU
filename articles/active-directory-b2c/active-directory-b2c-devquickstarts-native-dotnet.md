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
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Az Azure AD B2C: A Windows asztali alkalmazás elkészítésére.
Azure Active Directory (Azure AD) B2C használatával néhány néhány lépés elvégzésével hatékony önkiszolgáló identitáskezelési felügyeleti funkciók tooyour egy asztali alkalmazás adhat hozzá. Ez a cikk bemutatja, hogyan toocreate egy .NET Windows megjelenítési alaprendszer (WPF) "Feladatlista" alkalmazást, amely tartalmazza a felhasználói regisztráció, bejelentkezés és profilkezelést. hello app támogatni fogja előfizetési, és jelentkezzen be egy felhasználónevet vagy e-mailek használatával. Azt is támogatni fogja előfizetési, és jelentkezzen be például a Facebookhoz és a Google közösségi fiókokkal.

## <a name="get-an-azure-ad-b2c-directory"></a>Az Azure AD B2C-címtár beszerzése
Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.  A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást. Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.

## <a name="create-an-application"></a>Alkalmazás létrehozása
A következő lépésben toocreate egy alkalmazást a B2C-címtárban. Ez biztosítja, hogy az igények toosecurely kommunikálni az alkalmazást az Azure AD-információkat. hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).  Ügyeljen arra, hogy:

* Tartalmaznak egy **natív ügyfél** hello alkalmazásban.
* Másolás hello **átirányítási URI-** `urn:ietf:wg:oauth:2.0:oob`. A fenti hello alapértelmezett URL-.
* Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást. Erre később még szüksége lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Szabályzatok létrehozása
Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg. Ebben a kódmintában három identitásélményt tartalmaz: regisztráció, bejelentkezés és profilszerkesztés. Kell toocreate egy házirendet az egyes, lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Hello három házirend létrehozásakor ügyeljen arra, hogy:

* Válasszon **előfizetési Felhasználóazonosító** vagy **regisztrációs E-mail** hello Identitásszolgáltatók paneljén található.
* A regisztrációs házirendben válassza ki a **megjelenítendő nevet** és az egyéb regisztrációs attribútumokat.
* Alkalmazási jogcímnek minden házirend esetén válassza ki a **megjelenítendő nevet** és az **objektumazonosítót**. Kiválaszthat egyéb jogcímeket is.
* Másolás hello **neve** egyes házirendek létrehozása után. Hello előtag nem rendelkezhet `b2c_1_`.  A házirendek nevére később még szüksége lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután sikeresen létrehozott hello három olyan szabályzatot, készen áll a toobuild még az alkalmazás.

## <a name="download-hello-code"></a>Hello kód letöltése
Ez az oktatóanyag kód hello [kezelése a Githubon történik](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Ugrás toobuild hello mintát, ha Ön is [töltse le a projektvázát tartalmazó .zip-fájlt](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Hello vázat klónozhatja is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

befejeződött hello alkalmazás is van [elérhető .zip-fájlként](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) vagy hello `complete` hello ága adott adattár.

Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult. Hello `TaskClient` projekt hello WPF asztali alkalmazás felhasználói hello kommunikál. Ez az oktatóanyag céljából hello egy háttér-feladat webes API-t, minden felhasználói feladatlistát tároló, Azure-ban üzemeltetett meghívja.  Nem kell toobuild hello webes API, már meg futnak.

webes API-k biztonságosan hitelesíti a kérelmeket az Azure AD B2C segítségével hogyan toolearn tekintse meg a [webes API-k első lépések a cikk](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Házirendek végrehajtása
Az alkalmazás kommunikál az Azure AD B2C hello házirend tooexecute kívánják hello HTTP-kérelem részeként megadott hitelesítési üzenetek küldésével. Az asztali alkalmazások .NET, használhatja a hello tekintse meg a Microsoft hitelesítési könyvtár (MSAL) toosend OAuth 2.0 hitelesítési üzeneteket, végrehajtási házirendek és a jogkivonatok lekérésére, a hívás web API-k.

### <a name="install-msal"></a>MSAL telepítése
Adja hozzá a MSAL toohello `TaskClient` projektben hello Visual Studio Csomagkezelő konzol segítségével.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Adja meg a B2C-re vonatkozó információkat
Nyissa meg hello fájl `Globals.cs` , és cserélje hello tulajdonság értékek mindegyike saját. Ez az osztály a teljes használható `TaskClient` tooreference általánosan használt értékeit.

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

### <a name="create-hello-publicclientapplication"></a>Hello PublicClientApplication létrehozása
hello MSAL elsődleges osztálya: `PublicClientApplication`. Ez az osztály az alkalmazás az Azure AD B2C hello rendszer jelöli. Ha hello app initalizes hozható létre példánya `PublicClientApplication` a `MainWindow.xaml.cs`. Ez hello időszak alatt is használható.

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

### <a name="initiate-a-sign-up-flow"></a>A regisztrációs folyamat kezdeményezéséhez
Amikor egy felhasználó toosigns mentése mellett dönt, érdemes a regisztrációs folyamat által létrehozott hello regisztrációs szabályzatban használt tooinitiate. MSAL használatával egyszerűen meghívja a `pca.AcquireTokenAsync(...)`. paraméterek túl át hello`AcquireTokenAsync(...)` határozza meg, melyik jogkivonatot kap, hello hitelesítési kérelmet, és több használt hello házirend.

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

### <a name="initiate-a-sign-in-flow"></a>A bejelentkezési folyamat kezdeményezéséhez
A bejelentkezési folyamat a hello is kezdeményezhető azonos módon, hogy a regisztrációs folyamat elindítása. Amikor egy felhasználó bejelentkezik, ellenőrizze a hello azonos hívás tooMSAL, ezúttal a bejelentkezési házirend használatával:

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

### <a name="initiate-an-edit-profile-flow"></a>Egy profil szerkesztése folyamat kezdeményezéséhez
Ebben az esetben akkor is végre lehet hajtani egy profil szerkesztése házirend hello azonos módon:

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

A fenti esetek mindegyikét olyan, MSAL vagy ad vissza egy jogkivonatot a `AuthenticationResult` vagy kivételt jelez. Minden alkalommal, amikor a jogkivonat beszerzése MSAL, hello használható `AuthenticationResult.User` tooupdate hello felhasználói adatok hello alkalmazás, például a felhasználói felület hello objektum. ADAL is gyorsítótárak hello token használható hello alkalmazás többi részével.

### <a name="check-for-tokens-on-app-start"></a>Ellenőrizze a jogkivonatok az alkalmazás indítása
MSAL tookeep követése hello felhasználói bejelentkezési állapot is használható.  Az alkalmazáshoz, az azt szeretnénk, hello felhasználói tooremain jelentkezik be, akkor is, hello-alkalmazások bezárása, és nyissa meg újra.  Vissza a hello belül `OnInitialized` bírálja felül, MSAL által használható `AcquireTokenSilent` metódus toocheck gyorsítótárazott jogkivonatokat:

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

## <a name="call-hello-task-api"></a>Hello feladat API hívása
Most már használta MSAL tooexecute házirendek és a jogkivonatok lekérésére.  Ha azt szeretné, hogy egy ezek jogkivonatok toocall hello feladat API toouse, újra használhatja MSAL tartozó `AcquireTokenSilent` metódus toocheck gyorsítótárazott jogkivonatokat:

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

Ha túl az hívás hello`AcquireTokenSilentAsync(...)` sikeres lesz, és egy jogkivonat található hello gyorsítótár hello token toohello is hozzáadhat `Authorization` hello HTTP-kérelem fejlécében. hello feladat webes API-t fogja használni a fejléc tooauthenticate hello kérelem tooread hello felhasználói feladatlistát:

```C#
    ...
    // Once hello token has been returned by MSAL, add it toohello http authorization header, before making hello call tooaccess hello tooDo list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call hello tooDo list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-hello-user-out"></a>Bejelentkezési hello felhasználó
Végezetül MSAL használhatja a felhasználói munkamenet hello alkalmazással, ha hello felhasználó tooend **Kijelentkezés**.  MSAL használatakor mindez törlésével az összes hello jogkivonatok hello token gyorsítótárból:

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

## <a name="run-hello-sample-app"></a>Hello mintaalkalmazás futtatása
Végezetül készítse el és hello minta futtatásához.  Iratkozzon fel egy e-mail cím vagy a felhasználó nevét a hello alkalmazást. Jelentkezzen ki, és újból be kell jelentkeznie, hello azonos felhasználói. A felhasználói profil szerkesztése. Jelentkezzen ki, és jelentkezzen egy másik felhasználó.

## <a name="add-social-idps"></a>Közösségi IDPs hozzáadása
Jelenleg hello alkalmazás támogatja-e a csak felhasználói regisztráció és bejelentkezés használó **helyi fiókok**. Ezek a fiókok a B2C-címtárban tárolja, amely a felhasználónév és jelszó használata. Az Azure AD B2C segítségével támogathatja az egyéb identitás-szolgáltatóktól (IDPs) bármely, a kód módosítása nélkül.

tooadd közösségi IDPs tooyour alkalmazást, és kezdje az alábbi hello részletes utasításokat a cikkeiben. Az egyes IDP meg toosupport, egy alkalmazás tooregister kell, hogy a rendszer, és szerezzen be egy ügyfél-azonosítót.

* [Az IDP Facebook beállítása](active-directory-b2c-setup-fb-app.md)
* [Az IDP Google beállítása](active-directory-b2c-setup-goog-app.md)
* [Az IDP Amazon beállítása](active-directory-b2c-setup-amzn-app.md)
* [Az IDP LinkedIn beállítása](active-directory-b2c-setup-li-app.md)

Miután hello identity providers tooyour B2C directory, a tooinclude hello új IDPs, mint a három szabályzat hello leírása tooedit kell [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md). A házirend mentése után futtassa újra a hello alkalmazást. Új IDPs fel, mert a bejelentkezési és regisztrációs beállítások a identitással kapcsolatos műveletet mindegyikében hello kell megjelennie.

A házirendek kísérletezhet, és figyelje meg a mintaalkalmazás hello hatással. Vegye fel vagy távolítsa el a IDPs, kezelheti az alkalmazás jogcímét, vagy módosítsa a regisztrációs attribútumokat. Kísérletet, amíg meg nem látja hogyan házirendeket, a hitelesítési kérelmek és MSAL alkalmazássá.

Referenciaként hello befejeződött minta [egy .zip-fájlban is](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Ezenfelül a GitHubból is klónozhatja:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
