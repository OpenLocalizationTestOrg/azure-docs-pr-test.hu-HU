---
title: "Üzembe helyezéséhez, a hívja, és a webes API-k & REST API-k hitelesítéséhez az Azure Logic Apps |} Microsoft Docs"
description: "Üzembe helyezéséhez, a hitelesítés és a rendszer Integrációk az Azure Logic Apps-munkafolyamatok webes API-k & REST API-k hívás"
keywords: "webes API-k, a REST API-k, a összekötőket, a munkafolyamatok, a rendszer integrációja, hitelesítéshez"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 88c62d5ab046d8cf4bce5d23b776e517bb0e1d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a>Üzembe helyezéséhez, hívja, és egyéni API-k hitelesítse magát a logic apps összekötők

Miután [hozzon létre egyéni API-k](./logic-apps-create-api-app.md) biztosító műveletek vagy eseményindítók logic apps-munkafolyamatok használatához, az API-kat is telepítenie kell őket hívása előtt. Bár kérjen API-k a logikai alkalmazás, a legjobb élményt, adja hozzá a [Swagger-metaadatok](http://swagger.io/specification/) , amely az API-műveleteket és paramétereket ismerteti. A Swagger-fájl az API-együttműködését, és könnyebben integrálhatja a logic apps segítségével.

Telepítheti az API-k [webalkalmazások](../app-service-web/app-service-web-overview.md), de az API-k érdemes megfontolni [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md), amely megkönnyítheti a projekt felépítéséhez, tárolására és felhasználása a felhőben és helyszíni API-k. Nincs módosíthatja bármely kódot az API-k – csupán telepítse kódját egy API-alkalmazásba. Az API-kat tárolhatja a [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform-,-a-szolgáltatásajánlat (PaaS), amely biztosítja a legjobb, a legegyszerűbb és a leginkább méretezhető módszerek valamelyikével API üzemeltetéséhez.

Az API-kat a logic Apps alkalmazásokból hívások hitelesítéséhez, állíthat be Azure Active Directory az Azure portálon így nem kell frissíteni a kódot. Vagy igényelnek, és kényszeríteni a hitelesítést az API-kódban keresztül.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Az API-webalkalmazást vagy API-alkalmazás központi telepítése

Az egyéni API-t a logikai alkalmazás hívása előtt telepítse az API-webalkalmazást vagy API-alkalmazás az Azure App Service. Is, így a Swagger-dokumentum a Logic App Designer által is olvasható, API definition tulajdonságainak beállításához és kapcsolja be a [eltérő eredetű erőforrások megosztása (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) a webes alkalmazás vagy API-alkalmazásba.

1. Válassza ki a webalkalmazás és az API-alkalmazások az Azure-portálon.

2. A panelen, amely megnyitja a **API**, válassza a **API-definíció**. Állítsa be a **API definition hely** a swagger.json fájl URL-címet.

   Általában az URL-cím jelenik meg, a következő formátumban:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Az egyéni API-Swagger-fájl csatolása](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. A **API**, válassza a **CORS**. Állítsa be a CORS-házirend a **engedélyezett eredeteket** való  **"*"** (minden engedélyezése).

   Ez a beállítás lehetővé teszi a kérelmek az Logic App-tervezőből.

   ![A Logic App Designer kérések engedélyezése az egyéni API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

További információkért lásd: ezek a cikkek:

* [Adja hozzá a Swagger-metaadatokat az ASP.NET web API-k](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [ASP.NET web API-k telepítése az Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Az egyéni API-t hívja a logic app munkafolyamatok

Az API-definíció tulajdonságait és a CORS beállítása után az egyéni API-eseményindítók és műveletek közé tartozik a logic app munkafolyamat is kell válnia. 

*  A Logic Apps-tervezőben az előfizetés webhelyek tallózással elemre kattintva megtekintheti a webhelyek, amelyek Swagger URL-címeket.

*  A Swagger-dokumentum húzni elérhető műveletek és a bemeneti adatok megtekintéséhez használja a [HTTP + Swagger műveleti](../connectors/connectors-native-http-swagger.md).

*  API-k, beleértve az API-k, amelyek nem rendelkeznek, vagy teszi közzé a Swagger-dokumentum hívására bármikor létrehozhat egy kérelmet a [HTTP-művelet](../connectors/connectors-native-http.md).

## <a name="authenticate-calls-to-your-custom-api"></a>Az egyéni API-hívások hitelesítéséhez

Az alábbi módszerek az egyéni API-hívások biztonságossá teheti:

* [Nincs kódmódosításokat](#no-code): az API-t védelme [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) az Azure-portálon, így nem kell frissítse a kódot, vagy telepítse újra az API-t.

  > [!NOTE]
  > Alapértelmezés szerint az Azure AD-alapú hitelesítés, amely az Azure portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít. Ez a hitelesítés például az API-t egy adott bérlő, nem pedig egy adott felhasználó vagy alkalmazás zárolja. 

* [Frissítse az API-kódot](#update-code): az API védelme kényszerítésével [tanúsítványhitelesítés](#certificate), [az egyszerű hitelesítés](#basic), vagy [az Azure AD-alapú hitelesítés](#azure-ad-code) kód.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Az API-hívások hitelesítéséhez programkód módosítása nélkül

Ez a metódus általános lépéseket:

1. Hozzon létre két [Azure Active Directory (Azure AD) alkalmazás identitások](../app-service-api/app-service-api-dotnet-service-principal-auth.md): egy a Logic Apps alkalmazást, egy, a webes alkalmazás (vagy API-alkalmazás).

2. Az API-hívások hitelesítést végezni, a hitelesítő adatok használata (ügyfél-Azonosítót és titkos kulcs) a a [egyszerű](../app-service-api/app-service-api-dotnet-service-principal-auth.md) , amely a Logic Apps alkalmazást az Azure AD alkalmazás identitásának van társítva.

3. Vegye fel a Alkalmazásazonosítók a logic app-definíciót.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>1. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a logikai alkalmazásnak

A Logic Apps alkalmazást az Azure AD alkalmazás identitását használja az Azure AD hitelesítő. Csak kell beállítania a identitást egy alkalommal a címtáron. Például is választja a logic apps ugyanazzal az identitással használandó annak ellenére, hogy minden logikai alkalmazás egyedi azonosítók hozhat létre. Ezeket az identitásokat az Azure-portálon állíthatja [a klasszikus Azure portálon](#app-identity-logic-classic), vagy használjon [PowerShell](#powershell).

**A logikai alkalmazásnak az alkalmazásazonosító létrehozása az Azure-portálon**

1. Az a [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), válassza a **Azure Active Directory**. 

2. Győződjön meg arról, hogy a webes alkalmazás vagy API-alkalmazás könyvtárába van-e.

   > [!TIP]
   > Könyvtárak váltáshoz kattintson a profil, és válasszon másik címtárat. Másik lehetőségként válasszon **áttekintése** > **kapcsoló directory**.

3. A directory menü alatt **kezelése**, válassza a **App regisztrációk** > **új alkalmazás regisztrációja**.

   > [!TIP]
   > Alapértelmezés szerint a regisztrációk alkalmazáslistájában összes alkalmazás regisztrációja megjelenik a könyvtárban. Válassza ki, ha csak az alkalmazás regisztrációk mellett a keresőmezőbe, **alkalmazásaimat**. 

   ![Hozzon létre új alkalmazás regisztrálása](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. Nevezze el az alkalmazás azonosítóját, hagyja **alkalmazástípus** beállítása **webalkalmazás / API**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím**, és válassza a **létrehozása**.

   ![Adjon meg nevet és a bejelentkezés URL-címet az alkalmazás-azonosító](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   Az alkalmazás azonosítóját a Logic Apps alkalmazást most létrehozott alkalmazást regisztrációk listájában jelenik meg.

   ![A logikai alkalmazásnak identitása](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. Az alkalmazás regisztrációk listában válassza ki az új alkalmazás azonosítója. Másolja ki és mentse a **Alkalmazásazonosító** használja az "ügyfél-azonosító" a Logic Apps alkalmazást a 3. rész.

   ![Másolja ki és mentse a logikai alkalmazás azonosítója](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. Ha az alkalmazás identitását beállításait nem látható, válassza a **beállítások** vagy **összes beállítás**.

7. A **API-hozzáférés**, válassza a **kulcsok**. A **leírás**, adja meg a kulcs nevét. A **Expires**, válasszon egy időtartamot a kulcshoz.

   A létrehozni kívánt kulcs az Alkalmazásidentitás "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.

   ![Logic app identitás-kulcs létrehozása](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. Az eszköztáron válassza **mentése**. A **érték**, a kulcs csomópontként jelenik meg. 
**Ügyeljen arra, hogy másolja ki és mentse a kulcsot** későbbi használatra, mert a kulcs rejtett ha hagyja a fő panelen.

   A Logic Apps alkalmazást a 3. rész konfigurálásakor meg kell adnia ezt a kulcsot, mint a "secret" vagy a jelszó.

   ![Másolja ki és mentse a kulcsot későbbi használatra](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

**Az Alkalmazásidentitás a logikai alkalmazás létrehozása a klasszikus Azure portálon**

1. A klasszikus Azure portálon, válassza ki a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2. Válassza ki a ugyanabban a könyvtárban, amely a webes alkalmazás vagy API-alkalmazás használja.

3. Az a **alkalmazások** lapra, majd **Hozzáadás** az oldal alján.

4. Nevezze el az alkalmazás azonosítóját, és válassza a **következő** (jobbra).

5. A **alkalmazás tulajdonságainak**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím** és **App ID URI**, és válassza ki **Complete** (jelölő).

6. Az a **konfigurálása** fülre, másolja ki és mentse a **ügyfél-azonosító** a 3. rész használandó logikai alkalmazásnak.

7. A **kulcsok**, nyissa meg a **válassza ki a duration** listája. Válasszon egy időtartamot a kulcshoz.

   A létrehozni kívánt kulcs az Alkalmazásidentitás "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.

8. A lap alján válassza **mentése**. Lehetséges, hogy Várjon néhány másodpercig.

9. A **kulcsok**, feltétlenül másolja, és mentse a kulcsot, ami már jelenik meg. 

   A Logic Apps alkalmazást a 3. rész konfigurálásakor meg kell adnia ezt a kulcsot, mint a "secret" vagy a jelszó.

További információ megtudhatja, hogyan [konfigurálása az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

<a name="powershell"></a>

**A logikai alkalmazásnak az alkalmazásazonosító létrehozása a PowerShell**

Ez a feladat keresztül Azure Resource Manager PowerShell használatával végezheti el. A PowerShellben futtassa a következő parancsokat:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Ügyeljen arra, hogy másolja a **Bérlőazonosító** (az Azure AD-bérlő GUID), a **Alkalmazásazonosító**, és a használt jelszót.

További információ megtudhatja, hogyan [egyszerű szolgáltatás létrehozása a PowerShell segítségével férhetnek hozzá az erőforrásokhoz](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>2. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a webes alkalmazás vagy API-alkalmazás

Ha a webes alkalmazás vagy API-alkalmazás már telepítve van, kapcsolja be a hitelesítés, és az alkalmazásazonosító létrehozása az Azure portálon. Ellenkező esetben is [Azure Resource Manager-sablon telepítésekor hitelesítés bekapcsolásához](#authen-deploy). 

**Az alkalmazásazonosító létrehozásához, és központilag telepített alkalmazások az Azure portálon hitelesítés bekapcsolása**

1. Az a [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), található, és válassza ki a webalkalmazás vagy API-alkalmazásba. 

2. A **beállítások**, válassza a **hitelesítési/engedélyezési**. A **App Service hitelesítés**, kapcsolja be a hitelesítési **a**. A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**.

   ![Hitelesítés bekapcsolása](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. Most létrehoz egy alkalmazás azonosítóját, a webes alkalmazás vagy API-alkalmazás, ahogy az itt látható. Az a **Azure Active Directory beállításai** panelen állítsa **üzemmód** való **Express**. Válasszon **új AD-alkalmazás létrehozása**. Nevezze el az alkalmazás azonosítóját, és válassza a **OK**. 

   ![Az Alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás létrehozása](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. Az a **hitelesítési / engedélyezési panel**, válassza a **mentése**.

Most meg kell keresnie az ügyfél és bérlői azonosító az alkalmazásazonosító, amely a webes alkalmazás vagy API-alkalmazás van társítva. Ezek az azonosítók használhatja a 3. rész. Így folytatja ezeket a lépéseket az Azure-portálon vagy [a klasszikus Azure portálon](#find-id-classic).

**Alkalmazás azonosítója ügyfél-azonosító és Bérlőazonosító keresése a webes alkalmazás vagy API-alkalmazás az Azure-portálon**

1. A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**. 

   ![Válassza ki a "Az Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. Az a **Azure Active Directory beállításai** panelen állítsa **üzemmód** való **speciális**.

3. Másolás a **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.

   > [!TIP] 
   > Ha **ügyfél-azonosító** és **kiállítójának URL-címe** nem jelenik meg, próbálja meg frissíteni az Azure-portálon, és ismételje meg az 1.

4. A **kiállítójának URL-címe**, másolja ki és mentse a GUID csak a 3. rész. Is használhatja a GUID a webalkalmazás vagy API-alkalmazás központi telepítési sablont, ha szükséges.

   A GUID az adott bérlő GUID-azonosító ("bérlő azonosítója"), és meg kell jelennie az URL-cím:`https://sts.windows.net/{GUID}`

5. A módosítások mentése nélkül zárja be a **Azure Active Directory beállításai** panelen.

<a name="find-id-classic"></a>

**Alkalmazás azonosítója ügyfél-azonosító és Bérlőazonosító keresése a webes alkalmazás vagy API-alkalmazás a klasszikus Azure portálon**

1. A klasszikus Azure portálon, válassza ki a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2.  Válassza ki a címtárban, ami a webalkalmazás vagy API-alkalmazás használja.

3. Az a **keresési** mezőben található, és válassza ki a webalkalmazás vagy API-alkalmazás az alkalmazásidentitás.

4. Az a **konfigurálása** fülre, másolja a **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.

5. Miután beszerezte az ügyfél-azonosító alján a **konfigurálása** lapra, majd **végpontok megtekintése**.

6. Másolja az URL-címet **összevonási metaadat-dokumentum**, és keresse meg a URL-címet.

7. A metaadat-dokumentum, amely megnyitja, keresse meg a legfelső szintű **EntityDescriptor azonosító** elemet, amely rendelkezik egy **entityid beállítást** attribútum ezen a képernyőn:`https://sts.windows.net/{GUID}` 

      Ez az attribútum a GUID az adott bérlő GUID (bérlő azonosító).

8. A bérlő azonosítója másolja ki és mentse használatra azonosító a 3. rész és használni a webalkalmazás vagy API-alkalmazás központi telepítési sablont, ha szükséges.

További információ az alábbi témakörökben talál:

* [Az Azure App Service API-alkalmazások felhasználói hitelesítésének](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [Hitelesítési és engedélyezési az Azure App Service-ben](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

**Amikor telepít egy Azure Resource Manager sablonnal hitelesítés bekapcsolása**

A logikai alkalmazásnak az alkalmazás identitását továbbra is készítsen a webes alkalmazás vagy API-alkalmazást, amely eltér az Azure AD alkalmazás identitást. Az alkalmazásazonosító létrehozásához kövesse az előző lépést a 2. rész a az Azure-portálon. Is is hajtsa végre az 1. rész utasításait, de ügyeljen arra, hogy a webes alkalmazás vagy API-alkalmazás tényleges `https://{URL}` a **bejelentkezési URL-cím** és **App ID URI**. A fenti lépéseket hogy menti az ügyfél-azonosító és a bérlő azonosítója használható az alkalmazás központi telepítési sablont és a 3. rész.

> [!NOTE]
> Az Azure AD identitása a webes alkalmazás vagy API-alkalmazás létrehozásakor az Azure-portálon vagy a klasszikus Azure portálon, nem pedig PowerShell kell használnia. A PowerShell-parancsmag segítségével nem állítsa be a szükséges engedélyekkel a felhasználók bejelentkeznek egy webhelyet.

Az ügyfél és bérlői azonosító kap, miután tartalmazza ezek az azonosítók egy subresource a webalkalmazás vagy a központi telepítési sablont API-alkalmazásba:

   ```
   "resources": [
       {
           "apiVersion": "2015-08-01",
           "name": "web",
           "type": "config",
           "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
           "properties": {
               "siteAuthEnabled": true,
               "siteAuthSettings": {
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

Automatikus telepítése egy üres web app és az Azure Active Directory-hitelesítés, és logikai alkalmazás [sablon megjelenítése a teljes Itt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), vagy kattintson a **az Azure telepítéséhez** itt:

[![Üzembe helyezés az Azure-ban](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>3. lépés: A Logic Apps alkalmazást engedélyezési című feltöltése

Az előző címsablonnak már beállítása engedélyezési ismertetjük, de ha közvetlenül a logic app, meg kell adni a teljes körű engedéllyel szakaszban.

Nyissa meg a logic app-definíciót a kód nézetre, lépjen a **HTTP** művelet a szakaszban található a **engedélyezési** szakaszt, és ezt a sort tartalmaz:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Elem | Leírás |
| ------- | ----------- |
| Bérlői |Az Azure AD-bérlő GUID azonosítója |
| Célközönség |Szükséges. A GUID azonosítóját a cél erőforráson, amely a használni kívánt - az alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás az ügyfél-azonosító |
| clientId |A hozzáférés - az ügyfél-Azonosítót a logikai alkalmazásnak az alkalmazásidentitás a kérő ügyfél GUID azonosítója |
| titkos kulcs |Szükséges. A kulcs és a jelszót az alkalmazásazonosító, hogy az ügyfél által kért a hozzáférési jogkivonat |
| type |A hitelesítési típus. ActiveDirectoryOAuth hitelesítéshez értéke `ActiveDirectoryOAuth`. |

Példa:

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Kód biztonságos API-hívások

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Tanúsítványhitelesítés

A bejövő kéréseket a Logic Apps alkalmazást-webalkalmazást vagy API-alkalmazás az érvényesítéshez ügyfél tanúsítványokat is használhat. Állítsa be a kódját, ismerje meg [TLS kölcsönös hitelesítés konfigurálásával](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

Az a **engedélyezési** szakaszban, ezt a sort tartalmaz: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Elem | Leírás |
| ------- | ----------- |
| type |Szükséges. A hitelesítési típus. Az ügyfél SSL-tanúsítványok, az értéknek kell lennie `ClientCertificate`. |
| jelszó |Szükséges. Az ügyféltanúsítvány (PFX-fájl) eléréséhez szükséges jelszót |
| PFX |Szükséges. Base64-kódolású tartalmak az ügyféltanúsítvány (PFX-fájl) |

<a name="basic"></a>

#### <a name="basic-authentication"></a>Az egyszerű hitelesítés

Ellenőrizze a webalkalmazás vagy API-alkalmazás a Logic Apps alkalmazást a bejövő kéréseket, alapszintű hitelesítés, mint például a felhasználónév és jelszó is használhat. Az egyszerű hitelesítés egy közös mintát, és a hitelesítés a webalkalmazás vagy API-alkalmazás létrehozásához használt nyelvi is használhatja.

Az a **engedélyezési** szakaszban, ezt a sort tartalmaz:

`{"type": "basic", "username": "username", "password": "password"}`.

| Elem | Leírás |
| --- | --- |
| type |Szükséges. A hitelesítési típus. Az alapszintű hitelesítés, az értéknek kell lennie `Basic`. |
| felhasználónév |Szükséges. A felhasználónév-hitelesítéshez |
| jelszó |Szükséges. A jelszó a hitelesítéshez |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Az Azure Active Directory hitelesítési kód

Alapértelmezés szerint az Azure AD-alapú hitelesítés, amely az Azure portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít. Ez a hitelesítés például az API-t egy adott bérlő, nem pedig egy adott felhasználó vagy alkalmazás zárolja. 

API-hozzáférés korlátozása kód a Logic Apps alkalmazást, bontsa ki a fejlécet, amely rendelkezik a JSON webes jogkivonat (JWT). Ellenőrizze a hívó identitását, és nem egyezik a kérelmeket.

Továbblép, a hitelesítés megvalósításához teljes mértékben az Ön saját kódját, és nem használható az Azure-portálon megtudhatja, hogyan [hitelesítik magukat a helyszíni Active Directory, az Azure alkalmazásban](../app-service-web/web-sites-authentication-authorization.md).

Hozzon létre egy alkalmazás-azonosítót a logikai alkalmazásnak, és identitás segítségével az API-hívás, kövesse az előző lépéseket.

## <a name="next-steps"></a>Következő lépések

* [Ellenőrizze, hogy logic app teljesítményében diagnosztikai naplók és riasztások](logic-apps-monitor-your-logic-apps.md)