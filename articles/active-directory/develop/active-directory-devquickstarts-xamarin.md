---
title: "Első lépések AD Xamarin aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 6a0d189648b7071558ac1cf2b908808668960a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a><span data-ttu-id="9d829-103">Xamarin-alkalmazásokba az Azure AD integrálása</span><span class="sxs-lookup"><span data-stu-id="9d829-103">Integrate Azure AD with Xamarin apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="9d829-104">A Xamarin mobilalkalmazások írhat a C# futtatható iOS, Android és Windows (mobileszközök és számítógépek).</span><span class="sxs-lookup"><span data-stu-id="9d829-104">With Xamarin, you can write mobile apps in C# that can run on iOS, Android, and Windows (mobile devices and PCs).</span></span> <span data-ttu-id="9d829-105">Ha az alkalmazást a Xamarin most felépítése, Azure Active Directory (Azure AD) lehetővé teszi az Azure AD-fiókok felhasználóinak egyszerű tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="9d829-105">If you're building an app using Xamarin, Azure Active Directory (Azure AD) makes it simple tooauthenticate users with their Azure AD accounts.</span></span> <span data-ttu-id="9d829-106">hello app biztonságosan is használ egyetlen webes API-t az Azure AD, például az Office 365 API-k hello vagy hello Azure API által védett.</span><span class="sxs-lookup"><span data-stu-id="9d829-106">hello app can also securely consume any web API that's protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="9d829-107">A Xamarin-alkalmazásokat, amelyeket tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library (ADAL) biztosít.</span><span class="sxs-lookup"><span data-stu-id="9d829-107">For Xamarin apps that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="9d829-108">hello kizárólagos ADAL célja toomake megkönnyítik az alkalmazások tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="9d829-108">hello sole purpose of ADAL is toomake it easy for apps tooget access tokens.</span></span> <span data-ttu-id="9d829-109">toodemonstrate milyen egyszerűen van, ez a cikk bemutatja, hogyan toobuild DirectorySearcher alkalmazásokat, amelyek:</span><span class="sxs-lookup"><span data-stu-id="9d829-109">toodemonstrate how easy it is, this article shows how toobuild DirectorySearcher apps that:</span></span>

* <span data-ttu-id="9d829-110">Futtassa az iOS, Android, Windows asztali, Windows Phone és Windows Store.</span><span class="sxs-lookup"><span data-stu-id="9d829-110">Run on iOS, Android, Windows Desktop, Windows Phone, and Windows Store.</span></span>
* <span data-ttu-id="9d829-111">Egy egyetlen hordozható osztály könyvtár (PCL) tooauthenticate felhasználók és az Azure AD Graph API hello jogkivonatokhoz.</span><span class="sxs-lookup"><span data-stu-id="9d829-111">Use a single portable class library (PCL) tooauthenticate users and get tokens for hello Azure AD Graph API.</span></span>
* <span data-ttu-id="9d829-112">Keresse meg a megadott egyszerű felhasználónév a felhasználók számára egy könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="9d829-112">Search a directory for users with a given UPN.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="9d829-113">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="9d829-113">Before you get started</span></span>
* <span data-ttu-id="9d829-114">Töltse le a hello [projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), vagy töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="9d829-114">Download hello [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), or download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="9d829-115">Minden letöltés egy olyan Visual Studio 2013 megoldás.</span><span class="sxs-lookup"><span data-stu-id="9d829-115">Each download is a Visual Studio 2013 solution.</span></span>
* <span data-ttu-id="9d829-116">Az Azure AD-bérlő toocreate felhasználók és a nyilvántartás hello alkalmazást is kell.</span><span class="sxs-lookup"><span data-stu-id="9d829-116">You also need an Azure AD tenant in which toocreate users and register hello app.</span></span> <span data-ttu-id="9d829-117">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="9d829-117">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="9d829-118">Ha készen áll, hello eljárásokat hajtsa végre a következő négy részből áll hello.</span><span class="sxs-lookup"><span data-stu-id="9d829-118">When you are ready, follow hello procedures in hello next four sections.</span></span>

## <a name="step-1-set-up-your-xamarin-development-environment"></a><span data-ttu-id="9d829-119">1. lépés: A Xamarin fejlesztői környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="9d829-119">Step 1: Set up your Xamarin development environment</span></span>
<span data-ttu-id="9d829-120">Mivel ez az oktatóanyag iOS, Android és Windows projektek tartalmazza, kell mind a Visual Studio és Xamarin.</span><span class="sxs-lookup"><span data-stu-id="9d829-120">Because this tutorial includes projects for iOS, Android, and Windows, you need both Visual Studio and Xamarin.</span></span> <span data-ttu-id="9d829-121">teljes hello folyamat toocreate hello szükséges környezet [állítsa össze, és telepítse a Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="9d829-121">toocreate hello necessary environment, complete hello process in [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) on MSDN.</span></span> <span data-ttu-id="9d829-122">hello utasítások tartalmaznak anyagot további, Xamarin kapcsolatos toolearn tekinthető hello telepítések toobe befejeződött a várakozás közben.</span><span class="sxs-lookup"><span data-stu-id="9d829-122">hello instructions include material that you can review toolearn more about Xamarin while you're waiting for hello installations toobe completed.</span></span>

<span data-ttu-id="9d829-123">Hello telepítő befejezése után nyissa meg a hello megoldást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d829-123">After you've completed hello setup, open hello solution in Visual Studio.</span></span> <span data-ttu-id="9d829-124">Itt megtalálja hat projektek: öt platform-specifikus projekteket és egy PCL, DirectorySearcher.cs, amely minden platform között meg lesz osztva.</span><span class="sxs-lookup"><span data-stu-id="9d829-124">There, you will find six projects: five platform-specific projects and one PCL, DirectorySearcher.cs, which will be shared across all platforms.</span></span>

## <a name="step-2-register-hello-directorysearcher-app"></a><span data-ttu-id="9d829-125">2. lépés: Hello DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="9d829-125">Step 2: Register hello DirectorySearcher app</span></span>
<span data-ttu-id="9d829-126">tooenable hello alkalmazási tooget jogkivonatok, először tooregister azt az Azure ad bérlői, és adja meg az engedélyt tooaccess hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="9d829-126">tooenable hello app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API.</span></span> <span data-ttu-id="9d829-127">Ezt a következőképpen teheti meg:</span><span class="sxs-lookup"><span data-stu-id="9d829-127">Here's how:</span></span>

1. <span data-ttu-id="9d829-128">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9d829-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9d829-129">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="9d829-129">On hello top bar, click your account.</span></span> <span data-ttu-id="9d829-130">Ekkor a hello **Directory** listán, válassza hello tooregister hello alkalmazás kívánt Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="9d829-130">Then, under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="9d829-131">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9d829-131">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="9d829-132">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9d829-132">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="9d829-133">új toocreate **natív ügyfélalkalmazás**, hello utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="9d829-133">toocreate a new **Native Client Application**, follow hello prompts.</span></span>
  * <span data-ttu-id="9d829-134">**Név** hello app toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9d829-134">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="9d829-135">**Átirányítási URI** , hogy az Azure AD által használt tooreturn token válaszok protokollt és a karakterlánc kombinációját.</span><span class="sxs-lookup"><span data-stu-id="9d829-135">**Redirect URI** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span> <span data-ttu-id="9d829-136">Adjon meg egy értéket (például http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="9d829-136">Enter a value (for example, http://DirectorySearcher).</span></span>
6. <span data-ttu-id="9d829-137">Regisztráció befejezését követően az Azure AD hozzárendel hello app egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="9d829-137">After you’ve completed registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="9d829-138">Hello érték másolása hello **alkalmazás** lapon, mert később szüksége.</span><span class="sxs-lookup"><span data-stu-id="9d829-138">Copy hello value from hello **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="9d829-139">A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9d829-139">On hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="9d829-140">Válassza ki **Microsoft Graph** , hello API.</span><span class="sxs-lookup"><span data-stu-id="9d829-140">Select **Microsoft Graph** as hello API.</span></span> <span data-ttu-id="9d829-141">A **delegált engedélyek**, vegye fel a hello **címtáradatok olvasása** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="9d829-141">Under **Delegated Permissions**, add hello **Read Directory Data** permission.</span></span>  
<span data-ttu-id="9d829-142">Ez a művelet lehetővé teszi, hogy a hello app tooquery hello Graph API a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="9d829-142">This action enables hello app tooquery hello Graph API for users.</span></span>

## <a name="step-3-install-and-configure-adal"></a><span data-ttu-id="9d829-143">3. lépés: Telepítse és konfigurálja az adal-t</span><span class="sxs-lookup"><span data-stu-id="9d829-143">Step 3: Install and configure ADAL</span></span>
<span data-ttu-id="9d829-144">Most, hogy egy alkalmazást az Azure ad-ben, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="9d829-144">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="9d829-145">tooenable ADAL toocommunicate az Azure ad-vel, adjon neki hello app regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="9d829-145">tooenable ADAL toocommunicate with Azure AD, give it some information about hello app registration.</span></span>

1. <span data-ttu-id="9d829-146">Adja hozzá az ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="9d829-146">Add ADAL toohello DirectorySearcher project by using hello Package Manager Console.</span></span>

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

    <span data-ttu-id="9d829-147">Vegye figyelembe, hogy két függvénytár hivatkozások kerülnek tooeach projekt: hello PCL része adal-t és a platform-specifikus részét.</span><span class="sxs-lookup"><span data-stu-id="9d829-147">Note that two library references are added tooeach project: hello PCL portion of ADAL and a platform-specific portion.</span></span>
2. <span data-ttu-id="9d829-148">Hello DirectorySearcherLib projektben nyissa meg DirectorySearcher.cs.</span><span class="sxs-lookup"><span data-stu-id="9d829-148">In hello DirectorySearcherLib project, open DirectorySearcher.cs.</span></span>
3. <span data-ttu-id="9d829-149">Cserélje le a hello osztály kombinálását hello Azure-portálon megadott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="9d829-149">Replace hello class member values with hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="9d829-150">Kódja toothese értékek hivatkozik, amikor az adal-t használ.</span><span class="sxs-lookup"><span data-stu-id="9d829-150">Your code refers toothese values whenever it uses ADAL.</span></span>

  * <span data-ttu-id="9d829-151">Hello *bérlői* hello tartománya, akkor az Azure AD bérlője (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="9d829-151">hello *tenant* is hello domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="9d829-152">Hello *clientId* hello alkalmazás, amely hello portálról másolt hello ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9d829-152">hello *clientId* is hello client ID of hello app, which you copied from hello portal.</span></span>
  * <span data-ttu-id="9d829-153">Hello *returnUri* hello átirányítási URI-t hello portal (például http://DirectorySearcher) megadott értéke.</span><span class="sxs-lookup"><span data-stu-id="9d829-153">hello *returnUri* is hello redirect URI that you entered in hello portal (for example, http://DirectorySearcher).</span></span>

## <a name="step-4-use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="9d829-154">4. lépés: Használja ADAL tooget jogkivonatok az Azure AD-ből</span><span class="sxs-lookup"><span data-stu-id="9d829-154">Step 4: Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="9d829-155">Szinte teljes egészében hello alkalmazás hitelesítési logikát létrejönnie `DirectorySearcher.SearchByAlias(...)`.</span><span class="sxs-lookup"><span data-stu-id="9d829-155">Almost all of hello app's authentication logic lies in `DirectorySearcher.SearchByAlias(...)`.</span></span> <span data-ttu-id="9d829-156">Minden szükséges hello platform-specifikus projektben egy környezetfüggő paraméter toohello toopass `DirectorySearcher` PCL.</span><span class="sxs-lookup"><span data-stu-id="9d829-156">All that's necessary in hello platform-specific projects is toopass a contextual parameter toohello `DirectorySearcher` PCL.</span></span>

1. <span data-ttu-id="9d829-157">Nyissa meg a DirectorySearcher.cs, és adja hozzá az egy új paraméter toohello `SearchByAlias(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="9d829-157">Open DirectorySearcher.cs, and then add a new parameter toohello `SearchByAlias(...)` method.</span></span> <span data-ttu-id="9d829-158">`IPlatformParameters`hello környezetfüggő paraméter, amely magában foglalja a hello platform-specifikus objektumokat, ADAL igényeinek tooperform hello hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="9d829-158">`IPlatformParameters` is hello contextual parameter that encapsulates hello platform-specific objects that ADAL needs tooperform hello authentication.</span></span>

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. <span data-ttu-id="9d829-159">Inicializálni `AuthenticationContext`, vagyis hello elsődleges osztály az adal-t.</span><span class="sxs-lookup"><span data-stu-id="9d829-159">Initialize `AuthenticationContext`, which is hello primary class of ADAL.</span></span>  
<span data-ttu-id="9d829-160">Ez a művelet fázisok ADAL hello koordinálja az igényeinek toocommunicate az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="9d829-160">This action passes ADAL hello coordinates it needs toocommunicate with Azure AD.</span></span>
3. <span data-ttu-id="9d829-161">Hívja `AcquireTokenAsync(...)`, amely elfogadja hello `IPlatformParameters` objektumra, és hívja meg, amely a token toohello alkalmazások szükséges tooreturn hello hitelesítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="9d829-161">Call `AcquireTokenAsync(...)`, which accepts hello `IPlatformParameters` object and invokes hello authentication flow that's necessary tooreturn a token toohello app.</span></span>

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

    <span data-ttu-id="9d829-162">`AcquireTokenAsync(...)`első kísérlet tooreturn hello egy token kért erőforrás (hello Graph API ebben az esetben) felhasználók tooenter a hitelesítő adataikat (keresztül gyorsítótárazását, vagy frissíteni a régi jogkivonatok) értesítése nélkül.</span><span class="sxs-lookup"><span data-stu-id="9d829-162">`AcquireTokenAsync(...)` first attempts tooreturn a token for hello requested resource (hello Graph API in this case) without prompting users tooenter their credentials (via caching or refreshing old tokens).</span></span> <span data-ttu-id="9d829-163">Szükség esetén azt hello kért token beszerzése előtt felhasználók hello az Azure AD bejelentkezési lapját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9d829-163">As necessary, it shows users hello Azure AD sign-in page before acquiring hello requested token.</span></span>
4. <span data-ttu-id="9d829-164">Hello access token toohello Graph API kérést hello csatolása **engedélyezési** fejléc:</span><span class="sxs-lookup"><span data-stu-id="9d829-164">Attach hello access token toohello Graph API request in hello **Authorization** header:</span></span>

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

<span data-ttu-id="9d829-165">Ez minden, a hello `DirectorySearcher` PCL és hello app identitás kapcsolatos kódot.</span><span class="sxs-lookup"><span data-stu-id="9d829-165">That's all for hello `DirectorySearcher` PCL and hello app's identity-related code.</span></span> <span data-ttu-id="9d829-166">Most már toocall hello `SearchByAlias(...)` metódus minden egyes platform nézetekben és szükség esetén megfelelően kezeli tooadd kódját hello felhasználói felület életciklusát.</span><span class="sxs-lookup"><span data-stu-id="9d829-166">All that remains is toocall hello `SearchByAlias(...)` method in each platform's views and, where necessary, tooadd code for correctly handling hello UI lifecycle.</span></span>

### <a name="android"></a><span data-ttu-id="9d829-167">Android</span><span class="sxs-lookup"><span data-stu-id="9d829-167">Android</span></span>
1. <span data-ttu-id="9d829-168">A MainActivity.cs, adjon hozzá egy túl`SearchByAlias(...)` hello gombra kattintson kezelő:</span><span class="sxs-lookup"><span data-stu-id="9d829-168">In MainActivity.cs, add a call too`SearchByAlias(...)` in hello button click handler:</span></span>

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. <span data-ttu-id="9d829-169">Bírálja felül a hello `OnActivityResult` életciklus metódus tooforward hitelesítés átirányítja a felhasználókat hátsó toohello megfelelő módszert.</span><span class="sxs-lookup"><span data-stu-id="9d829-169">Override hello `OnActivityResult` lifecycle method tooforward any authentication redirects back toohello appropriate method.</span></span> <span data-ttu-id="9d829-170">Adal-t egy segédmetódust nyújt az Android:</span><span class="sxs-lookup"><span data-stu-id="9d829-170">ADAL provides a helper method for this in Android:</span></span>

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a><span data-ttu-id="9d829-171">Windows asztali</span><span class="sxs-lookup"><span data-stu-id="9d829-171">Windows Desktop</span></span>
<span data-ttu-id="9d829-172">A MainWindow.xaml.cs, hívás túl`SearchByAlias(...)` úgy, hogy egy `WindowInteropHelper` hello asztali `PlatformParameters` objektum:</span><span class="sxs-lookup"><span data-stu-id="9d829-172">In MainWindow.xaml.cs, make a call too`SearchByAlias(...)` by passing a `WindowInteropHelper` in hello desktop's `PlatformParameters` object:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a><span data-ttu-id="9d829-173">iOS</span><span class="sxs-lookup"><span data-stu-id="9d829-173">iOS</span></span>
<span data-ttu-id="9d829-174">A DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` objektum szükséges a hivatkozás toohello nézetvezérlő:</span><span class="sxs-lookup"><span data-stu-id="9d829-174">In DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` object takes a reference toohello View Controller:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a><span data-ttu-id="9d829-175">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="9d829-175">Windows Universal</span></span>
<span data-ttu-id="9d829-176">Az univerzális Windows-, nyissa meg a MainPage.xaml.cs, és megvalósításához hello `Search` metódust.</span><span class="sxs-lookup"><span data-stu-id="9d829-176">In Windows Universal, open MainPage.xaml.cs, and then implement hello `Search` method.</span></span> <span data-ttu-id="9d829-177">Ez a módszer egy segédmetódust a megosztott projekt tooupdate felhasználói felület szükség esetén használja.</span><span class="sxs-lookup"><span data-stu-id="9d829-177">This method uses a helper method in a shared project tooupdate UI as necessary.</span></span>

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a><span data-ttu-id="9d829-178">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d829-178">What's next</span></span>
<span data-ttu-id="9d829-179">Most már rendelkezik egy működő Xamarin-alkalmazás, amely hitelesíti a felhasználókat, és biztonságosan hívására webes API-k OAuth 2.0 különböző öt különböző platformokon.</span><span class="sxs-lookup"><span data-stu-id="9d829-179">You now have a working Xamarin app that can authenticate users and securely call web APIs by using OAuth 2.0 across five different platforms.</span></span>

<span data-ttu-id="9d829-180">Ha már nincs beállítva, feltöltve a felhasználókkal a bérlő, most már olyan hello idő toodo.</span><span class="sxs-lookup"><span data-stu-id="9d829-180">If you haven’t already populated your tenant with users, now is hello time toodo so.</span></span>

1. <span data-ttu-id="9d829-181">Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9d829-181">Run your DirectorySearcher app, and then sign in with one of hello users.</span></span>
2. <span data-ttu-id="9d829-182">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="9d829-182">Search for other users based on their UPN.</span></span>

<span data-ttu-id="9d829-183">ADAL segítségével könnyen tooincorporate közös identitás szolgáltatások hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="9d829-183">ADAL makes it easy tooincorporate common identity features into hello app.</span></span> <span data-ttu-id="9d829-184">Azt gondoskodik összes hello dirty munkahelyi meg, például a gyorsítótár kezelése, OAuth protokoll támogatása, a bejelentkezési felhasználói felület, a hello felhasználói bemutató és jogkivonatok frissítése lejárt.</span><span class="sxs-lookup"><span data-stu-id="9d829-184">It takes care of all hello dirty work for you, such as cache management, OAuth protocol support, presenting hello user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="9d829-185">Tooknow csak egy API-hívás, kell `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="9d829-185">You need tooknow only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="9d829-186">Hivatkozás, töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (a konfigurációs értékek nélkül).</span><span class="sxs-lookup"><span data-stu-id="9d829-186">For reference, download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="9d829-187">Most már továbbléphet tooadditional identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="9d829-187">You can now move on tooadditional identity scenarios.</span></span> <span data-ttu-id="9d829-188">Például [egy .NET webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="9d829-188">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
