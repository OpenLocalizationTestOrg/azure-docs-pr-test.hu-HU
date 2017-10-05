---
title: "Ismerkedés az Azure AD .NET |} Microsoft Docs"
description: "Hogyan hozható létre a .NET Windows asztali alkalmazás, amely integrálható az Azure ad-val jelentkezzen be, és meghívja az Azure AD API-k OAuth protokollt használó védett."
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
ms.openlocfilehash: 7a252e0e5243c7b7489373845531cb913ca1f6aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a><span data-ttu-id="3dded-103">Az Azure AD integrálása Windows asztali WPF-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="3dded-103">Integrate Azure AD into a Windows Desktop WPF App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="3dded-104">Ha az asztali alkalmazások, az Azure AD segítségével egyszerű és magától értetődő, hogy a felhasználókat az Active Directory-fiókok hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3dded-104">If you're developing a desktop application, Azure AD makes it simple and straightforward for you to authenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="3dded-105">Emellett lehetővé teszi az alkalmazás minden webes API-t az Azure AD, például az Office 365 API-k vagy az Azure API által védett biztonságosan felhasználását.</span><span class="sxs-lookup"><span data-stu-id="3dded-105">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="3dded-106">A .NET natív ügyfelek esetében a védett erőforrások eléréséhez szükséges az Azure AD az Active Directory Authentication Library, vagy az adal-t biztosít.</span><span class="sxs-lookup"><span data-stu-id="3dded-106">For .NET native clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="3dded-107">ADAL meg azzal a céllal életben megkönnyíti a hozzáférési jogkivonatok lekérésére, ha az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3dded-107">ADAL's sole purpose in life is to make it easy for your app to get access tokens.</span></span>  <span data-ttu-id="3dded-108">Annak bemutatásához, milyen könnyű van, itt azt fogja összeállíthat egy .NET WPF feladatlista alkalmazást, amely:</span><span class="sxs-lookup"><span data-stu-id="3dded-108">To demonstrate just how easy it is, here we'll build a .NET WPF To-Do List application that:</span></span>

* <span data-ttu-id="3dded-109">Lekérdezi hozzáférési jogkivonatok hívásakor az Azure AD Graph API segítségével a [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="3dded-109">Gets access tokens for calling the Azure AD Graph API using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="3dded-110">Egy könyvtárat a felhasználók egy adott aliasnév keres.</span><span class="sxs-lookup"><span data-stu-id="3dded-110">Searches a directory for users with a given alias.</span></span>
* <span data-ttu-id="3dded-111">Kimenő felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="3dded-111">Signs users out.</span></span>

<span data-ttu-id="3dded-112">A teljes működő alkalmazás létrehozásához szüksége:</span><span class="sxs-lookup"><span data-stu-id="3dded-112">To build the complete working application, you'll need to:</span></span>

1. <span data-ttu-id="3dded-113">Az alkalmazás regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="3dded-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="3dded-114">Telepítése és konfigurálása adal-t.</span><span class="sxs-lookup"><span data-stu-id="3dded-114">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="3dded-115">Adal-t használó tokenek lekérni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3dded-115">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="3dded-116">A kezdéshez [töltse le az alkalmazás vázat](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="3dded-116">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="3dded-117">Biztosítani kell, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3dded-117">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="3dded-118">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="3dded-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-the-directorysearcher-application"></a><span data-ttu-id="3dded-119">1. A DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3dded-119">1. Register the DirectorySearcher Application</span></span>
<span data-ttu-id="3dded-120">Ahhoz, hogy az alkalmazás a jogkivonatok lekérésére, először regisztrálja az Azure AD-bérlőben, és adja meg azt az Azure AD Graph API hozzáférése:</span><span class="sxs-lookup"><span data-stu-id="3dded-120">To enable your app to get tokens, you'll first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="3dded-121">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3dded-121">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3dded-122">A felső eszköztáron kattintson a fiókhoz, majd a a **Directory** menüben válassza ki az Active Directory-bérlőt, ahová be szeretné-e az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="3dded-122">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="3dded-123">Kattintson a **több szolgáltatások** a bal oldali navigációs válassza **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3dded-123">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="3dded-124">Kattintson a **App regisztrációk** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3dded-124">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="3dded-125">Kövesse az utasításokat, és hozzon létre egy új **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3dded-125">Follow the prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="3dded-126">A **neve** az alkalmazás ismerteti az alkalmazást a végfelhasználók számára</span><span class="sxs-lookup"><span data-stu-id="3dded-126">The **Name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="3dded-127">A **átirányítási Uri-** protokollt és a karakterlánc kombinációját, amely az Azure AD vissza a token válaszokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3dded-127">The **Redirect Uri** is a scheme and string combination that Azure AD will use to return token responses.</span></span>  <span data-ttu-id="3dded-128">Adja meg például egy adott értéket az alkalmazás `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="3dded-128">Enter a value specific to your application, e.g. `http://DirectorySearcher`.</span></span>
6. <span data-ttu-id="3dded-129">Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3dded-129">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="3dded-130">Ez az érték kell a következő szakaszokban lévő, másolja az alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="3dded-130">You'll need this value in the next sections, so copy it from the application page.</span></span>
7. <span data-ttu-id="3dded-131">Az a **beállítások** lapon, válassza ki **szükséges engedélyek** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3dded-131">From the **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="3dded-132">Válassza ki a **Microsoft Graph** az API-t, és adja hozzá a **címtáradatok olvasása** engedélyt a **delegált engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="3dded-132">Select the **Microsoft Graph** as the API and add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="3dded-133">Ez lehetővé teszi az alkalmazás lekérdezése a Graph API-t a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="3dded-133">This will enable your application to query the Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="3dded-134">2. Telepítése és konfigurálása adal-t</span><span class="sxs-lookup"><span data-stu-id="3dded-134">2. Install & Configure ADAL</span></span>
<span data-ttu-id="3dded-135">Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="3dded-135">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="3dded-136">Ahhoz, hogy az adal-t tudjanak kommunikálni az Azure AD meg kell biztosítania, bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="3dded-136">In order for ADAL to be able to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="3dded-137">Kezdje az adal-t ad hozzá a DirectorySearcher projekt a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="3dded-137">Begin by adding ADAL to the DirectorySearcher project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="3dded-138">A DirectorySearcher projektben nyissa meg a `app.config`.</span><span class="sxs-lookup"><span data-stu-id="3dded-138">In the DirectorySearcher project, open `app.config`.</span></span>  <span data-ttu-id="3dded-139">Cserélje le az értékeket az elemek a `<appSettings>` szakaszban az Azure portált megadott értékeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="3dded-139">Replace the values of the elements in the `<appSettings>` section to reflect the values you input into the Azure Portal.</span></span>  <span data-ttu-id="3dded-140">A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3dded-140">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="3dded-141">A `ida:Tenant` a tartományt az Azure AD-bérlőn, pl.: contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="3dded-141">The `ida:Tenant` is the domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="3dded-142">A `ida:ClientId` ClientID-azonosítóját az alkalmazás a portálról másolt van.</span><span class="sxs-lookup"><span data-stu-id="3dded-142">The `ida:ClientId` is the clientId of your application you copied from the portal.</span></span>
  * <span data-ttu-id="3dded-143">A `ida:RedirectUri` van az átirányítási URL-cím regisztrálta a portálon.</span><span class="sxs-lookup"><span data-stu-id="3dded-143">The `ida:RedirectUri` is the redirect url you registered in the portal.</span></span>

## <a name="3----use-adal-to-get-tokens-from-aad"></a><span data-ttu-id="3dded-144">3.    Adal-t használó tokenek lekérése az aad-ben</span><span class="sxs-lookup"><span data-stu-id="3dded-144">3.    Use ADAL to Get Tokens from AAD</span></span>
<span data-ttu-id="3dded-145">Az alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja `authContext.AcquireTokenAsync(...)`, és a többi adal-t.</span><span class="sxs-lookup"><span data-stu-id="3dded-145">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireTokenAsync(...)`, and ADAL does the rest.</span></span>  

* <span data-ttu-id="3dded-146">Az a `DirectorySearcher` projektben nyissa meg `MainWindow.xaml.cs` , és keresse meg a `MainWindow()` metódust.</span><span class="sxs-lookup"><span data-stu-id="3dded-146">In the `DirectorySearcher` project, open `MainWindow.xaml.cs` and locate the `MainWindow()` method.</span></span>  <span data-ttu-id="3dded-147">Az első lépés az, hogy az alkalmazás inicializálása `AuthenticationContext` -ADAL tartozó elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="3dded-147">The first step is to initialize your app's `AuthenticationContext` - ADAL's primary class.</span></span>  <span data-ttu-id="3dded-148">Ez az adott át adal-t a koordináták kommunikálni az Azure AD, és mondja el, akkor jogkivonatok gyorsítótárazásának kell.</span><span class="sxs-lookup"><span data-stu-id="3dded-148">This is where you pass ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* <span data-ttu-id="3dded-149">Keresse meg a `Search(...)` metódus, amely akkor kell meghívni, amikor a felhasználó cliks a "Search" gombra az alkalmazás felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="3dded-149">Now locate the `Search(...)` method, which will be invoked when the user cliks the "Search" button in the app's UI.</span></span>  <span data-ttu-id="3dded-150">Ez a módszer az Azure AD Graph API lekérdezéshez GET kérés teszi a felhasználók számára, amelyek egyszerű felhasználónév a megadott keresési kifejezés kezdődik.</span><span class="sxs-lookup"><span data-stu-id="3dded-150">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="3dded-151">Ahhoz, hogy a Graph API lekérdezni, meg kell adnia egy access_token a, de a `Authorization` fejlécet a kérelem - ezt az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="3dded-151">But in order to query the Graph API, you need to include an access_token in the `Authorization` header of the request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* <span data-ttu-id="3dded-152">Ha az alkalmazás kísérel meg jogkivonat meghívásával `AcquireTokenAsync(...)`, ADAL megkísérli jogkivonat vissza a felhasználói hitelesítő adatok kérése nélkül.</span><span class="sxs-lookup"><span data-stu-id="3dded-152">When your app requests a token by calling `AcquireTokenAsync(...)`, ADAL will attempt to return a token without asking the user for credentials.</span></span>  <span data-ttu-id="3dded-153">ADAL határozza meg, hogy a felhasználó bejelentkezhet a szolgáltatáshitelesítést egy token kell-e, ha azt fogja bejelentkezési párbeszédablakot jelenít meg, a felhasználói hitelesítő adatok összegyűjtése és térjen vissza a sikeres hitelesítés esetén egy token.</span><span class="sxs-lookup"><span data-stu-id="3dded-153">If ADAL determines that the user needs to sign in to get a token, it will display a login dialog, collect the user's credentials, and return a token upon successful authentication.</span></span>  <span data-ttu-id="3dded-154">Ha nem lehet visszaadni a jogkivonatot a bármilyen okból adal-t, akkor kivételhibát egy `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="3dded-154">If ADAL is unable to return a token for any reason, it will throw an `AdalException`.</span></span>
* <span data-ttu-id="3dded-155">Figyelje meg, hogy a `AuthenticationResult` objektum tartalmaz egy `UserInfo` objektum, amely segítségével az alkalmazás esetleg adatokat gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="3dded-155">Notice that the `AuthenticationResult` object contains a `UserInfo` object that can be used to collect information your app may need.</span></span>  <span data-ttu-id="3dded-156">Az a DirectorySearcher `UserInfo` segítségével testre szabhatja az alkalmazás Kezelőfelületén a felhasználói azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="3dded-156">In the DirectorySearcher, `UserInfo` is used to customize the app's UI with the user's id.</span></span>
* <span data-ttu-id="3dded-157">Amikor a felhasználó a "Sign Out" gombra kattint, annak érdekében, hogy a következő hívást szeretnénk `AcquireTokenAsync(...)` ekkor megkérdezi a felhasználót, hogy jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="3dded-157">When the user clicks the "Sign Out" button, we want to ensure that the next call to `AcquireTokenAsync(...)` will ask the user to sign in.</span></span>  <span data-ttu-id="3dded-158">Ez az adal-t, az egyszerű módon, ha a token gyorsítótár kiürítése:</span><span class="sxs-lookup"><span data-stu-id="3dded-158">With ADAL, this is as easy as clearing the token cache:</span></span>

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* <span data-ttu-id="3dded-159">Azonban ha a felhasználó nem a "Sign Out" gombra kattintva érdemes a felhasználói munkamenetet a következő alkalommal futtatja a DirectorySearcher a karbantartása.</span><span class="sxs-lookup"><span data-stu-id="3dded-159">However, if the user does not click the "Sign Out" button, you will want to maintain the user's session for the next time they run the DirectorySearcher.</span></span>  <span data-ttu-id="3dded-160">Az alkalmazás indításakor ellenőrizze az adal-t a jogkivonatok gyorsítótárát, vagy egy meglévő jogkivonatot, és ennek megfelelően frissíti a felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="3dded-160">When the app launches, you can check ADAL's token cache for an existing token and update the UI accordingly.</span></span>  <span data-ttu-id="3dded-161">Az a `CheckForCachedToken()` módszer, egy másik hívás `AcquireTokenAsync(...)`, ezúttal benyújtása a `PromptBehavior.Never` paraméter.</span><span class="sxs-lookup"><span data-stu-id="3dded-161">In the `CheckForCachedToken()` method, make another call to `AcquireTokenAsync(...)`, this time passing in the `PromptBehavior.Never` parameter.</span></span>  <span data-ttu-id="3dded-162">`PromptBehavior.Never`adal-t jelzi, hogy a felhasználói bejelentkezés nem kéri, és ADAL helyette kell kivételt jelez, ha nem adja vissza egy token.</span><span class="sxs-lookup"><span data-stu-id="3dded-162">`PromptBehavior.Never` will tell ADAL that the user should not be prompted for sign in, and ADAL should instead throw an exception if it is unable to return a token.</span></span>

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
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

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

<span data-ttu-id="3dded-163">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="3dded-163">Congratulations!</span></span> <span data-ttu-id="3dded-164">Most már rendelkezik működő .NET WPF-alkalmazás, amely képes a felhasználók hitelesítésére, biztonságosan hívja az OAuth 2.0 használatával webes API-k, és a felhasználóval kapcsolatos alapvető információk.</span><span class="sxs-lookup"><span data-stu-id="3dded-164">You now have a working .NET WPF application that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="3dded-165">Ha még nem tette meg, most már az egyes felhasználóival a bérlő feltölti idő.</span><span class="sxs-lookup"><span data-stu-id="3dded-165">If you haven't already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="3dded-166">Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="3dded-166">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="3dded-167">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="3dded-167">Search for other users based on their UPN.</span></span>  <span data-ttu-id="3dded-168">Zárja be az alkalmazást, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="3dded-168">Close the app, and re-run it.</span></span>  <span data-ttu-id="3dded-169">Figyelje meg, hogyan az ügyfél helye változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="3dded-169">Notice how the user's session remains intact.</span></span>  <span data-ttu-id="3dded-170">Jelentkezzen ki, és jelentkezzen be egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="3dded-170">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="3dded-171">Az adal TÁRAT megkönnyíti, hogy átfogó mindezeket a közös identitás funkciókat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="3dded-171">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="3dded-172">Ez gondoskodik a dirty munkát meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, a felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a bemutató.</span><span class="sxs-lookup"><span data-stu-id="3dded-172">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="3dded-173">Biztosan tudni, hogy szüksége egy egyetlen API-hívással `authContext.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="3dded-173">All you really need to know is a single API call, `authContext.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="3dded-174">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként biztosított [Itt](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="3dded-174">For reference, the completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="3dded-175">Most már továbbléphet további helyzeteket is.</span><span class="sxs-lookup"><span data-stu-id="3dded-175">You can now move on to additional scenarios.</span></span>  <span data-ttu-id="3dded-176">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="3dded-176">You may want to try:</span></span>

[<span data-ttu-id="3dded-177">.NET webes API-k, az Azure ad-vel biztonságos >></span><span class="sxs-lookup"><span data-stu-id="3dded-177">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

