---
title: "aaaCreate fut a hurokban, és hatóköröket annak megfelelően, vagy a munkafolyamatokban - Azure Logic Apps adatok debatch |} Microsoft Docs"
description: "Hozzon létre hurkok tooiterate keresztül adatokat, a hatókörök, műveleteit, vagy debatch adatok toostart az Azure Logic Apps további munkafolyamatok."
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
ms.openlocfilehash: e612ec2e83541f028916a07bf12c44e7b1f57ad1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a><span data-ttu-id="1646f-103">Logic Apps-hurkok, -hatókörök és -kibontás</span><span class="sxs-lookup"><span data-stu-id="1646f-103">Logic Apps Loops, Scopes, and Debatching</span></span>
  
<span data-ttu-id="1646f-104">A Logic Apps számos módon toowork tömbök, gyűjtemények, kötegek és hurkok egy munkafolyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="1646f-104">Logic Apps provides a number of ways toowork with arrays, collections, batches, and loops within a workflow.</span></span>
  
## <a name="foreach-loop-and-arrays"></a><span data-ttu-id="1646f-105">ForEach hurok és tömbök</span><span class="sxs-lookup"><span data-stu-id="1646f-105">ForEach loop and arrays</span></span>
  
<span data-ttu-id="1646f-106">A Logic Apps lehetővé teszi az adatok egy készleten tooloop, és egy műveletet minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="1646f-106">Logic Apps allows you tooloop over a set of data and perform an action for each item.</span></span>  <span data-ttu-id="1646f-107">Ez az alkalmazáson keresztül hello `foreach` művelet.</span><span class="sxs-lookup"><span data-stu-id="1646f-107">This is possible via hello `foreach` action.</span></span>  <span data-ttu-id="1646f-108">Hello Designer tooadd megadhat egy for-each ciklus.</span><span class="sxs-lookup"><span data-stu-id="1646f-108">In hello designer, you can specify tooadd a for each loop.</span></span>  <span data-ttu-id="1646f-109">Miután kijelölte hello tömb tooiterate keresztül kívánja, elkezdhet műveletek.</span><span class="sxs-lookup"><span data-stu-id="1646f-109">After selecting hello array you wish tooiterate over, you can begin adding actions.</span></span>  <span data-ttu-id="1646f-110">Jelenleg korlátozott tooonly egy-egy művelettel / foreach hurok, de ez a korlátozás a hét hamarosan hello azonnal fel kell oldani.</span><span class="sxs-lookup"><span data-stu-id="1646f-110">Currently you are limited tooonly one action per foreach loop, but this restriction will be lifted in hello coming weeks.</span></span>  <span data-ttu-id="1646f-111">Egyszer hello hurkon belül megkezdheti toospecify, mi történjen, minden egyes hello tömb értékénél.</span><span class="sxs-lookup"><span data-stu-id="1646f-111">Once within hello loop you can begin toospecify what should occur at each value of hello array.</span></span>

<span data-ttu-id="1646f-112">Kód-nézet használata esetén is megadhat egy for-each ciklus például alatt.</span><span class="sxs-lookup"><span data-stu-id="1646f-112">If using code-view, you can specify a for each loop like below.</span></span>  <span data-ttu-id="1646f-113">Itt látható egy példa egy for-each ciklus, amelyet az összes e-mail címet, amely tartalmazza a "microsoft.com" az e-mailt küld:</span><span class="sxs-lookup"><span data-stu-id="1646f-113">This is an example of a for each loop that sends an email for each email address that contains 'microsoft.com':</span></span>

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
  
  <span data-ttu-id="1646f-114">A `foreach` művelet is ismétlés tömbök too5, 000 sor mentése.</span><span class="sxs-lookup"><span data-stu-id="1646f-114">A `foreach` action can iterate over arrays up too5,000 rows.</span></span>  <span data-ttu-id="1646f-115">Alapértelmezés szerint minden egyes ismétlés párhuzamosan fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="1646f-115">Each iteration will execute in parallel by default.</span></span>  

### <a name="sequential-foreach-loops"></a><span data-ttu-id="1646f-116">Szekvenciális ForEach hurkok</span><span class="sxs-lookup"><span data-stu-id="1646f-116">Sequential ForEach loops</span></span>

<span data-ttu-id="1646f-117">a foreach hurok tooexecute egymás után, hello tooenable `Sequential` műveletet hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="1646f-117">tooenable a foreach loop tooexecute sequentially, hello `Sequential` operation option should be added.</span></span>

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a><span data-ttu-id="1646f-118">Hurok-ig</span><span class="sxs-lookup"><span data-stu-id="1646f-118">Until loop</span></span>
  
  <span data-ttu-id="1646f-119">Művelet vagy műveletsorozattal hajthat végre, amíg egy feltétel nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="1646f-119">You can perform an action or series of actions until a condition is met.</span></span>  <span data-ttu-id="1646f-120">hello Ennek leggyakoribb forgatókönyvet hívja meg a végpont csak a keresett hello választ kapott.</span><span class="sxs-lookup"><span data-stu-id="1646f-120">hello most common scenario for this is calling an endpoint until you get hello response you are looking for.</span></span>  <span data-ttu-id="1646f-121">Hello Designer tooadd megadhat egy hurok-ig.</span><span class="sxs-lookup"><span data-stu-id="1646f-121">In hello designer, you can specify tooadd an until loop.</span></span>  <span data-ttu-id="1646f-122">A felvett hello hurok található műveletek, akkor is hello kilépési feltétel, valamint hello hurok korlátok.</span><span class="sxs-lookup"><span data-stu-id="1646f-122">After adding actions inside hello loop, you can set hello exit condition, as well as hello loop limits.</span></span>  <span data-ttu-id="1646f-123">Van egy 1 perc késleltetéssel a hurok ciklusok között.</span><span class="sxs-lookup"><span data-stu-id="1646f-123">There is a 1 minute delay between loop cycles.</span></span>
  
  <span data-ttu-id="1646f-124">Kód-nézet használata esetén is megadhat egy amíg hurok, például alatt.</span><span class="sxs-lookup"><span data-stu-id="1646f-124">If using code-view, you can specify an until loop like below.</span></span>  <span data-ttu-id="1646f-125">Itt látható egy példa egy HTTP-végpont meghívása, amíg hello adott válasz törzsének értéke hello "Befejezve".</span><span class="sxs-lookup"><span data-stu-id="1646f-125">This is an example of calling an HTTP endpoint until hello response body has hello value 'Completed'.</span></span>  <span data-ttu-id="1646f-126">A feladat befejeződik mikor vagy</span><span class="sxs-lookup"><span data-stu-id="1646f-126">It will complete when either</span></span> 
  
  * <span data-ttu-id="1646f-127">HTTP-válasz "Befejezve" állapota</span><span class="sxs-lookup"><span data-stu-id="1646f-127">HTTP Response has status of 'Completed'</span></span>
  * <span data-ttu-id="1646f-128">1 óráig próbált</span><span class="sxs-lookup"><span data-stu-id="1646f-128">It has tried for 1 hour</span></span>
  * <span data-ttu-id="1646f-129">Az rendelkezik beállításpanelen 100 alkalommal</span><span class="sxs-lookup"><span data-stu-id="1646f-129">It has looped 100 times</span></span>
  
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
  
## <a name="spliton-and-debatching"></a><span data-ttu-id="1646f-130">SplitOn és debatching</span><span class="sxs-lookup"><span data-stu-id="1646f-130">SplitOn and debatching</span></span>

<span data-ttu-id="1646f-131">Néha egy eseményindító jelenhet meg szeretné, hogy toodebatch, és a elindítsanak egy munkafolyamatot elemenként elemekre tömbjét.</span><span class="sxs-lookup"><span data-stu-id="1646f-131">Sometimes a trigger may receive an array of items that you want toodebatch and start a workflow per item.</span></span>  <span data-ttu-id="1646f-132">Hello keresztül ehhez `spliton` parancsot.</span><span class="sxs-lookup"><span data-stu-id="1646f-132">This can be accomplished via hello `spliton` command.</span></span>  <span data-ttu-id="1646f-133">Alapértelmezés szerint az eseményindító swagger határozza meg a hasznos adatok között, amely tömb, ha egy `spliton` megjelenik, és indítsa el a elemenként futását.</span><span class="sxs-lookup"><span data-stu-id="1646f-133">By default, if your trigger swagger specifies a payload that is an array, a `spliton` will be added and start a run per item.</span></span>  <span data-ttu-id="1646f-134">SplitOn nem vehető tooa eseményindító.</span><span class="sxs-lookup"><span data-stu-id="1646f-134">SplitOn can only be added tooa trigger.</span></span>  <span data-ttu-id="1646f-135">Ez manuálisan konfigurálhatók, vagy definition kódnézetben felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="1646f-135">This can be manually configured or overridden in definition code-view.</span></span>  <span data-ttu-id="1646f-136">Jelenleg a SplitOn is debatch tömbök too5, 000 elemek mentése.</span><span class="sxs-lookup"><span data-stu-id="1646f-136">Currently SplitOn can debatch arrays up too5,000 items.</span></span>  <span data-ttu-id="1646f-137">Nem lehet egy `spliton` és hello szinkron válasz mintát is megvalósíthatja.</span><span class="sxs-lookup"><span data-stu-id="1646f-137">You cannot have a `spliton` and also implement hello synchronous response pattern.</span></span>  <span data-ttu-id="1646f-138">Minden munkafolyamat neve, amely rendelkezik egy `response` továbbá művelet túl`spliton` aszinkron módon futnak le és azonnali küldés `202 Accepted` választ.</span><span class="sxs-lookup"><span data-stu-id="1646f-138">Any workflow called that has a `response` action in addition too`spliton` will run asynchronously and send an immediate `202 Accepted` response.</span></span>  

<span data-ttu-id="1646f-139">A következő példa hello kódnézetben-SplitOn adhat meg.</span><span class="sxs-lookup"><span data-stu-id="1646f-139">SplitOn can be specified in code-view as hello following example.</span></span>  <span data-ttu-id="1646f-140">Ez elemek tömbje fogad, és az egyes sorokkal debatches.</span><span class="sxs-lookup"><span data-stu-id="1646f-140">This receives an array of items and debatches on each row.</span></span>

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

## <a name="scopes"></a><span data-ttu-id="1646f-141">Hatókörök</span><span class="sxs-lookup"><span data-stu-id="1646f-141">Scopes</span></span>

<span data-ttu-id="1646f-142">Már lehetséges toogroup műveletsorozat hatókör együttes használatával.</span><span class="sxs-lookup"><span data-stu-id="1646f-142">It is possible toogroup a series of actions together using a scope.</span></span>  <span data-ttu-id="1646f-143">Ez különösen fontos kivételkezelést megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="1646f-143">This is particularly useful for implementing exception handling.</span></span>  <span data-ttu-id="1646f-144">Hello Designer Új hatókör hozzáadása, és megkezdheti saját műveletek hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="1646f-144">In hello designer you can add a new scope, and begin adding any actions inside of it.</span></span>  <span data-ttu-id="1646f-145">Hatókörök hasonló hello kódnézetben definiálhatja:</span><span class="sxs-lookup"><span data-stu-id="1646f-145">You can define scopes in code-view like hello following:</span></span>


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