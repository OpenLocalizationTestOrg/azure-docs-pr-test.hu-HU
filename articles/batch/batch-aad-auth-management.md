---
title: "aaaUse Azure Active Directory tooauthenticate kötegelt megoldások |} Microsoft Docs"
description: "Az Azure resource manager-val készült alkalmazások, és az Azure AD hitelesíti hello kötegelt erőforrás-szolgáltató."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 192aa9f8d7cbfc0282a4a0c33ab1659f1f351525
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Hitelesítés kötegelt megoldásokat az Active Directoryban

A hitelesítést hello Azure Batch Management szolgáltatás alkalmazások [Azure Active Directory] [ aad_about] (az Azure AD). Az Azure AD a Microsoft több-bérlős felhőalapú címtár és az identity management szolgáltatás. Azure hello hitelesítésére az ügyfelek, a szolgáltatás-rendszergazdák és a szervezeti felhasználók az Azure AD használ a saját magát.

hello Batch Management .NET kódtár típusok kötegelt fiókok, kulcsait, alkalmazások és csomagok tesz elérhetővé. hello Batch Management .NET könyvtár az Azure-erőforrás-szolgáltató ügyfél, és együtt használni [Azure Resource Manager] [ resman_overview] toomanage ezeket az erőforrásokat programozott módon. Az Azure AD szükség tooauthenticate kérelmet bármely Azure-erőforrás szolgáltató ügyfél hello Batch Management .NET kódtárral dokumentumtár, beleértve és révén [Azure Resource Manager][resman_overview].

Ebben a cikkben azt megismerkedhet az Azure AD tooauthenticate hello Batch Management .NET kódtárral libraryt használó alkalmazások segítségével. Megmutatjuk, hogyan toouse az Azure AD tooauthenticate egy előfizetés rendszergazdai vagy társadminisztrátori, segítségével integrált hitelesítést. Hello használjuk [AccountManagment] [ acct_mgmt_sample] mintaprojektet, elérhető a Githubon, toowalk keresztül hello Batch Management .NET könyvtár az Azure AD használatával.

toolearn több használatáról hello Batch Management .NET kódtárral kódtár és hello AccountManagement mintaalkalmazás, lásd: [kezelése Batch fiókjainak és kvótáinak hello kötegelt felügyeleti ügyféloldali kódtára a .NET-hez a](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Az alkalmazás regisztrálása az Azure ad szolgáltatással

hello Azure [Active Directory Authentication Library] [ aad_adal] (adal-t) biztosít programozási felület tooAzure használatra AD az alkalmazásokon belül. toocall ADAL-nak az alkalmazásról, regisztrálnia kell az alkalmazást az Azure AD-bérlő. Ha a regisztrálnia az alkalmazást, az információk az alkalmazásról, beleértve azt a belül hello Azure AD-bérlő nevét adja meg információkat az Azure AD. Az Azure AD majd biztosít az Alkalmazásazonosító használt tooassociate az alkalmazás az Azure ad-val futásidőben. toolearn hello Alkalmazásazonosító, kapcsolatos további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban](../active-directory/develop/active-directory-application-objects.md).

tooregister hello AccountManagement mintaalkalmazást, kövesse hello hello [egy alkalmazás hozzáadása](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) szakasz [alkalmazások integrálása az Azure Active Directory] [ aad_integrate]. Adja meg **natív ügyfélalkalmazás** hello típusú alkalmazást. hello iparági szabványos OAuth 2.0 URI-JÁNAK hello **átirányítási URI-** van `urn:ietf:wg:oauth:2.0:oob`. Azonban megadhat bármilyen érvényes URI-azonosító (például `http://myaccountmanagementsample`) a hello **átirányítási URI-**, mert nem kell valódi végpont toobe:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Hello regisztrációs folyamat befejezése után kell lásd: hello Alkalmazásazonosítót és hello objektumazonosító (egyszerű szolgáltatásnév): az alkalmazás szerepel.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-hello-azure-resource-manager-api-access-tooyour-application"></a>Hello Azure Resource Manager API access tooyour alkalmazás

A következő toodelegate hozzáférés tooyour alkalmazás toohello Azure Resource Manager API lesz szüksége. a Resource Manager API hello hello Azure AD-azonosító **Windows Azure szolgáltatásfelügyeleti API**.

Kövesse az alábbi lépéseket a hello Azure-portálon:

1. Hello Azure-portálon hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.
2. Keresse meg az alkalmazás hello listában app regisztrációk hello nevét:

    ![Keresse meg az alkalmazás neve](./media/batch-aad-auth-management/search-app-registration.png)

3. Megjelenítési hello **beállítások** panelen. A hello **API-hozzáférés** szakaszban jelölje be **szükséges engedélyek**.
4. Kattintson a **Hozzáadás** tooadd egy új szükséges engedéllyel. 
5. Adja meg az 1. lépésben **Windows Azure szolgáltatásfelügyeleti API**, jelölje ki, hogy API eredmények hello listája, majd kattintson a hello **válasszon** gombra.
6. A 2. lépésben, válassza ki hello jelölőnégyzet mellett túl**Access Azure klasszikus üzembe helyezési modellel, mint a szervezeti felhasználók**, hello kattintson **válasszon** gombra.
7. Kattintson a hello **végzett** gombra.

Hello **szükséges engedélyek** panel most mutat be, hogy engedélyeket tooyour alkalmazás kapnak tooboth hello adal-t és a Resource Manager API-kat. Engedélyekkel tooADAL alapértelmezés szerint amikor az alkalmazás regisztrálására az Azure ad-val.

![Delegált engedélyek toohello Azure Resource Manager API](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD-végpontok

tooauthenticate a kötegelt megoldásokat az Azure ad-vel, szüksége lesz a jól ismert végpontokat.

- Hello **az Azure AD közös végpont** összegyűjtéséhez felületet, amikor egy adott bérlő nem szerepel, mint hello integrált hitelesítés általános hitelesítő adatokat biztosít:

    `https://login.microsoftonline.com/common`

- Hello **Azure Resource Manager-végpont** használt tooacquire kérelmek toohello Batch management szolgáltatás hitelesítéséhez jogkivonat van:

    `https://management.core.windows.net/`

hello AccountManagement mintaalkalmazás állandókat ezeket a végpontokat határoz meg. Ezek állandók változatlanul hagyja:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Az Alkalmazásazonosító hivatkozik 

Az ügyfélalkalmazás hello application ID (is hivatkozott tooas hello ügyfél-azonosító) tooaccess az Azure AD futásidőben. Az alkalmazás már regisztrált hello Azure-portálon, a kód toouse hello Alkalmazásazonosító regisztrált alkalmazás az Azure AD által biztosított frissíti. A hello AccountManagement mintaalkalmazást másolja az Alkalmazásazonosító hello Azure portál toohello megfelelő állandó:

```csharp
// Specify hello unique identifier (hello "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access hello Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Hello átirányítási URI-t hello regisztrációs folyamat során megadott szeretne másolni. hello átirányítási URI a kódban megadott hello átirányítási URI-t hello alkalmazás regisztrálásakor megadott egyeznie kell.

```csharp
// hello URI toowhich Azure AD will redirect in response tooan OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need toobe a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Az Azure AD hitelesítési jogkivonat

Miután hello AccountManagement minta regisztrálni kell az Azure AD-bérlő hello, és frissítse az hello forrás mintakódot az értékeket, hello minta készen áll az Azure AD tooauthenticate. Hello minta futtatásához hello adal-t próbálja tooacquire egy hitelesítési jogkivonatot. Ebben a lépésben megkérdezi a Microsoft hitelesítő adatait: 

```csharp
// Obtain an access token using hello "common" AAD resource. This allows hello application
// tooquery AAD for information that lies outside hello application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

Miután megadta a hitelesítő adatait, hello mintaalkalmazás lépne tooissue hitelesített kérelmek toohello Batch management szolgáltatás. 

## <a name="next-steps"></a>Következő lépések

További információ hello [AccountManagement mintaalkalmazás][acct_mgmt_sample], lásd: [kezelése Batch fiókjainak és kvótáinak a hello kötegelt felügyeleti ügyféloldali kódtára a .NET](batch-management-dotnet.md).

További információ az Azure AD toolearn lásd: hello [Azure Active Directory dokumentációjának](https://docs.microsoft.com/azure/active-directory/). Hogyan toouse ADAL találhatók hello bemutató részletes példák [Azure mintakódok](https://azure.microsoft.com/resources/samples/?service=active-directory) könyvtár.

tooauthenticate Batch szolgáltatás alkalmazások az Azure AD, lásd: [hitelesítéséhez kötegelt szolgáltatási megoldások és az Active Directory](batch-aad-auth.md). 


[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory hitelesítési forgatókönyvei"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directoryval"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
