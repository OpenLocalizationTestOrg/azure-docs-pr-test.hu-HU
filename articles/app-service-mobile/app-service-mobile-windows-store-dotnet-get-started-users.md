---
title: "aaaAdd authentication tooyour univerzális Windows Platform (UWP-) alkalmazás |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure App Service Mobile Apps tooauthenticate Identitásszolgáltatók, beleértve a különböző univerzális Windows Platform (UWP) alkalmazása felhasználóit: aad-ben, a Google, a Facebook, a Twitter és a Microsoft."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: ad4477e9509f1c40c33e71818e268f6857fe1e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-windows-app"></a>Hitelesítési tooyour Windows-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ez a témakör bemutatja, hogyan tooadd felhőalapú hitelesítés tooyour mobilalkalmazás. Ebben az oktatóanyagban az Azure App Service által támogatott identitásszolgáltató használatával Mobile Apps adja hozzá hitelesítési toohello univerzális Windows Platform (UWP) gyorsútmutató-projekt. Sikeresen alatt hitelesítése és engedélyezése által a mobil-háttéralkalmazást, után hello felhasználói azonosító érték jelenik meg.

Ez az oktatóanyag hello Mobile Apps gyors üzembe helyezés alapul. Először el kell végeznie hello oktatóanyag [Ismerkedés a Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és az App Service hello konfigurálása
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>A toohello engedélyezett külső átirányítási URL-címek hozzáadása

Biztonságos hitelesítéshez az szükséges, hogy az alkalmazás adja meg egy új URL-sémát. Ez lehetővé teszi, hogy hello authentication rendszer tooredirect hátsó tooyour alkalmazás hello hitelesítési folyamat befejezése után. Ebben az oktatóanyagban hello URL-séma használjuk _appname_ egész. Bármely választja URL-sémát is használhatja. Egyedi tooyour mobilalkalmazás kell lennie. tooenable hello átirányítási hello kiszolgáló oldalán:

1. Hello [Azure-portálon] válassza ki az App Service.

2. Kattintson a hello **hitelesítési / engedélyezési** menüjét.

3. A hello **engedélyezett külső átirányítási URL-t**, adja meg `url_scheme_of_your_app://easyauth.callback`.  Hello **url_scheme_of_your_app** hello URL-sémát a mobilalkalmazás Ez a karakterlánc.  Akkor érdemes követnie normál URL-cím meghatározása (használata betűket és számokat csak, és betűvel kezdődik) protokoll.  Meg kell győződnie, jegyezze fel az Ön által mivel kell tooadjust hello több helyen URL-sémát a mobilalkalmazás kódot hello karakterlánc.

4. Kattintson az **OK** gombra.

5. Kattintson a **Save** (Mentés) gombra.

## <a name="permissions"></a>Engedélyek tooauthenticated felhasználók korlátozása
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Most ellenőrizheti, hogy a névtelen hozzáférés tooyour háttér le van tiltva. Hello UWP alkalmazás projekt esetében állítsa be indítási projektként hello telepítheti és futtathatja hello alkalmazást; Győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg. Ez akkor fordul elő, mert megpróbál tooaccess a Mobile App kód, nem hitelesített felhasználónak hello alkalmazás, de hello *TodoItem* tábla most hitelesítést igényel.

Hello app tooauthenticate felhasználók frissíti a következő erőforrások kér az App Service előtt.

## <a name="add-authentication"></a>Hitelesítési toohello alkalmazás hozzáadása
1. Hello UWP-alkalmazásprojektet a MainPage.xaml.cs fájlban, és adja hozzá a következő kódrészletet hello:
   
        // Define a member variable for storing hello signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs hello authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' toohello name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    Ezt a kódot a Facebook-bejelentkezéskor hello felhasználó hitelesíti. Ha használja az identitásszolgáltató Facebook-on kívül, hello értékének módosítása **MobileServiceAuthenticationProvider** a szolgáltató toohello érték felett.
2. Cserélje le a hello **OnNavigatedTo()** MainPage.xaml.cs metódust. Ezután hozzáadhat egy **bejelentkezés** gomb toohello alkalmazás, amely elindítja a hitelesítést.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. Adja hozzá a következő kód részlet toohello MainPage.xaml.cs hello:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login hello user and then load data from hello mobile app.
            if (await AuthenticateAsync())
            {
                // Switch hello buttons and load items from hello mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. Nyissa meg a hello MainPage.xaml projektfájlt, keresse meg, amely meghatározza a hello hello elem **mentése** gombra, és cserélje le a következő kód hello:
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. Adja hozzá a következő kód részlet toohello App.xaml.cs hello:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. Nyissa meg Package.appxmanifest fájlt, keresse meg a túl**nyilatkozatok**, a **elérhető nyilatkozatok** legördülő listában válassza ki **protokoll** kattintson **hozzáadása** gombra. Mostantól konfigurálhatja az hello **tulajdonságok** a hello **protokoll** nyilatkozatot. A **megjelenített név**, adja hozzá az alkalmazás toodisplay toousers kívánja hello nevét. A **neve**, vegye fel a {url_scheme_of_your_app}.
7. Nyomja le az ENTER hello F5 kulcs toorun hello app, kattintson a hello **bejelentkezés** gombra, és jelentkezzen be a választott identitásszolgáltató hello az alkalmazásban. Miután a bejelentkezés sikeres, hello app hibák nélkül fut, és képes tooquery háttéralkalmazásának, és gondoskodjon a frissítések toodata.

## <a name="tokens"></a>Tárolja a hello hitelesítésére szolgáló jogkivonat hello ügyfél
hello előző példa mindkét hello identitásszolgáltató bemutatta, egy szabványos bejelentkezéshez, ami hello ügyfél toocontact igényel, és az App Service hello hello alkalmazás minden indításakor. Nem csak nem hatékony, futtathatja az ezzel a módszerrel történő használat-konfigurációelemmel kapcsolatos problémák toostart végezzen sok ügyfél app: hello ugyanannyi időt vesz igénybe. A jobb megoldás, toocache hello engedélyezési jogkivonat által visszaadott az App Service és a try toouse ez használata előtt a szolgáltató alapú bejelentkezés.

> [!NOTE]
> Használ-e ügyfél által felügyelt vagy a szolgáltatás által kezelt hitelesítési függetlenül alkalmazásszolgáltatások által kibocsátott hello token képes gyorsítótárazni. Ez az oktatóanyag a szolgáltatás által kezelt hitelesítést használ.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy elvégezte az oktatóanyag az egyszerű hitelesítés, vegye figyelembe az alábbi oktatóanyagok hello tooone a Folytatás:

* [Leküldéses értesítések tooyour alkalmazás hozzáadása](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a mobilalkalmazás háttér toouse Azure Notification Hubs toosend leküldéses értesítések konfigurálása.
* [Az offline szinkronizálás engedélyezése az alkalmazás számára](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Ismerje meg, hogyan támogatják a kapcsolat nélküli tooadd a az alkalmazás egy Mobile Apps-háttéralkalmazás segítségével. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók toointeract mobilalkalmazást&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
