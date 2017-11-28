---
title: "Ismerkedés az Azure AD Xamarin |} Microsoft Docs"
description: "Xamarin-alkalmazásokat, amelyek a bejelentkezés az Azure AD integrálása, meghívják a OAuth protokollt használó Azure AD-védett API hozhatnak létre."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 54ee403f283bc5dc79911e2e813dd513ff595828
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a><span data-ttu-id="093ea-103">Xamarin-alkalmazásokba az Azure AD integrálása</span><span class="sxs-lookup"><span data-stu-id="093ea-103">Integrate Azure AD with Xamarin apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="093ea-104">A Xamarin mobilalkalmazások írhat a C# futtatható iOS, Android és Windows (mobileszközök és számítógépek).</span><span class="sxs-lookup"><span data-stu-id="093ea-104">With Xamarin, you can write mobile apps in C# that can run on iOS, Android, and Windows (mobile devices and PCs).</span></span> <span data-ttu-id="093ea-105">Ha az alkalmazást a Xamarin most felépítése, Azure Active Directory (Azure AD) egyszerűen az Azure AD-fiókkal rendelkező felhasználók hitelesítésére.</span><span class="sxs-lookup"><span data-stu-id="093ea-105">If you're building an app using Xamarin, Azure Active Directory (Azure AD) makes it simple to authenticate users with their Azure AD accounts.</span></span> <span data-ttu-id="093ea-106">Az alkalmazás minden webes API-t az Azure AD, például az Office 365 API-k vagy az Azure API által védett biztonságosan is használ.</span><span class="sxs-lookup"><span data-stu-id="093ea-106">The app can also securely consume any web API that's protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="093ea-107">Xamarin-védett erőforrások elérését igénylő alkalmazások esetén az Azure AD az Active Directory Authentication Library (ADAL) biztosít.</span><span class="sxs-lookup"><span data-stu-id="093ea-107">For Xamarin apps that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="093ea-108">ADAL kizárólagos célja megkönnyíti a hozzáférési jogkivonatok segítségével alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="093ea-108">The sole purpose of ADAL is to make it easy for apps to get access tokens.</span></span> <span data-ttu-id="093ea-109">Annak bemutatásához, hogy milyen egyszerűen, ez a cikk bemutatja, hogyan DirectorySearcher alkalmazások készítéséhez, amely:</span><span class="sxs-lookup"><span data-stu-id="093ea-109">To demonstrate how easy it is, this article shows how to build DirectorySearcher apps that:</span></span>

* <span data-ttu-id="093ea-110">Futtassa az iOS, Android, Windows asztali, Windows Phone és Windows Store.</span><span class="sxs-lookup"><span data-stu-id="093ea-110">Run on iOS, Android, Windows Desktop, Windows Phone, and Windows Store.</span></span>
* <span data-ttu-id="093ea-111">Egy egyetlen hordozható osztálytár (PCL) segítségével hitelesíti a felhasználókat, és a jogkivonatok lekérésére, az Azure AD Graph API-hoz.</span><span class="sxs-lookup"><span data-stu-id="093ea-111">Use a single portable class library (PCL) to authenticate users and get tokens for the Azure AD Graph API.</span></span>
* <span data-ttu-id="093ea-112">Keresse meg a megadott egyszerű felhasználónév a felhasználók számára egy könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="093ea-112">Search a directory for users with a given UPN.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="093ea-113">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="093ea-113">Before you get started</span></span>
* <span data-ttu-id="093ea-114">Töltse le a [projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), vagy töltse le a [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="093ea-114">Download the [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), or download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="093ea-115">Minden letöltés egy olyan Visual Studio 2013 megoldás.</span><span class="sxs-lookup"><span data-stu-id="093ea-115">Each download is a Visual Studio 2013 solution.</span></span>
* <span data-ttu-id="093ea-116">Akkor is, amelyben a felhasználók létrehozásához, és regisztrálja az alkalmazást az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="093ea-116">You also need an Azure AD tenant in which to create users and register the app.</span></span> <span data-ttu-id="093ea-117">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="093ea-117">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="093ea-118">Ha készen áll, hajtsa végre a következő négy részből áll.</span><span class="sxs-lookup"><span data-stu-id="093ea-118">When you are ready, follow the procedures in the next four sections.</span></span>

## <a name="step-1-set-up-your-xamarin-development-environment"></a><span data-ttu-id="093ea-119">1. lépés: A Xamarin fejlesztői környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="093ea-119">Step 1: Set up your Xamarin development environment</span></span>
<span data-ttu-id="093ea-120">Mivel ez az oktatóanyag iOS, Android és Windows projektek tartalmazza, kell mind a Visual Studio és Xamarin.</span><span class="sxs-lookup"><span data-stu-id="093ea-120">Because this tutorial includes projects for iOS, Android, and Windows, you need both Visual Studio and Xamarin.</span></span> <span data-ttu-id="093ea-121">A szükséges környezet létrehozása, a folyamat befejezéséhez a [állítsa össze, és telepítse a Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="093ea-121">To create the necessary environment, complete the process in [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) on MSDN.</span></span> <span data-ttu-id="093ea-122">Az utasítások tartalmazzák az anyag tekinthető kapcsolatos további Xamarin Várakozás a telepítés végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="093ea-122">The instructions include material that you can review to learn more about Xamarin while you're waiting for the installations to be completed.</span></span>

<span data-ttu-id="093ea-123">A telepítés befejezése után nyissa meg a megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="093ea-123">After you've completed the setup, open the solution in Visual Studio.</span></span> <span data-ttu-id="093ea-124">Itt megtalálja hat projektek: öt platform-specifikus projekteket és egy PCL, DirectorySearcher.cs, amely minden platform között meg lesz osztva.</span><span class="sxs-lookup"><span data-stu-id="093ea-124">There, you will find six projects: five platform-specific projects and one PCL, DirectorySearcher.cs, which will be shared across all platforms.</span></span>

## <a name="step-2-register-the-directorysearcher-app"></a><span data-ttu-id="093ea-125">2. lépés: A DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="093ea-125">Step 2: Register the DirectorySearcher app</span></span>
<span data-ttu-id="093ea-126">Ahhoz, hogy az alkalmazás a jogkivonatok lekérésére, először regisztrálja az Azure AD-bérlőben, és adja meg azt az Azure AD Graph API hozzáférési engedélyt.</span><span class="sxs-lookup"><span data-stu-id="093ea-126">To enable the app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API.</span></span> <span data-ttu-id="093ea-127">Ezt a következőképpen teheti meg:</span><span class="sxs-lookup"><span data-stu-id="093ea-127">Here's how:</span></span>

1. <span data-ttu-id="093ea-128">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="093ea-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="093ea-129">A felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="093ea-129">On the top bar, click your account.</span></span> <span data-ttu-id="093ea-130">Ekkor a a **Directory** listára, válassza ki az Active Directory-bérlőt, ahol az alkalmazást regisztrálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="093ea-130">Then, under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="093ea-131">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="093ea-131">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="093ea-132">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="093ea-132">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="093ea-133">Hozzon létre egy új **natív ügyfélalkalmazás**, kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="093ea-133">To create a new **Native Client Application**, follow the prompts.</span></span>
  * <span data-ttu-id="093ea-134">**Név** az alkalmazásnak, hogy a felhasználók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="093ea-134">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="093ea-135">**Átirányítási URI** protokollt és a karakterlánc kombinációját, amely az Azure AD token válaszok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="093ea-135">**Redirect URI** is a scheme and string combination that Azure AD uses to return token responses.</span></span> <span data-ttu-id="093ea-136">Adjon meg egy értéket (például http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="093ea-136">Enter a value (for example, http://DirectorySearcher).</span></span>
6. <span data-ttu-id="093ea-137">Regisztráció befejezését követően az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="093ea-137">After you’ve completed registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="093ea-138">Másolja az értéket a **alkalmazás** lapon, mert később szüksége.</span><span class="sxs-lookup"><span data-stu-id="093ea-138">Copy the value from the **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="093ea-139">Az a **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="093ea-139">On the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="093ea-140">Válassza ki **Microsoft Graph** , az API-t.</span><span class="sxs-lookup"><span data-stu-id="093ea-140">Select **Microsoft Graph** as the API.</span></span> <span data-ttu-id="093ea-141">A **delegált engedélyek**, vegye fel a **címtáradatok olvasása** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="093ea-141">Under **Delegated Permissions**, add the **Read Directory Data** permission.</span></span>  
<span data-ttu-id="093ea-142">Ez a művelet lehetővé teszi az alkalmazásnak, hogy a felhasználók számára a Graph API lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="093ea-142">This action enables the app to query the Graph API for users.</span></span>

## <a name="step-3-install-and-configure-adal"></a><span data-ttu-id="093ea-143">3. lépés: Telepítse és konfigurálja az adal-t</span><span class="sxs-lookup"><span data-stu-id="093ea-143">Step 3: Install and configure ADAL</span></span>
<span data-ttu-id="093ea-144">Most, hogy egy alkalmazást az Azure ad-ben, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="093ea-144">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="093ea-145">Ahhoz, hogy az adal-t az Azure AD kommunikálni, adjon neki az alkalmazás regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="093ea-145">To enable ADAL to communicate with Azure AD, give it some information about the app registration.</span></span>

1. <span data-ttu-id="093ea-146">Adal-t a Csomagkezelő konzol használatával adja hozzá a DirectorySearcher projekt.</span><span class="sxs-lookup"><span data-stu-id="093ea-146">Add ADAL to the DirectorySearcher project by using the Package Manager Console.</span></span>

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    <span data-ttu-id="093ea-147">Vegye figyelembe, hogy két függvénytár hivatkozások kerülnek minden olyan projekthez: az adal-t és a platform-specifikus részét PCL része.</span><span class="sxs-lookup"><span data-stu-id="093ea-147">Note that two library references are added to each project: the PCL portion of ADAL and a platform-specific portion.</span></span>
2. <span data-ttu-id="093ea-148">A DirectorySearcherLib projektben nyissa meg a DirectorySearcher.cs.</span><span class="sxs-lookup"><span data-stu-id="093ea-148">In the DirectorySearcherLib project, open DirectorySearcher.cs.</span></span>
3. <span data-ttu-id="093ea-149">Az osztály kombinálását cserélje le az értékeket, amelyeket a megadott Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="093ea-149">Replace the class member values with the values that you entered in the Azure portal.</span></span> <span data-ttu-id="093ea-150">A kód hivatkozik ezeket az értékeket, minden alkalommal adal-t.</span><span class="sxs-lookup"><span data-stu-id="093ea-150">Your code refers to these values whenever it uses ADAL.</span></span>

  * <span data-ttu-id="093ea-151">A *bérlői* a tartományt az Azure AD bérlője (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="093ea-151">The *tenant* is the domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="093ea-152">A *clientId* az ügyfél-azonosító az alkalmazás, amely a portálról kimásolt.</span><span class="sxs-lookup"><span data-stu-id="093ea-152">The *clientId* is the client ID of the app, which you copied from the portal.</span></span>
  * <span data-ttu-id="093ea-153">A *returnUri* az átirányítási URI-t a portálon (például http://DirectorySearcher) megadott értéke.</span><span class="sxs-lookup"><span data-stu-id="093ea-153">The *returnUri* is the redirect URI that you entered in the portal (for example, http://DirectorySearcher).</span></span>

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="093ea-154">4. lépés: Használja az adal TÁRAT jogkivonatok lekérni az Azure AD</span><span class="sxs-lookup"><span data-stu-id="093ea-154">Step 4: Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="093ea-155">Szinte teljes egészében az alkalmazás hitelesítési logikát létrejönnie `DirectorySearcher.SearchByAlias(...)`.</span><span class="sxs-lookup"><span data-stu-id="093ea-155">Almost all of the app's authentication logic lies in `DirectorySearcher.SearchByAlias(...)`.</span></span> <span data-ttu-id="093ea-156">Minden szükséges, a platform-specifikus projektben felelt meg a környezetfüggő paramétere a `DirectorySearcher` PCL.</span><span class="sxs-lookup"><span data-stu-id="093ea-156">All that's necessary in the platform-specific projects is to pass a contextual parameter to the `DirectorySearcher` PCL.</span></span>

1. <span data-ttu-id="093ea-157">Nyissa meg a DirectorySearcher.cs, és adja hozzá az új paramétere a `SearchByAlias(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="093ea-157">Open DirectorySearcher.cs, and then add a new parameter to the `SearchByAlias(...)` method.</span></span> <span data-ttu-id="093ea-158">`IPlatformParameters`az a környezetfüggő paraméter, amely magában foglalja a platform-specifikus objektumok, amelyek a hitelesítés végrehajtásához szükséges adal-t.</span><span class="sxs-lookup"><span data-stu-id="093ea-158">`IPlatformParameters` is the contextual parameter that encapsulates the platform-specific objects that ADAL needs to perform the authentication.</span></span>

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. <span data-ttu-id="093ea-159">Inicializálni `AuthenticationContext`, vagyis az elsődleges osztály az adal-t.</span><span class="sxs-lookup"><span data-stu-id="093ea-159">Initialize `AuthenticationContext`, which is the primary class of ADAL.</span></span>  
<span data-ttu-id="093ea-160">Ez a művelet továbbítja az adal-t az Azure AD-val kommunikációhoz szükséges koordinátái.</span><span class="sxs-lookup"><span data-stu-id="093ea-160">This action passes ADAL the coordinates it needs to communicate with Azure AD.</span></span>
3. <span data-ttu-id="093ea-161">Hívás `AcquireTokenAsync(...)`, amely elfogadja a `IPlatformParameters` objektumra, és hívja meg a hitelesítési folyamat szükséges a jogkivonat az alkalmazáshoz való visszatérésre.</span><span class="sxs-lookup"><span data-stu-id="093ea-161">Call `AcquireTokenAsync(...)`, which accepts the `IPlatformParameters` object and invokes the authentication flow that's necessary to return a token to the app.</span></span>

    ```C#
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    <span data-ttu-id="093ea-162">`AcquireTokenAsync(...)`először próbálja elküldeni a kért erőforrás (az ebben az esetben Graph API) jogkivonatot arra kéri a felhasználóktól a hitelesítő adataikat (keresztül gyorsítótárazását, vagy frissíteni a régi jogkivonatok) nélkül.</span><span class="sxs-lookup"><span data-stu-id="093ea-162">`AcquireTokenAsync(...)` first attempts to return a token for the requested resource (the Graph API in this case) without prompting users to enter their credentials (via caching or refreshing old tokens).</span></span> <span data-ttu-id="093ea-163">Szükség esetén ez mutatja felhasználók az Azure AD bejelentkezési oldal a kért token beszerzése előtt.</span><span class="sxs-lookup"><span data-stu-id="093ea-163">As necessary, it shows users the Azure AD sign-in page before acquiring the requested token.</span></span>
4. <span data-ttu-id="093ea-164">A hozzáférési jogkivonat csatolása a Graph API-kérelem a a **engedélyezési** fejléc:</span><span class="sxs-lookup"><span data-stu-id="093ea-164">Attach the access token to the Graph API request in the **Authorization** header:</span></span>

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

<span data-ttu-id="093ea-165">Ez minden, az a `DirectorySearcher` PCL és az alkalmazás csomagazonosítóját identitás kapcsolatos kódot.</span><span class="sxs-lookup"><span data-stu-id="093ea-165">That's all for the `DirectorySearcher` PCL and the app's identity-related code.</span></span> <span data-ttu-id="093ea-166">Most már hívni a `SearchByAlias(...)` metódus az egyes platformokon nézetekben, és ahol szükséges, adja hozzá a felhasználói felület életciklusát megfelelően kezeli a kódot.</span><span class="sxs-lookup"><span data-stu-id="093ea-166">All that remains is to call the `SearchByAlias(...)` method in each platform's views and, where necessary, to add code for correctly handling the UI lifecycle.</span></span>

### <a name="android"></a><span data-ttu-id="093ea-167">Android</span><span class="sxs-lookup"><span data-stu-id="093ea-167">Android</span></span>
1. <span data-ttu-id="093ea-168">A MainActivity.cs, adjon hozzá egy `SearchByAlias(...)` kezelő kattintson a gombra:</span><span class="sxs-lookup"><span data-stu-id="093ea-168">In MainActivity.cs, add a call to `SearchByAlias(...)` in the button click handler:</span></span>

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. <span data-ttu-id="093ea-169">Bírálja felül a `OnActivityResult` továbbítani a hitelesítés életciklusa metódus átirányítja a felhasználókat vissza a megfelelő módszert.</span><span class="sxs-lookup"><span data-stu-id="093ea-169">Override the `OnActivityResult` lifecycle method to forward any authentication redirects back to the appropriate method.</span></span> <span data-ttu-id="093ea-170">Adal-t egy segédmetódust nyújt az Android:</span><span class="sxs-lookup"><span data-stu-id="093ea-170">ADAL provides a helper method for this in Android:</span></span>

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a><span data-ttu-id="093ea-171">Windows asztali</span><span class="sxs-lookup"><span data-stu-id="093ea-171">Windows Desktop</span></span>
<span data-ttu-id="093ea-172">A MainWindow.xaml.cs, hívás `SearchByAlias(...)` úgy, hogy egy `WindowInteropHelper` az asztalon `PlatformParameters` objektum:</span><span class="sxs-lookup"><span data-stu-id="093ea-172">In MainWindow.xaml.cs, make a call to `SearchByAlias(...)` by passing a `WindowInteropHelper` in the desktop's `PlatformParameters` object:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a><span data-ttu-id="093ea-173">iOS</span><span class="sxs-lookup"><span data-stu-id="093ea-173">iOS</span></span>
<span data-ttu-id="093ea-174">A DirSearchClient_iOSViewController.cs, az iOS `PlatformParameters` objektum veszi a nézetvezérlő hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="093ea-174">In DirSearchClient_iOSViewController.cs, the iOS `PlatformParameters` object takes a reference to the View Controller:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a><span data-ttu-id="093ea-175">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="093ea-175">Windows Universal</span></span>
<span data-ttu-id="093ea-176">Az univerzális Windows-, nyissa meg a MainPage.xaml.cs, és megvalósításához a `Search` metódust.</span><span class="sxs-lookup"><span data-stu-id="093ea-176">In Windows Universal, open MainPage.xaml.cs, and then implement the `Search` method.</span></span> <span data-ttu-id="093ea-177">Ez a módszer egy segédmetódust megosztott projektben frissíteni a felhasználói felület szükség esetén használja.</span><span class="sxs-lookup"><span data-stu-id="093ea-177">This method uses a helper method in a shared project to update UI as necessary.</span></span>

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a><span data-ttu-id="093ea-178">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="093ea-178">What's next</span></span>
<span data-ttu-id="093ea-179">Most már rendelkezik egy működő Xamarin-alkalmazás, amely hitelesíti a felhasználókat, és biztonságosan hívására webes API-k OAuth 2.0 különböző öt különböző platformokon.</span><span class="sxs-lookup"><span data-stu-id="093ea-179">You now have a working Xamarin app that can authenticate users and securely call web APIs by using OAuth 2.0 across five different platforms.</span></span>

<span data-ttu-id="093ea-180">Ha már nincs beállítva, feltöltve a felhasználókkal a bérlő, most az ehhez idő.</span><span class="sxs-lookup"><span data-stu-id="093ea-180">If you haven’t already populated your tenant with users, now is the time to do so.</span></span>

1. <span data-ttu-id="093ea-181">A DirectorySearcher alkalmazás futtatását, és jelentkezzen be a felhasználók közül.</span><span class="sxs-lookup"><span data-stu-id="093ea-181">Run your DirectorySearcher app, and then sign in with one of the users.</span></span>
2. <span data-ttu-id="093ea-182">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="093ea-182">Search for other users based on their UPN.</span></span>

<span data-ttu-id="093ea-183">ADAL megkönnyíti a közös szervezetiidentitás-szolgáltatások beépítse az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="093ea-183">ADAL makes it easy to incorporate common identity features into the app.</span></span> <span data-ttu-id="093ea-184">Azt gondoskodik a dirty munkát meg, például a gyorsítótár kezelése OAuth protokoll támogatása, a felhasználó egy bejelentkezési felhasználói felület, a bemutató, és a jogkivonatok frissítése lejárt.</span><span class="sxs-lookup"><span data-stu-id="093ea-184">It takes care of all the dirty work for you, such as cache management, OAuth protocol support, presenting the user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="093ea-185">Csak egyetlen API-hívással, ismernie kell `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="093ea-185">You need to know only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="093ea-186">Hivatkozás, töltse le a [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (a konfigurációs értékek nélkül).</span><span class="sxs-lookup"><span data-stu-id="093ea-186">For reference, download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="093ea-187">Most már továbbléphet további identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="093ea-187">You can now move on to additional identity scenarios.</span></span> <span data-ttu-id="093ea-188">Például [egy .NET webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="093ea-188">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
