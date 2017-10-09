---
title: az Azure Functions proxykkal aaaWork |} Microsoft Docs
description: Hogyan toouse Azure Functions proxyk
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.openlocfilehash: 4d94c89e8f8f2d2c771b01bae142bf9a4f3b7f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a>Együttműködik az Azure Functions proxyk (előzetes verzió)

> [!NOTE] 
> Az Azure Functions proxyk jelenleg előzetes verzió. Szabad, amíg preview, de a szabványos függvényekben számlázási tooproxy végrehajtások vonatkozik. További információkért lásd: [Azure Functions díjszabási](https://azure.microsoft.com/pricing/details/functions/).

Ez a cikk azt ismerteti, hogyan tooconfigure és a munkahelyi Azure Functions proxykkal. Ez a szolgáltatás végpontok is megadhat, a függvény alkalmazások, amelyeket a rendszer egy másik erőforrás. Ezek proxyk toobreak nagy API több függvény alkalmazásokba (például egy mikroszolgáltatási architektúra), miközben továbbra is egyetlen API felülete ügyfelek is használhatja.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <a name="enable"></a>Az Azure Functions proxyk engedélyezése

Proxyk alapértelmezés szerint nem engedélyezettek. Proxyk hozhat létre, amikor hello szolgáltatás le van tiltva, de nem hajtja végre. tooenable proxyk hello a következő:

1. Nyissa meg hello [Azure-portálon], és folytassa a tooyour függvény alkalmazást.
2. Válassza ki **Alkalmazásbeállítások működéséhez**.
3. Kapcsoló **engedélyezése Azure Functions proxyk (előzetes verzió)** túl**a**.

Is visszatérhet ide tooupdate hello proxyt futtatókörnyezet amint elérhetővé válnak az új szolgáltatásokat.


## <a name="create"></a>A proxy létrehozása

Ez a szakasz bemutatja, hogyan toocreate proxyval a hello Functions portálra.

1. Nyissa meg hello [Azure-portálon], és folytassa a tooyour függvény alkalmazást.
2. Hello bal oldali ablaktáblában jelöljön ki **új proxy**.
3. Adja meg a proxykiszolgáló nevét.
4. Hello végpontot, amely fel van fedve konfigurálása az függvény alkalmazás hello megadásával **útvonalsablonhoz** és **HTTP-metódus**. Ezek a paraméterek viselkednek függően toohello szabályainak [HTTP eseményindítók].
5. Set hello **háttérkiszolgáló URL-cím** tooanother végpont. Ehhez a végponthoz lehet egy másik függvény alkalmazásban függvény, vagy más API-k lehet. hello érték nem kell toobe statikus, és azt is hivatkozást [Alkalmazásbeállítások] és [hello eredeti ügyfélkérés származó paraméterek].
6. Kattintson a **Create** (Létrehozás) gombra.

A proxy most már létezik a függvény alkalmazás egy új végponton. Az ügyfél szempontjából egyenértékű tooan az Azure Functions HttpTrigger. Az új proxy kipróbálhatja hello Proxy URL-cím másolásával, és vizsgálja, hogy a kedvenc HTTP-ügyféllel.

## <a name="modify-requests-responses"></a>Kérelem és válasz módosítása

Az Azure Functions proxykat módosíthatja a kérelmek tooand válaszok hello háttérből. Az átalakításokat változókat is használhat, a [változókkal].

### <a name="modify-backend-request"></a>Hello háttér-kérelem módosítása

Alapértelmezés szerint hello háttér-kérelem hello eredeti kérelem másolatként inicializálása. Továbbá toosetting hello háttér-URL-címet, hogy módosításokat toohello HTTP-metódus, a fejlécek és a lekérdezési karakterlánc paraméterek. hello módosult értékek hivatkozhatnak [Alkalmazásbeállítások] és [hello eredeti ügyfélkérés származó paraméterek].

Jelenleg nincs portál élmény, a háttér-kérelmek módosítását. toolearn hogyan tooapply ezt a képességet a proxies.json, lásd: [megadása egy requestOverrides objektum].

### <a name="modify-response"></a>Hello válasz módosítása

Alapértelmezés szerint a hello ügyfél válaszára inicializálva van hello háttér-válasz egy másolatát. Módosítások toohello válasz állapotkódja, indoklás, fejlécek és body végezheti el. hello módosult értékek hivatkozhatnak [Alkalmazásbeállítások], [hello eredeti ügyfélkérés származó paraméterek], és [hello háttér-válasz származó paraméterek].

Jelenleg nincs portál élmény a válaszok módosítását. toolearn hogyan tooapply ezt a képességet a proxies.json, lásd: [megadása egy responseOverrides objektum].

## <a name="using-variables"></a>Változók használata

a proxy konfigurációját hello nem kell toobe statikus. Akkor is a feltétel az toouse változók hello eredeti kérelem, hello háttér-válasz vagy alkalmazás beállításait.

### <a name="request-parameters"></a>Hivatkozás a kérelemben szereplő paraméterek

A kérelemben szereplő paraméterek toohello háttér-URL-cím tulajdonsága bemeneti, illetve a kérelem és válasz módosítása részeként használható. Egyes paraméterek sablonból hello útvonal hello alap proxykonfigurációt megadott köthető, és mások hello bejövő kérelem tulajdonságainak származhatnak.

#### <a name="route-template-parameters"></a>Útvonal-Sablonparaméterek
Hello útvonalsablonhoz használt paraméterei elérhető toobe hivatkozás a név alapján. hello paraméternevei vannak kapcsos zárójelek között ({}).

Például, ha a proxy például rendelkezik egy útvonalsablonhoz `/pets/{petId}`, hello háttér-URL-címet tartalmazhat hello értékének `{petId}`, mint a `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Ha hello útvonalsablonhoz megszakítja a helyettesítő karakter, például a `/api/{*restOfPath}`, érték hello `{restOfPath}` egy bejövő hello kérelemből szegmenst fennmaradó hello karakterláncos ábrázolása.

#### <a name="additional-request-parameters"></a>A további kérelemben szereplő paraméterek
Ezenkívül toohello útvonal-Sablonparaméterek, konfigurációs értékeire hello a következő értékek használhatók:

* **{request.method}** : hello hello eredeti kérés használt HTTP-metódus.
* **{request.headers. \<Fejléc neve\>}**: hello eredeti kérelemből olvasható fejléc. Cserélje le  *\<fejléc neve\>*  hello névvel, amelyet az tooread hello fejléc. Ha hello fejléce nem szerepel-e hello kérésre, hello értéket fogja hello üres karakterlánc.
* **{request.querystring. \<ParameterName\>}**: A lekérdezési karakterlánc paraméterként hello eredeti kérelemből olvasható. Cserélje le  *\<ParameterName\>*  hello névvel, amelyet az tooread hello paraméter. Ha hello paraméter nem tartalmaz hello kérésre, hello értéket fogja hello üres karakterlánc.

### <a name="response-parameters"></a>Háttér-válasz paraméterek

Válasz paraméterek módosítása hello válasz toohello ügyfél részeként használhatók. a következő értékek hello a konfigurációs értékek használhatók:

* **{backend.response.statusCode}** : hello háttér-válasz visszaadott HTTP-állapotkód: hello.
* **{backend.response.statusReason}** : hello HTTP indoklás, amely akkor adja vissza a hello háttér-válasz.
* **{backend.response.headers. \<Fejléc neve\>}**: egy fejléc a hello háttér-válaszban olvasható. Cserélje le  *\<fejléc neve\>*  hello fejléc hello nevű tooread szeretné. Ha hello fejléce nem szerepel-e hello kérésre, hello értéket fogja hello üres karakterlánc.

### <a name="use-appsettings"></a>Referencia-Alkalmazásbeállítások

Is hivatkozhat [hello függvény alkalmazás definiált Alkalmazásbeállítások](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) által körülvevő hello beállítás nevét százalékjelek (%).

Például a háttér-URL-t *https://%ORDER_PROCESSING_HOST%/api/orders* kellene "% ORDER_PROCESSING_HOST %" hello ORDER_PROCESSING_HOST beállítási hello érték helyett.

> [!TIP] 
> Háttér-gazdagépek Alkalmazásbeállítások használni, ha több központi telepítését vagy tesztelési környezetben. Így biztosíthatja, hogy mindig beszélünk toohello jobb vissza az adott környezet befejezéséhez.

## <a name="advanced-configuration"></a>Speciális konfiguráció

hello proxyk konfigurált függvény app könyvtár hello gyökerében található proxies.json fájlban tárolódnak. Manuálisan szerkessze a fájlt és központi telepítése az alkalmazás részeként hello használatakor [telepítési módszerek](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) a Functions támogatja. hello szolgáltatást kell [engedélyezett](#enable) a hello fájl toobe feldolgozása. 

> [!TIP] 
> Ha nem állított be hello központi telepítési módszerekkel, akkor is hello proxies.json fájl használatához hello portálon. Nyissa meg tooyour függvény alkalmazást, jelölje be **Platform funkciói**, majd válassza ki **App Service-szerkesztő**. Ezzel a módszerrel hello teljes fájlstruktúra függvény alkalmazása megtekintheti és majd a módosításokat.

Proxies.JSON határozza meg a proxyk objektum, amely megnevezett proxyk és a definíciójukat. Nem kötelező, ha a szerkesztő lehetővé teszi, melyeket referenciaként használhat egy [JSON-séma](http://json.schemastore.org/proxies) kód befejezésére. Egy példa fájl hello következő nézhet ki:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Minden egyes proxy van egy rövid nevet, például a *proxy1* a fenti példa hello. hello megfelelő proxy definíciós objektumban hello a következő tulajdonságok határozzák meg:

* **matchCondition**: szükséges – az objektum meghatározása, amelynek hatására a proxy hello végrehajtásának hello kérelmeket. A megosztott két tulajdonságok tartalmaz [HTTP eseményindítók]:
    * _módszerek_: hello HTTP hello proxy módszerek tömb válaszol-e. Ha nincs megadva, hello proxy válaszol a HTTP-metódus tooall hello útvonalon.
    * _útvonal_: szükséges – hello útvonalsablonhoz, amely kérelem URL-címeket a proxy szabályozása meghatározása válaszol-e. Ellentétben a HTTP-eseményindítók, nincs alapértelmezett értéke.
* **backendUri**: hello URL-cím hello háttér-erőforrás toowhich hello kérelem küldése a proxyn keresztül kell lennie. Ez az érték hivatkozhat Alkalmazásbeállítások és paraméterek hello eredeti ügyfél kérelemből. Ha ez a tulajdonság nincs megadva, az Azure Functions válaszol egy HTTP 200 OK gombra.
* **requestOverrides**: átalakítások toohello háttér-kérelem definiáló objektum. Lásd: [megadása egy requestOverrides objektum].
* **responseOverrides**: átalakítások toohello ügyfél válaszára definiáló objektum. Lásd: [megadása egy responseOverrides objektum].

> [!NOTE] 
> hello útvonal tulajdonság Azure Functions proxyk nem veszi figyelembe hello routePrefix tulajdonsága hello funkciók a gazdagép konfigurálása. Ha azt szeretné, hogy egy címelőtagot, például /api tooinclude, akkor hello útvonal tulajdonság kell szerepelnie.

### <a name="requestOverrides"></a>Adja meg a requestOverrides objektum

hello requestOverrides objektum meghatározása változások toohello kérelem hello háttér-erőforrás neve. hello objektum következő tulajdonságai hello határozzák meg:

* **backend.Request.Method**: hello által használt toocall hello háttér HTTP-metódus.
* **backend.Request.QueryString. \<ParameterName\>**: A lekérdezési karakterlánc paraméterként, amely hello hívás toohello háttér állítható be. Cserélje le  *\<ParameterName\>*  hello névvel, amelyet az tooset hello paraméter. Ha üres karakterláncot hello áll rendelkezésre, hello paraméter hello háttér-kérelem nem tartalmazza.
* **backend.Request.Headers. \<Fejléc neve\>**: hello hívás toohello háttér beállítható fejléc. Cserélje le  *\<fejléc neve\>*  hello névvel, amelyet az tooset hello fejléc. Hello üres karakterláncot adjon meg, ha hello fejléce nem szerepel-e hello háttér-kérelemre.

Értékek hivatkozhatnak Alkalmazásbeállítások és paraméterek hello eredeti ügyfél kérelemből.

Hello következő egy példa konfiguráció látható:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>Adja meg a responseOverrides objektum

hello requestOverrides objektum, amely megfelelt hátsó toohello ügyfél toohello válasz végzett módosításokat határozza meg. hello objektum következő tulajdonságai hello határozzák meg:

* **response.statusCode**: hello HTTP-állapot kódját toobe toohello ügyfél adott vissza.
* **response.statusReason**: hello HTTP OK kifejezés toobe toohello ügyfél adott vissza.
* **Response.body**: hello karakterláncos ábrázolása törzsében toobe hello toohello ügyfél adott vissza.
* **Response.Headers. \<Fejléc neve\>**: hello válasz toohello ügyfél beállítható fejléc. Cserélje le  *\<fejléc neve\>*  hello névvel, amelyet az tooset hello fejléc. Hello üres karakterláncot adjon meg, ha hello fejléce nem hello válasz szerepel.

Értékek hivatkozhatnak alkalmazás beállításait, a paraméterek kérelemből hello eredeti ügyfél és a paraméterek a hello háttér-válaszban.

Hello következő egy példa konfiguráció látható:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> Ebben a példában hello törzs beállítása közvetlenül, ezért nem `backendUri` tulajdonság van szükség. hello példa bemutatja, hogyan használhatja Azure Functions proxyk mocking API-k esetében.

[Azure-portálon]: https://portal.azure.com
[HTTP eseményindítók]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify hello back-end request]: #modify-backend-request
[Modify hello response]: #modify-response
[megadása egy requestOverrides objektum]: #requestOverrides
[megadása egy responseOverrides objektum]: #responseOverrides
[Alkalmazásbeállítások]: #use-appsettings
[változókkal]: #using-variables
[hello eredeti ügyfélkérés származó paraméterek]: #request-parameters
[hello háttér-válasz származó paraméterek]: #response-parameters
