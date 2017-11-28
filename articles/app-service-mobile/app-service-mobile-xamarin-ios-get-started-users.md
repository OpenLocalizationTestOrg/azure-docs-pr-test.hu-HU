---
title: "Xamarin IOS-mobilalkalmazások hitelesítéssel elindítva aaaGet"
description: "Megtudhatja, hogyan toouse Mobile Apps tooauthenticate felhasználói identitás-szolgáltatóktól, beleértve az aad-ben, a Google, a Facebook, a Twitter és a Microsoft számos Xamarin iOS-alkalmazásába."
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
ms.openlocfilehash: 6458e9651b03df61c86b88b11953792e04bfa5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinios-app"></a><span data-ttu-id="ade41-103">Hitelesítési tooyour Xamarin.iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ade41-103">Add authentication tooyour Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="ade41-104">Ez a témakör bemutatja, hogyan egy App Service Mobile Apps az ügyfélalkalmazás tooauthenticate felhasználója.</span><span class="sxs-lookup"><span data-stu-id="ade41-104">This topic shows you how tooauthenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="ade41-105">Ebben az oktatóanyagban hozzáadja hitelesítési toohello Xamarin.iOS gyorsútmutató-projekt App Service által támogatott identitásszolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="ade41-105">In this tutorial, you add authentication toohello Xamarin.iOS quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="ade41-106">Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps által, hello felhasználói azonosító érték jelenik meg, és nem fogja korlátozott képes tooaccess tábla adatai.</span><span class="sxs-lookup"><span data-stu-id="ade41-106">After being successfully authenticated and authorized by your Mobile App, hello user ID value is displayed and you will be able tooaccess restricted table data.</span></span>

<span data-ttu-id="ade41-107">Először el kell végeznie hello oktatóanyag [Xamarin.iOS-alkalmazás létrehozása].</span><span class="sxs-lookup"><span data-stu-id="ade41-107">You must first complete hello tutorial [Create a Xamarin.iOS app].</span></span> <span data-ttu-id="ade41-108">Ha nem használ hello letöltése – első lépések, hello hitelesítési kiegészítő csomag tooyour projekt hozzá kell adnia.</span><span class="sxs-lookup"><span data-stu-id="ade41-108">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="ade41-109">Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ade41-109">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="ade41-110">Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="ade41-110">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-toohello-allowed-external-redirect-urls"></a><span data-ttu-id="ade41-111">A toohello engedélyezett külső átirányítási URL-címek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ade41-111">Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="ade41-112">Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát.</span><span class="sxs-lookup"><span data-stu-id="ade41-112">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="ade41-113">Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="ade41-113">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="ade41-114">Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész.</span><span class="sxs-lookup"><span data-stu-id="ade41-114">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="ade41-115">Bármely választja URL-sémát is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ade41-115">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="ade41-116">Egyedi tooyour mobilalkalmazás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ade41-116">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="ade41-117">tooenable hello átirányítási hello kiszolgáló oldalán:</span><span class="sxs-lookup"><span data-stu-id="ade41-117">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="ade41-118">Hello [Azure-portálon] válassza ki az App Service.</span><span class="sxs-lookup"><span data-stu-id="ade41-118">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="ade41-119">Kattintson a hello **hitelesítési / engedélyezési** menüjét.</span><span class="sxs-lookup"><span data-stu-id="ade41-119">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="ade41-120">A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="ade41-120">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="ade41-121">Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ade41-121">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="ade41-122">Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.</span><span class="sxs-lookup"><span data-stu-id="ade41-122">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="ade41-123">Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ade41-123">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="ade41-124">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ade41-124">Click **OK**.</span></span>

5. <span data-ttu-id="ade41-125">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ade41-125">Click **Save**.</span></span>

## <a name="restrict-permissions-tooauthenticated-users"></a><span data-ttu-id="ade41-126">Engedélyek tooauthenticated felhasználók korlátozása</span><span class="sxs-lookup"><span data-stu-id="ade41-126">Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="ade41-127">&nbsp;&nbsp;4.</span><span class="sxs-lookup"><span data-stu-id="ade41-127">&nbsp;&nbsp;4.</span></span> <span data-ttu-id="ade41-128">A Visual Studio és Xamarin Studio futtatható hello ügyfélprojekt egy eszközt vagy emulátort.</span><span class="sxs-lookup"><span data-stu-id="ade41-128">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator.</span></span> <span data-ttu-id="ade41-129">Győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ade41-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="ade41-130">hello hibája hello hibakereső naplózott toohello konzolja.</span><span class="sxs-lookup"><span data-stu-id="ade41-130">hello failure is logged toohello console of hello debugger.</span></span> <span data-ttu-id="ade41-131">Ezért a Visual Studio, megtekintheti az hello hiba hello kimeneti ablakban.</span><span class="sxs-lookup"><span data-stu-id="ade41-131">So in Visual Studio, you should see hello failure in hello output window.</span></span>

<span data-ttu-id="ade41-132">&nbsp;&nbsp;Ez nem engedélyezett a hiba oka az, hogy hello app kísérletek tooaccess a Mobile Apps-háttéralkalmazás nem hitelesített felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="ade41-132">&nbsp;&nbsp;This unauthorized failure happens because hello app attempts tooaccess your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="ade41-133">Hello *TodoItem* tábla most hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="ade41-133">hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="ade41-134">A következő hello ügyfél app toorequest erőforrások a Mobile Apps-háttéralkalmazás hello frissíti a hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="ade41-134">Next, you will update hello client app toorequest resources from hello Mobile App backend with an authenticated user.</span></span>

## <a name="add-authentication-toohello-app"></a><span data-ttu-id="ade41-135">Hitelesítési toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ade41-135">Add authentication toohello app</span></span>
<span data-ttu-id="ade41-136">Ebben a szakaszban adatok megjelenítése előtt hello app toodisplay bejelentkezési képernyőt fog módosítani.</span><span class="sxs-lookup"><span data-stu-id="ade41-136">In this section, you will modify hello app toodisplay a login screen before displaying data.</span></span> <span data-ttu-id="ade41-137">Hello alkalmazás indításakor tooyour App Service nem nem kapcsolódnak, és nem jelenik meg az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ade41-137">When hello app starts, it will not not connect tooyour App Service and will not display any data.</span></span> <span data-ttu-id="ade41-138">Után első alkalommal hello hello felhasználó hajt végre hello frissítési kézmozdulatának hello bejelentkezési képernyő jelenik meg; sikeres bejelentkezés után hello todo elemeket listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ade41-138">After hello first time that hello user performs hello refresh gesture, hello login screen will appear; after successful login hello list of todo items will be displayed.</span></span>

1. <span data-ttu-id="ade41-139">Hello ügyfél projektben nyissa meg a hello fájlt **QSTodoService.cs** és adja hozzá a következő hello utasítás használatával és `MobileServiceUser` az elérő toohello QSTodoService osztály:</span><span class="sxs-lookup"><span data-stu-id="ade41-139">In hello client project, open hello file **QSTodoService.cs** and add hello following using statement and `MobileServiceUser` with accessor toohello QSTodoService class:</span></span>
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. <span data-ttu-id="ade41-140">Adja hozzá nevű új metódust **hitelesítés** túl**QSTodoService** a definícióját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ade41-140">Add new method named **Authenticate** too**QSTodoService** with hello following definition:</span></span>

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

    >[AZURE.NOTE] <span data-ttu-id="ade41-141">Ha a Facebook-on kívül identitásszolgáltató használ, módosítsa hello értéket kapott túl**LoginAsync** tooone hello következő fent: _MicrosoftAccount_, _Twitter_, _Google_, vagy _WindowsAzureActiveDirectory_.</span><span class="sxs-lookup"><span data-stu-id="ade41-141">If you are using an identity provider other than a Facebook, change hello value passed too**LoginAsync** above tooone of hello following: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.</span></span>

3. <span data-ttu-id="ade41-142">Nyissa meg **QSTodoListViewController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ade41-142">Open **QSTodoListViewController.cs**.</span></span> <span data-ttu-id="ade41-143">Hello metódus definícióját módosítása **ViewDidLoad** hello hívás túl eltávolítása**RefreshAsync()** hello end közelében:</span><span class="sxs-lookup"><span data-stu-id="ade41-143">Modify hello method definition of **ViewDidLoad** removing hello call too**RefreshAsync()** near hello end:</span></span>
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out hello call tooRefreshAsync
            // await RefreshAsync();
        }
4. <span data-ttu-id="ade41-144">Hello módszer módosításához **RefreshAsync** tooauthenticate Ha hello **felhasználói** tulajdonsága null értékű.</span><span class="sxs-lookup"><span data-stu-id="ade41-144">Modify hello method **RefreshAsync** tooauthenticate if hello **User** property is null.</span></span> <span data-ttu-id="ade41-145">Adja hozzá a következő kód hello metódusdefiníciót hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="ade41-145">Add hello following code at hello top of hello method definition:</span></span>
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. <span data-ttu-id="ade41-146">Nyissa meg **AppDelegate.cs**, adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="ade41-146">Open **AppDelegate.cs**, add hello following method:</span></span>

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. <span data-ttu-id="ade41-147">Nyissa meg **Info.plist** fájlt, keresse meg a túl**URL-cím típusú** a hello **speciális** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ade41-147">Open **Info.plist** file, navigate too**URL Types** in hello **Advanced** section.</span></span> <span data-ttu-id="ade41-148">Mostantól konfigurálhatja az hello **azonosító** és hello **URL-sémákat** a URL-típust, és kattintson a **URL-típus hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ade41-148">Now configure hello **Identifier** and hello **URL Schemes** of your URL Type and click **Add URL Type**.</span></span> <span data-ttu-id="ade41-149">**URL-sémákat** hello ugyanaz, mint a {url_scheme_of_your_app} kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ade41-149">**URL Schemes** should be hello same as your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="ade41-150">A Visual Studio és Xamarin Studio tooyour Xamarin Build állomás Macintosh, futtassa a célcsoport-kezelési egy eszközt vagy emulátort hello ügyfélprojekt csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="ade41-150">In Visual Studio or Xamarin Studio connected tooyour Xamarin Build Host on your Mac, run hello client project targeting a device or emulator.</span></span> <span data-ttu-id="ade41-151">Ellenőrizze, hogy hello alkalmazáshoz nincs adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ade41-151">Verify that hello app displays no data.</span></span>
   
    <span data-ttu-id="ade41-152">Hajtsa végre a hello frissítési kézmozdulat modulba húzza lefelé elemek, aminek következtében hello bejelentkezési képernyő tooappear hello listáját.</span><span class="sxs-lookup"><span data-stu-id="ade41-152">Perform hello refresh gesture by pulling down hello list of items, which will cause hello login screen tooappear.</span></span> <span data-ttu-id="ade41-153">Miután sikeresen megadta az érvényes hitelesítő adatokat, hello app todo elemeket hello listáját jeleníti meg, és a frissítések toohello adatok végezheti el.</span><span class="sxs-lookup"><span data-stu-id="ade41-153">Once you have successfully entered valid credentials, hello app will display hello list of todo items, and you can make updates toohello data.</span></span>

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Xamarin.iOS-alkalmazás létrehozása]: app-service-mobile-xamarin-ios-get-started.md