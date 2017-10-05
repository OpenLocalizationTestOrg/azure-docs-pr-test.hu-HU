---
title: "Hurok- és hatókörök létrehozása vagy adatok a munkafolyamatokban - Azure Logic Apps debatch |} Microsoft Docs"
description: "Az adatok, a hatókörök, műveleteit iterációt hurkok létrehozása, vagy adatokat az Azure Logic Apps további munkafolyamatok indítására debatch."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 413a2ba9107ca259ed577825bf0a17ff5622f1ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a><span data-ttu-id="5ef16-103">Logic Apps-hurkok, -hatókörök és -kibontás</span><span class="sxs-lookup"><span data-stu-id="5ef16-103">Logic Apps Loops, Scopes, and Debatching</span></span>
  
<span data-ttu-id="5ef16-104">A Logic Apps használata tömbök, gyűjtemények, kötegekben, számos, és fut a hurokban, a munkafolyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="5ef16-104">Logic Apps provides a number of ways to work with arrays, collections, batches, and loops within a workflow.</span></span>
  
## <a name="foreach-loop-and-arrays"></a><span data-ttu-id="5ef16-105">ForEach hurok és tömbök</span><span class="sxs-lookup"><span data-stu-id="5ef16-105">ForEach loop and arrays</span></span>
  
<span data-ttu-id="5ef16-106">A Logic Apps segítségével hurokba építhető adatok, és egy műveletet minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="5ef16-106">Logic Apps allows you to loop over a set of data and perform an action for each item.</span></span>  <span data-ttu-id="5ef16-107">Ez az alkalmazáson keresztül a `foreach` művelet.</span><span class="sxs-lookup"><span data-stu-id="5ef16-107">This is possible via the `foreach` action.</span></span>  <span data-ttu-id="5ef16-108">A tervezőben, megadhat hozzáadása egy for-each ciklus.</span><span class="sxs-lookup"><span data-stu-id="5ef16-108">In the designer, you can specify to add a for each loop.</span></span>  <span data-ttu-id="5ef16-109">A tömb kívánja az ismétlés kiválasztása, után elkezdhet műveletek.</span><span class="sxs-lookup"><span data-stu-id="5ef16-109">After selecting the array you wish to iterate over, you can begin adding actions.</span></span>  <span data-ttu-id="5ef16-110">Jelenleg csak egy művelet foreach hurok száma korlátozott, de ez a korlátozás az elkövetkező hetektől azonnal fel kell oldani.</span><span class="sxs-lookup"><span data-stu-id="5ef16-110">Currently you are limited to only one action per foreach loop, but this restriction will be lifted in the coming weeks.</span></span>  <span data-ttu-id="5ef16-111">Egyszer a hurkon belül elkezdheti adja meg, hogy mi történjen a tömb egyes értéken.</span><span class="sxs-lookup"><span data-stu-id="5ef16-111">Once within the loop you can begin to specify what should occur at each value of the array.</span></span>

<span data-ttu-id="5ef16-112">Kód-nézet használata esetén is megadhat egy for-each ciklus például alatt.</span><span class="sxs-lookup"><span data-stu-id="5ef16-112">If using code-view, you can specify a for each loop like below.</span></span>  <span data-ttu-id="5ef16-113">Itt látható egy példa egy for-each ciklus, amelyet az összes e-mail címet, amely tartalmazza a "microsoft.com" az e-mailt küld:</span><span class="sxs-lookup"><span data-stu-id="5ef16-113">This is an example of a for each loop that sends an email for each email address that contains 'microsoft.com':</span></span>

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  <span data-ttu-id="5ef16-114">A `foreach` művelet is többször over tömbállandó legfeljebb 5000 sort.</span><span class="sxs-lookup"><span data-stu-id="5ef16-114">A `foreach` action can iterate over arrays up to 5,000 rows.</span></span>  <span data-ttu-id="5ef16-115">Alapértelmezés szerint minden egyes ismétlés párhuzamosan fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="5ef16-115">Each iteration will execute in parallel by default.</span></span>  

### <a name="sequential-foreach-loops"></a><span data-ttu-id="5ef16-116">Szekvenciális ForEach hurkok</span><span class="sxs-lookup"><span data-stu-id="5ef16-116">Sequential ForEach loops</span></span>

<span data-ttu-id="5ef16-117">Foreach hurkot hajtható végre az egymás után, hogy a `Sequential` műveletet hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="5ef16-117">To enable a foreach loop to execute sequentially, the `Sequential` operation option should be added.</span></span>

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a><span data-ttu-id="5ef16-118">Hurok-ig</span><span class="sxs-lookup"><span data-stu-id="5ef16-118">Until loop</span></span>
  
  <span data-ttu-id="5ef16-119">Művelet vagy műveletsorozattal hajthat végre, amíg egy feltétel nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="5ef16-119">You can perform an action or series of actions until a condition is met.</span></span>  <span data-ttu-id="5ef16-120">A leggyakoribb forgatókönyvhöz, amelyben ez a végpont hívja meg addig, amíg a keresett választ kapott.</span><span class="sxs-lookup"><span data-stu-id="5ef16-120">The most common scenario for this is calling an endpoint until you get the response you are looking for.</span></span>  <span data-ttu-id="5ef16-121">A tervezőben, megadhat hozzáadása egy hurok-ig.</span><span class="sxs-lookup"><span data-stu-id="5ef16-121">In the designer, you can specify to add an until loop.</span></span>  <span data-ttu-id="5ef16-122">Miután hozzáadta a hurok található műveletek, beállíthatja a kilépési feltételt, valamint a hurok korlátok.</span><span class="sxs-lookup"><span data-stu-id="5ef16-122">After adding actions inside the loop, you can set the exit condition, as well as the loop limits.</span></span>  <span data-ttu-id="5ef16-123">Van egy 1 perc késleltetéssel a hurok ciklusok között.</span><span class="sxs-lookup"><span data-stu-id="5ef16-123">There is a 1 minute delay between loop cycles.</span></span>
  
  <span data-ttu-id="5ef16-124">Kód-nézet használata esetén is megadhat egy amíg hurok, például alatt.</span><span class="sxs-lookup"><span data-stu-id="5ef16-124">If using code-view, you can specify an until loop like below.</span></span>  <span data-ttu-id="5ef16-125">Itt látható egy példa egy HTTP-végpont meghívása mindaddig, amíg az adott válasz törzsének értéke "Befejezve".</span><span class="sxs-lookup"><span data-stu-id="5ef16-125">This is an example of calling an HTTP endpoint until the response body has the value 'Completed'.</span></span>  <span data-ttu-id="5ef16-126">A feladat befejeződik mikor vagy</span><span class="sxs-lookup"><span data-stu-id="5ef16-126">It will complete when either</span></span> 
  
  * <span data-ttu-id="5ef16-127">HTTP-válasz "Befejezve" állapota</span><span class="sxs-lookup"><span data-stu-id="5ef16-127">HTTP Response has status of 'Completed'</span></span>
  * <span data-ttu-id="5ef16-128">1 óráig próbált</span><span class="sxs-lookup"><span data-stu-id="5ef16-128">It has tried for 1 hour</span></span>
  * <span data-ttu-id="5ef16-129">Az rendelkezik beállításpanelen 100 alkalommal</span><span class="sxs-lookup"><span data-stu-id="5ef16-129">It has looped 100 times</span></span>
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a><span data-ttu-id="5ef16-130">SplitOn és debatching</span><span class="sxs-lookup"><span data-stu-id="5ef16-130">SplitOn and debatching</span></span>

<span data-ttu-id="5ef16-131">Néha egy eseményindító kaphat debatch és elindítsanak egy munkafolyamatot elemenként kívánt elemek tömbjét.</span><span class="sxs-lookup"><span data-stu-id="5ef16-131">Sometimes a trigger may receive an array of items that you want to debatch and start a workflow per item.</span></span>  <span data-ttu-id="5ef16-132">Keresztül ehhez a `spliton` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5ef16-132">This can be accomplished via the `spliton` command.</span></span>  <span data-ttu-id="5ef16-133">Alapértelmezés szerint az eseményindító swagger határozza meg a hasznos adatok között, amely tömb, ha egy `spliton` megjelenik, és indítsa el a elemenként futását.</span><span class="sxs-lookup"><span data-stu-id="5ef16-133">By default, if your trigger swagger specifies a payload that is an array, a `spliton` will be added and start a run per item.</span></span>  <span data-ttu-id="5ef16-134">SplitOn csak egy eseményindító lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="5ef16-134">SplitOn can only be added to a trigger.</span></span>  <span data-ttu-id="5ef16-135">Ez manuálisan konfigurálhatók, vagy definition kódnézetben felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="5ef16-135">This can be manually configured or overridden in definition code-view.</span></span>  <span data-ttu-id="5ef16-136">Jelenleg is debatch SplitOn tömbállandó legfeljebb 5 000 elemnél.</span><span class="sxs-lookup"><span data-stu-id="5ef16-136">Currently SplitOn can debatch arrays up to 5,000 items.</span></span>  <span data-ttu-id="5ef16-137">Nem lehet egy `spliton` és a szinkron válasz mintát is megvalósíthatja.</span><span class="sxs-lookup"><span data-stu-id="5ef16-137">You cannot have a `spliton` and also implement the synchronous response pattern.</span></span>  <span data-ttu-id="5ef16-138">Minden munkafolyamat neve, amely rendelkezik egy `response` művelet kívül `spliton` aszinkron módon futnak le és azonnali küldés `202 Accepted` választ.</span><span class="sxs-lookup"><span data-stu-id="5ef16-138">Any workflow called that has a `response` action in addition to `spliton` will run asynchronously and send an immediate `202 Accepted` response.</span></span>  

<span data-ttu-id="5ef16-139">Az alábbi példa-kódnézetben SplitOn adhat meg.</span><span class="sxs-lookup"><span data-stu-id="5ef16-139">SplitOn can be specified in code-view as the following example.</span></span>  <span data-ttu-id="5ef16-140">Ez elemek tömbje fogad, és az egyes sorokkal debatches.</span><span class="sxs-lookup"><span data-stu-id="5ef16-140">This receives an array of items and debatches on each row.</span></span>

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a><span data-ttu-id="5ef16-141">Hatókörök</span><span class="sxs-lookup"><span data-stu-id="5ef16-141">Scopes</span></span>

<span data-ttu-id="5ef16-142">A hatókör együttes használatával műveletsorozat csoportosításához lehetőség.</span><span class="sxs-lookup"><span data-stu-id="5ef16-142">It is possible to group a series of actions together using a scope.</span></span>  <span data-ttu-id="5ef16-143">Ez különösen fontos kivételkezelést megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="5ef16-143">This is particularly useful for implementing exception handling.</span></span>  <span data-ttu-id="5ef16-144">A tervezőben új hatókör hozzáadása, és megkezdheti saját műveletek hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="5ef16-144">In the designer you can add a new scope, and begin adding any actions inside of it.</span></span>  <span data-ttu-id="5ef16-145">Meghatározhatja a hatókörök kódnézetben a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="5ef16-145">You can define scopes in code-view like the following:</span></span>


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```