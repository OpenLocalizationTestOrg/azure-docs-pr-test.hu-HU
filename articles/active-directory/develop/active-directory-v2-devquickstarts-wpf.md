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
# <a name="add-sign-in-tooa-windows-desktop-app"></a><span data-ttu-id="91c8c-103">Bejelentkezési tooa Windows asztali alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91c8c-103">Add sign-in tooa Windows Desktop app</span></span>
<span data-ttu-id="91c8c-104">Hello hello v2.0-végponttal gyorsan hitelesítési tooyour asztali alkalmazások személyes Microsoft-fiókot is támogatása és a munkahelyi vagy iskolai fiókokat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="91c8c-104">With hello hello v2.0 endpoint, you can quickly add authentication tooyour desktop apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="91c8c-105">Azt is lehetővé teszi, hogy az alkalmazás toosecurely kommunikálni a háttérkiszolgálón webes API-t, valamint [Microsoft Graph hello](https://graph.microsoft.io) és hello néhány [Office 365 egyesített API-k](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span><span class="sxs-lookup"><span data-stu-id="91c8c-105">It also enables your app toosecurely communicate with a backend web api, as well as [hello Microsoft Graph](https://graph.microsoft.io) and a few of hello [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span></span>

> [!NOTE]
> <span data-ttu-id="91c8c-106">Nem minden Azure Active Directory (AD) forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="91c8c-106">Not all Azure Active Directory (AD) scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="91c8c-107">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="91c8c-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="91c8c-108">A [az eszközön futó .NET-natív alkalmazások](active-directory-v2-flows.md#mobile-and-native-apps), az Azure AD a Microsoft Identity hitelesítési tár vagy MSAL hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="91c8c-108">For [.NET native apps that run on a device](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD provides hello Microsoft Identity Authentication Library, or MSAL.</span></span>  <span data-ttu-id="91c8c-109">MSAL meg azzal a céllal életben toomake megkönnyítik az alkalmazás tooget jogkivonatok webszolgáltatások hívásakor.</span><span class="sxs-lookup"><span data-stu-id="91c8c-109">MSAL's sole purpose in life is toomake it easy for your app tooget tokens for calling web services.</span></span>  <span data-ttu-id="91c8c-110">toodemonstrate milyen könnyű van, itt azt fogja hozza létre a .NET WPF feladatlista alkalmazását, amelyek:</span><span class="sxs-lookup"><span data-stu-id="91c8c-110">toodemonstrate just how easy it is, here we'll build a .NET WPF To-Do List app that:</span></span>

* <span data-ttu-id="91c8c-111">Jeleket a felhasználó hello & lekérdezi hozzáférési jogkivonatok használatával hello [OAuth 2.0 hitelesítési protokoll](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="91c8c-111">Signs hello user in & gets access tokens using hello [OAuth 2.0 authentication protocol](active-directory-v2-protocols.md).</span></span>
* <span data-ttu-id="91c8c-112">Biztonságosan meghívja a háttérkiszolgálón feladatlista webszolgáltatáshoz, amely OAuth 2.0 is védi.</span><span class="sxs-lookup"><span data-stu-id="91c8c-112">Securely calls a backend To-Do List web service, which is also secured by OAuth 2.0.</span></span>
* <span data-ttu-id="91c8c-113">Jeleket hello felhasználói ki.</span><span class="sxs-lookup"><span data-stu-id="91c8c-113">Signs hello user out.</span></span>

## <a name="download-sample-code"></a><span data-ttu-id="91c8c-114">Mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="91c8c-114">Download sample code</span></span>
<span data-ttu-id="91c8c-115">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="91c8c-115">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span></span>  <span data-ttu-id="91c8c-116">toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="91c8c-116">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

<span data-ttu-id="91c8c-117">hello végén, valamint az oktatóanyag befejezése hello app valósul meg.</span><span class="sxs-lookup"><span data-stu-id="91c8c-117">hello completed app is provided at hello end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="91c8c-118">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="91c8c-118">Register an app</span></span>
<span data-ttu-id="91c8c-119">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="91c8c-119">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="91c8c-120">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="91c8c-120">Make sure to:</span></span>

* <span data-ttu-id="91c8c-121">Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="91c8c-121">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="91c8c-122">Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="91c8c-122">Add hello **Mobile** platform for your app.</span></span>

## <a name="install--configure-msal"></a><span data-ttu-id="91c8c-123">Telepítse & MSAL konfigurálása</span><span class="sxs-lookup"><span data-stu-id="91c8c-123">Install & Configure MSAL</span></span>
<span data-ttu-id="91c8c-124">Most, hogy az alkalmazás regisztrálva a Microsoft, MSAL telepítse, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="91c8c-124">Now that you have an app registered with Microsoft, you can install MSAL and write your identity-related code.</span></span>  <span data-ttu-id="91c8c-125">Ahhoz, hogy MSAL toobe képes toocommunicate hello v2.0-végpontra, tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="91c8c-125">In order for MSAL toobe able toocommunicate hello v2.0 endpoint, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="91c8c-126">Első lépésként hozzáadása MSAL toohello TodoListClient projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="91c8c-126">Begin by adding MSAL toohello TodoListClient project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* <span data-ttu-id="91c8c-127">Hello TodoListClient projektben nyissa meg `app.config`.</span><span class="sxs-lookup"><span data-stu-id="91c8c-127">In hello TodoListClient project, open `app.config`.</span></span>  <span data-ttu-id="91c8c-128">Cserélje le a hello értékek hello hello elemeinek `<appSettings>` szakasz tooreflect hello értékek bemeneti hello app regisztrációs portált.</span><span class="sxs-lookup"><span data-stu-id="91c8c-128">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values you input into hello app registration portal.</span></span>  <span data-ttu-id="91c8c-129">A kód minden alkalommal MSAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="91c8c-129">Your code will reference these values whenever it uses MSAL.</span></span>
  
  * <span data-ttu-id="91c8c-130">Hello `ida:ClientId` hello van **alkalmazásazonosító** hello portálról másolta az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="91c8c-130">hello `ida:ClientId` is hello **Application Id** of your app you copied from hello portal.</span></span>
* <span data-ttu-id="91c8c-131">Hello TodoList-szolgáltatás projektben nyissa meg `web.config` hello hello projekt gyökerében található.</span><span class="sxs-lookup"><span data-stu-id="91c8c-131">In hello TodoList-Service project, open `web.config` in hello root of hello project.</span></span>  
  
  * <span data-ttu-id="91c8c-132">Cserélje le a hello `ida:Audience` értékét a hello azonos **alkalmazásazonosító** hello portálról.</span><span class="sxs-lookup"><span data-stu-id="91c8c-132">Replace hello `ida:Audience` value with hello same **Application Id** from hello portal.</span></span>

## <a name="use-msal-tooget-tokens"></a><span data-ttu-id="91c8c-133">Használjon MSAL tooget tokeneket</span><span class="sxs-lookup"><span data-stu-id="91c8c-133">Use MSAL tooget tokens</span></span>
<span data-ttu-id="91c8c-134">hello alapelv MSAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja a `app.AcquireToken(...)`, és MSAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="91c8c-134">hello basic principle behind MSAL is that whenever your app needs an access token, you simply call `app.AcquireToken(...)`, and MSAL does hello rest.</span></span>  

* <span data-ttu-id="91c8c-135">A hello `TodoListClient` projektben nyissa meg `MainWindow.xaml.cs` , és keresse meg a hello `OnInitialized(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="91c8c-135">In hello `TodoListClient` project, open `MainWindow.xaml.cs` and locate hello `OnInitialized(...)` method.</span></span>  <span data-ttu-id="91c8c-136">első lépés hello tooinitialize van az alkalmazás `PublicClientApplication` -natív alkalmazások képviselő MSAL tartozó elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="91c8c-136">hello first step is tooinitialize your app's `PublicClientApplication` - MSAL's primary class representing native applications.</span></span>  <span data-ttu-id="91c8c-137">Ez az adott át MSAL hello koordinátát kell toocommunicate az Azure ad-val, és mondja el, hogyan toocache jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="91c8c-137">This is where you pass MSAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* <span data-ttu-id="91c8c-138">Hello alkalmazás indításakor azt szeretné, hogy toocheck, és tekintse meg, ha hello felhasználó már aláírt hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="91c8c-138">When hello app starts up, we want toocheck and see if hello user is already signed into hello app.</span></span>  <span data-ttu-id="91c8c-139">Azonban nem szeretnénk olyan bejelentkezési felhasználói felület tooinvoke még – hello felhasználói, kattintson a "Bejelentkezés" toodo lesz biztosítjuk.</span><span class="sxs-lookup"><span data-stu-id="91c8c-139">However, we don't want tooinvoke a sign-in UI just yet - we'll make hello user click "Sign In" toodo so.</span></span>  <span data-ttu-id="91c8c-140">Emellett a hello `OnInitialized(...)` módszert:</span><span class="sxs-lookup"><span data-stu-id="91c8c-140">Also in hello `OnInitialized(...)` method:</span></span>

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

* <span data-ttu-id="91c8c-141">Hello felhasználó nem jelentkezett be, és hello "Bejelentkezés" gombra kattintva, ha azt szeretné, hogy a bejelentkezési felhasználói felület tooinvoke, és adja meg a hitelesítő adataik hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="91c8c-141">If hello user is not signed in and they click hello "Sign In" button, we want tooinvoke a login UI and have hello user enter their credentials.</span></span>  <span data-ttu-id="91c8c-142">Hello bejelentkezési gomb kezelő megvalósításához:</span><span class="sxs-lookup"><span data-stu-id="91c8c-142">Implement hello Sign-In button handler:</span></span>

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

* <span data-ttu-id="91c8c-143">Ha hello felhasználó sikeresen jelentkezik be, MSAL fogadja és gyorsítótárazza a jogkivonatot, és folytathatja a toocall hello `GetTodoList()` metódus az vetett bizalmat.</span><span class="sxs-lookup"><span data-stu-id="91c8c-143">If hello user successfully signs-in, MSAL will receive and cache a token for you, and you can proceed toocall hello `GetTodoList()` method with confidence.</span></span>  <span data-ttu-id="91c8c-144">A felhasználói feladatok elhagyta tooget csak a tooimplement hello `GetTodoList()` metódust.</span><span class="sxs-lookup"><span data-stu-id="91c8c-144">All that's left tooget a user's tasks is tooimplement hello `GetTodoList()` method.</span></span>

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

## <a name="run"></a><span data-ttu-id="91c8c-145">Futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="91c8c-145">Run</span></span>
<span data-ttu-id="91c8c-146">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="91c8c-146">Congratulations!</span></span> <span data-ttu-id="91c8c-147">Most már rendelkezik egy működő .NET WPF-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók & biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja.</span><span class="sxs-lookup"><span data-stu-id="91c8c-147">You now have a working .NET WPF app that has hello ability tooauthenticate users & securely call Web APIs using OAuth 2.0.</span></span>  <span data-ttu-id="91c8c-148">A mindkét projekt futtatásához, és jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="91c8c-148">Run your both projects, and sign in with either a personal Microsoft account or a work or school account.</span></span>  <span data-ttu-id="91c8c-149">Adja hozzá a feladatok toothat felhasználók feladatlistáit.</span><span class="sxs-lookup"><span data-stu-id="91c8c-149">Add tasks toothat user's To-Do list.</span></span>  <span data-ttu-id="91c8c-150">Jelentkezzen ki, és jelentkezzen be ismét egy másik felhasználó tooview a tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="91c8c-150">Sign out, and sign back in as another user tooview their To-Do list.</span></span>  <span data-ttu-id="91c8c-151">Hello-alkalmazások bezárása, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="91c8c-151">Close hello app, and re-run it.</span></span>  <span data-ttu-id="91c8c-152">Figyelje meg, hogyan hello felhasználói munkamenet változatlanul -, mert hello app gyorsítótárazza a tokenek egy helyi fájlban.</span><span class="sxs-lookup"><span data-stu-id="91c8c-152">Notice how hello user's session remains intact - that is because hello app caches tokens in a local file.</span></span>

<span data-ttu-id="91c8c-153">MSAL segítségével könnyen tooincorporate általános identitás-szolgáltatások az alkalmazásba személyes és munkahelyi fiókokat egyaránt használ.</span><span class="sxs-lookup"><span data-stu-id="91c8c-153">MSAL makes it easy tooincorporate common identity features into your app, using both personal and work accounts.</span></span>  <span data-ttu-id="91c8c-154">Azt gondoskodik az összes hello dirty munkahelyi meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a.</span><span class="sxs-lookup"><span data-stu-id="91c8c-154">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="91c8c-155">Valóban tooknow szüksége egy egyetlen API-hívással `app.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="91c8c-155">All you really need tooknow is a single API call, `app.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="91c8c-156">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="91c8c-156">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="91c8c-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91c8c-157">Next steps</span></span>
<span data-ttu-id="91c8c-158">Ön most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="91c8c-158">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="91c8c-159">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="91c8c-159">You may want tootry:</span></span>

* [<span data-ttu-id="91c8c-160">Hello v2.0-végponttal hello TodoListService webes API biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="91c8c-160">Securing hello TodoListService Web API with hello v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-dotnet-api.md)

<span data-ttu-id="91c8c-161">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="91c8c-161">For additional resources, check out:</span></span>  

* [<span data-ttu-id="91c8c-162">hello v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="91c8c-162">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="91c8c-163">StackOverflow "msal" címke >></span><span class="sxs-lookup"><span data-stu-id="91c8c-163">StackOverflow "msal" tag >></span></span>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="91c8c-164">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="91c8c-164">Get security updates for our products</span></span>
<span data-ttu-id="91c8c-165">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="91c8c-165">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

