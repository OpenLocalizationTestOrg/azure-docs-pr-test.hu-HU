---
title: "Az Azure Resource Manager sablon struktúra és szintaxis |} Microsoft Docs"
description: "A struktúra és az Azure Resource Manager-sablonok deklaratív JSON-szintaxis használatával tulajdonságait ismerteti."
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
ms.openlocfilehash: dc9b64062d7f68c83aa090eec96744819a5ca423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a><span data-ttu-id="184f3-103">A struktúra és az Azure Resource Manager-sablonok szintaxisát ismertetése</span><span class="sxs-lookup"><span data-stu-id="184f3-103">Understand the structure and syntax of Azure Resource Manager templates</span></span>
<span data-ttu-id="184f3-104">Ez a témakör ismerteti az Azure Resource Manager sablon szerkezete.</span><span class="sxs-lookup"><span data-stu-id="184f3-104">This topic describes the structure of an Azure Resource Manager template.</span></span> <span data-ttu-id="184f3-105">Azt mutatja be a különböző szakaszokat, egy sablon és az elérhető tulajdonságok köre szakaszt.</span><span class="sxs-lookup"><span data-stu-id="184f3-105">It presents the different sections of a template and the properties that are available in those sections.</span></span> <span data-ttu-id="184f3-106">A sablon JSON és összeállítani az üzemelő példány értékeit használó kifejezéseket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="184f3-106">The template consists of JSON and expressions that you can use to construct values for your deployment.</span></span> <span data-ttu-id="184f3-107">A sablonok létrehozásának részletes oktatóanyaga, lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-107">For a step-by-step tutorial on creating a template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>

## <a name="template-format"></a><span data-ttu-id="184f3-108">Sablon formátumban</span><span class="sxs-lookup"><span data-stu-id="184f3-108">Template format</span></span>
<span data-ttu-id="184f3-109">A legegyszerűbb struktúra, a sablon a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="184f3-109">In its simplest structure, a template contains the following elements:</span></span>

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

| <span data-ttu-id="184f3-110">Elem neve</span><span class="sxs-lookup"><span data-stu-id="184f3-110">Element name</span></span> | <span data-ttu-id="184f3-111">Szükséges</span><span class="sxs-lookup"><span data-stu-id="184f3-111">Required</span></span> | <span data-ttu-id="184f3-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="184f3-112">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="184f3-113">$schema</span><span class="sxs-lookup"><span data-stu-id="184f3-113">$schema</span></span> |<span data-ttu-id="184f3-114">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-114">Yes</span></span> |<span data-ttu-id="184f3-115">A JSON-fájl, amely leírja a sablon nyelvi verzióját helye.</span><span class="sxs-lookup"><span data-stu-id="184f3-115">Location of the JSON schema file that describes the version of the template language.</span></span> <span data-ttu-id="184f3-116">Az előző példában is látható URL-CÍMÉT használja.</span><span class="sxs-lookup"><span data-stu-id="184f3-116">Use the URL shown in the preceding example.</span></span> |
| <span data-ttu-id="184f3-117">contentVersion</span><span class="sxs-lookup"><span data-stu-id="184f3-117">contentVersion</span></span> |<span data-ttu-id="184f3-118">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-118">Yes</span></span> |<span data-ttu-id="184f3-119">A sablon (például 1.0.0.0) verzióját.</span><span class="sxs-lookup"><span data-stu-id="184f3-119">Version of the template (such as 1.0.0.0).</span></span> <span data-ttu-id="184f3-120">Bármely érték biztosíthat ehhez az elemhez.</span><span class="sxs-lookup"><span data-stu-id="184f3-120">You can provide any value for this element.</span></span> <span data-ttu-id="184f3-121">A sablon eszközzel való telepítésekor ez az érték segítségével győződjön meg arról, hogy a megfelelő sablon használatban van-e.</span><span class="sxs-lookup"><span data-stu-id="184f3-121">When deploying resources using the template, this value can be used to make sure that the right template is being used.</span></span> |
| <span data-ttu-id="184f3-122">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="184f3-122">parameters</span></span> |<span data-ttu-id="184f3-123">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-123">No</span></span> |<span data-ttu-id="184f3-124">Amikor központi telepítés végrehajtása testre szabhatja az erőforrások telepítése által biztosított értéket.</span><span class="sxs-lookup"><span data-stu-id="184f3-124">Values that are provided when deployment is executed to customize resource deployment.</span></span> |
| <span data-ttu-id="184f3-125">változók</span><span class="sxs-lookup"><span data-stu-id="184f3-125">variables</span></span> |<span data-ttu-id="184f3-126">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-126">No</span></span> |<span data-ttu-id="184f3-127">A sablon JSON-töredék, egyszerűbbé teheti a sablonnyelvi kifejezéseket használt értékek.</span><span class="sxs-lookup"><span data-stu-id="184f3-127">Values that are used as JSON fragments in the template to simplify template language expressions.</span></span> |
| <span data-ttu-id="184f3-128">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="184f3-128">resources</span></span> |<span data-ttu-id="184f3-129">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-129">Yes</span></span> |<span data-ttu-id="184f3-130">Telepített vagy frissített erőforráscsoportban erőforrástípusok esetében.</span><span class="sxs-lookup"><span data-stu-id="184f3-130">Resource types that are deployed or updated in a resource group.</span></span> |
| <span data-ttu-id="184f3-131">kimenetek</span><span class="sxs-lookup"><span data-stu-id="184f3-131">outputs</span></span> |<span data-ttu-id="184f3-132">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-132">No</span></span> |<span data-ttu-id="184f3-133">A telepítés utáni visszaadott értékek.</span><span class="sxs-lookup"><span data-stu-id="184f3-133">Values that are returned after deployment.</span></span> |

<span data-ttu-id="184f3-134">Minden elem beállítható tulajdonságokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="184f3-134">Each element contains properties you can set.</span></span> <span data-ttu-id="184f3-135">A következő példa a teljes szintaxissal sablon tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="184f3-135">The following example contains the full syntax for a template:</span></span>

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
                "description": "<description-of-the parameter>" 
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

<span data-ttu-id="184f3-136">Azt vizsgálja meg a sablon a témakör későbbi részében részletesebben részeit.</span><span class="sxs-lookup"><span data-stu-id="184f3-136">We examine the sections of the template in greater detail later in this topic.</span></span>

## <a name="expressions-and-functions"></a><span data-ttu-id="184f3-137">Kifejezések és funkciók</span><span class="sxs-lookup"><span data-stu-id="184f3-137">Expressions and functions</span></span>
<span data-ttu-id="184f3-138">A sablon alapvető szintaxisa a JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="184f3-138">The basic syntax of the template is JSON.</span></span> <span data-ttu-id="184f3-139">Azonban kifejezések és a funkciók bővíthetik a JSON a sablonon belül elérhető értékek.</span><span class="sxs-lookup"><span data-stu-id="184f3-139">However, expressions and functions extend the JSON values available within the template.</span></span>  <span data-ttu-id="184f3-140">Kifejezések íródtak belül JSON szövegkonstansok amelynek első és utolsó karaktere a szögletes: `[` és `]`, illetve.</span><span class="sxs-lookup"><span data-stu-id="184f3-140">Expressions are written within JSON string literals whose first and last characters are the brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="184f3-141">A kifejezés értéke legyen kiértékelve a sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="184f3-141">The value of the expression is evaluated when the template is deployed.</span></span> <span data-ttu-id="184f3-142">Megírva egy szöveges karakterlánc, amíg értékeli a kifejezés eredménye lehet eltérő típusú JSON, például a tömb vagy egész, attól függően, hogy a tényleges kifejezést.</span><span class="sxs-lookup"><span data-stu-id="184f3-142">While written as a string literal, the result of evaluating the expression can be of a different JSON type, such as an array or integer, depending on the actual expression.</span></span>  <span data-ttu-id="184f3-143">Indítsa el a zárójel szövegkonstansnak rendelkeznie `[`, de nem rendelkezik azt kifejezésként értelmezi, adja hozzá a karakterlánc elindítani egy extra zárójel `[[`.</span><span class="sxs-lookup"><span data-stu-id="184f3-143">To have a literal string start with a bracket `[`, but not have it interpreted as an expression, add an extra bracket to start the string with `[[`.</span></span>

<span data-ttu-id="184f3-144">Általában használ kifejezések funkciók konfigurálása a központi telepítési műveleteinek elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="184f3-144">Typically, you use expressions with functions to perform operations for configuring the deployment.</span></span> <span data-ttu-id="184f3-145">Csak a például a JavaScript, függvényhívások-ként formázott `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="184f3-145">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="184f3-146">A pont és a [index] operátorok használatával tulajdonságok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="184f3-146">You reference properties by using the dot and [index] operators.</span></span>

<span data-ttu-id="184f3-147">A következő példa bemutatja, hogyan hozhat létre értékek több funkciók használandó:</span><span class="sxs-lookup"><span data-stu-id="184f3-147">The following example shows how to use several functions when constructing values:</span></span>

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

<span data-ttu-id="184f3-148">Sablon funkciók teljes listáját lásd: [Azure Resource Manager sablonfüggvényei](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-148">For the full list of template functions, see [Azure Resource Manager template functions](resource-group-template-functions.md).</span></span> 

## <a name="parameters"></a><span data-ttu-id="184f3-149">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="184f3-149">Parameters</span></span>
<span data-ttu-id="184f3-150">A sablon a Paraméterek szakaszban adja meg az erőforrások telepítése során is bemeneti értékeket.</span><span class="sxs-lookup"><span data-stu-id="184f3-150">In the parameters section of the template, you specify which values you can input when deploying the resources.</span></span> <span data-ttu-id="184f3-151">A paraméterértékek lehetővé teszik a központi telepítés testreszabása egy adott környezetben (például a fejlesztői, tesztelési és éles) is lefednek értékek megadásával.</span><span class="sxs-lookup"><span data-stu-id="184f3-151">These parameter values enable you to customize the deployment by providing values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="184f3-152">A sablon a paraméterek megadása nem szükséges, de a paraméterek nélkül a sablon mindig központilag ugyanazon a nevét, helyét és tulajdonságok ugyanazokat az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="184f3-152">You do not have to provide parameters in your template, but without parameters your template would always deploy the same resources with the same names, locations, and properties.</span></span>

<span data-ttu-id="184f3-153">Megadhatja az alábbi szerkezettel paramétereket:</span><span class="sxs-lookup"><span data-stu-id="184f3-153">You define parameters with the following structure:</span></span>

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
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| <span data-ttu-id="184f3-154">Elem neve</span><span class="sxs-lookup"><span data-stu-id="184f3-154">Element name</span></span> | <span data-ttu-id="184f3-155">Szükséges</span><span class="sxs-lookup"><span data-stu-id="184f3-155">Required</span></span> | <span data-ttu-id="184f3-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="184f3-156">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="184f3-157">parameterName</span><span class="sxs-lookup"><span data-stu-id="184f3-157">parameterName</span></span> |<span data-ttu-id="184f3-158">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-158">Yes</span></span> |<span data-ttu-id="184f3-159">A paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="184f3-159">Name of the parameter.</span></span> <span data-ttu-id="184f3-160">Egy érvényes JavaScript-azonosítónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="184f3-160">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="184f3-161">type</span><span class="sxs-lookup"><span data-stu-id="184f3-161">type</span></span> |<span data-ttu-id="184f3-162">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-162">Yes</span></span> |<span data-ttu-id="184f3-163">A paraméter értékének típusa.</span><span class="sxs-lookup"><span data-stu-id="184f3-163">Type of the parameter value.</span></span> <span data-ttu-id="184f3-164">A táblázat után engedélyezett típusokkal listája látható.</span><span class="sxs-lookup"><span data-stu-id="184f3-164">See the list of allowed types after this table.</span></span> |
| <span data-ttu-id="184f3-165">DefaultValue érték</span><span class="sxs-lookup"><span data-stu-id="184f3-165">defaultValue</span></span> |<span data-ttu-id="184f3-166">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-166">No</span></span> |<span data-ttu-id="184f3-167">A paraméternek, ha nincs érték megadva, a paraméter alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="184f3-167">Default value for the parameter, if no value is provided for the parameter.</span></span> |
| <span data-ttu-id="184f3-168">Storageaccount_accounttype</span><span class="sxs-lookup"><span data-stu-id="184f3-168">allowedValues</span></span> |<span data-ttu-id="184f3-169">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-169">No</span></span> |<span data-ttu-id="184f3-170">Győződjön meg arról, hogy a megfelelő érték van megadva a paraméter megengedett értékei tömbje.</span><span class="sxs-lookup"><span data-stu-id="184f3-170">Array of allowed values for the parameter to make sure that the right value is provided.</span></span> |
| <span data-ttu-id="184f3-171">A MinValue</span><span class="sxs-lookup"><span data-stu-id="184f3-171">minValue</span></span> |<span data-ttu-id="184f3-172">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-172">No</span></span> |<span data-ttu-id="184f3-173">A minimális int típusú paraméterekhez, ez az érték értéke között lehet.</span><span class="sxs-lookup"><span data-stu-id="184f3-173">The minimum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="184f3-174">MaxValue</span><span class="sxs-lookup"><span data-stu-id="184f3-174">maxValue</span></span> |<span data-ttu-id="184f3-175">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-175">No</span></span> |<span data-ttu-id="184f3-176">A maximális int típusú paraméterekhez, ez az érték értéke között lehet.</span><span class="sxs-lookup"><span data-stu-id="184f3-176">The maximum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="184f3-177">a minLength</span><span class="sxs-lookup"><span data-stu-id="184f3-177">minLength</span></span> |<span data-ttu-id="184f3-178">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-178">No</span></span> |<span data-ttu-id="184f3-179">A minimális string, secureString és array típusú paraméterekhez, ez az érték hossza határokat is beleértve.</span><span class="sxs-lookup"><span data-stu-id="184f3-179">The minimum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="184f3-180">MaxLength</span><span class="sxs-lookup"><span data-stu-id="184f3-180">maxLength</span></span> |<span data-ttu-id="184f3-181">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-181">No</span></span> |<span data-ttu-id="184f3-182">A string, secureString és array típusú paraméterekhez, ez az érték hossza legfeljebb határokat is beleértve.</span><span class="sxs-lookup"><span data-stu-id="184f3-182">The maximum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="184f3-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="184f3-183">description</span></span> |<span data-ttu-id="184f3-184">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-184">No</span></span> |<span data-ttu-id="184f3-185">A paraméter, akkor jelenik meg, a felhasználók számára a portálon keresztül leírása.</span><span class="sxs-lookup"><span data-stu-id="184f3-185">Description of the parameter that is displayed to users through the portal.</span></span> |

<span data-ttu-id="184f3-186">Az engedélyezett típusokkal és az értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="184f3-186">The allowed types and values are:</span></span>

* <span data-ttu-id="184f3-187">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="184f3-187">**string**</span></span>
* <span data-ttu-id="184f3-188">**secureString**</span><span class="sxs-lookup"><span data-stu-id="184f3-188">**secureString**</span></span>
* <span data-ttu-id="184f3-189">**int**</span><span class="sxs-lookup"><span data-stu-id="184f3-189">**int**</span></span>
* <span data-ttu-id="184f3-190">**logikai érték**</span><span class="sxs-lookup"><span data-stu-id="184f3-190">**bool**</span></span>
* <span data-ttu-id="184f3-191">**objektum**</span><span class="sxs-lookup"><span data-stu-id="184f3-191">**object**</span></span> 
* <span data-ttu-id="184f3-192">**secureObject**</span><span class="sxs-lookup"><span data-stu-id="184f3-192">**secureObject**</span></span>
* <span data-ttu-id="184f3-193">**a tömb**</span><span class="sxs-lookup"><span data-stu-id="184f3-193">**array**</span></span>

<span data-ttu-id="184f3-194">Nem kötelező paraméter adja meg, adja meg a DefaultValue érték (üres is lehet).</span><span class="sxs-lookup"><span data-stu-id="184f3-194">To specify a parameter as optional, provide a defaultValue (can be an empty string).</span></span> 

<span data-ttu-id="184f3-195">A paraméter neve a sablon, amely megfelel a paramétert a parancsba a sablon telepítéséhez ad meg, ha nincs megadta kapcsolatos lehetséges egyértelműek.</span><span class="sxs-lookup"><span data-stu-id="184f3-195">If you specify a parameter name in your template that matches a parameter in the command to deploy the template, there is potential ambiguity about the values you provide.</span></span> <span data-ttu-id="184f3-196">Erőforrás-kezelő Ez zavart megszünteti a utótag hozzáadásával **FromTemplate** a sablon paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="184f3-196">Resource Manager resolves this confusion by adding the postfix **FromTemplate** to the template parameter.</span></span> <span data-ttu-id="184f3-197">Például, ha nevű paraméter **ResourceGroupName** a sablonban ütközik a **ResourceGroupName** paramétere a [új-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="184f3-197">For example, if you include a parameter named **ResourceGroupName** in your template, it conflicts with the **ResourceGroupName** parameter in the [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="184f3-198">A telepítés során kéri adjon meg egy értéket a **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="184f3-198">During deployment, you are prompted to provide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="184f3-199">Ez zavart ne általában a nem paraméterek telepítési műveleteihez használt paraméterek megegyező névvel.</span><span class="sxs-lookup"><span data-stu-id="184f3-199">In general, you should avoid this confusion by not naming parameters with the same name as parameters used for deployment operations.</span></span>

> [!NOTE]
> <span data-ttu-id="184f3-200">A jelszó, kulcsokat és más titkos adatokat kell használni a **secureString** típusa.</span><span class="sxs-lookup"><span data-stu-id="184f3-200">All passwords, keys, and other secrets should use the **secureString** type.</span></span> <span data-ttu-id="184f3-201">A JSON-objektumból átadni a bizalmas adatokat, ha a **secureObject** típusa.</span><span class="sxs-lookup"><span data-stu-id="184f3-201">If you pass sensitive data in a JSON object, use the **secureObject** type.</span></span> <span data-ttu-id="184f3-202">Sablonparaméterek secureString vagy secureObject típusú erőforrások telepítése után nem lehet olvasni.</span><span class="sxs-lookup"><span data-stu-id="184f3-202">Template parameters with secureString or secureObject types cannot be read after resource deployment.</span></span> 
> 
> <span data-ttu-id="184f3-203">Az üzembe helyezési előzményeket a következő bejegyzést például egy karakterláncot és objektumot, de nem a secureString és secureObject értékét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="184f3-203">For example, the following entry in the deployment history shows the value for a string and object but not for secureString and secureObject.</span></span>
>
> ![központi telepítés értékek megjelenítése](./media/resource-group-authoring-templates/show-parameters.png)  
>

<span data-ttu-id="184f3-205">A következő példa bemutatja, hogyan paraméterek megadásához:</span><span class="sxs-lookup"><span data-stu-id="184f3-205">The following example shows how to define parameters:</span></span>

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

<span data-ttu-id="184f3-206">A telepítés során az értékek a bemeneti tudnivalókat [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-206">For how to input the parameter values during deployment, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span> 

## <a name="variables"></a><span data-ttu-id="184f3-207">Változók</span><span class="sxs-lookup"><span data-stu-id="184f3-207">Variables</span></span>
<span data-ttu-id="184f3-208">A változók szakaszban, a sablon egész érték, amely használható hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="184f3-208">In the variables section, you construct values that can be used throughout your template.</span></span> <span data-ttu-id="184f3-209">Nem kell meghatároznia a változót, de azok gyakran leegyszerűsítheti a sablon csökkentésével összetett kifejezések.</span><span class="sxs-lookup"><span data-stu-id="184f3-209">You do not need to define variables, but they often simplify your template by reducing complex expressions.</span></span>

<span data-ttu-id="184f3-210">Megadhatja a változókat és az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="184f3-210">You define variables with the following structure:</span></span>

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

<span data-ttu-id="184f3-211">A következő példa bemutatja, hogyan adhat meg egy változót összeállított paraméter két érték közül:</span><span class="sxs-lookup"><span data-stu-id="184f3-211">The following example shows how to define a variable that is constructed from two parameter values:</span></span>

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

<span data-ttu-id="184f3-212">A következő példa bemutatja egy változó, amely egy összetett JSON-típus, és a változókat, amelyek más változók össze:</span><span class="sxs-lookup"><span data-stu-id="184f3-212">The next example shows a variable that is a complex JSON type, and variables that are constructed from other variables:</span></span>

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

## <a name="resources"></a><span data-ttu-id="184f3-213">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="184f3-213">Resources</span></span>
<span data-ttu-id="184f3-214">Erőforrások területen adja meg az erőforrások telepítése vagy frissítése.</span><span class="sxs-lookup"><span data-stu-id="184f3-214">In the resources section, you define the resources that are deployed or updated.</span></span> <span data-ttu-id="184f3-215">Ez a szakasz kérheti le bonyolult, mert ismernie kell a típusok esetében helyez üzembe adja meg a megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="184f3-215">This section can get complicated because you must understand the types you are deploying to provide the right values.</span></span> <span data-ttu-id="184f3-216">Az erőforrás-specifikus értékeket (apiVersion, típusa és tulajdonságait) meg kell adnia, lásd: [meghatározhatja az erőforrásokat az Azure Resource Manager sablonokban](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="184f3-216">For the resource-specific values (apiVersion, type, and properties) that you need to set, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span> 

<span data-ttu-id="184f3-217">Meghatározhatja az erőforrások az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="184f3-217">You define resources with the following structure:</span></span>

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

| <span data-ttu-id="184f3-218">Elem neve</span><span class="sxs-lookup"><span data-stu-id="184f3-218">Element name</span></span> | <span data-ttu-id="184f3-219">Szükséges</span><span class="sxs-lookup"><span data-stu-id="184f3-219">Required</span></span> | <span data-ttu-id="184f3-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="184f3-220">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="184f3-221">Az állapot</span><span class="sxs-lookup"><span data-stu-id="184f3-221">condition</span></span> | <span data-ttu-id="184f3-222">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-222">No</span></span> | <span data-ttu-id="184f3-223">Logikai érték, amely azt jelzi, hogy telepítve van-e az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="184f3-223">Boolean value that indicates whether the resource is deployed.</span></span> |
| <span data-ttu-id="184f3-224">apiVersion</span><span class="sxs-lookup"><span data-stu-id="184f3-224">apiVersion</span></span> |<span data-ttu-id="184f3-225">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-225">Yes</span></span> |<span data-ttu-id="184f3-226">Az erőforrás létrehozásához használt a REST API verziója.</span><span class="sxs-lookup"><span data-stu-id="184f3-226">Version of the REST API to use for creating the resource.</span></span> |
| <span data-ttu-id="184f3-227">type</span><span class="sxs-lookup"><span data-stu-id="184f3-227">type</span></span> |<span data-ttu-id="184f3-228">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-228">Yes</span></span> |<span data-ttu-id="184f3-229">Az erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="184f3-229">Type of the resource.</span></span> <span data-ttu-id="184f3-230">Ezt az értéket az erőforrás-szolgáltató és az erőforrástípus névtere kombinációja (például **Microsoft.Storage/storageAccounts**).</span><span class="sxs-lookup"><span data-stu-id="184f3-230">This value is a combination of the namespace of the resource provider and the resource type (such as **Microsoft.Storage/storageAccounts**).</span></span> |
| <span data-ttu-id="184f3-231">név</span><span class="sxs-lookup"><span data-stu-id="184f3-231">name</span></span> |<span data-ttu-id="184f3-232">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-232">Yes</span></span> |<span data-ttu-id="184f3-233">Az erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="184f3-233">Name of the resource.</span></span> <span data-ttu-id="184f3-234">A név URI összetevő korlátozások RFC3986 definiált kell követnie.</span><span class="sxs-lookup"><span data-stu-id="184f3-234">The name must follow URI component restrictions defined in RFC3986.</span></span> <span data-ttu-id="184f3-235">Emellett Azure-szolgáltatások elérhetővé tenni az erőforrásnév kívül felek ellenőrzése a nevét, győződjön meg arról, hogy nincs egy másik identitás hamisításának kísérlet.</span><span class="sxs-lookup"><span data-stu-id="184f3-235">In addition, Azure services that expose the resource name to outside parties validate the name to make sure it is not an attempt to spoof another identity.</span></span> |
| <span data-ttu-id="184f3-236">location</span><span class="sxs-lookup"><span data-stu-id="184f3-236">location</span></span> |<span data-ttu-id="184f3-237">Változó</span><span class="sxs-lookup"><span data-stu-id="184f3-237">Varies</span></span> |<span data-ttu-id="184f3-238">Támogatja a megadott erőforráscsoport földrajzi elhelyezkedését.</span><span class="sxs-lookup"><span data-stu-id="184f3-238">Supported geo-locations of the provided resource.</span></span> <span data-ttu-id="184f3-239">Kiválaszthatja a rendelkezésre álló helyeken, de általában érdemes válasszon egyet, amelynek mérete megközelítőleg a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="184f3-239">You can select any of the available locations, but typically it makes sense to pick one that is close to your users.</span></span> <span data-ttu-id="184f3-240">Általában is érdemes helyezendő erőforrásokat, amelyek ugyanabban a régióban kapcsolatba egymással.</span><span class="sxs-lookup"><span data-stu-id="184f3-240">Usually, it also makes sense to place resources that interact with each other in the same region.</span></span> <span data-ttu-id="184f3-241">A legtöbb erőforrás szükséges egy helyre, de néhány típust (például egy szerepkör-hozzárendelés) igényel egy helyre.</span><span class="sxs-lookup"><span data-stu-id="184f3-241">Most resource types require a location, but some types (such as a role assignment) do not require a location.</span></span> <span data-ttu-id="184f3-242">Lásd: [erőforrás hely beállítása az Azure Resource Manager sablonokban](resource-manager-template-location.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-242">See [Set resource location in Azure Resource Manager templates](resource-manager-template-location.md).</span></span> |
| <span data-ttu-id="184f3-243">tags</span><span class="sxs-lookup"><span data-stu-id="184f3-243">tags</span></span> |<span data-ttu-id="184f3-244">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-244">No</span></span> |<span data-ttu-id="184f3-245">Az erőforrás társított címkékkel.</span><span class="sxs-lookup"><span data-stu-id="184f3-245">Tags that are associated with the resource.</span></span> <span data-ttu-id="184f3-246">Lásd: [címkével olyan erőforrásokat az Azure Resource Manager sablonokban](resource-manager-template-tags.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-246">See [Tag resources in Azure Resource Manager templates](resource-manager-template-tags.md).</span></span> |
| <span data-ttu-id="184f3-247">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="184f3-247">comments</span></span> |<span data-ttu-id="184f3-248">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-248">No</span></span> |<span data-ttu-id="184f3-249">A Megjegyzések a a sablonban lévő erőforrások dokumentálása</span><span class="sxs-lookup"><span data-stu-id="184f3-249">Your notes for documenting the resources in your template</span></span> |
| <span data-ttu-id="184f3-250">Másolás</span><span class="sxs-lookup"><span data-stu-id="184f3-250">copy</span></span> |<span data-ttu-id="184f3-251">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-251">No</span></span> |<span data-ttu-id="184f3-252">Ha egynél több példány van szükség, az olyan erőforrások száma létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="184f3-252">If more than one instance is needed, the number of resources to create.</span></span> <span data-ttu-id="184f3-253">Az alapértelmezett mód párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="184f3-253">The default mode is parallel.</span></span> <span data-ttu-id="184f3-254">Adja meg a soros módban, ha nem szeretné, hogy az összes vagy egy időben üzembe helyezendő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="184f3-254">Specify serial mode when you do not want all or the resources to deploy at the same time.</span></span> <span data-ttu-id="184f3-255">További információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-255">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="184f3-256">dependsOn</span><span class="sxs-lookup"><span data-stu-id="184f3-256">dependsOn</span></span> |<span data-ttu-id="184f3-257">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-257">No</span></span> |<span data-ttu-id="184f3-258">Ehhez az erőforráshoz központi telepítése előtt telepíteni kell erőforrások.</span><span class="sxs-lookup"><span data-stu-id="184f3-258">Resources that must be deployed before this resource is deployed.</span></span> <span data-ttu-id="184f3-259">Erőforrás-kezelő kiértékeli az erőforrások közti függőségeket, és telepíti azokat a megfelelő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="184f3-259">Resource Manager evaluates the dependencies between resources and deploys them in the correct order.</span></span> <span data-ttu-id="184f3-260">Ha nincsenek függő erőforrások, párhuzamos központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="184f3-260">When resources are not dependent on each other, they are deployed in parallel.</span></span> <span data-ttu-id="184f3-261">Az érték lehet egy vesszővel elválasztott lista erőforrás nevét vagy egyedi erőforrás-azonosítók.</span><span class="sxs-lookup"><span data-stu-id="184f3-261">The value can be a comma-separated list of a resource names or resource unique identifiers.</span></span> <span data-ttu-id="184f3-262">Ez a sablon üzembe helyezett erőforrások csak felsorolása</span><span class="sxs-lookup"><span data-stu-id="184f3-262">Only list resources that are deployed in this template.</span></span> <span data-ttu-id="184f3-263">Erőforrások, amelyek nincsenek meghatározva a sablonban már léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="184f3-263">Resources that are not defined in this template must already exist.</span></span> <span data-ttu-id="184f3-264">Kerülje a szükségtelen függőségek hozzáadásával még a központi telepítés lassú, és hozzon létre körkörös függőségi viszony.</span><span class="sxs-lookup"><span data-stu-id="184f3-264">Avoid adding unnecessary dependencies as they can slow your deployment and create circular dependencies.</span></span> <span data-ttu-id="184f3-265">A beállítás függőségek útmutatást lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-265">For guidance on setting dependencies, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md).</span></span> |
| <span data-ttu-id="184f3-266">properties</span><span class="sxs-lookup"><span data-stu-id="184f3-266">properties</span></span> |<span data-ttu-id="184f3-267">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-267">No</span></span> |<span data-ttu-id="184f3-268">Erőforrás-specifikus konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="184f3-268">Resource-specific configuration settings.</span></span> <span data-ttu-id="184f3-269">A tulajdonságok értékeit ugyanazok, mint a REST API művelet (PUT metódust) létrehozni az erőforrást a kérés törzsében meg az értékeket.</span><span class="sxs-lookup"><span data-stu-id="184f3-269">The values for the properties are the same as the values you provide in the request body for the REST API operation (PUT method) to create the resource.</span></span> <span data-ttu-id="184f3-270">Egy tulajdonság több példányát létrehozni egy másolatot tömb is megadható.</span><span class="sxs-lookup"><span data-stu-id="184f3-270">You can also specify a copy array to create multiple instances of a property.</span></span> <span data-ttu-id="184f3-271">További információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-271">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="184f3-272">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="184f3-272">resources</span></span> |<span data-ttu-id="184f3-273">Nem</span><span class="sxs-lookup"><span data-stu-id="184f3-273">No</span></span> |<span data-ttu-id="184f3-274">A múltbeli erőforrástól függő gyermekszintű erőforrása.</span><span class="sxs-lookup"><span data-stu-id="184f3-274">Child resources that depend on the resource being defined.</span></span> <span data-ttu-id="184f3-275">Csak olyan típusú erőforrások a szülő erőforrás sémája által számukra engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="184f3-275">Only provide resource types that are permitted by the schema of the parent resource.</span></span> <span data-ttu-id="184f3-276">A gyermek-erőforrás teljesen minősített típusú tartalmaz szülő erőforrástípusra, például **Microsoft.Web/sites/extensions**.</span><span class="sxs-lookup"><span data-stu-id="184f3-276">The fully qualified type of the child resource includes the parent resource type, such as **Microsoft.Web/sites/extensions**.</span></span> <span data-ttu-id="184f3-277">A szülő erőforrás függőség nem utal.</span><span class="sxs-lookup"><span data-stu-id="184f3-277">Dependency on the parent resource is not implied.</span></span> <span data-ttu-id="184f3-278">Függőséget explicit módon meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="184f3-278">You must explicitly define that dependency.</span></span> |

<span data-ttu-id="184f3-279">A források szakaszában üzembe helyezendő erőforrásokat tömbjét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="184f3-279">The resources section contains an array of the resources to deploy.</span></span> <span data-ttu-id="184f3-280">Az egyes erőforrások belül is meghatározhat gyermekerőforrásait tömbjét.</span><span class="sxs-lookup"><span data-stu-id="184f3-280">Within each resource, you can also define an array of child resources.</span></span> <span data-ttu-id="184f3-281">Ezért a források szakaszában eredményezhet. a struktúra, például:</span><span class="sxs-lookup"><span data-stu-id="184f3-281">Therefore, your resources section could have a structure like:</span></span>

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

<span data-ttu-id="184f3-282">Gyermek erőforrások meghatározása kapcsolatos további információkért lásd: [Resource Manager-sablon nevét és típusát gyermek erőforrás beállított](resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-282">For more information about defining child resources, see [Set name and type for child resource in Resource Manager template](resource-manager-template-child-resource.md).</span></span>

<span data-ttu-id="184f3-283">A **feltétel** elem határozza meg, hogy telepítve van-e az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="184f3-283">The **condition** element specifies whether the resource is deployed.</span></span> <span data-ttu-id="184f3-284">Ez az elem értéke IGAZ vagy hamis oldja fel.</span><span class="sxs-lookup"><span data-stu-id="184f3-284">The value for this element resolves to true or false.</span></span> <span data-ttu-id="184f3-285">Például adja meg, hogy telepítve van-e a egy új tárfiókot, használja:</span><span class="sxs-lookup"><span data-stu-id="184f3-285">For example, to specify whether a new storage account is deployed, use:</span></span>

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

<span data-ttu-id="184f3-286">Például egy új vagy meglévő erőforrást használ, tekintse meg a [új vagy meglévő feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="184f3-286">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="184f3-287">Adja meg, hogy egy virtuális gép telepítve van-e a jelszó vagy SSH-kulcsot, adja meg a virtuális gép két verziója a sablonban és a használata **feltétel** használati megkülönböztetéséhez.</span><span class="sxs-lookup"><span data-stu-id="184f3-287">To specify whether a virtual machine is deployed with a password or SSH key, define two versions of the virtual machine in your template and use **condition** to differentiate usage.</span></span> <span data-ttu-id="184f3-288">Egy paraméter meghatározza, hogy mely forgatókönyv telepítését továbbítani.</span><span class="sxs-lookup"><span data-stu-id="184f3-288">Pass a parameter that specifies which scenario to deploy.</span></span>

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

<span data-ttu-id="184f3-289">Például egy jelszó vagy SSH-kulcs használatával a virtuális gép telepítése, lásd: [felhasználónév és SSH feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="184f3-289">For an example of using a password or SSH key to deploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="outputs"></a><span data-ttu-id="184f3-290">kimenetek</span><span class="sxs-lookup"><span data-stu-id="184f3-290">Outputs</span></span>
<span data-ttu-id="184f3-291">A kimenetek szakaszban adja meg központi telepítéséből a visszaadott érték.</span><span class="sxs-lookup"><span data-stu-id="184f3-291">In the Outputs section, you specify values that are returned from deployment.</span></span> <span data-ttu-id="184f3-292">Visszaadhatja például az URI telepített erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="184f3-292">For example, you could return the URI to access a deployed resource.</span></span>

<span data-ttu-id="184f3-293">A következő példa egy kimeneti definíciója szerkezetét mutatja:</span><span class="sxs-lookup"><span data-stu-id="184f3-293">The following example shows the structure of an output definition:</span></span>

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| <span data-ttu-id="184f3-294">Elem neve</span><span class="sxs-lookup"><span data-stu-id="184f3-294">Element name</span></span> | <span data-ttu-id="184f3-295">Szükséges</span><span class="sxs-lookup"><span data-stu-id="184f3-295">Required</span></span> | <span data-ttu-id="184f3-296">Leírás</span><span class="sxs-lookup"><span data-stu-id="184f3-296">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="184f3-297">outputName</span><span class="sxs-lookup"><span data-stu-id="184f3-297">outputName</span></span> |<span data-ttu-id="184f3-298">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-298">Yes</span></span> |<span data-ttu-id="184f3-299">A kimeneti érték nevét.</span><span class="sxs-lookup"><span data-stu-id="184f3-299">Name of the output value.</span></span> <span data-ttu-id="184f3-300">Egy érvényes JavaScript-azonosítónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="184f3-300">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="184f3-301">type</span><span class="sxs-lookup"><span data-stu-id="184f3-301">type</span></span> |<span data-ttu-id="184f3-302">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-302">Yes</span></span> |<span data-ttu-id="184f3-303">A kimeneti érték típusa.</span><span class="sxs-lookup"><span data-stu-id="184f3-303">Type of the output value.</span></span> <span data-ttu-id="184f3-304">Kimeneti értékek támogatásához sablon bemeneti paraméterként.</span><span class="sxs-lookup"><span data-stu-id="184f3-304">Output values support the same types as template input parameters.</span></span> |
| <span data-ttu-id="184f3-305">érték</span><span class="sxs-lookup"><span data-stu-id="184f3-305">value</span></span> |<span data-ttu-id="184f3-306">Igen</span><span class="sxs-lookup"><span data-stu-id="184f3-306">Yes</span></span> |<span data-ttu-id="184f3-307">Amely értékeli ki, és kimeneti értéket adja vissza a sablonnyelvi kifejezés.</span><span class="sxs-lookup"><span data-stu-id="184f3-307">Template language expression that is evaluated and returned as output value.</span></span> |

<span data-ttu-id="184f3-308">A következő példa bemutatja a kimenetek szakaszban visszaadott érték.</span><span class="sxs-lookup"><span data-stu-id="184f3-308">The following example shows a value that is returned in the Outputs section.</span></span>

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

<span data-ttu-id="184f3-309">A kimeneti használatáról további információk: [állapotát az Azure Resource Manager sablonokban megosztása](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-309">For more information about working with output, see [Sharing state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="template-limits"></a><span data-ttu-id="184f3-310">Sablon korlátok</span><span class="sxs-lookup"><span data-stu-id="184f3-310">Template limits</span></span>

<span data-ttu-id="184f3-311">A sablon 1 MB-nál, és minden paraméterfájl 64 KB méretének korlátozására.</span><span class="sxs-lookup"><span data-stu-id="184f3-311">Limit the size of your template to 1 MB, and each parameter file to 64 KB.</span></span> <span data-ttu-id="184f3-312">1 MB-os korlát vonatkozik a sablon a végső állapot után kiterjedt ismétlődő erőforrás-definíciókban és változók és a paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="184f3-312">The 1-MB limit applies to the final state of the template after it has been expanded with iterative resource definitions, and values for variables and parameters.</span></span> 

<span data-ttu-id="184f3-313">Emellett korlátozva:</span><span class="sxs-lookup"><span data-stu-id="184f3-313">You are also limited to:</span></span>

* <span data-ttu-id="184f3-314">256 paraméterek</span><span class="sxs-lookup"><span data-stu-id="184f3-314">256 parameters</span></span>
* <span data-ttu-id="184f3-315">256 változók</span><span class="sxs-lookup"><span data-stu-id="184f3-315">256 variables</span></span>
* <span data-ttu-id="184f3-316">800 erőforrások (például a példányszám)</span><span class="sxs-lookup"><span data-stu-id="184f3-316">800 resources (including copy count)</span></span>
* <span data-ttu-id="184f3-317">64 kimeneti értékek</span><span class="sxs-lookup"><span data-stu-id="184f3-317">64 output values</span></span>
* <span data-ttu-id="184f3-318">egy sablon kifejezés 24,576 karakter</span><span class="sxs-lookup"><span data-stu-id="184f3-318">24,576 characters in a template expression</span></span>

<span data-ttu-id="184f3-319">Néhány sablon korlátot azért lépheti túl a beágyazott sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="184f3-319">You can exceed some template limits by using a nested template.</span></span> <span data-ttu-id="184f3-320">További információkért lásd: [kapcsolt sablonok használata az Azure-erőforrások telepítésekor](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-320">For more information, see [Using linked templates when deploying Azure resources](resource-group-linked-templates.md).</span></span> <span data-ttu-id="184f3-321">A paraméterek, változók vagy kimenetek számának csökkentése érdekében kombinálható egy objektum több értéket.</span><span class="sxs-lookup"><span data-stu-id="184f3-321">To reduce the number of parameters, variables, or outputs, you can combine several values into an object.</span></span> <span data-ttu-id="184f3-322">További információkért lásd: [paraméterekként objektumok](resource-manager-objects-as-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-322">For more information, see [Objects as parameters](resource-manager-objects-as-parameters.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="184f3-323">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="184f3-323">Next steps</span></span>
* <span data-ttu-id="184f3-324">A különböző megoldástípusokhoz használható teljes sablonok megtekintéséhez lásd: [Azure gyorsindítási sablonok](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="184f3-324">To view complete templates for many different types of solutions, see the [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
* <span data-ttu-id="184f3-325">A sablonon belül használhatja a functions szolgáltatással kapcsolatos részletekért lásd: [Azure Resource Manager Sablonfüggvényei](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-325">For details about the functions you can use from within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md).</span></span>
* <span data-ttu-id="184f3-326">Egyesítenie több sablon üzembe helyezése során, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="184f3-326">To combine multiple templates during deployment, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="184f3-327">Szükség lehet egy másik erőforráscsoportban található erőforrások használatával.</span><span class="sxs-lookup"><span data-stu-id="184f3-327">You may need to use resources that exist within a different resource group.</span></span> <span data-ttu-id="184f3-328">Ez a forgatókönyv nem közös, ha a storage-fiókok vagy több erőforrás csoporttal megosztott virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="184f3-328">This scenario is common when working with storage accounts or virtual networks that are shared across multiple resource groups.</span></span> <span data-ttu-id="184f3-329">További információkért lásd: a [resourceId függvény](resource-group-template-functions-resource.md#resourceid).</span><span class="sxs-lookup"><span data-stu-id="184f3-329">For more information, see the [resourceId function](resource-group-template-functions-resource.md#resourceid).</span></span>
