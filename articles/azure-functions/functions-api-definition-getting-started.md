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
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a><span data-ttu-id="5a6b5-103">Létrehozása OpenAPI (Swagger) 2.0 metaadatok függvény alkalmazások (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-103">Creating OpenAPI 2.0 (Swagger) Metadata for a Function App (Preview)</span></span>

<span data-ttu-id="5a6b5-104">Ez a dokumentum végigvezeti az Azure Functions üzemeltetett egyszerű API-t egy OpenAPI definíciója létrehozásának részletes folyamatán.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-104">This document guides you through the step by step process of creating an OpenAPI Definition for a simple API hosted on Azure Functions.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a><span data-ttu-id="5a6b5-105">Mi az a OpenAPI (Swagger)?</span><span class="sxs-lookup"><span data-stu-id="5a6b5-105">What is OpenAPI (Swagger)?</span></span>
<span data-ttu-id="5a6b5-106">[Swagger-metaadatok](http://swagger.io/) egy olyan fájl, funkcióit és az API-működési módja határozza meg, és lehetővé teszi, hogy számos más szoftverek számára a REST API-t üzemeltető függvényt.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-106">[Swagger Metadata](http://swagger.io/) is a file that defines the functionality and operating modes of your API, and allows a function hosting a REST API to be consumed by a wide variety of other software.</span></span> <span data-ttu-id="5a6b5-107">Microsoft ajánlatokat, például PowerApps és [API-alkalmazások](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), valamint tooling eszköz, például a 3. fél fejlesztői [Postman](https://www.getpostman.com/docs/importing_swagger) és [számos további csomag](http://swagger.io/tools/) összes engedélyezése az API-t, a felhasznált egy Swagger-definíciójában.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-107">Microsoft offerings like PowerApps and [API Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier), as well as 3rd party developer tooling like [Postman](https://www.getpostman.com/docs/importing_swagger) and [many more packages](http://swagger.io/tools/) all allow your API to be consumed with a Swagger definition.</span></span>

## <span data-ttu-id="5a6b5-108"><a name="prepare-function"></a>A függvény létrehozásához egy egyszerű API-hoz</span><span class="sxs-lookup"><span data-stu-id="5a6b5-108"><a name="prepare-function"></a>Creating a Function with a simple API</span></span>
  <span data-ttu-id="5a6b5-109">Hozzon létre egy OpenAPI definícióját, először kell egy egyszerű API-val hozzon létre egy függvényt.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-109">To create an OpenAPI definition, we first need to create a Function with a simple API.</span></span> <span data-ttu-id="5a6b5-110">Ha már rendelkezik egy, a függvény alkalmazások üzemeltetett API, akkor ugorjon rögtön a következő szakasz</span><span class="sxs-lookup"><span data-stu-id="5a6b5-110">If you already have an API hosted on a Function App, you can skip straight to the next section</span></span>
1. <span data-ttu-id="5a6b5-111">Hozzon létre egy új funkció alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-111">Create a new Function App.</span></span>
    1. <span data-ttu-id="5a6b5-112">[Azure-portálon](https://portal.azure.com)  >  `+ New` > "Függvény alkalmazás" keresése</span><span class="sxs-lookup"><span data-stu-id="5a6b5-112">[Azure portal](https://portal.azure.com) > `+ New` > Search for "Function App"</span></span>
1. <span data-ttu-id="5a6b5-113">Hozzon létre egy új HTTP eseményindító függvényt belül az új függvény-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5a6b5-113">Create a new HTTP trigger function inside your new Function App</span></span>
    1. <span data-ttu-id="5a6b5-114">A függvény ki van töltve egy nagyon egyszerű REST API meghatározása kóddal.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-114">Your function is pre-populated with code defining a very simple REST API.</span></span>
    1. <span data-ttu-id="5a6b5-115">Bármilyen karakterlánc egy lekérdezési paraméterben, vagy a törzsben a függvénynek átadott "Hello {bemeneti}" adja vissza a rendszer</span><span class="sxs-lookup"><span data-stu-id="5a6b5-115">Any string passed to the Function as a query parameter or in the body is returned as "Hello {input}"</span></span>
1. <span data-ttu-id="5a6b5-116">Lépjen a `Integrate` lap az új HTTP-eseményindítóval függvény</span><span class="sxs-lookup"><span data-stu-id="5a6b5-116">Go to the `Integrate` tab of your new HTTP Trigger function</span></span>
    1. <span data-ttu-id="5a6b5-117">Váltás `Allowed HTTP methods` számára`Selected methods`</span><span class="sxs-lookup"><span data-stu-id="5a6b5-117">Toggle `Allowed HTTP methods` to `Selected methods`</span></span>
    1. <span data-ttu-id="5a6b5-118">A `Selected HTTP methods` törli a jelölőnégyzet jelölését a FELADÁS egy vagy több kivételével minden műveletet.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-118">In `Selected HTTP methods` uncheck every verb except POST.</span></span>
    <span data-ttu-id="5a6b5-119">![Kijelölt HTTP-metódus](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-119">![Selected HTTP Methods](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)</span></span>
    1. <span data-ttu-id="5a6b5-120">Ez a lépés leegyszerűsíti az API-definíció később.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-120">This step will simplify your API definition later on.</span></span>

## <span data-ttu-id="5a6b5-121"><a name="enable"></a>API Definition támogatásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5a6b5-121"><a name="enable"></a>Enabling API Definition Support</span></span>
1. <span data-ttu-id="5a6b5-122">Navigáljon a `your function name`  >  `Platform Features`  >  `API Definition` 
 ![definíció lapon található.](./media/functions-api-definition-getting-started/definitiontab.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-122">Navigate to `your function name` > `Platform Features` > `API Definition`
![Definition Tab](./media/functions-api-definition-getting-started/definitiontab.png)</span></span>
1. <span data-ttu-id="5a6b5-123">Állítsa be `API Definition Source` számára`Function (preview)`</span><span class="sxs-lookup"><span data-stu-id="5a6b5-123">Set `API Definition Source` to `Function (preview)`</span></span>
    1. <span data-ttu-id="5a6b5-124">Ez a lépés lehetővé teszi, hogy az függvény alkalmazások, beleértve a függvény App tartományból egy beágyazott másolatot készít egy OpenAPI fájl futtatásához a végpont OpenAPI beállítások együttese a [OpenAPI szerkesztő](http://editor.swagger.io), és a gyors üzembe helyezés definition generátor.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-124">This step enables a suite of OpenAPI options for your Function App, including an endpoint to host an OpenAPI file from your Function App's domain, an inline copy of the [OpenAPI Editor](http://editor.swagger.io), and a quickstart definition generator.</span></span>
<span data-ttu-id="5a6b5-125">![Engedélyezett meghatározása](./media/functions-api-definition-getting-started/enabledefinition.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-125">![Enabled Definition](./media/functions-api-definition-getting-started/enabledefinition.png)</span></span>

## <span data-ttu-id="5a6b5-126"><a name="create-definition"></a>Az API-definíció létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="5a6b5-126"><a name="create-definition"></a>Creating your API Definition from a template</span></span>
1. <span data-ttu-id="5a6b5-127">Kattintson a következőre: `Generate API Definition template`</span><span class="sxs-lookup"><span data-stu-id="5a6b5-127">Click `Generate API Definition template`</span></span>
    1. <span data-ttu-id="5a6b5-128">Ebben a lépésben vizsgálatokat végez a HTTP-eseményindítóval funkciók függvény alkalmazást, és functions.json a biztonsági adatok segítségével egy OpenAPI dokumentum létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-128">This step scans your Function App for HTTP Trigger functions and uses the info in functions.json to generate an OpenAPI document.</span></span>
1. <span data-ttu-id="5a6b5-129">A művelet objektum hozzáadása`paths: /api/yourfunctionroute post:`</span><span class="sxs-lookup"><span data-stu-id="5a6b5-129">Add an operation object to `paths: /api/yourfunctionroute post:`</span></span>
    1. <span data-ttu-id="5a6b5-130">A gyors üzembe helyezés OpenAPI dokumentum egy teljes OpenAPI doc Vázlat. További metaadatokra ahhoz, hogy a teljes OpenAPI definícióját, például a művelet objektumok és a válasz sablonok lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-130">The quickstart OpenAPI document is an outline of a full OpenAPI doc. It requires more metadata to be a full OpenAPI definition, such as operation objects and response templates.</span></span>
    1. <span data-ttu-id="5a6b5-131">A minta műveletet az alábbi objektumnak egy kitöltött szakasz előállított/használ, olyan parameter objektumot és a válasz objektum.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-131">The sample operation object below has a filled out produces/consumes section, a parameter object, and a response object.</span></span>
    
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
    >  <span data-ttu-id="5a6b5-132">x-ms-összegzés biztosít egy megjelenítési nevet a Logic Apps, a folyamat és a powerapps segítségével.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-132">x-ms-summary provides a display name in Logic Apps, Flow, and PowerApps.</span></span>
    >
    > <span data-ttu-id="5a6b5-133">Tekintse meg [testre szabhatja a Swagger-definícióval PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) további.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-133">Check out [customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/) to learn more.</span></span>

1. <span data-ttu-id="5a6b5-134">Kattintson a `save` menti a módosításokat ![Sablonleírásnak hozzáadása](./media/functions-api-definition-getting-started/addingtemplate.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-134">Click `save` to save your changes ![Adding Template Definition](./media/functions-api-definition-getting-started/addingtemplate.png)</span></span>

## <span data-ttu-id="5a6b5-135"><a name="use-definition"></a>Az API-definíció használatával</span><span class="sxs-lookup"><span data-stu-id="5a6b5-135"><a name="use-definition"></a>Using Your API Definition</span></span>
1. <span data-ttu-id="5a6b5-136">Másolás a `API definition URL` és illessze be egy új böngészőlapon, a nyers OpenAPI dokumentum megtekintésére.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-136">Copy your `API definition URL` and paste it into a new browser tab to view your raw OpenAPI document.</span></span>
1. <span data-ttu-id="5a6b5-137">Importálhatja a OpenAPI dokumentum tetszőleges számú eszközt a teszteléshez és integrációs URL-címet használ.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-137">You can import your OpenAPI document to any number of tools for testing and integration using that URL.</span></span>
    1. <span data-ttu-id="5a6b5-138">Sok Azure-erőforrások képesek automatikusan importálja a OpenAPI doc, az API Definition URL-cím segítségével a függvény Alkalmazásbeállítások mentett.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-138">Many Azure resources are able to automatically import your OpenAPI doc using the API Definition URL that is saved in your Function application settings.</span></span> <span data-ttu-id="5a6b5-139">A részeként a `Function` API definition forrás, URL-címet, frissítjük.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-139">As a part of the `Function` API definition source, we update that url for you.</span></span>


## <span data-ttu-id="5a6b5-140"><a name="test-definition"></a>A Swagger felhasználói felület segítségével tesztelheti az API-definíció</span><span class="sxs-lookup"><span data-stu-id="5a6b5-140"><a name="test-definition"></a>Using the Swagger UI to test your API definition</span></span>
<span data-ttu-id="5a6b5-141">A beágyazott API definition szerkesztő felhasználói felület nézetbe beépített eszközök vannak tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-141">There are testing tools built in to the UI view of the imbedded API definition editor.</span></span> <span data-ttu-id="5a6b5-142">Az API-kulcs hozzáadása, és használja a `Try this operation` az egyes módszerek gombra.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-142">Add your API key, and then use the `Try this operation` button under each method.</span></span> <span data-ttu-id="5a6b5-143">Az eszköz fogja használni az API-definíció formázásához a kérelmeket, és sikeres válaszok tájékoztatja arról, hogy a definíció helyes-e.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-143">The tool will use your API Definition to format the requests and successful responses will indicate that your definition is correct.</span></span>

### <a name="steps"></a><span data-ttu-id="5a6b5-144">lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5a6b5-144">Steps:</span></span>

1. <span data-ttu-id="5a6b5-145">A függvény API-kulcs másolása</span><span class="sxs-lookup"><span data-stu-id="5a6b5-145">Copy your function API key</span></span>
    1. <span data-ttu-id="5a6b5-146">Az API-kulcsot a HTTP-eseményindító függvény alatt található `function name` > `Keys` > `Function Keys` 
   ![funkcióbillentyűk](./media/functions-api-definition-getting-started/functionkey.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-146">The API key can be found in your HTTP Trigger Function under `function name` > `Keys` > `Function Keys`
![Function key](./media/functions-api-definition-getting-started/functionkey.png)</span></span>
1. <span data-ttu-id="5a6b5-147">Keresse meg a `API Definition` lap.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-147">Navigate to the `API Definition` page.</span></span>
    1. <span data-ttu-id="5a6b5-148">Kattintson a `Authenticate` , és adja hozzá a függvény az API-kulcs és a biztonsági objektum felső részén.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-148">Click `Authenticate` and add your Function API key to the security object at the top.</span></span>
  <span data-ttu-id="5a6b5-149">![OpenAPI kulcs](./media/functions-api-definition-getting-started/definitionTest auth.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-149">![OpenAPI key](./media/functions-api-definition-getting-started/definitionTest auth.png)</span></span>
1. <span data-ttu-id="5a6b5-150">Válassza ki`/api/yourfunctionroute` > `POST`</span><span class="sxs-lookup"><span data-stu-id="5a6b5-150">select `/api/yourfunctionroute` > `POST`</span></span>
1. <span data-ttu-id="5a6b5-151">Kattintson a `Try it out` és adjon meg egy nevet tesztelése</span><span class="sxs-lookup"><span data-stu-id="5a6b5-151">Click `Try it out` and enter a name to test</span></span>
1. <span data-ttu-id="5a6b5-152">A sikeres eredményt kell megjelennie `Pretty` 
 ![API-definíció tesztelése](./media/functions-api-definition-getting-started/definitionTest.png)</span><span class="sxs-lookup"><span data-stu-id="5a6b5-152">You should see a SUCCESS result under `Pretty`
![API Definition test](./media/functions-api-definition-getting-started/definitionTest.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a6b5-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a6b5-153">Next steps</span></span>
* [<span data-ttu-id="5a6b5-154">OpenAPI definíciójában funkciók áttekintése</span><span class="sxs-lookup"><span data-stu-id="5a6b5-154">OpenAPI Definition in Functions Overview</span></span>](functions-api-definition.md)
  * <span data-ttu-id="5a6b5-155">Olvassa el a teljes dokumentációt OpenAPI támogatásáról további információk.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-155">Read the full documentation for more info on OpenAPI support.</span></span>
* [<span data-ttu-id="5a6b5-156">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="5a6b5-156">Azure Functions developer reference</span></span>](functions-reference.md)  
  * <span data-ttu-id="5a6b5-157">Programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-157">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="5a6b5-158">Az Azure Functions GitHub-adattára</span><span class="sxs-lookup"><span data-stu-id="5a6b5-158">Azure Functions GitHub repository</span></span>](https://github.com/Azure/Azure-Functions/)
  * <span data-ttu-id="5a6b5-159">Tekintse meg a visszajelzését a az API definition támogatási előzetes funkciók GitHub!</span><span class="sxs-lookup"><span data-stu-id="5a6b5-159">Check out the Functions GitHub to give us feedback on the API definition support preview!</span></span> <span data-ttu-id="5a6b5-160">A GitHub probléma létrehozása semmit, szeretne látni frissített.</span><span class="sxs-lookup"><span data-stu-id="5a6b5-160">Make a GitHub issue for anything you'd like to see updated.</span></span>
