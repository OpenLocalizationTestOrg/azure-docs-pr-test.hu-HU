---
title: aaaGetting Started with Azure Functions OpenAPI metaadatok |} Microsoft Docs
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
ms.openlocfilehash: fee3464c9a0a11b6d3891ccd0e9c5343d6bedcad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Létrehozása OpenAPI (Swagger) 2.0 metaadatok függvény alkalmazások (előzetes verzió)

Ez a dokumentum végigvezeti hello lépésről lépésre egy OpenAPI definíció létrehozása az Azure Functions üzemeltetett egyszerű API-t.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Mi az a OpenAPI (Swagger)?
[Swagger-metaadatok](http://swagger.io/) egy fájlt, amely meghatározza a hello funkcióit és működési módja az API-t, és lehetővé teszi, hogy a következő függvényt egy REST API toobe számos más szoftverek által használt futtató. Microsoft ajánlatokat, például PowerApps és [API-alkalmazások](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), valamint tooling eszköz, például a 3. fél fejlesztői [Postman](https://www.getpostman.com/docs/importing_swagger) és [számos további csomag](http://swagger.io/tools/) összes engedélyezése a API toobe együtt egy Swagger-definíciójában.

## <a name="prepare-function"></a>A függvény létrehozásához egy egyszerű API-hoz
  egy OpenAPI definíciója toocreate, először kell toocreate függvény és egyszerű API-t. Ha már rendelkezik egy, a függvény alkalmazások üzemeltetett API, kihagyhatja a következő szakasz egyenes toohello
1. Hozzon létre egy új funkció alkalmazást.
    1. [Azure-portálon](https://portal.azure.com)  >  `+ New` > "Függvény alkalmazás" keresése
1. Hozzon létre egy új HTTP eseményindító függvényt belül az új függvény-alkalmazás
    1. A függvény ki van töltve egy nagyon egyszerű REST API meghatározása kóddal.
    1. Bármilyen karakterlánc toohello függvénynek átadott lekérdezési paraméterként vagy hello törzsében "Hello {bemeneti}" adja vissza a rendszer
1. Nyissa meg toohello `Integrate` lap az új HTTP-eseményindítóval függvény
    1. Váltás `Allowed HTTP methods` túl`Selected methods`
    1. A `Selected HTTP methods` törli a jelölőnégyzet jelölését a FELADÁS egy vagy több kivételével minden műveletet.
    ![Kijelölt HTTP-metódus](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
    1. Ez a lépés leegyszerűsíti az API-definíció később.

## <a name="enable"></a>API Definition támogatásának engedélyezése
1. Keresse meg a túl`your function name` > `Platform Features` > `API Definition`
![definíció lapon található.](./media/functions-api-definition-getting-started/definitiontab.png)
1. Állítsa be `API Definition Source` túl`Function (preview)`
    1. Ez a lépés lehetővé teszi, hogy a függvény az alkalmazások, beleértve egy végpont toohost egy OpenAPI fájl, a függvény App tartományból egy beágyazott másolatot hello OpenAPI beállítások együttese [OpenAPI szerkesztő](http://editor.swagger.io), és a gyors üzembe helyezés definition generátor.
![Engedélyezett meghatározása](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Az API-definíció létrehozása sablonból
1. Kattintson a következőre: `Generate API Definition template`
    1. Ez a lépés vizsgálatokat végez a HTTP-eseményindítóval funkciók függvény alkalmazást, és functions.json toogenerate egy OpenAPI dokumentum hello adatait használja.
1. A művelet objektum túl hozzáadása`paths: /api/yourfunctionroute post:`
    1. hello gyors üzembe helyezés OpenAPI dokumentum egy teljes OpenAPI doc Vázlat. További metaadatok toobe teljes OpenAPI definícióját, például a művelet objektumok és a válasz sablonok igényel.
    1. hello minta műveletet az alábbi objektumnak egy kitöltött szakasz előállított/használ, olyan parameter objektumot és a válasz objektum.
    
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
    > Tekintse meg [testre szabhatja a Swagger-definícióval PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) további toolearn.

1. Kattintson a `save` toosave a módosítások ![Sablonleírásnak hozzáadása](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Az API-definíció használatával
1. Másolás a `API definition URL` és illessze be egy új böngészőt lapon tooview a nyers OpenAPI dokumentumot.
1. Importálhatja a OpenAPI dokumentum tooany számú eszközt a teszteléshez és integrációs URL-címet használ.
    1. Sok Azure-erőforrások a OpenAPI doc használatával hello API Definition URL-CÍMÉT a függvény Alkalmazásbeállítások mentett képes tooautomatically importálása. Hello részeként `Function` API definition forrás URL-címet, frissítjük.


## <a name="test-definition"></a>A API-definíció hello Swagger felhasználói felület tootest használatával
Beágyazott hello API definition szerkesztő felhasználói felület nézetében toohello beépített eszközei éppen tesztel. Az API-kulcs hozzáadása, és a hello `Try this operation` az egyes módszerek gombra. hello eszköze fogja használni az API-definíció tooformat hello kérelmek és sikeres válaszok tájékoztatja arról, hogy a definíció helyes-e.

### <a name="steps"></a>lépéseket:

1. A függvény API-kulcs másolása
    1. a HTTP-eseményindító függvény alatt található hello API-kulcs `function name` > `Keys` > `Function Keys` 
   ![funkcióbillentyűk](./media/functions-api-definition-getting-started/functionkey.png)
1. Keresse meg a toohello `API Definition` lap.
    1. Kattintson a `Authenticate` , és adja hozzá a függvény API-kulcs toohello biztonsági objektum hello tetején.
  ![OpenAPI kulcs](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Válassza ki`/api/yourfunctionroute` > `POST`
1. Kattintson a `Try it out` , és írja be egy nevet tootest
1. A sikeres eredményt kell megjelennie `Pretty` 
 ![API-definíció tesztelése](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Következő lépések
* [OpenAPI definíciójában funkciók áttekintése](functions-api-definition.md)
  * Olvassa el a hello teljes dokumentációt OpenAPI támogatásáról további információk.
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)  
  * Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
* [Az Azure Functions GitHub-adattára](https://github.com/Azure/Azure-Functions/)
  * Tekintse meg hello funkciók GitHub toogive nekünk visszajelzést hello API definition támogatási preview! A GitHub probléma létrehozása semmit toosee frissíteni szeretné.
