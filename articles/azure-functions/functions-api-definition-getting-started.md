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
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a><span data-ttu-id="90589-103">Létrehozása OpenAPI (Swagger) 2.0 metaadatok függvény alkalmazások (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="90589-103">Creating OpenAPI 2.0 (Swagger) Metadata for a Function App (Preview)</span></span>

<span data-ttu-id="90589-104">Ez a dokumentum végigvezeti hello lépésről lépésre egy OpenAPI definíció létrehozása az Azure Functions üzemeltetett egyszerű API-t.</span><span class="sxs-lookup"><span data-stu-id="90589-104">This document guides you through hello step by step process of creating an OpenAPI Definition for a simple API hosted on Azure Functions.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a><span data-ttu-id="90589-105">Mi az a OpenAPI (Swagger)?</span><span class="sxs-lookup"><span data-stu-id="90589-105">What is OpenAPI (Swagger)?</span></span>
<span data-ttu-id="90589-106">[Swagger-metaadatok](http://swagger.io/) egy fájlt, amely meghatározza a hello funkcióit és működési módja az API-t, és lehetővé teszi, hogy a következő függvényt egy REST API toobe számos más szoftverek által használt futtató.</span><span class="sxs-lookup"><span data-stu-id="90589-106">[Swagger Metadata](http://swagger.io/) is a file that defines hello functionality and operating modes of your API, and allows a function hosting a REST API toobe consumed by a wide variety of other software.</span></span> <span data-ttu-id="90589-107">Microsoft ajánlatokat, például PowerApps és [API-alkalmazások](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), valamint tooling eszköz, például a 3. fél fejlesztői [Postman](https://www.getpostman.com/docs/importing_swagger) és [számos további csomag](http://swagger.io/tools/) összes engedélyezése a API toobe együtt egy Swagger-definíciójában.</span><span class="sxs-lookup"><span data-stu-id="90589-107">Microsoft offerings like PowerApps and [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), as well as 3rd party developer tooling like [Postman](https://www.getpostman.com/docs/importing_swagger) and [many more packages](http://swagger.io/tools/) all allow your API toobe consumed with a Swagger definition.</span></span>

## <span data-ttu-id="90589-108"><a name="prepare-function"></a>A függvény létrehozásához egy egyszerű API-hoz</span><span class="sxs-lookup"><span data-stu-id="90589-108"><a name="prepare-function"></a>Creating a Function with a simple API</span></span>
  <span data-ttu-id="90589-109">egy OpenAPI definíciója toocreate, először kell toocreate függvény és egyszerű API-t.</span><span class="sxs-lookup"><span data-stu-id="90589-109">toocreate an OpenAPI definition, we first need toocreate a Function with a simple API.</span></span> <span data-ttu-id="90589-110">Ha már rendelkezik egy, a függvény alkalmazások üzemeltetett API, kihagyhatja a következő szakasz egyenes toohello</span><span class="sxs-lookup"><span data-stu-id="90589-110">If you already have an API hosted on a Function App, you can skip straight toohello next section</span></span>
1. <span data-ttu-id="90589-111">Hozzon létre egy új funkció alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90589-111">Create a new Function App.</span></span>
    1. <span data-ttu-id="90589-112">[Azure-portálon](https://portal.azure.com)  >  `+ New` > "Függvény alkalmazás" keresése</span><span class="sxs-lookup"><span data-stu-id="90589-112">[Azure portal](https://portal.azure.com) > `+ New` > Search for "Function App"</span></span>
1. <span data-ttu-id="90589-113">Hozzon létre egy új HTTP eseményindító függvényt belül az új függvény-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="90589-113">Create a new HTTP trigger function inside your new Function App</span></span>
    1. <span data-ttu-id="90589-114">A függvény ki van töltve egy nagyon egyszerű REST API meghatározása kóddal.</span><span class="sxs-lookup"><span data-stu-id="90589-114">Your function is pre-populated with code defining a very simple REST API.</span></span>
    1. <span data-ttu-id="90589-115">Bármilyen karakterlánc toohello függvénynek átadott lekérdezési paraméterként vagy hello törzsében "Hello {bemeneti}" adja vissza a rendszer</span><span class="sxs-lookup"><span data-stu-id="90589-115">Any string passed toohello Function as a query parameter or in hello body is returned as "Hello {input}"</span></span>
1. <span data-ttu-id="90589-116">Nyissa meg toohello `Integrate` lap az új HTTP-eseményindítóval függvény</span><span class="sxs-lookup"><span data-stu-id="90589-116">Go toohello `Integrate` tab of your new HTTP Trigger function</span></span>
    1. <span data-ttu-id="90589-117">Váltás `Allowed HTTP methods` túl`Selected methods`</span><span class="sxs-lookup"><span data-stu-id="90589-117">Toggle `Allowed HTTP methods` too`Selected methods`</span></span>
    1. <span data-ttu-id="90589-118">A `Selected HTTP methods` törli a jelölőnégyzet jelölését a FELADÁS egy vagy több kivételével minden műveletet.</span><span class="sxs-lookup"><span data-stu-id="90589-118">In `Selected HTTP methods` uncheck every verb except POST.</span></span>
    <span data-ttu-id="90589-119">![Kijelölt HTTP-metódus](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span><span class="sxs-lookup"><span data-stu-id="90589-119">![Selected HTTP Methods](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span></span>
    1. <span data-ttu-id="90589-120">Ez a lépés leegyszerűsíti az API-definíció később.</span><span class="sxs-lookup"><span data-stu-id="90589-120">This step will simplify your API definition later on.</span></span>

## <span data-ttu-id="90589-121"><a name="enable"></a>API Definition támogatásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="90589-121"><a name="enable"></a>Enabling API Definition Support</span></span>
1. <span data-ttu-id="90589-122">Keresse meg a túl`your function name` > `Platform Features` > `API Definition`
![definíció lapon található.](./media/functions-api-definition-getting-started/definitiontab.png)</span><span class="sxs-lookup"><span data-stu-id="90589-122">Navigate too`your function name` > `Platform Features` > `API Definition`
![Definition Tab](./media/functions-api-definition-getting-started/definitiontab.png)</span></span>
1. <span data-ttu-id="90589-123">Állítsa be `API Definition Source` túl`Function (preview)`</span><span class="sxs-lookup"><span data-stu-id="90589-123">Set `API Definition Source` too`Function (preview)`</span></span>
    1. <span data-ttu-id="90589-124">Ez a lépés lehetővé teszi, hogy a függvény az alkalmazások, beleértve egy végpont toohost egy OpenAPI fájl, a függvény App tartományból egy beágyazott másolatot hello OpenAPI beállítások együttese [OpenAPI szerkesztő](http://editor.swagger.io), és a gyors üzembe helyezés definition generátor.</span><span class="sxs-lookup"><span data-stu-id="90589-124">This step enables a suite of OpenAPI options for your Function App, including an endpoint toohost an OpenAPI file from your Function App's domain, an inline copy of hello [OpenAPI Editor](http://editor.swagger.io), and a quickstart definition generator.</span></span>
<span data-ttu-id="90589-125">![Engedélyezett meghatározása](./media/functions-api-definition-getting-started/enabledefinition.png)</span><span class="sxs-lookup"><span data-stu-id="90589-125">![Enabled Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span></span>

## <span data-ttu-id="90589-126"><a name="create-definition"></a>Az API-definíció létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="90589-126"><a name="create-definition"></a>Creating your API Definition from a template</span></span>
1. <span data-ttu-id="90589-127">Kattintson a következőre: `Generate API Definition template`</span><span class="sxs-lookup"><span data-stu-id="90589-127">Click `Generate API Definition template`</span></span>
    1. <span data-ttu-id="90589-128">Ez a lépés vizsgálatokat végez a HTTP-eseményindítóval funkciók függvény alkalmazást, és functions.json toogenerate egy OpenAPI dokumentum hello adatait használja.</span><span class="sxs-lookup"><span data-stu-id="90589-128">This step scans your Function App for HTTP Trigger functions and uses hello info in functions.json toogenerate an OpenAPI document.</span></span>
1. <span data-ttu-id="90589-129">A művelet objektum túl hozzáadása`paths: /api/yourfunctionroute post:`</span><span class="sxs-lookup"><span data-stu-id="90589-129">Add an operation object too`paths: /api/yourfunctionroute post:`</span></span>
    1. <span data-ttu-id="90589-130">hello gyors üzembe helyezés OpenAPI dokumentum egy teljes OpenAPI doc Vázlat. További metaadatok toobe teljes OpenAPI definícióját, például a művelet objektumok és a válasz sablonok igényel.</span><span class="sxs-lookup"><span data-stu-id="90589-130">hello quickstart OpenAPI document is an outline of a full OpenAPI doc. It requires more metadata toobe a full OpenAPI definition, such as operation objects and response templates.</span></span>
    1. <span data-ttu-id="90589-131">hello minta műveletet az alábbi objektumnak egy kitöltött szakasz előállított/használ, olyan parameter objektumot és a válasz objektum.</span><span class="sxs-lookup"><span data-stu-id="90589-131">hello sample operation object below has a filled out produces/consumes section, a parameter object, and a response object.</span></span>
    
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
    >  <span data-ttu-id="90589-132">x-ms-összegzés biztosít egy megjelenítési nevet a Logic Apps, a folyamat és a powerapps segítségével.</span><span class="sxs-lookup"><span data-stu-id="90589-132">x-ms-summary provides a display name in Logic Apps, Flow, and PowerApps.</span></span>
    >
    > <span data-ttu-id="90589-133">Tekintse meg [testre szabhatja a Swagger-definícióval PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="90589-133">Check out [customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) toolearn more.</span></span>

1. <span data-ttu-id="90589-134">Kattintson a `save` toosave a módosítások ![Sablonleírásnak hozzáadása](./media/functions-api-definition-getting-started/addingtemplate.png)</span><span class="sxs-lookup"><span data-stu-id="90589-134">Click `save` toosave your changes ![Adding Template Definition](./media/functions-api-definition-getting-started/addingtemplate.png)</span></span>

## <span data-ttu-id="90589-135"><a name="use-definition"></a>Az API-definíció használatával</span><span class="sxs-lookup"><span data-stu-id="90589-135"><a name="use-definition"></a>Using Your API Definition</span></span>
1. <span data-ttu-id="90589-136">Másolás a `API definition URL` és illessze be egy új böngészőt lapon tooview a nyers OpenAPI dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="90589-136">Copy your `API definition URL` and paste it into a new browser tab tooview your raw OpenAPI document.</span></span>
1. <span data-ttu-id="90589-137">Importálhatja a OpenAPI dokumentum tooany számú eszközt a teszteléshez és integrációs URL-címet használ.</span><span class="sxs-lookup"><span data-stu-id="90589-137">You can import your OpenAPI document tooany number of tools for testing and integration using that URL.</span></span>
    1. <span data-ttu-id="90589-138">Sok Azure-erőforrások a OpenAPI doc használatával hello API Definition URL-CÍMÉT a függvény Alkalmazásbeállítások mentett képes tooautomatically importálása.</span><span class="sxs-lookup"><span data-stu-id="90589-138">Many Azure resources are able tooautomatically import your OpenAPI doc using hello API Definition URL that is saved in your Function application settings.</span></span> <span data-ttu-id="90589-139">Hello részeként `Function` API definition forrás URL-címet, frissítjük.</span><span class="sxs-lookup"><span data-stu-id="90589-139">As a part of hello `Function` API definition source, we update that url for you.</span></span>


## <span data-ttu-id="90589-140"><a name="test-definition"></a>A API-definíció hello Swagger felhasználói felület tootest használatával</span><span class="sxs-lookup"><span data-stu-id="90589-140"><a name="test-definition"></a>Using hello Swagger UI tootest your API definition</span></span>
<span data-ttu-id="90589-141">Beágyazott hello API definition szerkesztő felhasználói felület nézetében toohello beépített eszközei éppen tesztel.</span><span class="sxs-lookup"><span data-stu-id="90589-141">There are testing tools built in toohello UI view of hello imbedded API definition editor.</span></span> <span data-ttu-id="90589-142">Az API-kulcs hozzáadása, és a hello `Try this operation` az egyes módszerek gombra.</span><span class="sxs-lookup"><span data-stu-id="90589-142">Add your API key, and then use hello `Try this operation` button under each method.</span></span> <span data-ttu-id="90589-143">hello eszköze fogja használni az API-definíció tooformat hello kérelmek és sikeres válaszok tájékoztatja arról, hogy a definíció helyes-e.</span><span class="sxs-lookup"><span data-stu-id="90589-143">hello tool will use your API Definition tooformat hello requests and successful responses will indicate that your definition is correct.</span></span>

### <a name="steps"></a><span data-ttu-id="90589-144">lépéseket:</span><span class="sxs-lookup"><span data-stu-id="90589-144">Steps:</span></span>

1. <span data-ttu-id="90589-145">A függvény API-kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="90589-145">Copy your function API key</span></span>
    1. <span data-ttu-id="90589-146">a HTTP-eseményindító függvény alatt található hello API-kulcs `function name` > `Keys` > `Function Keys` 
   ![funkcióbillentyűk](./media/functions-api-definition-getting-started/functionkey.png)</span><span class="sxs-lookup"><span data-stu-id="90589-146">hello API key can be found in your HTTP Trigger Function under `function name` > `Keys` > `Function Keys`
![Function key](./media/functions-api-definition-getting-started/functionkey.png)</span></span>
1. <span data-ttu-id="90589-147">Keresse meg a toohello `API Definition` lap.</span><span class="sxs-lookup"><span data-stu-id="90589-147">Navigate toohello `API Definition` page.</span></span>
    1. <span data-ttu-id="90589-148">Kattintson a `Authenticate` , és adja hozzá a függvény API-kulcs toohello biztonsági objektum hello tetején.</span><span class="sxs-lookup"><span data-stu-id="90589-148">Click `Authenticate` and add your Function API key toohello security object at hello top.</span></span>
  <span data-ttu-id="90589-149">![OpenAPI kulcs](./media/functions-api-definition-getting-started/definitionTest auth.png)</span><span class="sxs-lookup"><span data-stu-id="90589-149">![OpenAPI key](./media/functions-api-definition-getting-started/definitionTest auth.png)</span></span>
1. <span data-ttu-id="90589-150">Válassza ki`/api/yourfunctionroute` > `POST`</span><span class="sxs-lookup"><span data-stu-id="90589-150">select `/api/yourfunctionroute` > `POST`</span></span>
1. <span data-ttu-id="90589-151">Kattintson a `Try it out` , és írja be egy nevet tootest</span><span class="sxs-lookup"><span data-stu-id="90589-151">Click `Try it out` and enter a name tootest</span></span>
1. <span data-ttu-id="90589-152">A sikeres eredményt kell megjelennie `Pretty` 
 ![API-definíció tesztelése](./media/functions-api-definition-getting-started/definitionTest.png)</span><span class="sxs-lookup"><span data-stu-id="90589-152">You should see a SUCCESS result under `Pretty`
![API Definition test](./media/functions-api-definition-getting-started/definitionTest.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="90589-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90589-153">Next steps</span></span>
* [<span data-ttu-id="90589-154">OpenAPI definíciójában funkciók áttekintése</span><span class="sxs-lookup"><span data-stu-id="90589-154">OpenAPI Definition in Functions Overview</span></span>](functions-api-definition.md)
  * <span data-ttu-id="90589-155">Olvassa el a hello teljes dokumentációt OpenAPI támogatásáról további információk.</span><span class="sxs-lookup"><span data-stu-id="90589-155">Read hello full documentation for more info on OpenAPI support.</span></span>
* [<span data-ttu-id="90589-156">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="90589-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  * <span data-ttu-id="90589-157">Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="90589-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="90589-158">Az Azure Functions GitHub-adattára</span><span class="sxs-lookup"><span data-stu-id="90589-158">Azure Functions GitHub repository</span></span>](https://github.com/Azure/Azure-Functions/)
  * <span data-ttu-id="90589-159">Tekintse meg hello funkciók GitHub toogive nekünk visszajelzést hello API definition támogatási preview!</span><span class="sxs-lookup"><span data-stu-id="90589-159">Check out hello Functions GitHub toogive us feedback on hello API definition support preview!</span></span> <span data-ttu-id="90589-160">A GitHub probléma létrehozása semmit toosee frissíteni szeretné.</span><span class="sxs-lookup"><span data-stu-id="90589-160">Make a GitHub issue for anything you'd like toosee updated.</span></span>
