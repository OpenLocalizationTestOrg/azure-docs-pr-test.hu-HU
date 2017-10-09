---
title: "aaaUse Azure Active Directory tooauthenticate Azure Batch szolgáltatás megoldások |} Microsoft Docs"
description: "Kötegelt támogatja az Azure AD a Batch szolgáltatás hello hitelesítéshez."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 6c825c30f1c80bb059a797a2e78367e599acd109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Kötegelt szolgáltatási megoldások és az Active Directory hitelesítéséhez

Az Azure Batch támogatja a hitelesítési [Azure Active Directory] [ aad_about] (az Azure AD). Az Azure AD a Microsoft több-bérlős felhőalapú címtár és az identity management szolgáltatás. Azure maga használja az Azure AD tooauthenticate, az ügyfelek, a szolgáltatás-rendszergazdák és a szervezeti felhasználók.

Az Azure AD-alapú hitelesítés használata az Azure Batch, az alábbi két módszer egyikével hitelesítheti:

- A **integrált hitelesítés** tooauthenticate egy olyan felhasználó, hello alkalmazás dolgozik. Integrált hitelesítést használó alkalmazások a felhasználói hitelesítő adatokat gyűjt, és ezen hitelesítő adatok tooauthenticate hozzáférés tooBatch erőforrást használ.
- Használatával egy **egyszerű** tooauthenticate egy felügyelet nélküli alkalmazás. Egy egyszerű definiálja a hello házirendet és egy alkalmazás engedélyeinek rendelés toorepresent hello alkalmazás futásidőben erőforrásokhoz való hozzáféréskor.

További információ az Azure AD toolearn lásd: hello [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/).

## <a name="authentication-and-pool-allocation-mode"></a>Hitelesítés és a tárolókészlet foglalási mód

Batch-fiók létrehozásakor megadhatja, ahol fiók létrehozott készleteket kell kiosztani. Választhat tooallocate készletek hello alapértelmezett Batch szolgáltatás előfizetés vagy a felhasználó az előfizetéshez. A kiválasztott érinti Önt a hitelesítéshez, hogyan fiókhoz tartozó hozzáférési tooresources.

- **A Batch-szolgáltatás előfizetésének**. Alapértelmezés szerint a Batch szolgáltatás előfizetés kötegelt készletek foglal le. Ha ezzel a beállítással hitelesítheti fiókhoz tartozó hozzáférési tooresources vagy [megosztott kulcsos](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) vagy az Azure ad-val.
- **Felhasználó-előfizetés.** Választhat tooallocate kötegelt készletek egy megadott felhasználó az előfizetéshez. Ha ezt a lehetőséget választja, hitelesítenie kell az Azure ad-val.

## <a name="endpoints-for-authentication"></a>A hitelesítési végpontot

tooinclude szüksége tooauthenticate Batch-alkalmazások az Azure ad-vel, néhány jól ismert végpontok a kódban.

### <a name="azure-ad-endpoint"></a>Azure AD-végpont

Alapszintű hello Azure AD-szolgáltató végpont esetében:

`https://login.microsoftonline.com/`

tooauthenticate az Azure ad-vel, ehhez a végponthoz hello Bérlőazonosító (directory ID) együtt használja. hello Bérlőazonosító hello Azure AD bérlő toouse hitelesítéshez azonosítja. tooretrieve hello Bérlőazonosító, hello című rész lépéseit követve [hello bérlői azonosító beszerzése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> hello bérlői-specifikus végpont a hitelesítést egy egyszerű szolgáltatás használata esetén szükséges. 
> 
> hello bérlői-specifikus végpont Önt a hitelesítéshez integrált hitelesítés használata kötelező, de ajánlott. Azonban használhatja hello Azure AD közös végpontot. hello közös végpont összegyűjtéséhez felületet, ha nincs megadva egy adott bérlő általános hitelesítő adatokat biztosít. hello közös végpont `https://login.microsoftonline.com/common`.
>
>

Az Azure AD-végpontok kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Kötegelt erőforrás végpont

Használjon hello **Azure Batch erőforrás végpont** tooacquire hitelesítéséhez jogkivonat kérelmek toohello Batch szolgáltatás:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Az alkalmazás regisztrálása a bérlő

hello első lépése az Azure AD tooauthenticate használatával regisztrálja az alkalmazást az Azure AD-bérlő. Az alkalmazás regisztrálása lehetővé teszi a toocall hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) a felhasználói kódból. hello adal-t az API-k biztosít az alkalmazásból az Azure AD-val hitelesítéséhez. Az alkalmazás regisztrálása szükség, hogy azt tervezi, hogy integrált hitelesítés toouse vagy egy egyszerű szolgáltatást.

Ha regisztrálja az alkalmazást, adja meg információkat az alkalmazás tooAzure AD kapcsolatos. Az Azure AD majd biztosít az Alkalmazásazonosító használt tooassociate az alkalmazás az Azure ad-val futásidőben. toolearn hello Alkalmazásazonosító, kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).

tooregister a kötegelt kérelem hello hello lépéseit kövesse [egy alkalmazás hozzáadása](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) szakasz [alkalmazások integrálása az Azure Active Directory][aad_integrate]. Ha natív alkalmazás regisztrálnia az alkalmazást, megadhat bármilyen érvényes URI-JÁNAK hello **átirányítási URI-**. Nem kell valódi végpont toobe.

Az alkalmazás regisztrálása után hello Alkalmazásazonosító jelenik meg:

![A kötegelt alkalmazás regisztrálása az Azure ad szolgáltatással](./media/batch-aad-auth/app-registration-data-plane.png)

Egy alkalmazás regisztrálása az Azure ad-vel kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).

## <a name="get-hello-tenant-id-for-your-active-directory"></a>Az Active Directory hello Bérlőazonosító beszerzése

hello Bérlőazonosító hello Azure AD-bérlő hitelesítési szolgáltatások tooyour alkalmazás biztosító azonosítja. tooget hello Bérlőazonosító, kövesse az alábbi lépéseket:

1. Hello Azure-portálon válassza ki az Active Directory szolgáltatásba.
2. Kattintson a **Tulajdonságok** elemre.
3. Másolja át a megadott hello directory azonosítóhoz GUID-érték hello Ez az érték rövidítése hello bérlő azonosítója.

![Másolja a hello könyvtár-azonosítója](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Integrált hitelesítés használata

tooauthenticate az integrált hitelesítés, az alkalmazás engedélyek tooconnect toohello Batch szolgáltatás API toogrant szüksége. Ez a lépés lehetővé teszi, hogy az alkalmazás tooauthenticate hívások toohello Batch szolgáltatás API az Azure ad-val.

Miután megismerte [az alkalmazás regisztrálva](#register-your-application-with-an-azure-ad-tenant), kövesse az alábbi lépéseket a hello toohello Batch szolgáltatás elérni az Azure portál toogrant:

1. Azure-portálon hello hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**.
2. Keresse meg az alkalmazás hello listában app regisztrációk hello nevét:

    ![Keresse meg az alkalmazás neve](./media/batch-aad-auth/search-app-registration.png)

3. Nyissa meg hello **beállítások** panel az alkalmazáshoz. A hello **API-hozzáférés** szakaszban jelölje be **szükséges engedélyek**.
4. A hello **szükséges engedélyek** panelen kattintson a hello **hozzáadása** gomb.
5. Az 1. lépésben keressen hello kötegelt API. Ezek a karakterláncok, amíg meg nem látja hello API mindegyikének keresése:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Újabb Azure AD-bérlők ezt a nevet használhatják.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** hello azonosítója hello kötegelt API. 
6. Miután megtalálta hello kötegelt API, válassza ki azt, majd kattintson a hello **válasszon** gombra.
6. A 2. lépésben, válassza ki hello jelölőnégyzet mellett túl**Azure Batch szolgáltatás** hello kattintson **válasszon** gombra.
7. Kattintson a hello **végzett** gombra.

Hello **szükséges engedélyek** most azt mutatja, hogy az Azure AD alkalmazás tooboth ADAL eléréséhez, és a Batch szolgáltatás API hello panelen. Engedélyekkel tooADAL automatikusan regisztrációt az alkalmazás az Azure ad-val.

![Támogatás API engedélyei](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Egy egyszerű szolgáltatás használata 

tooauthenticate felügyelet nélküli futó alkalmazást, használhat egy egyszerű szolgáltatást. Az alkalmazás regisztrálása után kövesse az alábbi lépéseket az Azure portál tooconfigure egy egyszerű szolgáltatásnév hello:

1. Egy alkalmazás titkos kulcs kérelmet.
2. Az RBAC szerepkör tooyour alkalmazás hozzárendelése.

### <a name="request-a-secret-key-for-your-application"></a>A kérelem egy alkalmazás titkos kulcs

Ha az alkalmazás egyszerű szolgáltatás végzi a hitelesítést, küldi hello Alkalmazásazonosítót és a titkos kulcs tooAzure AD. Kell toocreate kell és titkos kulcs toouse hello másolni a kódot.

Kövesse az alábbi lépéseket a hello Azure-portálon:

1. Azure-portálon hello hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**.
2. Keresse meg az alkalmazás hello listában app regisztrációk hello nevét.
3. Megjelenítési hello **beállítások** panelen. A hello **API-hozzáférés** szakaszban jelölje be **kulcsok**.
4. toocreate egy kulcsot, adja meg a hello kulcs leírását. Ezután válasszon egy időtartamot hello kulcs egy vagy két év. 
5. Kattintson a hello **mentése** toocreate gombra, és megjeleníti a hello kulcs. Nem fogja tudni tooaccess hello kulcsérték tooa biztonságos helyen, másolja azt követően újra hello panelen hagyja. 

    ![A titkos kulcs létrehozása](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-tooyour-application"></a>Az RBAC szerepkör tooyour alkalmazás hozzárendelése

az egyszerű szolgáltatás tooauthenticate, kell tooassign RBAC szerepkör tooyour kérelmet. Kövesse az alábbi lépéseket:

1. Hello Azure-portálon keresse meg az alkalmazás által használt toohello Batch-fiókhoz.
2. A hello **beállítások** hello Batch-fiók, jelölje be a panelt **hozzáférés-vezérlés (IAM)**.
3. Kattintson a hello **Hozzáadás** gombra. 
4. A hello **szerepkör** legördülő listából válassza ki bármelyik hello _közreműködő_ vagy _olvasó_ szerepkör az alkalmazáshoz. Ezek a szerepkörök további információkért lásd: [Ismerkedés a szerepköralapú hozzáférés-vezérlés az Azure-portálon hello](../active-directory/role-based-access-control-what-is.md).  
5. A hello **válasszon** mezőbe írja be az alkalmazás hello nevét. Válassza ki az alkalmazás hello listából, majd kattintson **mentése**.

Az alkalmazás ekkor meg kell jelennie a hozzáférés-vezérlési beállításokkal hozzárendelt RBAC szerepkörrel rendelkező. 

![Az RBAC szerepkör tooyour alkalmazás hozzárendelése](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-hello-tenant-id-for-your-azure-active-directory"></a>Az Azure Active Directory hello Bérlőazonosító beszerzése

hello Bérlőazonosító hello Azure AD-bérlő hitelesítési szolgáltatások tooyour alkalmazás biztosító azonosítja. tooget hello Bérlőazonosító, kövesse az alábbi lépéseket:

1. Hello Azure-portálon válassza ki az Active Directory szolgáltatásba.
2. Kattintson a **Tulajdonságok** elemre.
3. Másolja át a megadott hello directory azonosítóhoz GUID-érték hello Ez az érték rövidítése hello bérlő azonosítója.

![Másolja a hello könyvtár-azonosítója](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Kódpéldák

hello ebben a szakaszban szereplő példák megjelenítése az Azure AD használatával tooauthenticate integrált hitelesítés és az egyszerű szolgáltatás. A Kódminták .NET használja, de hello fogalmak hasonló, ha a többi nyelvet.

> [!NOTE]
> Egy Azure AD hitelesítési tokent egy óra múlva lejár. A hosszú élettartamú használatakor **BatchClient** objektum, azt javasoljuk, hogy a jogkivonat lekérése az ADAL az összes kérelem tooensure mindig van egy érvényes tokent. 
>
>
> tooachieve a .NET, ez egy olyan metódus írása, hogy beolvassa hello Azure ad-token, és adja át az adott metódus tooa **BatchTokenCredentials** meghatalmazottként objektum. hello delegáltmetódus minden kérelem toohello Batch szolgáltatás tooensure, amely egy érvényes jogkivonat van megadva a neve. Alapértelmezés szerint a ADAL jogkivonatokat, gyorsítótárazza, így egy új jogkivonatot az Azure AD kéri le a rendszer csak akkor, ha szükséges. Az Azure AD-jogkivonatokat kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Példa: az Azure AD hitelesítési integrálva Batch .NET

a Batch .NET-, referencia hello integrált hitelesítés tooauthenticate [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) csomag- és hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) csomag.

Adja meg a következőket hello `using` utasítások a kódban:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Az Azure AD-végpont a kódban, beleértve a hello hivatkozás hello bérlői azonosítóját. tooretrieve hello Bérlőazonosító, hello című rész lépéseit követve [hello bérlői azonosító beszerzése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Referencia erőforrás hello kötegelt szolgáltatásvégpont:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

A Batch-fiók hivatkozási:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Adja meg a hello alkalmazás Azonosítóját (ügyfél-azonosító) az alkalmazáshoz. az alkalmazás hello Azure-portálon való regisztráció a hello Alkalmazásazonosító érhető el:

```csharp
private const string ClientId = "<application-id>";
```

Hello átirányítási URI-t hello regisztrációs folyamat során megadott szeretne másolni. az átirányítási URI a kódban megadott meg kell egyeznie a hello átirányítási URI-t hello alkalmazás regisztrálásakor megadott hello:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

A visszahívási metódus tooacquire hello hitelesítésére szolgáló jogkivonat írni az Azure AD. Hello **GetAuthenticationTokenAsync** itt látható visszahívási metódus meghívja ADAL tooauthenticate a felhasználó, aki hello alkalmazás dolgozik. Hello **AcquireTokenAsync** metódus által adal-t kér hello felhasználói a hitelesítő adatait, és hello alkalmazás halad egyszer hello felhasználói eszközeivel (kivéve, ha már gyorsítótárazott hitelesítő adatok):

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire hello authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Összeállíthatja a **BatchTokenCredentials** objektum, amely hello delegált paramétert fogad. Használja ezeket a hitelesítő adatok tooopen egy **BatchClient** objektum. Is használhatja, amely **BatchClient** objektum a Batch szolgáltatás hello további műveleteket:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Példa: az Azure AD szolgáltatás egyszerű Batch .NET használatával

az egyszerű szolgáltatás a Batch .NET-, referencia hello tooauthenticate [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) csomag- és hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) csomag.

Adja meg a következőket hello `using` utasítások a kódban:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Az Azure AD-végpont a kódban, beleértve a hello hivatkozás hello bérlői azonosítóját. Egy egyszerű szolgáltatás használata esetén meg kell adnia egy bérlő-specifikus végpontot. tooretrieve hello Bérlőazonosító, hello című rész lépéseit követve [hello bérlői azonosító beszerzése az Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Referencia erőforrás hello kötegelt szolgáltatásvégpont:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

A Batch-fiók hivatkozási:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Adja meg a hello alkalmazás Azonosítóját (ügyfél-azonosító) az alkalmazáshoz. az alkalmazás hello Azure-portálon való regisztráció a hello Alkalmazásazonosító érhető el:

```csharp
private const string ClientId = "<application-id>";
```

Adja meg a titkos kulcs hello hello Azure-portálon fájlból másolt:

```csharp
private const string ClientKey = "<secret-key>";
```

A visszahívási metódus tooacquire hello hitelesítésére szolgáló jogkivonat írni az Azure AD. Hello **GetAuthenticationTokenAsync** visszahívási metódus hívások az adal TÁRAT a felügyelet nélküli hitelesítéshez itt látható:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Összeállíthatja a **BatchTokenCredentials** objektum, amely hello delegált paramétert fogad. Használja ezeket a hitelesítő adatok tooopen egy **BatchClient** objektum. Ezután használhatja, amely **BatchClient** objektum a Batch szolgáltatás hello további műveleteket:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a>Következő lépések

További információ az Azure AD toolearn lásd: hello [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/). Hogyan toouse ADAL találhatók hello bemutató részletes példák [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=active-directory) könyvtár.

toolearn szolgáltatásnevekről, kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md). a szolgáltatás egyszerű használatával toocreate hello Azure portál, lásd: [portál toocreate Active Directory-alkalmazást és egy egyszerű szolgáltatást, amely erőforrások eléréséhez használjon](../resource-group-create-service-principal-portal.md). A szolgáltatás egyszerű PowerShell vagy az Azure parancssori felület is létrehozhat. 

tooauthenticate kötegelt felügyeleti alkalmazások az Azure AD, lásd: [hitelesítéséhez kötegelt megoldásokat az Active Directory](batch-aad-auth-management.md). 

[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"
[azure_portal]: http://portal.azure.com
