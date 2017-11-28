---
title: "a Xamarin Android Mobile Apps hitelesítéssel elindítva aaaGet"
description: "Megtudhatja, hogyan toouse Mobile Apps tooauthenticate felhasználók az identitás-szolgáltatóktól, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos Xamarin Android-alkalmazás."
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
ms.openlocfilehash: 500a4efa816e4f6d75d359e31d6357da56a72f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinandroid-app"></a><span data-ttu-id="98d5c-103">Hitelesítési tooyour Xamarin.Android-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="98d5c-103">Add authentication tooyour Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="98d5c-104">Ez a témakör bemutatja, hogyan egy mobilalkalmazás az ügyfélalkalmazás tooauthenticate felhasználója.</span><span class="sxs-lookup"><span data-stu-id="98d5c-104">This topic shows you how tooauthenticate users of a Mobile App from your client application.</span></span> <span data-ttu-id="98d5c-105">Ebben az oktatóanyagban hozzáadja hitelesítési toohello gyorsútmutató-projekt Azure Mobile Apps által támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="98d5c-105">In this tutorial, you add authentication toohello quickstart project using an identity provider that is supported by Azure Mobile Apps.</span></span> <span data-ttu-id="98d5c-106">Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps hello, hello felhasználói azonosító érték jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98d5c-106">After being successfully authenticated and authorized in hello Mobile App, hello user ID value is displayed.</span></span>

<span data-ttu-id="98d5c-107">Ez az oktatóanyag hello mobilalkalmazás gyors üzembe helyezés alapul.</span><span class="sxs-lookup"><span data-stu-id="98d5c-107">This tutorial is based on hello Mobile App quickstart.</span></span> <span data-ttu-id="98d5c-108">Először is el kell végeznie hello oktatóanyag [Xamarin.Android-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="98d5c-108">You must also first complete hello tutorial [Create a Xamarin.Android app].</span></span> <span data-ttu-id="98d5c-109">Ha nem használ hello letöltése – első lépések, hello hitelesítési kiegészítő csomag tooyour projekt hozzá kell adnia.</span><span class="sxs-lookup"><span data-stu-id="98d5c-109">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="98d5c-110">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="98d5c-110">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <span data-ttu-id="98d5c-111"><a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="98d5c-111"><a name="register"></a>Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="98d5c-112"><a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="98d5c-112"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="98d5c-113">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="98d5c-113">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="98d5c-114">Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="98d5c-114">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="98d5c-115">Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="98d5c-115">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="98d5c-116">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="98d5c-116">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="98d5c-117">Egyedi tooyour mobilalkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="98d5c-117">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="98d5c-118">tooenable hello átirányítási hello kiszolgáló oldalán:</span><span class="sxs-lookup"><span data-stu-id="98d5c-118">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="98d5c-119">Hello [Azure-portálon] válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="98d5c-119">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="98d5c-120">Kattintson a hello **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="98d5c-120">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="98d5c-121">A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="98d5c-121">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="98d5c-122">Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="98d5c-122">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="98d5c-123">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="98d5c-123">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="98d5c-124">Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="98d5c-124">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="98d5c-125">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="98d5c-125">Click **OK**.</span></span>

5. <span data-ttu-id="98d5c-126">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="98d5c-126">Click **Save**.</span></span>

## <span data-ttu-id="98d5c-127"><a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="98d5c-127"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="98d5c-128">A Visual Studio és Xamarin Studio futtatható hello ügyfélprojekt egy eszközt vagy emulátort.</span><span class="sxs-lookup"><span data-stu-id="98d5c-128">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator.</span></span> <span data-ttu-id="98d5c-129">Győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98d5c-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="98d5c-130">Ez akkor fordul elő, mert hello app kísérletek tooaccess a Mobile Apps-háttéralkalmazás nem hitelesített felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="98d5c-130">This happens because hello app attempts tooaccess your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="98d5c-131">Hello *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="98d5c-131">hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="98d5c-132">A következő hello ügyfél app toorequest erőforrások a Mobile Apps-háttéralkalmazás hello frissíti a hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="98d5c-132">Next, you will update hello client app toorequest resources from hello Mobile App backend with an authenticated user.</span></span>

## <span data-ttu-id="98d5c-133"><a name="add-authentication"></a>Hitelesítési toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="98d5c-133"><a name="add-authentication"></a>Add authentication toohello app</span></span>
<span data-ttu-id="98d5c-134">hello app frissített toorequire felhasználók tootap hello **bejelentkezés** gombra, és hitelesíti a, mielőtt adatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="98d5c-134">hello app is updated toorequire users tootap hello **Sign in** button and authenticate before data is displayed.</span></span>

1. <span data-ttu-id="98d5c-135">Adja hozzá a következő kód toohello hello **TodoActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="98d5c-135">Add hello following code toohello **TodoActivity** class:</span></span>
   
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
                //Hide hello button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load hello data.
                OnRefreshItemsSelected();
            }
        }
   
    <span data-ttu-id="98d5c-136">Ezzel létrehoz egy új módszer tooauthenticate egy felhasználó és a metódus kezelő új **bejelentkezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="98d5c-136">This creates a new method tooauthenticate a user and a method handler for a new **Sign in** button.</span></span> <span data-ttu-id="98d5c-137">a fenti hello példakódot hello felhasználói Facebook-bejelentkezés használatával hitelesítve van.</span><span class="sxs-lookup"><span data-stu-id="98d5c-137">hello user in hello example code above is authenticated by using a Facebook login.</span></span> <span data-ttu-id="98d5c-138">A párbeszédablak használt toodisplay hello Felhasználóazonosító egyszer hitelesítve.</span><span class="sxs-lookup"><span data-stu-id="98d5c-138">A dialog is used toodisplay hello user ID once authenticated.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="98d5c-139">Ha eltérő Facebook identitásszolgáltató használ, módosítsa hello átadott túl**LoginAsync** tooone hello következő fent: *MicrosoftAccount*, *Twitter*, *Google*, vagy *WindowsAzureActiveDirectory*.</span><span class="sxs-lookup"><span data-stu-id="98d5c-139">If you are using an identity provider other than Facebook, change hello value passed too**LoginAsync** above tooone of hello following: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.</span></span>
   > 
   > 
2. <span data-ttu-id="98d5c-140">A hello **OnCreate** metódus, a következő kódsort Megjegyzés kibővített hello vagy törlése:</span><span class="sxs-lookup"><span data-stu-id="98d5c-140">In hello **OnCreate** method, delete or comment-out hello following line of code:</span></span>
   
        OnRefreshItemsSelected ();
3. <span data-ttu-id="98d5c-141">Hello Activity_To_Do.axml fájlban adja hozzá a hello következő *LoginUser* -definíciót, mielőtt hello meglévő gomb *additem metódust* gombra:</span><span class="sxs-lookup"><span data-stu-id="98d5c-141">In hello Activity_To_Do.axml file, add hello following *LoginUser* button definition before hello existing *AddItem* button:</span></span>
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. <span data-ttu-id="98d5c-142">Adja hozzá a következő elem toohello Strings.xml erőforrások fájl hello:</span><span class="sxs-lookup"><span data-stu-id="98d5c-142">Add hello following element toohello Strings.xml resources file:</span></span>
   
        <string name="login_button_text">Sign in</string>
5. <span data-ttu-id="98d5c-143">Nyissa meg a hello AndroidManifest.xml fájlhoz, adja hozzá a következő kódot hello `<application>` XML-elem:</span><span class="sxs-lookup"><span data-stu-id="98d5c-143">Open hello AndroidManifest.xml file, add hello following code inside `<application>` XML element:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. <span data-ttu-id="98d5c-144">A Visual Studio és Xamarin Studio hello ügyfélprojekt futtatnak egy eszközt vagy emulátort, és jelentkezzen be a választott identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="98d5c-144">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator and sign in with your chosen identity provider.</span></span> <span data-ttu-id="98d5c-145">Amikor sikeresen bejelentkezett, hello alkalmazás megjelenik a bejelentkezési Azonosítót, és todo elemeket, és hello listája tehet frissítések toohello adatokat.</span><span class="sxs-lookup"><span data-stu-id="98d5c-145">When you are successfully logged-in, hello app will display your login ID and hello list of todo items, and you can make updates toohello data.</span></span>

<!-- URLs. -->
[Xamarin.Android-alkalmazás létrehozása]: app-service-mobile-xamarin-android-get-started.md