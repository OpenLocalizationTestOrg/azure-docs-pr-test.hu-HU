---
title: "aaaCall, indítsa el, vagy a HTTP-végpontok – Azure Logic Apps munkafolyamatok beágyazásához |} Microsoft Docs"
description: "Az Azure Logic Apps HTTP végpontok toocall, eseményindító vagy nest munkafolyamatok beállítása"
services: logic-apps
keywords: "munkafolyamatok, HTTP-végpontok"
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 072a314c3bff75ab7696f86bb063bb7c03c4ae89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a><span data-ttu-id="bfb97-104">Hívja, eseményindító, vagy a logic Apps alkalmazások a HTTP-végpontokról munkafolyamatok egymásba</span><span class="sxs-lookup"><span data-stu-id="bfb97-104">Call, trigger, or nest workflows with HTTP endpoints in logic apps</span></span>

<span data-ttu-id="bfb97-105">Is natív módon teszi ki szinkron HTTP-végpontokról a logic apps eseményindítóként használható, hogy indul el, vagy hívja a logic apps segítségével egy URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bfb97-105">You can natively expose synchronous HTTP endpoints as triggers on logic apps so that you can trigger or call your logic apps through a URL.</span></span> <span data-ttu-id="bfb97-106">A logic Apps alkalmazásait is ágyazhatók be munkafolyamatok hívható végpontok egy mintát.</span><span class="sxs-lookup"><span data-stu-id="bfb97-106">You can also nest workflows in your logic apps by using a pattern of callable endpoints.</span></span>

<span data-ttu-id="bfb97-107">HTTP-végpontokról toocreate, adhat hozzá ezek az eseményindítók, hogy a logic apps bejövő kérelmek fogadására:</span><span class="sxs-lookup"><span data-stu-id="bfb97-107">toocreate HTTP endpoints, you can add these triggers so that your logic apps can receive incoming requests:</span></span>

* [<span data-ttu-id="bfb97-108">Kérés</span><span class="sxs-lookup"><span data-stu-id="bfb97-108">Request</span></span>](../connectors/connectors-native-reqres.md)

* [<span data-ttu-id="bfb97-109">API-kapcsolat Webhook</span><span class="sxs-lookup"><span data-stu-id="bfb97-109">API Connection Webhook</span></span>](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [<span data-ttu-id="bfb97-110">HTTP Webhook</span><span class="sxs-lookup"><span data-stu-id="bfb97-110">HTTP Webhook</span></span>](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > <span data-ttu-id="bfb97-111">Bár a jelen példában használják a hello **kérelem** eseményindító, akkor használhatja a hello felsorolt HTTP-eseményindítók, és minden alapelvek azonos toohello más alkalmazhatunk eseményindító.</span><span class="sxs-lookup"><span data-stu-id="bfb97-111">Although our examples use hello **Request** trigger, you can use any of hello listed HTTP triggers, and all principles identically apply toohello other trigger types.</span></span>

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a><span data-ttu-id="bfb97-112">A Logic Apps alkalmazást HTTP végpont beállítása</span><span class="sxs-lookup"><span data-stu-id="bfb97-112">Set up an HTTP endpoint for your logic app</span></span>

<span data-ttu-id="bfb97-113">toocreate HTTP-végponttal, adjon hozzá egy eseményindító, amely képes fogadni a beérkező kéréseket.</span><span class="sxs-lookup"><span data-stu-id="bfb97-113">toocreate an HTTP endpoint, add a trigger that can receive incoming requests.</span></span>

1. <span data-ttu-id="bfb97-114">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="bfb97-114">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="bfb97-115">Nyissa meg tooyour Logic Apps alkalmazást, és nyissa meg a Logic App Tervező.</span><span class="sxs-lookup"><span data-stu-id="bfb97-115">Go tooyour logic app, and open Logic App Designer.</span></span>

2. <span data-ttu-id="bfb97-116">Adjon hozzá egy eseményindító, amely lehetővé teszi, hogy a bejövő kérések fogadásához Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfb97-116">Add a trigger that lets your logic app receive incoming requests.</span></span> <span data-ttu-id="bfb97-117">Adja hozzá például hello **kérelem** eseményindító tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfb97-117">For example, add hello **Request** trigger tooyour logic app.</span></span>

3.  <span data-ttu-id="bfb97-118">A **kérelem törzse JSON-séma**, opcionálisan adhatja meg a JSON-séma hello tartalom (adatok) hello eseményindító tooreceive várható.</span><span class="sxs-lookup"><span data-stu-id="bfb97-118">Under **Request Body JSON Schema**, you can optionally enter a JSON schema for hello payload (data) that you expect hello trigger tooreceive.</span></span>

    <span data-ttu-id="bfb97-119">hello designer a sémát, hogy a logikai alkalmazás használhatja tooconsume, elemzési és pass adatait a munkafolyamaton keresztül hello eseményindító jogkivonatokat előállító használ.</span><span class="sxs-lookup"><span data-stu-id="bfb97-119">hello designer uses this schema for generating tokens that your logic app can use tooconsume, parse, and pass data from hello trigger through your workflow.</span></span> 
    <span data-ttu-id="bfb97-120">További részletek [JSON-sémák alapján generált jogkivonatok](#generated-tokens).</span><span class="sxs-lookup"><span data-stu-id="bfb97-120">More about [tokens generated from JSON schemas](#generated-tokens).</span></span>

    <span data-ttu-id="bfb97-121">Ehhez a példához írja be a hello séma hello designer látható:</span><span class="sxs-lookup"><span data-stu-id="bfb97-121">For this example, enter hello schema shown in hello designer:</span></span>

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Hello kérelem művelet hozzáadása][1]

    > [!TIP]
    > 
    > <span data-ttu-id="bfb97-123">A sémát a JSON hasznos generálása egy eszköz, például [jsonschema.net](http://jsonschema.net/), vagy a hello **kérelem** kiválasztásával eseményindító **használata minta hasznos toogenerate séma**.</span><span class="sxs-lookup"><span data-stu-id="bfb97-123">You can generate a schema for a sample JSON payload from a tool like [jsonschema.net](http://jsonschema.net/), or in hello **Request** trigger by choosing **Use sample payload toogenerate schema**.</span></span> 
    > <span data-ttu-id="bfb97-124">Adjon meg a minta-adattartalmat, és válassza a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="bfb97-124">Enter your sample payload, and choose **Done**.</span></span>

    <span data-ttu-id="bfb97-125">Ha például a minta hasznos:</span><span class="sxs-lookup"><span data-stu-id="bfb97-125">For example, this sample payload:</span></span>

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    <span data-ttu-id="bfb97-126">hoz létre ebben a sémában:</span><span class="sxs-lookup"><span data-stu-id="bfb97-126">generates this schema:</span></span>

    ```json
    }
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  <span data-ttu-id="bfb97-127">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfb97-127">Save your logic app.</span></span> <span data-ttu-id="bfb97-128">A **HTTP POST toothis URL-cím**, most látnia kell egy generált visszahívási URL-t, például ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="bfb97-128">Under **HTTP POST toothis URL**, you should now find a generated callback URL, like this example:</span></span>

    ![Generált visszahívási végpont URL-címe](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    <span data-ttu-id="bfb97-130">Az URL-cím a hitelesítéshez használt hello lekérdezési paraméterek közös hozzáférésű Jogosultságkód (SAS) kulcsot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bfb97-130">This URL contains a Shared Access Signature (SAS) key in hello query parameters that are used for authentication.</span></span> 
    <span data-ttu-id="bfb97-131">Hello HTTP végponti URL-cím is lekérheti a logic app – áttekintés hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bfb97-131">You can also get hello HTTP endpoint URL from your logic app overview in hello Azure portal.</span></span> <span data-ttu-id="bfb97-132">A **eseményindító előzmények**, válassza ki az eseményindító:</span><span class="sxs-lookup"><span data-stu-id="bfb97-132">Under **Trigger History**, select your trigger:</span></span>

    ![HTTP-végpont URL-cím beszerzése az Azure portálról][2]

    <span data-ttu-id="bfb97-134">Vagy adott hívás végrehajtása kaphat hello URL-címe:</span><span class="sxs-lookup"><span data-stu-id="bfb97-134">Or you can get hello URL by making this call:</span></span>

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-hello-http-method-for-your-trigger"></a><span data-ttu-id="bfb97-135">Az eseményindító hello HTTP-metódus módosítása</span><span class="sxs-lookup"><span data-stu-id="bfb97-135">Change hello HTTP method for your trigger</span></span>

<span data-ttu-id="bfb97-136">Alapértelmezés szerint hello **kérelem** eseményindító vár HTTP POST-kérelmet, de különböző HTTP-metódus használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="bfb97-136">By default, hello **Request** trigger expects an HTTP POST request, but you can use a different HTTP method.</span></span> 

> [!NOTE]
> <span data-ttu-id="bfb97-137">Megadhatja, hogy csak egy metódus típusa.</span><span class="sxs-lookup"><span data-stu-id="bfb97-137">You can specify only one method type.</span></span>

1. <span data-ttu-id="bfb97-138">Az a **kérelem** indítható el, válassza a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="bfb97-138">On your **Request** trigger, choose **Show advanced options**.</span></span>

2. <span data-ttu-id="bfb97-139">Nyissa meg hello **metódus** listája.</span><span class="sxs-lookup"><span data-stu-id="bfb97-139">Open hello **Method** list.</span></span> <span data-ttu-id="bfb97-140">Ehhez a példához válassza ki a **beolvasása** , hogy később tesztelheti a HTTP-végpont URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bfb97-140">For this example, select **GET** so that you can test your HTTP endpoint's URL later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bfb97-141">Jelölje be más HTTP-metódus, vagy adjon meg egyéni módszer a saját logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfb97-141">You can select any other HTTP method, or specify a custom method for your own logic app.</span></span>

    ![HTTP-metódus módosítása](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a><span data-ttu-id="bfb97-143">Fogadja el a paraméterek használatával a HTTP-végpont URL-címe</span><span class="sxs-lookup"><span data-stu-id="bfb97-143">Accept parameters through your HTTP endpoint URL</span></span>

<span data-ttu-id="bfb97-144">Ha azt szeretné, hogy a HTTP végpont URL-cím tooaccept paramétereket, testre szabhatja a trigger elem relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="bfb97-144">When you want your HTTP endpoint URL tooaccept parameters, customize your trigger's relative path.</span></span>

1. <span data-ttu-id="bfb97-145">Az a **kérelem** indítható el, válassza a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="bfb97-145">On your **Request** trigger, choose **Show advanced options**.</span></span> 

2. <span data-ttu-id="bfb97-146">A **metódus**, adja meg, amelyet a kérés toouse hello HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="bfb97-146">Under **Method**, specify hello HTTP method that you want your request toouse.</span></span> <span data-ttu-id="bfb97-147">Ehhez a példához válassza ki a hello **beolvasása** metódust, ha még nem tette meg, így ellenőrizheti, hogy a HTTP-végpont URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bfb97-147">For this example, select hello **GET** method, if you haven't already, so that you can test your HTTP endpoint's URL.</span></span>

      > [!NOTE]
      > <span data-ttu-id="bfb97-148">Relatív elérési útvonal az eseményindító megadása esetén az eseményindító közvetlenül is adjon meg egy HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="bfb97-148">When you specify a relative path for your trigger, you must also explicitly specify an HTTP method for your trigger.</span></span>

3. <span data-ttu-id="bfb97-149">A **relatív elérési út**, adja meg a hello relatív elérési út hello paraméter, amely az URL-CÍMÉRE kell fogadnia, például a `customers/{customerID}`.</span><span class="sxs-lookup"><span data-stu-id="bfb97-149">Under **Relative path**, specify hello relative path for hello parameter that your URL should accept, for example, `customers/{customerID}`.</span></span>

    ![Adja meg a hello HTTP-metódus és a relatív elérési út paraméter](./media/logic-apps-http-endpoint/relativeurl.png)

4. <span data-ttu-id="bfb97-151">toouse hello paraméter, adjon hozzá egy **válasz** művelet tooyour logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfb97-151">toouse hello parameter, add a **Response** action tooyour logic app.</span></span> <span data-ttu-id="bfb97-152">(Válassza ki az eseményindító **új lépés** > **művelet hozzáadása** > **válasz**)</span><span class="sxs-lookup"><span data-stu-id="bfb97-152">(Under your trigger, choose **New step** > **Add an action** > **Response**)</span></span> 

5. <span data-ttu-id="bfb97-153">A válaszban szereplő **törzs**, tartalmaz hello tokent hello paraméter, a trigger elem relatív elérési utat adott meg.</span><span class="sxs-lookup"><span data-stu-id="bfb97-153">In your response's **Body**, include hello token for hello parameter that you specified in your trigger's relative path.</span></span>

    <span data-ttu-id="bfb97-154">Például tooreturn `Hello {customerID}`, frissítse a válasz **törzs** rendelkező `Hello {customerID token}`.</span><span class="sxs-lookup"><span data-stu-id="bfb97-154">For example, tooreturn `Hello {customerID}`, update your response's **Body** with `Hello {customerID token}`.</span></span> 
    <span data-ttu-id="bfb97-155">dinamikus tartalom listába hello kell jelenik meg, és hello megjelenítése `customerID` meg tooselect jogkivonatból.</span><span class="sxs-lookup"><span data-stu-id="bfb97-155">hello dynamic content list should appear and show hello `customerID` token for you tooselect.</span></span>

    ![Adja hozzá a paraméter tooresponse törzse](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    <span data-ttu-id="bfb97-157">A **törzs** ebben a példában hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bfb97-157">Your **Body** should look like this example:</span></span>

    ![Választörzs paraméterrel](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. <span data-ttu-id="bfb97-159">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bfb97-159">Save your logic app.</span></span> 

    <span data-ttu-id="bfb97-160">A HTTP-végpont URL-CÍMÉT most már tartalmaz hello relatív elérési út, például:</span><span class="sxs-lookup"><span data-stu-id="bfb97-160">Your HTTP endpoint URL now includes hello relative path, for example:</span></span> 

    <span data-ttu-id="bfb97-161">HTTPS & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span><span class="sxs-lookup"><span data-stu-id="bfb97-161">https&#58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...</span></span>

7. <span data-ttu-id="bfb97-162">a HTTP-végpont, másolás és beillesztés hello frissített URL-cím egy másik böngészőben ablakba, de cserélje le tootest `{customerID}` a `123456`, nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="bfb97-162">tootest your HTTP endpoint, copy and paste hello updated URL into another browser window, but replace `{customerID}` with `123456`, and press Enter.</span></span>

    <span data-ttu-id="bfb97-163">A böngésző ezt a szöveget kell megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="bfb97-163">Your browser should show this text:</span></span> 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a><span data-ttu-id="bfb97-164">A logikai alkalmazásnak JSON-sémák alapján generált jogkivonatok</span><span class="sxs-lookup"><span data-stu-id="bfb97-164">Tokens generated from JSON schemas for your logic app</span></span>

<span data-ttu-id="bfb97-165">Ha megadta a JSON-séma a a **kérelem** indítás, hello Logic App Designer jogkivonatokat hoz létre a tulajdonságokat, hogy a séma.</span><span class="sxs-lookup"><span data-stu-id="bfb97-165">When you provide a JSON schema in your **Request** trigger, hello Logic App Designer generates tokens for properties in that schema.</span></span> <span data-ttu-id="bfb97-166">A jogkivonatok az adatokat a logic app munkafolyamaton keresztül használhatja.</span><span class="sxs-lookup"><span data-stu-id="bfb97-166">You can then use those tokens for passing data through your logic app workflow.</span></span>

<span data-ttu-id="bfb97-167">Ehhez a példához hello hozzáadásakor `title` és `name` tulajdonságok tooyour JSON-séma, a jogkivonatok most elérhető toouse munkafolyamat a későbbi lépésekben.</span><span class="sxs-lookup"><span data-stu-id="bfb97-167">For this example, if you add hello `title` and `name` properties tooyour JSON schema, their tokens are now available toouse in later workflow steps.</span></span> 

<span data-ttu-id="bfb97-168">Hello teljes JSON-séma a következő:</span><span class="sxs-lookup"><span data-stu-id="bfb97-168">Here is hello complete JSON schema:</span></span>

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a><span data-ttu-id="bfb97-169">Beágyazott munkafolyamatok a logic Apps alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="bfb97-169">Create nested workflows for logic apps</span></span>

<span data-ttu-id="bfb97-170">A Logic Apps alkalmazást a munkafolyamatok ágyazhatja más logikai alkalmazások fogadhassanak kérelmek hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="bfb97-170">You can nest workflows in your logic app by adding other logic apps that can receive requests.</span></span> <span data-ttu-id="bfb97-171">tooinclude a logic apps hozzáadása hello **Azure Logic Apps - válassza ki a Logic Apps munkafolyamat** művelet tooyour eseményindító.</span><span class="sxs-lookup"><span data-stu-id="bfb97-171">tooinclude these logic apps, add hello **Azure Logic Apps - Choose a Logic Apps workflow** action tooyour trigger.</span></span> <span data-ttu-id="bfb97-172">Ezután kiválaszthatja a megfelelő logic Apps alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="bfb97-172">You can then select from eligible logic apps.</span></span>

![Egy másik logikai alkalmazás hozzáadása](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a><span data-ttu-id="bfb97-174">Hívás vagy eseményindítót, a logic apps HTTP-végpontokról keresztül</span><span class="sxs-lookup"><span data-stu-id="bfb97-174">Call or trigger logic apps through HTTP endpoints</span></span>

<span data-ttu-id="bfb97-175">A HTTP-végpont létrehozása után a logikai alkalmazás keresztül is elindíthatja egy `POST` metódus toohello teljes URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bfb97-175">After you create your HTTP endpoint, you can trigger your logic app through a `POST` method toohello full URL.</span></span> <span data-ttu-id="bfb97-176">A Logic apps van a közvetlen hozzáférés végpontok beépített támogatása.</span><span class="sxs-lookup"><span data-stu-id="bfb97-176">Logic apps have built-in support for direct-access endpoints.</span></span>

## <a name="reference-content-from-an-incoming-request"></a><span data-ttu-id="bfb97-177">Olyan bejövő kérelemre a referencia-tartalom</span><span class="sxs-lookup"><span data-stu-id="bfb97-177">Reference content from an incoming request</span></span>

<span data-ttu-id="bfb97-178">Ha a tartalom hello főelem típusából `application/json`, tulajdonságok hello bejövő kérelem hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="bfb97-178">If hello content's type is `application/json`, you can reference properties from hello incoming request.</span></span> <span data-ttu-id="bfb97-179">Ellenkező esetben tartalmat a rendszer egyetlen bináris egységbe, hogy átadhatók tooother API-k.</span><span class="sxs-lookup"><span data-stu-id="bfb97-179">Otherwise, content is treated as a single binary unit that you can pass tooother APIs.</span></span> <span data-ttu-id="bfb97-180">Ez a tartalom belül hello munkafolyamat tooreference kell konvertálnia ezt a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="bfb97-180">tooreference this content inside hello workflow, you must convert that content.</span></span> <span data-ttu-id="bfb97-181">Ha át például `application/xml` tartalom, használhat `@xpath()` egy XPath kiolvasásához vagy `@json()` XML tooJSON alakításának.</span><span class="sxs-lookup"><span data-stu-id="bfb97-181">For example, if you pass `application/xml` content, you can use `@xpath()` for an XPath extraction, or `@json()` for converting XML tooJSON.</span></span> <span data-ttu-id="bfb97-182">További tudnivalók [tartalomtípusok használata](../logic-apps/logic-apps-content-type.md).</span><span class="sxs-lookup"><span data-stu-id="bfb97-182">Learn about [working with content types](../logic-apps/logic-apps-content-type.md).</span></span>

<span data-ttu-id="bfb97-183">tooget hello kimenetét olyan bejövő kérelemre, használhatja a hello `@triggerOutputs()` függvény.</span><span class="sxs-lookup"><span data-stu-id="bfb97-183">tooget hello output from an incoming request, you can use hello `@triggerOutputs()` function.</span></span> <span data-ttu-id="bfb97-184">hello kimeneti ebben a példában látható:</span><span class="sxs-lookup"><span data-stu-id="bfb97-184">hello output might look like this example:</span></span>

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

<span data-ttu-id="bfb97-185">tooaccess hello `body` tulajdonság pontosabban használható hello `@triggerBody()` helyi.</span><span class="sxs-lookup"><span data-stu-id="bfb97-185">tooaccess hello `body` property specifically, you can use hello `@triggerBody()` shortcut.</span></span> 

## <a name="respond-toorequests"></a><span data-ttu-id="bfb97-186">Válaszoljon toorequests</span><span class="sxs-lookup"><span data-stu-id="bfb97-186">Respond toorequests</span></span>

<span data-ttu-id="bfb97-187">Érdemes lehet toorespond toocertain kérelmek indítsa el a logikai alkalmazás tartalom toohello hívó vissza.</span><span class="sxs-lookup"><span data-stu-id="bfb97-187">You might want toorespond toocertain requests that start a logic app by returning content toohello caller.</span></span> <span data-ttu-id="bfb97-188">tooconstruct hello állapotkód, fejléc és a válasz törzsében, használhatja a hello **válasz** művelet.</span><span class="sxs-lookup"><span data-stu-id="bfb97-188">tooconstruct hello status code, header, and body for your response, you can use hello **Response** action.</span></span> <span data-ttu-id="bfb97-189">Ez a művelet bárhol megjelenhet a Logic Apps alkalmazást, nem csak hello végén a munkafolyamatba.</span><span class="sxs-lookup"><span data-stu-id="bfb97-189">This action can appear anywhere in your logic app, not just at hello end of your workflow.</span></span>

> [!NOTE] 
> <span data-ttu-id="bfb97-190">Ha nem adja meg a Logic Apps alkalmazást egy **válasz**, hello HTTP-végpont válaszol *azonnal* rendelkező egy **202 elfogadott** állapotát.</span><span class="sxs-lookup"><span data-stu-id="bfb97-190">If your logic app doesn't include a **Response**, hello HTTP endpoint responds *immediately* with a **202 Accepted** status.</span></span> <span data-ttu-id="bfb97-191">Emellett hello eredeti kérelem tooget hello válasz, hello válasz szükséges lépéseket kell belül lefutnak hello [kérelem időkorlátja](./logic-apps-limits-and-config.md) kivéve, ha meghívja a hello munkafolyamat beágyazott logikai alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="bfb97-191">Also, for hello original request tooget hello response, all steps required for hello response must finish within hello [request timeout limit](./logic-apps-limits-and-config.md) unless you call hello workflow as a nested logic app.</span></span> <span data-ttu-id="bfb97-192">Nincs válasz a határidőn belül történik, ha a bejövő kérelem hello érvénytelenné válik, és hello HTTP-válasz fogadása **408 az ügyfél időtúllépése**.</span><span class="sxs-lookup"><span data-stu-id="bfb97-192">If no response happens within this limit, hello incoming request times out and receives hello HTTP response **408 Client timeout**.</span></span> <span data-ttu-id="bfb97-193">Beágyazott logic apps hello szülő logikai alkalmazás továbbra is toowait befejezéséig, függetlenül attól, hogy mennyi idő szükséges választ.</span><span class="sxs-lookup"><span data-stu-id="bfb97-193">For nested logic apps, hello parent logic app continues toowait for a response until completed, regardless of how much time is required.</span></span>

### <a name="construct-hello-response"></a><span data-ttu-id="bfb97-194">Szerkezet hello válasz</span><span class="sxs-lookup"><span data-stu-id="bfb97-194">Construct hello response</span></span>

<span data-ttu-id="bfb97-195">Megadhat több fejléc és a tartalom bármilyen típusú hello adott válasz törzse.</span><span class="sxs-lookup"><span data-stu-id="bfb97-195">You can include more than one header and any type of content in hello response body.</span></span> <span data-ttu-id="bfb97-196">A példa egy válasz, a hello fejléc határozza meg, hogy rendelkezik-e hello válasz tartalomtípusa `application/json`.</span><span class="sxs-lookup"><span data-stu-id="bfb97-196">In our example response, hello header specifies that hello response has content type `application/json`.</span></span> <span data-ttu-id="bfb97-197">hello törzsében és `title` és `name`frissítve korábban hello hello JSON-séma alapján **kérelem** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="bfb97-197">and hello body contains `title` and `name`, based on hello JSON schema updated previously for hello **Request** trigger.</span></span>

![HTTP-válasz művelet][3]

<span data-ttu-id="bfb97-199">Válaszok ezekkel a tulajdonságokkal rendelkeznek:</span><span class="sxs-lookup"><span data-stu-id="bfb97-199">Responses have these properties:</span></span>

| <span data-ttu-id="bfb97-200">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bfb97-200">Property</span></span> | <span data-ttu-id="bfb97-201">Leírás</span><span class="sxs-lookup"><span data-stu-id="bfb97-201">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bfb97-202">statusCode</span><span class="sxs-lookup"><span data-stu-id="bfb97-202">statusCode</span></span> |<span data-ttu-id="bfb97-203">Megadja a hello HTTP-állapotkód válaszol toohello bejövő kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="bfb97-203">Specifies hello HTTP status code for responding toohello incoming request.</span></span> <span data-ttu-id="bfb97-204">Ez a kód bármilyen érvényes állapotkód 2xx, 4xx vagy 5xx kezdetű lehet.</span><span class="sxs-lookup"><span data-stu-id="bfb97-204">This code can be any valid status code that starts with 2xx, 4xx, or 5xx.</span></span> <span data-ttu-id="bfb97-205">3xx állapotkódok azonban nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="bfb97-205">However, 3xx status codes are not permitted.</span></span> |
| <span data-ttu-id="bfb97-206">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="bfb97-206">headers</span></span> |<span data-ttu-id="bfb97-207">Definiálja a fejlécek tooinclude tetszőleges számú hello válasz.</span><span class="sxs-lookup"><span data-stu-id="bfb97-207">Defines any number of headers tooinclude in hello response.</span></span> |
| <span data-ttu-id="bfb97-208">Törzs</span><span class="sxs-lookup"><span data-stu-id="bfb97-208">body</span></span> |<span data-ttu-id="bfb97-209">Adja meg a szervezet objektum, amely egy karakterlánc lehet, egy JSON-objektumból, vagy az előző lépésben hivatkozott még bináris tartalom.</span><span class="sxs-lookup"><span data-stu-id="bfb97-209">Specifies a body object that can be a string, a JSON object, or even binary content referenced from a previous step.</span></span> |

<span data-ttu-id="bfb97-210">Íme, milyen láthatóhoz hasonló most a hello hello JSON-séma **válasz** művelet:</span><span class="sxs-lookup"><span data-stu-id="bfb97-210">Here's what hello JSON schema looks like now for hello **Response** action:</span></span>

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> <span data-ttu-id="bfb97-211">Válassza a tooview hello teljes JSON-definíció a Logic Apps alkalmazást, a Logic App Designer hello **nézet Code**.</span><span class="sxs-lookup"><span data-stu-id="bfb97-211">tooview hello complete JSON definition for your logic app, on hello Logic App Designer, choose **Code view**.</span></span>

## <a name="q--a"></a><span data-ttu-id="bfb97-212">Kérdések és válaszok</span><span class="sxs-lookup"><span data-stu-id="bfb97-212">Q & A</span></span>

#### <a name="q-what-about-url-security"></a><span data-ttu-id="bfb97-213">K: Mi a helyzet URL-cím biztonsági?</span><span class="sxs-lookup"><span data-stu-id="bfb97-213">Q: What about URL security?</span></span>

<span data-ttu-id="bfb97-214">V: azure biztonságosan logika visszahívási URL-címek egy közös hozzáférésű Jogosultságkód (SAS) használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bfb97-214">A: Azure securely generates logic app callback URLs using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="bfb97-215">Az aláírás haladnak keresztül lekérdezési paramétert, és ellenőrizni kell, mielőtt a Logic Apps alkalmazást is érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="bfb97-215">This signature passes through as a query parameter and must be validated before your logic app can fire.</span></span> <span data-ttu-id="bfb97-216">Az Azure létrehoz egy titkos kulcsot a Logic Apps alkalmazást, a hello eseményindító nevét és a hello műveletet végzett egyedi kombinációját használva hello aláírás.</span><span class="sxs-lookup"><span data-stu-id="bfb97-216">Azure generates hello signature using a unique combination of a secret key per logic app, hello trigger name, and hello operation that's performed.</span></span> <span data-ttu-id="bfb97-217">Kivéve, ha valaki hozzáférést toohello titkos logic app kulccsal rendelkezik, azok nem hoznak létre érvényes aláírással.</span><span class="sxs-lookup"><span data-stu-id="bfb97-217">So unless someone has access toohello secret logic app key, they cannot generate a valid signature.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="bfb97-218">A gyártás és biztonságos erősen ajánlott elleni hívja közvetlenül hello böngészőből a Logic Apps alkalmazást, mert:</span><span class="sxs-lookup"><span data-stu-id="bfb97-218">For production and secure systems, we strongly recommend against calling your logic app directly from hello browser because:</span></span>
   > 
   > * <span data-ttu-id="bfb97-219">hello megosztott elérési kulcsával hello URL-cím jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bfb97-219">hello shared access key appears in hello URL.</span></span>
   > * <span data-ttu-id="bfb97-220">Biztonságos tartalom házirendek tooshared tartományok miatt nem tudja kezelni a logikai alkalmazást az ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="bfb97-220">You can't manage secure content policies due tooshared domains across Logic App customers.</span></span>

#### <a name="q-can-i-configure-http-endpoints-further"></a><span data-ttu-id="bfb97-221">K: beállítani a HTTP-végpontokról további?</span><span class="sxs-lookup"><span data-stu-id="bfb97-221">Q: Can I configure HTTP endpoints further?</span></span>

<span data-ttu-id="bfb97-222">V: Igen, támogatja a HTTP-végpontokról a keresztül speciális konfigurációs [ **API Management**](../api-management/api-management-key-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="bfb97-222">A: Yes, HTTP endpoints support more advanced configuration through [**API Management**](../api-management/api-management-key-concepts.md).</span></span> <span data-ttu-id="bfb97-223">Ezt a szolgáltatást is kínál hello funkció, akkor tooconsistently kezelhetők minden az API-kat, beleértve a logic apps, állítson be egyéni tartománynevek, használja a további hitelesítési módszerek és egyéb, például:</span><span class="sxs-lookup"><span data-stu-id="bfb97-223">This service also offers hello capability for you tooconsistently manage all your APIs, including logic apps, set up custom domain names, use more authentication methods, and more, for example:</span></span>

* [<span data-ttu-id="bfb97-224">Hello kérelem módszer váltása</span><span class="sxs-lookup"><span data-stu-id="bfb97-224">Change hello request method</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [<span data-ttu-id="bfb97-225">Hello URL-szegmensek hello kérelem módosítása</span><span class="sxs-lookup"><span data-stu-id="bfb97-225">Change hello URL segments of hello request</span></span>](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* <span data-ttu-id="bfb97-226">Állítsa be az API Management tartományok hello [Azure-portálon](https://portal.azure.com/ "Azure-portálon")</span><span class="sxs-lookup"><span data-stu-id="bfb97-226">Set up your API Management domains in hello [Azure portal](https://portal.azure.com/ "Azure portal")</span></span>
* <span data-ttu-id="bfb97-227">Az egyszerű hitelesítés házirend toocheck beállítása</span><span class="sxs-lookup"><span data-stu-id="bfb97-227">Set up policy toocheck for Basic authentication</span></span>

#### <a name="q-what-changed-when-hello-schema-migrated-from-hello-december-1-2014-preview"></a><span data-ttu-id="bfb97-228">K: milyen megváltozik, miközben hello séma átemelt hello 2014. decemberi 1 preview?</span><span class="sxs-lookup"><span data-stu-id="bfb97-228">Q: What changed when hello schema migrated from hello December 1, 2014 preview?</span></span>

<span data-ttu-id="bfb97-229">V: Itt található egy Összegzés, ezekről a változásokról:</span><span class="sxs-lookup"><span data-stu-id="bfb97-229">A: Here's a summary about these changes:</span></span>

| <span data-ttu-id="bfb97-230">2014. decemberi 1 preview</span><span class="sxs-lookup"><span data-stu-id="bfb97-230">December 1, 2014 preview</span></span> | <span data-ttu-id="bfb97-231">2016. június 1.</span><span class="sxs-lookup"><span data-stu-id="bfb97-231">June 1, 2016</span></span> |
| --- | --- |
| <span data-ttu-id="bfb97-232">Kattintson a **HTTP-figyelő** API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="bfb97-232">Click **HTTP Listener** API App</span></span> |<span data-ttu-id="bfb97-233">Kattintson a **kézi indítási** (API-alkalmazás nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="bfb97-233">Click **Manual trigger** (no API App required)</span></span> |
| <span data-ttu-id="bfb97-234">HTTP-figyelő beállítás "*automatikusan választ küld*"</span><span class="sxs-lookup"><span data-stu-id="bfb97-234">HTTP Listener setting "*Sends response automatically*"</span></span> |<span data-ttu-id="bfb97-235">Tartalmaznak egy **válasz** művelet, vagy nem az hello munkafolyamat-definíció</span><span class="sxs-lookup"><span data-stu-id="bfb97-235">Either include a **Response** action or not in hello workflow definition</span></span> |
| <span data-ttu-id="bfb97-236">Basic vagy az OAuth-hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bfb97-236">Configure Basic or OAuth authentication</span></span> |<span data-ttu-id="bfb97-237">API Management szolgáltatáson keresztül</span><span class="sxs-lookup"><span data-stu-id="bfb97-237">via API Management</span></span> |
| <span data-ttu-id="bfb97-238">HTTP-módszer konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bfb97-238">Configure HTTP method</span></span> |<span data-ttu-id="bfb97-239">A **speciális beállítások megjelenítése**, válassza a HTTP-metódus</span><span class="sxs-lookup"><span data-stu-id="bfb97-239">Under **Show advanced options**, choose an HTTP method</span></span> |
| <span data-ttu-id="bfb97-240">Adja meg relatív elérési útját</span><span class="sxs-lookup"><span data-stu-id="bfb97-240">Configure relative path</span></span> |<span data-ttu-id="bfb97-241">A **speciális beállítások megjelenítése**, relatív elérési út</span><span class="sxs-lookup"><span data-stu-id="bfb97-241">Under **Show advanced options**, add a relative path</span></span> |
| <span data-ttu-id="bfb97-242">Hivatkozás hello bejövő törzs keresztül`@triggerOutputs().body.Content`</span><span class="sxs-lookup"><span data-stu-id="bfb97-242">Reference hello incoming body through `@triggerOutputs().body.Content`</span></span> |<span data-ttu-id="bfb97-243">A hivatkozás`@triggerOutputs().body`</span><span class="sxs-lookup"><span data-stu-id="bfb97-243">Reference through `@triggerOutputs().body`</span></span> |
| <span data-ttu-id="bfb97-244">**HTTP-válasz küldése** műveletet a hello HTTP-figyelő</span><span class="sxs-lookup"><span data-stu-id="bfb97-244">**Send HTTP response** action on hello HTTP Listener</span></span> |<span data-ttu-id="bfb97-245">Kattintson a **válaszoljon tooHTTP kérelem** (API-alkalmazás nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="bfb97-245">Click **Respond tooHTTP request** (no API App required)</span></span> |

## <a name="get-help"></a><span data-ttu-id="bfb97-246">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="bfb97-246">Get help</span></span>

<span data-ttu-id="bfb97-247">tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="bfb97-247">tooask questions, answer questions, and learn what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="bfb97-248">toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="bfb97-248">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfb97-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bfb97-249">Next steps</span></span>

* [<span data-ttu-id="bfb97-250">Logikaialkalmazás-definíciók készítése</span><span class="sxs-lookup"><span data-stu-id="bfb97-250">Author logic app definitions</span></span>](./logic-apps-author-definitions.md)
* [<span data-ttu-id="bfb97-251">Hibák és kivételek kezelése</span><span class="sxs-lookup"><span data-stu-id="bfb97-251">Handle errors and exceptions</span></span>](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
