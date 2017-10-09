---
title: "aaaSchema frissíti 1 – 2015. augusztus előzetes verzió – Azure Logic Apps |} Microsoft Docs"
description: "Az Azure Logic Apps JSON-meghatározások létrehozása a séma verziója 2015-08-01. dátumú előnézeti"
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.openlocfilehash: 950cd18a27aa1859c4f0b6116de3fb8699d746c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Az Azure Logic Apps - 2015. augusztus 1 preview sémafrissítések

Az új séma- és API-t az Azure Logic Apps verzió magában foglalja a fontos fejlesztést tartalmaz, amelyek a logic apps több megbízható és egyszerűbb toouse:

*   Hello **APIApp** művelet típus frissített tooa új [ **APIConnection** ](#api-connections) művelet típusa.
*   **Ismételje meg a** túl a rendszer átnevezi[**Foreach**](#foreach).
*   Hello [ **HTTP-figyelő** API-alkalmazás](#http-listener) már nincs szükség.
*   Gyermek munkafolyamatok hívása használ egy [új séma](#child-workflows).

<a name="api-connections"></a>
## <a name="move-tooapi-connections"></a>Helyezze át a tooAPI kapcsolatok

hello legnagyobb változás az, hogy már nem rendelkezik toodeploy API-alkalmazások az Azure-előfizetése, hogy használhassa az API-k. Itt módon hello API-k használhatók:

* Felügyelt API-k
* Az egyéni webes API-k

Mindkét lehetőség némileg eltérően kell kezelni, mert a felügyelet és az üzemeltetési modellek nem egyezik. Ez a modell egyik előnye most már nem korlátozódik az Azure erőforráscsoport üzembe helyezett tooresources. 

### <a name="managed-apis"></a>Felügyelt API-k

A Microsoft kezeli az egyes API-k a nevünkben, például az Office 365, a Salesforce, a Twitter és az FTP. Használhatja az egyes felügyelt API-k-van, például a Bing fordítására, míg más konfigurációs. Ez a konfiguráció elnevezése egy *kapcsolat*.

Például Office 365 használatakor kell létrehoznia az Office 365 bejelentkezési jogkivonatot tartalmaz egy kapcsolatot. Ez a token biztonságosan tárolhassa és, hogy a Logic Apps alkalmazást mindig hívhatja meg az Office 365 API hello frissülnek. Azt is megteheti Ha azt szeretné, hogy tooconnect tooyour SQL vagy az FTP-kiszolgáló, kell létrehoznia, amely rendelkezik a kapcsolati karakterlánc hello kapcsolatot. 

Ez a definíció az e műveletek végrehajtása nevezzük `APIConnection`. Itt látható egy példa, amely behívja az Office 365 toosend egy e-mailt kapcsolatot:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Hello `host` célja a bemeneti adatok része, amely egyedi tooAPI kapcsolatokat, és fonókábel részeket tartalmazza: `api` és `connection`.

Hello `api` hello futásidejű URL-CÍMÉT, amely a felügyeli, ahol API van-e tárolva van. Megtekintheti a rendelkezésre álló összes hello felügyelt API-k hívásával `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

API-t használ, amikor előfordulhat, hogy hello API, vagy előfordulhat, hogy nem rendelkezik ilyennel *kapcsolódási paraméterek* definiálva. Ha hello API nem, nem *kapcsolat* szükséges. Ha hello API, létre kell hoznia egy kapcsolatot. hello létrehozott kapcsolat hello névvel rendelkezik, amelynek választja. Majd hivatkozhat hello neve a hello `connection` belül hello objektum `host` objektum. egy kapcsolat egy erőforráscsoport, a hívást a toocreate:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

A szervezet a következő hello:

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{hello name of hello storage account -- hello set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Felügyelt API-k az Azure Resource Manager sablon telepítése

Létrehozhat egy teljes alkalmazás az Azure Resource Manager-sablonok, mindaddig, amíg az interaktív bejelentkezés nem szükséges.
Bejelentkezési szükség, ha minden hello Azure Resource Manager sablonnal állíthat be, de továbbra is fennáll toovisit hello portál tooauthorize hello kapcsolatok. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/', parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:, Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Ebben a példában, hogy hello kapcsolatok csak olyan erőforrásokat, amelyek az erőforráscsoportban élő látható. Hivatkozó hello felügyelt API-k elérhető tooyou az előfizetésben.

### <a name="your-custom-web-apis"></a>Az egyéni webes API-k

Ha a saját API-k, nem Microsoft által felügyelt is, használja a hello beépített **HTTP** művelet toocall őket. Ideális élményt akkor a Swagger-végpont az API-érdemes felfedi. A végpont lehetővé teszi, hogy a hello Logic App Designer toorender hello bemeneti adatokat, és kiírja az API. A Swagger hello designer is csak jeleníti meg a hello bemenetekhez és kimenetekhez nem átlátszó JSON-objektumként.

Egy példa ábrázoló hello Ez új `metadata.apiDefinitionUrl` tulajdonság:

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

Ha az Azure App Service Web API működteti, a Web API automatikusan hello designer elérhető műveletek hello listájában jelenik meg. Ha nem, vannak toopaste hello URL-címet közvetlenül. hello Swagger-végpont nem hitelesített toobe használható a Logic App Designer hello kell lennie, bár hello API maga bármilyen módszerrel, amely támogatja a Swagger biztonságossá teheti.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Hívás a 2015-08-01. dátumú előnézeti API-alkalmazások telepítése

Ha korábban telepítette az API-alkalmazások, hívása hello hello alkalmazás **HTTP** művelet.

Például, ha a Dropbox toolist fájlokat, használja a **2014 12-01. dátumú előnézeti** verzió sémadefiníciót előfordulhat, hogy hasonlót:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Hello egyenértékű HTTP-művelet ehhez a példához hasonló hogyan hozhat létre, amíg változatlan marad a Logic app-definíciót hello hello paraméterek szakaszban:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata": {
              "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method": "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Útmutató alapján egyenként-ezeket a tulajdonságokat:

| A művelet tulajdonság | Leírás |
| --- | --- |
| `type` |`Http`ahelyett, hogy`APIapp` |
| `metadata.apiDefinitionUrl` |toouse Ez a művelet a Logic App Designer hello hello metaadat-végpontjához, amely értékekből összeállított tartalmazza:`{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Értékekből összeállított:`{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Mindig`POST` |
| `inputs.body` |Azonos toohello API App paraméterek |
| `inputs.authentication` |Azonos toohello API-alkalmazás hitelesítési |

Ez a módszer minden API-alkalmazás műveletre kell működnie. Ne feledje azonban, hogy a korábbi API-alkalmazások támogatása megszűnt. Ezért telepítse át a tooone hello a két előző lehetőségeket, egy felügyelt API-t vagy az egyéni webes API-t üzemeltető.

<a name="foreach"></a>
## <a name="renamed-repeat-tooforeach"></a>Átnevezi a "repeat" too'foreach "

Hello előző séma verziójához, mennyi visszajelzései érkezett, amely **ismételje meg a** zavaró, és nem megfelelő rögzíti, amelyek **ismételje meg a** volt, a-each ciklus. Ennek eredményeképpen azt átnevezett `repeat` túl`foreach`. Például korábban meg kellene írni:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Szeretné most a írni:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

hello függvény `@repeatItem()` volt korábban használt tooreference hello az aktuális elem alatt többször is keresztül. Ez a funkció most egyszerűsített túl`@item()`. 

### <a name="reference-outputs-from-foreach"></a>Hivatkozás kimeneteinek "foreach"

Az egyszerűség kedvéért hello kiírja a `foreach` műveletek az objektum nincs burkolójuk `repeatItems`. Amíg az előző hello kimenete hello `repeat` példa volt:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Most ezeket kimenetének létrehozása a következők:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Korábban tooget toohello törzsét való hivatkozáskor ezek kimenetek hello művelet:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

Most helyette teheti:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

A módosítások hello funkciók `@repeatItem()`, `@repeatBody()`, és `@repeatOutputs()` törlődnek.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Natív HTTP-figyelő

hello HTTP-figyelő most beépített képességei. Így már nem kell toodeploy HTTP Listener API-alkalmazást. Lásd: [kapcsolatos részletek hello toomake a Logic app végpont hívható Itt](../logic-apps/logic-apps-http-endpoint.md). 

A módosítások hello eltávolítása `@accessKeys()` függvénynek, amely azt írni a hello `@listCallbackURL()` függvény az első hello végpont, amikor erre szükség van. Emellett most definiálnia kell legalább egy eseményindító a Logic Apps alkalmazást. Ha azt szeretné, túl`/run` hello munkafolyamat, akkor ezek az eseményindítók egyike: `manual`, `apiConnectionWebhook`, vagy `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Gyermek-munkafolyamatok hívása

Korábban gyermek munkafolyamatok hívása szükséges toohello munkafolyamat első hello hozzáférési jogkivonatot, és a Beillesztés hello token hello logic app-definíciót toocall, ahová a gyermek a munkafolyamatra címen. Hello új sémával hello Logic Apps motor automatikusan létrehozza a futási időben SAS, Ön nem rendelkezik túl illessze be a titkos kulcsok hello definition hello alárendelt munkafolyamat. Például:

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
        },
        "queries": {
            "extrafield": "specialValue"
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "conditions": []
}
```

Egy második fokozása azt hozzáadásakor jogosultságot ad hello gyermek munkafolyamatok teljes hozzáférési toohello bejövő kérelem. Ez azt jelenti, hogy paraméterek átadhatók a hello *lekérdezések* szakasz és hello *fejlécek* objektum és, hogy teljesen definiálhat hello teljes törzsében.

Végezetül a szükséges változtatásokat toohello alárendelt munkafolyamat vannak. Amíg korábban hívhatja az alárendelt munkafolyamat közvetlenül, most meg kell adnia egy eseményindító végpont hello szülő toocall hello munkafolyamata. Általában szeretné beállítani, amely rendelkezik egy eseményindító `manual` írja be, és használja, hogy az eseményindító hello szülő-definícióban. Megjegyzés: hello `host` kifejezetten a tulajdonságnak egy `triggerName` mert mindig meg kell adnia amelynek eseményindítója meghívott.

## <a name="other-changes"></a>Az egyéb módosítások

### <a name="new-queries-property"></a>Új "lekérdezések" tulajdonság

Minden művelettípusok mostantól egy új bemeneti nevű `queries`. A bemeneti strukturált objektum ahelyett, hogy kézzel tooassemble hello karakterlánc lehet.

### <a name="renamed-parse-function-toojson"></a>Átnevezett "parse()" függvény too'json() "

Azt hozzá további tartalomtípus hamarosan, így azt átnevezték hello `parse()` függvény túl`json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Hamarosan: vállalati integrációs API-k

Nem tudunk hello vállalati integrációs API-k, például a AS2 még a felügyelt verziók. Ugyanakkor a meglévő üzembe helyezett BizTalk API-k keresztül hello HTTP-művelet is használhatja. További információkért lásd "A már telepített API-alkalmazások használata" hello a [integrációs terv](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). 
