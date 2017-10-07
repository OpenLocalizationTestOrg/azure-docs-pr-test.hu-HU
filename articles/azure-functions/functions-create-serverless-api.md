---
title: "az Azure Functions használatával egy kiszolgáló nélküli API aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate egy kiszolgáló nélküli API-t az Azure Functions használatával"
services: functions
author: mattchenderson
manager: erikre
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 877e3b229d5477fc5fec594ccd284fb55d7f3c07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a>Az Azure Functions használatával egy kiszolgáló nélküli API létrehozása

Ebből az oktatóanyagból megtudhatja, hogyan Azure Functions lehetővé teszi a toobuild kiválóan méretezhető API-k. Az Azure Functions rendelkezik beépített HTTP eseményindítók és kötések, amelyek révén könnyen tooauthor gyűjteménye a különböző nyelveken, beleértve a Node.JS, a C#, és több végpont. Ebben az oktatóanyagban testre fogja szabni egy HTTP eseményindító toohandle konkrét műveletek az API kialakításában. Is előkészítheti a növekvő az API integrálása az Azure Functions proxyk és utánzatait API-k beállítása. Mindez valósítható meg hello funkciók kiszolgáló nélküli számítási környezet felett, erőforrások méretezésével kapcsolatos tooworry nincs – a API logika csak összpontosíthat.

## <a name="prerequisites"></a>Előfeltételek 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Ez az oktatóanyag hello részeinek hello eredményül kapott függvény fog történni.

### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

Nyissa meg hello Azure-portálon. toodo, jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com) fel fiókjával.

## <a name="customize-your-http-function"></a>A HTTP-funkció testreszabása

Alapértelmezés szerint a HTTP-eseményindítóval aktivált függvény konfigurált tooaccept HTTP-metódus. Emellett van alapértelmezett URL-cím hello `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`. Ha hello gyors üzembe helyezés, majd követi `<funcname>` valószínűleg valahogy "HttpTriggerJS1". Ebben a szakaszban hello függvény toorespond csak tooGET kéréseket a meghatározott módosítja `/api/hello` útvonal helyett. 

Keresse meg a hello Azure-portálon tooyour függvényt. Válassza ki **integráció** a bal oldali navigációs hello.

![Egy HTTP-funkció testreszabása](./media/functions-create-serverless-api/customizing-http.png)

Használja a HTTP-eseményindító beállítások hello táblázatban megadottak szerint.

| Mező | Mintaérték | Leírás |
|---|---|---|
| Engedélyezett HTTP-metódusok | A kijelölt metódusok | Meghatározza, hogy mely HTTP-metódus lehet használt tooinvoke ezt a függvényt |
| A kijelölt HTTP-metódusok | GET | Lehetővé teszi, hogy csak kijelölt HTTP módszerek toobe tooinvoke e funkció használata |
| Útvonal-sablon | hello | Meghatározza, hogy milyen útvonal használt tooinvoke ezt a függvényt |

Vegye figyelembe, hogy Ön nem foglal hello `/api` kiinduló hello útvonalsablonhoz, elérési út előtagot, mivel ez egy globális beállítás kezeli.

Kattintson a **Save** (Mentés) gombra.

A HTTP-funkciók testreszabása többet is megtudhat [Azure Functions HTTP és a webhook kötések](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).

### <a name="test-your-api"></a>Az API-teszt

A következő tesztelheti a függvény toosee hello új API felület használata.

Keresse meg a visszafelé toohello fejlesztési lap bal oldali navigációs hello hello függvény nevére kattintva.

Kattintson a **függvény URL-cím beszerzése** és hello URL-Címének másolása. Láthatja, hogy az általa használt hello `/api/hello` címre.

Hello URL-cím másolása egy új böngészőlapon vagy a kívánt REST-ügyfél. Böngészők alapértelmezés szerint fogja használni a GET.

Hello funkció futtatását, és győződjön meg arról, hogy működik. Szükség lehet tooprovide hello "név" paraméternek meg egy lekérdezési karakterlánc toosatisfy hello gyors üzembe helyezés.

A HTTP-metódus egy másik tooconfirm hello függvény végrehajtása nem hello-végpont meghívása is próbálkozhat. Ehhez szüksége lesz egy, a többi ügyfél, például cURL, a Postman vagy a Fiddler toouse.

## <a name="proxies-overview"></a>Proxyk áttekintése

Hello a következő szakaszban az API-proxyn keresztül fog surface. Az Azure Functions proxyk, amely lehetővé teszi tooforward kérelmek tooother erőforrások előzetes verziójú funkciók. Adhat meg egy HTTP-végpont például csak a HTTP-eseményindítóval, de ahelyett kód tooexecute adott végpontra metódus meghívásakor, akkor implementálásához URL-cím tooa távoli. Ez lehetővé teszi több API forrásokból történő egyetlen API felülete, amelyek segítségével az ügyfelek tooconsume toocompose. Ez különösen fontos, ha toobuild mikroszolgáltatások létrehozására, az API-t.

Egy is proxypont tooany HTTP erőforrás, például:
- Azure Functions 
- Az API-alkalmazások [az Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- A docker-tároló [Linux App Service](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)
- Más üzemeltetett API-k

toolearn proxykat, kapcsolatos további információkért lásd: [használata az Azure Functions proxyk (előzetes verzió)].

## <a name="create-your-first-proxy"></a>Az első proxy létrehozása

Ez a szakasz hoz létre egy új proxy mely szolgál, egy előtér-tooyour általános API. 

### <a name="setting-up-hello-frontend-environment"></a>Hello előtér környezet beállítása

Ismételje meg a hello túl[függvény-alkalmazás létrehozása](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate egy új funkció alkalmazást a amely hoz létre a proxy. Az új alkalmazás erre a célra hello előtér az API-hoz, és korábban Szerkesztés hello függvény alkalmazás erre a célra egy háttér.

Keresse meg a tooyour új előtér függvény app hello portálon.

Válassza ki **beállítások**. Majd átváltása **engedélyezése Azure Functions proxyk (előzetes verzió)** túl "a".

Válassza ki **Platform beállítások** válassza **Alkalmazásbeállítások**.

Görgessen lefelé, túl**Alkalmazásbeállítások** , és hozzon létre egy új beállítás "HELLO_HOST" kulcs. Állítsa be a háttérkiszolgáló függvény alkalmazáshoz, a érték toohello gazdagépe pl. `<YourApp>.azurewebsites.net`. Ez már korábban átmásolt a HTTP-függvény tesztelése során hello URL-cím része. Ezt a beállítást később hello konfigurációban fog hivatkozni.

> [!NOTE] 
> Alkalmazásbeállítások ajánlottak hello gazdagép konfigurációs tooprevent hello proxy kódolt környezet függőségei. Alkalmazásbeállítások használata azt jelenti, hogy hello proxykonfigurációt áthelyezheti különböző környezetek között, hello környezetfüggő app beállítások lesznek alkalmazva.

Kattintson a **Save** (Mentés) gombra.

### <a name="creating-a-proxy-on-hello-frontend"></a>A proxy hello előtér létrehozása

Lépjen vissza tooyour előtér függvény app hello portálon.

Hello bal oldali navigációs, kattintson a plusz jelre "+" következő hello túl "Proxyk (előzetes verzió)".

![A proxy létrehozása](./media/functions-create-serverless-api/creating-proxy.png)

Proxybeállításainak használata hello táblázatban megadottak szerint.

| Mező | Mintaérték | Leírás |
|---|---|---|
| Név | HelloProxy | Rövid név csak olyan felügyeleti |
| Útvonal-sablon | / api/hello | Meghatározza, hogy milyen útvonal használt tooinvoke a proxy |
| Háttérkiszolgáló URL-címe | https://%HELLO_HOST%/API/hello | Megadja a hello végpont toowhich hello kérelem küldése a proxyn keresztül kell lennie. |

Ne feledje, hogy a proxyk nem nyújtanak hello `/api` alap elérési útja előtag és a hello útvonalsablonhoz szerepelnie kell.

Hello `%HELLO_HOST%` szintaxis használatával a korábban létrehozott hello Alkalmazásbeállítás hivatkozik. hello megoldás URL-címet fog mutatni tooyour eredeti funkciókat.

Kattintson a **Create** (Létrehozás) gombra.

Az új proxy kipróbálhatja hello Proxy URL-cím másolásával és a tesztelés hello böngészőben vagy a kedvenc HTTP-ügyféllel.

## <a name="create-a-mock-api"></a>Egy utánzatait API létrehozása

A proxy toocreate utánzatait API a következő megoldást fogja használni. Ez lehetővé teszi az ügyfél fejlesztési tooprogress, anélkül, hogy teljesen végre hello háttér. Később a fejlesztési hozzon létre egy új függvény alkalmazást, amely támogatja a logikai és a proxy tooit átirányítási sikerült.

toocreate ez mock API, létre fogunk hozni egy új proxy ezúttal hello [App Service-szerkesztő](https://github.com/projectkudu/kudu/wiki/App-Service-Editor). tooget indult el, keresse meg a tooyour függvény app hello portálon. Válassza ki **Platform funkciói** és található **App Service-szerkesztő**. A hivatkozásra kattintva az megnyílik az App Service-szerkesztő hello új lapon.

Válassza ki `proxies.json` a bal oldali navigációs hello. Ez az hello fájlt, amely az összes a proxyk hello konfigurációs tárolja. Hello használatakor [működik a központi telepítési módszerekkel](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), hello fájl verziókezelő rendszer megmaradjanak. További információ a fájl toolearn lásd: [proxyk speciális konfigurációs](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).

Ha végrehajtotta eddig, a proxies.json hello hasonlóan kell kinéznie:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

Ezután a utánzatait API fogja hozzáadni. Cserélje le a proxies.json fájl hello következő:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

Ezzel hozzáad egy új proxy, "GetUserByName" hello backendUri tulajdonság nélkül. Helyett egy másik erőforrás, módosítja egy válasz felülbírálás használatával proxyk hello alapértelmezett válaszát. Kérelem és válasz felülbírálások használhatók a URL-t együtt. Ez akkor különösen hasznos, ha a proxy használatát az ügynökökön tooa örökölt rendszer, amikor szükség lehet toomodify fejlécek, lekérdezési paraméterek, toolearn stb. További információk a kérelem-válasz felülbírálások, lásd a [kérelmeit és válaszait a proxyk módosítása](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).

Tesztelje az utánzatait API-t hívó hello `/api/users/{username}` végpont egy böngésző vagy a kedvenc REST-ügyfél használatával. Lehet, hogy tooreplace _{username}_ egy karakterláncértéket, amely egy felhasználónévvel rendelkező.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban, megtudta, hogyan toobuild és testreszabása az Azure Functions egy API-t. Is megtanulta, hogyan toobring több API-k, beleértve a mocks, együtt, egy egyesített API felületen. Használhat ezen módszerek toobuild kimenő bármely összetettségi API-k, a kiszolgáló nélküli hello futtatott összes számítási modellt az Azure Functions által biztosított.

hello következő hivatkozásokat hasznos lehet, az API-további fejleszt:

- [Az Azure Functions HTTP és a webhook kötések](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- [használata az Azure Functions proxyk (előzetes verzió)]
- [Az Azure Functions API-k (előzetes verzió) dokumentálása](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[használata az Azure Functions proxyk (előzetes verzió)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
