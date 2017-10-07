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
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>A logic apps segítségével JSON munkafolyamat-meghatározások létrehozása

A munkafolyamat-definícióhoz hozhat létre [Azure Logic Apps](logic-apps-what-are-logic-apps.md) egyszerű, deklaratív JSON nyelv. Ha még nem tette meg, olvassa el [hogyan toocreate az első logikai alkalmazást a Logic App tervezővel](logic-apps-create-a-logic-app.md). Lásd még hello [útmutató hello Munkafolyamatdefiníciós nyelve a teljes](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Ismételje meg a során

olyan tömb, amely nem rendelkezik too10, 000 elemek fel és egy műveletet minden elemhez, hello használata révén tooiterate [foreach típus](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Kijavíthassa a hibákat, ha valamilyen hiba

Általában érdemes tooinclude egy *szervizelési lépés* – néhány logika, amely végrehajtja a *csak, ha* egy vagy több, a hívás sikertelen. Ez a példa adatok lekérése a különböző helyek, de hello hívás sikertelen lesz, ha azt szeretnénk, egy üzenet tooPOST valahol, azt követheti nyomon, hogy hiba le később:  

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

toospecify, amely `postToErrorMessageQueue` csak futtat `readData` rendelkezik `Failed`, hello használata `runAfter` tulajdonság, például toospecify a lehetséges értékek listáját, hogy `runAfter` lehet `["Succeeded", "Failed"]`.

Végül, ebben a példában a hello hiba most kezeli, mivel azt már nem megjelölni futtató hello `Failed`. Hozzáadott hello lépés kezelése ebben a példában ez a hiba, mert rendelkezik-e futtatni hello `Succeeded` bár egy lépésben `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>Két vagy több lépést végre párhuzamosan

toorun párhuzamosan több művelet hello `runAfter` tulajdonságot meg kell futásidőben. 

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

Ebben a példában is `branch1` és `branch2` toorun után vannak beállítva `readData`. Ennek eredményeképpen a mindkét ágak párhuzamosan futnak. mindkét ágak hello időbélyegzőjét megegyezik.

![Párhuzamos](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Csatlakozás két párhuzamos ág

Csatlakozhat a két elem toohello hozzáadásával toorun beállított párhuzamos műveletek `runAfter` tulajdonság hello előző példában látható módon.

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

## <a name="map-list-items-tooa-different-configuration"></a>Térkép elemek tooa különböző konfigurációjának felsorolása

Ezt követően tegyük fel, hogy szeretnénk tooget eltérő tartalomra hello tulajdonság értéke alapján. Paraméterként létrehozhatunk olyan értékek toodestinations térképére:  

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

Ebben az esetben azt először kapnak a cikkek listáját. A paraméterként megadott hello kategória alapján, hello második lépésben használ a térkép toolook hello URL-cím mentése hello tartalom beolvasása.

Néhány eset toonote itt: 

*   Hello [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) függvény ellenőrzi, hogy hello kategória sem felel meg egyik ismert meghatározott kategóriákba hello.

*   Után azt lekérése hello kategória, azt is lekéréses hello elem hello leképezés szögletes zárójelbe használatával:`parameters[...]`

## <a name="process-strings"></a>Folyamat karakterláncok

Használhatja a különböző funkciók toomanipulate karakterláncok. Tegyük fel, hogy egy karakterlánc toopass tooa rendszer szeretnénk, de jelenleg nem megfelelő kezelését a karakteres kódolási kapcsolatos biztosnak kell. Egy elem toobase64 Ez a karakterlánc kódolása. Azonban egy URL-címben escape-karaktersorozat tooavoid fogjuk tooreplace néhány karaktert. 

Szeretnénk továbbá hello rendelés nevének egy részét, mert hello első öt karakterek nem használhatók.

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

Toooutside belül működik:

1. Első hello [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) hello megrendelő megadásához, így azt vissza hello karakterek összesített száma.

2. Kivonás 5, mert azt szeretnénk, ha egy rövidebb karakterláncot.

3. Hello ténylegesen, érvénybe [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). Először indexnél `5` és folytassa a hátralévő részét hello karakterlánc hello.

4. Alakítsa át a substring tooa [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) karakterlánc.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden hello `+` karakterből álló `-` karaktereket.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)minden hello `/` karakterből álló `_` karaktereket.

## <a name="work-with-date-times"></a>Időpontok használata

Időpontok hasznos lehet, különösen olyan adatforrást, amelynek a természetes nem támogatja toopull adatait próbálja *eseményindítók*. Használhatja a időpontok mennyi ideig számos lépés végzése kereséséhez.

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

Ebben a példában a Microsoft hello kibontása `startTime` hello előző lépésben. Ezután azt beolvasni hello aktuális idejét, és egy második kivonása:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

Például használhatja más időegységekkel `minutes` vagy `hours`. E két érték végül azt is összehasonlíthatja. Ha hello első érték kisebb, mint a második érték hello, akkor több mint egy második hello először megbízást óta eltelt.

tooformat dátum, karakterlánc is is használhatók. Például a tooget hello RFC1123, használjuk [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). toolearn dátum formázása, lásd: [Munkafolyamatdefiníciós nyelve](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>A különböző környezetek üzembe helyezéshez megadott paraméterek

Gyakran központi telepítési életciklusának rendelkezik, a környezet, egy átmeneti és éles környezetben. Használhat például hello ugyanazon definíció ezekben a környezetekben, de különböző adatbázist használja. Hasonlóképpen, érdemes toouse ugyanazon definíció hello között különböző régiókban a magas rendelkezésre állású, de szeretné, hogy minden logic app példány tootalk toothat terület adatbázis.
Ebben a forgatókönyvben eltér paraméterek véve *futásidejű* ahol Ehelyett használjon hello `trigger()` hello előző példában szemléltetett működéséhez.

Kezdésként használhatja az ebben a példában például egy alapszintű definíciója:

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

A tényleges hello `PUT` kérés hello a logic apps, megadhatja a hello paraméter `uri`. Mivel az alapértelmezett értéke már nem létezik, hello logic app hasznos kell ezt a paramétert:

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

Minden környezetben biztosítható egy másik értéket hello `connection` paraméter. 

Az összes hello beállításokat kell és logic Apps alkalmazásokat kezeléséhez, a következő témakörben: hello [REST API-dokumentáció](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
