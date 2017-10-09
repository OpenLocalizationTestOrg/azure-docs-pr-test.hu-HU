---
title: "Első lépések AD Windows Phone aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild egy Windows Phone-alkalmazás, amely integrálható a bejelentkezéshez az Azure AD és az Azure AD védett OAuth protokollt használó API-k."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e766bfcdfae10483772154f4b5facdec05fc846f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a><span data-ttu-id="5aafb-103">Azure AD integrálása a Windows Phone-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5aafb-103">Integrate Azure AD with a Windows Phone App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="5aafb-104">A Windows Phone 8.1-es és korábbi verziójú projektek nem támogatottak a Visual Studio 2017-ben.</span><span class="sxs-lookup"><span data-stu-id="5aafb-104">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="5aafb-105">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="5aafb-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="5aafb-106">Ha a Windows Phone 8.1-alkalmazást, az Azure AD teszi egyszerű és magától értetődő, az Ön tooauthenticate a felhasználókat az Active Directory-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="5aafb-106">If you're developing a Windows Phone 8.1 app, Azure AD makes it simple and straightforward for you tooauthenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="5aafb-107">Emellett lehetővé teszi az alkalmazás toosecurely használatához minden webes API-t az Azure AD által védett, például Office 365 API-k hello vagy hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="5aafb-107">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

> [!NOTE]
> <span data-ttu-id="5aafb-108">A fenti ADAL v2.0 használja.</span><span class="sxs-lookup"><span data-stu-id="5aafb-108">This code sample uses ADAL v2.0.</span></span>  <span data-ttu-id="5aafb-109">Hello legújabb technológiát, célszerű lehet tooinstead próbálkozzon a [Windows Universal oktatóanyagot ADAL v3.0 használatával](active-directory-devquickstarts-windowsstore.md).</span><span class="sxs-lookup"><span data-stu-id="5aafb-109">For hello latest technology, you may want tooinstead try our [Windows Universal Tutorial using ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span></span>  <span data-ttu-id="5aafb-110">Ha valóban egy alkalmazást a Windows Phone 8.1-es, ez a hello megfelelő hely.</span><span class="sxs-lookup"><span data-stu-id="5aafb-110">If you are indeed building an app for Windows Phone 8.1, this is hello right place.</span></span>  <span data-ttu-id="5aafb-111">ADAL v2.0 továbbra is teljes mértékben támogatja, és hello fejlesztését alkalmazások agianst Windows Phone 8.1-es ajánlott módja használja az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5aafb-111">ADAL v2.0 is still fully supported, and is hello recommended way of developing apps agianst Windows Phone 8.1 using Azure AD.</span></span>
> 
> 

<span data-ttu-id="5aafb-112">A .NET natív ügyfelek számára, amelyek tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library vagy adal-t biztosít.</span><span class="sxs-lookup"><span data-stu-id="5aafb-112">For .NET native clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="5aafb-113">ADAL meg azzal a céllal életben toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="5aafb-113">ADAL’s sole purpose in life is toomake it easy for your app tooget access tokens.</span></span>  <span data-ttu-id="5aafb-114">toodemonstrate milyen könnyű van, itt azt fogja létrehozása egy "Directory keresővel" Windows Phone 8.1-alkalmazást, amely:</span><span class="sxs-lookup"><span data-stu-id="5aafb-114">toodemonstrate just how easy it is, here we’ll build a "Directory Searcher" Windows Phone 8.1 app that:</span></span>

* <span data-ttu-id="5aafb-115">Lekérdezi hozzáférési jogkivonatainak hello segítségével a hello Azure AD Graph API felület meghívásakor [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="5aafb-115">Gets access tokens for calling hello Azure AD Graph API using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="5aafb-116">Megkeresi a megadott egyszerű felhasználónév a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="5aafb-116">Searches a directory for users with a given UPN.</span></span>
* <span data-ttu-id="5aafb-117">Kimenő felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="5aafb-117">Signs users out.</span></span>

<span data-ttu-id="5aafb-118">toobuild hello teljes működő alkalmazást kell:</span><span class="sxs-lookup"><span data-stu-id="5aafb-118">toobuild hello complete working application, you’ll need to:</span></span>

1. <span data-ttu-id="5aafb-119">Az alkalmazás regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="5aafb-119">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="5aafb-120">Telepítése és konfigurálása adal-t.</span><span class="sxs-lookup"><span data-stu-id="5aafb-120">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="5aafb-121">Használja az Azure AD ADAL tooget jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="5aafb-121">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="5aafb-122">elindult, tooget [töltse le a projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="5aafb-122">tooget started, [download a skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="5aafb-123">Egy Visual Studio 2013 megoldást.</span><span class="sxs-lookup"><span data-stu-id="5aafb-123">Each is a Visual Studio 2013 solution.</span></span>  <span data-ttu-id="5aafb-124">Biztosítani kell, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5aafb-124">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="5aafb-125">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="5aafb-125">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-hello-directory-searcher-application"></a><span data-ttu-id="5aafb-126">1. Hello Directory keresővel alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="5aafb-126">1. Register hello Directory Searcher Application</span></span>
<span data-ttu-id="5aafb-127">tooenable a tooget alkalmazási jogkivonatok, először tooregister azt az Azure ad bérlői, és biztosítson számára engedély tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="5aafb-127">tooenable your app tooget tokens, you’ll first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="5aafb-128">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5aafb-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="5aafb-129">Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5aafb-129">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="5aafb-130">Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5aafb-130">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="5aafb-131">Kattintson a **App regisztrációk** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5aafb-131">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="5aafb-132">Hello utasításokat követve, és hozzon létre egy új **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5aafb-132">Follow hello prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="5aafb-133">Hello **neve** hello az alkalmazás ismerteti, az alkalmazás tooend-felhasználók</span><span class="sxs-lookup"><span data-stu-id="5aafb-133">hello **Name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="5aafb-134">Hello **átirányítási Uri-** protokollt és a karakterlánc kombinációját, hogy az Azure AD tooreturn token válaszokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="5aafb-134">hello **Redirect Uri** is a scheme and string combination that Azure AD will use tooreturn token responses.</span></span>  <span data-ttu-id="5aafb-135">Például a lépést, adjon meg egy helyőrzőt értéket `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="5aafb-135">Enter a placeholder value for now, e.g. `http://DirectorySearcher`.</span></span>  <span data-ttu-id="5aafb-136">Később lesz lecserélni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="5aafb-136">We'll replace this value later.</span></span>
6. <span data-ttu-id="5aafb-137">Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="5aafb-137">Once you’ve completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="5aafb-138">Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="5aafb-138">You’ll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="5aafb-139">A hello **beállítások** lapon, válassza ki **szükséges engedélyek** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5aafb-139">From hello **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="5aafb-140">Jelölje be hello **Microsoft Graph** , hello API, és adja hozzá a hello **címtáradatok olvasása** engedélyt a **delegált engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="5aafb-140">Select hello **Microsoft Graph** as hello API and add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="5aafb-141">Ez lehetővé teszi az alkalmazás tooquery hello Graph API a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="5aafb-141">This will enable your application tooquery hello Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="5aafb-142">2. Telepítése és konfigurálása adal-t</span><span class="sxs-lookup"><span data-stu-id="5aafb-142">2. Install & Configure ADAL</span></span>
<span data-ttu-id="5aafb-143">Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="5aafb-143">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="5aafb-144">Ahhoz, hogy képes toocommunicate ADAL toobe az Azure AD-val, tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="5aafb-144">In order for ADAL toobe able toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="5aafb-145">Első lépésként hozzáadása ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="5aafb-145">Begin by adding ADAL toohello DirectorySearcher project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="5aafb-146">Hello DirectorySearcher projektben nyissa meg `MainPage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="5aafb-146">In hello DirectorySearcher project, open `MainPage.xaml.cs`.</span></span>  <span data-ttu-id="5aafb-147">Cserélje le a hello hello értékek `Config Values` régió tooreflect hello értékei a bemenő hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="5aafb-147">Replace hello values in hello `Config Values` region tooreflect hello values you input into hello Azure Portal.</span></span>  <span data-ttu-id="5aafb-148">A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5aafb-148">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="5aafb-149">Hello `tenant` hello tartománya, akkor az Azure AD-bérlőn, pl.: contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="5aafb-149">hello `tenant` is hello domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="5aafb-150">Hello `clientId` hello clientId hello portálról másolta az alkalmazás van.</span><span class="sxs-lookup"><span data-stu-id="5aafb-150">hello `clientId` is hello clientId of your application you copied from hello portal.</span></span>
* <span data-ttu-id="5aafb-151">A Windows Phone-alkalmazás most kell toodiscover hello visszahívási uri.</span><span class="sxs-lookup"><span data-stu-id="5aafb-151">You now need toodiscover hello callback uri for your Windows Phone app.</span></span>  <span data-ttu-id="5aafb-152">Állítson be egy töréspontot a sort a hello `MainPage` módszert:</span><span class="sxs-lookup"><span data-stu-id="5aafb-152">Set a breakpoint on this line in hello `MainPage` method:</span></span>

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* <span data-ttu-id="5aafb-153">Hello alkalmazás futtatását, és másolja fenntartott hello értékének `redirectUri` amikor hello töréspont elérte-e.</span><span class="sxs-lookup"><span data-stu-id="5aafb-153">Run hello app, and copy aside hello value of `redirectUri` when hello breakpoint is hit.</span></span>  <span data-ttu-id="5aafb-154">Az alábbiakhoz hasonlóan</span><span class="sxs-lookup"><span data-stu-id="5aafb-154">It should look something like</span></span>

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* <span data-ttu-id="5aafb-155">Vissza a hello **konfigurálása** hello Azure felügyeleti portálon, az alkalmazás lapján cserélje le a hello hello értékének **RedirectUri** ezzel az értékkel.</span><span class="sxs-lookup"><span data-stu-id="5aafb-155">Back on hello **Configure** tab of your application in hello Azure Management Portal, replace hello value of hello **RedirectUri** with this value.</span></span>  

## <a name="3-use-adal-tooget-tokens-from-aad"></a><span data-ttu-id="5aafb-156">3. Használja az ADAL tooGet jogkivonatok az aad-ben</span><span class="sxs-lookup"><span data-stu-id="5aafb-156">3. Use ADAL tooGet Tokens from AAD</span></span>
<span data-ttu-id="5aafb-157">hello alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja `authContext.AcquireToken(…)`, és ADAL rest hello.</span><span class="sxs-lookup"><span data-stu-id="5aafb-157">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does hello rest.</span></span>  

* <span data-ttu-id="5aafb-158">első lépés hello tooinitialize van az alkalmazás `AuthenticationContext` -ADAL tartozó elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="5aafb-158">hello first step is tooinitialize your app’s `AuthenticationContext` - ADAL’s primary class.</span></span>  <span data-ttu-id="5aafb-159">Ez az adott át ADAL hello koordinátát kell toocommunicate az Azure ad-val, és mondja el, hogyan toocache jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="5aafb-159">This is where you pass ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* <span data-ttu-id="5aafb-160">Keresse meg hello `Search(...)` metódus, amely fog kell meghívni, ha hello felhasználói cliks hello hello alkalmazás felhasználói felületén a "Search" gombra.</span><span class="sxs-lookup"><span data-stu-id="5aafb-160">Now locate hello `Search(...)` method, which will be invoked when hello user cliks hello "Search" button in hello app's UI.</span></span>  <span data-ttu-id="5aafb-161">Ez a módszer egy GET kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="5aafb-161">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="5aafb-162">Rendelés tooquery hello Graph API-val, a tooinclude egy access_token a hello szükség van, de `Authorization` hello fejlécének kérelem - Ez az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="5aafb-162">But in order tooquery hello Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try tooget a token without triggering any user prompt.
    // ADAL will check whether hello requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained hello QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* <span data-ttu-id="5aafb-163">Interaktív hitelesítésre szükség, ha adal-t fogja használni a Windows Phone webes hitelesítés Broker (WAB) és [folytatási modell](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD Bejelentkezés lapon.</span><span class="sxs-lookup"><span data-stu-id="5aafb-163">If interactive authentication is necessary, ADAL will use Windows Phone's Web Authentication Broker (WAB) and [continuation model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD sign in page.</span></span>  <span data-ttu-id="5aafb-164">Hello felhasználó jelentkezik be, amikor a toopass ADAL hello eredmények hello WAB egymásra kell.</span><span class="sxs-lookup"><span data-stu-id="5aafb-164">When hello user signs in, your app needs toopass ADAL hello results of hello WAB interaction.</span></span>  <span data-ttu-id="5aafb-165">Ez az lehető legegyszerűbb hello végrehajtási `ContinueWebAuthentication` felület:</span><span class="sxs-lookup"><span data-stu-id="5aafb-165">This is as simple as implementing hello `ContinueWebAuthentication` interface:</span></span>

```C#
// This method is automatically invoked when hello application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass hello authentication interaction results tooADAL, which will
    // conclude hello token acquisition operation and invoke hello callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* <span data-ttu-id="5aafb-166">Most azt is, hogy idő toouse hello `AuthenticationResult` adott vissza tooyour alkalmazás adal-t.</span><span class="sxs-lookup"><span data-stu-id="5aafb-166">Now it's time toouse hello `AuthenticationResult` that ADAL returned tooyour app.</span></span>  <span data-ttu-id="5aafb-167">A hello `QueryGraph(...)` visszahívási, csatolja hello access_token toohello GET kérést hello Authorization fejlécet szerezte be:</span><span class="sxs-lookup"><span data-stu-id="5aafb-167">In hello `QueryGraph(...)` callback, attach hello access_token you acquired toohello GET request in hello Authorization header:</span></span>

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add hello access token toohello Authorization Header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* <span data-ttu-id="5aafb-168">Is használhatja a hello `AuthenticationResult` toodisplay információt az alkalmazásban hello felhasználói objektum.</span><span class="sxs-lookup"><span data-stu-id="5aafb-168">You can also use hello `AuthenticationResult` object toodisplay information about hello user in your app.</span></span> <span data-ttu-id="5aafb-169">A hello `QueryGraph(...)` metódust, használjon hello eredmény tooshow hello felhasználó azonosítója hello oldalon:</span><span class="sxs-lookup"><span data-stu-id="5aafb-169">In hello `QueryGraph(...)` method, use hello result tooshow hello user's id on hello page:</span></span>

```C#
// Update hello Page UI toorepresent hello signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* <span data-ttu-id="5aafb-170">Végezetül kívül, valamint az alkalmazás ADAL toosign hello felhasználói is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5aafb-170">Finally, you can use ADAL toosign hello user out of hte application as well.</span></span>  <span data-ttu-id="5aafb-171">Amikor hello felhasználó hello "Kijelentkezés" gombra kattint, szeretnénk, hogy a következő hívás túl hello tooensure`AcquireTokenSilentAsync(...)` sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5aafb-171">When hello user clicks hello "Sign Out" button, we want tooensure that hello next call too`AcquireTokenSilentAsync(...)` will fail.</span></span>  <span data-ttu-id="5aafb-172">Ez az adal-t, az egyszerű módon, ha hello token gyorsítótár kiürítése:</span><span class="sxs-lookup"><span data-stu-id="5aafb-172">With ADAL, this is as easy as clearing hello token cache:</span></span>

```C#
private void SignOut()
{
    // Clear session state from hello token cache.
    authContext.TokenCache.Clear();

    ...
}
```

<span data-ttu-id="5aafb-173">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="5aafb-173">Congratulations!</span></span> <span data-ttu-id="5aafb-174">Ön most Windows Phone-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és működik alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5aafb-174">You now have a working Windows Phone app that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="5aafb-175">Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.</span><span class="sxs-lookup"><span data-stu-id="5aafb-175">If you haven’t already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="5aafb-176">Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="5aafb-176">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="5aafb-177">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="5aafb-177">Search for other users based on their UPN.</span></span>  <span data-ttu-id="5aafb-178">Hello-alkalmazások bezárása, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="5aafb-178">Close hello app, and re-run it.</span></span>  <span data-ttu-id="5aafb-179">Figyelje meg, hogyan hello felhasználói munkamenet nem módosulnak.</span><span class="sxs-lookup"><span data-stu-id="5aafb-179">Notice how hello user’s session remains intact.</span></span>  <span data-ttu-id="5aafb-180">Jelentkezzen ki, és jelentkezzen be egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="5aafb-180">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="5aafb-181">ADAL teszi, hogy könnyen tooincorporate mindezeket a funkciókat az alkalmazásba közös identitás.</span><span class="sxs-lookup"><span data-stu-id="5aafb-181">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="5aafb-182">Azt gondoskodik az összes hello dirty munkahelyi meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a.</span><span class="sxs-lookup"><span data-stu-id="5aafb-182">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="5aafb-183">Valóban tooknow szüksége egy egyetlen API-hívással `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="5aafb-183">All you really need tooknow is a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="5aafb-184">Megadott befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként [Itt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="5aafb-184">For reference, hello completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="5aafb-185">Most már továbbléphet tooadditional identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="5aafb-185">You can now move on tooadditional identity scenarios.</span></span>  <span data-ttu-id="5aafb-186">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="5aafb-186">You may want tootry:</span></span>

[<span data-ttu-id="5aafb-187">.NET webes API-k, az Azure ad-vel biztonságos >></span><span class="sxs-lookup"><span data-stu-id="5aafb-187">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

