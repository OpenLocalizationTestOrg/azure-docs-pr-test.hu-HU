---
title: "aaaAzure AD a Windows Store bevezetés |} Microsoft Docs"
description: "Build Windows Áruházbeli alkalmazások, amelyek a bejelentkezés az Azure AD integrálása, meghívják a Azure AD API-k OAuth protokollt használó védett."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1d12c7b928bc0e94fb823f8db4a09ff416205e2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a>Azure AD integrálása a Windows Áruházbeli alkalmazások
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows áruház 8.1 és korábbi verziójú projektek nem támogatottak a Visual Studio 2017.  További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Ha az hello Windows áruház-alkalmazások, Azure Active Directory (Azure AD) segítségével egyszerű és magától értetődő tooauthenticate a felhasználókat az Active Directory-fiókokat. Integrálja az Azure AD-val, az alkalmazások biztonságos felhasználhat a webes API-t az Azure AD, például az Office 365 API-k hello vagy hello Azure API által védett.

A Windows áruház asztali alkalmazások, amelyeknek tooaccess védett erőforrások az Azure AD hello Active Directory Authentication Library (ADAL) biztosít. hello kizárólagos ADAL célja toomake megkönnyítik hello app tooget hozzáférési jogkivonatok. toodemonstrate milyen egyszerűen, ez a cikk bemutatja, hogyan toobuild egy DirectorySearcher a Windows Áruházbeli alkalmazást, amely:

* Lekérdezi hozzáférési jogkivonatainak hello Azure AD Graph API felület meghívásakor hello segítségével [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Megkeresi a felhasználók számára egy megadott egyszerű felhasználónév (UPN).
* Kimenő felhasználók jeleket.

## <a name="before-you-get-started"></a>A kezdés előtt
* Töltse le a hello [projekt vázát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), vagy töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Minden letöltés egy olyan Visual Studio 2015 megoldás.
* Az Azure AD-bérlő toocreate felhasználók és a nyilvántartás hello alkalmazást is kell. Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

Ha készen áll, hello eljárásokat hajtsa végre a következő három szakasz hello.

## <a name="step-1-register-hello-directorysearcher-app"></a>1. lépés: Hello DirectorySearcher alkalmazás regisztrálása
tooenable hello alkalmazási tooget jogkivonatok, először tooregister azt az Azure ad bérlői, és adja meg az engedélyt tooaccess hello Azure AD Graph API. Ezt a következőképpen teheti meg:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiókját. Ekkor a hello **Directory** listán, válassza hello tooregister hello alkalmazás kívánt Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Kövesse az utasításokat toocreate hello egy **natív ügyfélalkalmazás**.
  * **Név** hello app toousers ismerteti.
  * **Átirányítási URI** , hogy az Azure AD által használt tooreturn token válaszok protokollt és a karakterlánc kombinációját. Adja meg a helyőrző értékét most (például **http://DirectorySearcher**). Cseréli le hello érték később.
6. Hello regisztráció befejezését követően az Azure AD hozzárendel hello app egy egyedi azonosítót. Másolja a hello hello értékét **alkalmazás** lapon, mert később szüksége.
7. A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.
8. A hello **Azure Active Directory** alkalmazást, jelölje be **Microsoft Graph** , hello API.
9. A **delegált engedélyek**, adja hozzá a hello **hozzáférés hello a címtárhoz hello bejelentkezett felhasználó** engedéllyel. Ezzel engedélyezi a hello app tooquery hello Graph API a felhasználók számára.

## <a name="step-2-install-and-configure-adal"></a>2. lépés: Telepítse és konfigurálja az adal-t
Most, hogy egy alkalmazást az Azure ad-ben, telepítse az adal-t, és az identitás-kapcsolódó kód írása. tooenable ADAL toocommunicate az Azure ad-vel, adjon neki hello app regisztrációs adatait.

1. Adja hozzá az ADAL toohello DirectorySearcher projekt hello Csomagkezelő konzol használatával.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. Hello DirectorySearcher projektben nyissa meg a MainPage.xaml.cs.
3. Cserélje le a hello hello értékek **konfigurációs értékeire** régió hello Azure-portálon megadott hello értékekkel. Kódja toothese értékek hivatkozik, amikor az adal-t használ.
  * Hello *bérlői* hello tartománya, akkor az Azure AD bérlője (például contoso.onmicrosoft.com).
  * Hello *clientId* hello alkalmazás, amely hello portálról másolt hello ügyfél-azonosító.
4. A Windows Áruházbeli alkalmazással most kell toodiscover hello visszahívási URI. Állítson be egy töréspontot a sort a hello `MainPage` módszert:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Hello megoldás, meggyőződött arról, hogy, hogy az összes csomaghivatkozásokhoz visszaállnak kiépítését. Ha hiányoznak a csomagokat, nyissa meg a NuGet-Csomagkezelő hello, és állítsa vissza őket.
6. Hello alkalmazás futtatását, és másolja a hello értékének `redirectUri` amikor hello töréspont elérte-e. hello érték hasonlóan kell kinéznie hello következő:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Vissza a hello **beállítások** hello hello Azure-portál alkalmazás lapon vegye fel a **RedirectUri** az előző érték hello.  

## <a name="step-3-use-adal-tooget-tokens-from-azure-ad"></a>3. lépés: Használja ADAL tooget jogkivonatok az Azure AD-ből
hello alapelv ADAL mögött, hogy amikor hello alkalmazásnak kell olyan hozzáférési jogkivonatot, egyszerűen meghívja `authContext.AcquireToken(…)`, és ADAL rest hello.  

1. Hello alkalmazás inicializálása `AuthenticationContext`, vagyis hello elsődleges osztály az adal-t. Ez a művelet továbbítja az Azure ad-val toocommunicate kell, és mondja el hogyan ADAL hello koordináták toocache jogkivonatokat.

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Keresse meg a hello `Search(...)` metódus, amely esetén a felhasználók kattintanak, hello **keresési** hello alkalmazás Kezelőfelületén gombjára. Ez a módszer egy get kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik. tooquery hello Graph API-t tartalmaz olyan hozzáférési jogkivonatot hello kérelem **engedélyezési** fejléc. Ez az adal-t honnan.

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Ha hello app kísérel meg jogkivonat meghívásával `AcquireTokenAsync(...)`, ADAL tooreturn jogkivonat kísérletek hello felhasználói hitelesítő adatok kérése nélkül. Ha ADAL hello felhasználó toosign tooget jogkivonatot az kell, azt bejelentkezési párbeszédpanelt jelenít meg, hello felhasználói hitelesítő adatokat gyűjt, és egy jogkivonatot ad vissza, sikeres hitelesítést követően. Ha az adal TÁRAT nem tooreturn jogkivonat bármilyen okból, hello *AuthenticationResult* állapot: Hiba történt.
3. Most már idő toouse hello hozzáférési jogkivonatot kapott. Is a hello `Search(...)` módszer, hello token toohello Graph API hello beolvasni a kérelmet csatolása **engedélyezési** fejléc:

    ```C#
    // Add hello access token toohello Authorization header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Használhatja a hello `AuthenticationResult` objektum hello felhasználó hello alkalmazás, például az hello felhasználói Azonosítók toodisplay adatait:

    ```C#
    // Update hello page UI toorepresent hello signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. ADAL toosign felhasználók hello alkalmazásból is használható. Ha a hello kattintás hello **Kijelentkezés** gombra, győződjön meg arról, hogy hello következő hívás túl`AcquireTokenAsync(...)` jeleníti meg, bejelentkezhet. Az ADAL Ez a művelet nem egyszerű módon, ha hello token gyorsítótár kiürítése:

    ```C#
    private void SignOut()
    {
        // Clear session state from hello token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>A következő lépések
Most már rendelkezik egy működő Windows Áruházbeli alkalmazást, amely hitelesíti a felhasználókat, biztonságos webes API-k OAuth 2.0 használatával hívható, és alapszintű hello felhasználó adatainak beolvasása.

Ha már nincs beállítva, feltöltve a felhasználókkal a bérlő, most már olyan hello idő toodo.
1. Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be valamelyik hello felhasználók.
2. Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.
3. Hello-alkalmazások bezárása, és újra futtathatja. Figyelje meg, hogyan hello felhasználói munkamenet nem módosulnak.
4. Kattintson a jobb gombbal a toodisplay hello alsó sáv jelentkezzen ki, majd jelentkezzen be egy másik felhasználó.

ADAL teszi, hogy könnyen tooincorporate közös identitás funkciók ilyen hello alkalmazásba. Azt gondoskodik összes hello dirty munkahelyi meg, például a gyorsítótár kezelése, OAuth protokoll támogatása, a bejelentkezési felhasználói felület, a hello felhasználói bemutató és jogkivonatok frissítése lejárt. Tooknow csak egy API-hívás, kell `authContext.AcquireToken*(…)`.

Hivatkozás, töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (a konfigurációs értékek nélkül).

Most már továbbléphet tooadditional identitás forgatókönyvek. Például [egy .NET webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
