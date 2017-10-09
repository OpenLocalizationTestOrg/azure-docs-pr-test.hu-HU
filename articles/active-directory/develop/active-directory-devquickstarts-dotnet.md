---
title: "első lépések aaaAzure AD .NET |} Microsoft Docs"
description: "Hogyan toobuild egy .NET Windows asztali alkalmazás, amely integrálható a bejelentkezéshez az Azure AD és az Azure AD védett OAuth protokollt használó API-k."
services: active-directory
documentationcenter: .net
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c09b358f24c7bfb371b34cf72ca48c0a45042f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Az Azure AD integrálása Windows asztali WPF-alkalmazások
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Ha az asztali alkalmazások, az Azure AD teszi egyszerű és magától értetődő, az Ön tooauthenticate a felhasználókat az Active Directory-fiókokat.  Emellett lehetővé teszi az alkalmazás toosecurely használatához minden webes API-t az Azure AD által védett, például Office 365 API-k hello vagy hello Azure API.

A .NET natív ügyfelek számára, amelyek tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library vagy adal-t biztosít.  ADAL meg azzal a céllal életben toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok.  toodemonstrate milyen könnyű van, itt azt kell egy .NET WPF feladatlista alkalmazás létrehozása, amely:

* Lekérdezi hozzáférési jogkivonatainak hello segítségével a hello Azure AD Graph API felület meghívásakor [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Egy könyvtárat a felhasználók egy adott aliasnév keres.
* Kimenő felhasználók jeleket.

toobuild hello teljes működő alkalmazást kell:

1. Az alkalmazás regisztrálása az Azure ad-val.
2. Telepítése és konfigurálása adal-t.
3. Használja az Azure AD ADAL tooget jogkivonatokat.

elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Biztosítani kell, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.  Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

## <a name="1-register-hello-directorysearcher-application"></a>1. Hello DirectorySearcher alkalmazás regisztrálása
tooenable a tooget alkalmazási jogkivonatok, először tooregister azt az Azure ad bérlői, és biztosítson számára engedély tooaccess hello Azure AD Graph API:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.
4. Kattintson a **App regisztrációk** válassza **Hozzáadás**.
5. Hello utasításokat követve, és hozzon létre egy új **natív ügyfélalkalmazás**.
  * Hello **neve** hello az alkalmazás ismerteti, az alkalmazás tooend-felhasználók
  * Hello **átirányítási Uri-** protokollt és a karakterlánc kombinációját, hogy az Azure AD tooreturn token válaszokat fogja használni.  Adjon meg egy értéket adott tooyour alkalmazást, pl. `http://DirectorySearcher`.
6. Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.  Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás oldalról.
7. A hello **beállítások** lapon, válassza ki **szükséges engedélyek** válassza **Hozzáadás**. Jelölje be hello **Microsoft Graph** , hello API, és adja hozzá a hello **címtáradatok olvasása** engedélyt a **delegált engedélyek**.  Ez lehetővé teszi az alkalmazás tooquery hello Graph API a felhasználók számára.

## <a name="2-install--configure-adal"></a>2. Telepítése és konfigurálása adal-t
Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.  Ahhoz, hogy képes toocommunicate ADAL toobe az Azure AD-val, tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.

* Első lépésként hozzáadása ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Hello DirectorySearcher projektben nyissa meg `app.config`.  Cserélje le a hello értékek hello hello elemeinek `<appSettings>` szakasz tooreflect hello értékei a bemenő hello Azure portálon.  A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.
  * Hello `ida:Tenant` hello tartománya, akkor az Azure AD-bérlőn, pl.: contoso.onmicrosoft.com
  * Hello `ida:ClientId` hello clientId hello portálról másolta az alkalmazás van.
  * Hello `ida:RedirectUri` hello van regisztrált hello portál URL-címének átirányítása.

## <a name="3----use-adal-tooget-tokens-from-aad"></a>3.    Használja az ADAL tooGet jogkivonatok az aad-ben
hello alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja `authContext.AcquireTokenAsync(...)`, és ADAL rest hello.  

* A hello `DirectorySearcher` projektben nyissa meg `MainWindow.xaml.cs` , és keresse meg a hello `MainWindow()` metódust.  első lépés hello tooinitialize van az alkalmazás `AuthenticationContext` -ADAL tartozó elsődleges osztály.  Ez az adott át ADAL hello koordinátát kell toocommunicate az Azure ad-val, és mondja el, hogyan toocache jogkivonatokat.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Keresse meg hello `Search(...)` metódus, amely fog kell meghívni, ha hello felhasználói cliks hello hello alkalmazás felhasználói felületén a "Search" gombra.  Ez a módszer egy GET kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.  Rendelés tooquery hello Graph API-val, a tooinclude egy access_token a hello szükség van, de `Authorization` hello fejlécének kérelem - Ez az adal-t honnan.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate hello Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for hello tooDo item name");
        return;
    }

    // Get an Access Token for hello Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled hello sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* Ha az alkalmazás kísérel meg jogkivonat meghívásával `AcquireTokenAsync(...)`, ADAL megpróbál tooreturn jogkivonat hello felhasználói hitelesítő adatok kérése nélkül.  Ha ADAL hello felhasználó van szüksége a tooget jogkivonat toosign, azt fogja megjelenítési bejelentkezési hello felhasználói hitelesítő adatok összegyűjtése, térjen vissza a sikeres hitelesítés esetén egy token.  Ha az adal TÁRAT nem tooreturn jogkivonat bármilyen okból, akkor kivételhibát egy `AdalException`.
* Figyelje meg, hogy hello `AuthenticationResult` objektum tartalmaz egy `UserInfo` objektum, amely lehet például az alkalmazás esetleg használt toocollect információ.  A hello DirectorySearcher `UserInfo` használt toocustomize hello alkalmazás Kezelőfelületén hello felhasználói azonosítóval van.
* Amikor hello felhasználó hello "Kijelentkezés" gombra kattint, szeretnénk, hogy a következő hívás túl hello tooensure`AcquireTokenAsync(...)` hello felhasználói toosign a felhasználói jóváhagyást kér.  Ez az adal-t, az egyszerű módon, ha hello token gyorsítótár kiürítése:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear hello token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Azonban ha hello felhasználói nem hello "Kijelentkezés" gombra kattintva célszerű toomaintain hello felhasználói munkamenetet a hello hello DirectorySearcher következő futtatásakor.  Hello alkalmazás indításakor ADAL által jogkivonatok gyorsítótárát egy meglévő jogkivonat esetében ellenőrizze, és ennek megfelelően frissülnek hello felhasználói felületén.  A hello `CheckForCachedToken()` módszer, egy másik hívás túl`AcquireTokenAsync(...)`, ezúttal hello benyújtása `PromptBehavior.Never` paraméter.  `PromptBehavior.Never`jelzi az adal-t hello felhasználói bejelentkezés nem kéri, és ADAL helyette kivételt kell előidéznie nem tooreturn esetén egy token.

```C#
public async void CheckForCachedToken() 
{
    // As hello application starts, try tooget an access token without prompting hello user.  If one exists, show hello user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed toomain page without singing hello user in.
        return;
    }

    // A valid token is in hello cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Gratulálunk! Ön most .NET WPF-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és működik alapszintű hello felhasználó adatainak beolvasása.  Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.  Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.  Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.  Hello-alkalmazások bezárása, és futtassa újból.  Figyelje meg, hogyan hello felhasználói munkamenet nem módosulnak.  Jelentkezzen ki, és jelentkezzen be egy másik felhasználó.

ADAL teszi, hogy könnyen tooincorporate mindezeket a funkciókat az alkalmazásba közös identitás.  Azt gondoskodik az összes hello dirty munkahelyi meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a.  Valóban tooknow szüksége egy egyetlen API-hívással `authContext.AcquireTokenAsync(...)`.

Megadott befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként [Itt](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Most már továbbléphet tooadditional forgatókönyvek.  Érdemes lehet tootry:

[.NET webes API-k, az Azure ad-vel biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

