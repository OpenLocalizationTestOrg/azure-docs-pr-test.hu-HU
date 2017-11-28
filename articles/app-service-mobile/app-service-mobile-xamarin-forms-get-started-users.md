---
title: "a Xamarin Forms app Mobile Apps hitelesítéssel elindítva aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Mobile Apps tooauthenticate Identitásszolgáltatók, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos Xamarin Forms alkalmazása felhasználóit."
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
ms.openlocfilehash: 7f6716619f33d9cc4f866c41effba8f048dc49fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarin-forms-app"></a><span data-ttu-id="87f33-103">Hitelesítési tooyour Xamarin Forms alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87f33-103">Add authentication tooyour Xamarin Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a><span data-ttu-id="87f33-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="87f33-104">Overview</span></span>
<span data-ttu-id="87f33-105">Ez a témakör bemutatja, hogyan egy App Service Mobile Apps az ügyfélalkalmazás tooauthenticate felhasználója.</span><span class="sxs-lookup"><span data-stu-id="87f33-105">This topic shows you how tooauthenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="87f33-106">Ebben az oktatóanyagban hozzáadja hitelesítési hello gyorsútmutató-projekt Xamarin Forms App Service által támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="87f33-106">In this tutorial, you add authentication to hello Xamarin Forms quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="87f33-107">Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps által, a hello felhasználói azonosító értéke megjelenik-e, és korlátozott képes tooaccess adatait fogja.</span><span class="sxs-lookup"><span data-stu-id="87f33-107">After being successfully authenticated and authorized by your Mobile App, hello user ID value is displayed, and you will be able tooaccess restricted table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87f33-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="87f33-108">Prerequisites</span></span>
<span data-ttu-id="87f33-109">Ez az oktatóanyag legjobb eredmény hello, javasoljuk, hogy az első teljes hello [Xamarin Forms-alkalmazás létrehozása] [ 1] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="87f33-109">For hello best result with this tutorial, we recommend that you first complete hello [Create a Xamarin Forms app][1] tutorial.</span></span> <span data-ttu-id="87f33-110">Ez az oktatóanyag befejezése után fog egy többplatformos TodoList alkalmazást a Xamarin Forms projektet.</span><span class="sxs-lookup"><span data-stu-id="87f33-110">After you complete this tutorial, you will have a Xamarin Forms project that is a multi-platform TodoList app.</span></span>

<span data-ttu-id="87f33-111">Ha nem használ hello letöltése – első lépések, hello hitelesítési kiegészítő csomag tooyour projekt hozzá kell adnia.</span><span class="sxs-lookup"><span data-stu-id="87f33-111">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="87f33-112">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][2].</span><span class="sxs-lookup"><span data-stu-id="87f33-112">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps][2].</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="87f33-113">Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="87f33-113">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="87f33-114"><a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87f33-114"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="87f33-115">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="87f33-115">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="87f33-116">Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="87f33-116">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="87f33-117">Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="87f33-117">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="87f33-118">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="87f33-118">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="87f33-119">Egyedi tooyour mobilalkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="87f33-119">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="87f33-120">tooenable hello átirányítási hello kiszolgáló oldalán:</span><span class="sxs-lookup"><span data-stu-id="87f33-120">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="87f33-121">Hello [Azure-portálon] válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="87f33-121">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="87f33-122">Kattintson a hello **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="87f33-122">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="87f33-123">A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="87f33-123">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="87f33-124">Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="87f33-124">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="87f33-125">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="87f33-125">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="87f33-126">Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="87f33-126">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="87f33-127">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="87f33-127">Click **OK**.</span></span>

5. <span data-ttu-id="87f33-128">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="87f33-128">Click **Save**.</span></span>

## <a name="restrict-permissions-tooauthenticated-users"></a><span data-ttu-id="87f33-129">Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="87f33-129">Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-toohello-portable-class-library"></a><span data-ttu-id="87f33-130">Hitelesítési toohello hordozható osztály könyvtár hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87f33-130">Add authentication toohello portable class library</span></span>
<span data-ttu-id="87f33-131">Mobile Apps használ hello [LoginAsync] [ 3] hello bővítmény metódusa [MobileServiceClient] [ 4] toosign a felhasználók az App Service hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="87f33-131">Mobile Apps uses hello [LoginAsync][3] extension method on hello [MobileServiceClient][4] toosign in a user with App Service authentication.</span></span> <span data-ttu-id="87f33-132">A példa a kiszolgáló által kezelt hitelesítési folyamat, amely megjeleníti hello szolgáltató bejelentkezési felületen hello alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="87f33-132">This sample uses a server-managed authentication flow that displays hello provider's sign-in interface in hello app.</span></span> <span data-ttu-id="87f33-133">További információkért lásd: [hitelesítési kiszolgáló által kezelt][5].</span><span class="sxs-lookup"><span data-stu-id="87f33-133">For more information, see [Server-managed authentication][5].</span></span> <span data-ttu-id="87f33-134">Az éles alkalmazásban jobb felhasználói élmény biztosítása érdekében érdemes lehet helyette használatával [hitelesítési ügyfél által felügyelt][6].</span><span class="sxs-lookup"><span data-stu-id="87f33-134">To provide a better user experience in your production app, you should consider instead using [Client-managed authentication][6].</span></span>

<span data-ttu-id="87f33-135">a Xamarin Forms-projekt tooauthenticate megadása egy **IAuthenticate** hello hordozható Class Library hello alkalmazás felülettel.</span><span class="sxs-lookup"><span data-stu-id="87f33-135">tooauthenticate with a Xamarin Forms project, define an **IAuthenticate** interface in hello Portable Class Library for hello app.</span></span> <span data-ttu-id="87f33-136">Majd adja hozzá a **bejelentkezési** gomb toohello felhasználói felület hello hordozható Class Library, amelyek a meghatározott toostart hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="87f33-136">Then add a **Sign-in** button toohello user interface defined in hello Portable Class Library, which you click toostart authentication.</span></span> <span data-ttu-id="87f33-137">Adatok betöltése a mobil-háttéralkalmazás hello sikeres hitelesítés után.</span><span class="sxs-lookup"><span data-stu-id="87f33-137">Data is loaded from hello mobile app backend after successful authentication.</span></span>

<span data-ttu-id="87f33-138">Alkalmazzon hello **IAuthenticate** felületet az alkalmazás által támogatott platformokon.</span><span class="sxs-lookup"><span data-stu-id="87f33-138">Implement hello **IAuthenticate** interface for each platform supported by your app.</span></span>

1. <span data-ttu-id="87f33-139">A Visual Studio és Xamarin Studio, nyissa meg az App.cs hello projektből az **hordozható** hello neve, amely hordozható Class Library projektet, majd adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="87f33-139">In Visual Studio or Xamarin Studio, open App.cs from hello project with **Portable** in hello name, which is Portable Class Library project, then  add hello following `using` statement:</span></span>

        using System.Threading.Tasks;
2. <span data-ttu-id="87f33-140">A App.cs, adja hozzá a hello következő `IAuthenticate` hello előtt közvetlenül felület `App` definition osztályban.</span><span class="sxs-lookup"><span data-stu-id="87f33-140">In App.cs, add hello following `IAuthenticate` interface definition immediately before hello `App` class definition.</span></span>

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. <span data-ttu-id="87f33-141">a platform-specifikus megvalósításának tooinitialize hello illesztőfelület adja hozzá a következő statikus tagok toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="87f33-141">tooinitialize hello interface with a platform-specific implementation, add hello following static members toohello **App** class.</span></span>

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. <span data-ttu-id="87f33-142">Nyissa meg a TodoList.xaml hello hordozható Class Library projekt, adja hozzá a következő hello **gomb** hello elemének *buttonsPanel* elrendezés elem hello meglévő gomb után:</span><span class="sxs-lookup"><span data-stu-id="87f33-142">Open TodoList.xaml from hello Portable Class Library project, add hello following **Button** element in hello *buttonsPanel* layout element, after hello existing button:</span></span>

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    <span data-ttu-id="87f33-143">Ez a gomb elindítja a hitelesítési kiszolgáló által felügyelt és a mobil-háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="87f33-143">This button triggers server-managed authentication with your mobile app backend.</span></span>
5. <span data-ttu-id="87f33-144">Nyissa meg a TodoList.xaml.cs hello hordozható Class Library projektet, majd adja hozzá a következő mező toohello hello `TodoList` osztály:</span><span class="sxs-lookup"><span data-stu-id="87f33-144">Open TodoList.xaml.cs from hello Portable Class Library project, then add hello following field toohello `TodoList` class:</span></span>

        // Track whether hello user has authenticated.
        bool authenticated = false;
6. <span data-ttu-id="87f33-145">Cserélje le a hello **OnAppearing** hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="87f33-145">Replace hello **OnAppearing** method with hello following code:</span></span>

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems tootrue in order toosynchronize hello data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide hello Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    <span data-ttu-id="87f33-146">Ez a kód gondoskodik arról, hogy az adatok csak frissítése hello szolgáltatás az Ön hitelesítése után.</span><span class="sxs-lookup"><span data-stu-id="87f33-146">This code makes sure that data is only refreshed from hello service after you have been authenticated.</span></span>
7. <span data-ttu-id="87f33-147">Adja hozzá a következő hello kezelője hello **Clicked** esemény toohello **TodoList** osztály:</span><span class="sxs-lookup"><span data-stu-id="87f33-147">Add hello following handler for hello **Clicked** event toohello **TodoList** class:</span></span>

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems tootrue toosynchronize hello data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. <span data-ttu-id="87f33-148">A módosítások mentéséhez és a hibák ellenőrzése hello hordozható Class Library projekt újraépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="87f33-148">Save your changes and rebuild hello Portable Class Library project verifying no errors.</span></span>

## <a name="add-authentication-toohello-android-app"></a><span data-ttu-id="87f33-149">Hitelesítési toohello Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87f33-149">Add authentication toohello Android app</span></span>
<span data-ttu-id="87f33-150">Ez a szakasz bemutatja, hogyan tooimplement hello **IAuthenticate** hello Android-alkalmazás projekt felületet.</span><span class="sxs-lookup"><span data-stu-id="87f33-150">This section shows how tooimplement hello **IAuthenticate** interface in hello Android app project.</span></span> <span data-ttu-id="87f33-151">Ez a szakasz kihagyása, ha Android-eszköz nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="87f33-151">Skip this section if you are not supporting Android devices.</span></span>

1. <span data-ttu-id="87f33-152">A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal hello **droid** projektre, majd **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="87f33-152">In Visual Studio or Xamarin Studio, right-click hello **droid** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="87f33-153">Nyomja le az F5 toostart hello hello hibakeresőben projektre, majd győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="87f33-153">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="87f33-154">hello 401-es kód jön létre, mert a hozzáférés hello háttérkiszolgálón csak korlátozott tooauthorized felhasználók.</span><span class="sxs-lookup"><span data-stu-id="87f33-154">hello 401 code is produced because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="87f33-155">Nyissa meg a MainActivity.cs hello Android-projekt, és adja hozzá a következő hello `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="87f33-155">Open MainActivity.cs in hello Android project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="87f33-156">Frissítés hello **MainActivity** osztály tooimplement hello **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="87f33-156">Update hello **MainActivity** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. <span data-ttu-id="87f33-157">Frissítés hello **MainActivity** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely hello által igényelt **IAuthenticate**  csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="87f33-157">Update hello **MainActivity** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

            // Display hello success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    <span data-ttu-id="87f33-158">Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a [MobileServiceAuthenticationProvider][7].</span><span class="sxs-lookup"><span data-stu-id="87f33-158">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider][7].</span></span>

6. <span data-ttu-id="87f33-159">Adja hozzá a következő kódot hello <application> AndroidManifest.xml csomópontján:</span><span class="sxs-lookup"><span data-stu-id="87f33-159">Add hello following code inside <application> node of AndroidManifest.xml:</span></span>

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

1. <span data-ttu-id="87f33-160">Adja hozzá a következő kód toohello hello **OnCreate** hello metódusában **MainActivity** túl osztály hello hívása előtt`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="87f33-160">Add hello following code toohello **OnCreate** method of hello **MainActivity** class before hello call too`LoadApplication()`:</span></span>

        // Initialize hello authenticator before loading hello app.
        App.Init((IAuthenticate)this);

    <span data-ttu-id="87f33-161">Ez a kód biztosítja hello hitelesítő inicializálása előtt hello alkalmazás terhelés.</span><span class="sxs-lookup"><span data-stu-id="87f33-161">This code ensures hello authenticator is initialized before hello app loads.</span></span>
2. <span data-ttu-id="87f33-162">Hello app újraépítése, majd futtassa, majd hello hitelesítési szolgáltatót választotta, és ellenőrizze, képes tooaccess adatokat egy hitelesített felhasználóként jelentkezzen.</span><span class="sxs-lookup"><span data-stu-id="87f33-162">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="add-authentication-toohello-ios-app"></a><span data-ttu-id="87f33-163">Hitelesítési toohello iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87f33-163">Add authentication toohello iOS app</span></span>
<span data-ttu-id="87f33-164">Ez a szakasz bemutatja, hogyan tooimplement hello **IAuthenticate** hello iOS-alkalmazás projekt felületet.</span><span class="sxs-lookup"><span data-stu-id="87f33-164">This section shows how tooimplement hello **IAuthenticate** interface in hello iOS app project.</span></span> <span data-ttu-id="87f33-165">Ez a szakasz kihagyása, ha nem támogatja a iOS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="87f33-165">Skip this section if you are not supporting iOS devices.</span></span>

1. <span data-ttu-id="87f33-166">A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal hello **iOS** projektre, majd **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="87f33-166">In Visual Studio or Xamarin Studio, right-click hello **iOS** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="87f33-167">Nyomja le az F5 toostart hello hello hibakeresőben projektre, majd győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="87f33-167">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="87f33-168">hello 401-es válasz jön létre, mert a hozzáférés hello háttérkiszolgálón csak korlátozott tooauthorized felhasználók.</span><span class="sxs-lookup"><span data-stu-id="87f33-168">hello 401 response is produced because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="87f33-169">Nyissa meg AppDelegate.cs hello iOS-projektre, és adja hozzá a következő hello `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="87f33-169">Open AppDelegate.cs in hello iOS project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="87f33-170">Frissítés hello **AppDelegate** osztály tooimplement hello **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="87f33-170">Update hello **AppDelegate** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. <span data-ttu-id="87f33-171">Frissítés hello **AppDelegate** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely hello által igényelt **IAuthenticate**  csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="87f33-171">Update hello **AppDelegate** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

            // Display hello success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    <span data-ttu-id="87f33-172">Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a(z) [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="87f33-172">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

6. <span data-ttu-id="87f33-173">Hello AppDelegate osztály OpenUrl (UIApplication alkalmazás által igényelt NSUrl URL-címe, NSDictionary beállítások) metódus túlterhelési hozzáadásával frissítése</span><span class="sxs-lookup"><span data-stu-id="87f33-173">Update hello AppDelegate class by adding OpenUrl(UIApplication app, NSUrl url, NSDictionary options) method overload</span></span>

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. <span data-ttu-id="87f33-174">Adja hozzá a következő kód toohello üzletági hello **FinishedLaunching** előtt hello kiszolgálómetódus-hívás túl`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="87f33-174">Add hello following line of code toohello **FinishedLaunching** method before hello call too`LoadApplication()`:</span></span>

        App.Init(this);

    <span data-ttu-id="87f33-175">Ez a kód biztosítja hello hitelesítő inicializálása előtt hello app be van töltve.</span><span class="sxs-lookup"><span data-stu-id="87f33-175">This code ensures hello authenticator is initialized before hello app is loaded.</span></span>

6. <span data-ttu-id="87f33-176">Adja hozzá **{url_scheme_of_your_app}** Info.plist tooURL sémákat.</span><span class="sxs-lookup"><span data-stu-id="87f33-176">Add **{url_scheme_of_your_app}** tooURL Schemes in Info.plist.</span></span>

7. <span data-ttu-id="87f33-177">Hello app újraépítése, majd futtassa, majd hello hitelesítési szolgáltatót választotta, és ellenőrizze, képes tooaccess adatokat egy hitelesített felhasználóként jelentkezzen.</span><span class="sxs-lookup"><span data-stu-id="87f33-177">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="add-authentication-toowindows-10-including-phone-app-projects"></a><span data-ttu-id="87f33-178">Adja hozzá a hitelesítési tooWindows 10 (többek között a következőket telefon) alkalmazás projektek</span><span class="sxs-lookup"><span data-stu-id="87f33-178">Add authentication tooWindows 10 (including Phone) app projects</span></span>
<span data-ttu-id="87f33-179">Ez a szakasz bemutatja, hogyan tooimplement hello **IAuthenticate** felület hello Windows 10-alkalmazást projektekben.</span><span class="sxs-lookup"><span data-stu-id="87f33-179">This section shows how tooimplement hello **IAuthenticate** interface in hello Windows 10 app projects.</span></span> <span data-ttu-id="87f33-180">Ugyanezek a lépések alkalmazni az univerzális Windows Platform (UWP) projektek, de hello segítségével hello **UWP** project (beállításértékeket módosításokkal).</span><span class="sxs-lookup"><span data-stu-id="87f33-180">hello same steps apply for Universal Windows Platform (UWP) projects, but using hello **UWP** project (with noted changes).</span></span> <span data-ttu-id="87f33-181">Ez a szakasz kihagyása, ha nem támogatja a Windows-eszközök.</span><span class="sxs-lookup"><span data-stu-id="87f33-181">Skip this section if you are not supporting Windows devices.</span></span>

1. <span data-ttu-id="87f33-182">"A Visual Studióban, kattintson a jobb gombbal bármelyik hello **UWP** projektre, majd **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="87f33-182">"In Visual Studio, right-click either hello **UWP** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="87f33-183">Nyomja le az F5 toostart hello hello hibakeresőben projektre, majd győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="87f33-183">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="87f33-184">hello 401-es válasz akkor fordul elő, mert hello háttérkiszolgálón hozzáférés csak korlátozott tooauthorized felhasználók.</span><span class="sxs-lookup"><span data-stu-id="87f33-184">hello 401 response happens because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="87f33-185">Nyissa meg a MainPage.xaml.cs hello Windows-alkalmazás projekt, és adja hozzá a következő hello `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="87f33-185">Open MainPage.xaml.cs for hello Windows app project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    <span data-ttu-id="87f33-186">Cserélje le `<your_Portable_Class_Library_namespace>` a hordozható osztály szalagtár hello névtérrel.</span><span class="sxs-lookup"><span data-stu-id="87f33-186">Replace `<your_Portable_Class_Library_namespace>` with hello namespace for your portable class library.</span></span>
4. <span data-ttu-id="87f33-187">Frissítés hello **MainPage** osztály tooimplement hello **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="87f33-187">Update hello **MainPage** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public sealed partial class MainPage : IAuthenticate
5. <span data-ttu-id="87f33-188">Frissítés hello **MainPage** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely hello által igényelt **IAuthenticate** csatoló, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="87f33-188">Update hello **MainPage** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

            // Display hello success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    <span data-ttu-id="87f33-189">Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a(z) [MobileServiceAuthenticationProvider].</span><span class="sxs-lookup"><span data-stu-id="87f33-189">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

1. <span data-ttu-id="87f33-190">Adja hozzá a következő kódsort hello hello konstruktora hello **MainPage** túl osztály hello hívása előtt`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="87f33-190">Add hello following line of code in hello constructor for hello **MainPage** class before hello call too`LoadApplication()`:</span></span>

        // Initialize hello authenticator before loading hello app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    <span data-ttu-id="87f33-191">Cserélje le `<your_Portable_Class_Library_namespace>` a hordozható osztály szalagtár hello névtérrel.</span><span class="sxs-lookup"><span data-stu-id="87f33-191">Replace `<your_Portable_Class_Library_namespace>` with hello namespace for your portable class library.</span></span>

3. <span data-ttu-id="87f33-192">Ha használ **UWP**, adja hozzá a következő hello **OnActivated** metódus-felülbírálása toohello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="87f33-192">If you are using **UWP**, add hello following **OnActivated** method override toohello **App** class:</span></span>

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   <span data-ttu-id="87f33-193">Ha hello metódus-felülbírálása már létezik, adja hozzá hello feltételes kód részlet megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="87f33-193">When hello method override already exists, add hello conditional code from hello preceding snippet.</span></span>  <span data-ttu-id="87f33-194">Ez a kód nincs szükség az univerzális Windows-projektek esetében.</span><span class="sxs-lookup"><span data-stu-id="87f33-194">This code is not required for Universal Windows projects.</span></span>

3. <span data-ttu-id="87f33-195">Adja hozzá **{url_scheme_of_your_app}** a Package.appxmanifest.</span><span class="sxs-lookup"><span data-stu-id="87f33-195">Add **{url_scheme_of_your_app}** in Package.appxmanifest.</span></span> 

4. <span data-ttu-id="87f33-196">Hello app újraépítése, majd futtassa, majd hello hitelesítési szolgáltatót választotta, és ellenőrizze, képes tooaccess adatokat egy hitelesített felhasználóként jelentkezzen.</span><span class="sxs-lookup"><span data-stu-id="87f33-196">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87f33-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87f33-197">Next steps</span></span>
<span data-ttu-id="87f33-198">Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:</span><span class="sxs-lookup"><span data-stu-id="87f33-198">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="87f33-199">Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="87f33-199">Add push notifications tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)

  <span data-ttu-id="87f33-200">Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a mobilalkalmazás háttér toouse Azure Notification Hubs toosend leküldéses értesítések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="87f33-200">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="87f33-201">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="87f33-201">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  <span data-ttu-id="87f33-202">Ismerje meg, hogyan tooadd offline támogatják az alkalmazást a Mobile Apps-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="87f33-202">Learn how tooadd offline support your app using a Mobile App backend.</span></span> <span data-ttu-id="87f33-203">Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást - megtekintését, hozzáadását és - adatok módosítását, akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="87f33-203">Offline sync allows end users toointeract with a mobile app - viewing, adding, or modifying data - even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
