---
title: "Ismerkedés az Azure AD a Windows Store |} Microsoft Docs"
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
ms.openlocfilehash: 6b5189dc06d7f8b0ed4426944948b904feba847e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a><span data-ttu-id="0dcad-103">Azure AD integrálása a Windows Áruházbeli alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0dcad-103">Integrate Azure AD with Windows Store apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="0dcad-104">Windows áruház 8.1 és korábbi verziójú projektek nem támogatottak a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0dcad-104">Windows Store 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="0dcad-105">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="0dcad-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="0dcad-106">Ha az alkalmazások a Windows áruházhoz, Azure Active Directory (Azure AD) segítségével egyszerű és magától értetődő, a felhasználókat az Active Directory-fiókok hitelesítésére.</span><span class="sxs-lookup"><span data-stu-id="0dcad-106">If you're developing apps for the Windows Store, Azure Active Directory (Azure AD) makes it simple and straightforward to authenticate your users with their Active Directory accounts.</span></span> <span data-ttu-id="0dcad-107">Integrálja az Azure AD-val, az alkalmazások biztonságos felhasználhat a webes API-t az Azure AD, például az Office 365 API-k vagy az Azure API által védett.</span><span class="sxs-lookup"><span data-stu-id="0dcad-107">By integrating with Azure AD, an app can securely consume any web API that's protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="0dcad-108">Windows áruház asztali alkalmazások esetén, amely a védett erőforrások eléréséhez szükséges az Azure AD az Active Directory Authentication Library (ADAL) biztosít.</span><span class="sxs-lookup"><span data-stu-id="0dcad-108">For Windows Store desktop apps that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="0dcad-109">ADAL kizárólagos célja az alkalmazásnak, hogy a hozzáférési jogkivonatok lekérésére, könnyen.</span><span class="sxs-lookup"><span data-stu-id="0dcad-109">The sole purpose of ADAL is to make it easy for the app to get access tokens.</span></span> <span data-ttu-id="0dcad-110">Annak bemutatásához, hogy milyen egyszerűen, ez a cikk bemutatja, hogyan hozhat létre a DirectorySearcher Windows Áruházbeli alkalmazások, amelyek:</span><span class="sxs-lookup"><span data-stu-id="0dcad-110">To demonstrate how easy it is, this article shows how to build a DirectorySearcher Windows Store app that:</span></span>

* <span data-ttu-id="0dcad-111">Lekérdezi az Azure AD Graph API felület meghívásakor használatával jogkivonatainak hozzáférni a [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="0dcad-111">Gets access tokens for calling the Azure AD Graph API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="0dcad-112">Megkeresi a felhasználók számára egy megadott egyszerű felhasználónév (UPN).</span><span class="sxs-lookup"><span data-stu-id="0dcad-112">Searches a directory for users with a given user principal name (UPN).</span></span>
* <span data-ttu-id="0dcad-113">Kimenő felhasználók jeleket.</span><span class="sxs-lookup"><span data-stu-id="0dcad-113">Signs users out.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="0dcad-114">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="0dcad-114">Before you get started</span></span>
* <span data-ttu-id="0dcad-115">Töltse le a [projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), vagy töltse le a [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="0dcad-115">Download the [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), or download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span></span> <span data-ttu-id="0dcad-116">Minden letöltés egy olyan Visual Studio 2015 megoldás.</span><span class="sxs-lookup"><span data-stu-id="0dcad-116">Each download is a Visual Studio 2015 solution.</span></span>
* <span data-ttu-id="0dcad-117">Akkor is, amelyben a felhasználók létrehozásához, és regisztrálja az alkalmazást az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="0dcad-117">You also need an Azure AD tenant in which to create users and register the app.</span></span> <span data-ttu-id="0dcad-118">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="0dcad-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="0dcad-119">Ha készen áll, hajtsa végre a következő három szakasz.</span><span class="sxs-lookup"><span data-stu-id="0dcad-119">When you are ready, follow the procedures in the next three sections.</span></span>

## <a name="step-1-register-the-directorysearcher-app"></a><span data-ttu-id="0dcad-120">1. lépés: A DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="0dcad-120">Step 1: Register the DirectorySearcher app</span></span>
<span data-ttu-id="0dcad-121">Ahhoz, hogy az alkalmazás a jogkivonatok lekérésére, először regisztrálja az Azure AD-bérlőben, és adja meg azt az Azure AD Graph API hozzáférési engedélyt.</span><span class="sxs-lookup"><span data-stu-id="0dcad-121">To enable the app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API.</span></span> <span data-ttu-id="0dcad-122">Ezt a következőképpen teheti meg:</span><span class="sxs-lookup"><span data-stu-id="0dcad-122">Here's how:</span></span>

1. <span data-ttu-id="0dcad-123">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0dcad-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0dcad-124">A felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="0dcad-124">On the top bar, click your account.</span></span> <span data-ttu-id="0dcad-125">Ekkor a a **Directory** listára, válassza ki az Active Directory-bérlőt, ahol az alkalmazást regisztrálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="0dcad-125">Then, under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="0dcad-126">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0dcad-126">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="0dcad-127">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="0dcad-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="0dcad-128">Kövesse a megjelenő utasításokat hozzon létre egy **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0dcad-128">Follow the prompts to create a **Native Client Application**.</span></span>
  * <span data-ttu-id="0dcad-129">**Név** az alkalmazásnak, hogy a felhasználók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0dcad-129">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="0dcad-130">**Átirányítási URI** protokollt és a karakterlánc kombinációját, amely az Azure AD token válaszok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0dcad-130">**Redirect URI** is a scheme and string combination that Azure AD uses to return token responses.</span></span> <span data-ttu-id="0dcad-131">Adja meg a helyőrző értékét most (például **http://DirectorySearcher**).</span><span class="sxs-lookup"><span data-stu-id="0dcad-131">Enter a placeholder value for now (for example, **http://DirectorySearcher**).</span></span> <span data-ttu-id="0dcad-132">Cseréli le a érték később.</span><span class="sxs-lookup"><span data-stu-id="0dcad-132">You'll replace the value later.</span></span>
6. <span data-ttu-id="0dcad-133">A regisztráció befejezését követően az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="0dcad-133">After you’ve completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="0dcad-134">A kimásolhatja az értéket a **alkalmazás** lapon, mert később szüksége.</span><span class="sxs-lookup"><span data-stu-id="0dcad-134">Copy the value on the **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="0dcad-135">Az a **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="0dcad-135">On the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="0dcad-136">Az a **Azure Active Directory** alkalmazást, jelölje be **Microsoft Graph** , az API-t.</span><span class="sxs-lookup"><span data-stu-id="0dcad-136">For the **Azure Active Directory** app, select **Microsoft Graph** as the API.</span></span>
9. <span data-ttu-id="0dcad-137">A **delegált engedélyek**, vegye fel a **hozzáférés a címtárhoz a bejelentkezett felhasználó az** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="0dcad-137">Under **Delegated Permissions**, add the **Access the directory as the signed-in user** permission.</span></span> <span data-ttu-id="0dcad-138">Így lehetővé teszi az alkalmazásnak, hogy a felhasználók számára a Graph API lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="0dcad-138">Doing so enables the app to query the Graph API for users.</span></span>

## <a name="step-2-install-and-configure-adal"></a><span data-ttu-id="0dcad-139">2. lépés: Telepítse és konfigurálja az adal-t</span><span class="sxs-lookup"><span data-stu-id="0dcad-139">Step 2: Install and configure ADAL</span></span>
<span data-ttu-id="0dcad-140">Most, hogy egy alkalmazást az Azure ad-ben, telepítse az adal-t, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="0dcad-140">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="0dcad-141">Ahhoz, hogy az adal-t az Azure AD kommunikálni, adjon neki az alkalmazás regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="0dcad-141">To enable ADAL to communicate with Azure AD, give it some information about the app registration.</span></span>

1. <span data-ttu-id="0dcad-142">Adal-t a Csomagkezelő konzol használatával adja hozzá a DirectorySearcher projekt.</span><span class="sxs-lookup"><span data-stu-id="0dcad-142">Add ADAL to the DirectorySearcher project by using the Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. <span data-ttu-id="0dcad-143">A DirectorySearcher projektben nyissa meg a MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="0dcad-143">In the DirectorySearcher project, open MainPage.xaml.cs.</span></span>
3. <span data-ttu-id="0dcad-144">Cserélje le az értékeket a **konfigurációs értékeire** régió az Azure-portálon megadott értékekkel.</span><span class="sxs-lookup"><span data-stu-id="0dcad-144">Replace the values in the **Config Values** region with the values that you entered in the Azure portal.</span></span> <span data-ttu-id="0dcad-145">A kód hivatkozik ezeket az értékeket, minden alkalommal adal-t.</span><span class="sxs-lookup"><span data-stu-id="0dcad-145">Your code refers to these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="0dcad-146">A *bérlői* a tartományt az Azure AD bérlője (például contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="0dcad-146">The *tenant* is the domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="0dcad-147">A *clientId* az ügyfél-azonosító az alkalmazás, amely a portálról kimásolt.</span><span class="sxs-lookup"><span data-stu-id="0dcad-147">The *clientId* is the client ID of the app, which you copied from the portal.</span></span>
4. <span data-ttu-id="0dcad-148">Most szeretné felderíteni a visszahívási URI a Windows Áruházbeli alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0dcad-148">You now need to discover the callback URI for your Windows Store app.</span></span> <span data-ttu-id="0dcad-149">Ez a sor állítson be egy töréspontot a `MainPage` módszert:</span><span class="sxs-lookup"><span data-stu-id="0dcad-149">Set a breakpoint on this line in the `MainPage` method:</span></span>
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. <span data-ttu-id="0dcad-150">A megoldás felépítéséhez, annak biztosítása, hogy az összes csomag-hivatkozások, a rendszer visszaállítja.</span><span class="sxs-lookup"><span data-stu-id="0dcad-150">Build the solution, making sure that all package references are restored.</span></span> <span data-ttu-id="0dcad-151">Ha hiányoznak a csomagokat, nyissa meg a NuGet-Csomagkezelőt, és állítsa vissza őket.</span><span class="sxs-lookup"><span data-stu-id="0dcad-151">If any packages are missing, open the NuGet Package Manager and restore them.</span></span>
6. <span data-ttu-id="0dcad-152">Futtassa az alkalmazást, és másolja a értékének `redirectUri` amikor a töréspont elérte-e.</span><span class="sxs-lookup"><span data-stu-id="0dcad-152">Run the app, and copy the value of `redirectUri` when the breakpoint is hit.</span></span> <span data-ttu-id="0dcad-153">Az érték hasonlóan kell kinéznie a következő:</span><span class="sxs-lookup"><span data-stu-id="0dcad-153">The value should look something like the following:</span></span>

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. <span data-ttu-id="0dcad-154">Újra be a **beállítások** lapon vegye fel az alkalmazás az Azure portálon, a **RedirectUri** az előző érték.</span><span class="sxs-lookup"><span data-stu-id="0dcad-154">Back on the **Settings** tab of the app in the Azure portal, add a **RedirectUri** with the preceding value.</span></span>  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="0dcad-155">3. lépés: Használja az adal TÁRAT jogkivonatok lekérni az Azure AD</span><span class="sxs-lookup"><span data-stu-id="0dcad-155">Step 3: Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="0dcad-156">Az alapelv ADAL mögött, hogy az alkalmazás olyan hozzáférési jogkivonatot kell, ha egyszerűen meghívja `authContext.AcquireToken(…)`, és a többi adal-t.</span><span class="sxs-lookup"><span data-stu-id="0dcad-156">The basic principle behind ADAL is that whenever the app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does the rest.</span></span>  

1. <span data-ttu-id="0dcad-157">Az alkalmazás inicializálása `AuthenticationContext`, vagyis az elsődleges osztály az adal-t.</span><span class="sxs-lookup"><span data-stu-id="0dcad-157">Initialize the app’s `AuthenticationContext`, which is the primary class of ADAL.</span></span> <span data-ttu-id="0dcad-158">Ez a művelet továbbítja az adal-t a koordináták kommunikálni az Azure AD, és mondja el, akkor jogkivonatok gyorsítótárazásának kell.</span><span class="sxs-lookup"><span data-stu-id="0dcad-158">This action passes ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. <span data-ttu-id="0dcad-159">Keresse meg a `Search(...)` metódust, ha a felhasználók kattintanak, amelyet a **keresési** az alkalmazás Kezelőfelületén gombjára.</span><span class="sxs-lookup"><span data-stu-id="0dcad-159">Locate the `Search(...)` method, which is invoked when users click the **Search** button on the app's UI.</span></span> <span data-ttu-id="0dcad-160">Ez a módszer az Azure AD Graph API lekérdezéshez get kérés teszi a felhasználók számára, amelyek egyszerű felhasználónév a megadott keresési kifejezés kezdődik.</span><span class="sxs-lookup"><span data-stu-id="0dcad-160">This method makes a get request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span> <span data-ttu-id="0dcad-161">A Graph API lekérdezéséhez közé tartoznak az olyan hozzáférési jogkivonatot a kérelemben szereplő **engedélyezési** fejléc.</span><span class="sxs-lookup"><span data-stu-id="0dcad-161">To query the Graph API, include an access token in the request's **Authorization** header.</span></span> <span data-ttu-id="0dcad-162">Ez az adal-t honnan.</span><span class="sxs-lookup"><span data-stu-id="0dcad-162">This is where ADAL comes in.</span></span>

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
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    <span data-ttu-id="0dcad-163">Ha kér az alkalmazás jogkivonat meghívásával `AcquireTokenAsync(...)`, adal-t próbálja elküldeni a jogkivonatot a felhasználói hitelesítő adatok kérése nélkül.</span><span class="sxs-lookup"><span data-stu-id="0dcad-163">When the app requests a token by calling `AcquireTokenAsync(...)`, ADAL attempts to return a token without asking the user for credentials.</span></span> <span data-ttu-id="0dcad-164">ADAL határozza meg, hogy a felhasználó bejelentkezhet a szolgáltatáshitelesítést egy token kell-e, ha azt egy bejelentkezési párbeszédpanel jelenik meg, a felhasználó hitelesítő adatait gyűjti és egy jogkivonatot ad vissza, sikeres hitelesítést követően.</span><span class="sxs-lookup"><span data-stu-id="0dcad-164">If ADAL determines that the user needs to sign in to get a token, it displays a sign-in dialog box, collects the user's credentials, and returns a token after authentication succeeds.</span></span> <span data-ttu-id="0dcad-165">Ha adal-t nem lehet visszaadni a jogkivonat bármilyen okból a *AuthenticationResult* állapot: Hiba történt.</span><span class="sxs-lookup"><span data-stu-id="0dcad-165">If ADAL is unable to return a token for any reason, the *AuthenticationResult* status is an error.</span></span>
3. <span data-ttu-id="0dcad-166">Most meg kell használni a kapott hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="0dcad-166">Now it's time to use the access token you just acquired.</span></span> <span data-ttu-id="0dcad-167">A is a `Search(...)` módszer, csatolni a tokent a Graph API get kérést a **engedélyezési** fejléc:</span><span class="sxs-lookup"><span data-stu-id="0dcad-167">Also in the `Search(...)` method, attach the token to the Graph API get request in the **Authorization** header:</span></span>

    ```C#
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. <span data-ttu-id="0dcad-168">Használhatja a `AuthenticationResult` objektum információt szeretne megjeleníteni a felhasználó az alkalmazásban, például a felhasználói azonosító:</span><span class="sxs-lookup"><span data-stu-id="0dcad-168">You can use the `AuthenticationResult` object to display information about the user in the app, such as the user's ID:</span></span>

    ```C#
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. <span data-ttu-id="0dcad-169">ADAL segítségével is bejelentkezhetnek a felhasználók az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="0dcad-169">You can also use ADAL to sign users out of the app.</span></span> <span data-ttu-id="0dcad-170">Amikor a felhasználó kattint a **Kijelentkezés** gombra, győződjön meg arról, hogy a következő hívást `AcquireTokenAsync(...)` jeleníti meg, bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="0dcad-170">When the user clicks the **Sign Out** button, ensure that the next call to `AcquireTokenAsync(...)` shows a sign-in view.</span></span> <span data-ttu-id="0dcad-171">Az ADAL Ez a művelet nem egyszerű módon, ha a token gyorsítótár kiürítése:</span><span class="sxs-lookup"><span data-stu-id="0dcad-171">With ADAL, this action is as easy as clearing the token cache:</span></span>

    ```C#
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a><span data-ttu-id="0dcad-172">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="0dcad-172">What's next</span></span>
<span data-ttu-id="0dcad-173">Most már rendelkezik egy működő, amely hitelesíti a felhasználókat, biztonságos webes API-k OAuth 2.0 használatával hívható, és a felhasználóval kapcsolatos alapvető információk a Windows áruház egy alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="0dcad-173">You now have a working Windows Store app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about the user.</span></span>

<span data-ttu-id="0dcad-174">Ha már nincs beállítva, feltöltve a felhasználókkal a bérlő, most az ehhez idő.</span><span class="sxs-lookup"><span data-stu-id="0dcad-174">If you haven’t already populated your tenant with users, now is the time to do so.</span></span>
1. <span data-ttu-id="0dcad-175">A DirectorySearcher alkalmazás futtatását, és jelentkezzen be a felhasználók közül.</span><span class="sxs-lookup"><span data-stu-id="0dcad-175">Run your DirectorySearcher app, and then sign in with one of the users.</span></span>
2. <span data-ttu-id="0dcad-176">Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="0dcad-176">Search for other users based on their UPN.</span></span>
3. <span data-ttu-id="0dcad-177">Zárja be az alkalmazást, és újra futtathatja.</span><span class="sxs-lookup"><span data-stu-id="0dcad-177">Close the app, and rerun it.</span></span> <span data-ttu-id="0dcad-178">Figyelje meg, hogyan az ügyfél helye változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="0dcad-178">Notice how the user’s session remains intact.</span></span>
4. <span data-ttu-id="0dcad-179">Kattintson a jobb gombbal az alsó sáv megjelenítése jelentkezzen ki, majd jelentkezzen be egy másik felhasználó.</span><span class="sxs-lookup"><span data-stu-id="0dcad-179">Sign out by right-clicking to display the bottom bar, and then sign back in as another user.</span></span>

<span data-ttu-id="0dcad-180">ADAL megkönnyíti a közös identitás funkciók ilyen beépítse az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0dcad-180">ADAL makes it easy to incorporate all these common identity features into the app.</span></span> <span data-ttu-id="0dcad-181">Azt gondoskodik a dirty munkát meg, például a gyorsítótár kezelése OAuth protokoll támogatása, a felhasználó egy bejelentkezési felhasználói felület, a bemutató, és a jogkivonatok frissítése lejárt.</span><span class="sxs-lookup"><span data-stu-id="0dcad-181">It takes care of all the dirty work for you, such as cache management, OAuth protocol support, presenting the user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="0dcad-182">Csak egyetlen API-hívással, ismernie kell `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="0dcad-182">You need to know only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="0dcad-183">Hivatkozás, töltse le a [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (a konfigurációs értékek nélkül).</span><span class="sxs-lookup"><span data-stu-id="0dcad-183">For reference, download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="0dcad-184">Most már továbbléphet további identitás forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="0dcad-184">You can now move on to additional identity scenarios.</span></span> <span data-ttu-id="0dcad-185">Például [egy .NET webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="0dcad-185">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
