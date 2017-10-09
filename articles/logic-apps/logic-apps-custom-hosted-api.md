---
title: "aaaDeploy, hívja, és a webes API-k & REST API-k hitelesítéshez az Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: ca1e4f28196b21f43b7c9ab94029684121e36f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a>Üzembe helyezéséhez, hívja, és egyéni API-k hitelesítse magát a logic apps összekötők

Miután [hozzon létre egyéni API-k](./logic-apps-create-api-app.md) , amely biztosít műveletek vagy eseményindítók toouse a logic apps munkafolyamatok, telepítenie kell az API-k hívása előtt be őket. Bár a logic App hello legjobb élmény meghívhatja API-k, adja hozzá [Swagger-metaadatok](http://swagger.io/specification/) , amely az API-műveleteket és paramétereket ismerteti. A Swagger-fájl az API-együttműködését, és könnyebben integrálhatja a logic apps segítségével.

Telepítheti az API-k [webalkalmazások](../app-service-web/app-service-web-overview.md), de az API-k érdemes megfontolni [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md), amely megkönnyítheti a projekt felépítéséhez, üzemeltetni, és felhasználhatják az API-k hello felhőben és a helyszíni. Nincs toochange összes kódot az API-k előnyeit – csupán telepítse a kód tooan API-alkalmazásba. Az API-kat tárolhatja a [Azure App Service](../app-service/app-service-value-prop-what-is.md), a platform,--szolgáltatás (PaaS) ajánlat, amely nyújt hello legjobb legegyszerűbb és legjobban méretezhető módszerek valamelyikével API üzemeltetéséhez.

logic apps tooyour API-k tooauthenticate hívást, állíthat be Azure Active Directory hello az Azure-portálon így nem kell tooupdate a kódot. Vagy igényelnek, és kényszeríteni a hitelesítést az API-kódban keresztül.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Az API-webalkalmazást vagy API-alkalmazás központi telepítése

Az egyéni API-t a logikai alkalmazás hívása előtt telepítse a webes alkalmazás vagy API app tooAzure App Service API. Ezenkívül toomake a Swagger-dokumentum hello Logic App Designer által is olvasható hello API-definíció tulajdonságainak beállítása, és kapcsolja be a [eltérő eredetű erőforrások megosztása (CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) a webes alkalmazás vagy API-alkalmazásba.

1. Hello Azure-portálon válassza ki a webalkalmazás vagy API-alkalmazásba.

2. Hello panelen, amely megnyitja a **API**, válassza a **API-definíció**. Set hello **API definition hely** toohello a swagger.json fájl URL-címe.

   Általában hello URL-cím jelenik meg, a következő formátumban:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Az egyéni API-hivatkozás tooSwagger fájlja](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. A **API**, válassza a **CORS**. Állítsa be a CORS-házirend hello a **engedélyezett eredeteket** túl**"*"** (minden engedélyezése).

   Ez a beállítás lehetővé teszi a kérelmek az Logic App-tervezőből.

   ![A Logic App Designer tooyour egyéni API kérések engedélyezéséhez](media/logic-apps-custom-hosted-api/custom-api-cors.png)

További információkért lásd: ezek a cikkek:

* [Adja hozzá a Swagger-metaadatokat az ASP.NET web API-k](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [Az ASP.NET web API-k tooAzure App Service telepítése](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Az egyéni API-t hívja a logic app munkafolyamatok

Hello API-definíció tulajdonságait és a CORS beállítása után az egyéni API-eseményindítók és műveletek kell válnia tooinclude a logic app munkafolyamat. 

*  tooview hello webhelyeket, amelyeken a Swagger URL-címek, megkeresheti a Logic Apps Designer hello előfizetés webhelyek.

*  tooview elérhető műveletek és a bemenetek húzni a Swagger-dokumentum, használja a hello [HTTP + Swagger műveleti](../connectors/connectors-native-http-swagger.md).

*  toocall API-k, beleértve az API-k, amelyek nem rendelkeznek, vagy teszi közzé a Swagger-dokumentum mindig hozhat létre egy kérelem hello [HTTP-művelet](../connectors/connectors-native-http.md).

## <a name="authenticate-calls-tooyour-custom-api"></a>Hitelesítés hívások tooyour egyéni API

Biztonságossá teheti a hívások tooyour egyéni API a következő lehetőségeket biztosítva:

* [Nincs kódmódosításokat](#no-code): az API-t védelme [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) keresztül hello Azure-portálon, ezért nem van tooupdate a kódot, vagy telepítse újra az API-t.

  > [!NOTE]
  > Alapértelmezés szerint hello Azure AD-alapú hitelesítés, amely a hello Azure-portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít. Ez a hitelesítés például zárolja a API toojust, egy adott bérlő, nem tooa adott felhasználó vagy alkalmazás. 

* [Frissítse az API-kódot](#update-code): az API védelme kényszerítésével [tanúsítványhitelesítés](#certificate), [az egyszerű hitelesítés](#basic), vagy [az Azure AD-alapú hitelesítés](#azure-ad-code) kód.

<a name="no-code"></a>

### <a name="authenticate-calls-tooyour-api-without-changing-code"></a>Hitelesítés hívások tooyour API programkód módosítása nélkül

Ez a módszer hello általános lépései a következő:

1. Hozzon létre két [Azure Active Directory (Azure AD) alkalmazás identitások](../app-service-api/app-service-api-dotnet-service-principal-auth.md): egy a Logic Apps alkalmazást, egy, a webes alkalmazás (vagy API-alkalmazás).

2. tooauthenticate hívások tooyour API-t hello hitelesítő adatait (ügyfél-Azonosítót és titkos kulcs) használni hello [egyszerű](../app-service-api/app-service-api-dotnet-service-principal-auth.md) , amelyhez társítva hello a Logic Apps alkalmazást az Azure AD alkalmazás azonosítója.

3. Hello alkalmazás azonosítók szerepeljenek a logic app-definíciót.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>1. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a logikai alkalmazásnak

A Logic Apps alkalmazást az Azure AD alkalmazás identitás tooauthenticate szemben az Azure AD használja. Csak akkor kell tooset ezzel az identitással be egyszer a címtáron. Például kiválaszthatja toouse hello azonos identitást a logic apps, annak ellenére, hogy minden logikai alkalmazás egyedi azonosítók hozhat létre. Ezeket az identitásokat az Azure-portálon hello állíthat be [a klasszikus Azure portálon](#app-identity-logic-classic), vagy használjon [PowerShell](#powershell).

**Hozzon létre a logikai alkalmazásnak hello Alkalmazásidentitás hello Azure-portálon**

1. A hello [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), válassza a **Azure Active Directory**. 

2. Győződjön meg arról, hogy hello bejelentkezés a webes alkalmazás vagy API-alkalmazás könyvtárába.

   > [!TIP]
   > tooswitch könyvtárak, kattintson a profil, és válasszon másik címtárat. Másik lehetőségként válasszon **áttekintése** > **kapcsoló directory**.

3. Hello directory menü alatt **kezelése**, válassza a **App regisztrációk** > **új alkalmazás regisztrációja**.

   > [!TIP]
   > Alapértelmezés szerint hello app regisztrációk listán látható az összes alkalmazás regisztrációja a címtárban. tooview csak az alkalmazás regisztrációk, tovább toohello keresőmezőbe, válasszon **alkalmazásaimat**. 

   ![Hozzon létre új alkalmazás regisztrálása](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. Nevezze el az alkalmazás azonosítóját, hagyja **alkalmazástípus** túl beállítása**Web app / API**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím**, Válassza **Hozzon létre**.

   ![Adjon meg nevet és a bejelentkezés URL-címet az alkalmazás-azonosító](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   a Logic Apps alkalmazást most létrehozott hello Alkalmazásidentitás hello app regisztrációk listájában jelenik meg.

   ![A logikai alkalmazásnak identitása](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. Hello app regisztrációk listában válassza ki az új alkalmazás azonosítója. Másolja ki és mentse a hello **Alkalmazásazonosító** toouse, hello "ügyfél-azonosító" a logikai alkalmazásnak a 3. rész.

   ![Másolja ki és mentse a logikai alkalmazás azonosítója](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. Ha az alkalmazás identitását beállításait nem látható, válassza a **beállítások** vagy **összes beállítás**.

7. A **API-hozzáférés**, válassza a **kulcsok**. A **leírás**, adja meg a kulcs nevét. A **Expires**, válasszon egy időtartamot a kulcshoz.

   hello kulcs, amely a létrehozni kívánt hello alkalmazás identitása "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.

   ![Logic app identitás-kulcs létrehozása](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. A hello eszköztárában kattintson **mentése**. A **érték**, a kulcs csomópontként jelenik meg. 
**Győződjön meg arról, hogy toocopy, és mentse a kulcsot** későbbi használatra mert rejtett hello kulcs ha hagyja hello fő paneljén.

   A Logic Apps alkalmazást a 3. rész konfigurálásakor adja meg, ez a kulcs mint hello "titkos" vagy a jelszó.

   ![Másolja ki és mentse a kulcsot későbbi használatra](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

**Hozzon létre a logikai alkalmazásnak hello Alkalmazásidentitás hello a klasszikus Azure portálon**

1. A klasszikus Azure portálon hello, válassza a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2. Válassza ki hello ugyanabban a könyvtárban, amely a webes alkalmazás vagy API-alkalmazás használja.

3. A hello **alkalmazások** lapra, majd **Hozzáadás** hello lap hello alján.

4. Nevezze el az alkalmazás azonosítóját, és válassza a **következő** (jobbra).

5. A **alkalmazás tulajdonságainak**, adjon meg egy egyedi karakterláncot formázni egy tartományt a **bejelentkezési URL-cím** és **App ID URI**, és válassza ki **Complete** (jelölő).

6. A hello **konfigurálása** fülre, másolja ki és mentse a hello **ügyfél-azonosító** a logic app toouse a 3. rész esetében.

7. A **kulcsok**, nyissa meg hello **válassza ki a duration** listája. Válasszon egy időtartamot a kulcshoz.

   hello kulcs, amely a létrehozni kívánt hello alkalmazás identitása "titkos" vagy a logikai alkalmazásnak jelszó funkcionál.

8. Hello lap hello alján válassza **mentése**. Lehetséges, hogy toowait néhány másodpercig.

9. A **kulcsok**, győződjön meg arról, hogy toocopy, és mentse hello kulcsot, amely csomópontként jelenik meg. 

   A Logic Apps alkalmazást a 3. rész konfigurálásakor adja meg, ez a kulcs mint hello "titkos" vagy a jelszó.

További információ megtudhatja, hogyan túl [konfigurálása az App Service alkalmazás toouse Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

<a name="powershell"></a>

**A logikai alkalmazásnak hello Alkalmazásidentitás létrehozása a PowerShell**

Ez a feladat keresztül Azure Resource Manager PowerShell használatával végezheti el. A PowerShellben futtassa a következő parancsokat:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Győződjön meg arról, hogy toocopy hello **Bérlőazonosító** (az Azure AD-bérlő GUID), hello **Alkalmazásazonosító**, és hello használt jelszót.

További információ megtudhatja, hogyan túl [hozzon létre egy egyszerű PowerShell tooaccess erőforrások](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>2. lépés: Hozzon létre egy Azure AD alkalmazás-azonosítót a webes alkalmazás vagy API-alkalmazás

Ha a webes alkalmazás vagy API-alkalmazás már üzembe van helyezve, hitelesítés bekapcsolása, és hozzon létre hello Alkalmazásidentitás hello Azure-portálon. Ellenkező esetben is [Azure Resource Manager-sablon telepítésekor hitelesítés bekapcsolásához](#authen-deploy). 

**Hello Alkalmazásidentitás létrehozása, és kapcsolja be a hitelesítés a hello Azure-portál a telepített alkalmazások**

1. A hello [Azure-portálon](https://portal.azure.com "https://portal.azure.com"), található, és válassza ki a webalkalmazás vagy API-alkalmazásba. 

2. A **beállítások**, válassza a **hitelesítési/engedélyezési**. A **App Service hitelesítés**, kapcsolja be a hitelesítési **a**. A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**.

   ![Hitelesítés bekapcsolása](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. Most létrehoz egy alkalmazás azonosítóját, a webes alkalmazás vagy API-alkalmazás, ahogy az itt látható. A hello **Azure Active Directory beállításai** panelen állítsa **üzemmód** túl**Express**. Válasszon **új AD-alkalmazás létrehozása**. Nevezze el az alkalmazás azonosítóját, és válassza a **OK**. 

   ![Az Alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás létrehozása](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. A hello **hitelesítési / engedélyezési panel**, válassza a **mentése**.

Most kell hello ügyfél-azonosító található, és bérlői azonosító hello alkalmazás identitás, amely a webes alkalmazás vagy API-alkalmazás van társítva. Ezek az azonosítók használhatja a 3. rész. Így folytatja ezeket a lépéseket az Azure-portálon hello vagy [a klasszikus Azure portálon](#find-id-classic).

**Alkalmazás azonosítója ügyfél-azonosító található, és a bérlői azonosító a webes alkalmazás vagy API-alkalmazásba hello Azure-portálon**

1. A **hitelesítésszolgáltatókat**, válassza a **Azure Active Directory**. 

   ![Válassza ki a "Az Azure Active Directory"](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. A hello **Azure Active Directory beállításai** panelen állítsa **üzemmód** túl**speciális**.

3. Másolás hello **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.

   > [!TIP] 
   > Ha **ügyfél-azonosító** és **kiállítójának URL-címe** nem jelenik meg, próbálja meg frissíteni az hello Azure-portálon, és ismételje meg az 1.

4. A **kiállítójának URL-címe**, másolja és mentse csak hello GUID 3. rész. Is használhatja a GUID a webalkalmazás vagy API-alkalmazás központi telepítési sablont, ha szükséges.

   A GUID az adott bérlő GUID-azonosító ("bérlő azonosítója"), és meg kell jelennie az URL-cím:`https://sts.windows.net/{GUID}`

5. A módosítások mentése nélkül zárja be a hello **Azure Active Directory beállításai** panelen.

<a name="find-id-classic"></a>

**Alkalmazás azonosítója ügyfél-azonosító található, és a bérlői azonosító a webalkalmazás vagy a klasszikus Azure portálon hello API-alkalmazás**

1. A klasszikus Azure portálon hello, válassza a [ **Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

2.  Válassza ki a webalkalmazás vagy API-alkalmazást használó hello könyvtárat.

3. A hello **keresési** mezőben található, és válassza ki a webalkalmazás vagy API-alkalmazás hello alkalmazásidentitás.

4. A hello **konfigurálása** lap, a Másolás hello **ügyfél-azonosító**, és mentse a GUID azonosítóját használja a 3. rész.

5. Miután beszerezte a hello ügyfél-azonosító hello hello alján **konfigurálása** lapra, majd **végpontok megtekintése**.

6. Hello URL-Címének másolása **összevonási metaadat-dokumentum**, és keresse meg a toothat URL-CÍMÉT.

7. Hello metaadat-dokumentum, amely megnyitja, keresse meg hello legfelső szintű **EntityDescriptor azonosító** elemet, amely rendelkezik egy **entityid beállítást** attribútum ezen a képernyőn:`https://sts.windows.net/{GUID}` 

      Ez az attribútum a GUID hello az adott bérlő GUID (bérlő azonosító).

8. Hello Bérlőazonosító másolja ki és mentse használatra azonosító 3. rész, továbbá a webalkalmazás vagy API-alkalmazás központi telepítési sablont, toouse, ha szükséges.

További információ az alábbi témakörökben talál:

* [Az Azure App Service API-alkalmazások felhasználói hitelesítésének](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [Hitelesítési és engedélyezési az Azure App Service-ben](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

**Amikor telepít egy Azure Resource Manager sablonnal hitelesítés bekapcsolása**

A logikai alkalmazásnak hello app identitásból továbbra is készítsen a webes alkalmazás vagy API-alkalmazást, amely eltér az Azure AD alkalmazás identitást. toocreate hello Alkalmazásidentitás, hajtsa végre hello előző lépések a 2. rész hello Azure-portálon. Is is hajtsa végre az 1. rész hello lépéseket, de győződjön meg arról, hogy toouse a webes alkalmazás vagy az API-alkalmazás tényleges `https://{URL}` a **bejelentkezési URL-cím** és **App ID URI**. A fenti lépéseket hogy toosave mindkét hello ügyfél és az alkalmazás központi telepítési sablont a használatra, valamint a 3. rész bérlő-azonosító.

> [!NOTE]
> Hello Azure AD alkalmazás azonosítóját a webes alkalmazás vagy API-alkalmazás létrehozásakor hello Azure-portálon vagy a klasszikus Azure portálon, nem pedig PowerShell kell használnia. hello PowerShell-parancsmag segítségével nem hello szükséges engedélyek beállítása toosign felhasználók webhelyen.

Miután beszerezte a hello ügyfél-azonosító és a bérlő azonosítója, ezek az azonosítók egy subresource a webes alkalmazás vagy API-alkalmazás központi telepítési sablonba tartalmazza:

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

tooautomatically telepítése egy üres web app és az Azure Active Directory-hitelesítés, és logikai alkalmazás [hello teljes sablon megtekintése Itt](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json), vagy kattintson a **tooAzure telepítése** itt:

[![TooAzure telepítése](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-hello-authorization-section-in-your-logic-app"></a>3. rész: Hello engedélyezési szakaszában, a logikai alkalmazás feltöltése

hello előző címsablonnak már ez a hitelesítési szakasz beállítása, de ha közvetlenül hello logic app, meg kell adni a hello teljes engedélyezési szakasz.

Nyissa meg a logic app-definíciót a kód nézetre, nyissa meg toohello **HTTP** művelet szakaszban, a keresés hello **engedélyezési** szakaszt, és ezt a sort tartalmaz:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Elem | Leírás |
| ------- | ----------- |
| Bérlői |hello GUID hello Azure AD-bérlő számára |
| Célközönség |Kötelező. hello GUID hello cél erőforrás, amelyet az tooaccess - hello hello alkalmazásidentitás a webes alkalmazás vagy API-alkalmazás az ügyfél-azonosító |
| clientId |a hozzáférés - hello alkalmazásidentitás a logikai alkalmazásnak az ügyfél-azonosító hello kérő hello ügyfél GUID hello |
| titkos kulcs |Kötelező. hello kulcs vagy jelszó hello Alkalmazásidentitás hello hozzáférési jogkivonatot kérő hello ügyfél |
| type |hello hitelesítési típus. ActiveDirectoryOAuth hitelesítéshez hello értéke `ActiveDirectoryOAuth`. |

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

toovalidate hello érkező kéréseket a logic app tooyour webalkalmazás vagy API-alkalmazás, ügyfél-tanúsítványok is használhatja. Ismerje meg, a kód tooset [hogyan tooconfigure TLS kölcsönös hitelesítés](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

A hello **engedélyezési** szakaszban, ezt a sort tartalmaz: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Elem | Leírás |
| ------- | ----------- |
| type |Kötelező. hello hitelesítési típus. Az ügyfél SSL-tanúsítványok, hello értéknek kell lennie `ClientCertificate`. |
| jelszó |Kötelező. hello jelszót hello ügyféltanúsítvány (PFX-fájl) |
| PFX |Kötelező. Base64-kódolású tartalmak hello ügyféltanúsítvány (PFX-fájl) |

<a name="basic"></a>

#### <a name="basic-authentication"></a>Az egyszerű hitelesítés

a logic app tooyour webalkalmazás vagy API-alkalmazás toovalidate bejövő kérelmet, alapszintű hitelesítés, mint például a felhasználónév és jelszó is használhatja. Az egyszerű hitelesítés egy megszokott mintát követi, és használhatja a bármely használt nyelv toobuild a hitelesítést, a webes alkalmazás vagy API-alkalmazás.

A hello **engedélyezési** szakaszban, ezt a sort tartalmaz:

`{"type": "basic", "username": "username", "password": "password"}`.

| Elem | Leírás |
| --- | --- |
| type |Kötelező. hello hitelesítési típus. Az egyszerű hitelesítés hello értéknek kell lennie `Basic`. |
| felhasználónév |Kötelező. hitelesítés hello felhasználóneve |
| jelszó |Kötelező. hello jelszó a hitelesítéshez |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Az Azure Active Directory hitelesítési kód

Alapértelmezés szerint hello Azure AD-alapú hitelesítés, amely a hello Azure-portálon kapcsolja be a minden részletre kiterjedő engedélyezési nem biztosít. Ez a hitelesítés például zárolja a API toojust, egy adott bérlő, nem tooa adott felhasználó vagy alkalmazás. 

toorestrict API access tooyour logikai alkalmazás kód, bontsa ki a hello fejlécet, amelynek hello JSON webes jogkivonat (JWT). Ellenőrizze a hello hívó identitását, és a kérelmeket, amelyek nem egyeznek.

Ha folytatja, tooimplement teljes egészében a saját kódot, és nem használható hello Azure-portálon, a hitelesítés megtudhatja, hogyan túl [hitelesítik magukat a helyszíni Active Directory, az Azure alkalmazásban](../app-service-web/web-sites-authentication-authorization.md).

Az Alkalmazásidentitás a logikai alkalmazásnak toocreate és, hogy identitás toocall az API-t használnak, akkor kövesse az előző lépéseket hello.

## <a name="next-steps"></a>Következő lépések

* [Ellenőrizze, hogy logic app teljesítményében diagnosztikai naplók és riasztások](logic-apps-monitor-your-logic-apps.md)