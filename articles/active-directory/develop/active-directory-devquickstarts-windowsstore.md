---
title: "aaaAzure AD a Windows Store bevezetés |} Microsoft Docs"
description: "Build Windows Áruházbeli alkalmazások, amelyek a bejelentkezés az Azure AD integrálása, meghívják a Azure AD API-k OAuth protokollt használó védett."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1d12c7b928bc0e94fb823f8db4a09ff416205e2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a><span data-ttu-id="e9a86-103">Azure AD integrálása a Windows Áruházbeli alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e9a86-103">Integrate Azure AD with Windows Store apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="e9a86-104">Windows áruház 8.1 és korábbi verziójú projektek nem támogatottak a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e9a86-104">Windows Store 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="e9a86-105">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="e9a86-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="e9a86-106">Ha az hello Windows áruház-alkalmazások, Azure Active Directory (Azure AD) segítségével egyszerű és magától értetődő tooauthenticate a felhasználókat az Active Directory-fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e9a86-106">If you're developing apps for hello Windows Store, Azure Active Directory (Azure AD) makes it simple and straightforward tooauthenticate your users with their Active Directory accounts.</span></span> <span data-ttu-id="e9a86-107">Integrálja az Azure AD-val, az alkalmazások biztonságos felhasználhat a webes API-t az Azure AD, például az Office 365 API-k hello vagy hello Azure API által védett.</span><span class="sxs-lookup"><span data-stu-id="e9a86-107">By integrating with Azure AD, an app can securely consume any web API that's protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="e9a86-108">A Windows áruház asztali alkalmazások, amelyeknek tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library (ADAL) biztosít.</span><span class="sxs-lookup"><span data-stu-id="e9a86-108">For Windows Store desktop apps that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="e9a86-109">hello kizárólagos ADAL célja toomake megkönnyítik hello app tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="e9a86-109">hello sole purpose of ADAL is toomake it easy for hello app tooget access tokens.</span></span> <span data-ttu-id="e9a86-110">toodemonstrate milyen egyszerűen, ez a cikk bemutatja, hogyan toobuild egy DirectorySearcher a Windows Áruházbeli alkalmazást, amely:</span><span class="sxs-lookup"><span data-stu-id="e9a86-110">toodemonstrate how easy it is, this article shows how toobuild a DirectorySearcher Windows Store app that:</span></span>

* <span data-ttu-id="e9a86-111">Lekérdezi hozzáférési jogkivonatainak hello Azure AD Graph API felület meghívásakor hello segítségével [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9a86-111">Gets access tokens for calling hello Azure AD Graph API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="e9a86-112">Megkeresi a felhasználók számára egy megadott egyszerű felhasználónév (UPN).</span><span class="sxs-lookup"><span data-stu-id="e9a86-112">Searches a directory for users with a given user principal name (UPN).</span></span>
* <span data-ttu-id="e9a86-113">Kimenő felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="e9a86-113">Signs users out.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="e9a86-114">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="e9a86-114">Before you get started</span></span>
* <span data-ttu-id="e9a86-115">Töltse le a hello [projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), vagy töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e9a86-115">Download hello [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), or download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span></span> <span data-ttu-id="e9a86-116">Minden letöltés egy olyan Visual Studio 2015 megoldás.</span><span class="sxs-lookup"><span data-stu-id="e9a86-116">Each download is a Visual Studio 2015 solution.</span></span>
* <span data-ttu-id="e9a86-117">Az Azure AD-bérlő toocreate felhasználók és a nyilvántartás hello alkalmazást is kell.</span><span class="sxs-lookup"><span data-stu-id="e9a86-117">You also need an Azure AD tenant in which toocreate users and register hello app.</span></span> <span data-ttu-id="e9a86-118">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="e9a86-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="e9a86-119">Ha készen áll, hello eljárásokat hajtsa végre a következő három szakasz hello.</span><span class="sxs-lookup"><span data-stu-id="e9a86-119">When you are ready, follow hello procedures in hello next three sections.</span></span>

## <a name="step-1-register-hello-directorysearcher-app"></a><span data-ttu-id="e9a86-120">1. lépés: Hello DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e9a86-120">Step 1: Register hello DirectorySearcher app</span></span>
<span data-ttu-id="e9a86-121">tooenable hello alkalmazási tooget jogkivonatok, először tooregister azt az Azure ad bérlői, és adja meg az engedélyt tooaccess hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="e9a86-121">tooenable hello app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API.</span></span> <span data-ttu-id="e9a86-122">Ezt a következőképpen teheti meg:</span><span class="sxs-lookup"><span data-stu-id="e9a86-122">Here's how:</span></span>

1. <span data-ttu-id="e9a86-123">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e9a86-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e9a86-124">Hello felső sávon kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="e9a86-124">On hello top bar, click your account.</span></span> <span data-ttu-id="e9a86-125">Ekkor a hello **Directory** listán, válassza hello tooregister hello alkalmazás kívánt Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="e9a86-125">Then, under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="e9a86-126">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e9a86-126">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="e9a86-127">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e9a86-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="e9a86-128">Kövesse az utasításokat toocreate hello egy **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e9a86-128">Follow hello prompts toocreate a **Native Client Application**.</span></span>
  * <span data-ttu-id="e9a86-129">**Név** hello app toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="e9a86-129">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="e9a86-130">**Átirányítási URI** , hogy az Azure AD által használt tooreturn token válaszok protokollt és a karakterlánc kombinációját.</span><span class="sxs-lookup"><span data-stu-id="e9a86-130">**Redirect URI** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span> <span data-ttu-id="e9a86-131">Adja meg a helyőrző értékét most (például **http://DirectorySearcher**).</span><span class="sxs-lookup"><span data-stu-id="e9a86-131">Enter a placeholder value for now (for example, **http://DirectorySearcher**).</span></span> <span data-ttu-id="e9a86-132">Cseréli le hello érték később.</span><span class="sxs-lookup"><span data-stu-id="e9a86-132">You'll replace hello value later.</span></span>
6. <span data-ttu-id="e9a86-133">Hello regisztráció befejezését követően az Azure AD hozzárendel hello app egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e9a86-133">After you’ve completed hello registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="e9a86-134">Másolja a hello hello értékét **alkalmazás** lapon, mert később szüksége.</span><span class="sxs-lookup"><span data-stu-id="e9a86-134">Copy hello value on hello **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="e9a86-135">A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e9a86-135">On hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="e9a86-136">A hello **Azure Active Directory** alkalmazást, jelölje be **Microsoft Graph** , hello API.</span><span class="sxs-lookup"><span data-stu-id="e9a86-136">For hello **Azure Active Directory** app, select **Microsoft Graph** as hello API.</span></span>
9. <span data-ttu-id="e9a86-137">A **delegált engedélyek**, adja hozzá a hello **hozzáférés hello a címtárhoz hello bejelentkezett felhasználó** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="e9a86-137">Under **Delegated Permissions**, add hello **Access hello directory as hello signed-in user** permission.</span></span> <span data-ttu-id="e9a86-138">Ezzel engedélyezi a hello app tooquery hello Graph API a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="e9a86-138">Doing so enables hello app tooquery hello Graph API for users.</span></span>

## <a name="step-2-install-and-configure-adal"></a><span data-ttu-id="e9a86-139">2. lépés: Telepítse és konfigurálja az adal-t</span><span class="sxs-lookup"><span data-stu-id="e9a86-139">Step 2: Install and configure ADAL</span></span>
<span data-ttu-id="e9a86-140">Most, hogy egy alkalmazást az Azure ad-ben, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="e9a86-140">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="e9a86-141">tooenable ADAL toocommunicate az Azure ad-vel, adjon neki hello app regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="e9a86-141">tooenable ADAL toocommunicate with Azure AD, give it some information about hello app registration.</span></span>

1. <span data-ttu-id="e9a86-142">Adja hozzá az ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="e9a86-142">Add ADAL toohello DirectorySearcher project by using hello Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. <span data-ttu-id="e9a86-143">Hello DirectorySearcher projektben nyissa meg a MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="e9a86-143">In hello DirectorySearcher project, open MainPage.xaml.cs.</span></span>
3. <span data-ttu-id="e9a86-144">Cserélje le a hello hello értékek **konfigurációs értékeire** régió hello Azure-portálon megadott hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="e9a86-144">Replace hello values in hello **Config Values** region with hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="e9a86-145">Kódja toothese értékek hivatkozik, amikor az adal-t használ.</span><span class="sxs-lookup"><span data-stu-id="e9a86-145">Your code refers toothese values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="e9a86-146">Hello *bérlői* hello tartománya, akkor az Azure AD bérlője (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="e9a86-146">hello *tenant* is hello domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="e9a86-147">Hello *clientId* hello alkalmazás, amely hello portálról másolt hello ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="e9a86-147">hello *clientId* is hello client ID of hello app, which you copied from hello portal.</span></span>
4. <span data-ttu-id="e9a86-148">A Windows Áruházbeli alkalmazással most kell toodiscover hello visszahívási URI.</span><span class="sxs-lookup"><span data-stu-id="e9a86-148">You now need toodiscover hello callback URI for your Windows Store app.</span></span> <span data-ttu-id="e9a86-149">Állítson be egy töréspontot a sort a hello `MainPage` módszert:</span><span class="sxs-lookup"><span data-stu-id="e9a86-149">Set a breakpoint on this line in hello `MainPage` method:</span></span>
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. <span data-ttu-id="e9a86-150">Hello megoldás, meggyőződött arról, hogy, hogy az összes csomaghivatkozásokhoz visszaállnak kiépítését.</span><span class="sxs-lookup"><span data-stu-id="e9a86-150">Build hello solution, making sure that all package references are restored.</span></span> <span data-ttu-id="e9a86-151">Ha hiányoznak a csomagokat, nyissa meg a NuGet-Csomagkezelő hello, és állítsa vissza őket.</span><span class="sxs-lookup"><span data-stu-id="e9a86-151">If any packages are missing, open hello NuGet Package Manager and restore them.</span></span>
6. <span data-ttu-id="e9a86-152">Hello alkalmazás futtatását, és másolja a hello értékének `redirectUri` amikor hello töréspont elérte-e.</span><span class="sxs-lookup"><span data-stu-id="e9a86-152">Run hello app, and copy hello value of `redirectUri` when hello breakpoint is hit.</span></span> <span data-ttu-id="e9a86-153">hello érték hasonlóan kell kinéznie hello következő:</span><span class="sxs-lookup"><span data-stu-id="e9a86-153">hello value should look something like hello following:</span></span>

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. <span data-ttu-id="e9a86-154">Vissza a hello **beállítások** hello hello Azure-portál alkalmazás lapon vegye fel a **RedirectUri** az előző érték hello.</span><span class="sxs-lookup"><span data-stu-id="e9a86-154">Back on hello **Settings** tab of hello app in hello Azure portal, add a **RedirectUri** with hello preceding value.</span></span>  

## <a name="step-3-use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="e9a86-155">3. lépés: Használja ADAL tooget jogkivonatok az Azure AD-ből</span><span class="sxs-lookup"><span data-stu-id="e9a86-155">Step 3: Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="e9a86-156">hello alapelv ADAL mögött, hogy amikor hello alkalmazásnak kell olyan hozzáférési jogkivonatot, egyszerűen meghívja `authContext.AcquireToken(…)`, és ADAL rest hello.</span><span class="sxs-lookup"><span data-stu-id="e9a86-156">hello basic principle behind ADAL is that whenever hello app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does hello rest.</span></span>  

1. <span data-ttu-id="e9a86-157">Hello alkalmazás inicializálása `AuthenticationContext`, vagyis hello elsődleges osztály az adal-t.</span><span class="sxs-lookup"><span data-stu-id="e9a86-157">Initialize hello app’s `AuthenticationContext`, which is hello primary class of ADAL.</span></span> <span data-ttu-id="e9a86-158">Ez a művelet továbbítja az Azure ad-val toocommunicate kell, és mondja el hogyan ADAL hello koordináták toocache jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="e9a86-158">This action passes ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. <span data-ttu-id="e9a86-159">Keresse meg a hello `Search(...)` metódus, amely esetén a felhasználók kattintanak, hello **keresési** hello alkalmazás Kezelőfelületén gombjára.</span><span class="sxs-lookup"><span data-stu-id="e9a86-159">Locate hello `Search(...)` method, which is invoked when users click hello **Search** button on hello app's UI.</span></span> <span data-ttu-id="e9a86-160">Ez a módszer egy get kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="e9a86-160">This method makes a get request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span> <span data-ttu-id="e9a86-161">tooquery hello Graph API-t tartalmaz olyan hozzáférési jogkivonatot hello kérelem **engedélyezési** fejléc.</span><span class="sxs-lookup"><span data-stu-id="e9a86-161">tooquery hello Graph API, include an access token in hello request's **Authorization** header.</span></span> <span data-ttu-id="e9a86-162">Ez az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="e9a86-162">This is where ADAL comes in.</span></span>

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    <span data-ttu-id="e9a86-163">Ha hello app kísérel meg jogkivonat meghívásával `AcquireTokenAsync(...)`, ADAL tooreturn jogkivonat kísérletek hello felhasználói hitelesítő adatok kérése nélkül.</span><span class="sxs-lookup"><span data-stu-id="e9a86-163">When hello app requests a token by calling `AcquireTokenAsync(...)`, ADAL attempts tooreturn a token without asking hello user for credentials.</span></span> <span data-ttu-id="e9a86-164">Ha ADAL hello felhasználó toosign tooget jogkivonatot az kell, azt bejelentkezési párbeszédpanelt jelenít meg, hello felhasználói hitelesítő adatokat gyűjt, és egy jogkivonatot ad vissza, sikeres hitelesítést követően.</span><span class="sxs-lookup"><span data-stu-id="e9a86-164">If ADAL determines that hello user needs toosign in tooget a token, it displays a sign-in dialog box, collects hello user's credentials, and returns a token after authentication succeeds.</span></span> <span data-ttu-id="e9a86-165">Ha az adal TÁRAT nem tooreturn jogkivonat bármilyen okból, hello *AuthenticationResult* állapot: Hiba történt.</span><span class="sxs-lookup"><span data-stu-id="e9a86-165">If ADAL is unable tooreturn a token for any reason, hello *AuthenticationResult* status is an error.</span></span>
3. <span data-ttu-id="e9a86-166">Most már idő toouse hello hozzáférési jogkivonatot kapott.</span><span class="sxs-lookup"><span data-stu-id="e9a86-166">Now it's time toouse hello access token you just acquired.</span></span> <span data-ttu-id="e9a86-167">Is a hello `Search(...)` módszer, hello token toohello Graph API hello beolvasni a kérelmet csatolása **engedélyezési** fejléc:</span><span class="sxs-lookup"><span data-stu-id="e9a86-167">Also in hello `Search(...)` method, attach hello token toohello Graph API get request in hello **Authorization** header:</span></span>

    ```C#
    // Add hello access token toohello Authorization header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. <span data-ttu-id="e9a86-168">Használhatja a hello `AuthenticationResult` objektum hello felhasználó hello alkalmazás, például az hello felhasználói Azonosítók toodisplay adatait:</span><span class="sxs-lookup"><span data-stu-id="e9a86-168">You can use hello `AuthenticationResult` object toodisplay information about hello user in hello app, such as hello user's ID:</span></span>

    ```C#
    // Update hello page UI toorepresent hello signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. <span data-ttu-id="e9a86-169">ADAL toosign felhasználók hello alkalmazásból is használható.</span><span class="sxs-lookup"><span data-stu-id="e9a86-169">You can also use ADAL toosign users out of hello app.</span></span> <span data-ttu-id="e9a86-170">Ha a hello kattintás hello **Kijelentkezés** gombra, győződjön meg arról, hogy hello következő hívás túl`AcquireTokenAsync(...)` jeleníti meg, bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="e9a86-170">When hello user clicks hello **Sign Out** button, ensure that hello next call too`AcquireTokenAsync(...)` shows a sign-in view.</span></span> <span data-ttu-id="e9a86-171">Az ADAL Ez a művelet nem egyszerű módon, ha hello token gyorsítótár kiürítése:</span><span class="sxs-lookup"><span data-stu-id="e9a86-171">With ADAL, this action is as easy as clearing hello token cache:</span></span>

    ```C#
    private void SignOut()
    {
        // Clear session state from hello token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a><span data-ttu-id="e9a86-172">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9a86-172">What's next</span></span>
<span data-ttu-id="e9a86-173">Most már rendelkezik egy működő Windows Áruházbeli alkalmazást, amely hitelesíti a felhasználókat, biztonságos webes API-k OAuth 2.0 használatával hívható, és alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e9a86-173">You now have a working Windows Store app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about hello user.</span></span>

<span data-ttu-id="e9a86-174">Ha már nincs beállítva, feltöltve a felhasználókkal a bérlő, most már olyan hello idő toodo.</span><span class="sxs-lookup"><span data-stu-id="e9a86-174">If you haven’t already populated your tenant with users, now is hello time toodo so.</span></span>
1. <span data-ttu-id="e9a86-175">Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e9a86-175">Run your DirectorySearcher app, and then sign in with one of hello users.</span></span>
2. <span data-ttu-id="e9a86-176">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="e9a86-176">Search for other users based on their UPN.</span></span>
3. <span data-ttu-id="e9a86-177">Hello-alkalmazások bezárása, és újra futtathatja.</span><span class="sxs-lookup"><span data-stu-id="e9a86-177">Close hello app, and rerun it.</span></span> <span data-ttu-id="e9a86-178">Figyelje meg, hogyan hello felhasználói munkamenet nem módosulnak.</span><span class="sxs-lookup"><span data-stu-id="e9a86-178">Notice how hello user’s session remains intact.</span></span>
4. <span data-ttu-id="e9a86-179">Kattintson a jobb gombbal a toodisplay hello alsó sáv jelentkezzen ki, majd jelentkezzen be egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e9a86-179">Sign out by right-clicking toodisplay hello bottom bar, and then sign back in as another user.</span></span>

<span data-ttu-id="e9a86-180">ADAL teszi, hogy könnyen tooincorporate közös identitás funkciók ilyen hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e9a86-180">ADAL makes it easy tooincorporate all these common identity features into hello app.</span></span> <span data-ttu-id="e9a86-181">Azt gondoskodik összes hello dirty munkahelyi meg, például a gyorsítótár kezelése, OAuth protokoll támogatása, a bejelentkezési felhasználói felület, a hello felhasználói bemutató és jogkivonatok frissítése lejárt.</span><span class="sxs-lookup"><span data-stu-id="e9a86-181">It takes care of all hello dirty work for you, such as cache management, OAuth protocol support, presenting hello user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="e9a86-182">Tooknow csak egy API-hívás, kell `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="e9a86-182">You need tooknow only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="e9a86-183">Hivatkozás, töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (a konfigurációs értékek nélkül).</span><span class="sxs-lookup"><span data-stu-id="e9a86-183">For reference, download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="e9a86-184">Most már továbbléphet tooadditional identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="e9a86-184">You can now move on tooadditional identity scenarios.</span></span> <span data-ttu-id="e9a86-185">Például [egy .NET webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e9a86-185">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
