---
title: "Első lépések AD Windows Phone aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild egy Windows Phone-alkalmazás, amely integrálható a bejelentkezéshez az Azure AD és az Azure AD védett OAuth protokollt használó API-k."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e766bfcdfae10483772154f4b5facdec05fc846f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Azure AD integrálása a Windows Phone-alkalmazás
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> A Windows Phone 8.1-es és korábbi verziójú projektek nem támogatottak a Visual Studio 2017-ben.  További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Ha a Windows Phone 8.1-alkalmazást, az Azure AD teszi egyszerű és magától értetődő, az Ön tooauthenticate a felhasználókat az Active Directory-fiókokat.  Emellett lehetővé teszi az alkalmazás toosecurely használatához minden webes API-t az Azure AD által védett, például Office 365 API-k hello vagy hello Azure API.

> [!NOTE]
> A fenti ADAL v2.0 használja.  Hello legújabb technológiát, célszerű lehet tooinstead próbálkozzon a [Windows Universal oktatóanyagot ADAL v3.0 használatával](active-directory-devquickstarts-windowsstore.md).  Ha valóban egy alkalmazást a Windows Phone 8.1-es, ez a hello megfelelő hely.  ADAL v2.0 továbbra is teljes mértékben támogatja, és hello fejlesztését alkalmazások agianst Windows Phone 8.1-es ajánlott módja használja az Azure AD.
> 
> 

A .NET natív ügyfelek számára, amelyek tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library vagy adal-t biztosít.  ADAL meg azzal a céllal életben toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok.  toodemonstrate milyen könnyű van, itt azt fogja létrehozása egy "Directory keresővel" Windows Phone 8.1-alkalmazást, amely:

* Lekérdezi hozzáférési jogkivonatainak hello segítségével a hello Azure AD Graph API felület meghívásakor [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Megkeresi a megadott egyszerű felhasználónév a felhasználók számára.
* Kimenő felhasználók jeleket.

toobuild hello teljes működő alkalmazást kell:

1. Az alkalmazás regisztrálása az Azure ad-val.
2. Telepítése és konfigurálása adal-t.
3. Használja az Azure AD ADAL tooget jogkivonatokat.

elindult, tooget [töltse le a projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Egy Visual Studio 2013 megoldást.  Biztosítani kell, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő.  Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

## <a name="1-register-hello-directory-searcher-application"></a>1. Hello Directory keresővel alkalmazás regisztrálása
tooenable a tooget alkalmazási jogkivonatok, először tooregister azt az Azure ad bérlői, és biztosítson számára engedély tooaccess hello Azure AD Graph API:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.
4. Kattintson a **App regisztrációk** válassza **Hozzáadás**.
5. Hello utasításokat követve, és hozzon létre egy új **natív ügyfélalkalmazás**.
  * Hello **neve** hello az alkalmazás ismerteti, az alkalmazás tooend-felhasználók
  * Hello **átirányítási Uri-** protokollt és a karakterlánc kombinációját, hogy az Azure AD tooreturn token válaszokat fogja használni.  Például a lépést, adjon meg egy helyőrzőt értéket `http://DirectorySearcher`.  Később lesz lecserélni ezt az értéket.
6. Miután végrehajtotta a regisztráció, AAD fogja hozzárendelni az alkalmazás egy egyedi azonosítót.  Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.
7. A hello **beállítások** lapon, válassza ki **szükséges engedélyek** válassza **Hozzáadás**. Jelölje be hello **Microsoft Graph** , hello API, és adja hozzá a hello **címtáradatok olvasása** engedélyt a **delegált engedélyek**.  Ez lehetővé teszi az alkalmazás tooquery hello Graph API a felhasználók számára.

## <a name="2-install--configure-adal"></a>2. Telepítése és konfigurálása adal-t
Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.  Ahhoz, hogy képes toocommunicate ADAL toobe az Azure AD-val, tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.

* Első lépésként hozzáadása ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Hello DirectorySearcher projektben nyissa meg `MainPage.xaml.cs`.  Cserélje le a hello hello értékek `Config Values` régió tooreflect hello értékei a bemenő hello Azure portálon.  A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.
  * Hello `tenant` hello tartománya, akkor az Azure AD-bérlőn, pl.: contoso.onmicrosoft.com
  * Hello `clientId` hello clientId hello portálról másolta az alkalmazás van.
* A Windows Phone-alkalmazás most kell toodiscover hello visszahívási uri.  Állítson be egy töréspontot a sort a hello `MainPage` módszert:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Hello alkalmazás futtatását, és másolja fenntartott hello értékének `redirectUri` amikor hello töréspont elérte-e.  Az alábbiakhoz hasonlóan

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Vissza a hello **konfigurálása** hello Azure felügyeleti portálon, az alkalmazás lapján cserélje le a hello hello értékének **RedirectUri** ezzel az értékkel.  

## <a name="3-use-adal-tooget-tokens-from-aad"></a>3. Használja az ADAL tooGet jogkivonatok az aad-ben
hello alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja `authContext.AcquireToken(…)`, és ADAL rest hello.  

* első lépés hello tooinitialize van az alkalmazás `AuthenticationContext` -ADAL tartozó elsődleges osztály.  Ez az adott át ADAL hello koordinátát kell toocommunicate az Azure ad-val, és mondja el, hogyan toocache jogkivonatokat.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Keresse meg hello `Search(...)` metódus, amely fog kell meghívni, ha hello felhasználói cliks hello hello alkalmazás felhasználói felületén a "Search" gombra.  Ez a módszer egy GET kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.  Rendelés tooquery hello Graph API-val, a tooinclude egy access_token a hello szükség van, de `Authorization` hello fejlécének kérelem - Ez az adal-t honnan.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try tooget a token without triggering any user prompt.
    // ADAL will check whether hello requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained hello QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* Interaktív hitelesítésre szükség, ha adal-t fogja használni a Windows Phone webes hitelesítés Broker (WAB) és [folytatási modell](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD Bejelentkezés lapon.  Hello felhasználó jelentkezik be, amikor a toopass ADAL hello eredmények hello WAB egymásra kell.  Ez az lehető legegyszerűbb hello végrehajtási `ContinueWebAuthentication` felület:

```C#
// This method is automatically invoked when hello application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass hello authentication interaction results tooADAL, which will
    // conclude hello token acquisition operation and invoke hello callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* Most azt is, hogy idő toouse hello `AuthenticationResult` adott vissza tooyour alkalmazás adal-t.  A hello `QueryGraph(...)` visszahívási, csatolja hello access_token toohello GET kérést hello Authorization fejlécet szerezte be:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add hello access token toohello Authorization Header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* Is használhatja a hello `AuthenticationResult` toodisplay információt az alkalmazásban hello felhasználói objektum. A hello `QueryGraph(...)` metódust, használjon hello eredmény tooshow hello felhasználó azonosítója hello oldalon:

```C#
// Update hello Page UI toorepresent hello signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Végezetül kívül, valamint az alkalmazás ADAL toosign hello felhasználói is használhatja.  Amikor hello felhasználó hello "Kijelentkezés" gombra kattint, szeretnénk, hogy a következő hívás túl hello tooensure`AcquireTokenSilentAsync(...)` sikertelen lesz.  Ez az adal-t, az egyszerű módon, ha hello token gyorsítótár kiürítése:

```C#
private void SignOut()
{
    // Clear session state from hello token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gratulálunk! Ön most Windows Phone-alkalmazás, amely rendelkezik hello képességét tooauthenticate felhasználók, biztonságosan hívja a webes API-k, az OAuth 2.0 verziót használja, és működik alapszintű hello felhasználó adatainak beolvasása.  Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.  Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.  Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.  Hello-alkalmazások bezárása, és futtassa újból.  Figyelje meg, hogyan hello felhasználói munkamenet nem módosulnak.  Jelentkezzen ki, és jelentkezzen be egy másik felhasználó.

ADAL teszi, hogy könnyen tooincorporate mindezeket a funkciókat az alkalmazásba közös identitás.  Azt gondoskodik az összes hello dirty munkahelyi meg - gyorsítótár kezelése, az OAuth-protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a.  Valóban tooknow szüksége egy egyetlen API-hívással `authContext.AcquireToken*(…)`.

Megadott befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként [Itt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Most már továbbléphet tooadditional identitás forgatókönyvek.  Érdemes lehet tootry:

[.NET webes API-k, az Azure ad-vel biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

