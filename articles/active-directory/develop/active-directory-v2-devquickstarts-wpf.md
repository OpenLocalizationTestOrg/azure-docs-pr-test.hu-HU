---
title: "aaaAzure Active Directory v2.0 .NET natív alkalmazással |} Microsoft Docs"
description: "Hogyan toobuild natív .NET-alkalmazás, amely bejelentkezik felhasználók mindkét személyes Microsoft Account és munkahelyi vagy iskolai fiókját."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 46d81e09-bad0-44ce-9026-881805976e72
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/30/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9418eeba02b800feee5cb00219574eb16506f0a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-windows-desktop-app"></a>Bejelentkezési tooa Windows asztali alkalmazás hozzáadása
Hello hello v2.0-végponttal gyorsan hitelesítési tooyour asztali alkalmazások személyes Microsoft-fiókot is támogatása és a munkahelyi vagy iskolai fiókokat is hozzáadhat.  Azt is lehetővé teszi, hogy az alkalmazás toosecurely kommunikálni a háttérkiszolgálón webes API-t, valamint [Microsoft Graph hello](https://graph.microsoft.io) és hello néhány [Office 365 egyesített API-k](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [!NOTE]
> Nem minden Azure Active Directory (AD) forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

A [az eszközön futó .NET-natív alkalmazások](active-directory-v2-flows.md#mobile-and-native-apps), az Azure AD a Microsoft Identity hitelesítési tár vagy MSAL hello biztosít.  MSAL meg azzal a céllal életben toomake megkönnyítik az alkalmazás tooget jogkivonatok webszolgáltatások hívásakor.  toodemonstrate milyen könnyű van, itt azt fogja hozza létre a .NET WPF feladatlista alkalmazását, amelyek:

* Jeleket a felhasználó hello & lekérdezi hozzáférési jogkivonatok használatával hello [OAuth 2.0 hitelesítési protokoll](active-directory-v2-protocols.md).
* Biztonságosan meghívja a háttérkiszolgálón feladatlista webszolgáltatáshoz, amely OAuth 2.0 is védi.
* Jeleket hello felhasználói ki.

## <a name="download-sample-code"></a>Mintakód letöltése
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

hello végén, valamint az oktatóanyag befejezése hello app valósul meg.

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.
* Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.

## <a name="install--configure-msal"></a>Telepítse & MSAL konfigurálása
Most, hogy az alkalmazás regisztrálva a Microsoft, MSAL telepítse, és az identitás-kapcsolódó kód írása.  Ahhoz, hogy MSAL toobe képes toocommunicate hello v2.0-végpontra, tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.

* Első lépésként hozzáadása MSAL toohello TodoListClient projekt hello Csomagkezelő konzol használatával.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* Hello TodoListClient projektben nyissa meg `app.config`.  Cserélje le a hello értékek hello hello elemeinek `<appSettings>` szakasz tooreflect hello értékek bemeneti hello app regisztrációs portált.  A kód minden alkalommal MSAL hivatkozik ezeket az értékeket.
  
  * Hello `ida:ClientId` hello van **alkalmazásazonosító** hello portálról másolta az alkalmazás.
* Hello TodoList-szolgáltatás projektben nyissa meg `web.config` hello hello projekt gyökerében található.  
  
  * Cserélje le a hello `ida:Audience` értékét a hello azonos **alkalmazásazonosító** hello portálról.

## <a name="use-msal-tooget-tokens"></a>Használjon MSAL tooget tokeneket
hello alapelv MSAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja a `app.AcquireToken(...)`, és MSAL hello rest.  

* A hello `TodoListClient` projektben nyissa meg `MainWindow.xaml.cs` , és keresse meg a hello `OnInitialized(...)` metódust.  első lépés hello tooinitialize van az alkalmazás `PublicClientApplication` -natív alkalmazások képviselő MSAL tartozó elsődleges osztály.  Ez az adott át MSAL hello koordinátát kell toocommunicate az Azure ad-val, és mondja el, hogyan toocache jogkivonatokat.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* Hello alkalmazás indításakor azt szeretné, hogy toocheck, és tekintse meg, ha hello felhasználó már aláírt hello alkalmazásba.  Azonban nem szeretnénk olyan bejelentkezési felhasználói felület tooinvoke még – hello felhasználói, kattintson a "Bejelentkezés" toodo lesz biztosítjuk.  Emellett a hello `OnInitialized(...)` módszert:

```C#
// As hello app starts, we want toocheck toosee if hello user is already signed in.
// You can do so by trying tooget a token from MSAL, using hello method
// AcquireTokenSilent.  This forces MSAL toothrow an exception if it cannot
// get a token for hello user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in hello cache - or MSAL was able tooget a new oen via refresh token.
    // Proceed toofetch hello user's tasks from hello TodoListService via hello GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, hello app should take no action,
        // and simply show hello user hello sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

* Hello felhasználó nem jelentkezett be, és hello "Bejelentkezés" gombra kattintva, ha azt szeretné, hogy a bejelentkezési felhasználói felület tooinvoke, és adja meg a hitelesítő adataik hello felhasználó.  Hello bejelentkezési gomb kezelő megvalósításához:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign hello user out if they clicked hello "Clear Cache" button

// If hello user clicked hello 'Sign-In' button, force
// MSAL tooprompt hello user for credentials by using
// AcquireTokenAsync, a method that is guaranteed tooshow a prompt toohello user.
// MSAL will get a token for hello TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If hello user canceled hello login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by hello user");
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


}
```

* Ha hello felhasználó sikeresen jelentkezik be, MSAL fogadja és gyorsítótárazza a jogkivonatot, és folytathatja a toocall hello `GetTodoList()` metódus az vetett bizalmat.  A felhasználói feladatok elhagyta tooget csak a tooimplement hello `GetTodoList()` metódust.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try tooget an access token toocall hello TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL toothrow an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show hello user a message
    // and let them click hello Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once hello token has been returned by MSAL,
// add it toohello http authorization header,
// before making hello call tooaccess hello tooDo list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When hello user is done managing their To-Do List, they may finally sign out of hello app by clicking hello "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If hello user clicked hello 'clear cache' button,
        // clear hello MSAL token cache and show hello user as signed out.
        // It's also necessary tooclear hello cookies from hello browser
        // control so hello next user has a chance toosign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Futtassa a következőt:
Gratulálunk! Most már rendelkezik egy működő .NET WPF-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók & biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja.  A mindkét projekt futtatásához, és jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal.  Adja hozzá a feladatok toothat felhasználók feladatlistáit.  Jelentkezzen ki, és jelentkezzen be ismét egy másik felhasználó tooview a tennivalók listájára.  Hello-alkalmazások bezárása, és futtassa újból.  Figyelje meg, hogyan hello felhasználói munkamenet változatlanul -, mert hello app gyorsítótárazza a tokenek egy helyi fájlban.

MSAL segítségével könnyen tooincorporate általános identitás-szolgáltatások az alkalmazásba személyes és munkahelyi fiókokat egyaránt használ.  Azt gondoskodik az összes hello dirty munkahelyi meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a.  Valóban tooknow szüksége egy egyetlen API-hívással `app.AcquireTokenAsync(...)`.

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Következő lépések
Ön most már továbbléphet az összetettebb témákra.  Érdemes lehet tootry:

* [Hello v2.0-végponttal hello TodoListService webes API biztonságossá tétele](active-directory-v2-devquickstarts-dotnet-api.md)

További forrásokért tekintse meg:  

* [hello v2.0 – útmutató fejlesztőknek >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "msal" címke >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.

