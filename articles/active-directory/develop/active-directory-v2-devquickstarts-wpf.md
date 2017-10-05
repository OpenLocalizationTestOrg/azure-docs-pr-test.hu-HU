---
title: "Az Azure Active Directory v2.0 .NET natív alkalmazással |} Microsoft Docs"
description: "Hogyan hozhat létre, mely aláírja a felhasználók be mindkét személyes Microsoft-Account natív .NET-alkalmazás és a munkahelyi vagy iskolai fiókját."
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
ms.openlocfilehash: 7389f55ee6fef9548abb0ca4ac1bbd0399868d47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-a-windows-desktop-app"></a><span data-ttu-id="c16c8-103">Bejelentkezés a Windows asztali alkalmazások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c16c8-103">Add sign-in to a Windows Desktop app</span></span>
<span data-ttu-id="c16c8-104">Az a v2.0-végponttól adhat gyorsan hozzá támogatja a személyes Microsoft-fiókot is az asztali alkalmazások hitelesítés és a munkahelyi vagy iskolai fiókok.</span><span class="sxs-lookup"><span data-stu-id="c16c8-104">With the the v2.0 endpoint, you can quickly add authentication to your desktop apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="c16c8-105">Emellett lehetővé teszi az alkalmazásnak, hogy biztonságosan tudjon kommunikálni a háttér webes API-t, valamint [a Microsoft Graph](https://graph.microsoft.io) és néhány a [Office 365 egyesített API-k](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span><span class="sxs-lookup"><span data-stu-id="c16c8-105">It also enables your app to securely communicate with a backend web api, as well as [the Microsoft Graph](https://graph.microsoft.io) and a few of the [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span></span>

> [!NOTE]
> <span data-ttu-id="c16c8-106">Nem minden Azure Active Directory (AD) forgatókönyvek és funkciók támogatják a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="c16c8-106">Not all Azure Active Directory (AD) scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="c16c8-107">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="c16c8-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="c16c8-108">A [az eszközön futó .NET-natív alkalmazások](active-directory-v2-flows.md#mobile-and-native-apps), az Azure AD a Microsoft Identity hitelesítési tár vagy MSAL biztosít.</span><span class="sxs-lookup"><span data-stu-id="c16c8-108">For [.NET native apps that run on a device](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD provides the Microsoft Identity Authentication Library, or MSAL.</span></span>  <span data-ttu-id="c16c8-109">MSAL meg azzal a céllal életben megkönnyítik az alkalmazások jogkivonatok lekérésére történt a webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c16c8-109">MSAL's sole purpose in life is to make it easy for your app to get tokens for calling web services.</span></span>  <span data-ttu-id="c16c8-110">Annak bemutatásához, milyen könnyű van, itt azt fogja hozza létre a .NET WPF feladatlista alkalmazását, amelyek:</span><span class="sxs-lookup"><span data-stu-id="c16c8-110">To demonstrate just how easy it is, here we'll build a .NET WPF To-Do List app that:</span></span>

* <span data-ttu-id="c16c8-111">A felhasználó bejelentkezik, és lekérdezi a hozzáférési jogkivonatok használatával a [OAuth 2.0 hitelesítési protokoll](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="c16c8-111">Signs the user in & gets access tokens using the [OAuth 2.0 authentication protocol](active-directory-v2-protocols.md).</span></span>
* <span data-ttu-id="c16c8-112">Biztonságosan meghívja a háttérkiszolgálón feladatlista webszolgáltatáshoz, amely OAuth 2.0 is védi.</span><span class="sxs-lookup"><span data-stu-id="c16c8-112">Securely calls a backend To-Do List web service, which is also secured by OAuth 2.0.</span></span>
* <span data-ttu-id="c16c8-113">A felhasználó kijelentkezik.</span><span class="sxs-lookup"><span data-stu-id="c16c8-113">Signs the user out.</span></span>

## <a name="download-sample-code"></a><span data-ttu-id="c16c8-114">Mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="c16c8-114">Download sample code</span></span>
<span data-ttu-id="c16c8-115">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="c16c8-115">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span></span>  <span data-ttu-id="c16c8-116">Követéséhez is [töltse le az alkalmazás vázát egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="c16c8-116">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

<span data-ttu-id="c16c8-117">A kész alkalmazásról, valamint az oktatóanyag végén valósul meg.</span><span class="sxs-lookup"><span data-stu-id="c16c8-117">The completed app is provided at the end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="c16c8-118">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c16c8-118">Register an app</span></span>
<span data-ttu-id="c16c8-119">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="c16c8-119">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="c16c8-120">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="c16c8-120">Make sure to:</span></span>

* <span data-ttu-id="c16c8-121">Másolja le a **alkalmazásazonosító** be az alkalmazáshoz hozzárendelt szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="c16c8-121">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="c16c8-122">Adja hozzá a **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="c16c8-122">Add the **Mobile** platform for your app.</span></span>

## <a name="install--configure-msal"></a><span data-ttu-id="c16c8-123">Telepítse & MSAL konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c16c8-123">Install & Configure MSAL</span></span>
<span data-ttu-id="c16c8-124">Most, hogy az alkalmazás regisztrálva a Microsoft, MSAL telepítse, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="c16c8-124">Now that you have an app registered with Microsoft, you can install MSAL and write your identity-related code.</span></span>  <span data-ttu-id="c16c8-125">Ahhoz, hogy tudjon kommunikálni a v2.0-végponttal való MSAL meg kell biztosítania, bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="c16c8-125">In order for MSAL to be able to communicate the v2.0 endpoint, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="c16c8-126">Első lépésként MSAL ad hozzá a TodoListClient projekt a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="c16c8-126">Begin by adding MSAL to the TodoListClient project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* <span data-ttu-id="c16c8-127">A TodoListClient projektben nyissa meg a `app.config`.</span><span class="sxs-lookup"><span data-stu-id="c16c8-127">In the TodoListClient project, open `app.config`.</span></span>  <span data-ttu-id="c16c8-128">Cserélje le az értékeket az elemek a `<appSettings>` szakaszban az alkalmazás regisztrációs portált megadott értékeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="c16c8-128">Replace the values of the elements in the `<appSettings>` section to reflect the values you input into the app registration portal.</span></span>  <span data-ttu-id="c16c8-129">A kód minden alkalommal MSAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c16c8-129">Your code will reference these values whenever it uses MSAL.</span></span>
  
  * <span data-ttu-id="c16c8-130">A `ida:ClientId` van a **alkalmazásazonosító** az alkalmazás a portálról másolta.</span><span class="sxs-lookup"><span data-stu-id="c16c8-130">The `ida:ClientId` is the **Application Id** of your app you copied from the portal.</span></span>
* <span data-ttu-id="c16c8-131">Nyissa meg a TodoList-projekt, `web.config` a projekt gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="c16c8-131">In the TodoList-Service project, open `web.config` in the root of the project.</span></span>  
  
  * <span data-ttu-id="c16c8-132">Cserélje le a `ida:Audience` azonos érték **alkalmazásazonosító** a portálról.</span><span class="sxs-lookup"><span data-stu-id="c16c8-132">Replace the `ida:Audience` value with the same **Application Id** from the portal.</span></span>

## <a name="use-msal-to-get-tokens"></a><span data-ttu-id="c16c8-133">A jogkivonatok lekérésére MSAL segítségével</span><span class="sxs-lookup"><span data-stu-id="c16c8-133">Use MSAL to get tokens</span></span>
<span data-ttu-id="c16c8-134">Az alapelv MSAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja a `app.AcquireToken(...)`, és a többi MSAL.</span><span class="sxs-lookup"><span data-stu-id="c16c8-134">The basic principle behind MSAL is that whenever your app needs an access token, you simply call `app.AcquireToken(...)`, and MSAL does the rest.</span></span>  

* <span data-ttu-id="c16c8-135">Az a `TodoListClient` projektben nyissa meg `MainWindow.xaml.cs` , és keresse meg a `OnInitialized(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="c16c8-135">In the `TodoListClient` project, open `MainWindow.xaml.cs` and locate the `OnInitialized(...)` method.</span></span>  <span data-ttu-id="c16c8-136">Az első lépés az, hogy az alkalmazás inicializálása `PublicClientApplication` -natív alkalmazások képviselő MSAL tartozó elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="c16c8-136">The first step is to initialize your app's `PublicClientApplication` - MSAL's primary class representing native applications.</span></span>  <span data-ttu-id="c16c8-137">Ez azért, ahol át MSAL kommunikálni az Azure AD, és mondja el, akkor jogkivonatok gyorsítótárazásának kell koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="c16c8-137">This is where you pass MSAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* <span data-ttu-id="c16c8-138">Az alkalmazás indításakor azt szeretnénk, ha a felhasználó már bejelentkezett az alkalmazásba ellenőrizheti, hogy végzi el.</span><span class="sxs-lookup"><span data-stu-id="c16c8-138">When the app starts up, we want to check and see if the user is already signed into the app.</span></span>  <span data-ttu-id="c16c8-139">Azonban a bejelentkezési felhasználói felület meghívása még nem szeretnénk - lesz biztosítjuk, kattintson a "Bejelentkezés" ehhez a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="c16c8-139">However, we don't want to invoke a sign-in UI just yet - we'll make the user click "Sign In" to do so.</span></span>  <span data-ttu-id="c16c8-140">A is a `OnInitialized(...)` módszert:</span><span class="sxs-lookup"><span data-stu-id="c16c8-140">Also in the `OnInitialized(...)` method:</span></span>

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
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

* <span data-ttu-id="c16c8-141">Ha a felhasználó nem jelentkezett be, és a "Bejelentkezés" gombra kattintva, szeretnénk meghívni egy bejelentkezési felhasználói felület és a felhasználó adja meg a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c16c8-141">If the user is not signed in and they click the "Sign In" button, we want to invoke a login UI and have the user enter their credentials.</span></span>  <span data-ttu-id="c16c8-142">A bejelentkezési gomb kezelő megvalósításához:</span><span class="sxs-lookup"><span data-stu-id="c16c8-142">Implement the Sign-In button handler:</span></span>

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

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
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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

* <span data-ttu-id="c16c8-143">Ha a felhasználó sikeresen jelentkezik be, MSAL fogadja és gyorsítótárazza a jogkivonatot, és folytathatja a hívás a `GetTodoList()` metódus az vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="c16c8-143">If the user successfully signs-in, MSAL will receive and cache a token for you, and you can proceed to call the `GetTodoList()` method with confidence.</span></span>  <span data-ttu-id="c16c8-144">Összes felhasználói feladatok beolvasása marad, hogy alkalmazza a `GetTodoList()` metódust.</span><span class="sxs-lookup"><span data-stu-id="c16c8-144">All that's left to get a user's tasks is to implement the `GetTodoList()` method.</span></span>

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

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

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

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

## <a name="run"></a><span data-ttu-id="c16c8-145">Futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="c16c8-145">Run</span></span>
<span data-ttu-id="c16c8-146">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="c16c8-146">Congratulations!</span></span> <span data-ttu-id="c16c8-147">Most már rendelkezik egy működő .NET WPF-alkalmazás, amely képes a felhasználók hitelesítéséhez és biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="c16c8-147">You now have a working .NET WPF app that has the ability to authenticate users & securely call Web APIs using OAuth 2.0.</span></span>  <span data-ttu-id="c16c8-148">A mindkét projekt futtatásához, és jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c16c8-148">Run your both projects, and sign in with either a personal Microsoft account or a work or school account.</span></span>  <span data-ttu-id="c16c8-149">Feladatok hozzáadása, a felhasználó tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="c16c8-149">Add tasks to that user's To-Do list.</span></span>  <span data-ttu-id="c16c8-150">Jelentkezzen ki, és jelentkezzen be egy másik felhasználó megtekintéséhez a tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="c16c8-150">Sign out, and sign back in as another user to view their To-Do list.</span></span>  <span data-ttu-id="c16c8-151">Zárja be az alkalmazást, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="c16c8-151">Close the app, and re-run it.</span></span>  <span data-ttu-id="c16c8-152">Figyelje meg, hogyan sértetlenek maradnak a felhasználói munkamenet -, amelyek oka, hogy az alkalmazás gyorsítótárazza a tokenek egy helyi fájlban.</span><span class="sxs-lookup"><span data-stu-id="c16c8-152">Notice how the user's session remains intact - that is because the app caches tokens in a local file.</span></span>

<span data-ttu-id="c16c8-153">MSAL megkönnyíti az alkalmazásba, az általános identitás-szolgáltatások átfogó személyes és munkahelyi fiókokat egyaránt használ.</span><span class="sxs-lookup"><span data-stu-id="c16c8-153">MSAL makes it easy to incorporate common identity features into your app, using both personal and work accounts.</span></span>  <span data-ttu-id="c16c8-154">Ez gondoskodik a dirty munkát meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, a felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a bemutató.</span><span class="sxs-lookup"><span data-stu-id="c16c8-154">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="c16c8-155">Biztosan tudni, hogy szüksége egy egyetlen API-hívással `app.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="c16c8-155">All you really need to know is a single API call, `app.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="c16c8-156">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="c16c8-156">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="c16c8-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c16c8-157">Next steps</span></span>
<span data-ttu-id="c16c8-158">Ön most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="c16c8-158">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="c16c8-159">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="c16c8-159">You may want to try:</span></span>

* [<span data-ttu-id="c16c8-160">A v2.0-végponttal a TodoListService webes API biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="c16c8-160">Securing the TodoListService Web API with the v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-dotnet-api.md)

<span data-ttu-id="c16c8-161">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="c16c8-161">For additional resources, check out:</span></span>  

* [<span data-ttu-id="c16c8-162">A v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="c16c8-162">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="c16c8-163">StackOverflow "msal" címke >></span><span class="sxs-lookup"><span data-stu-id="c16c8-163">StackOverflow "msal" tag >></span></span>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="c16c8-164">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="c16c8-164">Get security updates for our products</span></span>
<span data-ttu-id="c16c8-165">Javasoljuk, hogy kérjen értesítést a bekövetkező biztonsági incidensekről. Látogasson el [erre a lapra](https://technet.microsoft.com/security/dd252948), és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="c16c8-165">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

