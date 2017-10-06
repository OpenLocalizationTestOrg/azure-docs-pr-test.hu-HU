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
# <a name="add-authentication-tooyour-xamarinios-app"></a>Hitelesítési tooyour Xamarin.iOS-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ez a témakör bemutatja, hogyan egy App Service Mobile Apps az ügyfélalkalmazás tooauthenticate felhasználója. Ebben az oktatóanyagban hozzáadja hitelesítési toohello Xamarin.iOS gyorsútmutató-projekt App Service által támogatott identitásszolgáltató használatával. Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps által, hello felhasználói azonosító érték jelenik meg, és nem fogja korlátozott képes tooaccess tábla adatai.

Először el kell végeznie hello oktatóanyag [Xamarin.iOS-alkalmazás létrehozása]. Ha nem használ hello letöltése – első lépések, hello hitelesítési kiegészítő csomag tooyour projekt hozzá kell adnia. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-toohello-allowed-external-redirect-urls"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása

Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát. Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után. Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész. Bármely választja URL-sémát is használhatja. Egyedi tooyour mobilalkalmazás kell lennie. tooenable hello átirányítási hello kiszolgáló oldalán:

1. Hello [Azure-portálon] válassza ki az App Service.

2. Kattintson a hello **hitelesítési / engedélyezési** menüjét.

3. A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.  Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.  Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.

4. Kattintson az **OK** gombra.

5. Kattintson a **Save** (Mentés) gombra.

## <a name="restrict-permissions-tooauthenticated-users"></a>Engedélyek tooauthenticated felhasználók korlátozása
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. A Visual Studio és Xamarin Studio futtatható hello ügyfélprojekt egy eszközt vagy emulátort. Győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg. hello hibája hello hibakereső naplózott toohello konzolja. Ezért a Visual Studio, megtekintheti az hello hiba hello kimeneti ablakban.

&nbsp;&nbsp;Ez nem engedélyezett a hiba oka az, hogy hello app kísérletek tooaccess a Mobile Apps-háttéralkalmazás nem hitelesített felhasználóként. Hello *TodoItem* tábla most hitelesítést igényel.

A következő hello ügyfél app toorequest erőforrások a Mobile Apps-háttéralkalmazás hello frissíti a hitelesített felhasználók.

## <a name="add-authentication-toohello-app"></a>Hitelesítési toohello alkalmazás hozzáadása
Ebben a szakaszban adatok megjelenítése előtt hello app toodisplay bejelentkezési képernyőt fog módosítani. Hello alkalmazás indításakor tooyour App Service nem nem kapcsolódnak, és nem jelenik meg az adatokat. Után első alkalommal hello hello felhasználó hajt végre hello frissítési kézmozdulatának hello bejelentkezési képernyő jelenik meg; sikeres bejelentkezés után hello todo elemeket listája jelenik meg.

1. Hello ügyfél projektben nyissa meg a hello fájlt **QSTodoService.cs** és adja hozzá a következő hello utasítás használatával és `MobileServiceUser` az elérő toohello QSTodoService osztály:
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Adja hozzá nevű új metódust **hitelesítés** túl**QSTodoService** a definícióját a következő hello:

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

    >[AZURE.NOTE] Ha a Facebook-on kívül identitásszolgáltató használ, módosítsa hello értéket kapott túl**LoginAsync** tooone hello következő fent: _MicrosoftAccount_, _Twitter_, _Google_, vagy _WindowsAzureActiveDirectory_.

3. Nyissa meg **QSTodoListViewController.cs**. Hello metódus definícióját módosítása **ViewDidLoad** hello hívás túl eltávolítása**RefreshAsync()** hello end közelében:
   
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
4. Hello módszer módosításához **RefreshAsync** tooauthenticate Ha hello **felhasználói** tulajdonsága null értékű. Adja hozzá a következő kód hello metódusdefiníciót hello tetején hello:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. Nyissa meg **AppDelegate.cs**, adja hozzá a következő metódus hello:

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. Nyissa meg **Info.plist** fájlt, keresse meg a túl**URL-cím típusú** a hello **speciális** szakasz. Mostantól konfigurálhatja az hello **azonosító** és hello **URL-sémákat** a URL-típust, és kattintson a **URL-típus hozzáadása**. **URL-sémákat** hello ugyanaz, mint a {url_scheme_of_your_app} kell lennie.
7. A Visual Studio és Xamarin Studio tooyour Xamarin Build állomás Macintosh, futtassa a célcsoport-kezelési egy eszközt vagy emulátort hello ügyfélprojekt csatlakoztatva. Ellenőrizze, hogy hello alkalmazáshoz nincs adatait jeleníti meg.
   
    Hajtsa végre a hello frissítési kézmozdulat modulba húzza lefelé elemek, aminek következtében hello bejelentkezési képernyő tooappear hello listáját. Miután sikeresen megadta az érvényes hitelesítő adatokat, hello app todo elemeket hello listáját jeleníti meg, és a frissítések toohello adatok végezheti el.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Xamarin.iOS-alkalmazás létrehozása]: app-service-mobile-xamarin-ios-get-started.md