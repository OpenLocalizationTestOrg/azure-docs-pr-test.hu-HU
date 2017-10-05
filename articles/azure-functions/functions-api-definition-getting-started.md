---
title: "Ismerkedés az Azure Functions OpenAPI metaadatok a |} Microsoft Docs"
description: "Ismerkedés az Azure Functions OpenAPI támogatása"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: 9b861aacf31e17866293732dc2323f56014c1877
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Létrehozása OpenAPI (Swagger) 2.0 metaadatok függvény alkalmazások (előzetes verzió)

Ez a dokumentum végigvezeti az Azure Functions üzemeltetett egyszerű API-t egy OpenAPI definíciója létrehozásának részletes folyamatán.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Mi az a OpenAPI (Swagger)?
[Swagger-metaadatok](http://swagger.io/) egy olyan fájl, funkcióit és az API-működési módja határozza meg, és lehetővé teszi, hogy számos más szoftverek számára a REST API-t üzemeltető függvényt. Microsoft ajánlatokat, például PowerApps és [API-alkalmazások](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), valamint tooling eszköz, például a 3. fél fejlesztői [Postman](https://www.getpostman.com/docs/importing_swagger) és [számos további csomag](http://swagger.io/tools/) összes engedélyezése az API-t, a felhasznált egy Swagger-definíciójában.

## <a name="prepare-function"></a>A függvény létrehozásához egy egyszerű API-hoz
  Hozzon létre egy OpenAPI definícióját, először kell egy egyszerű API-val hozzon létre egy függvényt. Ha már rendelkezik egy, a függvény alkalmazások üzemeltetett API, akkor ugorjon rögtön a következő szakasz
1. Hozzon létre egy új funkció alkalmazást.
    1. [Azure-portálon](https://portal.azure.com)  >  `+ New` > "Függvény alkalmazás" keresése
1. Hozzon létre egy új HTTP eseményindító függvényt belül az új függvény-alkalmazás
    1. A függvény ki van töltve egy nagyon egyszerű REST API meghatározása kóddal.
    1. Bármilyen karakterlánc egy lekérdezési paraméterben, vagy a törzsben a függvénynek átadott "Hello {bemeneti}" adja vissza a rendszer
1. Lépjen a `Integrate` lap az új HTTP-eseményindítóval függvény
    1. Váltás `Allowed HTTP methods` számára`Selected methods`
    1. A `Selected HTTP methods` törli a jelölőnégyzet jelölését a FELADÁS egy vagy több kivételével minden műveletet.
    ![Kijelölt HTTP-metódus](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
    1. Ez a lépés leegyszerűsíti az API-definíció később.

## <a name="enable"></a>API Definition támogatásának engedélyezése
1. Navigáljon a `your function name`  >  `Platform Features`  >  `API Definition` 
 ![definíció lapon található.](./media/functions-api-definition-getting-started/definitiontab.png)
1. Állítsa be `API Definition Source` számára`Function (preview)`
    1. Ez a lépés lehetővé teszi, hogy az függvény alkalmazások, beleértve a függvény App tartományból egy beágyazott másolatot készít egy OpenAPI fájl futtatásához a végpont OpenAPI beállítások együttese a [OpenAPI szerkesztő](http://editor.swagger.io), és a gyors üzembe helyezés definition generátor.
![Engedélyezett meghatározása](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Az API-definíció létrehozása sablonból
1. Kattintson a következőre: `Generate API Definition template`
    1. Ebben a lépésben vizsgálatokat végez a HTTP-eseményindítóval funkciók függvény alkalmazást, és functions.json a biztonsági adatok segítségével egy OpenAPI dokumentum létrehozása.
1. A művelet objektum hozzáadása`paths: /api/yourfunctionroute post:`
    1. A gyors üzembe helyezés OpenAPI dokumentum egy teljes OpenAPI doc Vázlat. További metaadatokra ahhoz, hogy a teljes OpenAPI definícióját, például a művelet objektumok és a válasz sablonok lehet szükség.
    1. A minta műveletet az alábbi objektumnak egy kitöltött szakasz előállított/használ, olyan parameter objektumot és a válasz objektum.
    
    ```yaml
      post:
        operationId: /api/yourfunctionroute/post
        consumes: [application/json]
        produces: [application/json]
        parameters:
          - name: name
            in: body
            description: Your name
            x-ms-summary: Your name
            required: true
            schema:
              type: object
              properties:
                name:
                  type: string
        description: >-
          Replace with Operation Object
          #http://swagger.io/specification/#operationObject
        responses:
          '200':
            description: A Greeting
            x-ms-summary: Your name
            schema:
              type: string
        security:
          - apikeyQuery: []
    ```
    
    > [!NOTE]
    >  x-ms-összegzés biztosít egy megjelenítési nevet a Logic Apps, a folyamat és a powerapps segítségével.
    >
    > Tekintse meg [testre szabhatja a Swagger-definícióval PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) további.

1. Kattintson a `save` menti a módosításokat ![Sablonleírásnak hozzáadása](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Az API-definíció használatával
1. Másolás a `API definition URL` és illessze be egy új böngészőlapon, a nyers OpenAPI dokumentum megtekintésére.
1. Importálhatja a OpenAPI dokumentum tetszőleges számú eszközt a teszteléshez és integrációs URL-címet használ.
    1. Sok Azure-erőforrások képesek automatikusan importálja a OpenAPI doc, az API Definition URL-cím segítségével a függvény Alkalmazásbeállítások mentett. A részeként a `Function` API definition forrás, URL-címet, frissítjük.


## <a name="test-definition"></a>A Swagger felhasználói felület segítségével tesztelheti az API-definíció
A beágyazott API definition szerkesztő felhasználói felület nézetbe beépített eszközök vannak tesztelése. Az API-kulcs hozzáadása, és használja a `Try this operation` az egyes módszerek gombra. Az eszköz fogja használni az API-definíció formázásához a kérelmeket, és sikeres válaszok tájékoztatja arról, hogy a definíció helyes-e.

### <a name="steps"></a>lépéseket:

1. A függvény API-kulcs másolása
    1. Az API-kulcsot a HTTP-eseményindító függvény alatt található `function name` > `Keys` > `Function Keys` 
   ![funkcióbillentyűk](./media/functions-api-definition-getting-started/functionkey.png)
1. Keresse meg a `API Definition` lap.
    1. Kattintson a `Authenticate` , és adja hozzá a függvény az API-kulcs és a biztonsági objektum felső részén.
  ![OpenAPI kulcs](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Válassza ki`/api/yourfunctionroute` > `POST`
1. Kattintson a `Try it out` és adjon meg egy nevet tesztelése
1. A sikeres eredményt kell megjelennie `Pretty` 
 ![API-definíció tesztelése](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Következő lépések
* [OpenAPI definíciójában funkciók áttekintése](functions-api-definition.md)
  * Olvassa el a teljes dokumentációt OpenAPI támogatásáról további információk.
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)  
  * Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
* [Az Azure Functions GitHub-adattára](https://github.com/Azure/Azure-Functions/)
  * Tekintse meg a visszajelzését a az API definition támogatási előzetes funkciók GitHub! A GitHub probléma létrehozása semmit, szeretne látni frissített.