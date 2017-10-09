---
title: "aaaAzure Resource Manager sablon struktúra és szintaxis |} Microsoft Docs"
description: "Hello struktúra és az Azure Resource Manager-sablonok deklaratív JSON-szintaxis használatával tulajdonságait ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0709852f8777c91cc1704d6bca16257a017d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-structure-and-syntax-of-azure-resource-manager-templates"></a><span data-ttu-id="b46f4-103">Hello struktúra és az Azure Resource Manager-sablonok szintaxisát ismertetése</span><span class="sxs-lookup"><span data-stu-id="b46f4-103">Understand hello structure and syntax of Azure Resource Manager templates</span></span>
<span data-ttu-id="b46f4-104">Ez a témakör ismerteti az Azure Resource Manager sablon hello szerkezete.</span><span class="sxs-lookup"><span data-stu-id="b46f4-104">This topic describes hello structure of an Azure Resource Manager template.</span></span> <span data-ttu-id="b46f4-105">Azt mutatja be a különböző szakaszaiban hello egy sablon és hello elérhető tulajdonságok köre szakaszt.</span><span class="sxs-lookup"><span data-stu-id="b46f4-105">It presents hello different sections of a template and hello properties that are available in those sections.</span></span> <span data-ttu-id="b46f4-106">hello sablon JSON és, hogy tooconstruct értékeket is használhat az üzembe helyezéshez kifejezések áll.</span><span class="sxs-lookup"><span data-stu-id="b46f4-106">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="b46f4-107">A sablonok létrehozásának részletes oktatóanyaga, lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-107">For a step-by-step tutorial on creating a template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>

## <a name="template-format"></a><span data-ttu-id="b46f4-108">Sablon formátumban</span><span class="sxs-lookup"><span data-stu-id="b46f4-108">Template format</span></span>
<span data-ttu-id="b46f4-109">A legegyszerűbb szerkezetét egy sablon tartalmazza a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="b46f4-109">In its simplest structure, a template contains hello following elements:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| <span data-ttu-id="b46f4-110">Elem neve</span><span class="sxs-lookup"><span data-stu-id="b46f4-110">Element name</span></span> | <span data-ttu-id="b46f4-111">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b46f4-111">Required</span></span> | <span data-ttu-id="b46f4-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="b46f4-112">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b46f4-113">$schema</span><span class="sxs-lookup"><span data-stu-id="b46f4-113">$schema</span></span> |<span data-ttu-id="b46f4-114">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-114">Yes</span></span> |<span data-ttu-id="b46f4-115">Hello JSON séma-fájl helyét, amely leírja a hello hello sablon nyelvi verzióját.</span><span class="sxs-lookup"><span data-stu-id="b46f4-115">Location of hello JSON schema file that describes hello version of hello template language.</span></span> <span data-ttu-id="b46f4-116">A fenti példa hello látható hello URL-címet használja.</span><span class="sxs-lookup"><span data-stu-id="b46f4-116">Use hello URL shown in hello preceding example.</span></span> |
| <span data-ttu-id="b46f4-117">contentVersion</span><span class="sxs-lookup"><span data-stu-id="b46f4-117">contentVersion</span></span> |<span data-ttu-id="b46f4-118">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-118">Yes</span></span> |<span data-ttu-id="b46f4-119">Hello sablon (például 1.0.0.0) verzióját.</span><span class="sxs-lookup"><span data-stu-id="b46f4-119">Version of hello template (such as 1.0.0.0).</span></span> <span data-ttu-id="b46f4-120">Bármely érték biztosíthat ehhez az elemhez.</span><span class="sxs-lookup"><span data-stu-id="b46f4-120">You can provide any value for this element.</span></span> <span data-ttu-id="b46f4-121">Erőforrások hello sablonnal való telepítésekor, az érték lehet használt toomake arra, hogy hello a megfelelő sablon használja.</span><span class="sxs-lookup"><span data-stu-id="b46f4-121">When deploying resources using hello template, this value can be used toomake sure that hello right template is being used.</span></span> |
| <span data-ttu-id="b46f4-122">paraméterek</span><span class="sxs-lookup"><span data-stu-id="b46f4-122">parameters</span></span> |<span data-ttu-id="b46f4-123">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-123">No</span></span> |<span data-ttu-id="b46f4-124">Értékek által biztosított, ha a központi telepítés végrehajtása toocustomize erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="b46f4-124">Values that are provided when deployment is executed toocustomize resource deployment.</span></span> |
| <span data-ttu-id="b46f4-125">változók</span><span class="sxs-lookup"><span data-stu-id="b46f4-125">variables</span></span> |<span data-ttu-id="b46f4-126">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-126">No</span></span> |<span data-ttu-id="b46f4-127">Hello toosimplify sablon sablonnyelvi kifejezéseket JSON szilánkok használt értékek.</span><span class="sxs-lookup"><span data-stu-id="b46f4-127">Values that are used as JSON fragments in hello template toosimplify template language expressions.</span></span> |
| <span data-ttu-id="b46f4-128">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="b46f4-128">resources</span></span> |<span data-ttu-id="b46f4-129">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-129">Yes</span></span> |<span data-ttu-id="b46f4-130">Telepített vagy frissített erőforráscsoportban erőforrástípusok esetében.</span><span class="sxs-lookup"><span data-stu-id="b46f4-130">Resource types that are deployed or updated in a resource group.</span></span> |
| <span data-ttu-id="b46f4-131">kimenetek</span><span class="sxs-lookup"><span data-stu-id="b46f4-131">outputs</span></span> |<span data-ttu-id="b46f4-132">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-132">No</span></span> |<span data-ttu-id="b46f4-133">A telepítés utáni visszaadott értékek.</span><span class="sxs-lookup"><span data-stu-id="b46f4-133">Values that are returned after deployment.</span></span> |

<span data-ttu-id="b46f4-134">Minden elem beállítható tulajdonságokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b46f4-134">Each element contains properties you can set.</span></span> <span data-ttu-id="b46f4-135">a következő példa hello sablon hello teljes szintaxisát tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b46f4-135">hello following example contains hello full syntax for a template:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-hello parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

<span data-ttu-id="b46f4-136">Azt vizsgálja meg a témakör későbbi részében részletesebben hello sablon hello szakaszait.</span><span class="sxs-lookup"><span data-stu-id="b46f4-136">We examine hello sections of hello template in greater detail later in this topic.</span></span>

## <a name="expressions-and-functions"></a><span data-ttu-id="b46f4-137">Kifejezések és funkciók</span><span class="sxs-lookup"><span data-stu-id="b46f4-137">Expressions and functions</span></span>
<span data-ttu-id="b46f4-138">hello alapszintű hello sablon szintaxisa JSON.</span><span class="sxs-lookup"><span data-stu-id="b46f4-138">hello basic syntax of hello template is JSON.</span></span> <span data-ttu-id="b46f4-139">Kifejezések és a funkciók kiterjesztése hello JSON értékek belül hello sablon érhető el.</span><span class="sxs-lookup"><span data-stu-id="b46f4-139">However, expressions and functions extend hello JSON values available within hello template.</span></span>  <span data-ttu-id="b46f4-140">Kifejezések íródtak belül JSON szövegkonstansok amelynek első és utolsó karaktere hello zárójelek közé: `[` és `]`, illetve.</span><span class="sxs-lookup"><span data-stu-id="b46f4-140">Expressions are written within JSON string literals whose first and last characters are hello brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="b46f4-141">hello érték hello kifejezés kiértékelése hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="b46f4-141">hello value of hello expression is evaluated when hello template is deployed.</span></span> <span data-ttu-id="b46f4-142">Megírva egy szöveges karakterlánc, miközben hello eredménye hello kifejezés kiértékelése lehet eltérő típusú JSON, például a tömb vagy egész, attól függően, hogy hello tényleges kifejezést.</span><span class="sxs-lookup"><span data-stu-id="b46f4-142">While written as a string literal, hello result of evaluating hello expression can be of a different JSON type, such as an array or integer, depending on hello actual expression.</span></span>  <span data-ttu-id="b46f4-143">szövegkonstansnak indítsa el a zárójel toohave `[`, de nem rendelkezik az kifejezésként értelmezi, a felesleges zárójel toostart hello karakterlánc hozzáadása `[[`.</span><span class="sxs-lookup"><span data-stu-id="b46f4-143">toohave a literal string start with a bracket `[`, but not have it interpreted as an expression, add an extra bracket toostart hello string with `[[`.</span></span>

<span data-ttu-id="b46f4-144">Általában akkor használják kifejezések funkciók tooperform műveletekkel hello telepítési konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="b46f4-144">Typically, you use expressions with functions tooperform operations for configuring hello deployment.</span></span> <span data-ttu-id="b46f4-145">Csak a például a JavaScript, függvényhívások-ként formázott `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="b46f4-145">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="b46f4-146">Tulajdonságok hello pont és a [index] operátorok használatával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b46f4-146">You reference properties by using hello dot and [index] operators.</span></span>

<span data-ttu-id="b46f4-147">hello a következő példa bemutatja, hogyan több működik térített toouse értékeket:</span><span class="sxs-lookup"><span data-stu-id="b46f4-147">hello following example shows how toouse several functions when constructing values:</span></span>

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

<span data-ttu-id="b46f4-148">Hello a sablon funkciók teljes listáját, lásd: [Azure Resource Manager sablonfüggvényei](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-148">For hello full list of template functions, see [Azure Resource Manager template functions](resource-group-template-functions.md).</span></span> 

## <a name="parameters"></a><span data-ttu-id="b46f4-149">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="b46f4-149">Parameters</span></span>
<span data-ttu-id="b46f4-150">Hello paraméterek szakaszban hello sablon megadhatja, mely értékei telepítésekor szövegszerkesztőben hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b46f4-150">In hello parameters section of hello template, you specify which values you can input when deploying hello resources.</span></span> <span data-ttu-id="b46f4-151">A paraméterértékek lehetővé teszik a toocustomize hello telepítési egy adott környezetben (például a fejlesztői, tesztelési és éles) is lefednek értékek megadásával.</span><span class="sxs-lookup"><span data-stu-id="b46f4-151">These parameter values enable you toocustomize hello deployment by providing values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="b46f4-152">Nincs tooprovide paramétereket a sablonban, de a paraméterek nélkül a sablon mindig központilag hello ugyanazokat az erőforrásokat a hello azonos nevek, helyek és -tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="b46f4-152">You do not have tooprovide parameters in your template, but without parameters your template would always deploy hello same resources with hello same names, locations, and properties.</span></span>

<span data-ttu-id="b46f4-153">A következő struktúra hello definiálhatja a paramétereket:</span><span class="sxs-lookup"><span data-stu-id="b46f4-153">You define parameters with hello following structure:</span></span>

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-hello parameter>" 
        }
    }
}
```

| <span data-ttu-id="b46f4-154">Elem neve</span><span class="sxs-lookup"><span data-stu-id="b46f4-154">Element name</span></span> | <span data-ttu-id="b46f4-155">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b46f4-155">Required</span></span> | <span data-ttu-id="b46f4-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="b46f4-156">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b46f4-157">parameterName</span><span class="sxs-lookup"><span data-stu-id="b46f4-157">parameterName</span></span> |<span data-ttu-id="b46f4-158">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-158">Yes</span></span> |<span data-ttu-id="b46f4-159">Hello paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="b46f4-159">Name of hello parameter.</span></span> <span data-ttu-id="b46f4-160">Egy érvényes JavaScript-azonosítónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b46f4-160">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="b46f4-161">type</span><span class="sxs-lookup"><span data-stu-id="b46f4-161">type</span></span> |<span data-ttu-id="b46f4-162">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-162">Yes</span></span> |<span data-ttu-id="b46f4-163">Hello paraméter értékének típusa.</span><span class="sxs-lookup"><span data-stu-id="b46f4-163">Type of hello parameter value.</span></span> <span data-ttu-id="b46f4-164">Engedélyezett típusokkal hello listáját lásd a táblázat után.</span><span class="sxs-lookup"><span data-stu-id="b46f4-164">See hello list of allowed types after this table.</span></span> |
| <span data-ttu-id="b46f4-165">DefaultValue érték</span><span class="sxs-lookup"><span data-stu-id="b46f4-165">defaultValue</span></span> |<span data-ttu-id="b46f4-166">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-166">No</span></span> |<span data-ttu-id="b46f4-167">Hello paraméter, ha nincs érték megadva hello paraméter alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="b46f4-167">Default value for hello parameter, if no value is provided for hello parameter.</span></span> |
| <span data-ttu-id="b46f4-168">Storageaccount_accounttype</span><span class="sxs-lookup"><span data-stu-id="b46f4-168">allowedValues</span></span> |<span data-ttu-id="b46f4-169">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-169">No</span></span> |<span data-ttu-id="b46f4-170">Engedélyezett értékek hello paraméter toomake, hogy hello jobb oldali értéktengely rendelkezik tömbje.</span><span class="sxs-lookup"><span data-stu-id="b46f4-170">Array of allowed values for hello parameter toomake sure that hello right value is provided.</span></span> |
| <span data-ttu-id="b46f4-171">A MinValue</span><span class="sxs-lookup"><span data-stu-id="b46f4-171">minValue</span></span> |<span data-ttu-id="b46f4-172">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-172">No</span></span> |<span data-ttu-id="b46f4-173">hello minimális int típusú paraméterekhez, ez az érték értéke között lehet.</span><span class="sxs-lookup"><span data-stu-id="b46f4-173">hello minimum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="b46f4-174">MaxValue</span><span class="sxs-lookup"><span data-stu-id="b46f4-174">maxValue</span></span> |<span data-ttu-id="b46f4-175">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-175">No</span></span> |<span data-ttu-id="b46f4-176">hello maximális int típusú paraméterekhez, ez az érték értéke között lehet.</span><span class="sxs-lookup"><span data-stu-id="b46f4-176">hello maximum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="b46f4-177">a minLength</span><span class="sxs-lookup"><span data-stu-id="b46f4-177">minLength</span></span> |<span data-ttu-id="b46f4-178">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-178">No</span></span> |<span data-ttu-id="b46f4-179">hello minimális string, secureString és array típusú paraméterekhez, ez az érték hossza határokat is beleértve.</span><span class="sxs-lookup"><span data-stu-id="b46f4-179">hello minimum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="b46f4-180">MaxLength</span><span class="sxs-lookup"><span data-stu-id="b46f4-180">maxLength</span></span> |<span data-ttu-id="b46f4-181">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-181">No</span></span> |<span data-ttu-id="b46f4-182">hello maximális string, secureString és array típusú paraméterekhez, ez az érték hossza határokat is beleértve.</span><span class="sxs-lookup"><span data-stu-id="b46f4-182">hello maximum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="b46f4-183">leírás</span><span class="sxs-lookup"><span data-stu-id="b46f4-183">description</span></span> |<span data-ttu-id="b46f4-184">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-184">No</span></span> |<span data-ttu-id="b46f4-185">Hello paraméter, amely leírása toousers hello portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b46f4-185">Description of hello parameter that is displayed toousers through hello portal.</span></span> |

<span data-ttu-id="b46f4-186">hello engedélyezett típusokkal és értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="b46f4-186">hello allowed types and values are:</span></span>

* <span data-ttu-id="b46f4-187">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="b46f4-187">**string**</span></span>
* <span data-ttu-id="b46f4-188">**secureString**</span><span class="sxs-lookup"><span data-stu-id="b46f4-188">**secureString**</span></span>
* <span data-ttu-id="b46f4-189">**int**</span><span class="sxs-lookup"><span data-stu-id="b46f4-189">**int**</span></span>
* <span data-ttu-id="b46f4-190">**logikai érték**</span><span class="sxs-lookup"><span data-stu-id="b46f4-190">**bool**</span></span>
* <span data-ttu-id="b46f4-191">**objektum**</span><span class="sxs-lookup"><span data-stu-id="b46f4-191">**object**</span></span> 
* <span data-ttu-id="b46f4-192">**secureObject**</span><span class="sxs-lookup"><span data-stu-id="b46f4-192">**secureObject**</span></span>
* <span data-ttu-id="b46f4-193">**a tömb**</span><span class="sxs-lookup"><span data-stu-id="b46f4-193">**array**</span></span>

<span data-ttu-id="b46f4-194">toospecify választható, mert egy paraméter adja meg a DefaultValue érték (üres is lehet).</span><span class="sxs-lookup"><span data-stu-id="b46f4-194">toospecify a parameter as optional, provide a defaultValue (can be an empty string).</span></span> 

<span data-ttu-id="b46f4-195">A paraméter neve a sablon megfelelő hello parancs toodeploy hello sablon egyik paraméterének ad meg, ha nincs megadta hello értékek lehetséges egyértelműek.</span><span class="sxs-lookup"><span data-stu-id="b46f4-195">If you specify a parameter name in your template that matches a parameter in hello command toodeploy hello template, there is potential ambiguity about hello values you provide.</span></span> <span data-ttu-id="b46f4-196">Erőforrás-kezelő hello utótag hozzáadásával oldja fel a félreértések **FromTemplate** toohello sablonparaméter.</span><span class="sxs-lookup"><span data-stu-id="b46f4-196">Resource Manager resolves this confusion by adding hello postfix **FromTemplate** toohello template parameter.</span></span> <span data-ttu-id="b46f4-197">Például, ha nevű paraméter **ResourceGroupName** a sablonban ütközik hello **ResourceGroupName** hello paraméterének [ Új AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b46f4-197">For example, if you include a parameter named **ResourceGroupName** in your template, it conflicts with hello **ResourceGroupName** parameter in hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="b46f4-198">A telepítés során áll felszólító tooprovide értéket **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="b46f4-198">During deployment, you are prompted tooprovide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="b46f4-199">Ez zavart ne általában a nem azonos nevet telepítési műveleteihez használt paraméterekként hello paramétereket.</span><span class="sxs-lookup"><span data-stu-id="b46f4-199">In general, you should avoid this confusion by not naming parameters with hello same name as parameters used for deployment operations.</span></span>

> [!NOTE]
> <span data-ttu-id="b46f4-200">Jelszavak, kulcsokat és más titkos adatokat kell használni a hello **secureString** típusa.</span><span class="sxs-lookup"><span data-stu-id="b46f4-200">All passwords, keys, and other secrets should use hello **secureString** type.</span></span> <span data-ttu-id="b46f4-201">Ha JSON-objektum átadni a bizalmas adatokat, használja a hello **secureObject** típusa.</span><span class="sxs-lookup"><span data-stu-id="b46f4-201">If you pass sensitive data in a JSON object, use hello **secureObject** type.</span></span> <span data-ttu-id="b46f4-202">Sablonparaméterek secureString vagy secureObject típusú erőforrások telepítése után nem lehet olvasni.</span><span class="sxs-lookup"><span data-stu-id="b46f4-202">Template parameters with secureString or secureObject types cannot be read after resource deployment.</span></span> 
> 
> <span data-ttu-id="b46f4-203">Például hello hello üzembe helyezési előzményeket a következő bejegyzés mutatja egy karakterláncot és objektumot, de nem a secureString és secureObject hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="b46f4-203">For example, hello following entry in hello deployment history shows hello value for a string and object but not for secureString and secureObject.</span></span>
>
> ![központi telepítés értékek megjelenítése](./media/resource-group-authoring-templates/show-parameters.png)  
>

<span data-ttu-id="b46f4-205">a következő példa azt mutatja meg hogyan hello toodefine paraméterek:</span><span class="sxs-lookup"><span data-stu-id="b46f4-205">hello following example shows how toodefine parameters:</span></span>

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

<span data-ttu-id="b46f4-206">Hogyan tooinput hello paraméter értékei üzembe helyezése során, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-206">For how tooinput hello parameter values during deployment, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span> 

## <a name="variables"></a><span data-ttu-id="b46f4-207">Változók</span><span class="sxs-lookup"><span data-stu-id="b46f4-207">Variables</span></span>
<span data-ttu-id="b46f4-208">Hello a változók szakaszban, a sablon egész érték, amely használható hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b46f4-208">In hello variables section, you construct values that can be used throughout your template.</span></span> <span data-ttu-id="b46f4-209">Nem kell toodefine változók, de azok gyakran leegyszerűsítheti a sablon csökkentésével összetett kifejezések.</span><span class="sxs-lookup"><span data-stu-id="b46f4-209">You do not need toodefine variables, but they often simplify your template by reducing complex expressions.</span></span>

<span data-ttu-id="b46f4-210">A következő struktúra hello adja meg a változókat:</span><span class="sxs-lookup"><span data-stu-id="b46f4-210">You define variables with hello following structure:</span></span>

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

<span data-ttu-id="b46f4-211">a következő példa azt mutatja meg hogyan hello toodefine paraméter két érték közül összeállított változó:</span><span class="sxs-lookup"><span data-stu-id="b46f4-211">hello following example shows how toodefine a variable that is constructed from two parameter values:</span></span>

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

<span data-ttu-id="b46f4-212">hello következő példában egy változó, amely egy összetett JSON-típus, és a változókat, amelyek más változók össze:</span><span class="sxs-lookup"><span data-stu-id="b46f4-212">hello next example shows a variable that is a complex JSON type, and variables that are constructed from other variables:</span></span>

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a><span data-ttu-id="b46f4-213">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="b46f4-213">Resources</span></span>
<span data-ttu-id="b46f4-214">Hello erőforrások szakaszában vannak telepítve, vagy frissített hello erőforrások határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b46f4-214">In hello resources section, you define hello resources that are deployed or updated.</span></span> <span data-ttu-id="b46f4-215">Ez a szakasz kérheti le bonyolult, mert ismernie kell a hello meg kell adnia tett tooprovide hello megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="b46f4-215">This section can get complicated because you must understand hello types you are deploying tooprovide hello right values.</span></span> <span data-ttu-id="b46f4-216">Tekintse meg a hello erőforrás-specifikus értékeket (apiVersion, típusa és tulajdonságait), hogy kell-e tooset [meghatározhatja az erőforrásokat az Azure Resource Manager sablonokban](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="b46f4-216">For hello resource-specific values (apiVersion, type, and properties) that you need tooset, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span> 

<span data-ttu-id="b46f4-217">Erőforrások a következő struktúra hello határozhat meg:</span><span class="sxs-lookup"><span data-stu-id="b46f4-217">You define resources with hello following structure:</span></span>

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| <span data-ttu-id="b46f4-218">Elem neve</span><span class="sxs-lookup"><span data-stu-id="b46f4-218">Element name</span></span> | <span data-ttu-id="b46f4-219">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b46f4-219">Required</span></span> | <span data-ttu-id="b46f4-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="b46f4-220">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b46f4-221">Az állapot</span><span class="sxs-lookup"><span data-stu-id="b46f4-221">condition</span></span> | <span data-ttu-id="b46f4-222">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-222">No</span></span> | <span data-ttu-id="b46f4-223">Logikai érték, amely azt jelzi, hogy telepítve van-e a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b46f4-223">Boolean value that indicates whether hello resource is deployed.</span></span> |
| <span data-ttu-id="b46f4-224">apiVersion</span><span class="sxs-lookup"><span data-stu-id="b46f4-224">apiVersion</span></span> |<span data-ttu-id="b46f4-225">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-225">Yes</span></span> |<span data-ttu-id="b46f4-226">Hello REST API toouse hello erőforrás létrehozásához verzióját.</span><span class="sxs-lookup"><span data-stu-id="b46f4-226">Version of hello REST API toouse for creating hello resource.</span></span> |
| <span data-ttu-id="b46f4-227">type</span><span class="sxs-lookup"><span data-stu-id="b46f4-227">type</span></span> |<span data-ttu-id="b46f4-228">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-228">Yes</span></span> |<span data-ttu-id="b46f4-229">Hello erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="b46f4-229">Type of hello resource.</span></span> <span data-ttu-id="b46f4-230">Az értéket nem hello névtér hello erőforrás-szolgáltató és hello erőforrástípus kombinációja (például **Microsoft.Storage/storageAccounts**).</span><span class="sxs-lookup"><span data-stu-id="b46f4-230">This value is a combination of hello namespace of hello resource provider and hello resource type (such as **Microsoft.Storage/storageAccounts**).</span></span> |
| <span data-ttu-id="b46f4-231">név</span><span class="sxs-lookup"><span data-stu-id="b46f4-231">name</span></span> |<span data-ttu-id="b46f4-232">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-232">Yes</span></span> |<span data-ttu-id="b46f4-233">Hello erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="b46f4-233">Name of hello resource.</span></span> <span data-ttu-id="b46f4-234">hello neve URI összetevő korlátozások RFC3986 definiált kell követnie.</span><span class="sxs-lookup"><span data-stu-id="b46f4-234">hello name must follow URI component restrictions defined in RFC3986.</span></span> <span data-ttu-id="b46f4-235">Emellett Azure-szolgáltatások hello erőforrás neve toooutside felek visszaállítását érvényesítése hello neve toomake rá, hogy ez nem egy kísérlet toospoof másik identitást.</span><span class="sxs-lookup"><span data-stu-id="b46f4-235">In addition, Azure services that expose hello resource name toooutside parties validate hello name toomake sure it is not an attempt toospoof another identity.</span></span> |
| <span data-ttu-id="b46f4-236">location</span><span class="sxs-lookup"><span data-stu-id="b46f4-236">location</span></span> |<span data-ttu-id="b46f4-237">Változó</span><span class="sxs-lookup"><span data-stu-id="b46f4-237">Varies</span></span> |<span data-ttu-id="b46f4-238">A megadott erőforrás hello támogatott földrajzi-helyét.</span><span class="sxs-lookup"><span data-stu-id="b46f4-238">Supported geo-locations of hello provided resource.</span></span> <span data-ttu-id="b46f4-239">Kiválaszthatja hello elérhető helyek valamelyikén, de általában teszi logika toopick tartalmazó Bezárás tooyour felhasználók.</span><span class="sxs-lookup"><span data-stu-id="b46f4-239">You can select any of hello available locations, but typically it makes sense toopick one that is close tooyour users.</span></span> <span data-ttu-id="b46f4-240">Általában is érdemes tooplace erőforrások adatcserét a hello azonos régióban.</span><span class="sxs-lookup"><span data-stu-id="b46f4-240">Usually, it also makes sense tooplace resources that interact with each other in hello same region.</span></span> <span data-ttu-id="b46f4-241">A legtöbb erőforrás szükséges egy helyre, de néhány típust (például egy szerepkör-hozzárendelés) igényel egy helyre.</span><span class="sxs-lookup"><span data-stu-id="b46f4-241">Most resource types require a location, but some types (such as a role assignment) do not require a location.</span></span> <span data-ttu-id="b46f4-242">Lásd: [erőforrás hely beállítása az Azure Resource Manager sablonokban](resource-manager-template-location.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-242">See [Set resource location in Azure Resource Manager templates](resource-manager-template-location.md).</span></span> |
| <span data-ttu-id="b46f4-243">tags</span><span class="sxs-lookup"><span data-stu-id="b46f4-243">tags</span></span> |<span data-ttu-id="b46f4-244">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-244">No</span></span> |<span data-ttu-id="b46f4-245">Hello erőforrás társított címkékkel.</span><span class="sxs-lookup"><span data-stu-id="b46f4-245">Tags that are associated with hello resource.</span></span> <span data-ttu-id="b46f4-246">Lásd: [címkével olyan erőforrásokat az Azure Resource Manager sablonokban](resource-manager-template-tags.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-246">See [Tag resources in Azure Resource Manager templates](resource-manager-template-tags.md).</span></span> |
| <span data-ttu-id="b46f4-247">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b46f4-247">comments</span></span> |<span data-ttu-id="b46f4-248">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-248">No</span></span> |<span data-ttu-id="b46f4-249">A Megjegyzések a hello erőforrások a sablonban dokumentálása</span><span class="sxs-lookup"><span data-stu-id="b46f4-249">Your notes for documenting hello resources in your template</span></span> |
| <span data-ttu-id="b46f4-250">Másolás</span><span class="sxs-lookup"><span data-stu-id="b46f4-250">copy</span></span> |<span data-ttu-id="b46f4-251">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-251">No</span></span> |<span data-ttu-id="b46f4-252">Ha egynél több példány van szükség, hello erőforrások toocreate száma.</span><span class="sxs-lookup"><span data-stu-id="b46f4-252">If more than one instance is needed, hello number of resources toocreate.</span></span> <span data-ttu-id="b46f4-253">hello alapértelmezett módja párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="b46f4-253">hello default mode is parallel.</span></span> <span data-ttu-id="b46f4-254">Adja meg a soros módban, ha nem szeretné, hogy az összes vagy erőforrások toodeploy hello: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="b46f4-254">Specify serial mode when you do not want all or hello resources toodeploy at hello same time.</span></span> <span data-ttu-id="b46f4-255">További információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-255">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="b46f4-256">dependsOn</span><span class="sxs-lookup"><span data-stu-id="b46f4-256">dependsOn</span></span> |<span data-ttu-id="b46f4-257">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-257">No</span></span> |<span data-ttu-id="b46f4-258">Ehhez az erőforráshoz központi telepítése előtt telepíteni kell erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b46f4-258">Resources that must be deployed before this resource is deployed.</span></span> <span data-ttu-id="b46f4-259">Erőforrás-kezelő erőforrások közti függőségeket hello kiértékeli, és telepíti azokat a megfelelő sorrendben hello.</span><span class="sxs-lookup"><span data-stu-id="b46f4-259">Resource Manager evaluates hello dependencies between resources and deploys them in hello correct order.</span></span> <span data-ttu-id="b46f4-260">Ha nincsenek függő erőforrások, párhuzamos központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="b46f4-260">When resources are not dependent on each other, they are deployed in parallel.</span></span> <span data-ttu-id="b46f4-261">hello érték lehet egy vesszővel elválasztott lista erőforrás nevét vagy egyedi erőforrás-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="b46f4-261">hello value can be a comma-separated list of a resource names or resource unique identifiers.</span></span> <span data-ttu-id="b46f4-262">Ez a sablon üzembe helyezett erőforrások csak felsorolása</span><span class="sxs-lookup"><span data-stu-id="b46f4-262">Only list resources that are deployed in this template.</span></span> <span data-ttu-id="b46f4-263">Erőforrások, amelyek nincsenek meghatározva a sablonban már léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="b46f4-263">Resources that are not defined in this template must already exist.</span></span> <span data-ttu-id="b46f4-264">Kerülje a szükségtelen függőségek hozzáadásával még a központi telepítés lassú, és hozzon létre körkörös függőségi viszony.</span><span class="sxs-lookup"><span data-stu-id="b46f4-264">Avoid adding unnecessary dependencies as they can slow your deployment and create circular dependencies.</span></span> <span data-ttu-id="b46f4-265">A beállítás függőségek útmutatást lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-265">For guidance on setting dependencies, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md).</span></span> |
| <span data-ttu-id="b46f4-266">properties</span><span class="sxs-lookup"><span data-stu-id="b46f4-266">properties</span></span> |<span data-ttu-id="b46f4-267">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-267">No</span></span> |<span data-ttu-id="b46f4-268">Erőforrás-specifikus konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b46f4-268">Resource-specific configuration settings.</span></span> <span data-ttu-id="b46f4-269">hello tulajdonságok értékeinek hello vannak hello azonos hello REST API művelet (PUT metódust) toocreate hello erőforrás hello kérés törzsében meg hello értékként.</span><span class="sxs-lookup"><span data-stu-id="b46f4-269">hello values for hello properties are hello same as hello values you provide in hello request body for hello REST API operation (PUT method) toocreate hello resource.</span></span> <span data-ttu-id="b46f4-270">A másolási tömb toocreate tulajdonság több példányát is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="b46f4-270">You can also specify a copy array toocreate multiple instances of a property.</span></span> <span data-ttu-id="b46f4-271">További információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-271">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="b46f4-272">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="b46f4-272">resources</span></span> |<span data-ttu-id="b46f4-273">Nem</span><span class="sxs-lookup"><span data-stu-id="b46f4-273">No</span></span> |<span data-ttu-id="b46f4-274">Múltbeli hello erőforrástól függő gyermekszintű erőforrása.</span><span class="sxs-lookup"><span data-stu-id="b46f4-274">Child resources that depend on hello resource being defined.</span></span> <span data-ttu-id="b46f4-275">Csak olyan típusú erőforrások hello séma hello szülő erőforrás által számukra engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b46f4-275">Only provide resource types that are permitted by hello schema of hello parent resource.</span></span> <span data-ttu-id="b46f4-276">hello teljesen minősített típusú hello gyermek erőforrás tartalmaz hello szülő erőforrástípusra, például **Microsoft.Web/sites/extensions**.</span><span class="sxs-lookup"><span data-stu-id="b46f4-276">hello fully qualified type of hello child resource includes hello parent resource type, such as **Microsoft.Web/sites/extensions**.</span></span> <span data-ttu-id="b46f4-277">Hello szülő erőforrás függőség nem utal.</span><span class="sxs-lookup"><span data-stu-id="b46f4-277">Dependency on hello parent resource is not implied.</span></span> <span data-ttu-id="b46f4-278">Függőséget explicit módon meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="b46f4-278">You must explicitly define that dependency.</span></span> |

<span data-ttu-id="b46f4-279">hello források szakaszában hello erőforrások toodeploy tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b46f4-279">hello resources section contains an array of hello resources toodeploy.</span></span> <span data-ttu-id="b46f4-280">Az egyes erőforrások belül is meghatározhat gyermekerőforrásait tömbjét.</span><span class="sxs-lookup"><span data-stu-id="b46f4-280">Within each resource, you can also define an array of child resources.</span></span> <span data-ttu-id="b46f4-281">Ezért a források szakaszában eredményezhet. a struktúra, például:</span><span class="sxs-lookup"><span data-stu-id="b46f4-281">Therefore, your resources section could have a structure like:</span></span>

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

<span data-ttu-id="b46f4-282">Gyermek erőforrások meghatározása kapcsolatos további információkért lásd: [Resource Manager-sablon nevét és típusát gyermek erőforrás beállított](resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-282">For more information about defining child resources, see [Set name and type for child resource in Resource Manager template](resource-manager-template-child-resource.md).</span></span>

<span data-ttu-id="b46f4-283">Hello **feltétel** elem határozza meg, hogy telepítve van-e a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b46f4-283">hello **condition** element specifies whether hello resource is deployed.</span></span> <span data-ttu-id="b46f4-284">hello érték ehhez az elemhez oldja fel a tootrue vagy HAMIS eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="b46f4-284">hello value for this element resolves tootrue or false.</span></span> <span data-ttu-id="b46f4-285">Például toospecify hogy telepít egy új tárfiókot, a rendszer használni:</span><span class="sxs-lookup"><span data-stu-id="b46f4-285">For example, toospecify whether a new storage account is deployed, use:</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="b46f4-286">Például egy új vagy meglévő erőforrást használ, tekintse meg a [új vagy meglévő feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="b46f4-286">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="b46f4-287">toospecify egy virtuális gép telepítve van a jelszóval vagy SSH-kulcsban, hogy adjon meg hello virtuális gép két verziója a sablon és -felhasználási **feltétel** toodifferentiate használat.</span><span class="sxs-lookup"><span data-stu-id="b46f4-287">toospecify whether a virtual machine is deployed with a password or SSH key, define two versions of hello virtual machine in your template and use **condition** toodifferentiate usage.</span></span> <span data-ttu-id="b46f4-288">A paraméter meghatározza, hogy mely a forgatókönyv toodeploy továbbítja.</span><span class="sxs-lookup"><span data-stu-id="b46f4-288">Pass a parameter that specifies which scenario toodeploy.</span></span>

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

<span data-ttu-id="b46f4-289">Például a jelszóval vagy SSH-kulcs toodeploy virtuális gép, [felhasználónév és SSH feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="b46f4-289">For an example of using a password or SSH key toodeploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="outputs"></a><span data-ttu-id="b46f4-290">kimenetek</span><span class="sxs-lookup"><span data-stu-id="b46f4-290">Outputs</span></span>
<span data-ttu-id="b46f4-291">Hello kimenetek szakaszban adja meg központi telepítéséből a visszaadott érték.</span><span class="sxs-lookup"><span data-stu-id="b46f4-291">In hello Outputs section, you specify values that are returned from deployment.</span></span> <span data-ttu-id="b46f4-292">Például visszaadhatja hello URI tooaccess üzembe helyezett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b46f4-292">For example, you could return hello URI tooaccess a deployed resource.</span></span>

<span data-ttu-id="b46f4-293">hello alábbi példa bemutatja egy kimeneti definíciója hello szerkezete:</span><span class="sxs-lookup"><span data-stu-id="b46f4-293">hello following example shows hello structure of an output definition:</span></span>

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| <span data-ttu-id="b46f4-294">Elem neve</span><span class="sxs-lookup"><span data-stu-id="b46f4-294">Element name</span></span> | <span data-ttu-id="b46f4-295">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b46f4-295">Required</span></span> | <span data-ttu-id="b46f4-296">Leírás</span><span class="sxs-lookup"><span data-stu-id="b46f4-296">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="b46f4-297">outputName</span><span class="sxs-lookup"><span data-stu-id="b46f4-297">outputName</span></span> |<span data-ttu-id="b46f4-298">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-298">Yes</span></span> |<span data-ttu-id="b46f4-299">Hello kimeneti érték nevét.</span><span class="sxs-lookup"><span data-stu-id="b46f4-299">Name of hello output value.</span></span> <span data-ttu-id="b46f4-300">Egy érvényes JavaScript-azonosítónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b46f4-300">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="b46f4-301">type</span><span class="sxs-lookup"><span data-stu-id="b46f4-301">type</span></span> |<span data-ttu-id="b46f4-302">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-302">Yes</span></span> |<span data-ttu-id="b46f4-303">Hello kimeneti érték típusa.</span><span class="sxs-lookup"><span data-stu-id="b46f4-303">Type of hello output value.</span></span> <span data-ttu-id="b46f4-304">Kimeneti értékek hello azonos sablon bemeneti paraméterként típusokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="b46f4-304">Output values support hello same types as template input parameters.</span></span> |
| <span data-ttu-id="b46f4-305">érték</span><span class="sxs-lookup"><span data-stu-id="b46f4-305">value</span></span> |<span data-ttu-id="b46f4-306">Igen</span><span class="sxs-lookup"><span data-stu-id="b46f4-306">Yes</span></span> |<span data-ttu-id="b46f4-307">Amely értékeli ki, és kimeneti értéket adja vissza a sablonnyelvi kifejezés.</span><span class="sxs-lookup"><span data-stu-id="b46f4-307">Template language expression that is evaluated and returned as output value.</span></span> |

<span data-ttu-id="b46f4-308">hello következő példa bemutatja a hello kimenetek szakaszban visszaadott érték.</span><span class="sxs-lookup"><span data-stu-id="b46f4-308">hello following example shows a value that is returned in hello Outputs section.</span></span>

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

<span data-ttu-id="b46f4-309">A kimeneti használatáról további információk: [állapotát az Azure Resource Manager sablonokban megosztása](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-309">For more information about working with output, see [Sharing state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="template-limits"></a><span data-ttu-id="b46f4-310">Sablon korlátok</span><span class="sxs-lookup"><span data-stu-id="b46f4-310">Template limits</span></span>

<span data-ttu-id="b46f4-311">Hello méretének korlátozása a a sablon too1 MB, és minden paraméter fájl too64 KB.</span><span class="sxs-lookup"><span data-stu-id="b46f4-311">Limit hello size of your template too1 MB, and each parameter file too64 KB.</span></span> <span data-ttu-id="b46f4-312">hello 1 MB-os korlát vonatkozik toohello végső állapot hello sablon, miután kiterjedt ismétlődő erőforrás-definíciókban és változók és a paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="b46f4-312">hello 1-MB limit applies toohello final state of hello template after it has been expanded with iterative resource definitions, and values for variables and parameters.</span></span> 

<span data-ttu-id="b46f4-313">Emellett korlátozva:</span><span class="sxs-lookup"><span data-stu-id="b46f4-313">You are also limited to:</span></span>

* <span data-ttu-id="b46f4-314">256 paraméterek</span><span class="sxs-lookup"><span data-stu-id="b46f4-314">256 parameters</span></span>
* <span data-ttu-id="b46f4-315">256 változók</span><span class="sxs-lookup"><span data-stu-id="b46f4-315">256 variables</span></span>
* <span data-ttu-id="b46f4-316">800 erőforrások (például a példányszám)</span><span class="sxs-lookup"><span data-stu-id="b46f4-316">800 resources (including copy count)</span></span>
* <span data-ttu-id="b46f4-317">64 kimeneti értékek</span><span class="sxs-lookup"><span data-stu-id="b46f4-317">64 output values</span></span>
* <span data-ttu-id="b46f4-318">egy sablon kifejezés 24,576 karakter</span><span class="sxs-lookup"><span data-stu-id="b46f4-318">24,576 characters in a template expression</span></span>

<span data-ttu-id="b46f4-319">Néhány sablon korlátot azért lépheti túl a beágyazott sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="b46f4-319">You can exceed some template limits by using a nested template.</span></span> <span data-ttu-id="b46f4-320">További információkért lásd: [kapcsolt sablonok használata az Azure-erőforrások telepítésekor](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-320">For more information, see [Using linked templates when deploying Azure resources](resource-group-linked-templates.md).</span></span> <span data-ttu-id="b46f4-321">tooreduce hello száma paramétereket, változók vagy kimenetek, kombinálható több értéket egy objektumot.</span><span class="sxs-lookup"><span data-stu-id="b46f4-321">tooreduce hello number of parameters, variables, or outputs, you can combine several values into an object.</span></span> <span data-ttu-id="b46f4-322">További információkért lásd: [paraméterekként objektumok](resource-manager-objects-as-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-322">For more information, see [Objects as parameters](resource-manager-objects-as-parameters.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b46f4-323">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b46f4-323">Next steps</span></span>
* <span data-ttu-id="b46f4-324">tooview teljes sablonok számos különböző típusú megoldások, lásd: hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="b46f4-324">tooview complete templates for many different types of solutions, see hello [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
* <span data-ttu-id="b46f4-325">Használhatja a sablonon belül a hello functions szolgáltatással kapcsolatos részletekért lásd: [Azure Resource Manager Sablonfüggvényei](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-325">For details about hello functions you can use from within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md).</span></span>
* <span data-ttu-id="b46f4-326">toocombine több sablon telepítése során lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b46f4-326">toocombine multiple templates during deployment, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="b46f4-327">Szükség lehet toouse belül egy másik erőforráscsoportban található erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="b46f4-327">You may need toouse resources that exist within a different resource group.</span></span> <span data-ttu-id="b46f4-328">Ez a forgatókönyv nem közös, ha a storage-fiókok vagy több erőforrás csoporttal megosztott virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="b46f4-328">This scenario is common when working with storage accounts or virtual networks that are shared across multiple resource groups.</span></span> <span data-ttu-id="b46f4-329">További információkért lásd: hello [resourceId függvény](resource-group-template-functions-resource.md#resourceid).</span><span class="sxs-lookup"><span data-stu-id="b46f4-329">For more information, see hello [resourceId function](resource-group-template-functions-resource.md#resourceid).</span></span>
