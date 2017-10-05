---
title: "Ismerkedés a Mobile Apps a Xamarin Forms app hitelesítéssel |} Microsoft Docs"
description: "Megtudhatja, hogyan hitelesíti a felhasználókat a Xamarin Forms alkalmazás Identitásszolgáltatók, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos, a Mobile Apps segítségével."
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: syntaxc4
editor: 
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: panarasi
ms.openlocfilehash: 9e14e95793bcc81ad46783fd50ba223eec4ea360
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a><span data-ttu-id="76d59-103">Hitelesítés hozzáadása a Xamarin Forms alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="76d59-103">Add authentication to your Xamarin Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a><span data-ttu-id="76d59-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="76d59-104">Overview</span></span>
<span data-ttu-id="76d59-105">Ez a témakör bemutatja, hogyan hitelesítheti egy App Service Mobile Apps az ügyfélalkalmazás felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="76d59-105">This topic shows you how to authenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="76d59-106">Ebben az oktatóanyagban hozzáadja a Xamarin Forms gyorsútmutató-projekt használja az identitásszolgáltató az App Service által támogatott hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="76d59-106">In this tutorial, you add authentication to the Xamarin Forms quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="76d59-107">Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps által, a felhasználói azonosító érték jelenik meg, és fogja tudni elérni a korlátozott tábla adatai.</span><span class="sxs-lookup"><span data-stu-id="76d59-107">After being successfully authenticated and authorized by your Mobile App, the user ID value is displayed, and you will be able to access restricted table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76d59-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="76d59-108">Prerequisites</span></span>
<span data-ttu-id="76d59-109">Ez az oktatóanyag a legjobb eredmény, javasoljuk, hogy hajtsa végre a [Xamarin Forms-alkalmazás létrehozása] [ 1] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="76d59-109">For the best result with this tutorial, we recommend that you first complete the [Create a Xamarin Forms app][1] tutorial.</span></span> <span data-ttu-id="76d59-110">Ez az oktatóanyag befejezése után fog egy többplatformos TodoList alkalmazást a Xamarin Forms projektet.</span><span class="sxs-lookup"><span data-stu-id="76d59-110">After you complete this tutorial, you will have a Xamarin Forms project that is a multi-platform TodoList app.</span></span>

<span data-ttu-id="76d59-111">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a hitelesítési kiterjesztés csomag fel a projektbe.</span><span class="sxs-lookup"><span data-stu-id="76d59-111">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="76d59-112">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a][2].</span><span class="sxs-lookup"><span data-stu-id="76d59-112">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps][2].</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="76d59-113">Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="76d59-113">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="76d59-114"><a name="redirecturl"></a>Az alkalmazás hozzáadása az engedélyezett külső átirányítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="76d59-114"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="76d59-115">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="76d59-115">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="76d59-116">Ez lehetővé teszi a hitelesítési rendszer visszairányítja az alkalmazás a hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="76d59-116">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="76d59-117">Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="76d59-117">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="76d59-118">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="76d59-118">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="76d59-119">A mobilalkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="76d59-119">It should be unique to your mobile application.</span></span> <span data-ttu-id="76d59-120">A kiszolgáló oldalán engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="76d59-120">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="76d59-121">Válassza ki az App Service az [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="76d59-121">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="76d59-122">Kattintson a **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="76d59-122">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="76d59-123">Az a **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="76d59-123">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="76d59-124">A **url_scheme_of_your_app** ezt a karakterláncot a rendszer az URL-séma, a mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="76d59-124">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="76d59-125">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="76d59-125">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="76d59-126">Meg kell győződnie, jegyezze fel a karakterláncot, amely úgy dönt, mivel úgy, hogy az URL-séma több helyen a mobilalkalmazás kódot kell.</span><span class="sxs-lookup"><span data-stu-id="76d59-126">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="76d59-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="76d59-127">Click **OK**.</span></span>

5. <span data-ttu-id="76d59-128">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="76d59-128">Click **Save**.</span></span>

## <a name="restrict-permissions-to-authenticated-users"></a><span data-ttu-id="76d59-129">A hitelesített felhasználókhoz jogosultságok korlátozása</span><span class="sxs-lookup"><span data-stu-id="76d59-129">Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a><span data-ttu-id="76d59-130">Hitelesítés hozzáadása a hordozható osztálytár</span><span class="sxs-lookup"><span data-stu-id="76d59-130">Add authentication to the portable class library</span></span>
<span data-ttu-id="76d59-131">Mobile Apps használja a [LoginAsync] [ 3] bővítmény metódust a [MobileServiceClient] [ 4] a felhasználó bejelentkezhet az App Service hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="76d59-131">Mobile Apps uses the [LoginAsync][3] extension method on the [MobileServiceClient][4] to sign in a user with App Service authentication.</span></span> <span data-ttu-id="76d59-132">Ez a minta egy hitelesítési kiszolgáló által felügyelt folyamat, amely megjeleníti a szolgáltató bejelentkezési felületen az alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="76d59-132">This sample uses a server-managed authentication flow that displays the provider's sign-in interface in the app.</span></span> <span data-ttu-id="76d59-133">További információkért lásd: [hitelesítési kiszolgáló által kezelt][5].</span><span class="sxs-lookup"><span data-stu-id="76d59-133">For more information, see [Server-managed authentication][5].</span></span> <span data-ttu-id="76d59-134">Az éles alkalmazásban jobb felhasználói élmény biztosítása érdekében érdemes lehet helyette használatával [hitelesítési ügyfél által felügyelt][6].</span><span class="sxs-lookup"><span data-stu-id="76d59-134">To provide a better user experience in your production app, you should consider instead using [Client-managed authentication][6].</span></span>

<span data-ttu-id="76d59-135">A Xamarin Forms projekt hitelesítéshez, adja meg egy **IAuthenticate** felületet az alkalmazás a hordozható osztály könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="76d59-135">To authenticate with a Xamarin Forms project, define an **IAuthenticate** interface in the Portable Class Library for the app.</span></span> <span data-ttu-id="76d59-136">Majd adja hozzá a **bejelentkezési** a felhasználói felület gombra kattintva indítsa el a hitelesítés a felhasználó hordozható Class Library definiált.</span><span class="sxs-lookup"><span data-stu-id="76d59-136">Then add a **Sign-in** button to the user interface defined in the Portable Class Library, which you click to start authentication.</span></span> <span data-ttu-id="76d59-137">Adatok betöltése a mobil-háttéralkalmazás a sikeres hitelesítést követően.</span><span class="sxs-lookup"><span data-stu-id="76d59-137">Data is loaded from the mobile app backend after successful authentication.</span></span>

<span data-ttu-id="76d59-138">Alkalmazzon a **IAuthenticate** felületet az alkalmazás által támogatott platformokon.</span><span class="sxs-lookup"><span data-stu-id="76d59-138">Implement the **IAuthenticate** interface for each platform supported by your app.</span></span>

1. <span data-ttu-id="76d59-139">A Visual Studio és Xamarin Studio, nyissa meg az App.cs a projektből az **hordozható** a neve, amely hordozható Class Library projektet, majd adja hozzá a következő `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="76d59-139">In Visual Studio or Xamarin Studio, open App.cs from the project with **Portable** in the name, which is Portable Class Library project, then  add the following `using` statement:</span></span>

        using System.Threading.Tasks;
2. <span data-ttu-id="76d59-140">A App.cs, adja hozzá a következő `IAuthenticate` felület előtt közvetlenül a `App` osztály definícióját.</span><span class="sxs-lookup"><span data-stu-id="76d59-140">In App.cs, add the following `IAuthenticate` interface definition immediately before the `App` class definition.</span></span>

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. <span data-ttu-id="76d59-141">A platform-specifikus megvalósításának kapcsolaton inicializálni a következő statikus tagokat adjanak hozzá a **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="76d59-141">To initialize the interface with a platform-specific implementation, add the following static members to the **App** class.</span></span>

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. <span data-ttu-id="76d59-142">Nyissa meg a TodoList.xaml a hordozható Class Library projekt, adja hozzá a következő **gomb** eleme a *buttonsPanel* elrendezés elem, a meglévő gomb után:</span><span class="sxs-lookup"><span data-stu-id="76d59-142">Open TodoList.xaml from the Portable Class Library project, add the following **Button** element in the *buttonsPanel* layout element, after the existing button:</span></span>

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    <span data-ttu-id="76d59-143">Ez a gomb elindítja a hitelesítési kiszolgáló által felügyelt és a mobil-háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="76d59-143">This button triggers server-managed authentication with your mobile app backend.</span></span>
5. <span data-ttu-id="76d59-144">Nyissa meg a TodoList.xaml.cs a hordozható Class Library projekt, majd adja hozzá a következő mező a `TodoList` osztály:</span><span class="sxs-lookup"><span data-stu-id="76d59-144">Open TodoList.xaml.cs from the Portable Class Library project, then add the following field to the `TodoList` class:</span></span>

        // Track whether the user has authenticated.
        bool authenticated = false;
6. <span data-ttu-id="76d59-145">Cserélje le a **OnAppearing** metódus a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="76d59-145">Replace the **OnAppearing** method with the following code:</span></span>

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    <span data-ttu-id="76d59-146">Ez a kód gondoskodik arról, hogy az adatok csak frissülnek a szolgáltatás Ön hitelesítése után.</span><span class="sxs-lookup"><span data-stu-id="76d59-146">This code makes sure that data is only refreshed from the service after you have been authenticated.</span></span>
7. <span data-ttu-id="76d59-147">Adja hozzá a következő kezelőt a **Clicked** esemény a **TodoList** osztály:</span><span class="sxs-lookup"><span data-stu-id="76d59-147">Add the following handler for the **Clicked** event to the **TodoList** class:</span></span>

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. <span data-ttu-id="76d59-148">A módosítások mentéséhez és a hibák ellenőrzése hordozható Class Library projekt újraépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="76d59-148">Save your changes and rebuild the Portable Class Library project verifying no errors.</span></span>

## <a name="add-authentication-to-the-android-app"></a><span data-ttu-id="76d59-149">Hitelesítés hozzáadása az Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="76d59-149">Add authentication to the Android app</span></span>
<span data-ttu-id="76d59-150">Ez a szakasz bemutatja, hogyan megvalósításához a **IAuthenticate** az Android-alkalmazás projekt felületet.</span><span class="sxs-lookup"><span data-stu-id="76d59-150">This section shows how to implement the **IAuthenticate** interface in the Android app project.</span></span> <span data-ttu-id="76d59-151">Ez a szakasz kihagyása, ha Android-eszköz nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="76d59-151">Skip this section if you are not supporting Android devices.</span></span>

1. <span data-ttu-id="76d59-152">A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal a **droid** projektre, majd **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="76d59-152">In Visual Studio or Xamarin Studio, right-click the **droid** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="76d59-153">Nyomja le az F5 billentyűt a projekt indításához a hibakereső, majd győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="76d59-153">Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="76d59-154">A 401-es kód jön létre, mert a háttérkiszolgálón hozzáférést csak a jogosult felhasználókra korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="76d59-154">The 401 code is produced because access on the backend is restricted to authorized users only.</span></span>
3. <span data-ttu-id="76d59-155">Nyissa meg az Android-projekt MainActivity.cs, és adja hozzá a következő `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="76d59-155">Open MainActivity.cs in the Android project and add the following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="76d59-156">Frissítés a **MainActivity** osztály megvalósításához a **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="76d59-156">Update the **MainActivity** class to implement the **IAuthenticate** interface, as follows:</span></span>

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. <span data-ttu-id="76d59-157">Frissítés a **MainActivity** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely szükséges a **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="76d59-157">Update the **MainActivity** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate** interface, as follows:</span></span>

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this, 
                    MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    <span data-ttu-id="76d59-158">Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a [MobileServiceAuthenticationProvider][7].</span><span class="sxs-lookup"><span data-stu-id="76d59-158">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider][7].</span></span>

6. <span data-ttu-id="76d59-159">Adja hozzá a következő kódot <application> AndroidManifest.xml csomópontján:</span><span class="sxs-lookup"><span data-stu-id="76d59-159">Add the following code inside <application> node of AndroidManifest.xml:</span></span>

```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
```

1. <span data-ttu-id="76d59-160">Adja hozzá a következő kódot a **OnCreate** metódusában a **MainActivity** hívása előtt osztály `LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="76d59-160">Add the following code to the **OnCreate** method of the **MainActivity** class before the call to `LoadApplication()`:</span></span>

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    <span data-ttu-id="76d59-161">Ez a kód biztosítja, hogy a hitelesítő inicializálása előtt az alkalmazás terhelés.</span><span class="sxs-lookup"><span data-stu-id="76d59-161">This code ensures the authenticator is initialized before the app loads.</span></span>
2. <span data-ttu-id="76d59-162">Építse újra az alkalmazást, majd futtassa, majd jelentkezzen be a hitelesítési szolgáltatót választotta, és ellenőrizze akkor érhessék el az adatokat egy hitelesített felhasználóként-e.</span><span class="sxs-lookup"><span data-stu-id="76d59-162">Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.</span></span>

## <a name="add-authentication-to-the-ios-app"></a><span data-ttu-id="76d59-163">Hitelesítés hozzáadása az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="76d59-163">Add authentication to the iOS app</span></span>
<span data-ttu-id="76d59-164">Ez a szakasz bemutatja, hogyan megvalósításához a **IAuthenticate** az iOS-alkalmazás projekt felülettel.</span><span class="sxs-lookup"><span data-stu-id="76d59-164">This section shows how to implement the **IAuthenticate** interface in the iOS app project.</span></span> <span data-ttu-id="76d59-165">Ez a szakasz kihagyása, ha nem támogatja a iOS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="76d59-165">Skip this section if you are not supporting iOS devices.</span></span>

1. <span data-ttu-id="76d59-166">A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal a **iOS** projektre, majd **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="76d59-166">In Visual Studio or Xamarin Studio, right-click the **iOS** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="76d59-167">Nyomja le az F5 billentyűt a projekt indításához a hibakereső, majd győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="76d59-167">Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="76d59-168">A 401-es válasz jön létre, mert a háttérkiszolgálón hozzáférést csak a jogosult felhasználókra korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="76d59-168">The 401 response is produced because access on the backend is restricted to authorized users only.</span></span>
3. <span data-ttu-id="76d59-169">Nyissa meg a AppDelegate.cs az iOS-projektre, és adja hozzá a következő `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="76d59-169">Open AppDelegate.cs in the iOS project and add the following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="76d59-170">Frissítés a **AppDelegate** osztály megvalósításához a **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="76d59-170">Update the **AppDelegate** class to implement the **IAuthenticate** interface, as follows:</span></span>

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. <span data-ttu-id="76d59-171">Frissítés a **AppDelegate** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely szükséges a **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="76d59-171">Update the **AppDelegate** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate** interface, as follows:</span></span>

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;
                    }
                }
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    <span data-ttu-id="76d59-172">Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a(z) [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="76d59-172">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

6. <span data-ttu-id="76d59-173">A AppDelegate osztály OpenUrl (UIApplication alkalmazás által igényelt NSUrl URL-címe, NSDictionary beállítások) metódus túlterhelési hozzáadásával frissítése</span><span class="sxs-lookup"><span data-stu-id="76d59-173">Update the AppDelegate class by adding OpenUrl(UIApplication app, NSUrl url, NSDictionary options) method overload</span></span>

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. <span data-ttu-id="76d59-174">Adja hozzá a következő kódsort a **FinishedLaunching** metódus hívása előtt `LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="76d59-174">Add the following line of code to the **FinishedLaunching** method before the call to `LoadApplication()`:</span></span>

        App.Init(this);

    <span data-ttu-id="76d59-175">Ez a kód biztosítja, hogy a hitelesítő inicializálása előtt az alkalmazáshoz be van töltve.</span><span class="sxs-lookup"><span data-stu-id="76d59-175">This code ensures the authenticator is initialized before the app is loaded.</span></span>

6. <span data-ttu-id="76d59-176">Adja hozzá **{url_scheme_of_your_app}** az URL-sémákat Info.plist-ben.</span><span class="sxs-lookup"><span data-stu-id="76d59-176">Add **{url_scheme_of_your_app}** to URL Schemes in Info.plist.</span></span>

7. <span data-ttu-id="76d59-177">Építse újra az alkalmazást, majd futtassa, majd jelentkezzen be a hitelesítési szolgáltatót választotta, és ellenőrizze akkor érhessék el az adatokat egy hitelesített felhasználóként-e.</span><span class="sxs-lookup"><span data-stu-id="76d59-177">Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.</span></span>

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a><span data-ttu-id="76d59-178">Hitelesítés hozzáadása a Windows 10 (beleértve a telefonszám) alkalmazás projektek</span><span class="sxs-lookup"><span data-stu-id="76d59-178">Add authentication to Windows 10 (including Phone) app projects</span></span>
<span data-ttu-id="76d59-179">Ez a szakasz bemutatja, hogyan megvalósításához a **IAuthenticate** felület a Windows 10-alkalmazást projektekben.</span><span class="sxs-lookup"><span data-stu-id="76d59-179">This section shows how to implement the **IAuthenticate** interface in the Windows 10 app projects.</span></span> <span data-ttu-id="76d59-180">Ugyanezek a lépések alkalmazni az univerzális Windows Platform (UWP) projektek, de a segítségével a **UWP** project (beállításértékeket módosításokkal).</span><span class="sxs-lookup"><span data-stu-id="76d59-180">The same steps apply for Universal Windows Platform (UWP) projects, but using the **UWP** project (with noted changes).</span></span> <span data-ttu-id="76d59-181">Ez a szakasz kihagyása, ha nem támogatja a Windows-eszközök.</span><span class="sxs-lookup"><span data-stu-id="76d59-181">Skip this section if you are not supporting Windows devices.</span></span>

1. <span data-ttu-id="76d59-182">"A Visual Studióban, kattintson a jobb gombbal bármelyik a **UWP** projektre, majd **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="76d59-182">"In Visual Studio, right-click either the **UWP** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="76d59-183">Nyomja le az F5 billentyűt a projekt indításához a hibakereső, majd győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="76d59-183">Press F5 to start the project in the debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="76d59-184">A 401-es válasz az oka, hogy a háttérkiszolgálón hozzáférést csak a jogosult felhasználókra korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="76d59-184">The 401 response happens because access on the backend is restricted to authorized users only.</span></span>
3. <span data-ttu-id="76d59-185">Nyissa meg a MainPage.xaml.cs a Windows-alkalmazás projekt, és adja hozzá a következő `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="76d59-185">Open MainPage.xaml.cs for the Windows app project and add the following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    <span data-ttu-id="76d59-186">Cserélje le `<your_Portable_Class_Library_namespace>` a hordozható osztály szalagtár névtérrel.</span><span class="sxs-lookup"><span data-stu-id="76d59-186">Replace `<your_Portable_Class_Library_namespace>` with the namespace for your portable class library.</span></span>
4. <span data-ttu-id="76d59-187">Frissítés a **MainPage** osztály megvalósításához a **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="76d59-187">Update the **MainPage** class to implement the **IAuthenticate** interface, as follows:</span></span>

        public sealed partial class MainPage : IAuthenticate
5. <span data-ttu-id="76d59-188">Frissítés a **MainPage** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely szükséges a **IAuthenticate**csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="76d59-188">Update the **MainPage** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by the **IAuthenticate** interface, as follows:</span></span>

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }

            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    <span data-ttu-id="76d59-189">Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a(z) [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="76d59-189">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

1. <span data-ttu-id="76d59-190">Adja hozzá a következő kódsort konstruktorában a **MainPage** hívása előtt osztály `LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="76d59-190">Add the following line of code in the constructor for the **MainPage** class before the call to `LoadApplication()`:</span></span>

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    <span data-ttu-id="76d59-191">Cserélje le `<your_Portable_Class_Library_namespace>` a hordozható osztály szalagtár névtérrel.</span><span class="sxs-lookup"><span data-stu-id="76d59-191">Replace `<your_Portable_Class_Library_namespace>` with the namespace for your portable class library.</span></span>

3. <span data-ttu-id="76d59-192">Ha használ **UWP**, adja hozzá a következő **OnActivated** metódus bírálja felül, ha a **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="76d59-192">If you are using **UWP**, add the following **OnActivated** method override to the **App** class:</span></span>

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   <span data-ttu-id="76d59-193">Ha a metódus-felülbírálása már létezik, adja hozzá a feltételes kódot az előző részlet.</span><span class="sxs-lookup"><span data-stu-id="76d59-193">When the method override already exists, add the conditional code from the preceding snippet.</span></span>  <span data-ttu-id="76d59-194">Ez a kód nincs szükség az univerzális Windows-projektek esetében.</span><span class="sxs-lookup"><span data-stu-id="76d59-194">This code is not required for Universal Windows projects.</span></span>

3. <span data-ttu-id="76d59-195">Adja hozzá **{url_scheme_of_your_app}** a Package.appxmanifest.</span><span class="sxs-lookup"><span data-stu-id="76d59-195">Add **{url_scheme_of_your_app}** in Package.appxmanifest.</span></span> 

4. <span data-ttu-id="76d59-196">Építse újra az alkalmazást, majd futtassa, majd jelentkezzen be a hitelesítési szolgáltatót választotta, és ellenőrizze akkor érhessék el az adatokat egy hitelesített felhasználóként-e.</span><span class="sxs-lookup"><span data-stu-id="76d59-196">Rebuild the app, run it, then sign in with the authentication provider you chose and verify you are able to access data as an authenticated user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="76d59-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76d59-197">Next steps</span></span>
<span data-ttu-id="76d59-198">Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, fontolja meg valamelyik az alábbi oktatóanyagok folytatása:</span><span class="sxs-lookup"><span data-stu-id="76d59-198">Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:</span></span>

* [<span data-ttu-id="76d59-199">Leküldéses értesítések hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="76d59-199">Add push notifications to your app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)

  <span data-ttu-id="76d59-200">Ismerje meg, hogyan adhat leküldéses értesítéseket az alkalmazásához, illetve hogyan konfigurálhatja Mobile Apps-háttéralkalmazását az Azure Notification Hubs használatára a leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="76d59-200">Learn how to add push notifications support to your app and configure your Mobile App backend to use Azure Notification Hubs to send push notifications.</span></span>
* [<span data-ttu-id="76d59-201">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="76d59-201">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  <span data-ttu-id="76d59-202">Megtudhatja, hogyan adhat offline támogatást az alkalmazást a Mobile Apps-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="76d59-202">Learn how to add offline support your app using a Mobile App backend.</span></span> <span data-ttu-id="76d59-203">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók számára, amellyel kommunikálni tud a mobilalkalmazás - megtekintését, hozzáadását és - adatok módosítását, akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="76d59-203">Offline sync allows end users to interact with a mobile app - viewing, adding, or modifying data - even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
