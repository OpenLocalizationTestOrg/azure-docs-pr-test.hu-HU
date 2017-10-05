---
title: Adja meg a JSON - Azure Logic Apps munkafolyamatok |} Microsoft Docs
description: "Munkafolyamat-definícióhoz írásával a JSON-ban a logic Apps alkalmazások"
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
ms.openlocfilehash: 7f9e5a10066df8a464c285273e77a85c0d562ebb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a><span data-ttu-id="76a50-103">A logic apps segítségével JSON munkafolyamat-meghatározások létrehozása</span><span class="sxs-lookup"><span data-stu-id="76a50-103">Create workflow definitions for logic apps using JSON</span></span>

<span data-ttu-id="76a50-104">A munkafolyamat-definícióhoz hozhat létre [Azure Logic Apps](logic-apps-what-are-logic-apps.md) egyszerű, deklaratív JSON nyelv.</span><span class="sxs-lookup"><span data-stu-id="76a50-104">You can create workflow definitions for [Azure Logic Apps](logic-apps-what-are-logic-apps.md) with simple, declarative JSON language.</span></span> <span data-ttu-id="76a50-105">Ha még nem tette meg, olvassa el [az első logikai alkalmazás létrehozása a Logic App tervezővel](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="76a50-105">If you haven't already, first review [how to create your first logic app with Logic App Designer](logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="76a50-106">További tájékoztatás a [hivatkozás a Munkafolyamatdefiníciós nyelve a teljes](http://aka.ms/logicappsdocs).</span><span class="sxs-lookup"><span data-stu-id="76a50-106">Also, see the [full reference for the Workflow Definition Language](http://aka.ms/logicappsdocs).</span></span>

## <a name="repeat-steps-over-a-list"></a><span data-ttu-id="76a50-107">Ismételje meg a során</span><span class="sxs-lookup"><span data-stu-id="76a50-107">Repeat steps over a list</span></span>

<span data-ttu-id="76a50-108">Olyan tömb, amely legfeljebb 10 000 elem iterációt, és minden elem egy műveletet, használja a [foreach típus](logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="76a50-108">To iterate through an array that has up to 10,000 items and perform an action for each item, use the [foreach type](logic-apps-loops-and-scopes.md).</span></span>

## <a name="handle-failures-if-something-goes-wrong"></a><span data-ttu-id="76a50-109">Kijavíthassa a hibákat, ha valamilyen hiba</span><span class="sxs-lookup"><span data-stu-id="76a50-109">Handle failures if something goes wrong</span></span>

<span data-ttu-id="76a50-110">Általában olyan szeretne hozzáadni egy *szervizelési lépés* – néhány logika, amely végrehajtja a *csak, ha* egy vagy több, a hívás sikertelen.</span><span class="sxs-lookup"><span data-stu-id="76a50-110">Usually, you want to include a *remediation step* — some logic that executes *if and only if* one or more of your calls fail.</span></span> <span data-ttu-id="76a50-111">Ebben a példában adatok lekérése a különböző helyek, de a hívás sikertelen lesz, ha azt szeretné-e valahol utáni egy üzenetet, így azt követheti nyomon, hogy hiba le később:</span><span class="sxs-lookup"><span data-stu-id="76a50-111">This example gets data from various places, but if the call fails, we want to POST a message somewhere so we can track down that failure later:</span></span>  

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

<span data-ttu-id="76a50-112">Annak meghatározása, hogy `postToErrorMessageQueue` csak futtat `readData` rendelkezik `Failed`, használja a `runAfter` tulajdonságot, például a lehetséges értékek listáját adja meg, hogy `runAfter` lehet `["Succeeded", "Failed"]`.</span><span class="sxs-lookup"><span data-stu-id="76a50-112">To specify that `postToErrorMessageQueue` only runs after `readData` has `Failed`, use the `runAfter` property, for example, to specify a list of possible values, so that `runAfter` could be `["Succeeded", "Failed"]`.</span></span>

<span data-ttu-id="76a50-113">Végül, ez a példa most kezeli a hiba, mert azt már nem jelölje meg a Futtatás mint `Failed`.</span><span class="sxs-lookup"><span data-stu-id="76a50-113">Finally, because this example now handles the error, we no longer mark the run as `Failed`.</span></span> <span data-ttu-id="76a50-114">Mivel azt adja meg a lépés kezelése ebben a példában ez a hiba, rendelkezik-e a Futtatás `Succeeded` bár egy lépésben `Failed`.</span><span class="sxs-lookup"><span data-stu-id="76a50-114">Because we added the step for handling this failure in this example, the run has `Succeeded` although one step `Failed`.</span></span>

## <a name="execute-two-or-more-steps-in-parallel"></a><span data-ttu-id="76a50-115">Két vagy több lépést végre párhuzamosan</span><span class="sxs-lookup"><span data-stu-id="76a50-115">Execute two or more steps in parallel</span></span>

<span data-ttu-id="76a50-116">Ezzel párhuzamosan több műveletek futtatására a `runAfter` tulajdonságot meg kell futásidőben.</span><span class="sxs-lookup"><span data-stu-id="76a50-116">To run multiple actions in parallel, the `runAfter` property must be equivalent at runtime.</span></span> 

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

<span data-ttu-id="76a50-117">Ebben a példában is `branch1` és `branch2` futtatása után értékre van beállítva `readData`.</span><span class="sxs-lookup"><span data-stu-id="76a50-117">In this example, both `branch1` and `branch2` are set to run after `readData`.</span></span> <span data-ttu-id="76a50-118">Ennek eredményeképpen a mindkét ágak párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="76a50-118">As a result, both branches run in parallel.</span></span> <span data-ttu-id="76a50-119">Mindkét ágak időbélyegzője megegyezik.</span><span class="sxs-lookup"><span data-stu-id="76a50-119">The timestamp for both branches is identical.</span></span>

![Párhuzamos](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a><span data-ttu-id="76a50-121">Csatlakozás két párhuzamos ág</span><span class="sxs-lookup"><span data-stu-id="76a50-121">Join two parallel branches</span></span>

<span data-ttu-id="76a50-122">Két műveletek elemek hozzáadásával párhuzamosan futó beállított csatlakozhat a `runAfter` tulajdonság az előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="76a50-122">You can join two actions that are set to run in parallel by adding items to the `runAfter` property as in the previous example.</span></span>

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

## <a name="map-list-items-to-a-different-configuration"></a><span data-ttu-id="76a50-124">Egy másik konfigurációs listaelemek leképezése</span><span class="sxs-lookup"><span data-stu-id="76a50-124">Map list items to a different configuration</span></span>

<span data-ttu-id="76a50-125">Ezt követően tegyük fel, hogy azt szeretné, hogy a tulajdonság értékének alapján különböző tartalom.</span><span class="sxs-lookup"><span data-stu-id="76a50-125">Next, let's say that we want to get different content based on the value of a property.</span></span> <span data-ttu-id="76a50-126">Paraméterként létrehozhatunk olyan értékek célhelyekre térképet:</span><span class="sxs-lookup"><span data-stu-id="76a50-126">We can create a map of values to destinations as a parameter:</span></span>  

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

<span data-ttu-id="76a50-127">Ebben az esetben azt először kapnak a cikkek listáját.</span><span class="sxs-lookup"><span data-stu-id="76a50-127">In this case, we first get a list of articles.</span></span> <span data-ttu-id="76a50-128">A paraméterként megadott kategória alapján, a második lépésben használja a térkép kereséséhez a tartalom első URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="76a50-128">Based on the category that was defined as a parameter, the second step uses a map to look up the URL for getting the content.</span></span>

<span data-ttu-id="76a50-129">Néhány eset Itt figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="76a50-129">Some times to note here:</span></span> 

*   <span data-ttu-id="76a50-130">A [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) függvény ellenőrzi, hogy a kategória megegyezik-e ismert meghatározott kategóriák közül.</span><span class="sxs-lookup"><span data-stu-id="76a50-130">The [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) function checks whether the category matches one of the known defined categories.</span></span>

*   <span data-ttu-id="76a50-131">Miután azt lekérése a kategóriát, azt is lekéréses szögletes zárójelbe használatával leképezés elem:`parameters[...]`</span><span class="sxs-lookup"><span data-stu-id="76a50-131">After we get the category, we can pull the item from the map using square brackets: `parameters[...]`</span></span>

## <a name="process-strings"></a><span data-ttu-id="76a50-132">Folyamat karakterláncok</span><span class="sxs-lookup"><span data-stu-id="76a50-132">Process strings</span></span>

<span data-ttu-id="76a50-133">Különböző funkciókat karakterláncok módosítására használhatja.</span><span class="sxs-lookup"><span data-stu-id="76a50-133">You can use various functions to manipulate strings.</span></span> <span data-ttu-id="76a50-134">Tegyük fel például, a karakterlánc, amely azt szeretnénk, hogy egy system van, de jelenleg nem megfelelő kezelését a karakteres kódolási kapcsolatos benne.</span><span class="sxs-lookup"><span data-stu-id="76a50-134">For example, suppose we have a string that we want to pass to a system, but we aren't confident about proper handling for character encoding.</span></span> <span data-ttu-id="76a50-135">Egy elem Base64 kódolással. Ez a karakterlánc kódolása.</span><span class="sxs-lookup"><span data-stu-id="76a50-135">One option is to base64 encode this string.</span></span> <span data-ttu-id="76a50-136">Azonban egy URL-címben escape-karaktersorozat elkerüléséhez fogjuk néhány karakterek.</span><span class="sxs-lookup"><span data-stu-id="76a50-136">However, to avoid escaping in a URL, we are going to replace a few characters.</span></span> 

<span data-ttu-id="76a50-137">Szeretnénk továbbá rendelés nevének egy részét, mert az első öt karakterek nem használhatók.</span><span class="sxs-lookup"><span data-stu-id="76a50-137">We also want a substring of the order's name because the first five characters are not used.</span></span>

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

<span data-ttu-id="76a50-138">Munka a belül a kívül:</span><span class="sxs-lookup"><span data-stu-id="76a50-138">Working from inside to outside:</span></span>

1. <span data-ttu-id="76a50-139">Beolvasása a [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) a megrendelő megadásához, így azt vissza karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="76a50-139">Get the [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) for the orderer's name, so we get back the total number of characters.</span></span>

2. <span data-ttu-id="76a50-140">Kivonás 5, mert azt szeretnénk, ha egy rövidebb karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="76a50-140">Subtract 5 because we want a shorter string.</span></span>

3. <span data-ttu-id="76a50-141">Ténylegesen, igénybe vehet a [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span><span class="sxs-lookup"><span data-stu-id="76a50-141">Actually, take the [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span></span> <span data-ttu-id="76a50-142">Először indexnél `5` és nyissa meg a fennmaradó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="76a50-142">We start at index `5` and go the remainder of the string.</span></span>

4. <span data-ttu-id="76a50-143">A keresendő karakterláncrészletet átalakítani egy [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="76a50-143">Convert this substring to a [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string.</span></span>

5. <span data-ttu-id="76a50-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden a `+` karakterből álló `-` karaktereket.</span><span class="sxs-lookup"><span data-stu-id="76a50-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all the `+` characters with `-` characters.</span></span>

6. <span data-ttu-id="76a50-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden a `/` karakterből álló `_` karaktereket.</span><span class="sxs-lookup"><span data-stu-id="76a50-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all the `/` characters with `_` characters.</span></span>

## <a name="work-with-date-times"></a><span data-ttu-id="76a50-146">Időpontok használata</span><span class="sxs-lookup"><span data-stu-id="76a50-146">Work with Date Times</span></span>

<span data-ttu-id="76a50-147">Időpontok hasznos lehet, különösen olyan adatforrást, amelynek a természetes nem támogatja olvasnak be adatokat próbál *eseményindítók*.</span><span class="sxs-lookup"><span data-stu-id="76a50-147">Date Times can be useful, particularly when you are trying to pull data from a data source that doesn't naturally support *triggers*.</span></span> <span data-ttu-id="76a50-148">Használhatja a időpontok mennyi ideig számos lépés végzése kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="76a50-148">You can also use Date Times for finding how long various steps are taking.</span></span>

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

<span data-ttu-id="76a50-149">Ez a példa azt bontsa ki a `startTime` az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="76a50-149">In this example, we extract the `startTime` from the previous step.</span></span> <span data-ttu-id="76a50-150">Ezután azt beolvasni az aktuális idejét, és egy második kivonása:</span><span class="sxs-lookup"><span data-stu-id="76a50-150">Then we get the current time, and subtract one second:</span></span>

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

<span data-ttu-id="76a50-151">Például használhatja más időegységekkel `minutes` vagy `hours`.</span><span class="sxs-lookup"><span data-stu-id="76a50-151">You can use other units of time, like `minutes` or `hours`.</span></span> <span data-ttu-id="76a50-152">E két érték végül azt is összehasonlíthatja.</span><span class="sxs-lookup"><span data-stu-id="76a50-152">Finally, we can compare these two values.</span></span> <span data-ttu-id="76a50-153">Ha az első érték kisebb, mint a második érték, akkor több mint egy második megbízást először óta eltelt.</span><span class="sxs-lookup"><span data-stu-id="76a50-153">If the first value is less than the second value, then more than one second has passed since the order was first placed.</span></span>

<span data-ttu-id="76a50-154">Dátumok formázásához karakterlánc is használhatja azt.</span><span class="sxs-lookup"><span data-stu-id="76a50-154">To format dates, we can use string formatters.</span></span> <span data-ttu-id="76a50-155">Például a RFC1123 beszerzéséhez használjuk [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="76a50-155">For example, to get the RFC1123, we use [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span> <span data-ttu-id="76a50-156">Dátum formázás, lásd: [Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="76a50-156">To learn about date formatting, see [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span>

## <a name="deployment-parameters-for-different-environments"></a><span data-ttu-id="76a50-157">A különböző környezetek üzembe helyezéshez megadott paraméterek</span><span class="sxs-lookup"><span data-stu-id="76a50-157">Deployment parameters for different environments</span></span>

<span data-ttu-id="76a50-158">Gyakran központi telepítési életciklusának rendelkezik, a környezet, egy átmeneti és éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="76a50-158">Commonly, deployment lifecycles have a development environment, a staging environment, and a production environment.</span></span> <span data-ttu-id="76a50-159">Például előfordulhat, hogy ugyanazon definíció ezekben a környezetekben használható, de különböző adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="76a50-159">For example, you might use the same definition in all these environments but use different databases.</span></span> <span data-ttu-id="76a50-160">Hasonlóképpen érdemes ugyanazon definíció különböző régiókban használja a magas rendelkezésre állású, de szeretné, hogy felvegye a adott régióban adatbázis minden logic app-példány.</span><span class="sxs-lookup"><span data-stu-id="76a50-160">Likewise, you might want to use the same definition across different regions for high availability but want each logic app instance to talk to that region's database.</span></span>
<span data-ttu-id="76a50-161">Ebben a forgatókönyvben eltér paraméterek véve *futásidejű* ahol Ehelyett használjon a `trigger()` az előző példában szemléltetett működéséhez.</span><span class="sxs-lookup"><span data-stu-id="76a50-161">This scenario differs from taking parameters at *runtime* where instead, you should use the `trigger()` function as in the previous example.</span></span>

<span data-ttu-id="76a50-162">Kezdésként használhatja az ebben a példában például egy alapszintű definíciója:</span><span class="sxs-lookup"><span data-stu-id="76a50-162">You can start with a basic definition like this example:</span></span>

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

<span data-ttu-id="76a50-163">A tényleges `PUT` kérhetnek a logic apps, megadhatja, hogy a paraméter `uri`.</span><span class="sxs-lookup"><span data-stu-id="76a50-163">In the actual `PUT` request for the logic apps, you can provide the parameter `uri`.</span></span> <span data-ttu-id="76a50-164">Mivel az alapértelmezett értéke már nem létezik, a logic app forgalma kell ezt a paramétert:</span><span class="sxs-lookup"><span data-stu-id="76a50-164">Because a default value no longer exists, the logic app payload requires this parameter:</span></span>

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
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

<span data-ttu-id="76a50-165">Minden környezetben, adjon meg más értéket a a `connection` paraméter.</span><span class="sxs-lookup"><span data-stu-id="76a50-165">In each environment, you can provide a different value for the `connection` parameter.</span></span> 

<span data-ttu-id="76a50-166">Az összes lehetséges, hogy rendelkezik, és logic Apps alkalmazásokat kezeléséhez, tekintse meg a [REST API-dokumentáció](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span><span class="sxs-lookup"><span data-stu-id="76a50-166">For all the options that you have for creating and managing logic apps, see the [REST API documentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span></span> 
