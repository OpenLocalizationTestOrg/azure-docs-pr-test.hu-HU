---
title: "első lépések aaaAzure AD .NET |} Microsoft Docs"
description: "Hogyan toobuild egy .NET Windows asztali alkalmazás, amely integrálható a bejelentkezéshez az Azure AD és az Azure AD védett OAuth protokollt használó API-k."
services: active-directory
documentationcenter: .net
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c09b358f24c7bfb371b34cf72ca48c0a45042f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a><span data-ttu-id="7ed5c-103">Az Azure AD integrálása Windows asztali WPF-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="7ed5c-103">Integrate Azure AD into a Windows Desktop WPF App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="7ed5c-104">Ha az asztali alkalmazások, az Azure AD teszi egyszerű és magától értetődő, az Ön tooauthenticate a felhasználókat az Active Directory-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-104">If you're developing a desktop application, Azure AD makes it simple and straightforward for you tooauthenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="7ed5c-105">Emellett lehetővé teszi az alkalmazás toosecurely használatához minden webes API-t az Azure AD által védett, például Office 365 API-k hello vagy hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-105">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="7ed5c-106">A .NET natív ügyfelek számára, amelyek tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library vagy adal-t biztosít.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-106">For .NET native clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="7ed5c-107">ADAL meg azzal a céllal életben toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-107">ADAL's sole purpose in life is toomake it easy for your app tooget access tokens.</span></span>  <span data-ttu-id="7ed5c-108">toodemonstrate milyen könnyű van, itt azt kell egy .NET WPF feladatlista alkalmazás létrehozása, amely:</span><span class="sxs-lookup"><span data-stu-id="7ed5c-108">toodemonstrate just how easy it is, here we'll build a .NET WPF To-Do List application that:</span></span>

* <span data-ttu-id="7ed5c-109">Lekérdezi hozzáférési jogkivonatainak hello segítségével a hello Azure AD Graph API felület meghívásakor [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ed5c-109">Gets access tokens for calling hello Azure AD Graph API using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="7ed5c-110">Egy könyvtárat a felhasználók egy adott aliasnév keres.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-110">Searches a directory for users with a given alias.</span></span>
* <span data-ttu-id="7ed5c-111">Kimenő felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-111">Signs users out.</span></span>

<span data-ttu-id="7ed5c-112">toobuild hello teljes működő alkalmazást kell:</span><span class="sxs-lookup"><span data-stu-id="7ed5c-112">toobuild hello complete working application, you'll need to:</span></span>

1. <span data-ttu-id="7ed5c-113">Az alkalmazás regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="7ed5c-114">Telepítése és konfigurálása adal-t.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-114">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="7ed5c-115">Használja az Azure AD ADAL tooget jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-115">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="7ed5c-116">elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="7ed5c-116">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="7ed5c-117">Biztosítani kell, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-117">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="7ed5c-118">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="7ed5c-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-hello-directorysearcher-application"></a><span data-ttu-id="7ed5c-119">1. Hello DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="7ed5c-119">1. Register hello DirectorySearcher Application</span></span>
<span data-ttu-id="7ed5c-120">tooenable a tooget alkalmazási jogkivonatok, először tooregister azt az Azure ad bérlői, és biztosítson számára engedély tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="7ed5c-120">tooenable your app tooget tokens, you'll first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="7ed5c-121">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7ed5c-121">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7ed5c-122">Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-122">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="7ed5c-123">Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-123">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="7ed5c-124">Kattintson a **App regisztrációk** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-124">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="7ed5c-125">Hello utasításokat követve, és hozzon létre egy új **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-125">Follow hello prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="7ed5c-126">Hello **neve** hello az alkalmazás ismerteti, az alkalmazás tooend-felhasználók</span><span class="sxs-lookup"><span data-stu-id="7ed5c-126">hello **Name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="7ed5c-127">Hello **átirányítási Uri-** protokollt és a karakterlánc kombinációját, hogy az Azure AD tooreturn token válaszokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-127">hello **Redirect Uri** is a scheme and string combination that Azure AD will use tooreturn token responses.</span></span>  <span data-ttu-id="7ed5c-128">Adjon meg egy értéket adott tooyour alkalmazást, pl. `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-128">Enter a value specific tooyour application, e.g. `http://DirectorySearcher`.</span></span>
6. <span data-ttu-id="7ed5c-129">Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-129">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="7ed5c-130">Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-130">You'll need this value in hello next sections, so copy it from hello application page.</span></span>
7. <span data-ttu-id="7ed5c-131">A hello **beállítások** lapon, válassza ki **szükséges engedélyek** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-131">From hello **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="7ed5c-132">Jelölje be hello **Microsoft Graph** , hello API, és adja hozzá a hello **címtáradatok olvasása** engedélyt a **delegált engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-132">Select hello **Microsoft Graph** as hello API and add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="7ed5c-133">Ez lehetővé teszi az alkalmazás tooquery hello Graph API a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-133">This will enable your application tooquery hello Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="7ed5c-134">2. Telepítése és konfigurálása adal-t</span><span class="sxs-lookup"><span data-stu-id="7ed5c-134">2. Install & Configure ADAL</span></span>
<span data-ttu-id="7ed5c-135">Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-135">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="7ed5c-136">Ahhoz, hogy képes toocommunicate ADAL toobe az Azure AD-val, tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-136">In order for ADAL toobe able toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="7ed5c-137">Első lépésként hozzáadása ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-137">Begin by adding ADAL toohello DirectorySearcher project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="7ed5c-138">Hello DirectorySearcher projektben nyissa meg `app.config`.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-138">In hello DirectorySearcher project, open `app.config`.</span></span>  <span data-ttu-id="7ed5c-139">Cserélje le a hello értékek hello hello elemeinek `<appSettings>` szakasz tooreflect hello értékei a bemenő hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-139">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values you input into hello Azure Portal.</span></span>  <span data-ttu-id="7ed5c-140">A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-140">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="7ed5c-141">Hello `ida:Tenant` hello tartománya, akkor az Azure AD-bérlőn, pl.: contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="7ed5c-141">hello `ida:Tenant` is hello domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="7ed5c-142">Hello `ida:ClientId` hello clientId hello portálról másolta az alkalmazás van.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-142">hello `ida:ClientId` is hello clientId of your application you copied from hello portal.</span></span>
  * <span data-ttu-id="7ed5c-143">Hello `ida:RedirectUri` hello van regisztrált hello portál URL-címének átirányítása.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-143">hello `ida:RedirectUri` is hello redirect url you registered in hello portal.</span></span>

## <a name="3----use-adal-tooget-tokens-from-aad"></a><span data-ttu-id="7ed5c-144">3.    Használja az ADAL tooGet jogkivonatok az aad-ben</span><span class="sxs-lookup"><span data-stu-id="7ed5c-144">3.    Use ADAL tooGet Tokens from AAD</span></span>
<span data-ttu-id="7ed5c-145">hello alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja `authContext.AcquireTokenAsync(...)`, és ADAL rest hello.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-145">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireTokenAsync(...)`, and ADAL does hello rest.</span></span>  

* <span data-ttu-id="7ed5c-146">A hello `DirectorySearcher` projektben nyissa meg `MainWindow.xaml.cs` , és keresse meg a hello `MainWindow()` metódust.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-146">In hello `DirectorySearcher` project, open `MainWindow.xaml.cs` and locate hello `MainWindow()` method.</span></span>  <span data-ttu-id="7ed5c-147">első lépés hello tooinitialize van az alkalmazás `AuthenticationContext` -ADAL tartozó elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-147">hello first step is tooinitialize your app's `AuthenticationContext` - ADAL's primary class.</span></span>  <span data-ttu-id="7ed5c-148">Ez az adott át ADAL hello koordinátát kell toocommunicate az Azure ad-val, és mondja el, hogyan toocache jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-148">This is where you pass ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* <span data-ttu-id="7ed5c-149">Keresse meg hello `Search(...)` metódus, amely fog kell meghívni, ha hello felhasználói cliks hello hello alkalmazás felhasználói felületén a "Search" gombra.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-149">Now locate hello `Search(...)` method, which will be invoked when hello user cliks hello "Search" button in hello app's UI.</span></span>  <span data-ttu-id="7ed5c-150">Ez a módszer egy GET kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-150">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="7ed5c-151">Rendelés tooquery hello Graph API-val, a tooinclude egy access_token a hello szükség van, de `Authorization` hello fejlécének kérelem - Ez az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-151">But in order tooquery hello Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate hello Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for hello tooDo item name");
        return;
    }

    // Get an Access Token for hello Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled hello sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* <span data-ttu-id="7ed5c-152">Ha az alkalmazás kísérel meg jogkivonat meghívásával `AcquireTokenAsync(...)`, ADAL megpróbál tooreturn jogkivonat hello felhasználói hitelesítő adatok kérése nélkül.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-152">When your app requests a token by calling `AcquireTokenAsync(...)`, ADAL will attempt tooreturn a token without asking hello user for credentials.</span></span>  <span data-ttu-id="7ed5c-153">Ha ADAL hello felhasználó van szüksége a tooget jogkivonat toosign, azt fogja megjelenítési bejelentkezési hello felhasználói hitelesítő adatok összegyűjtése, térjen vissza a sikeres hitelesítés esetén egy token.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-153">If ADAL determines that hello user needs toosign in tooget a token, it will display a login dialog, collect hello user's credentials, and return a token upon successful authentication.</span></span>  <span data-ttu-id="7ed5c-154">Ha az adal TÁRAT nem tooreturn jogkivonat bármilyen okból, akkor kivételhibát egy `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-154">If ADAL is unable tooreturn a token for any reason, it will throw an `AdalException`.</span></span>
* <span data-ttu-id="7ed5c-155">Figyelje meg, hogy hello `AuthenticationResult` objektum tartalmaz egy `UserInfo` objektum, amely lehet például az alkalmazás esetleg használt toocollect információ.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-155">Notice that hello `AuthenticationResult` object contains a `UserInfo` object that can be used toocollect information your app may need.</span></span>  <span data-ttu-id="7ed5c-156">A hello DirectorySearcher `UserInfo` használt toocustomize hello alkalmazás Kezelőfelületén hello felhasználói azonosítóval van.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-156">In hello DirectorySearcher, `UserInfo` is used toocustomize hello app's UI with hello user's id.</span></span>
* <span data-ttu-id="7ed5c-157">Amikor hello felhasználó hello "Kijelentkezés" gombra kattint, szeretnénk, hogy a következő hívás túl hello tooensure`AcquireTokenAsync(...)` hello felhasználói toosign a felhasználói jóváhagyást kér.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-157">When hello user clicks hello "Sign Out" button, we want tooensure that hello next call too`AcquireTokenAsync(...)` will ask hello user toosign in.</span></span>  <span data-ttu-id="7ed5c-158">Ez az adal-t, az egyszerű módon, ha hello token gyorsítótár kiürítése:</span><span class="sxs-lookup"><span data-stu-id="7ed5c-158">With ADAL, this is as easy as clearing hello token cache:</span></span>

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear hello token cache
    authContext.TokenCache.Clear();

    ...
}
```

* <span data-ttu-id="7ed5c-159">Azonban ha hello felhasználói nem hello "Kijelentkezés" gombra kattintva célszerű toomaintain hello felhasználói munkamenetet a hello hello DirectorySearcher következő futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-159">However, if hello user does not click hello "Sign Out" button, you will want toomaintain hello user's session for hello next time they run hello DirectorySearcher.</span></span>  <span data-ttu-id="7ed5c-160">Hello alkalmazás indításakor ADAL által jogkivonatok gyorsítótárát egy meglévő jogkivonat esetében ellenőrizze, és ennek megfelelően frissülnek hello felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-160">When hello app launches, you can check ADAL's token cache for an existing token and update hello UI accordingly.</span></span>  <span data-ttu-id="7ed5c-161">A hello `CheckForCachedToken()` módszer, egy másik hívás túl`AcquireTokenAsync(...)`, ezúttal hello benyújtása `PromptBehavior.Never` paraméter.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-161">In hello `CheckForCachedToken()` method, make another call too`AcquireTokenAsync(...)`, this time passing in hello `PromptBehavior.Never` parameter.</span></span>  <span data-ttu-id="7ed5c-162">`PromptBehavior.Never`jelzi az adal-t hello felhasználói bejelentkezés nem kéri, és ADAL helyette kivételt kell előidéznie nem tooreturn esetén egy token.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-162">`PromptBehavior.Never` will tell ADAL that hello user should not be prompted for sign in, and ADAL should instead throw an exception if it is unable tooreturn a token.</span></span>

```C#
public async void CheckForCachedToken() 
{
    // As hello application starts, try tooget an access token without prompting hello user.  If one exists, show hello user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed toomain page without singing hello user in.
        return;
    }

    // A valid token is in hello cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

<span data-ttu-id="7ed5c-163">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="7ed5c-163">Congratulations!</span></span> <span data-ttu-id="7ed5c-164">Ön most .NET WPF-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és működik alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-164">You now have a working .NET WPF application that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="7ed5c-165">Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-165">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="7ed5c-166">Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-166">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="7ed5c-167">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-167">Search for other users based on their UPN.</span></span>  <span data-ttu-id="7ed5c-168">Hello-alkalmazások bezárása, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-168">Close hello app, and re-run it.</span></span>  <span data-ttu-id="7ed5c-169">Figyelje meg, hogyan hello felhasználói munkamenet nem módosulnak.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-169">Notice how hello user's session remains intact.</span></span>  <span data-ttu-id="7ed5c-170">Jelentkezzen ki, és jelentkezzen be egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-170">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="7ed5c-171">ADAL teszi, hogy könnyen tooincorporate mindezeket a funkciókat az alkalmazásba közös identitás.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-171">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="7ed5c-172">Azt gondoskodik az összes hello dirty munkahelyi meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-172">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="7ed5c-173">Valóban tooknow szüksége egy egyetlen API-hívással `authContext.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-173">All you really need tooknow is a single API call, `authContext.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="7ed5c-174">Megadott befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként [Itt](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="7ed5c-174">For reference, hello completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="7ed5c-175">Most már továbbléphet tooadditional forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="7ed5c-175">You can now move on tooadditional scenarios.</span></span>  <span data-ttu-id="7ed5c-176">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="7ed5c-176">You may want tootry:</span></span>

[<span data-ttu-id="7ed5c-177">.NET webes API-k, az Azure ad-vel biztonságos >></span><span class="sxs-lookup"><span data-stu-id="7ed5c-177">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

