---
title: "egy webes API háttéralkalmazás, az Azure Active Directory és az API Management aaaProtect |} Microsoft Docs"
description: "Ismerje meg, hogy egy webes API háttéralkalmazás, az Azure Active Directory és az API Management tooprotect."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: f856ff03-64a1-4548-9ec4-c0ec4cc1600f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: f4b323034354aa09579c643bade47257fbf1e5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprotect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Hogyan tooprotect egy webes API háttéralkalmazás, az Azure Active Directory és az API Management
a következő videó bemutatja hogyan hello toobuild egy webes API háttéralkalmazás, és megvédhetik azokat az Azure Active Directory és az API Management OAuth 2.0 protokoll használatával.  Ez a cikk áttekintést nyújt, és további információk a hello hello videó lépéseket. A 24 perces videó bemutatja, hogyan számára:

* Hozza létre egy webes API háttéralkalmazás, és biztosíthatja az AAD - kezdő pozíció: 1:30-ben
* Az API Management - 7:10 kezdődő hello API importálása
* Hello Developer portálon toocall hello API – 9:09 kezdődő konfigurálása
* Egy asztali alkalmazás toocall hello API – 18:08 kezdődő konfigurálása
* Konfigurálja a JWT-ellenőrzési házirend toopre-kérelmek - 20:47 kezdődő engedélyezése

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

## <a name="create-an-azure-ad-directory"></a>Az Azure AD-címtár létrehozása
a Web API biztonsági másolatot az Azure Active Directoryval kell toosecure egy AAD-bérlőt. Ez a videó egy bérlő nevű **APIMDemo** szolgál. toocreate egy AAD-bérlőt bejelentkezési toohello [klasszikus Azure portál](https://manage.windowsazure.com) kattintson **új**->**alkalmazásszolgáltatások**->**aktív Directory**->**Directory**->**egyéni létrehozás**. 

![Azure Active Directory][api-management-create-aad-menu]

Ebben a példában egy nevű könyvtár **APIMDemo** létrejön egy alapértelmezett tartomány nevű **DemoAPIM.onmicrosoft.com**. Ez a könyvtár használja a rendszer hello videó keresztül.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Azure Active Directory által biztosított webes API szolgáltatás létrehozása
Ebben a lépésben egy webes API háttéralkalmazás létrehozása a Visual Studio 2013. Ez a lépés a videó hello kezdődik, 1:30. toocreate webes API háttéralkalmazás-projekt a Visual Studio kattintson **fájl**->**új**->**projekt**, és válassza a **ASP.NET webalkalmazás Alkalmazás** a hello **webes** sablonok listájának. Ez a videó hello projekt neve **APIMAADDemo**. Kattintson a **OK** toocreate hello projekt. 

![Visual Studio][api-management-new-web-app]

Kattintson a **Web API** a hello **jelöljön ki egy sablon listát** toocreate Web API-projektet. Kattintson az Azure Directory hitelesítési tooconfigure **hitelesítés módosítása**.

![Új projekt][api-management-new-project]

Kattintson a **munkahelyi és iskolai fiókok**, és adja meg a hello **tartomány** , az AAD-bérlőt. A példa hello a tartomány pedig **DemoAPIM.onmicrosoft.com**. a címtár hello tartomány hello lehet lekérni **tartományok** a címtár lapján.

![Tartományok][api-management-aad-domains]

Hello szükséges beállítások konfigurálása a hello **hitelesítés módosítása** párbeszédpanel megnyitásához, és kattintson **OK**.

![Hitelesítés módosítása][api-management-change-authentication]

Amikor rákattint **OK** Visual Studio megpróbál tooregister az Azure AD-címtár az alkalmazásba, és előfordulhat, hogy a Visual Studio által kért toosign. Jelentkezzen be egy rendszergazdai fiókkal a címtáron.

![Jelentkezzen be tooVisual Studio][api-management-sign-in-vidual-studio]

Ebben a projektben, mint egy Azure webes API-ellenőrzés hello mezőjének tooconfigure **hello felhőben lévő gazdagéphez** majd **OK**.

![Új projekt][api-management-new-project-cloud]

Előfordulhat, hogy a tooAzure rákérdezéses toosign, és a Web App hello beállításához használhatja.

![Konfigurálás][api-management-configure-web-app]

Ebben a példában új **App Service-csomag** nevű **APIMAADDemo** van megadva.

Kattintson a **OK** tooconfigure hello Web App és hello projekt létrehozásához.

## <a name="add-hello-code-toohello-web-api-project"></a>Hello kód toohello Web API-projekt hozzáadása
hello következő lépése hello videó hello kód toohello Web API-projektet ad hozzá. Ez a lépés 4:35 kezdődik.

hello Web API ebben a példában egy modell és a vezérlő alapvető Számológép szolgáltatás megvalósítja. tooadd hello modellt hello szolgáltatáshoz, kattintson a jobb gombbal **modellek** a **Megoldáskezelőben** válassza **Hozzáadás**, **osztály**. Hello osztály neve `CalcInput` kattintson **Hozzáadás**.

Adja hozzá a következő hello `using` utasítás toohello felső részén hello `CalcInput.cs` fájlt.

```c#
using Newtonsoft.Json;
```

Cserélje le a következő kód hello generált hello osztály.

```c#
public class CalcInput
{
    [JsonProperty(PropertyName = "a")]
    public int a;

    [JsonProperty(PropertyName = "b")]
    public int b;
}
```

Kattintson a jobb gombbal **tartományvezérlők** a **Megoldáskezelőben** válassza **Hozzáadás**->**vezérlő**. Válasszon **Web API 2 vezérlő - üres** kattintson **Hozzáadás**. Típus **CalcController** hello, tartományvezérlői nevet, és kattintson a **Hozzáadás**.

![Vezérlő hozzáadása][api-management-add-controller]

Adja hozzá a következő hello `using` utasítás toohello felső részén hello `CalcController.cs` fájlt.

```c#
using System.IO;
using System.Web;
using APIMAADDemo.Models;
```

Generált hello vezérlőosztály cserélje le a következő kód hello. Ez a kód megvalósítja hello `Add`, `Subtract`, `Multiply`, és `Divide` hello alapvető Számológép API működésére.

```c#
[Authorize]
public class CalcController : ApiController
{
    [Route("api/add")]
    [HttpGet]
    public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/sub")]
    [HttpGet]
    public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/mul")]
    [HttpGet]
    public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }

    [Route("api/div")]
    [HttpGet]
    public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
    {
        string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
        HttpResponseMessage response = Request.CreateResponse();
        response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
        return response;
    }
}
```

Nyomja le az **F6** toobuild, és ellenőrizze a hello megoldás.

## <a name="publish-hello-project-tooazure"></a>Hello projekt tooAzure közzététele
Ez a lépés hello Visual Studio projekt közzétett tooAzure. Ez a lépés a videó hello 5:45 kezdődik.

toopublish hello projekt tooAzure, kattintson a jobb gombbal a hello **APIMAADDemo** a Visual Studio projekt, és válassza a **közzététel**. Hello alapértelmezett beállítások megtartásához a hello **webhely közzététele** párbeszédpanel megnyitásához, és kattintson **közzététel**.

![Webes közzététel][api-management-web-publish]

## <a name="grant-permissions-toohello-azure-ad-backend-service-application"></a>Engedélyek toohello az Azure AD szolgáltatás a háttéralkalmazás
Egy új hello háttérszolgáltatás-alkalmazást az Azure AD-címtár és a webes API-projekt közzétételi folyamat hello konfigurálása részét képező jön létre. Ebben a lépésben hello videót 6:13, kezdődő engedélyekkel toohello webes API háttéralkalmazás.

![Alkalmazás][api-management-aad-backend-app]

Kattintson a szükséges hello tooconfigure hello Alkalmazásengedélyek hello nevére. Keresse meg a toohello **konfigurálása** lapot, és görgessen lefelé toohello **engedélyek tooother alkalmazások** szakasz. Kattintson a hello **Alkalmazásengedélyek** melletti legördülő **Windows** **Azure Active Directory**, hello jelölőnégyzetet a **címtáradatokolvasása**, és kattintson a **mentése**.

![Engedélyek hozzáadása][api-management-aad-add-permissions]

> [!NOTE]
> Ha **Windows** **Azure Active Directory** van engedélyek tooother alkalmazások nem szereplő, kattintson a **alkalmazás hozzáadása** , és adja hozzá a hello listából.
> 
> 

Jegyezze fel a hello **App Id URI** használható egy későbbi lépésben, ha egy Azure AD-alkalmazást úgy van konfigurálva hello API Management fejlesztői portálján.

![App Id URI][api-management-aad-sso-uri]

## <a name="import-hello-web-api-into-api-management"></a>Webes API hello importálja az API Management
API-k hello Azure portálon keresztül érhető el hello API publisher portálon vannak konfigurálva. tooreach, kattintson a **Publisher portal** hello eszköztáron az API Management szolgáltatás. Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [kezelése az első API] [ Manage your first API] oktatóanyag.

![Közzétevő portál][api-management-management-console]

A műveleteket lehet [tooAPIs manuálisan hozzáadni](api-management-howto-add-operations.md), vagy az importálható lesz. Ez a videó műveletek importált 6:40 kezdődő Swagger formátumú.

Hozzon létre egy fájlt `calcapi.json` a következő tartalmát, és mentse azt tooyour számítógép. Győződjön meg arról, hogy hello `host` attribútum tooyour webes API háttéralkalmazás mutat. Ebben a példában `"host": "apimaaddemo.azurewebsites.net"` szolgál.

```json
{
  "swagger": "2.0",
  "info": {
    "title": "Calculator",
    "description": "Arithmetics over HTTP!",
    "version": "1.0"
  },
  "host": "apimaaddemo.azurewebsites.net",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/add?a={a}&b={b}": {
      "get": {
        "description": "Responds with a sum of two numbers.",
        "operationId": "Add two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>51</code>.",
            "required": true,
            "type": "string",
            "default": "51",
            "enum": [
              "51"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>49</code>.",
            "required": true,
            "type": "string",
            "default": "49",
            "enum": [
              "49"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/sub?a={a}&b={b}": {
      "get": {
        "description": "Responds with a difference between two numbers.",
        "operationId": "Subtract two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>50</code>.",
            "required": true,
            "type": "string",
            "default": "50",
            "enum": [
              "50"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/div?a={a}&b={b}": {
      "get": {
        "description": "Responds with a quotient of two numbers.",
        "operationId": "Divide two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>100</code>.",
            "required": true,
            "type": "string",
            "default": "100",
            "enum": [
              "100"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          }
        ],
        "responses": { }
      }
    },
    "/mul?a={a}&b={b}": {
      "get": {
        "description": "Responds with a product of two numbers.",
        "operationId": "Multiply two integers",
        "parameters": [
          {
            "name": "a",
            "in": "query",
            "description": "First operand. Default value is <code>20</code>.",
            "required": true,
            "type": "string",
            "default": "20",
            "enum": [
              "20"
            ]
          },
          {
            "name": "b",
            "in": "query",
            "description": "Second operand. Default value is <code>5</code>.",
            "required": true,
            "type": "string",
            "default": "5",
            "enum": [
              "5"
            ]
          }
        ],
        "responses": { }
      }
    }
  }
}
```

tooimport hello Számológép API, kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **importálási API**.

![API importálása gomb][api-management-import-api]

Hajtsa végre a következő lépéseket tooconfigure hello Számológép API hello.

1. Kattintson a **fájlból**, keresse meg a toohello `calculator.json` fájlt mentette, majd kattintson a hello **Swagger** választógombot.
2. Típus **Számológép** történő hello **webes API URL-címe utótag** szövegmező.
3. Kattintson a hello **(nem kötelező) termékek** listát, és válassza **alapszintű**.
4. Kattintson a **mentése** tooimport hello API.

![Új API hozzáadása][api-management-import-new-api]

Hello API importálása után hello hello API az összefoglalás lapon megjelenik hello publisher portálon.

## <a name="call-hello-api-unsuccessfully-from-hello-developer-portal"></a>Hello API sikertelenül hívható hello developer portálról
Ezen a ponton hello API API Management importálták, de nem még hívható sikeresen hello developer portálról mert hello háttérszolgáltatást védi az Azure AD-alapú hitelesítés. Ezt mutatják be hello videó kezdődő 7:40 hello lépések használatával.

Kattintson a **fejlesztői portálján** hello publisher portál jobb felső oldalán hello.

![Fejlesztői portál][api-management-developer-portal-menu]

Kattintson a **API-k** hello kattintson **Számológép** API.

![Fejlesztői portál][api-management-dev-portal-apis]

Kattintson a **kipróbálás**.

![Kipróbálom][api-management-dev-portal-try-it]

Kattintson a **küldése** meg és jegyezze fel a hello állapotú **401 nem engedélyezett**.

![Küldés][api-management-dev-portal-send-401]

hello kérelem nem engedélyezett, mert hello háttér-API Azure Active Directory védi. Sikeresen megtörtént a hello API hívása előtt hello fejlesztői portálján konfigurált tooauthorize fejlesztők OAuth 2.0 használatával kell lennie. Ez a folyamat a következő szakaszok hello leírását.

## <a name="register-hello-developer-portal-as-an-aad-application"></a>Hello fejlesztői portálján regisztrálható egy AAD-alkalmazást
hello első lépés az OAuth 2.0 használatával hello developer portálon tooauthorize fejlesztők tooregister hello fejlesztői portálján, mint egy AAD-alkalmazást. Ezt mutatják 8:27 hello videó kezdve.

Keresse meg az ebben a példában ez a videó első lépése hello toohello az Azure AD bérlő **APIMDemo** , és keresse meg a toohello **alkalmazások** fülre.

![Új alkalmazás][api-management-aad-new-application-devportal]

Kattintson a hello **Hozzáadás** toocreate egy új Azure Active Directory-alkalmazás gombra, és válassza a **a szerveztem által fejlesztett alkalmazás hozzáadása**.

![Új alkalmazás][api-management-new-aad-application-menu]

Válasszon **webes alkalmazáshoz és/vagy webes API**, adjon meg egy nevet, majd kattintson a Tovább nyílra hello. Ebben a példában **APIMDeveloperPortal** szolgál.

![Új alkalmazás][api-management-aad-new-application-devportal-1]

A **bejelentkezési URL-cím** meg hello URL-címet a API Management szolgáltatás és a hozzáfűző `/signin`. Ebben a példában `https://contoso5.portal.azure-api.net/signin` szolgál.

A **azonosító URL-címet** meg hello URL-címet a API Management szolgáltatás és a hozzáfűző néhány egyedi karaktert. Ezek lehetnek a kívánt karaktereket, és ebben a példában `https://contoso5.portal.azure-api.net/dp` szolgál. Ha hello szükséges **alkalmazás tulajdonságainak** vannak konfigurálva, kattintson a hello pipa toocreate hello alkalmazásra.

![Új alkalmazás][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Az API Management OAuth 2.0 hitelesítési kiszolgáló konfigurálása
következő lépés hello tooconfigure az OAuth 2.0 hitelesítési kiszolgáló az API Management. Ez a lépés mutatják be hello videó 9:43 kezdve.

Kattintson a **biztonsági** hello bal oldali hello API Management menüben kattintson **OAuth 2.0**, és kattintson a **adja hozzá az engedélyezési** kiszolgáló.

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server]

Adjon meg egy nevet és leírást nem kötelező hello **neve** és **leírás** mezőket. A mezők kitöltése használt tooidentify hello OAuth 2.0 hitelesítési kiszolgáló belül hello API Management service-példány. Ebben a példában **engedélyezési server bemutató** szolgál. Később egy API-hitelesítéshez használt OAuth 2.0-kiszolgáló toobe megadásakor kiválaszthatja ezt a nevet.

A hello **ügyfél regisztrációs URL-címe** adja meg például a helyőrző értékét `http://localhost`.  Hello **ügyfél regisztrációs URL-címe** toohello lapon, hogy a felhasználók pontok toocreate használhatja és a saját felhasználói konfigurálása a felhasználói fiókok kezelését támogató OAuth 2.0-s szolgáltatók. Ebben a példában a felhasználók létrehozása és nem a saját felhasználói konfigurálása, így helyőrzőjeként szolgál.

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-1]

Ezt követően adja meg **engedélyezési végpont URL-címet** és **végponti URL-cím Token**.

![engedélyezési kiszolgáló][api-management-add-authorization-server-1a]

Ezek az értékek lekérhetők hello **App végpontok** hello hello fejlesztői portálon létrehozott AAD-alkalmazást oldalán. tooaccess hello végpontok lépjen toohello **konfigurálása** lapján hello AAD-alkalmazást, és kattintson a **végpontok megtekintése**.

![Alkalmazás][api-management-aad-devportal-application]

![Végpontok megtekintése][api-management-aad-view-endpoints]

Másolás hello **OAuth 2.0 hitelesítési végpont** és illessze be hello **engedélyezési végpont URL-címet** szövegmező.

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-2]

Másolás hello **OAuth 2.0 token-végpont** és illessze be hello **végponti URL-cím Token** szövegmező.

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-2a]

Továbbá a jogkivonat végpontjához hello toopasting hozzáadása egy további válaszüzenettörzs-paramétert nevű **erőforrás** és hello értékhez használja a hello **App Id URI** hello hello háttérszolgáltatást, de a AAD-alkalmazást a Visual Studio-projekt hello közzétételekor létrehozott.

![App Id URI][api-management-aad-sso-uri]

Ezt követően adja meg a hello ügyfél hitelesítő adatait. Ezek a kívánt erőforrás hello hello hitelesítő adatainak tooaccess, ebben az esetben hello fejlesztői portálján.

![Ügyfél hitelesítő adatait][api-management-client-credentials]

tooget hello **ügyfél-azonosító**, keresse meg a toohello **konfigurálása** hello hello developer portálon, és másolja hello AAD-alkalmazást lapján **ügyfél-azonosító**.

tooget hello **Ügyfélkulcs** hello kattintson **válassza ki a duration** hello legördülő **kulcsok** szakaszt, és adjon meg egy időközt. Ebben a példában 1 év szolgál.

![Ügyfél-azonosító][api-management-aad-client-id]

Kattintson a **mentése** toosave hello konfigurációs és megjelenítési hello kulcs. 

> [!IMPORTANT]
> Jegyezze fel ezt a kulcsot. Hello Azure Active Directory konfigurációs ablak bezárása után hello kulcs nem jeleníthető meg újra.
> 
> 

Másolás hello kulcs toohello vágólap, kapcsoló hátsó toohello publisher portal hello kulcs beillesztése hello **Ügyfélkulcs** szövegmező, és kattintson a **mentése**.

![Engedélyezési kiszolgáló hozzáadása][api-management-add-authorization-server-3]

Azonnal hello ügyfél hitelesítő adatait a következő kód egy hitelesítésengedélyezési. Az engedélyezési kód másolja, majd kapcsoló hátsó tooyour az Azure AD fejlesztői portál alkalmazás konfigurálása lap, és hello hitelesítésengedélyezési beillesztheti hello **válasz URL-CÍMEN** mezőben, majd kattintson a **mentése** újra.

![Válasz URL-címe][api-management-aad-reply-url]

következő lépés hello tooconfigure hello engedélyeinek hello fejlesztői portálján AAD-alkalmazást. Kattintson a **Alkalmazásengedélyek** és hello jelölőnégyzetet a **címtáradatok olvasása**. Kattintson a **mentése** toosave ez módosítsa, majd kattintson a **alkalmazás hozzáadása**.

![Engedélyek hozzáadása][api-management-add-devportal-permissions]

Hello keresés ikonra, kattintson típus **APIM** történő hello kezdve mezőben, válassza ki **APIMAADDemo**, és kattintson a pipa jelre toosave hello.

![Engedélyek hozzáadása][api-management-aad-add-app-permissions]

Kattintson a **delegált engedélyek** a **APIMAADDemo** és hello jelölőnégyzetet a **hozzáférés APIMAADDemo**, és kattintson a **mentése**. Ez lehetővé teszi, hogy hello fejlesztői portálalkalmazás tooaccess hello háttérszolgáltatáshoz.

![Engedélyek hozzáadása][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-hello-calculator-api"></a>A Számológép API hello OAuth 2.0 felhasználó engedélyezése
Most, hogy hello OAuth 2.0-kiszolgálót, adja meg azt a biztonsági beállítások hello az API-t. Ez a lépés mutatják be hello videó kezdő pozíció: 14:30.

Kattintson a **API-k** a bal oldali menü hello, és kattintson a **Számológép** tooview, és konfigurálja a beállításokat.

![A Számológép API][api-management-calc-api]

Keresse meg a toohello **biztonsági** fület, ellenőrizze a hello **OAuth 2.0** jelölőnégyzetet, jelölje be hello kívánt engedélyezési kiszolgáló hello **engedélyezési kiszolgáló** legördülő, kattintson **Mentése**.

![A Számológép API][api-management-enable-aad-calculator]

## <a name="successfully-call-hello-calculator-api-from-hello-developer-portal"></a>Sikeresen meg tudja hívni hello Számológép API hello developer portálról
Most, hogy hello OAuth 2.0 hitelesítési hello API konfigurálva van, műveletei sikeresen hívhatók hello fejlesztői központból. Ez a lépés mutatják be hello videó 15:00-tól kezdve.

Keresse meg a visszafelé toohello **két egész számok hozzáadása** hello Számológép szolgáltatás hello fejlesztői portálján, majd kattintson a művelet **kipróbálás**. Megjegyzés: hello új elem a hello **engedélyezési** szakasz megfelelő toohello engedélyezési kiszolgáló az előzőekben adott hozzá.

![A Számológép API][api-management-calc-authorization-server]

Válassza ki **engedélyezési kód** hello engedély legördülő listában, és adja meg hello fiók toouse hello hitelesítő adatait. Ha már nem kérheti hello fiókkal bejelentkezve.

![A Számológép API][api-management-devportal-authorization-code]

Kattintson a **küldése** és megjegyzés hello **válaszállapot** a **200 OK** és hello eredmények hello művelet hello válasz tartalma.

![A Számológép API][api-management-devportal-response]

## <a name="configure-a-desktop-application-toocall-hello-api"></a>Egy asztali alkalmazás toocall hello API konfigurálása
a videó hello hello a következő eljárással kezdődik, 16:30, és egy egyszerű asztali alkalmazás toocall hello API konfigurálja. hello első lépéseként tooregister hello asztali alkalmazás az Azure ad-ben, és adjon neki toohello könyvtár és toohello háttér szolgáltatás. 18:25 nincs bemutatója hello asztali alkalmazás egy műveletet a hello Számológép API hívása.

## <a name="configure-a-jwt-validation-policy-toopre-authorize-requests"></a>Konfigurálja a JWT-ellenőrzési házirend toopre-kérések engedélyezésére
hello hello videó az utolsó eljárás 20:48 kezdődik, és bemutatja, hogyan toouse hello [érvényesítése JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) házirend toopre-kérelmek hitelesítéséhez érvényesítésével megjeleníthető az egyes bejövő kérelmek hello hozzáférési jogkivonatok. Hello kérelem nem érvényesítette hello érvényesítése JWT házirend, ha hello kérelem API Management le van tiltva, és nem kerül át toohello háttér mentén.

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
    <required-claims>
        <claim name="aud">
            <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
        </claim>
    </required-claims>
</validate-jwt>
```

Egy másik bemutató konfigurálása, és ez a házirend használatával, lásd: [felhő fedik le a epizód 177: több API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és too13:50 előre. Gyors toosee hello házirendeket a Helyicsoportházirend-szerkesztő hello az too15:00 továbbítja, és majd a művelet hívásának hello developer portálról vagy anélkül hello bemutatója too18:50 szükséges engedélyezési jogkivonat.

## <a name="next-steps"></a>Következő lépések
* Tekintse meg több [videók](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API-kezeléssel kapcsolatos.
* A más módon toosecure a háttérszolgáltatás, lásd: [kölcsönös tanúsítványhitelesítés](api-management-howto-mutual-certificates.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md
