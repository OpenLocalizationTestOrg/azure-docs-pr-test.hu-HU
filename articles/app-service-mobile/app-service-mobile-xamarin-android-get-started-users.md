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
# <a name="add-authentication-tooyour-xamarinandroid-app"></a>Hitelesítési tooyour Xamarin.Android-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ez a témakör bemutatja, hogyan egy mobilalkalmazás az ügyfélalkalmazás tooauthenticate felhasználója. Ebben az oktatóanyagban hozzáadja hitelesítési toohello gyorsútmutató-projekt Azure Mobile Apps által támogatott identitásszolgáltató használatával. Miután sikeresen alatt hitelesítése és engedélyezése a Mobile Apps hello, hello felhasználói azonosító érték jelenik meg.

Ez az oktatóanyag hello mobilalkalmazás gyors üzembe helyezés alapul. Először is el kell végeznie hello oktatóanyag [Xamarin.Android-alkalmazás létrehozása]. Ha nem használ hello letöltése – első lépések, hello hitelesítési kiegészítő csomag tooyour projekt hozzá kell adnia. Kiszolgáló bővítménycsomagok kapcsolatos további információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Regisztrálja az alkalmazást a hitelesítéshez, és konfigurálja az App Service szolgáltatások
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

A Visual Studio és Xamarin Studio futtatható hello ügyfélprojekt egy eszközt vagy emulátort. Győződjön meg arról, hogy a 401-es (nem engedélyezett) egy állapotkód: nem kezelt kivétel hello alkalmazás indítása után jelenik meg. Ez akkor fordul elő, mert hello app kísérletek tooaccess a Mobile Apps-háttéralkalmazás nem hitelesített felhasználóként. Hello *TodoItem* tábla most hitelesítést igényel.

A következő hello ügyfél app toorequest erőforrások a Mobile Apps-háttéralkalmazás hello frissíti a hitelesített felhasználók.

## <a name="add-authentication"></a>Hitelesítési toohello alkalmazás hozzáadása
hello app frissített toorequire felhasználók tootap hello **bejelentkezés** gombra, és hitelesíti a, mielőtt adatok jelennek meg.

1. Adja hozzá a következő kód toohello hello **TodoActivity** osztály:
   
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
   
    Ezzel létrehoz egy új módszer tooauthenticate egy felhasználó és a metódus kezelő új **bejelentkezés** gombra. a fenti hello példakódot hello felhasználói Facebook-bejelentkezés használatával hitelesítve van. A párbeszédablak használt toodisplay hello Felhasználóazonosító egyszer hitelesítve.
   
   > [!NOTE]
   > Ha eltérő Facebook identitásszolgáltató használ, módosítsa hello átadott túl**LoginAsync** tooone hello következő fent: *MicrosoftAccount*, *Twitter*, *Google*, vagy *WindowsAzureActiveDirectory*.
   > 
   > 
2. A hello **OnCreate** metódus, a következő kódsort Megjegyzés kibővített hello vagy törlése:
   
        OnRefreshItemsSelected ();
3. Hello Activity_To_Do.axml fájlban adja hozzá a hello következő *LoginUser* -definíciót, mielőtt hello meglévő gomb *additem metódust* gombra:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Adja hozzá a következő elem toohello Strings.xml erőforrások fájl hello:
   
        <string name="login_button_text">Sign in</string>
5. Nyissa meg a hello AndroidManifest.xml fájlhoz, adja hozzá a következő kódot hello `<application>` XML-elem:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. A Visual Studio és Xamarin Studio hello ügyfélprojekt futtatnak egy eszközt vagy emulátort, és jelentkezzen be a választott identitásszolgáltató. Amikor sikeresen bejelentkezett, hello alkalmazás megjelenik a bejelentkezési Azonosítót, és todo elemeket, és hello listája tehet frissítések toohello adatokat.

<!-- URLs. -->
[Xamarin.Android-alkalmazás létrehozása]: app-service-mobile-xamarin-android-get-started.md