---
title: "az Active Directory aaaAzure hitelesítési és erőforrás-kezelő |} Microsoft Docs"
description: "Fejlesztői útmutató tooauthentication hello Azure Resource Manager API-t és az Azure Active Directory, az alkalmazások integrálása más Azure-előfizetések."
services: azure-resource-manager,active-directory
documentationcenter: na
author: dushyantgill
manager: timlt
editor: tysonn
ms.assetid: 17b2b40d-bf42-4c7d-9a88-9938409c5088
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/27/2016
ms.author: dugill;tomfitz
ms.openlocfilehash: 757e45fdb28488b45de70647746461888bf35a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-resource-manager-authentication-api-tooaccess-subscriptions"></a>Erőforrás-kezelő hitelesítési API tooaccess előfizetések használata
## <a name="introduction"></a>Bevezetés
Ha egy szoftver fejlesztő, akinek szüksége van egy alkalmazás, amely felügyeli az ügyfél Azure-erőforrások toocreate, ez a témakör bemutatja, hogyan tooauthenticate a hello Azure Resource Manager API-k és szerezhet hozzáférést tooresources más előfizetésekhez.

Az alkalmazás érhető el több módon Resource Manager API-k hello:

1. **Felhasználói + alkalmazás-hozzáférés**: az alkalmazáshoz, amelyet a bejelentkezett felhasználó nevében erőforrásokhoz férnek hozzá. Ez a módszer az alkalmazások, például webes alkalmazásokat vagy csak "interaktív kezelése" Azure-erőforrások kezelésére szolgáló parancssori eszköz működik.
2. **Csak alkalmazás hozzáférés**: démon szolgáltatások és az ütemezett feladatok futó alkalmazások számára. hello app identitás toohello erőforrások közvetlen hozzáférést kap. Ez a megközelítés hosszú távú távfelügyeleti (felügyelet) hozzáférési tooAzure igénylő alkalmazások esetében működik.

Ez a témakör részletes útmutatást toocreate egy olyan alkalmazáshoz, ezek hitelesítési módszerek alkalmazza. Azt illusztrálja, hogyan tooperform minden lépés REST API-t vagy a C#. hello teljes ASP.NET MVC alkalmazás érhető el: [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

## <a name="what-hello-web-app-does"></a>Milyen hello webes alkalmazás nem
a webalkalmazás hello:

1. Bejelentkezik egy Azure felhasználói.
2. Felhasználói toogrant hello web app hozzáférés tooResource Manager kéri.
3. Lekérdezi a felhasználó + alkalmazás hozzáférési jogkivonat erőforrás-kezelő eléréséhez.
4. Használja a jogkivonatot (a 3. lépés) toocall erőforrás-kezelő és a szerepkör-hozzárendelés hello app szolgáltatás egyszerű tooa hello az előfizetést, amely hello app hosszú távú hozzáférés toohello előfizetés.
5. Csak alkalmazás-hozzáférési jogkivonat lekérése.
6. (Az 5. lépés) token toomanage erőforrásokat használ hello előfizetésben Resource Manageren keresztül.

Itt található hello webalkalmazás hello végpontok közötti áramlását.

![Erőforrás-kezelő hitelesítési folyamat](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Felhasználóként, hello előfizetési azonosítót megadta hello előfizetés toouse szeretne:

![Adja meg az előfizetés-azonosító](./media/resource-manager-api-authentication/sample-ux-1.png)

Válassza ki a hello fiók toouse való bejelentkezéskor.

![Válasszon fiókot](./media/resource-manager-api-authentication/sample-ux-2.png)

Adja meg a hitelesítő adatokat.

![Adjon meg hitelesítő adatokat](./media/resource-manager-api-authentication/sample-ux-3.png)

Alkalmazás-hozzáférés tooyour hello Azure-előfizetések engedélyezése:

![Hozzáférés biztosítása](./media/resource-manager-api-authentication/sample-ux-4.png)

A csatlakoztatott előfizetések kezelése:

![Csatlakozás az előfizetés](./media/resource-manager-api-authentication/sample-ux-7.png)

## <a name="register-application"></a>Alkalmazás regisztrálása
A kezdéshez kódolási regisztrálja a webalkalmazás az Azure Active Directory (AD). hello alkalmazás regisztrálása egy központi identitás, az alkalmazás az Azure AD-hoz létre. Alapvető információkat az alkalmazás OAuth-Ügyfélazonosító, a válasz URL-címek és a hitelesítő adatok például az alkalmazás által tooauthenticate és access Azure Resource Manager API-k magánál tartja. hello app regisztrációs különböző delegált engedélyek, amelyet az alkalmazás Microsoft APIs hello felhasználó nevében elérésekor hello is rögzíti.

Az alkalmazás más előfizetés fér hozzá, mert a több-bérlős alkalmazásként kell konfigurálnia. toopass érvényesítési, adjon meg egy társított az Azure Active Directory tartományhoz. az Azure Active Directory, a bejelentkezés toohello társított toosee hello tartományok [klasszikus portál](https://manage.windowsazure.com). Válassza ki az Azure Active Directory majd **tartományok**.

hello a következő példa bemutatja, hogyan tooregister hello Azure PowerShell használatával alkalmazást. Hello legújabb verzióját (2016 augusztusától) Azure PowerShell adja meg a parancs toowork kell rendelkeznie.

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true

hello AD-alkalmazást, a toolog, kell hello alkalmazás azonosítóját és jelszavát. toosee hello alkalmazásazonosító visszaadott hello előző parancsot használja:

    $app.ApplicationId

hello a következő példa bemutatja, hogyan tooregister hello Azure parancssori felület használatával alkalmazást.

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

hello eredmények tartalmazzák a hello AppId, amely hello alkalmazásként hitelesítéséhez szükséges.

### <a name="optional-configuration---certificate-credential"></a>Opcionális konfigurációs - tanúsítvány hitelesítő adatok
Az alkalmazások is támogatja tanúsítvány hitelesítő adatokat az Azure AD: hozzon létre egy önaláírt tanúsítványt, tartsa hello titkos kulcs és adja hozzá a nyilvános kulcs tooyour az Azure AD-alkalmazás regisztrációja hello. A hitelesítéshez az alkalmazás küld egy kis hasznos tooAzure AD a titkos kulccsal aláírva, és az Azure AD hello aláírás regisztrált hello nyilvános kulccsal ellenőrzi.

Egy AD-alkalmazás létrehozása a tanúsítvánnyal kapcsolatos információkért lásd: [egyszerű szolgáltatás használata az Azure PowerShell toocreate tooaccess erőforrások](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority) vagy [egyszerű szolgáltatás használata az Azure parancssori felület toocreate tooaccess erőforrások](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Előfizetés-azonosító bérlőazonosító beszerzése
egy jogkivonatot, amely lehet használt toocall erőforrás-kezelő toorequest, az alkalmazásban tooknow hello Bérlőazonosító hello Azure-előfizetés üzemeltető hello Azure AD-bérlő szükséges. Nagy valószínűséggel a felhasználókkal, hogy az előfizetés-azonosítók, de előfordulhat, hogy nem tudják a bérlői azonosító az Azure Active Directory. tooget hello felhasználói hello felhasználó bérlőazonosító, kérjen hello előfizetés-azonosító. Adja meg, hogy az előfizetés-azonosító, hello előfizetés vonatkozó kérelem küldésekor:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

hello kérelem sikertelen lesz, mivel hello felhasználó nem bejelentkezése még, de hello bérlőazonosító beolvasható hello válasz. Ezt a kivételt a hello válasz fejléc értéke hello bérlőazonosító lekérdezni **WWW-Authenticate**. Ez a megvalósítás a hello látja [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) metódust.

## <a name="get-user--app-access-token"></a>Felhasználói + alkalmazás hozzáférési jogkivonat beszerzése
Az alkalmazás átirányítja a felhasználókat hello felhasználói tooAzure az OAuth 2.0 engedélyezik kérelem - tooauthenticate hello felhasználó hitelesítő adatainak a AD, és vissza egy engedélyezési kódot. Az alkalmazás a Resource Manager hello engedélyezési kód tooget olyan hozzáférési jogkivonatot használja. Hello [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) hello engedélyezési kérést hoz létre.

Ez a témakör bemutatja a hello REST API kérelmek tooauthenticate hello felhasználó. Is használhatja segítő szalagtárak tooperform hitelesítést a kódban. Ezek a kódtárak kapcsolatos további információkért lásd: [Azure Active Directory hitelesítési Kódtárai](../active-directory/active-directory-authentication-libraries.md). Egy alkalmazás Identitáskezelés integrálásával útmutatóért lásd: [Azure Active Directory fejlesztői útmutatója](../active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Hitelesítési kérelmek (OAuth 2.0-s)
Nyissa meg azonosító Connect/OAuth2.0 engedélyezik lekérdezési toohello az Azure AD hitelesítési végpontra ki:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

hello lekérdezési karakterlánc paraméterek érhetők el ehhez a kérelemhez ismerteti a hello [egy engedélyezési kód kérése](../active-directory/develop/active-directory-protocols-oauth-code.md#request-an-authorization-code) témakör.

a következő példa azt mutatja meg hogyan hello toorequest OAuth2.0 engedélyezési:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Az Azure AD hitelesíti a hello felhasználót, és, ha szükséges, megkérdezi hello felhasználói toogrant engedély toohello alkalmazás. Hello engedélyezési kód toohello válasz URL-CÍMEN, az alkalmazás adja vissza. Attól függően, hogy hello kért response_mode, az Azure AD vagy küld vissza hello adatok a lekérdezési karakterláncban vagy post-adatokat.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Hitelesítési kérelmek (Open ID Connect)
Ha nem csak kívánja tooaccess Azure Resource Manager hello felhasználó nevében, de is lehetővé teszi a felhasználó toosign hello tooyour alkalmazásokban az Azure AD-fiókjában, ki egy nyitott azonosító csatlakozás engedélyezése lekérdezési. A Open ID Connect az alkalmazás is kap egy id_token, hogy a az alkalmazás toosign hello felhasználó használhatja az Azure AD-ből.

hello lekérdezési karakterlánc paraméterek érhetők el ehhez a kérelemhez ismerteti a hello [küldési hello bejelentkezési kérelmek](../active-directory/develop/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) témakör.

A következő egy példa egy Open ID Connect kérelem:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Az Azure AD hitelesíti a hello felhasználót, és, ha szükséges, megkérdezi hello felhasználói toogrant engedély toohello alkalmazás. Hello engedélyezési kód toohello válasz URL-CÍMEN, az alkalmazás adja vissza. Attól függően, hogy hello kért response_mode, az Azure AD vagy küld vissza hello adatok a lekérdezési karakterláncban vagy post-adatokat.

Az Open ID Connect válasz példája:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Jogkivonatkérelem (OAuth2.0 Code Grant Flow)
Most, hogy az alkalmazás az Azure AD-ből kapott hello engedélyezési kódot, akkor idő tooget hello hozzáférési jogkivonatot az Azure Resource Manager.  Utáni OAuth2.0 Code Grant Token kérelem toohello Azure AD Token végpont:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

hello lekérdezési karakterlánc paraméterek érhetők el ehhez a kérelemhez ismerteti a hello [hello engedélyezési kód használata](../active-directory/develop/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) témakör.

a következő példa hello code grant token jelszó hitelesítő adat vonatkozó kérést láthatók:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Amikor olyan tanúsítvány hitelesítő adatait, hozzon létre JSON webes jogkivonat (JWT) és (RSA SHA256) bejelentkezési hello titkos kulcs a az alkalmazás tanúsítvány hitelesítő adatok használatával. hello jogcímtípusok hello jogkivonat megjelennek-e [JWT jogkivonat jogcímek](../active-directory/develop/active-directory-protocols-oauth-code.md#jwt-token-claims). Referenciáért lásd: hello [Active Directory hitelesítési könyvtár (.NET) kód](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) toosign ügyfél helyességi feltétel JWT-jogkivonatokat.

Lásd: hello [Open ID Connect spec](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) talál részletes információt ügyfél-hitelesítéshez.

a következő példa hello code grant token tanúsítvány hitelesítő adat vonatkozó kérést láthatók:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Egy példa egy válasz code grant jogkivonat:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Kezeli a code grant token válasz
A sikeres token válasz az Azure Resource Manager hello (felhasználó + alkalmazás) hozzáférési jogkivonatot tartalmaz. Az alkalmazás által a hozzáférési token tooaccess erőforrás-kezelő, hello felhasználó nevében. az Azure AD által kibocsátott hozzáférési jogkivonatok élettartama hello egy óra. Nem valószínű, hogy a webes alkalmazás toorenew hello (felhasználó + alkalmazás) hozzáférési jogkivonatot kell-e. Ha toorenew hello hozzáférési jogkivonat szükséges, használja a hello frissítési jogkivonat, amely az alkalmazás fogad hello token válaszként. Utáni OAuth2.0 Token kérelem toohello Azure AD Token végpont:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

hello hello frissítési kérelem rendelkező paraméterek toouse részelemcímkék ismertetését [frissítés hello hozzáférési jogkivonat](../active-directory/develop/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

hello következő példa bemutatja, hogyan toouse hello frissítési token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Bár a frissítési jogkivonatokat használt tooget új hozzáférési jogkivonatok az Azure Resource Manager lehet, nem alkalmasak legyenek offline hozzáférés az alkalmazás. hello frissítési jogkivonatok élettartama korlátozva, és frissítési jogkivonatok kötött toohello felhasználó. Ha hello felhasználó elhagyja a szervezet hello, hello alkalmazás hello frissítési jogkivonat használatával elveszítette a hozzáférését. Ez a megközelítés alkalmazásoknál, amelyek nem használják a csapatok toomanage az Azure-erőforrások.

## <a name="check-if-user-can-assign-access-toosubscription"></a>Ha a felhasználói hozzáférés toosubscription rendelhet ellenőrzése
Az alkalmazás most már rendelkezik egy token tooaccess Azure Resource Manager hello felhasználó nevében. következő lépés hello tooconnect app toohello előfizetése van. A csatlakozás után az alkalmazás kezelhető előfizetésekben akkor is, ha hello felhasználó nem található (hosszú távú offline hozzáférést).

Minden előfizetés tooconnect hívja a hello [erőforrás-kezelő listázási engedély](https://docs.microsoft.com/rest/api/authorization/permissions) API toodetermine hello felhasználó rendelkezik-e felügyeleti jogokkal hello az előfizetéshez.

Hello [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) hello ASP.NET MVC mintaalkalmazás metódusában valósítja meg a hívás.

a következő példa azt mutatja meg hogyan hello toorequest előfizetésekből a felhasználó engedélyeit. a rendszer hello azonosítóját hello előfizetés 83cfe939-2402-4581-b761-4f59b0a041e4.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Az előfizetés hello válasz tooget felhasználói engedélyeinek példája:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

hello engedélyek API több engedélyeket ad vissza. Minden engedélyt engedélyezett műveletek (műveletek) áll, és nem engedélyezett műveletek (notactions). Ha egy művelet megtalálható-e hello engedélyezett műveletek listája bármilyen engedéllyel, és nem található meg a hello notactions listája az engedélyt, hello felhasználói tooperform művelet engedélyezett. **Microsoft.Authorization/RoleAssignments/Write** , hogy, amely hozzáférést biztosít a rights management hello művelet. Az alkalmazás számára ez a művelet karakterlánc hello műveletek és minden engedély notactions reguláris kifejezéssel egyező kell elemezni hello engedélyek eredmény toolook.

## <a name="get-app-only-access-token"></a>Csak alkalmazás-token beszerzése
Tudja, ha hello felhasználói hozzáférés toohello Azure-előfizetés is hozzárendelhető. hello lépések a következők:

1. Rendelje hozzá a hello megfelelő RBAC szerepkör tooyour alkalmazás azonosítóját hello az előfizetésben.
2. Annak ellenőrzése hello hozzáférés hozzárendelése, hello alkalmazás engedélyt hello előfizetésben lekérdezésével vagy erőforrás-kezelő csak alkalmazás tokent elérésével.
3. Az alkalmazások "csatlakoztatott előfizetések" adatszerkezet - hello azonosító hello előfizetés megőrzése rekord hello kapcsolatot.

Nézzük szorosabb hello első lépésben. tooassign hello megfelelő RBAC szerepkör toohello alkalmazás azonosítóját, meg kell határoznia:

* Az Alkalmazásidentitás hello felhasználó Azure Active Directoryban hello objektum azonosítója
* hello RBAC szerepkörhöz tartozik, amely az alkalmazás által igényelt hello előfizetésben hello azonosítója

Az alkalmazás hitelesíti a felhasználót az Azure AD-ből, ha azt hoz létre egyszerű az alkalmazáshoz, hogy az Azure AD-ben. Azure lehetővé teszi, hogy az RBAC-szerepkörök toobe hozzárendelt tooservice rendszerbiztonsági tagok toogrant közvetlen hozzáférést toocorresponding alkalmazások az Azure-erőforrások. Erre akkor pontosan mit toodo jelenítsük. Lekérdezés hello Azure AD Graph API toodetermine hello azonosítója hello szolgáltatás egyszerű hello bejelentkezett felhasználó az alkalmazás csomagazonosítóját az Azure AD.

Csak akkor kell egy hozzáférési jogkivonatot az Azure Resource Manager - egy új hozzáférési jogkivonat toocall hello Azure AD Graph API van szüksége. Azure AD-ben minden egyes alkalmazás rendelkezik engedéllyel tooquery saját szolgáltatás elsődleges objektumot, így csak alkalmazás-hozzáférési tokent is elegendő.

<a id="app-azure-ad-graph" />

### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Szerezze be a csak alkalmazás-hozzáférési tokent az Azure AD Graph API
tooauthenticate alkalmazása és egy jogkivonat tooAzure AD Graph API, get ki egy ügyfél-hitelesítő adat Grant OAuth2.0 folyamat jogkivonatkérelem tooAzure AD jogkivonat végpontjához (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/Token**).

Hello [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) metódus hello ASP.net MVC mintaalkalmazás csak alkalmazás hozzáférjen a Graph API a jogkivonat lekérdezi az Active Directory Authentication Library .NET hello.

hello lekérdezési karakterlánc paraméterek érhetők el ehhez a kérelemhez ismerteti a hello [kérelem egy hozzáférési jogkivonat](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) témakör.

Egy példa egy kérelem ügyfél-hitelesítési jogkivonat engedélyezése:

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Egy példa egy válasz ügyfél-hitelesítési jogkivonat engedélyezése:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>A felhasználó az Azure AD az alkalmazás egyszerű ObjectId beolvasása
Most, használja a hello csak alkalmazás-hozzáférési jogkivonat tooquery hello [Azure AD Graph Szolgáltatásnevekről](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API toodetermine hello hello alkalmazás egyszerű hello könyvtárban objektumazonosítója.

Hello [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) hello ASP.net MVC mintaalkalmazás metódusában valósítja meg a hívás.

a következő példa azt mutatja meg hogyan hello toorequest egy alkalmazás egyszerű. a0448380-c346-4f9f-b897-c18733de9394 hello alkalmazás ügyfélazonosítója hello.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

hello alábbi példa azt mutatja az alkalmazás szolgáltatási választ toohello kérelmet egyszerű

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow hello application tooaccess CloudSense on behalf of hello signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow hello application tooaccess CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC szerepkör-azonosító lekérése
tooassign hello megfelelő RBAC szerepkör tooyour egyszerű szolgáltatásnév, meg kell határoznia hello Azure RBAC szerepkör hello azonosítója.

hello megfelelő RBAC szerepkört az alkalmazáshoz:

* Ha az alkalmazás csak figyeli hello előfizetés, változtatások nélkül, hello előfizetés csak olvasási engedélyre van szükség. Rendelje hozzá a hello **olvasó** szerepkör.
* Ha az alkalmazás kezeli az Azure hello előfizetés, entitások létrehozása/módosítása vagy törlése egyikét igényli hello közreműködői engedélyekkel.
  * toomanage egy adott típusú erőforrás, rendeljen hello erőforrás-specifikus közreműködői szerepkört (virtuális gép közreműködő, virtuális hálózat közreműködő, tárolási fiók közreműködői, stb.)
  * toomanage minden erőforrástípus, hozzárendelése hello **közreműködő** szerepkör.

az alkalmazás szerepkör-hozzárendelés hello látható toousers, így a Válasszon hello legkevesebb szükséges jogosultsággal.

Hello hívás [erőforrás-kezelő szerepkör-definíció API](https://docs.microsoft.com/rest/api/authorization/roledefinitions) Azure RBAC-szerepkörök és a keresési majd ismétlés hello eredmény toofind hello toolist szükséges szerepkör-definíció neve.

Hello [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) hello ASP.net MVC mintaalkalmazás metódusában valósítja meg a hívás.

hello következő kérelem példa azt mutatja meg, hogyan tooget Azure RBAC szerepkör-azonosító. a rendszer hello azonosítóját hello előfizetés 09cbd307-aa71-4aca-b346-5f253e6e3ebb.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

hello válasz hello a következő formátumban kell megadni:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access toothem.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access toothem.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Toocall helyezni az API nem kell. Miután meghatározta hello jól ismert GUID-hello szerepkör-definíció, hogyan hozhat létre, hello szerepkör-definíció azonosítója:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Az alábbiakban a gyakran használt beépített szerepkörök hello jól ismert azonosítóját:

| Szerepkör | GUID |
| --- | --- |
| Olvasó |acdd72a7-3385-48EF-bd42-f606fba81ae7 |
| Közreműködő |b24988ac-6180-42a0-ab88-20f7382dd24c |
| Virtuális gép közreműködő |d73bb868-a0df-4d4d-bd69-98a00b01fccb |
| Virtuális hálózat közreműködő |b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
| Tárolási fiók közreműködői |86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
| Webhely közreműködő |de139f84-1756-47ae-9be6-808fbbe84772 |
| Webes terv közreműködő |2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
| SQL Server közreműködő |6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
| SQL DB Contributor |9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |

### <a name="assign-rbac-role-tooapplication"></a>Az RBAC szerepkör tooapplication hozzárendelése
Minden tooassign hello megfelelő RBAC szerepkör tooyour szolgáltatásnevet kell hello segítségével [erőforrás-kezelő szerepkör-hozzárendelés létrehozására](https://docs.microsoft.com/rest/api/authorization/roleassignments) API.

Hello [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) hello ASP.net MVC mintaalkalmazás metódusában valósítja meg a hívás.

Egy példa kérelem tooassign RBAC szerepkör tooapplication:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

Hello kérelem hello a következő értékek használhatók:

| GUID | Leírás |
| --- | --- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb |hello előfizetés hello azonosítója |
| c3097b31-7309-4c59-b4e3-770f8406bad2 |hello szolgáltatás egyszerű hello alkalmazás hello objektum azonosítója |
| acdd72a7-3385-48EF-bd42-f606fba81ae7 |hello olvasó szerepkört hello azonosítója |
| 4f87261d-2816-465d-8311-70a27558df4c |egy új guid hello új szerepkör-hozzárendelés létrehozása |

hello válasz hello a következő formátumban kell megadni:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Szerezze be a csak alkalmazás-hozzáférési tokent az Azure Resource Manager
toovalidate az alkalmazás hello szükséges hozzáféréssel rendelkezzen a hello előfizetés, a vizsgálati művelet végrehajtásához a hello előfizetés csak alkalmazás token használatával.

tooget csak alkalmazás-hozzáférési tokent, kövesse az utasításokat szakaszából [csak alkalmazás-token beszerzése az Azure AD Graph API](#app-azure-ad-graph), egy másik érték megadásával hello erőforrás paraméter:

    https://management.core.windows.net/

Hello [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) metódus hello ASP.NET MVC mintaalkalmazás csak alkalmazás hozzáférés az Azure Resource Manager használatával token lekérdezi az Active Directory Authentication Library .net hello.

#### <a name="get-applications-permissions-on-subscription"></a>Az alkalmazás engedélyeket szerezni az előfizetésben
amely az alkalmazás rendelkezik hello toocheck szükséges hozzáférés az Azure-előfizetéssel, akkor is hívhat hello [erőforrás-kezelő engedélyei](https://docs.microsoft.com/rest/api/authorization/permissions) API. Ezt a módszert akkor határozza meg, hogy hello felhasználó rendelkezik-e a hozzáférés-kezelés jogosultságokkal hello előfizetés hasonló toohow. Azonban a megadott idő hello engedélyek API hello csak alkalmazás jogkivonattal, hogy az előző lépésben hello hívható.

Hello [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) hello ASP.NET MVC mintaalkalmazás metódusában valósítja meg a hívás.

## <a name="manage-connected-subscriptions"></a>Csatlakoztatott előfizetések kezelése
Hello megfelelő RBAC szerepkör hozzárendelése tooyour alkalmazás egyszerű hello előfizetés esetén az alkalmazás is tartsa figyelés vagy kezelése csak alkalmazás-hozzáférési jogkivonatok segítségével csatlakozzon az Azure Resource Manager.

Ha egy előfizetés tulajdonosa eltávolítja az alkalmazás szerepkör-hozzárendelés hello klasszikus portál vagy a parancssori eszközök, az alkalmazás használatával az adott előfizetéshez nem képesek tooaccess. Ebben az esetben kell, hogy a külső hello alkalmazásból megszakadt hello kapcsolat hello előfizetéssel hello felhasználó értesítése, és számukra beállítás túl "javítás" hello kapcsolat. "Javítás" egyszerűen csak akkor hozza létre hello szerepkör-hozzárendelés offline törölve lett.

Hello felhasználói tooconnect előfizetések tooyour alkalmazás engedélyezve van, mint engedélyeznie kell a hello felhasználói toodisconnect előfizetések túl. Az access felügyeleti szempontból válassza le azt jelenti, amely rendelkezik egy hello alkalmazás egyszerű szolgáltatásnév hello előfizetés hello szerepkör-hozzárendelés eltávolítása. Másik lehetőségként olyan állapotokat hello előfizetés hello alkalmazásban előfordulhat, hogy el kell távolítani túl.
Csak a hello előfizetésben felügyeleti engedéllyel rendelkező felhasználók is képes toodisconnect hello előfizetés.

Hello [RevokeRoleFromServicePrincipalOnSubscription metódus](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) az ASP.net MVC hello mintaalkalmazás valósítja meg a hívás.

Ennyi az egész – a felhasználók most már egyszerűen csatlakozhat, és kezelheti az Azure-előfizetések az alkalmazással.
