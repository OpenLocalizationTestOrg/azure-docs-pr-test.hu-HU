---
title: "Ismerkedés az Azure AD-Windows Phone |} Microsoft Docs"
description: "Hogyan hozható létre, amely integrálható az Azure ad-val jelentkezzen be, és meghívja az Azure AD egy Windows Phone-alkalmazás OAuth protokollt használó API-k védett."
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
ms.openlocfilehash: 03c4b6d225dce99d79ef6c1ba2af43af8dea3eae
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a><span data-ttu-id="482dd-103">Azure AD integrálása a Windows Phone-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="482dd-103">Integrate Azure AD with a Windows Phone App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="482dd-104">A Windows Phone 8.1-es és korábbi verziójú projektek nem támogatottak a Visual Studio 2017-ben.</span><span class="sxs-lookup"><span data-stu-id="482dd-104">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="482dd-105">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="482dd-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="482dd-106">Ha a Windows Phone 8.1-alkalmazást, az Azure AD segítségével egyszerű és magától értetődő, hogy a felhasználókat az Active Directory-fiókok hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="482dd-106">If you're developing a Windows Phone 8.1 app, Azure AD makes it simple and straightforward for you to authenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="482dd-107">Emellett lehetővé teszi az alkalmazás minden webes API-t az Azure AD, például az Office 365 API-k vagy az Azure API által védett biztonságosan felhasználását.</span><span class="sxs-lookup"><span data-stu-id="482dd-107">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

> [!NOTE]
> <span data-ttu-id="482dd-108">A fenti ADAL v2.0 használja.</span><span class="sxs-lookup"><span data-stu-id="482dd-108">This code sample uses ADAL v2.0.</span></span>  <span data-ttu-id="482dd-109">A legújabb technológiák, érdemes lehet helyette próbálkozzon az [Windows Universal oktatóanyagot ADAL v3.0 használatával](active-directory-devquickstarts-windowsstore.md).</span><span class="sxs-lookup"><span data-stu-id="482dd-109">For the latest technology, you may want to instead try our [Windows Universal Tutorial using ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span></span>  <span data-ttu-id="482dd-110">Ha valóban egy alkalmazást a Windows Phone 8.1-es, ez az a megfelelő helyen.</span><span class="sxs-lookup"><span data-stu-id="482dd-110">If you are indeed building an app for Windows Phone 8.1, this is the right place.</span></span>  <span data-ttu-id="482dd-111">ADAL v2.0 továbbra is teljes mértékben támogatja, és az ajánlott módszer a Windows Phone 8.1-es alkalmazások agianst fejlődő használja az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="482dd-111">ADAL v2.0 is still fully supported, and is the recommended way of developing apps agianst Windows Phone 8.1 using Azure AD.</span></span>
> 
> 

<span data-ttu-id="482dd-112">A .NET natív ügyfelek esetében a védett erőforrások eléréséhez szükséges az Azure AD az Active Directory Authentication Library, vagy az adal-t biztosít.</span><span class="sxs-lookup"><span data-stu-id="482dd-112">For .NET native clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="482dd-113">ADAL meg azzal a céllal életben megkönnyíti a hozzáférési jogkivonatok lekérésére, ha az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="482dd-113">ADAL’s sole purpose in life is to make it easy for your app to get access tokens.</span></span>  <span data-ttu-id="482dd-114">Annak bemutatásához, milyen könnyű van, itt azt fogja össze egy "Directory keresővel" Windows Phone 8.1-alkalmazást, amely:</span><span class="sxs-lookup"><span data-stu-id="482dd-114">To demonstrate just how easy it is, here we’ll build a "Directory Searcher" Windows Phone 8.1 app that:</span></span>

* <span data-ttu-id="482dd-115">Lekérdezi hozzáférési jogkivonatok hívásakor az Azure AD Graph API segítségével a [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="482dd-115">Gets access tokens for calling the Azure AD Graph API using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="482dd-116">Megkeresi a megadott egyszerű felhasználónév a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="482dd-116">Searches a directory for users with a given UPN.</span></span>
* <span data-ttu-id="482dd-117">Kimenő felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="482dd-117">Signs users out.</span></span>

<span data-ttu-id="482dd-118">A teljes működő alkalmazás létrehozásához szüksége:</span><span class="sxs-lookup"><span data-stu-id="482dd-118">To build the complete working application, you’ll need to:</span></span>

1. <span data-ttu-id="482dd-119">Az alkalmazás regisztrálása az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="482dd-119">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="482dd-120">Telepítése és konfigurálása adal-t.</span><span class="sxs-lookup"><span data-stu-id="482dd-120">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="482dd-121">Adal-t használó tokenek lekérni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="482dd-121">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="482dd-122">A kezdéshez [töltse le a projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="482dd-122">To get started, [download a skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="482dd-123">Egy Visual Studio 2013 megoldást.</span><span class="sxs-lookup"><span data-stu-id="482dd-123">Each is a Visual Studio 2013 solution.</span></span>  <span data-ttu-id="482dd-124">Biztosítani kell, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="482dd-124">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="482dd-125">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="482dd-125">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-the-directory-searcher-application"></a><span data-ttu-id="482dd-126">1. A könyvtár keresővel alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="482dd-126">1. Register the Directory Searcher Application</span></span>
<span data-ttu-id="482dd-127">Ahhoz, hogy az alkalmazás a jogkivonatok lekérésére, először regisztrálja az Azure AD-bérlőben, és adja meg azt az Azure AD Graph API hozzáférése:</span><span class="sxs-lookup"><span data-stu-id="482dd-127">To enable your app to get tokens, you’ll first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="482dd-128">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="482dd-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="482dd-129">A felső eszköztáron kattintson a fiókhoz, majd a a **Directory** menüben válassza ki az Active Directory-bérlőt, ahová be szeretné-e az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="482dd-129">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="482dd-130">Kattintson a **több szolgáltatások** a bal oldali navigációs válassza **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="482dd-130">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="482dd-131">Kattintson a **App regisztrációk** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="482dd-131">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="482dd-132">Kövesse az utasításokat, és hozzon létre egy új **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="482dd-132">Follow the prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="482dd-133">A **neve** az alkalmazás ismerteti az alkalmazást a végfelhasználók számára</span><span class="sxs-lookup"><span data-stu-id="482dd-133">The **Name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="482dd-134">A **átirányítási Uri-** protokollt és a karakterlánc kombinációját, amely az Azure AD vissza a token válaszokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="482dd-134">The **Redirect Uri** is a scheme and string combination that Azure AD will use to return token responses.</span></span>  <span data-ttu-id="482dd-135">Például a lépést, adjon meg egy helyőrzőt értéket `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="482dd-135">Enter a placeholder value for now, e.g. `http://DirectorySearcher`.</span></span>  <span data-ttu-id="482dd-136">Később lesz lecserélni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="482dd-136">We'll replace this value later.</span></span>
6. <span data-ttu-id="482dd-137">Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="482dd-137">Once you’ve completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="482dd-138">Ez az érték kell a következő szakaszokban lévő, másolja az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="482dd-138">You’ll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="482dd-139">Az a **beállítások** lapon, válassza ki **szükséges engedélyek** válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="482dd-139">From the **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="482dd-140">Válassza ki a **Microsoft Graph** az API-t, és adja hozzá a **címtáradatok olvasása** engedélyt a **delegált engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="482dd-140">Select the **Microsoft Graph** as the API and add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="482dd-141">Ez lehetővé teszi az alkalmazás lekérdezése a Graph API-t a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="482dd-141">This will enable your application to query the Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="482dd-142">2. Telepítése és konfigurálása adal-t</span><span class="sxs-lookup"><span data-stu-id="482dd-142">2. Install & Configure ADAL</span></span>
<span data-ttu-id="482dd-143">Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="482dd-143">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="482dd-144">Ahhoz, hogy az adal-t tudjanak kommunikálni az Azure AD meg kell biztosítania, bizonyos információkat az alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="482dd-144">In order for ADAL to be able to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="482dd-145">Kezdje az adal-t ad hozzá a DirectorySearcher projekt a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="482dd-145">Begin by adding ADAL to the DirectorySearcher project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="482dd-146">A DirectorySearcher projektben nyissa meg a `MainPage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="482dd-146">In the DirectorySearcher project, open `MainPage.xaml.cs`.</span></span>  <span data-ttu-id="482dd-147">Cserélje le az értékeket a `Config Values` régió be az Azure-portálra megadott értékeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="482dd-147">Replace the values in the `Config Values` region to reflect the values you input into the Azure Portal.</span></span>  <span data-ttu-id="482dd-148">A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="482dd-148">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="482dd-149">A `tenant` a tartományt az Azure AD-bérlőn, pl.: contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="482dd-149">The `tenant` is the domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="482dd-150">A `clientId` ClientID-azonosítóját az alkalmazás a portálról másolt van.</span><span class="sxs-lookup"><span data-stu-id="482dd-150">The `clientId` is the clientId of your application you copied from the portal.</span></span>
* <span data-ttu-id="482dd-151">Most meg kell a Windows Phone-alkalmazás visszahívási URI-jának felderítése.</span><span class="sxs-lookup"><span data-stu-id="482dd-151">You now need to discover the callback uri for your Windows Phone app.</span></span>  <span data-ttu-id="482dd-152">Ez a sor állítson be egy töréspontot a `MainPage` módszert:</span><span class="sxs-lookup"><span data-stu-id="482dd-152">Set a breakpoint on this line in the `MainPage` method:</span></span>

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* <span data-ttu-id="482dd-153">Futtassa az alkalmazást, és másolja fenntartott értékének `redirectUri` amikor a töréspont elérte-e.</span><span class="sxs-lookup"><span data-stu-id="482dd-153">Run the app, and copy aside the value of `redirectUri` when the breakpoint is hit.</span></span>  <span data-ttu-id="482dd-154">Az alábbiakhoz hasonlóan</span><span class="sxs-lookup"><span data-stu-id="482dd-154">It should look something like</span></span>

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* <span data-ttu-id="482dd-155">Vissza a **konfigurálása** fülre az alkalmazás az Azure felügyeleti portálján, cserélje le a a **RedirectUri** ezzel az értékkel.</span><span class="sxs-lookup"><span data-stu-id="482dd-155">Back on the **Configure** tab of your application in the Azure Management Portal, replace the value of the **RedirectUri** with this value.</span></span>  

## <a name="3-use-adal-to-get-tokens-from-aad"></a><span data-ttu-id="482dd-156">3. Adal-t használó tokenek lekérése az aad-ben</span><span class="sxs-lookup"><span data-stu-id="482dd-156">3. Use ADAL to Get Tokens from AAD</span></span>
<span data-ttu-id="482dd-157">Az alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja `authContext.AcquireToken(…)`, és a többi adal-t.</span><span class="sxs-lookup"><span data-stu-id="482dd-157">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does the rest.</span></span>  

* <span data-ttu-id="482dd-158">Az első lépés az, hogy az alkalmazás inicializálása `AuthenticationContext` -ADAL tartozó elsődleges osztály.</span><span class="sxs-lookup"><span data-stu-id="482dd-158">The first step is to initialize your app’s `AuthenticationContext` - ADAL’s primary class.</span></span>  <span data-ttu-id="482dd-159">Ez az adott át adal-t a koordináták kommunikálni az Azure AD, és mondja el, akkor jogkivonatok gyorsítótárazásának kell.</span><span class="sxs-lookup"><span data-stu-id="482dd-159">This is where you pass ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* <span data-ttu-id="482dd-160">Keresse meg a `Search(...)` metódus, amely akkor kell meghívni, amikor a felhasználó cliks a "Search" gombra az alkalmazás felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="482dd-160">Now locate the `Search(...)` method, which will be invoked when the user cliks the "Search" button in the app's UI.</span></span>  <span data-ttu-id="482dd-161">Ez a módszer az Azure AD Graph API lekérdezéshez GET kérés teszi a felhasználók számára, amelyek egyszerű felhasználónév a megadott keresési kifejezés kezdődik.</span><span class="sxs-lookup"><span data-stu-id="482dd-161">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="482dd-162">Ahhoz, hogy a Graph API lekérdezni, meg kell adnia egy access_token a, de a `Authorization` fejlécet a kérelem - ezt az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="482dd-162">But in order to query the Graph API, you need to include an access_token in the `Authorization` header of the request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* <span data-ttu-id="482dd-163">Interaktív hitelesítésre szükség, ha adal-t fogja használni a Windows Phone webes hitelesítés Broker (WAB) és [folytatási modell](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) az Azure AD bejelentkezési megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="482dd-163">If interactive authentication is necessary, ADAL will use Windows Phone's Web Authentication Broker (WAB) and [continuation model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) to display the Azure AD sign in page.</span></span>  <span data-ttu-id="482dd-164">Amikor a felhasználó bejelentkezik, az alkalmazás ADAL továbbítani kell a WAB interakció eredményeit.</span><span class="sxs-lookup"><span data-stu-id="482dd-164">When the user signs in, your app needs to pass ADAL the results of the WAB interaction.</span></span>  <span data-ttu-id="482dd-165">Ez az végrehajtási egyszerűen a `ContinueWebAuthentication` felület:</span><span class="sxs-lookup"><span data-stu-id="482dd-165">This is as simple as implementing the `ContinueWebAuthentication` interface:</span></span>

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* <span data-ttu-id="482dd-166">Mostantól az időpontok a `AuthenticationResult` , amely az alkalmazás adal-t küld vissza.</span><span class="sxs-lookup"><span data-stu-id="482dd-166">Now it's time to use the `AuthenticationResult` that ADAL returned to your app.</span></span>  <span data-ttu-id="482dd-167">Az a `QueryGraph(...)` visszahívási, csatlakoztassa a access_token szerezte be a GET-kérés hitelesítési fejlécéhez a:</span><span class="sxs-lookup"><span data-stu-id="482dd-167">In the `QueryGraph(...)` callback, attach the access_token you acquired to the GET request in the Authorization header:</span></span>

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* <span data-ttu-id="482dd-168">Használhatja a `AuthenticationResult` objektum információt szeretne megjeleníteni a felhasználó az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="482dd-168">You can also use the `AuthenticationResult` object to display information about the user in your app.</span></span> <span data-ttu-id="482dd-169">Az a `QueryGraph(...)` metódust, az eredmény segítségével az oldalon a felhasználói azonosító megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="482dd-169">In the `QueryGraph(...)` method, use the result to show the user's id on the page:</span></span>

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* <span data-ttu-id="482dd-170">Adal-t használó végül jelentkezzen ki, valamint az alkalmazás a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="482dd-170">Finally, you can use ADAL to sign the user out of hte application as well.</span></span>  <span data-ttu-id="482dd-171">Amikor a felhasználó a "Sign Out" gombra kattint, annak érdekében, hogy a következő hívást szeretnénk `AcquireTokenSilentAsync(...)` sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="482dd-171">When the user clicks the "Sign Out" button, we want to ensure that the next call to `AcquireTokenSilentAsync(...)` will fail.</span></span>  <span data-ttu-id="482dd-172">Ez az adal-t, az egyszerű módon, ha a token gyorsítótár kiürítése:</span><span class="sxs-lookup"><span data-stu-id="482dd-172">With ADAL, this is as easy as clearing the token cache:</span></span>

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

<span data-ttu-id="482dd-173">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="482dd-173">Congratulations!</span></span> <span data-ttu-id="482dd-174">Most működik Windows Phone-alkalmazást, amely képes a felhasználók hitelesítésére, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és a felhasználó alapszintű adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="482dd-174">You now have a working Windows Phone app that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="482dd-175">Ha még nem tette meg, most már az egyes felhasználóival a bérlő feltölti idő.</span><span class="sxs-lookup"><span data-stu-id="482dd-175">If you haven’t already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="482dd-176">Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="482dd-176">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="482dd-177">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="482dd-177">Search for other users based on their UPN.</span></span>  <span data-ttu-id="482dd-178">Zárja be az alkalmazást, és futtassa újból.</span><span class="sxs-lookup"><span data-stu-id="482dd-178">Close the app, and re-run it.</span></span>  <span data-ttu-id="482dd-179">Figyelje meg, hogyan az ügyfél helye változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="482dd-179">Notice how the user’s session remains intact.</span></span>  <span data-ttu-id="482dd-180">Jelentkezzen ki, és jelentkezzen be egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="482dd-180">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="482dd-181">Az adal TÁRAT megkönnyíti, hogy átfogó mindezeket a közös identitás funkciókat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="482dd-181">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="482dd-182">Ez gondoskodik a dirty munkát meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, a felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a bemutató.</span><span class="sxs-lookup"><span data-stu-id="482dd-182">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="482dd-183">Biztosan tudni, hogy szüksége egy egyetlen API-hívással `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="482dd-183">All you really need to know is a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="482dd-184">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként biztosított [Itt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="482dd-184">For reference, the completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="482dd-185">Most már továbbléphet további identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="482dd-185">You can now move on to additional identity scenarios.</span></span>  <span data-ttu-id="482dd-186">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="482dd-186">You may want to try:</span></span>

[<span data-ttu-id="482dd-187">.NET webes API-k, az Azure ad-vel biztonságos >></span><span class="sxs-lookup"><span data-stu-id="482dd-187">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

