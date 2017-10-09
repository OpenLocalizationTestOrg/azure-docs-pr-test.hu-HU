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
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a><span data-ttu-id="056b8-103">Az Azure Logic Apps - 2015. augusztus 1 preview sémafrissítések</span><span class="sxs-lookup"><span data-stu-id="056b8-103">Schema updates for Azure Logic Apps - August 1, 2015 preview</span></span>

<span data-ttu-id="056b8-104">Az új séma- és API-t az Azure Logic Apps verzió magában foglalja a fontos fejlesztést tartalmaz, amelyek a logic apps több megbízható és egyszerűbb toouse:</span><span class="sxs-lookup"><span data-stu-id="056b8-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier toouse:</span></span>

*   <span data-ttu-id="056b8-105">Hello **APIApp** művelet típus frissített tooa új [ **APIConnection** ](#api-connections) művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="056b8-105">hello **APIApp** action type is updated tooa new [**APIConnection**](#api-connections) action type.</span></span>
*   <span data-ttu-id="056b8-106">**Ismételje meg a** túl a rendszer átnevezi[**Foreach**](#foreach).</span><span class="sxs-lookup"><span data-stu-id="056b8-106">**Repeat** is renamed too[**Foreach**](#foreach).</span></span>
*   <span data-ttu-id="056b8-107">Hello [ **HTTP-figyelő** API-alkalmazás](#http-listener) már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="056b8-107">hello [**HTTP Listener** API App](#http-listener) is no longer required.</span></span>
*   <span data-ttu-id="056b8-108">Gyermek munkafolyamatok hívása használ egy [új séma](#child-workflows).</span><span class="sxs-lookup"><span data-stu-id="056b8-108">Calling child workflows uses a [new schema](#child-workflows).</span></span>

<a name="api-connections"></a>
## <a name="move-tooapi-connections"></a><span data-ttu-id="056b8-109">Helyezze át a tooAPI kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="056b8-109">Move tooAPI connections</span></span>

<span data-ttu-id="056b8-110">hello legnagyobb változás az, hogy már nem rendelkezik toodeploy API-alkalmazások az Azure-előfizetése, hogy használhassa az API-k.</span><span class="sxs-lookup"><span data-stu-id="056b8-110">hello biggest change is that you no longer have toodeploy API Apps into your Azure subscription so you can use APIs.</span></span> <span data-ttu-id="056b8-111">Itt módon hello API-k használhatók:</span><span class="sxs-lookup"><span data-stu-id="056b8-111">Here are hello ways that you can use APIs:</span></span>

* <span data-ttu-id="056b8-112">Felügyelt API-k</span><span class="sxs-lookup"><span data-stu-id="056b8-112">Managed APIs</span></span>
* <span data-ttu-id="056b8-113">Az egyéni webes API-k</span><span class="sxs-lookup"><span data-stu-id="056b8-113">Your custom Web APIs</span></span>

<span data-ttu-id="056b8-114">Mindkét lehetőség némileg eltérően kell kezelni, mert a felügyelet és az üzemeltetési modellek nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="056b8-114">Each way is handled slightly differently because their management and hosting models are different.</span></span> <span data-ttu-id="056b8-115">Ez a modell egyik előnye most már nem korlátozódik az Azure erőforráscsoport üzembe helyezett tooresources.</span><span class="sxs-lookup"><span data-stu-id="056b8-115">One advantage of this model is you're no longer constrained tooresources that are deployed in your Azure resource group.</span></span> 

### <a name="managed-apis"></a><span data-ttu-id="056b8-116">Felügyelt API-k</span><span class="sxs-lookup"><span data-stu-id="056b8-116">Managed APIs</span></span>

<span data-ttu-id="056b8-117">A Microsoft kezeli az egyes API-k a nevünkben, például az Office 365, a Salesforce, a Twitter és az FTP.</span><span class="sxs-lookup"><span data-stu-id="056b8-117">Microsoft manages some APIs on your behalf, such as Office 365, Salesforce, Twitter, and FTP.</span></span> <span data-ttu-id="056b8-118">Használhatja az egyes felügyelt API-k-van, például a Bing fordítására, míg más konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="056b8-118">You can use some managed APIs as-is, such as Bing Translate, while others require configuration.</span></span> <span data-ttu-id="056b8-119">Ez a konfiguráció elnevezése egy *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="056b8-119">This configuration is called a *connection*.</span></span>

<span data-ttu-id="056b8-120">Például Office 365 használatakor kell létrehoznia az Office 365 bejelentkezési jogkivonatot tartalmaz egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="056b8-120">For example, when you use Office 365, you must create a connection that contains your Office 365 sign-in token.</span></span> <span data-ttu-id="056b8-121">Ez a token biztonságosan tárolhassa és, hogy a Logic Apps alkalmazást mindig hívhatja meg az Office 365 API hello frissülnek.</span><span class="sxs-lookup"><span data-stu-id="056b8-121">This token is securely stored and refreshed so that your logic app can always call hello Office 365 API.</span></span> <span data-ttu-id="056b8-122">Azt is megteheti Ha azt szeretné, hogy tooconnect tooyour SQL vagy az FTP-kiszolgáló, kell létrehoznia, amely rendelkezik a kapcsolati karakterlánc hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="056b8-122">Alternatively, if you want tooconnect tooyour SQL or FTP server, you must create a connection that has hello connection string.</span></span> 

<span data-ttu-id="056b8-123">Ez a definíció az e műveletek végrehajtása nevezzük `APIConnection`.</span><span class="sxs-lookup"><span data-stu-id="056b8-123">In this definition, these actions are called `APIConnection`.</span></span> <span data-ttu-id="056b8-124">Itt látható egy példa, amely behívja az Office 365 toosend egy e-mailt kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="056b8-124">Here is an example of a connection that calls Office 365 toosend an email:</span></span>

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

<span data-ttu-id="056b8-125">Hello `host` célja a bemeneti adatok része, amely egyedi tooAPI kapcsolatokat, és fonókábel részeket tartalmazza: `api` és `connection`.</span><span class="sxs-lookup"><span data-stu-id="056b8-125">hello `host` object is portion of inputs that is unique tooAPI connections, and contains tow parts: `api` and `connection`.</span></span>

<span data-ttu-id="056b8-126">Hello `api` hello futásidejű URL-CÍMÉT, amely a felügyeli, ahol API van-e tárolva van.</span><span class="sxs-lookup"><span data-stu-id="056b8-126">hello `api` has hello runtime URL of where that managed API is hosted.</span></span> <span data-ttu-id="056b8-127">Megtekintheti a rendelkezésre álló összes hello felügyelt API-k hívásával `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span><span class="sxs-lookup"><span data-stu-id="056b8-127">You can see all hello available managed APIs by calling `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span></span>

<span data-ttu-id="056b8-128">API-t használ, amikor előfordulhat, hogy hello API, vagy előfordulhat, hogy nem rendelkezik ilyennel *kapcsolódási paraméterek* definiálva.</span><span class="sxs-lookup"><span data-stu-id="056b8-128">When you use an API, hello API might or might not have any *connection parameters* defined.</span></span> <span data-ttu-id="056b8-129">Ha hello API nem, nem *kapcsolat* szükséges.</span><span class="sxs-lookup"><span data-stu-id="056b8-129">If hello API doesn't, no *connection* is required.</span></span> <span data-ttu-id="056b8-130">Ha hello API, létre kell hoznia egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="056b8-130">If hello API does, you must create a connection.</span></span> <span data-ttu-id="056b8-131">hello létrehozott kapcsolat hello névvel rendelkezik, amelynek választja.</span><span class="sxs-lookup"><span data-stu-id="056b8-131">hello created connection has hello name that you choose.</span></span> <span data-ttu-id="056b8-132">Majd hivatkozhat hello neve a hello `connection` belül hello objektum `host` objektum.</span><span class="sxs-lookup"><span data-stu-id="056b8-132">You then reference hello name in hello `connection` object inside hello `host` object.</span></span> <span data-ttu-id="056b8-133">egy kapcsolat egy erőforráscsoport, a hívást a toocreate:</span><span class="sxs-lookup"><span data-stu-id="056b8-133">toocreate a connection in a resource group, call:</span></span>

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

<span data-ttu-id="056b8-134">A szervezet a következő hello:</span><span class="sxs-lookup"><span data-stu-id="056b8-134">With hello following body:</span></span>

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

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a><span data-ttu-id="056b8-135">Felügyelt API-k az Azure Resource Manager sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="056b8-135">Deploy managed APIs in an Azure Resource Manager template</span></span>

<span data-ttu-id="056b8-136">Létrehozhat egy teljes alkalmazás az Azure Resource Manager-sablonok, mindaddig, amíg az interaktív bejelentkezés nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="056b8-136">You can create a full application in an Azure Resource Manager template as long as interactive sign-in isn't required.</span></span>
<span data-ttu-id="056b8-137">Bejelentkezési szükség, ha minden hello Azure Resource Manager sablonnal állíthat be, de továbbra is fennáll toovisit hello portál tooauthorize hello kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="056b8-137">If sign-in is required, you can set up everything with hello Azure Resource Manager template, but you still have toovisit hello portal tooauthorize hello connections.</span></span> 

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

<span data-ttu-id="056b8-138">Ebben a példában, hogy hello kapcsolatok csak olyan erőforrásokat, amelyek az erőforráscsoportban élő látható.</span><span class="sxs-lookup"><span data-stu-id="056b8-138">You can see in this example that hello connections are just resources that live in your resource group.</span></span> <span data-ttu-id="056b8-139">Hivatkozó hello felügyelt API-k elérhető tooyou az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="056b8-139">They reference hello managed APIs available tooyou in your subscription.</span></span>

### <a name="your-custom-web-apis"></a><span data-ttu-id="056b8-140">Az egyéni webes API-k</span><span class="sxs-lookup"><span data-stu-id="056b8-140">Your custom Web APIs</span></span>

<span data-ttu-id="056b8-141">Ha a saját API-k, nem Microsoft által felügyelt is, használja a hello beépített **HTTP** művelet toocall őket.</span><span class="sxs-lookup"><span data-stu-id="056b8-141">If you use your own APIs, not Microsoft-managed ones, use hello built-in **HTTP** action toocall them.</span></span> <span data-ttu-id="056b8-142">Ideális élményt akkor a Swagger-végpont az API-érdemes felfedi.</span><span class="sxs-lookup"><span data-stu-id="056b8-142">For an ideal experience, you should expose a Swagger endpoint for your API.</span></span> <span data-ttu-id="056b8-143">A végpont lehetővé teszi, hogy a hello Logic App Designer toorender hello bemeneti adatokat, és kiírja az API.</span><span class="sxs-lookup"><span data-stu-id="056b8-143">This endpoint enables hello Logic App Designer toorender hello inputs and outputs for your API.</span></span> <span data-ttu-id="056b8-144">A Swagger hello designer is csak jeleníti meg a hello bemenetekhez és kimenetekhez nem átlátszó JSON-objektumként.</span><span class="sxs-lookup"><span data-stu-id="056b8-144">Without Swagger, hello designer can only show hello inputs and outputs as opaque JSON objects.</span></span>

<span data-ttu-id="056b8-145">Egy példa ábrázoló hello Ez új `metadata.apiDefinitionUrl` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="056b8-145">Here is an example showing hello new `metadata.apiDefinitionUrl` property:</span></span>

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

<span data-ttu-id="056b8-146">Ha az Azure App Service Web API működteti, a Web API automatikusan hello designer elérhető műveletek hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="056b8-146">If you host your Web API on Azure App Service, your Web API automatically appears in hello list of actions available in hello designer.</span></span> <span data-ttu-id="056b8-147">Ha nem, vannak toopaste hello URL-címet közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="056b8-147">If not, you have toopaste in hello URL directly.</span></span> <span data-ttu-id="056b8-148">hello Swagger-végpont nem hitelesített toobe használható a Logic App Designer hello kell lennie, bár hello API maga bármilyen módszerrel, amely támogatja a Swagger biztonságossá teheti.</span><span class="sxs-lookup"><span data-stu-id="056b8-148">hello Swagger endpoint must be unauthenticated toobe usable in hello Logic App Designer, although you can secure hello API itself with whatever methods that Swagger supports.</span></span>

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a><span data-ttu-id="056b8-149">Hívás a 2015-08-01. dátumú előnézeti API-alkalmazások telepítése</span><span class="sxs-lookup"><span data-stu-id="056b8-149">Call deployed API apps with 2015-08-01-preview</span></span>

<span data-ttu-id="056b8-150">Ha korábban telepítette az API-alkalmazások, hívása hello hello alkalmazás **HTTP** művelet.</span><span class="sxs-lookup"><span data-stu-id="056b8-150">If you previously deployed an API App, you can call hello app with hello **HTTP** action.</span></span>

<span data-ttu-id="056b8-151">Például, ha a Dropbox toolist fájlokat, használja a **2014 12-01. dátumú előnézeti** verzió sémadefiníciót előfordulhat, hogy hasonlót:</span><span class="sxs-lookup"><span data-stu-id="056b8-151">For example, if you use Dropbox toolist files, your **2014-12-01-preview** schema version definition might have something like:</span></span>

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

<span data-ttu-id="056b8-152">Hello egyenértékű HTTP-művelet ehhez a példához hasonló hogyan hozhat létre, amíg változatlan marad a Logic app-definíciót hello hello paraméterek szakaszban:</span><span class="sxs-lookup"><span data-stu-id="056b8-152">You can construct hello equivalent HTTP action like this example, while hello parameters section of hello Logic app definition remains unchanged:</span></span>

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

<span data-ttu-id="056b8-153">Útmutató alapján egyenként-ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="056b8-153">Walking through these properties one-by-one:</span></span>

| <span data-ttu-id="056b8-154">A művelet tulajdonság</span><span class="sxs-lookup"><span data-stu-id="056b8-154">Action property</span></span> | <span data-ttu-id="056b8-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="056b8-155">Description</span></span> |
| --- | --- |
| `type` |<span data-ttu-id="056b8-156">`Http`ahelyett, hogy`APIapp`</span><span class="sxs-lookup"><span data-stu-id="056b8-156">`Http` instead of `APIapp`</span></span> |
| `metadata.apiDefinitionUrl` |<span data-ttu-id="056b8-157">toouse Ez a művelet a Logic App Designer hello hello metaadat-végpontjához, amely értékekből összeállított tartalmazza:`{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span><span class="sxs-lookup"><span data-stu-id="056b8-157">toouse this action in hello Logic App Designer, include hello metadata endpoint, which is constructed from: `{api app host.gateway}/api/service/apidef/{last segment of hello api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span></span> |
| `inputs.uri` |<span data-ttu-id="056b8-158">Értékekből összeállított:`{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14`</span><span class="sxs-lookup"><span data-stu-id="056b8-158">Constructed from: `{api app host.gateway}/api/service/invoke/{last segment of hello api app host.id}/{api app operation}?api-version=2015-01-14`</span></span> |
| `inputs.method` |<span data-ttu-id="056b8-159">Mindig`POST`</span><span class="sxs-lookup"><span data-stu-id="056b8-159">Always `POST`</span></span> |
| `inputs.body` |<span data-ttu-id="056b8-160">Azonos toohello API App paraméterek</span><span class="sxs-lookup"><span data-stu-id="056b8-160">Identical toohello API App parameters</span></span> |
| `inputs.authentication` |<span data-ttu-id="056b8-161">Azonos toohello API-alkalmazás hitelesítési</span><span class="sxs-lookup"><span data-stu-id="056b8-161">Identical toohello API App authentication</span></span> |

<span data-ttu-id="056b8-162">Ez a módszer minden API-alkalmazás műveletre kell működnie.</span><span class="sxs-lookup"><span data-stu-id="056b8-162">This approach should work for all API App actions.</span></span> <span data-ttu-id="056b8-163">Ne feledje azonban, hogy a korábbi API-alkalmazások támogatása megszűnt.</span><span class="sxs-lookup"><span data-stu-id="056b8-163">However, remember that these previous API Apps are no longer supported.</span></span> <span data-ttu-id="056b8-164">Ezért telepítse át a tooone hello a két előző lehetőségeket, egy felügyelt API-t vagy az egyéni webes API-t üzemeltető.</span><span class="sxs-lookup"><span data-stu-id="056b8-164">So you should move tooone of hello two other previous options, a managed API or hosting your custom Web API.</span></span>

<a name="foreach"></a>
## <a name="renamed-repeat-tooforeach"></a><span data-ttu-id="056b8-165">Átnevezi a "repeat" too'foreach "</span><span class="sxs-lookup"><span data-stu-id="056b8-165">Renamed 'repeat' too'foreach'</span></span>

<span data-ttu-id="056b8-166">Hello előző séma verziójához, mennyi visszajelzései érkezett, amely **ismételje meg a** zavaró, és nem megfelelő rögzíti, amelyek **ismételje meg a** volt, a-each ciklus.</span><span class="sxs-lookup"><span data-stu-id="056b8-166">For hello previous schema version, we received much customer feedback that **Repeat** was confusing and didn't properly capture that **Repeat** was really a for-each loop.</span></span> <span data-ttu-id="056b8-167">Ennek eredményeképpen azt átnevezett `repeat` túl`foreach`.</span><span class="sxs-lookup"><span data-stu-id="056b8-167">As a result, we have renamed `repeat` too`foreach`.</span></span> <span data-ttu-id="056b8-168">Például korábban meg kellene írni:</span><span class="sxs-lookup"><span data-stu-id="056b8-168">For example, previously you would write:</span></span>

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

<span data-ttu-id="056b8-169">Szeretné most a írni:</span><span class="sxs-lookup"><span data-stu-id="056b8-169">Now you would write:</span></span>

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

<span data-ttu-id="056b8-170">hello függvény `@repeatItem()` volt korábban használt tooreference hello az aktuális elem alatt többször is keresztül.</span><span class="sxs-lookup"><span data-stu-id="056b8-170">hello function `@repeatItem()` was previously used tooreference hello current item being iterated over.</span></span> <span data-ttu-id="056b8-171">Ez a funkció most egyszerűsített túl`@item()`.</span><span class="sxs-lookup"><span data-stu-id="056b8-171">This function is now simplified too`@item()`.</span></span> 

### <a name="reference-outputs-from-foreach"></a><span data-ttu-id="056b8-172">Hivatkozás kimeneteinek "foreach"</span><span class="sxs-lookup"><span data-stu-id="056b8-172">Reference outputs from 'foreach'</span></span>

<span data-ttu-id="056b8-173">Az egyszerűség kedvéért hello kiírja a `foreach` műveletek az objektum nincs burkolójuk `repeatItems`.</span><span class="sxs-lookup"><span data-stu-id="056b8-173">For simplification, hello outputs from `foreach` actions are not wrapped in an object called `repeatItems`.</span></span> <span data-ttu-id="056b8-174">Amíg az előző hello kimenete hello `repeat` példa volt:</span><span class="sxs-lookup"><span data-stu-id="056b8-174">While hello outputs from hello previous `repeat` example were:</span></span>

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

<span data-ttu-id="056b8-175">Most ezeket kimenetének létrehozása a következők:</span><span class="sxs-lookup"><span data-stu-id="056b8-175">Now these outputs are:</span></span>

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

<span data-ttu-id="056b8-176">Korábban tooget toohello törzsét való hivatkozáskor ezek kimenetek hello művelet:</span><span class="sxs-lookup"><span data-stu-id="056b8-176">Previously, tooget toohello body of hello action when referencing these outputs:</span></span>

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

<span data-ttu-id="056b8-177">Most helyette teheti:</span><span class="sxs-lookup"><span data-stu-id="056b8-177">Now you can do instead:</span></span>

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

<span data-ttu-id="056b8-178">A módosítások hello funkciók `@repeatItem()`, `@repeatBody()`, és `@repeatOutputs()` törlődnek.</span><span class="sxs-lookup"><span data-stu-id="056b8-178">With these changes, hello functions `@repeatItem()`, `@repeatBody()`, and `@repeatOutputs()` are removed.</span></span>

<a name="http-listener"></a>
## <a name="native-http-listener"></a><span data-ttu-id="056b8-179">Natív HTTP-figyelő</span><span class="sxs-lookup"><span data-stu-id="056b8-179">Native HTTP listener</span></span>

<span data-ttu-id="056b8-180">hello HTTP-figyelő most beépített képességei.</span><span class="sxs-lookup"><span data-stu-id="056b8-180">hello HTTP Listener capabilities are now built in.</span></span> <span data-ttu-id="056b8-181">Így már nem kell toodeploy HTTP Listener API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="056b8-181">So you no longer need toodeploy an HTTP Listener API App.</span></span> <span data-ttu-id="056b8-182">Lásd: [kapcsolatos részletek hello toomake a Logic app végpont hívható Itt](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="056b8-182">See [hello full details for how toomake your Logic app endpoint callable here](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<span data-ttu-id="056b8-183">A módosítások hello eltávolítása `@accessKeys()` függvénynek, amely azt írni a hello `@listCallbackURL()` függvény az első hello végpont, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="056b8-183">With these changes, we removed hello `@accessKeys()` function, which we replaced with hello `@listCallbackURL()` function for getting hello endpoint when necessary.</span></span> <span data-ttu-id="056b8-184">Emellett most definiálnia kell legalább egy eseményindító a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="056b8-184">Also, you must now define at least one trigger in your logic app.</span></span> <span data-ttu-id="056b8-185">Ha azt szeretné, túl`/run` hello munkafolyamat, akkor ezek az eseményindítók egyike: `manual`, `apiConnectionWebhook`, vagy `httpWebhook`.</span><span class="sxs-lookup"><span data-stu-id="056b8-185">If you want too`/run` hello workflow, you must have one of these triggers: `manual`, `apiConnectionWebhook`, or `httpWebhook`.</span></span>

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a><span data-ttu-id="056b8-186">Gyermek-munkafolyamatok hívása</span><span class="sxs-lookup"><span data-stu-id="056b8-186">Call child workflows</span></span>

<span data-ttu-id="056b8-187">Korábban gyermek munkafolyamatok hívása szükséges toohello munkafolyamat első hello hozzáférési jogkivonatot, és a Beillesztés hello token hello logic app-definíciót toocall, ahová a gyermek a munkafolyamatra címen.</span><span class="sxs-lookup"><span data-stu-id="056b8-187">Previously, calling child workflows required going toohello workflow, getting hello access token, and pasting hello token in hello logic app definition where you want toocall that child workflow.</span></span> <span data-ttu-id="056b8-188">Hello új sémával hello Logic Apps motor automatikusan létrehozza a futási időben SAS, Ön nem rendelkezik túl illessze be a titkos kulcsok hello definition hello alárendelt munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="056b8-188">With hello new schema, hello Logic Apps engine automatically generates a SAS at runtime for hello child workflow so you don't have too paste any secrets into hello definition.</span></span> <span data-ttu-id="056b8-189">Például:</span><span class="sxs-lookup"><span data-stu-id="056b8-189">Here is an example:</span></span>

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

<span data-ttu-id="056b8-190">Egy második fokozása azt hozzáadásakor jogosultságot ad hello gyermek munkafolyamatok teljes hozzáférési toohello bejövő kérelem.</span><span class="sxs-lookup"><span data-stu-id="056b8-190">A second improvement is we are giving hello child workflows full access toohello incoming request.</span></span> <span data-ttu-id="056b8-191">Ez azt jelenti, hogy paraméterek átadhatók a hello *lekérdezések* szakasz és hello *fejlécek* objektum és, hogy teljesen definiálhat hello teljes törzsében.</span><span class="sxs-lookup"><span data-stu-id="056b8-191">That means that you can pass parameters in hello *queries* section and in hello *headers* object and that you can fully define hello entire body.</span></span>

<span data-ttu-id="056b8-192">Végezetül a szükséges változtatásokat toohello alárendelt munkafolyamat vannak.</span><span class="sxs-lookup"><span data-stu-id="056b8-192">Finally, there are required changes toohello child workflow.</span></span> <span data-ttu-id="056b8-193">Amíg korábban hívhatja az alárendelt munkafolyamat közvetlenül, most meg kell adnia egy eseményindító végpont hello szülő toocall hello munkafolyamata.</span><span class="sxs-lookup"><span data-stu-id="056b8-193">While you could previously call a child workflow directly, now you must define a trigger endpoint in hello workflow for hello parent toocall.</span></span> <span data-ttu-id="056b8-194">Általában szeretné beállítani, amely rendelkezik egy eseményindító `manual` írja be, és használja, hogy az eseményindító hello szülő-definícióban.</span><span class="sxs-lookup"><span data-stu-id="056b8-194">Generally, you would add a trigger that has `manual` type, and then use that trigger in hello parent definition.</span></span> <span data-ttu-id="056b8-195">Megjegyzés: hello `host` kifejezetten a tulajdonságnak egy `triggerName` mert mindig meg kell adnia amelynek eseményindítója meghívott.</span><span class="sxs-lookup"><span data-stu-id="056b8-195">Note hello `host` property specifically has a `triggerName` because you must always specify which trigger you are invoking.</span></span>

## <a name="other-changes"></a><span data-ttu-id="056b8-196">Az egyéb módosítások</span><span class="sxs-lookup"><span data-stu-id="056b8-196">Other changes</span></span>

### <a name="new-queries-property"></a><span data-ttu-id="056b8-197">Új "lekérdezések" tulajdonság</span><span class="sxs-lookup"><span data-stu-id="056b8-197">New 'queries' property</span></span>

<span data-ttu-id="056b8-198">Minden művelettípusok mostantól egy új bemeneti nevű `queries`.</span><span class="sxs-lookup"><span data-stu-id="056b8-198">All action types now support a new input called `queries`.</span></span> <span data-ttu-id="056b8-199">A bemeneti strukturált objektum ahelyett, hogy kézzel tooassemble hello karakterlánc lehet.</span><span class="sxs-lookup"><span data-stu-id="056b8-199">This input can be a structured object, rather than you having tooassemble hello string by hand.</span></span>

### <a name="renamed-parse-function-toojson"></a><span data-ttu-id="056b8-200">Átnevezett "parse()" függvény too'json() "</span><span class="sxs-lookup"><span data-stu-id="056b8-200">Renamed 'parse()' function too'json()'</span></span>

<span data-ttu-id="056b8-201">Azt hozzá további tartalomtípus hamarosan, így azt átnevezték hello `parse()` függvény túl`json()`.</span><span class="sxs-lookup"><span data-stu-id="056b8-201">We are adding more content types soon, so we renamed hello `parse()` function too`json()`.</span></span>

## <a name="coming-soon-enterprise-integration-apis"></a><span data-ttu-id="056b8-202">Hamarosan: vállalati integrációs API-k</span><span class="sxs-lookup"><span data-stu-id="056b8-202">Coming soon: Enterprise Integration APIs</span></span>

<span data-ttu-id="056b8-203">Nem tudunk hello vállalati integrációs API-k, például a AS2 még a felügyelt verziók.</span><span class="sxs-lookup"><span data-stu-id="056b8-203">We don't have managed versions yet of hello Enterprise Integration APIs, like AS2.</span></span> <span data-ttu-id="056b8-204">Ugyanakkor a meglévő üzembe helyezett BizTalk API-k keresztül hello HTTP-művelet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="056b8-204">Meanwhile, you can use your existing deployed BizTalk APIs through hello HTTP action.</span></span> <span data-ttu-id="056b8-205">További információkért lásd "A már telepített API-alkalmazások használata" hello a [integrációs terv](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span><span class="sxs-lookup"><span data-stu-id="056b8-205">For details, see "Using your already deployed API apps" in hello [integration roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span></span> 
