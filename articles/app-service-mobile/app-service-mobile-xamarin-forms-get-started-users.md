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
# <a name="add-authentication-tooyour-xamarin-forms-app"></a>Hitelesítési tooyour Xamarin Forms alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan egy App Service Mobile Apps az ügyfélalkalmazás tooauthenticate felhasználója. Ebben az oktatóanyagban hozzáadja hitelesítési hello gyorsútmutató-projekt Xamarin Forms App Service által támogatott identitásszolgáltató használatával. Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps által, a hello felhasználói azonosító értéke megjelenik-e, és korlátozott képes tooaccess adatait fogja.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag legjobb eredmény hello, javasoljuk, hogy az első teljes hello [Xamarin Forms-alkalmazás létrehozása] [ 1] oktatóanyag. Ez az oktatóanyag befejezése után fog egy többplatformos TodoList alkalmazást a Xamarin Forms projektet.

Ha nem használ hello letöltése – első lépések, hello hitelesítési kiegészítő csomag tooyour projekt hozzá kell adnia. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása

Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát. Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után. Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész. Bármely választja URL-sémát is használhatja. Egyedi tooyour mobilalkalmazás kell lennie. tooenable hello átirányítási hello kiszolgáló oldalán:

1. Hello [Azure-portálon] válassza ki az App Service.

2. Kattintson a hello **hitelesítési / engedélyezési** menüjét.

3. A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.  Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.  Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.

4. Kattintson az **OK** gombra.

5. Kattintson a **Save** (Mentés) gombra.

## <a name="restrict-permissions-tooauthenticated-users"></a>Engedélyek tooauthenticated felhasználók korlátozása
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-toohello-portable-class-library"></a>Hitelesítési toohello hordozható osztály könyvtár hozzáadása
Mobile Apps használ hello [LoginAsync] [ 3] hello bővítmény metódusa [MobileServiceClient] [ 4] toosign a felhasználók az App Service hitelesítés. A példa a kiszolgáló által kezelt hitelesítési folyamat, amely megjeleníti hello szolgáltató bejelentkezési felületen hello alkalmazás használja. További információkért lásd: [hitelesítési kiszolgáló által kezelt][5]. Az éles alkalmazásban jobb felhasználói élmény biztosítása érdekében érdemes lehet helyette használatával [hitelesítési ügyfél által felügyelt][6].

a Xamarin Forms-projekt tooauthenticate megadása egy **IAuthenticate** hello hordozható Class Library hello alkalmazás felülettel. Majd adja hozzá a **bejelentkezési** gomb toohello felhasználói felület hello hordozható Class Library, amelyek a meghatározott toostart hitelesítés. Adatok betöltése a mobil-háttéralkalmazás hello sikeres hitelesítés után.

Alkalmazzon hello **IAuthenticate** felületet az alkalmazás által támogatott platformokon.

1. A Visual Studio és Xamarin Studio, nyissa meg az App.cs hello projektből az **hordozható** hello neve, amely hordozható Class Library projektet, majd adja hozzá a következő hello `using` utasítást:

        using System.Threading.Tasks;
2. A App.cs, adja hozzá a hello következő `IAuthenticate` hello előtt közvetlenül felület `App` definition osztályban.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. a platform-specifikus megvalósításának tooinitialize hello illesztőfelület adja hozzá a következő statikus tagok toohello hello **App** osztály.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Nyissa meg a TodoList.xaml hello hordozható Class Library projekt, adja hozzá a következő hello **gomb** hello elemének *buttonsPanel* elrendezés elem hello meglévő gomb után:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Ez a gomb elindítja a hitelesítési kiszolgáló által felügyelt és a mobil-háttéralkalmazás.
5. Nyissa meg a TodoList.xaml.cs hello hordozható Class Library projektet, majd adja hozzá a következő mező toohello hello `TodoList` osztály:

        // Track whether hello user has authenticated.
        bool authenticated = false;
6. Cserélje le a hello **OnAppearing** hello kód a következő metódust:

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

    Ez a kód gondoskodik arról, hogy az adatok csak frissítése hello szolgáltatás az Ön hitelesítése után.
7. Adja hozzá a következő hello kezelője hello **Clicked** esemény toohello **TodoList** osztály:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems tootrue toosynchronize hello data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. A módosítások mentéséhez és a hibák ellenőrzése hello hordozható Class Library projekt újraépítéséhez.

## <a name="add-authentication-toohello-android-app"></a>Hitelesítési toohello Android-alkalmazás hozzáadása
Ez a szakasz bemutatja, hogyan tooimplement hello **IAuthenticate** hello Android-alkalmazás projekt felületet. Ez a szakasz kihagyása, ha Android-eszköz nem támogatja.

1. A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal hello **droid** projektre, majd **beállítás kezdőprojektként**.
2. Nyomja le az F5 toostart hello hello hibakeresőben projektre, majd győződjön meg arról, hogy egy állapotkód: nem kezelt kivétel a 401-es (nem engedélyezett) jelenik meg, az alkalmazás indítása után. hello 401-es kód jön létre, mert a hozzáférés hello háttérkiszolgálón csak korlátozott tooauthorized felhasználók.
3. Nyissa meg a MainActivity.cs hello Android-projekt, és adja hozzá a következő hello `using` utasításokat:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Frissítés hello **MainActivity** osztály tooimplement hello **IAuthenticate** csatoló, az alábbiak szerint:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Frissítés hello **MainActivity** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely hello által igényelt **IAuthenticate**  csatoló, az alábbiak szerint:

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

    Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a [MobileServiceAuthenticationProvider][7].

6. Adja hozzá a következő kódot hello <application> AndroidManifest.xml csomópontján:

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

1. Adja hozzá a következő kód toohello hello **OnCreate** hello metódusában **MainActivity** túl osztály hello hívása előtt`LoadApplication()`:

        // Initialize hello authenticator before loading hello app.
        App.Init((IAuthenticate)this);

    Ez a kód biztosítja hello hitelesítő inicializálása előtt hello alkalmazás terhelés.
2. Hello app újraépítése, majd futtassa, majd hello hitelesítési szolgáltatót választotta, és ellenőrizze, képes tooaccess adatokat egy hitelesített felhasználóként jelentkezzen.

## <a name="add-authentication-toohello-ios-app"></a>Hitelesítési toohello iOS-alkalmazás hozzáadása
Ez a szakasz bemutatja, hogyan tooimplement hello **IAuthenticate** hello iOS-alkalmazás projekt felületet. Ez a szakasz kihagyása, ha nem támogatja a iOS-eszközök.

1. A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal hello **iOS** projektre, majd **beállítás kezdőprojektként**.
2. Nyomja le az F5 toostart hello hello hibakeresőben projektre, majd győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg. hello 401-es válasz jön létre, mert a hozzáférés hello háttérkiszolgálón csak korlátozott tooauthorized felhasználók.
3. Nyissa meg AppDelegate.cs hello iOS-projektre, és adja hozzá a következő hello `using` utasításokat:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Frissítés hello **AppDelegate** osztály tooimplement hello **IAuthenticate** csatoló, az alábbiak szerint:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Frissítés hello **AppDelegate** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely hello által igényelt **IAuthenticate**  csatoló, az alábbiak szerint:

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

    Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a(z) [MobileServiceAuthenticationProvider].

6. Hello AppDelegate osztály OpenUrl (UIApplication alkalmazás által igényelt NSUrl URL-címe, NSDictionary beállítások) metódus túlterhelési hozzáadásával frissítése

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. Adja hozzá a következő kód toohello üzletági hello **FinishedLaunching** előtt hello kiszolgálómetódus-hívás túl`LoadApplication()`:

        App.Init(this);

    Ez a kód biztosítja hello hitelesítő inicializálása előtt hello app be van töltve.

6. Adja hozzá **{url_scheme_of_your_app}** Info.plist tooURL sémákat.

7. Hello app újraépítése, majd futtassa, majd hello hitelesítési szolgáltatót választotta, és ellenőrizze, képes tooaccess adatokat egy hitelesített felhasználóként jelentkezzen.

## <a name="add-authentication-toowindows-10-including-phone-app-projects"></a>Adja hozzá a hitelesítési tooWindows 10 (többek között a következőket telefon) alkalmazás projektek
Ez a szakasz bemutatja, hogyan tooimplement hello **IAuthenticate** felület hello Windows 10-alkalmazást projektekben. Ugyanezek a lépések alkalmazni az univerzális Windows Platform (UWP) projektek, de hello segítségével hello **UWP** project (beállításértékeket módosításokkal). Ez a szakasz kihagyása, ha nem támogatja a Windows-eszközök.

1. "A Visual Studióban, kattintson a jobb gombbal bármelyik hello **UWP** projektre, majd **beállítás kezdőprojektként**.
2. Nyomja le az F5 toostart hello hello hibakeresőben projektre, majd győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg. hello 401-es válasz akkor fordul elő, mert hello háttérkiszolgálón hozzáférés csak korlátozott tooauthorized felhasználók.
3. Nyissa meg a MainPage.xaml.cs hello Windows-alkalmazás projekt, és adja hozzá a következő hello `using` utasításokat:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Cserélje le `<your_Portable_Class_Library_namespace>` a hordozható osztály szalagtár hello névtérrel.
4. Frissítés hello **MainPage** osztály tooimplement hello **IAuthenticate** csatoló, az alábbiak szerint:

        public sealed partial class MainPage : IAuthenticate
5. Frissítés hello **MainPage** hozzáadásával osztály egy **MobileServiceUser** mező és egy **hitelesítés** metódus, amely hello által igényelt **IAuthenticate** csatoló, az alábbiak szerint:

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

    Ha használja az identitásszolgáltató Facebook-on kívül, adjon meg más értéket a(z) [MobileServiceAuthenticationProvider].

1. Adja hozzá a következő kódsort hello hello konstruktora hello **MainPage** túl osztály hello hívása előtt`LoadApplication()`:

        // Initialize hello authenticator before loading hello app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Cserélje le `<your_Portable_Class_Library_namespace>` a hordozható osztály szalagtár hello névtérrel.

3. Ha használ **UWP**, adja hozzá a következő hello **OnActivated** metódus-felülbírálása toohello **App** osztály:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   Ha hello metódus-felülbírálása már létezik, adja hozzá hello feltételes kód részlet megelőző hello.  Ez a kód nincs szükség az univerzális Windows-projektek esetében.

3. Adja hozzá **{url_scheme_of_your_app}** a Package.appxmanifest. 

4. Hello app újraépítése, majd futtassa, majd hello hitelesítési szolgáltatót választotta, és ellenőrizze, képes tooaccess adatokat egy hitelesített felhasználóként jelentkezzen.

## <a name="next-steps"></a>Következő lépések
Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:

* [Leküldéses értesítések tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-forms-get-started-push.md)

  Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a mobilalkalmazás háttér toouse Azure Notification Hubs toosend leküldéses értesítések konfigurálása.
* [Az offline szinkronizálás engedélyezése az alkalmazás számára](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Ismerje meg, hogyan tooadd offline támogatják az alkalmazást a Mobile Apps-háttéralkalmazás segítségével. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a befejező felhasználóknak toointeract mobilalkalmazást - megtekintését, hozzáadását és - adatok módosítását, akkor is, ha nincs hálózati kapcsolat.

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
