---
title: a JSON - Azure Logic Apps aaaDefine munkafolyamatok |} Microsoft Docs
description: "Hogyan toowrite munkafolyamat-definícióhoz a JSON-ban a logic Apps alkalmazások"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 0d69d334ecee9c3e7f8684cfde68ef0e85280358
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a><span data-ttu-id="529e7-103">A logic apps segítségével JSON munkafolyamat-meghatározások létrehozása</span><span class="sxs-lookup"><span data-stu-id="529e7-103">Create workflow definitions for logic apps using JSON</span></span>

<span data-ttu-id="529e7-104">A munkafolyamat-definícióhoz hozhat létre [Azure Logic Apps](logic-apps-what-are-logic-apps.md) egyszerű, deklaratív JSON nyelv.</span><span class="sxs-lookup"><span data-stu-id="529e7-104">You can create workflow definitions for [Azure Logic Apps](logic-apps-what-are-logic-apps.md) with simple, declarative JSON language.</span></span> <span data-ttu-id="529e7-105">Ha még nem tette meg, olvassa el [hogyan toocreate az első logikai alkalmazást a Logic App tervezővel](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="529e7-105">If you haven't already, first review [how toocreate your first logic app with Logic App Designer](logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="529e7-106">Lásd még hello [útmutató hello Munkafolyamatdefiníciós nyelve a teljes](http://aka.ms/logicappsdocs).</span><span class="sxs-lookup"><span data-stu-id="529e7-106">Also, see hello [full reference for hello Workflow Definition Language](http://aka.ms/logicappsdocs).</span></span>

## <a name="repeat-steps-over-a-list"></a><span data-ttu-id="529e7-107">Ismételje meg a során</span><span class="sxs-lookup"><span data-stu-id="529e7-107">Repeat steps over a list</span></span>

<span data-ttu-id="529e7-108">olyan tömb, amely nem rendelkezik too10, 000 elemek fel és egy műveletet minden elemhez, hello használata révén tooiterate [foreach típus](logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="529e7-108">tooiterate through an array that has up too10,000 items and perform an action for each item, use hello [foreach type](logic-apps-loops-and-scopes.md).</span></span>

## <a name="handle-failures-if-something-goes-wrong"></a><span data-ttu-id="529e7-109">Kijavíthassa a hibákat, ha valamilyen hiba</span><span class="sxs-lookup"><span data-stu-id="529e7-109">Handle failures if something goes wrong</span></span>

<span data-ttu-id="529e7-110">Általában érdemes tooinclude egy *szervizelési lépés* – néhány logika, amely végrehajtja a *csak, ha* egy vagy több, a hívás sikertelen.</span><span class="sxs-lookup"><span data-stu-id="529e7-110">Usually, you want tooinclude a *remediation step* — some logic that executes *if and only if* one or more of your calls fail.</span></span> <span data-ttu-id="529e7-111">Ez a példa adatok lekérése a különböző helyek, de hello hívás sikertelen lesz, ha azt szeretnénk, egy üzenet tooPOST valahol, azt követheti nyomon, hogy hiba le később:</span><span class="sxs-lookup"><span data-stu-id="529e7-111">This example gets data from various places, but if hello call fails, we want tooPOST a message somewhere so we can track down that failure later:</span></span>  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="529e7-112">toospecify, amely `postToErrorMessageQueue` csak futtat `readData` rendelkezik `Failed`, hello használata `runAfter` tulajdonság, például toospecify a lehetséges értékek listáját, hogy `runAfter` lehet `["Succeeded", "Failed"]`.</span><span class="sxs-lookup"><span data-stu-id="529e7-112">toospecify that `postToErrorMessageQueue` only runs after `readData` has `Failed`, use hello `runAfter` property, for example, toospecify a list of possible values, so that `runAfter` could be `["Succeeded", "Failed"]`.</span></span>

<span data-ttu-id="529e7-113">Végül, ebben a példában a hello hiba most kezeli, mivel azt már nem megjelölni futtató hello `Failed`.</span><span class="sxs-lookup"><span data-stu-id="529e7-113">Finally, because this example now handles hello error, we no longer mark hello run as `Failed`.</span></span> <span data-ttu-id="529e7-114">Hozzáadott hello lépés kezelése ebben a példában ez a hiba, mert rendelkezik-e futtatni hello `Succeeded` bár egy lépésben `Failed`.</span><span class="sxs-lookup"><span data-stu-id="529e7-114">Because we added hello step for handling this failure in this example, hello run has `Succeeded` although one step `Failed`.</span></span>

## <a name="execute-two-or-more-steps-in-parallel"></a><span data-ttu-id="529e7-115">Két vagy több lépést végre párhuzamosan</span><span class="sxs-lookup"><span data-stu-id="529e7-115">Execute two or more steps in parallel</span></span>

<span data-ttu-id="529e7-116">toorun párhuzamosan több művelet hello `runAfter` tulajdonságot meg kell futásidőben.</span><span class="sxs-lookup"><span data-stu-id="529e7-116">toorun multiple actions in parallel, hello `runAfter` property must be equivalent at runtime.</span></span> 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="529e7-117">Ebben a példában is `branch1` és `branch2` toorun után vannak beállítva `readData`.</span><span class="sxs-lookup"><span data-stu-id="529e7-117">In this example, both `branch1` and `branch2` are set toorun after `readData`.</span></span> <span data-ttu-id="529e7-118">Ennek eredményeképpen a mindkét ágak párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="529e7-118">As a result, both branches run in parallel.</span></span> <span data-ttu-id="529e7-119">mindkét ágak hello időbélyegzőjét megegyezik.</span><span class="sxs-lookup"><span data-stu-id="529e7-119">hello timestamp for both branches is identical.</span></span>

![Párhuzamos](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a><span data-ttu-id="529e7-121">Csatlakozás két párhuzamos ág</span><span class="sxs-lookup"><span data-stu-id="529e7-121">Join two parallel branches</span></span>

<span data-ttu-id="529e7-122">Csatlakozhat a két elem toohello hozzáadásával toorun beállított párhuzamos műveletek `runAfter` tulajdonság hello előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="529e7-122">You can join two actions that are set toorun in parallel by adding items toohello `runAfter` property as in hello previous example.</span></span>

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Párhuzamos](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-tooa-different-configuration"></a><span data-ttu-id="529e7-124">Térkép elemek tooa különböző konfigurációjának felsorolása</span><span class="sxs-lookup"><span data-stu-id="529e7-124">Map list items tooa different configuration</span></span>

<span data-ttu-id="529e7-125">Ezt követően tegyük fel, hogy szeretnénk tooget eltérő tartalomra hello tulajdonság értéke alapján.</span><span class="sxs-lookup"><span data-stu-id="529e7-125">Next, let's say that we want tooget different content based on hello value of a property.</span></span> <span data-ttu-id="529e7-126">Paraméterként létrehozhatunk olyan értékek toodestinations térképére:</span><span class="sxs-lookup"><span data-stu-id="529e7-126">We can create a map of values toodestinations as a parameter:</span></span>  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

<span data-ttu-id="529e7-127">Ebben az esetben azt először kapnak a cikkek listáját.</span><span class="sxs-lookup"><span data-stu-id="529e7-127">In this case, we first get a list of articles.</span></span> <span data-ttu-id="529e7-128">A paraméterként megadott hello kategória alapján, hello második lépésben használ a térkép toolook hello URL-cím mentése hello tartalom beolvasása.</span><span class="sxs-lookup"><span data-stu-id="529e7-128">Based on hello category that was defined as a parameter, hello second step uses a map toolook up hello URL for getting hello content.</span></span>

<span data-ttu-id="529e7-129">Néhány eset toonote itt:</span><span class="sxs-lookup"><span data-stu-id="529e7-129">Some times toonote here:</span></span> 

*   <span data-ttu-id="529e7-130">Hello [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) függvény ellenőrzi, hogy hello kategória sem felel meg egyik ismert meghatározott kategóriákba hello.</span><span class="sxs-lookup"><span data-stu-id="529e7-130">hello [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) function checks whether hello category matches one of hello known defined categories.</span></span>

*   <span data-ttu-id="529e7-131">Után azt lekérése hello kategória, azt is lekéréses hello elem hello leképezés szögletes zárójelbe használatával:`parameters[...]`</span><span class="sxs-lookup"><span data-stu-id="529e7-131">After we get hello category, we can pull hello item from hello map using square brackets: `parameters[...]`</span></span>

## <a name="process-strings"></a><span data-ttu-id="529e7-132">Folyamat karakterláncok</span><span class="sxs-lookup"><span data-stu-id="529e7-132">Process strings</span></span>

<span data-ttu-id="529e7-133">Használhatja a különböző funkciók toomanipulate karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="529e7-133">You can use various functions toomanipulate strings.</span></span> <span data-ttu-id="529e7-134">Tegyük fel, hogy egy karakterlánc toopass tooa rendszer szeretnénk, de jelenleg nem megfelelő kezelését a karakteres kódolási kapcsolatos biztosnak kell.</span><span class="sxs-lookup"><span data-stu-id="529e7-134">For example, suppose we have a string that we want toopass tooa system, but we aren't confident about proper handling for character encoding.</span></span> <span data-ttu-id="529e7-135">Egy elem toobase64 Ez a karakterlánc kódolása.</span><span class="sxs-lookup"><span data-stu-id="529e7-135">One option is toobase64 encode this string.</span></span> <span data-ttu-id="529e7-136">Azonban egy URL-címben escape-karaktersorozat tooavoid fogjuk tooreplace néhány karaktert.</span><span class="sxs-lookup"><span data-stu-id="529e7-136">However, tooavoid escaping in a URL, we are going tooreplace a few characters.</span></span> 

<span data-ttu-id="529e7-137">Szeretnénk továbbá hello rendelés nevének egy részét, mert hello első öt karakterek nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="529e7-137">We also want a substring of hello order's name because hello first five characters are not used.</span></span>

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="529e7-138">Toooutside belül működik:</span><span class="sxs-lookup"><span data-stu-id="529e7-138">Working from inside toooutside:</span></span>

1. <span data-ttu-id="529e7-139">Első hello [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) hello megrendelő megadásához, így azt vissza hello karakterek összesített száma.</span><span class="sxs-lookup"><span data-stu-id="529e7-139">Get hello [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) for hello orderer's name, so we get back hello total number of characters.</span></span>

2. <span data-ttu-id="529e7-140">Kivonás 5, mert azt szeretnénk, ha egy rövidebb karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="529e7-140">Subtract 5 because we want a shorter string.</span></span>

3. <span data-ttu-id="529e7-141">Hello ténylegesen, érvénybe [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span><span class="sxs-lookup"><span data-stu-id="529e7-141">Actually, take hello [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span></span> <span data-ttu-id="529e7-142">Először indexnél `5` és folytassa a hátralévő részét hello karakterlánc hello.</span><span class="sxs-lookup"><span data-stu-id="529e7-142">We start at index `5` and go hello remainder of hello string.</span></span>

4. <span data-ttu-id="529e7-143">Alakítsa át a substring tooa [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="529e7-143">Convert this substring tooa [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string.</span></span>

5. <span data-ttu-id="529e7-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden hello `+` karakterből álló `-` karaktereket.</span><span class="sxs-lookup"><span data-stu-id="529e7-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all hello `+` characters with `-` characters.</span></span>

6. <span data-ttu-id="529e7-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden hello `/` karakterből álló `_` karaktereket.</span><span class="sxs-lookup"><span data-stu-id="529e7-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all hello `/` characters with `_` characters.</span></span>

## <a name="work-with-date-times"></a><span data-ttu-id="529e7-146">Időpontok használata</span><span class="sxs-lookup"><span data-stu-id="529e7-146">Work with Date Times</span></span>

<span data-ttu-id="529e7-147">Időpontok hasznos lehet, különösen olyan adatforrást, amelynek a természetes nem támogatja toopull adatait próbálja *eseményindítók*.</span><span class="sxs-lookup"><span data-stu-id="529e7-147">Date Times can be useful, particularly when you are trying toopull data from a data source that doesn't naturally support *triggers*.</span></span> <span data-ttu-id="529e7-148">Használhatja a időpontok mennyi ideig számos lépés végzése kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="529e7-148">You can also use Date Times for finding how long various steps are taking.</span></span>

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="529e7-149">Ebben a példában a Microsoft hello kibontása `startTime` hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="529e7-149">In this example, we extract hello `startTime` from hello previous step.</span></span> <span data-ttu-id="529e7-150">Ezután azt beolvasni hello aktuális idejét, és egy második kivonása:</span><span class="sxs-lookup"><span data-stu-id="529e7-150">Then we get hello current time, and subtract one second:</span></span>

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

<span data-ttu-id="529e7-151">Például használhatja más időegységekkel `minutes` vagy `hours`.</span><span class="sxs-lookup"><span data-stu-id="529e7-151">You can use other units of time, like `minutes` or `hours`.</span></span> <span data-ttu-id="529e7-152">E két érték végül azt is összehasonlíthatja.</span><span class="sxs-lookup"><span data-stu-id="529e7-152">Finally, we can compare these two values.</span></span> <span data-ttu-id="529e7-153">Ha hello első érték kisebb, mint a második érték hello, akkor több mint egy második hello először megbízást óta eltelt.</span><span class="sxs-lookup"><span data-stu-id="529e7-153">If hello first value is less than hello second value, then more than one second has passed since hello order was first placed.</span></span>

<span data-ttu-id="529e7-154">tooformat dátum, karakterlánc is is használhatók.</span><span class="sxs-lookup"><span data-stu-id="529e7-154">tooformat dates, we can use string formatters.</span></span> <span data-ttu-id="529e7-155">Például a tooget hello RFC1123, használjuk [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="529e7-155">For example, tooget hello RFC1123, we use [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span> <span data-ttu-id="529e7-156">toolearn dátum formázása, lásd: [Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="529e7-156">toolearn about date formatting, see [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span>

## <a name="deployment-parameters-for-different-environments"></a><span data-ttu-id="529e7-157">A különböző környezetek üzembe helyezéshez megadott paraméterek</span><span class="sxs-lookup"><span data-stu-id="529e7-157">Deployment parameters for different environments</span></span>

<span data-ttu-id="529e7-158">Gyakran központi telepítési életciklusának rendelkezik, a környezet, egy átmeneti és éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="529e7-158">Commonly, deployment lifecycles have a development environment, a staging environment, and a production environment.</span></span> <span data-ttu-id="529e7-159">Használhat például hello ugyanazon definíció ezekben a környezetekben, de különböző adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="529e7-159">For example, you might use hello same definition in all these environments but use different databases.</span></span> <span data-ttu-id="529e7-160">Hasonlóképpen, érdemes toouse ugyanazon definíció hello között különböző régiókban a magas rendelkezésre állású, de szeretné, hogy minden logic app példány tootalk toothat terület adatbázis.</span><span class="sxs-lookup"><span data-stu-id="529e7-160">Likewise, you might want toouse hello same definition across different regions for high availability but want each logic app instance tootalk toothat region's database.</span></span>
<span data-ttu-id="529e7-161">Ebben a forgatókönyvben eltér paraméterek véve *futásidejű* ahol Ehelyett használjon hello `trigger()` hello előző példában szemléltetett működéséhez.</span><span class="sxs-lookup"><span data-stu-id="529e7-161">This scenario differs from taking parameters at *runtime* where instead, you should use hello `trigger()` function as in hello previous example.</span></span>

<span data-ttu-id="529e7-162">Kezdésként használhatja az ebben a példában például egy alapszintű definíciója:</span><span class="sxs-lookup"><span data-stu-id="529e7-162">You can start with a basic definition like this example:</span></span>

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

<span data-ttu-id="529e7-163">A tényleges hello `PUT` kérés hello a logic apps, megadhatja a hello paraméter `uri`.</span><span class="sxs-lookup"><span data-stu-id="529e7-163">In hello actual `PUT` request for hello logic apps, you can provide hello parameter `uri`.</span></span> <span data-ttu-id="529e7-164">Mivel az alapértelmezett értéke már nem létezik, hello logic app hasznos kell ezt a paramétert:</span><span class="sxs-lookup"><span data-stu-id="529e7-164">Because a default value no longer exists, hello logic app payload requires this parameter:</span></span>

```
{
    "properties": {},
        "definition": {
          // Use hello definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

<span data-ttu-id="529e7-165">Minden környezetben biztosítható egy másik értéket hello `connection` paraméter.</span><span class="sxs-lookup"><span data-stu-id="529e7-165">In each environment, you can provide a different value for hello `connection` parameter.</span></span> 

<span data-ttu-id="529e7-166">Az összes hello beállításokat kell és logic Apps alkalmazásokat kezeléséhez, a következő témakörben: hello [REST API-dokumentáció](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span><span class="sxs-lookup"><span data-stu-id="529e7-166">For all hello options that you have for creating and managing logic apps, see hello [REST API documentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span></span> 
