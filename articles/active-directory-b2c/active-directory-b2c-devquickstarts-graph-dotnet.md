---
title: "Az Azure Active Directory B2C: Használata hello Graph API-val |} Microsoft Docs"
description: "Hogyan toocall hello Graph API a B2C-bérlő az alkalmazás identitását tooautomate hello folyamat használatával."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f9904516-d9f7-43b1-ae4f-e4d9eb1c67a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: parakhj
ms.openlocfilehash: a17cdc4adf57dbf22592d99ef8ecde41652763fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-use-hello-graph-api"></a>Az Azure AD B2C: Hello Graph API használata
Az Azure Active Directory (Azure AD) B2C-bérlők általában nagyon nagy toobe. Ez azt jelenti, hogy a közös bérlői felügyeleti feladatok elvégzését toobe kell. Egy elsődleges példája felhasználók kezelése. Szükség lehet egy meglévő felhasználó tároló tooa B2C-bérlő toomigrate. Előfordulhat, hogy toohost felhasználói regisztráció szeretné, hogy a saját oldalon, és hozzon létre felhasználói fiókokat a hello háttérben Azure AD B2C-címtárban. Ilyen típusú feladatok hello képességét toocreate, olvasási, frissítési igényelnek, és törölje a felhasználói fiókokat. Ezek a feladatok hello Azure AD Graph API használatával teheti meg.

A B2C bérlőre nincsenek két elsődleges módja hello Graph API-val folytatott kommunikáció.

* Interaktív, egyszer futó feladatok meg kell összekötőként hello B2C-bérlő a rendszergazdai fiók hello feladatok végrehajtásakor. Ebben a módban van szükség egy rendszergazda toosign hitelesítő adataival, hogy a rendszergazda minden hívások toohello Graph API végrehajtása előtt.
* Automatikus, folyamatos feladatokat érdemes használni a valamilyen Ön által biztosított szolgáltatásfiók hello megfelelő jogokkal tooperform felügyeleti feladatokkal. Az Azure AD meg ehhez az alkalmazásnak, és tooAzure AD hitelesítése. Ehhez használja az **Alkalmazásazonosító** hello használó [OAuth 2.0 ügyfél hitelesítő adatai megadják](../active-directory/develop/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). Ebben az esetben hello alkalmazás úgy működik, mint maga nem felhasználóként, toocall hello Graph API-val.

Ez a cikk mutatjuk be, hogyan tooperform hello automatikus használati eset. toodemonstrate, azt fogja összeállítása a .NET 4.5 `B2CGraphClient` , amely elvégzi a felhasználó létrehozása, olvasása, frissítése és Törlés (CRUD) típusú műveletek. hello ügyfél egy Windows-parancssori felület (CLI), amely lehetővé teszi tooinvoke lesz a különböző módszereket. Azonban hello kód írása toobehave nem interaktív, automatikus módon.

## <a name="get-an-azure-ad-b2c-tenant"></a>Az Azure AD B2C-bérlő beszerzése
Előtt hozzon létre az alkalmazások vagy felhasználók számára, vagy minden kommunikálni az Azure AD, szüksége lesz egy Azure AD B2C-bérlő és egy globális rendszergazdai fiók hello-bérlőben. Ha már, a bérlő nincs [Ismerkedés az Azure AD B2C](active-directory-b2c-get-started.md).

## <a name="register-your-application-in-your-tenant"></a>Az alkalmazás regisztrálása-bérlőben
Miután a B2C-bérlő, kell tooregister hello keresztül az alkalmazás [Azure Portal](https://portal.azure.com).

> [!IMPORTANT]
> toouse hello Graph API-t a B2C bérlőre, szüksége lesz egy dedikált alkalmazás tooregister hello általános használatával *App regisztrációk* panel az Azure-portálon hello **nem** az Azure AD B2C  *Alkalmazások* panelen. Nem használhat újra hello már meglévő B2C alkalmazások az Azure AD B2C hello regisztrált *alkalmazások* panelen.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD B2C-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.
4. Hello utasításokat követve, és hozzon létre egy új alkalmazást. 
    1. Válassza ki **Web App / API** , hello alkalmazás típusa.    
    2. Adjon meg **bármely átirányítási URI** (pl. https://B2CGraphAPI), mert nincs megfelelő ehhez a példához.  
5. hello alkalmazás lesz most megjelenjenek az alkalmazások, hello listájában kattintson rá tooobtain hello **Alkalmazásazonosító** (más néven Ügyfélazonosítót). Másolja, szüksége lehet rájuk egy későbbi szakasz ismerteti.
6. Hello-beállítások panelen kattintson a **kulcsok** , és adja hozzá egy új kulcsot (más néven ügyfélkulcs). Is másolja azt egy későbbi szakasz ismerteti a használatra.

## <a name="configure-create-read-and-update-permissions-for-your-application"></a>Konfigurálása létrehozása, olvasása, és az alkalmazás engedélyeinek frissítése
Most tooconfigure az összes hello alkalmazás tooget szükséges engedélyek toocreate, olvasása, frissítése, és törli a felhasználókat.

1. A Folytatás hello Azure portal alkalmazás regisztrációk panelen, jelölje be az alkalmazás.
2. Hello-beállítások panelen kattintson a **szükséges engedélyek**.
3. Hello szükséges engedélyek panelen kattintson a **Windows Azure Active Directory**.
4. Hello hozzáférés engedélyezése a panelen, jelölje ki a hello **címtáradatok olvasása és írása** engedélyt **Alkalmazásengedélyek** kattintson **mentése**.
5. Végezetül vissza hello szükséges engedélyek panelen kattintson a hello **engedélyt adjon** gombra.

Most már rendelkezik egy alkalmazás, amely rendelkezik engedéllyel toocreate, olvassa el és frissítés felhasználóit a B2C-bérlő.

## <a name="configure-delete-permissions-for-your-application"></a>Az alkalmazás törlése engedélyek konfigurálása
Jelenleg hello *címtáradatok olvasása és írása* engedély does **nem** közé tartozik a hello képességét toodo bármely törlések, például a felhasználók törlése. Ha toogive az alkalmazás hello képességét toodelete felhasználóinak, toodo PowerShell érintő további lépések lesz szüksége, ellenkező esetben kihagyhatja toohello következő szakaszt.

Először töltse le és telepítse hello [Microsoft Online Services bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152). Majd letöltéséhez és telepítéséhez hello [64 bites Azure Active Directory-modul Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

Hello PowerShell-modul telepítése után nyissa meg a Powershellt, és csatlakozzon a tooyour B2C-bérlő. Futtatása után `Get-Credential`, akkor a rendszer bekéri a felhasználói nevet és jelszót, Enter hello felhasználónév és a B2C bérlő rendszergazdai fiók jelszavát.

> [!IMPORTANT]
> A B2C toouse bérlői rendszergazdai fiókot, amely kell **helyi** toohello B2C-bérlő. Ezek a fiókok néznek ki: myusername@myb2ctenant.onmicrosoft.com.

```powershell
Connect-MsolService
```

Most hello fogjuk használni **Alkalmazásazonosító** hello parancsfájlban tooassign hello alkalmazás hello felhasználói fiók rendszergazdai szerepkör, amely lehetővé teszi az toodelete felhasználók alatt. Ezek a szerepkörök jól ismert azonosítók rendelkezik, így a toodo szüksége van bemeneti a **Alkalmazásazonosító** hello parancsfájlban az alábbi.

```powershell
$applicationId = "<YOUR_APPLICATION_ID>"
$sp = Get-MsolServicePrincipal -AppPrincipalId $applicationId
Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId $sp.ObjectId -RoleMemberType servicePrincipal
```

Az alkalmazás most már rendelkezik a B2C-bérlő felhasználóit engedélyek toodelete is.

## <a name="download-configure-and-build-hello-sample-code"></a>Töltse le, konfigurálása, és állítsa be hello mintakód
Először hello mintakód letöltése, és lekérése is fusson. Majd most elindítjuk azt részletes bemutatása.  Is [.zip-fájlként hello mintakód letöltése](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Akkor is klónozhatja egy olyan könyvtárba, az Ön által választott:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Nyissa meg hello `B2CGraphClient\B2CGraphClient.sln` Visual Studio-megoldást a Visual Studióban. A hello `B2CGraphClient` projektet, nyissa meg hello fájl `App.config`. Hello három alkalmazásbeállítást cserélje le a saját értékeit:

```
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{hello ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{hello Key from above}" />
</appSettings>
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Kattintson a jobb gombbal a hello `B2CGraphClient` megoldás és a rebuild hello minta. Ha sikeres, most rendelkeznie kell egy `B2C.exe` található végrehajtható fájl `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-hello-graph-api"></a>Felhasználói CRUD műveletek build hello Graph API használatával
toouse hello B2CGraphClient, nyisson meg egy `cmd` Windows parancsot a parancssorba, és módosítsa a könyvtárat toohello `Debug` könyvtár. Futtassa a hello `B2C Help` parancsot.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Ez megjeleníti minden egyes parancsnál rövid leírása. Minden alkalommal, amikor aktiválják az alábbi parancsok egyikét `B2CGraphClient` egy kérelem toohello Azure AD Graph API segítségével.

### <a name="get-an-access-token"></a>Hozzáférési jogkivonat lekérése
A kérelem toohello Graph API hitelesítési olyan hozzáférési jogkivonatot kell rendelkeznie. `B2CGraphClient`felhasználási hello nyílt forráskódú Active Directory Authentication Library (ADAL) toohelp jogkivonatot szerezni. ADAL egyszerűbbé teszi a token beszerzési biztosító egyszerű API-t, és néhány fontos részleteket, például a gyorsítótár hozzáférési jogkivonatok figyelembe vételével. Toouse ADAL tooget jogkivonatokat, azonban nincs. Jogkivonatok HTTP-kérelmek létrehozásával is beszerezheti.

> [!NOTE]
> A fenti sorrendben toocommunicate hello Graph API-val rendelkező ADAL v2 használja.  Az order tooget hozzáférési jogkivonatok használható az Azure AD Graph API hello ADAL v2 és v3 kell használnia.
> 
> 

Ha `B2CGraphClient` fut, létrehoz egy példánya hello `B2CGraphClient` osztály. Ez az osztály konstruktorában hello beállítása az ADAL-hitelesítés állványok:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // hello client_id, client_secret, and tenant are provided in Program.cs, which pulls hello values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // hello AuthenticationContext is ADAL's primary class, in which you indicate hello tenant toouse.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // hello ClientCredential is where you pass in your client_id and client_secret, which are
    // provided tooAzure AD in order tooreceive an access_token by using hello app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Hello fogjuk használni `B2C Get-User` példaként parancsot. Ha `B2C Get-User` nélkül semmilyen további bemenetek, hello CLI hívások hello meghívták `B2CGraphClient.GetAllUsers(...)` metódust. Ez a metódus meghívja `B2CGraphClient.SendGraphGetRequest(...)`, amely elküld egy HTTP GET kérést toohello Graph API-val. Mielőtt `B2CGraphClient.SendGraphGetRequest(...)` küld hello GET kérés, először nyer access token ADAL használatával:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL tooacquire a token by using hello app's identity (hello credential)
    // hello first parameter is hello resource we want an access_token for; in this case, hello Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Kaphat access token hello Graph API-t hívó hello ADAL által `AuthenticationContext.AcquireToken(...)` metódust. Adal-t adja vissza egy `access_token` , amely hello alkalmazás azonosítóját jelöli.

### <a name="read-users"></a>Olvassa el a felhasználók
Ha szeretné, hogy tooget azoknak a felhasználóknak, vagy egy adott felhasználó beszerezni hello Graph API-val, HTTP küldhet `GET` toohello kérelem `/users` végpont. Egy bérlő hello felhasználói kérelem így néz ki:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

toosee a kérelem futtatásához:

 ```
 > B2C Get-User
 ```

Két fontos részek toonote van:

* hello ADAL keresztül megszerzett jogkivonattal hozzáadott toohello `Authorization` hello segítségével fejléc `Bearer` séma.
* A B2C bérlőre, kell használnia hello lekérdezésparaméter `api-version=1.6`.

Ezen adatok mindegyikét kezeli, hello `B2CGraphClient.SendGraphGetRequest(...)` módszert:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure toouse hello 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append hello access token for hello Graph API toohello Authorization header of hello request by using hello Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Felhasználói fiókok létrehozása
A B2C-bérlő felhasználói fiókokat hoz létre, amikor HTTP küldhet `POST` toohello kérelem `/users` végpont:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required toocreate consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier hello user uses toosign in toohello account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set too'LocalAccount'
    "displayName": "Joe Consumer",                // a value that can be used for displaying toohello end user
    "mailNickname": "joec",                        // an email alias for hello user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set toofalse
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Ezeket a tulajdonságokat a kérésben többsége szükséges toocreate identitásrendszerében a felhasználók. több, kattintson a toolearn [Itt](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Vegye figyelembe, hogy hello `//` illusztrációs megjelent megjegyzések. Ne szerepeljen az azokat a tényleges kérelmet.

toosee hello kérelem, hello a következő parancsok egyikét futtatja:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Hello `Create-User` parancs fogadja bemeneti paraméterként egy .JSON kiterjesztésű fájlt. Ez tartalmazza a user objektum JSON-ábrázolását. Nincsenek két minta .JSON kiterjesztésű fájlok hello mintakód: `usertemplate-email.json` és `usertemplate-username.json`. Ezek a fájlok toosuit módosíthatja az igényeinek. Ezenkívül toohello szükséges a fenti mezők, több választható mezőket, használható tartalmazza ezeket a fájlokat. Hello opcionális mezők értékét a részletek megtalálhatók a hello [Azure AD Graph API entitáshivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Láthatja, hogyan hello POST-kérelmet a összeállított `B2CGraphClient.SendGraphPostRequest(...)`.

* Az access token toohello csatol `Authorization` hello kérelem fejlécében.
* Állítja a `api-version=1.6`.
* Ez magában foglalja hello JSON felhasználói objektum hello kérelem hello törzsében.

> [!NOTE]
> Ha hello fiókok kívánt toomigrate ki egy meglévő felhasználó rendelkezik mint hello alacsonyabb jelszó erőssége [erős jelszó erőssége kényszeríti ki az Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), letilthatja a hello erős jelszó megkövetelésének hello használatával`DisableStrongPassword`hello érték `passwordPolicies` tulajdonság. Például módosíthatja hello az alábbiak szerint fent megadott felhasználói kérelem létrehozása: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.
> 
> 

### <a name="update-consumer-user-accounts"></a>Felhasználói fiókok frissítése
Felhasználói objektumok frissítésekor hello érték. a fiókot használja, amelyet toocreate felhasználói objektumok hasonló toohello. Ez a folyamat hello HTTP használja, de `PATCH` módszert:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",                // this request updates only hello user's displayName
}
```

Próbálja meg a felhasználó tooupdate azáltal, hogy frissíti a JSON-fájlokat az új adatokat. Ezután `B2CGraphClient` toorun, ezek a parancsok egyikét:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Vizsgálja meg a hello `B2CGraphClient.SendGraphPatchRequest(...)` kapcsolatos részletes tudnivalókért metódus toosend ehhez a kérelemhez.

### <a name="search-users"></a>Felhasználók keresése
Felhasználók B2C-bérlőben lévő több módon is kereshet. Egy hello felhasználói objektum azonosítója vagy két hello felhasználói bejelentkezési azonosítót használ (azaz hello `signInNames` tulajdonság).

Futtassa a következő parancsok toosearch egy adott felhasználó hello egyikét:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Íme néhány példa:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Felhasználók törlése
felhasználó törléséhez hello folyamat egyszerű. Használjon hello HTTP `DELETE` metódus és szerkezet hello URL-cím elé hello javítsa ki az Objektumazonosító:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

toosee például adja meg a parancs és a nézet hello törlési kérelem, amely nyomtatott toohello konzol:

```
> B2C Delete-User <object-id-of-user>
```

Vizsgálja meg a hello `B2CGraphClient.SendGraphDeleteRequest(...)` kapcsolatos részletes tudnivalókért metódus toosend ehhez a kérelemhez.

Hello Azure AD Graph API számos más műveletet toouser felügyeleti hozzáadását végezheti el. A [Azure AD Graph API-referencia](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) minden művelet együtt minta kérelmek részleteit.

## <a name="use-custom-attributes"></a>Egyéni attribútumok használata
A legtöbb felhasználói alkalmazások kell toostore valamilyen egyéni felhasználói profillal kapcsolatos információk. Ezt megteheti például toodefine B2C-bérlőben egyéni attribútumot. Ezután is kezelheti, hogy attribútum hello ugyanúgy más tulajdonságok úgy kezelje, a user objektum. Frissítheti az hello attribútum delete hello attribútum hello attribútum lekérdezés küldése hello attribútum bejelentkezési jogkivonatokat, és több jogcímként.

a B2C bérlőre, az egyéni attribútum toodefine lásd: hello [B2C egyéni attribútumhivatkozás](active-directory-b2c-reference-custom-attr.md).

Megtekintheti a B2C-bérlő segítségével definiált egyéni attribútumok hello `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

hello kimeneti ezeket a funkciókat, mint felfedi hello összes egyéni attribútumot, részleteit:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Használhatja a teljes név, például hello `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, a felhasználói objektumok tulajdonságainál.  Frissítse a .JSON kiterjesztésű fájlt hello új tulajdonság és hello tulajdonság értékét, és futtassa:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

A `B2CGraphClient`, programozott módon kezelje a B2C bérlő felhasználók szolgáltatás-alkalmazással. `B2CGraphClient`saját alkalmazás identitását tooauthenticate toohello Azure AD Graph API-t használ. Egy ügyfélkulcsot a jogkivonatok is megkapja. Mivel használhatja ezt a funkciót az alkalmazásba, ne felejtse néhány fő szempontot B2C-alkalmazásokhoz:

* Toogrant hello alkalmazás hello megfelelő engedélyekkel a hello bérlői van szüksége.
* Most kell toouse adal-t (nem MSAL) tooget hozzáférési jogkivonatok. (Ön is is küldhet közvetlen, szalagtár használata nélkül.)
* A Graph API hello hívásakor használható `api-version=1.6`.
* Amikor létrehozása és frissítése identitásrendszerében a felhasználók, néhány tulajdonságok szükségesek, fent leírt módon.

Ha bármilyen kérdése vagy hello Graph API segítségével szeretné tooperform műveletek kéréseinek van a B2C-bérlő megjegyzést szóljon ebben a cikkben, vagy probléma fájlt hello GitHub kód a minta-tárházban.

