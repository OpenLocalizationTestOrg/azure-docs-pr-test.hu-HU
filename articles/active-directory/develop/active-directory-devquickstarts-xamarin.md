---
title: "Első lépések AD Xamarin aaaAzure |} Microsoft Docs"
description: "Xamarin-alkalmazásokat, amelyek a bejelentkezés az Azure AD integrálása, meghívják a OAuth protokollt használó Azure AD-védett API hozhatnak létre."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6a0d189648b7071558ac1cf2b908808668960a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a>Xamarin-alkalmazásokba az Azure AD integrálása
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

A Xamarin mobilalkalmazások írhat a C# futtatható iOS, Android és Windows (mobileszközök és számítógépek). Ha az alkalmazást a Xamarin most felépítése, Azure Active Directory (Azure AD) lehetővé teszi az Azure AD-fiókok felhasználóinak egyszerű tooauthenticate. hello app biztonságosan is használ egyetlen webes API-t az Azure AD, például az Office 365 API-k hello vagy hello Azure API által védett.

A Xamarin-alkalmazásokat, amelyeket tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library (ADAL) biztosít. hello kizárólagos ADAL célja toomake megkönnyítik az alkalmazások tooget hozzáférési jogkivonatok. toodemonstrate milyen egyszerűen van, ez a cikk bemutatja, hogyan toobuild DirectorySearcher alkalmazásokat, amelyek:

* Futtassa az iOS, Android, Windows asztali, Windows Phone és Windows Store.
* Egy egyetlen hordozható osztály könyvtár (PCL) tooauthenticate felhasználók és az Azure AD Graph API hello jogkivonatokhoz.
* Keresse meg a megadott egyszerű felhasználónév a felhasználók számára egy könyvtárat.

## <a name="before-you-get-started"></a>A kezdés előtt
* Töltse le a hello [projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), vagy töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Minden letöltés egy olyan Visual Studio 2013 megoldás.
* Az Azure AD-bérlő toocreate felhasználók és a nyilvántartás hello alkalmazást is kell. Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

Ha készen áll, hello eljárásokat hajtsa végre a következő négy részből áll hello.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>1. lépés: A Xamarin fejlesztői környezet beállítása
Mivel ez az oktatóanyag iOS, Android és Windows projektek tartalmazza, kell mind a Visual Studio és Xamarin. teljes hello folyamat toocreate hello szükséges környezet [állítsa össze, és telepítse a Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) az MSDN Webhelyén. hello utasítások tartalmaznak anyagot további, Xamarin kapcsolatos toolearn tekinthető hello telepítések toobe befejeződött a várakozás közben.

Hello telepítő befejezése után nyissa meg a hello megoldást a Visual Studio. Itt megtalálja hat projektek: öt platform-specifikus projekteket és egy PCL, DirectorySearcher.cs, amely minden platform között meg lesz osztva.

## <a name="step-2-register-hello-directorysearcher-app"></a>2. lépés: Hello DirectorySearcher alkalmazás regisztrálása
tooenable hello alkalmazási tooget jogkivonatok, először tooregister azt az Azure ad bérlői, és adja meg az engedélyt tooaccess hello Azure AD Graph API. Ezt a következőképpen teheti meg:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiókját. Ekkor a hello **Directory** listán, válassza hello tooregister hello alkalmazás kívánt Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. új toocreate **natív ügyfélalkalmazás**, hello utasításokat követve.
  * **Név** hello app toousers ismerteti.
  * **Átirányítási URI** , hogy az Azure AD által használt tooreturn token válaszok protokollt és a karakterlánc kombinációját. Adjon meg egy értéket (például http://DirectorySearcher).
6. Regisztráció befejezését követően az Azure AD hozzárendel hello app egy egyedi azonosítót. Hello érték másolása hello **alkalmazás** lapon, mert később szüksége.
7. A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.
8. Válassza ki **Microsoft Graph** , hello API. A **delegált engedélyek**, vegye fel a hello **címtáradatok olvasása** engedéllyel.  
Ez a művelet lehetővé teszi, hogy a hello app tooquery hello Graph API a felhasználók számára.

## <a name="step-3-install-and-configure-adal"></a>3. lépés: Telepítse és konfigurálja az adal-t
Most, hogy egy alkalmazást az Azure ad-ben, telepítse az adal-t, és az identitás-kapcsolódó kód írása. tooenable ADAL toocommunicate az Azure ad-vel, adjon neki hello app regisztrációs adatait.

1. Adja hozzá az ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    Vegye figyelembe, hogy két függvénytár hivatkozások kerülnek tooeach projekt: hello PCL része adal-t és a platform-specifikus részét.
2. Hello DirectorySearcherLib projektben nyissa meg DirectorySearcher.cs.
3. Cserélje le a hello osztály kombinálását hello Azure-portálon megadott hello értékekkel. Kódja toothese értékek hivatkozik, amikor az adal-t használ.

  * Hello *bérlői* hello tartománya, akkor az Azure AD bérlője (például contoso.onmicrosoft.com).
  * Hello *clientId* hello alkalmazás, amely hello portálról másolt hello ügyfél-azonosító.
  * Hello *returnUri* hello átirányítási URI-t hello portal (például http://DirectorySearcher) megadott értéke.

## <a name="step-4-use-adal-tooget-tokens-from-azure-ad"></a>4. lépés: Használja ADAL tooget jogkivonatok az Azure AD-ből
Szinte teljes egészében hello alkalmazás hitelesítési logikát létrejönnie `DirectorySearcher.SearchByAlias(...)`. Minden szükséges hello platform-specifikus projektben egy környezetfüggő paraméter toohello toopass `DirectorySearcher` PCL.

1. Nyissa meg a DirectorySearcher.cs, és adja hozzá az egy új paraméter toohello `SearchByAlias(...)` metódust. `IPlatformParameters`hello környezetfüggő paraméter, amely magában foglalja a hello platform-specifikus objektumokat, ADAL igényeinek tooperform hello hitelesítés.

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Inicializálni `AuthenticationContext`, vagyis hello elsődleges osztály az adal-t.  
Ez a művelet fázisok ADAL hello koordinálja az igényeinek toocommunicate az Azure ad-val.
3. Hívja `AcquireTokenAsync(...)`, amely elfogadja hello `IPlatformParameters` objektumra, és hívja meg, amely a token toohello alkalmazások szükséges tooreturn hello hitelesítési folyamat.

    ```C#
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)`első kísérlet tooreturn hello egy token kért erőforrás (hello Graph API ebben az esetben) felhasználók tooenter a hitelesítő adataikat (keresztül gyorsítótárazását, vagy frissíteni a régi jogkivonatok) értesítése nélkül. Szükség esetén azt hello kért token beszerzése előtt felhasználók hello az Azure AD bejelentkezési lapját jeleníti meg.
4. Hello access token toohello Graph API kérést hello csatolása **engedélyezési** fejléc:

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Ez minden, a hello `DirectorySearcher` PCL és hello app identitás kapcsolatos kódot. Most már toocall hello `SearchByAlias(...)` metódus minden egyes platform nézetekben és szükség esetén megfelelően kezeli tooadd kódját hello felhasználói felület életciklusát.

### <a name="android"></a>Android
1. A MainActivity.cs, adjon hozzá egy túl`SearchByAlias(...)` hello gombra kattintson kezelő:

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Bírálja felül a hello `OnActivityResult` életciklus metódus tooforward hitelesítés átirányítja a felhasználókat hátsó toohello megfelelő módszert. Adal-t egy segédmetódust nyújt az Android:

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows asztali
A MainWindow.xaml.cs, hívás túl`SearchByAlias(...)` úgy, hogy egy `WindowInteropHelper` hello asztali `PlatformParameters` objektum:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
A DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` objektum szükséges a hivatkozás toohello nézetvezérlő:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
Az univerzális Windows-, nyissa meg a MainPage.xaml.cs, és megvalósításához hello `Search` metódust. Ez a módszer egy segédmetódust a megosztott projekt tooupdate felhasználói felület szükség esetén használja.

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a>A következő lépések
Most már rendelkezik egy működő Xamarin-alkalmazás, amely hitelesíti a felhasználókat, és biztonságosan hívására webes API-k OAuth 2.0 különböző öt különböző platformokon.

Ha már nincs beállítva, feltöltve a felhasználókkal a bérlő, most már olyan hello idő toodo.

1. Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik hello felhasználók.
2. Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.

ADAL segítségével könnyen tooincorporate közös identitás szolgáltatások hello alkalmazásba. Azt gondoskodik összes hello dirty munkahelyi meg, például a gyorsítótár kezelése, OAuth protokoll támogatása, a bejelentkezési felhasználói felület, a hello felhasználói bemutató és jogkivonatok frissítése lejárt. Tooknow csak egy API-hívás, kell `authContext.AcquireToken*(…)`.

Hivatkozás, töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (a konfigurációs értékek nélkül).

Most már továbbléphet tooadditional identitás forgatókönyvek. Például [egy .NET webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
