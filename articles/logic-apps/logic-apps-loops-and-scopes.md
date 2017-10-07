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
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logic Apps-hurkok, -hatókörök és -kibontás
  
A Logic Apps számos módon toowork tömbök, gyűjtemények, kötegek és hurkok egy munkafolyamaton belül.
  
## <a name="foreach-loop-and-arrays"></a>ForEach hurok és tömbök
  
A Logic Apps lehetővé teszi az adatok egy készleten tooloop, és egy műveletet minden elemhez.  Ez az alkalmazáson keresztül hello `foreach` művelet.  Hello Designer tooadd megadhat egy for-each ciklus.  Miután kijelölte hello tömb tooiterate keresztül kívánja, elkezdhet műveletek.  Jelenleg korlátozott tooonly egy-egy művelettel / foreach hurok, de ez a korlátozás a hét hamarosan hello azonnal fel kell oldani.  Egyszer hello hurkon belül megkezdheti toospecify, mi történjen, minden egyes hello tömb értékénél.

Kód-nézet használata esetén is megadhat egy for-each ciklus például alatt.  Itt látható egy példa egy for-each ciklus, amelyet az összes e-mail címet, amely tartalmazza a "microsoft.com" az e-mailt küld:

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
  
  A `foreach` művelet is ismétlés tömbök too5, 000 sor mentése.  Alapértelmezés szerint minden egyes ismétlés párhuzamosan fogja végrehajtani.  

### <a name="sequential-foreach-loops"></a>Szekvenciális ForEach hurkok

a foreach hurok tooexecute egymás után, hello tooenable `Sequential` műveletet hozzá kell adni.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Hurok-ig
  
  Művelet vagy műveletsorozattal hajthat végre, amíg egy feltétel nem teljesül.  hello Ennek leggyakoribb forgatókönyvet hívja meg a végpont csak a keresett hello választ kapott.  Hello Designer tooadd megadhat egy hurok-ig.  A felvett hello hurok található műveletek, akkor is hello kilépési feltétel, valamint hello hurok korlátok.  Van egy 1 perc késleltetéssel a hurok ciklusok között.
  
  Kód-nézet használata esetén is megadhat egy amíg hurok, például alatt.  Itt látható egy példa egy HTTP-végpont meghívása, amíg hello adott válasz törzsének értéke hello "Befejezve".  A feladat befejeződik mikor vagy 
  
  * HTTP-válasz "Befejezve" állapota
  * 1 óráig próbált
  * Az rendelkezik beállításpanelen 100 alkalommal
  
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
  
## <a name="spliton-and-debatching"></a>SplitOn és debatching

Néha egy eseményindító jelenhet meg szeretné, hogy toodebatch, és a elindítsanak egy munkafolyamatot elemenként elemekre tömbjét.  Hello keresztül ehhez `spliton` parancsot.  Alapértelmezés szerint az eseményindító swagger határozza meg a hasznos adatok között, amely tömb, ha egy `spliton` megjelenik, és indítsa el a elemenként futását.  SplitOn nem vehető tooa eseményindító.  Ez manuálisan konfigurálhatók, vagy definition kódnézetben felülbírálható.  Jelenleg a SplitOn is debatch tömbök too5, 000 elemek mentése.  Nem lehet egy `spliton` és hello szinkron válasz mintát is megvalósíthatja.  Minden munkafolyamat neve, amely rendelkezik egy `response` továbbá művelet túl`spliton` aszinkron módon futnak le és azonnali küldés `202 Accepted` választ.  

A következő példa hello kódnézetben-SplitOn adhat meg.  Ez elemek tömbje fogad, és az egyes sorokkal debatches.

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

## <a name="scopes"></a>Hatókörök

Már lehetséges toogroup műveletsorozat hatókör együttes használatával.  Ez különösen fontos kivételkezelést megvalósításához.  Hello Designer Új hatókör hozzáadása, és megkezdheti saját műveletek hozzáadását.  Hatókörök hasonló hello kódnézetben definiálhatja:


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