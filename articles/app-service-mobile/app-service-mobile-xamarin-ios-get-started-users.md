---
title: "Ismerkedés a Mobile Apps Xamarin IOS-hitelesítés"
description: "Útmutató a Mobile Apps segítségével hitelesíti a felhasználókat identitás-szolgáltatóktól, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos a Xamarin IOS-es alkalmazás."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: glenga
ms.openlocfilehash: 454b2df5a9bf8cfba93befea54370957ab044d95
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinios-app"></a><span data-ttu-id="d38d9-103">Bővítse Xamarin.iOS-alkalmazását hitelesítési funkcióval</span><span class="sxs-lookup"><span data-stu-id="d38d9-103">Add authentication to your Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="d38d9-104">Ez a témakör bemutatja, hogyan hitelesítheti egy App Service Mobile Apps az ügyfélalkalmazás felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="d38d9-104">This topic shows you how to authenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="d38d9-105">Ebben az oktatóanyagban hozzáadja hitelesítési gyorsútmutató-projekt Xamarin.iOS App Service által támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="d38d9-105">In this tutorial, you add authentication to the Xamarin.iOS quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="d38d9-106">Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps által, a felhasználói azonosító érték jelenik meg, és fogja tudni elérni a korlátozott tábla adatai.</span><span class="sxs-lookup"><span data-stu-id="d38d9-106">After being successfully authenticated and authorized by your Mobile App, the user ID value is displayed and you will be able to access restricted table data.</span></span>

<span data-ttu-id="d38d9-107">Először el kell végeznie az oktatóanyag [Xamarin.iOS-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="d38d9-107">You must first complete the tutorial [Create a Xamarin.iOS app].</span></span> <span data-ttu-id="d38d9-108">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a hitelesítési kiterjesztés csomag fel a projektbe.</span><span class="sxs-lookup"><span data-stu-id="d38d9-108">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="d38d9-109">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="d38d9-109">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="d38d9-110">Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d38d9-110">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><span data-ttu-id="d38d9-111">Az alkalmazás hozzáadása az engedélyezett külső átirányítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="d38d9-111">Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="d38d9-112">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="d38d9-112">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="d38d9-113">Ez lehetővé teszi a hitelesítési rendszer visszairányítja az alkalmazás a hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="d38d9-113">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="d38d9-114">Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="d38d9-114">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="d38d9-115">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d38d9-115">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="d38d9-116">A mobilalkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d38d9-116">It should be unique to your mobile application.</span></span> <span data-ttu-id="d38d9-117">A kiszolgáló oldalán engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="d38d9-117">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="d38d9-118">Válassza ki az App Service az [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="d38d9-118">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="d38d9-119">Kattintson a **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="d38d9-119">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="d38d9-120">Az a **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="d38d9-120">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="d38d9-121">A **url_scheme_of_your_app** ezt a karakterláncot a rendszer az URL-séma, a mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d38d9-121">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="d38d9-122">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="d38d9-122">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="d38d9-123">Meg kell győződnie, jegyezze fel a karakterláncot, amely úgy dönt, mivel úgy, hogy az URL-séma több helyen a mobilalkalmazás kódot kell.</span><span class="sxs-lookup"><span data-stu-id="d38d9-123">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="d38d9-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d38d9-124">Click **OK**.</span></span>

5. <span data-ttu-id="d38d9-125">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d38d9-125">Click **Save**.</span></span>

## <a name="restrict-permissions-to-authenticated-users"></a><span data-ttu-id="d38d9-126">A hitelesített felhasználókhoz jogosultságok korlátozása</span><span class="sxs-lookup"><span data-stu-id="d38d9-126">Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="d38d9-127">&nbsp;&nbsp;4.</span><span class="sxs-lookup"><span data-stu-id="d38d9-127">&nbsp;&nbsp;4.</span></span> <span data-ttu-id="d38d9-128">A Visual Studio és Xamarin Studio futtatható az ügyfélprojekt egy eszközt vagy emulátort.</span><span class="sxs-lookup"><span data-stu-id="d38d9-128">In Visual Studio or Xamarin Studio, run the client project on a device or emulator.</span></span> <span data-ttu-id="d38d9-129">Győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="d38d9-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="d38d9-130">A problémát a hibakereső konzoljába naplózza.</span><span class="sxs-lookup"><span data-stu-id="d38d9-130">The failure is logged to the console of the debugger.</span></span> <span data-ttu-id="d38d9-131">Ezért a Visual Studio kell megjelennie a hiba a kimeneti ablakban.</span><span class="sxs-lookup"><span data-stu-id="d38d9-131">So in Visual Studio, you should see the failure in the output window.</span></span>

<span data-ttu-id="d38d9-132">&nbsp;&nbsp;A jogosulatlan hiba akkor fordul elő, mert az alkalmazás megpróbál hozzáférni a mobil-háttéralkalmazást, nem hitelesített felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="d38d9-132">&nbsp;&nbsp;This unauthorized failure happens because the app attempts to access your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="d38d9-133">A *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="d38d9-133">The *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="d38d9-134">A következő később frissíteni az ügyfélalkalmazás az erőforrásokat a Mobile Apps-háttéralkalmazás a hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="d38d9-134">Next, you will update the client app to request resources from the Mobile App backend with an authenticated user.</span></span>

## <a name="add-authentication-to-the-app"></a><span data-ttu-id="d38d9-135">Hitelesítés hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="d38d9-135">Add authentication to the app</span></span>
<span data-ttu-id="d38d9-136">Ebben a szakaszban az alkalmazás egy bejelentkezési képernyőt megjelenítendő adatok megjelenítése előtt fog módosítani.</span><span class="sxs-lookup"><span data-stu-id="d38d9-136">In this section, you will modify the app to display a login screen before displaying data.</span></span> <span data-ttu-id="d38d9-137">Az alkalmazás indításakor nem nem csatlakoznak az App Service, és nem jelenik meg az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d38d9-137">When the app starts, it will not not connect to your App Service and will not display any data.</span></span> <span data-ttu-id="d38d9-138">Az első alkalommal, amikor a felhasználó hajtja végre a frissítési kézmozdulat a bejelentkezési képernyőn megjelenik; sikeres bejelentkezés után a teendőlista elemeinek listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d38d9-138">After the first time that the user performs the refresh gesture, the login screen will appear; after successful login the list of todo items will be displayed.</span></span>

1. <span data-ttu-id="d38d9-139">Az ügyfél projektben nyissa meg a fájl **QSTodoService.cs** és adja hozzá a következő utasítás használatával és `MobileServiceUser` az elérő QSTodoService osztályra:</span><span class="sxs-lookup"><span data-stu-id="d38d9-139">In the client project, open the file **QSTodoService.cs** and add the following using statement and `MobileServiceUser` with accessor to the QSTodoService class:</span></span>
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. <span data-ttu-id="d38d9-140">Adja hozzá nevű új metódust **hitelesítés** való **QSTodoService** a következő definícióját:</span><span class="sxs-lookup"><span data-stu-id="d38d9-140">Add new method named **Authenticate** to **QSTodoService** with the following definition:</span></span>

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                AppDelegate.ResumeWithURL = url => url.Scheme == "zumoe2etestapp" && client.ResumeWithURL(url);
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] <span data-ttu-id="d38d9-141">Ha használja az identitásszolgáltató eltérő a Facebook, módosítsa az értéket, átadott **LoginAsync** fent a következőhöz: _MicrosoftAccount_, _Twitter_,  _Google_, vagy _WindowsAzureActiveDirectory_.</span><span class="sxs-lookup"><span data-stu-id="d38d9-141">If you are using an identity provider other than a Facebook, change the value passed to **LoginAsync** above to one of the following: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.</span></span>

3. <span data-ttu-id="d38d9-142">Nyissa meg **QSTodoListViewController.cs**.</span><span class="sxs-lookup"><span data-stu-id="d38d9-142">Open **QSTodoListViewController.cs**.</span></span> <span data-ttu-id="d38d9-143">Módosítsa a metódus definícióját **ViewDidLoad** hívása eltávolítása **RefreshAsync()** vége:</span><span class="sxs-lookup"><span data-stu-id="d38d9-143">Modify the method definition of **ViewDidLoad** removing the call to **RefreshAsync()** near the end:</span></span>
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out the call to RefreshAsync
            // await RefreshAsync();
        }
4. <span data-ttu-id="d38d9-144">A módszer módosításához **RefreshAsync** – Ha a **felhasználói** tulajdonsága null értékű.</span><span class="sxs-lookup"><span data-stu-id="d38d9-144">Modify the method **RefreshAsync** to authenticate if the **User** property is null.</span></span> <span data-ttu-id="d38d9-145">A metódusdefiníciót tetején adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d38d9-145">Add the following code at the top of the method definition:</span></span>
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. <span data-ttu-id="d38d9-146">Nyissa meg **AppDelegate.cs**, adja hozzá a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="d38d9-146">Open **AppDelegate.cs**, add the following method:</span></span>

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. <span data-ttu-id="d38d9-147">Nyissa meg **Info.plist** fájlt, keresse meg **URL-cím típusú** a a **speciális** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d38d9-147">Open **Info.plist** file, navigate to **URL Types** in the **Advanced** section.</span></span> <span data-ttu-id="d38d9-148">Mostantól konfigurálhatja a **azonosító** és a **URL-sémákat** a URL-típust, és kattintson a **URL-típus hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d38d9-148">Now configure the **Identifier** and the **URL Schemes** of your URL Type and click **Add URL Type**.</span></span> <span data-ttu-id="d38d9-149">**URL-sémákat** meg kell egyeznie a {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="d38d9-149">**URL Schemes** should be the same as your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="d38d9-150">A Visual Studio és Xamarin Studio csatlakozott a Xamarin létrehozása a Mac, futtassa a célcsoport-kezelési egy eszközt vagy emulátort ügyfél-projektet.</span><span class="sxs-lookup"><span data-stu-id="d38d9-150">In Visual Studio or Xamarin Studio connected to your Xamarin Build Host on your Mac, run the client project targeting a device or emulator.</span></span> <span data-ttu-id="d38d9-151">Győződjön meg arról, hogy az alkalmazás nem jelennek meg adatok.</span><span class="sxs-lookup"><span data-stu-id="d38d9-151">Verify that the app displays no data.</span></span>
   
    <span data-ttu-id="d38d9-152">Hajtsa végre a frissítési kézmozdulat modulba húzza le az elemeket, és emiatt a bejelentkezési képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d38d9-152">Perform the refresh gesture by pulling down the list of items, which will cause the login screen to appear.</span></span> <span data-ttu-id="d38d9-153">Miután sikeresen megadta az érvényes hitelesítő adatokat, az alkalmazás a teendőlista elemeinek listáját jeleníti meg, és frissíthetik az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d38d9-153">Once you have successfully entered valid credentials, the app will display the list of todo items, and you can make updates to the data.</span></span>

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
<span data-ttu-id="d38d9-154">[Xamarin.iOS-alkalmazás létrehozása]: app-service-mobile-xamarin-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="d38d9-154">[Create a Xamarin.iOS app]: app-service-mobile-xamarin-ios-get-started.md</span></span>