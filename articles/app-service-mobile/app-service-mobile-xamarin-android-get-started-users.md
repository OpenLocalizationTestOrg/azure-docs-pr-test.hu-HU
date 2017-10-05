---
title: "Ismerkedés a Mobile Apps a Xamarin Android hitelesítés"
description: "Útmutató a Mobile Apps segítségével hitelesíti a felhasználókat az identitás-szolgáltatóktól, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos Xamarin Android-alkalmazás."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 8f9a1109018c708d52cdcb7b8bce43861cecd31c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a><span data-ttu-id="32122-103">Hitelesítés hozzáadása Xamarin.Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="32122-103">Add authentication to your Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="32122-104">Ez a témakör bemutatja, hogyan hitelesítheti a Mobile Apps, az ügyfélalkalmazás felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="32122-104">This topic shows you how to authenticate users of a Mobile App from your client application.</span></span> <span data-ttu-id="32122-105">Ebben az oktatóanyagban hitelesítés használata az Azure Mobile Apps által támogatott identitásszolgáltató gyorsútmutató-projekt hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="32122-105">In this tutorial, you add authentication to the quickstart project using an identity provider that is supported by Azure Mobile Apps.</span></span> <span data-ttu-id="32122-106">Miután sikeresen alatt hitelesítése és engedélyezése a mobileszköz-alkalmazás, a felhasználói azonosító értéke jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="32122-106">After being successfully authenticated and authorized in the Mobile App, the user ID value is displayed.</span></span>

<span data-ttu-id="32122-107">Ez az oktatóanyag a Mobile Apps gyors üzembe helyezés alapul.</span><span class="sxs-lookup"><span data-stu-id="32122-107">This tutorial is based on the Mobile App quickstart.</span></span> <span data-ttu-id="32122-108">Először is el kell végeznie az oktatóanyag [Xamarin.Android-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="32122-108">You must also first complete the tutorial [Create a Xamarin.Android app].</span></span> <span data-ttu-id="32122-109">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, hozzá kell adnia a hitelesítési kiterjesztés csomag fel a projektbe.</span><span class="sxs-lookup"><span data-stu-id="32122-109">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="32122-110">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="32122-110">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <span data-ttu-id="32122-111"><a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="32122-111"><a name="register"></a>Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="32122-112"><a name="redirecturl"></a>Az alkalmazás hozzáadása az engedélyezett külső átirányítási URL-címek</span><span class="sxs-lookup"><span data-stu-id="32122-112"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="32122-113">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="32122-113">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="32122-114">Ez lehetővé teszi a hitelesítési rendszer visszairányítja az alkalmazás a hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="32122-114">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="32122-115">Ebben az oktatóanyagban az URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="32122-115">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="32122-116">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="32122-116">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="32122-117">A mobilalkalmazás egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="32122-117">It should be unique to your mobile application.</span></span> <span data-ttu-id="32122-118">A kiszolgáló oldalán engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="32122-118">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="32122-119">Válassza ki az App Service az [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="32122-119">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="32122-120">Kattintson a **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="32122-120">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="32122-121">Az a **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="32122-121">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="32122-122">A **url_scheme_of_your_app** ezt a karakterláncot a rendszer az URL-séma, a mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="32122-122">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="32122-123">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="32122-123">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="32122-124">Meg kell győződnie, jegyezze fel a karakterláncot, amely úgy dönt, mivel úgy, hogy az URL-séma több helyen a mobilalkalmazás kódot kell.</span><span class="sxs-lookup"><span data-stu-id="32122-124">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="32122-125">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="32122-125">Click **OK**.</span></span>

5. <span data-ttu-id="32122-126">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="32122-126">Click **Save**.</span></span>

## <span data-ttu-id="32122-127"><a name="permissions"></a>A hitelesített felhasználókhoz jogosultságok korlátozása</span><span class="sxs-lookup"><span data-stu-id="32122-127"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="32122-128">A Visual Studio és Xamarin Studio futtatható az ügyfélprojekt egy eszközt vagy emulátort.</span><span class="sxs-lookup"><span data-stu-id="32122-128">In Visual Studio or Xamarin Studio, run the client project on a device or emulator.</span></span> <span data-ttu-id="32122-129">Győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="32122-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="32122-130">Ez akkor fordul elő, mert az alkalmazás megpróbál hozzáférni a mobil-háttéralkalmazást, nem hitelesített felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="32122-130">This happens because the app attempts to access your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="32122-131">A *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="32122-131">The *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="32122-132">A következő később frissíteni az ügyfélalkalmazás az erőforrásokat a Mobile Apps-háttéralkalmazás a hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="32122-132">Next, you will update the client app to request resources from the Mobile App backend with an authenticated user.</span></span>

## <span data-ttu-id="32122-133"><a name="add-authentication"></a>Hitelesítés hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="32122-133"><a name="add-authentication"></a>Add authentication to the app</span></span>
<span data-ttu-id="32122-134">A felhasználók koppintson frissítette az alkalmazást a **bejelentkezés** gombra, és hitelesíti a, mielőtt adatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="32122-134">The app is updated to require users to tap the **Sign in** button and authenticate before data is displayed.</span></span>

1. <span data-ttu-id="32122-135">Adja hozzá a következő kódot a **TodoActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="32122-135">Add the following code to the **TodoActivity** class:</span></span>
   
        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    <span data-ttu-id="32122-136">Ezzel létrehoz egy új módszer a felhasználók és a metódus kezelő hitelesítésére az új **bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="32122-136">This creates a new method to authenticate a user and a method handler for a new **Sign in** button.</span></span> <span data-ttu-id="32122-137">A fenti példa kód a felhasználó hitelesítése egy Facebook-bejelentkezési használatával.</span><span class="sxs-lookup"><span data-stu-id="32122-137">The user in the example code above is authenticated by using a Facebook login.</span></span> <span data-ttu-id="32122-138">A párbeszédpanel segítségével egyszer hitelesített felhasználói azonosító megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="32122-138">A dialog is used to display the user ID once authenticated.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="32122-139">Ha eltérő Facebook identitásszolgáltató használ, módosítsa az értéket, átadott **LoginAsync** fent a következőhöz: *MicrosoftAccount*, *Twitter*, *Google*, vagy *WindowsAzureActiveDirectory*.</span><span class="sxs-lookup"><span data-stu-id="32122-139">If you are using an identity provider other than Facebook, change the value passed to **LoginAsync** above to one of the following: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.</span></span>
   > 
   > 
2. <span data-ttu-id="32122-140">Az a **OnCreate** metódus, törlése vagy a következő kódsort a Megjegyzés kibővített:</span><span class="sxs-lookup"><span data-stu-id="32122-140">In the **OnCreate** method, delete or comment-out the following line of code:</span></span>
   
        OnRefreshItemsSelected ();
3. <span data-ttu-id="32122-141">A Activity_To_Do.axml fájlban adja hozzá a következő *LoginUser* -definíciót, mielőtt a meglévő gomb *additem metódust* gombra:</span><span class="sxs-lookup"><span data-stu-id="32122-141">In the Activity_To_Do.axml file, add the following *LoginUser* button definition before the existing *AddItem* button:</span></span>
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. <span data-ttu-id="32122-142">A következő elem hozzáadása a Strings.xml erőforrások fájlhoz:</span><span class="sxs-lookup"><span data-stu-id="32122-142">Add the following element to the Strings.xml resources file:</span></span>
   
        <string name="login_button_text">Sign in</string>
5. <span data-ttu-id="32122-143">Nyissa meg az AndroidManifest.xml fájlhoz, adja hozzá a következő kódot `<application>` XML-elem:</span><span class="sxs-lookup"><span data-stu-id="32122-143">Open the AndroidManifest.xml file, add the following code inside `<application>` XML element:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. <span data-ttu-id="32122-144">A Visual Studio és Xamarin Studio a ügyfélprojekt futtatnak egy eszközt vagy emulátort, és jelentkezzen be a választott identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="32122-144">In Visual Studio or Xamarin Studio, run the client project on a device or emulator and sign in with your chosen identity provider.</span></span> <span data-ttu-id="32122-145">Amikor sikeresen bejelentkezett, az alkalmazás megjelenik a bejelentkezési azonosító és a teendőlista elemeinek listáját, és frissíthetik az adatokat.</span><span class="sxs-lookup"><span data-stu-id="32122-145">When you are successfully logged-in, the app will display your login ID and the list of todo items, and you can make updates to the data.</span></span>

<!-- URLs. -->
<span data-ttu-id="32122-146">[Xamarin.Android-alkalmazás létrehozása]: app-service-mobile-xamarin-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="32122-146">[Create a Xamarin.Android app]: app-service-mobile-xamarin-android-get-started.md</span></span>