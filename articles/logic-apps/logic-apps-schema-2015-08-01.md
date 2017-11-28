---
title: "Séma frissíti 1 – 2015. augusztus előzetes verzió – Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 35d7a56d5607dcc18a4407c65b92962d3d0dcd1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a><span data-ttu-id="f0554-103">Az Azure Logic Apps - 2015. augusztus 1 preview sémafrissítések</span><span class="sxs-lookup"><span data-stu-id="f0554-103">Schema updates for Azure Logic Apps - August 1, 2015 preview</span></span>

<span data-ttu-id="f0554-104">Az új séma- és API-t az Azure Logic Apps verzió magában foglalja a fontos fejlesztést tartalmaz, amelyek a logic apps megbízhatóbb és könnyebben használható:</span><span class="sxs-lookup"><span data-stu-id="f0554-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier to use:</span></span>

*   <span data-ttu-id="f0554-105">A **APIApp** művelettípus frissítése, hogy egy új [ **APIConnection** ](#api-connections) művelet típusa.</span><span class="sxs-lookup"><span data-stu-id="f0554-105">The **APIApp** action type is updated to a new [**APIConnection**](#api-connections) action type.</span></span>
*   <span data-ttu-id="f0554-106">**Ismételje meg a** neve módosult az [ **Foreach**](#foreach).</span><span class="sxs-lookup"><span data-stu-id="f0554-106">**Repeat** is renamed to [**Foreach**](#foreach).</span></span>
*   <span data-ttu-id="f0554-107">A [ **HTTP-figyelő** API-alkalmazás](#http-listener) már nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="f0554-107">The [**HTTP Listener** API App](#http-listener) is no longer required.</span></span>
*   <span data-ttu-id="f0554-108">Gyermek munkafolyamatok hívása használ egy [új séma](#child-workflows).</span><span class="sxs-lookup"><span data-stu-id="f0554-108">Calling child workflows uses a [new schema](#child-workflows).</span></span>

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a><span data-ttu-id="f0554-109">API-kapcsolatok áthelyezése</span><span class="sxs-lookup"><span data-stu-id="f0554-109">Move to API connections</span></span>

<span data-ttu-id="f0554-110">A legnagyobb módosítása, hogy már nem kell API-alkalmazások üzembe helyezés az Azure-előfizetéssel, így API-kat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f0554-110">The biggest change is that you no longer have to deploy API Apps into your Azure subscription so you can use APIs.</span></span> <span data-ttu-id="f0554-111">Az alábbiakban a módszereket, hogy API-kat:</span><span class="sxs-lookup"><span data-stu-id="f0554-111">Here are the ways that you can use APIs:</span></span>

* <span data-ttu-id="f0554-112">Felügyelt API-k</span><span class="sxs-lookup"><span data-stu-id="f0554-112">Managed APIs</span></span>
* <span data-ttu-id="f0554-113">Az egyéni webes API-k</span><span class="sxs-lookup"><span data-stu-id="f0554-113">Your custom Web APIs</span></span>

<span data-ttu-id="f0554-114">Mindkét lehetőség némileg eltérően kell kezelni, mert a felügyelet és az üzemeltetési modellek nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="f0554-114">Each way is handled slightly differently because their management and hosting models are different.</span></span> <span data-ttu-id="f0554-115">Ez a modell egyik előnye, még az Azure erőforráscsoportban már nem korlátozódik telepített erőforrások esetén.</span><span class="sxs-lookup"><span data-stu-id="f0554-115">One advantage of this model is you're no longer constrained to resources that are deployed in your Azure resource group.</span></span> 

### <a name="managed-apis"></a><span data-ttu-id="f0554-116">Felügyelt API-k</span><span class="sxs-lookup"><span data-stu-id="f0554-116">Managed APIs</span></span>

<span data-ttu-id="f0554-117">A Microsoft kezeli az egyes API-k a nevünkben, például az Office 365, a Salesforce, a Twitter és az FTP.</span><span class="sxs-lookup"><span data-stu-id="f0554-117">Microsoft manages some APIs on your behalf, such as Office 365, Salesforce, Twitter, and FTP.</span></span> <span data-ttu-id="f0554-118">Használhatja az egyes felügyelt API-k-van, például a Bing fordítására, míg más konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="f0554-118">You can use some managed APIs as-is, such as Bing Translate, while others require configuration.</span></span> <span data-ttu-id="f0554-119">Ez a konfiguráció elnevezése egy *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="f0554-119">This configuration is called a *connection*.</span></span>

<span data-ttu-id="f0554-120">Például Office 365 használatakor kell létrehoznia az Office 365 bejelentkezési jogkivonatot tartalmaz egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f0554-120">For example, when you use Office 365, you must create a connection that contains your Office 365 sign-in token.</span></span> <span data-ttu-id="f0554-121">Ez a token biztonságosan tárolhassa és frissítése, hogy a Logic Apps alkalmazást mindig meghívhatja az Office 365 API-t.</span><span class="sxs-lookup"><span data-stu-id="f0554-121">This token is securely stored and refreshed so that your logic app can always call the Office 365 API.</span></span> <span data-ttu-id="f0554-122">Azt is megteheti Ha azt szeretné, az SQL- vagy FTP-kiszolgálóhoz való csatlakozáshoz, kell létrehoznia, amely rendelkezik a kapcsolati karakterlánc kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f0554-122">Alternatively, if you want to connect to your SQL or FTP server, you must create a connection that has the connection string.</span></span> 

<span data-ttu-id="f0554-123">Ez a definíció az e műveletek végrehajtása nevezzük `APIConnection`.</span><span class="sxs-lookup"><span data-stu-id="f0554-123">In this definition, these actions are called `APIConnection`.</span></span> <span data-ttu-id="f0554-124">Itt látható egy példa, amely behívja az Office 365 e-mailt küldhet kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="f0554-124">Here is an example of a connection that calls Office 365 to send an email:</span></span>

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

<span data-ttu-id="f0554-125">A `host` célja a bemeneti adatok része, amely egyedi API kapcsolatok, és fonókábel részeket tartalmazza: `api` és `connection`.</span><span class="sxs-lookup"><span data-stu-id="f0554-125">The `host` object is portion of inputs that is unique to API connections, and contains tow parts: `api` and `connection`.</span></span>

<span data-ttu-id="f0554-126">A `api` a futtatókörnyezet URL-CÍMÉT, amely a felügyeli, ahol API van-e tárolva van.</span><span class="sxs-lookup"><span data-stu-id="f0554-126">The `api` has the runtime URL of where that managed API is hosted.</span></span> <span data-ttu-id="f0554-127">Megtekintheti az összes elérhető felügyelt API-k hívásával `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span><span class="sxs-lookup"><span data-stu-id="f0554-127">You can see all the available managed APIs by calling `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.</span></span>

<span data-ttu-id="f0554-128">API-t használ, amikor előfordulhat, hogy az API-t, vagy előfordulhat, hogy nem rendelkezik ilyennel *kapcsolódási paraméterek* definiálva.</span><span class="sxs-lookup"><span data-stu-id="f0554-128">When you use an API, the API might or might not have any *connection parameters* defined.</span></span> <span data-ttu-id="f0554-129">Ha nem az API-t, nem *kapcsolat* szükséges.</span><span class="sxs-lookup"><span data-stu-id="f0554-129">If the API doesn't, no *connection* is required.</span></span> <span data-ttu-id="f0554-130">Ha az API-t, létre kell hoznia egy kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f0554-130">If the API does, you must create a connection.</span></span> <span data-ttu-id="f0554-131">A kapcsolat létrehozása a nevet, amely meg van.</span><span class="sxs-lookup"><span data-stu-id="f0554-131">The created connection has the name that you choose.</span></span> <span data-ttu-id="f0554-132">Majd hivatkozzon a neve a `connection` objektum belül a `host` objektum.</span><span class="sxs-lookup"><span data-stu-id="f0554-132">You then reference the name in the `connection` object inside the `host` object.</span></span> <span data-ttu-id="f0554-133">Kapcsolat létrehozása egy erőforráscsoport, hívja meg:</span><span class="sxs-lookup"><span data-stu-id="f0554-133">To create a connection in a resource group, call:</span></span>

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

<span data-ttu-id="f0554-134">A következő szervezethez:</span><span class="sxs-lookup"><span data-stu-id="f0554-134">With the following body:</span></span>

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a><span data-ttu-id="f0554-135">Felügyelt API-k az Azure Resource Manager sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="f0554-135">Deploy managed APIs in an Azure Resource Manager template</span></span>

<span data-ttu-id="f0554-136">Létrehozhat egy teljes alkalmazás az Azure Resource Manager-sablonok, mindaddig, amíg az interaktív bejelentkezés nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f0554-136">You can create a full application in an Azure Resource Manager template as long as interactive sign-in isn't required.</span></span>
<span data-ttu-id="f0554-137">Bejelentkezési szükség, ha minden, az Azure Resource Manager sablon állíthat be, de továbbra is meg kell keresse fel a portálra, ahol a kapcsolatok engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0554-137">If sign-in is required, you can set up everything with the Azure Resource Manager template, but you still have to visit the portal to authorize the connections.</span></span> 

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

<span data-ttu-id="f0554-138">Ebben a példában, hogy a kapcsolatok csak olyan erőforrásokat, amelyek az erőforráscsoportban élő látható.</span><span class="sxs-lookup"><span data-stu-id="f0554-138">You can see in this example that the connections are just resources that live in your resource group.</span></span> <span data-ttu-id="f0554-139">A felügyelt API-k az előfizetésben elérhető hivatkozó.</span><span class="sxs-lookup"><span data-stu-id="f0554-139">They reference the managed APIs available to you in your subscription.</span></span>

### <a name="your-custom-web-apis"></a><span data-ttu-id="f0554-140">Az egyéni webes API-k</span><span class="sxs-lookup"><span data-stu-id="f0554-140">Your custom Web APIs</span></span>

<span data-ttu-id="f0554-141">Ha a saját API-k, nem Microsoft által felügyelt is, használja a beépített **HTTP** művelet hívására őket.</span><span class="sxs-lookup"><span data-stu-id="f0554-141">If you use your own APIs, not Microsoft-managed ones, use the built-in **HTTP** action to call them.</span></span> <span data-ttu-id="f0554-142">Ideális élményt akkor a Swagger-végpont az API-érdemes felfedi.</span><span class="sxs-lookup"><span data-stu-id="f0554-142">For an ideal experience, you should expose a Swagger endpoint for your API.</span></span> <span data-ttu-id="f0554-143">Ez a végpont lehetővé teszi, hogy a Logic App Designer lehet megjeleníteni az adatokat, és kiírja az API.</span><span class="sxs-lookup"><span data-stu-id="f0554-143">This endpoint enables the Logic App Designer to render the inputs and outputs for your API.</span></span> <span data-ttu-id="f0554-144">Swagger a Tervező is csak jeleníti meg a be- és kimenetekkel nem átlátszó JSON-objektumként.</span><span class="sxs-lookup"><span data-stu-id="f0554-144">Without Swagger, the designer can only show the inputs and outputs as opaque JSON objects.</span></span>

<span data-ttu-id="f0554-145">Az új bemutató példa `metadata.apiDefinitionUrl` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="f0554-145">Here is an example showing the new `metadata.apiDefinitionUrl` property:</span></span>

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

<span data-ttu-id="f0554-146">Ha az Azure App Service Web API működteti, a Web API automatikusan megjelenik a Tervező elérhető műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="f0554-146">If you host your Web API on Azure App Service, your Web API automatically appears in the list of actions available in the designer.</span></span> <span data-ttu-id="f0554-147">Ha nem, illessze be az URL-címet közvetlenül kell.</span><span class="sxs-lookup"><span data-stu-id="f0554-147">If not, you have to paste in the URL directly.</span></span> <span data-ttu-id="f0554-148">A Swagger-végpont kell lennie a hitelesítés nélküli is használható a Logic App Designer, habár maga a API bármilyen módszerrel, amely támogatja a Swagger biztonságossá teheti.</span><span class="sxs-lookup"><span data-stu-id="f0554-148">The Swagger endpoint must be unauthenticated to be usable in the Logic App Designer, although you can secure the API itself with whatever methods that Swagger supports.</span></span>

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a><span data-ttu-id="f0554-149">Hívás a 2015-08-01. dátumú előnézeti API-alkalmazások telepítése</span><span class="sxs-lookup"><span data-stu-id="f0554-149">Call deployed API apps with 2015-08-01-preview</span></span>

<span data-ttu-id="f0554-150">Ha korábban telepítette az API-alkalmazások, hívása az alkalmazás és a **HTTP** művelet.</span><span class="sxs-lookup"><span data-stu-id="f0554-150">If you previously deployed an API App, you can call the app with the **HTTP** action.</span></span>

<span data-ttu-id="f0554-151">Ha a Dropbox segítségével tulajdonságlista-fájlok, például a **2014 12-01. dátumú előnézeti** verzió sémadefiníciót előfordulhat, hogy hasonlót:</span><span class="sxs-lookup"><span data-stu-id="f0554-151">For example, if you use Dropbox to list files, your **2014-12-01-preview** schema version definition might have something like:</span></span>

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

<span data-ttu-id="f0554-152">Ehhez a példához hasonló egyenértékű HTTP-művelet hogyan hozhat létre, amíg változatlan marad a Logic app-definíciót a Paraméterek szakaszban:</span><span class="sxs-lookup"><span data-stu-id="f0554-152">You can construct the equivalent HTTP action like this example, while the parameters section of the Logic app definition remains unchanged:</span></span>

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

<span data-ttu-id="f0554-153">Útmutató alapján egyenként-ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="f0554-153">Walking through these properties one-by-one:</span></span>

| <span data-ttu-id="f0554-154">A művelet tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f0554-154">Action property</span></span> | <span data-ttu-id="f0554-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="f0554-155">Description</span></span> |
| --- | --- |
| `type` |<span data-ttu-id="f0554-156">`Http`ahelyett, hogy`APIapp`</span><span class="sxs-lookup"><span data-stu-id="f0554-156">`Http` instead of `APIapp`</span></span> |
| `metadata.apiDefinitionUrl` |<span data-ttu-id="f0554-157">Ez a művelet a Logic App Designer használatához a metaadat-végpontjához, amely értékekből összeállított a következők:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span><span class="sxs-lookup"><span data-stu-id="f0554-157">To use this action in the Logic App Designer, include the metadata endpoint, which is constructed from: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard`</span></span> |
| `inputs.uri` |<span data-ttu-id="f0554-158">Értékekből összeállított:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14`</span><span class="sxs-lookup"><span data-stu-id="f0554-158">Constructed from: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14`</span></span> |
| `inputs.method` |<span data-ttu-id="f0554-159">Mindig`POST`</span><span class="sxs-lookup"><span data-stu-id="f0554-159">Always `POST`</span></span> |
| `inputs.body` |<span data-ttu-id="f0554-160">Az API-alkalmazás paraméterek azonos</span><span class="sxs-lookup"><span data-stu-id="f0554-160">Identical to the API App parameters</span></span> |
| `inputs.authentication` |<span data-ttu-id="f0554-161">Az API-alkalmazás hitelesítési azonos</span><span class="sxs-lookup"><span data-stu-id="f0554-161">Identical to the API App authentication</span></span> |

<span data-ttu-id="f0554-162">Ez a módszer minden API-alkalmazás műveletre kell működnie.</span><span class="sxs-lookup"><span data-stu-id="f0554-162">This approach should work for all API App actions.</span></span> <span data-ttu-id="f0554-163">Ne feledje azonban, hogy a korábbi API-alkalmazások támogatása megszűnt.</span><span class="sxs-lookup"><span data-stu-id="f0554-163">However, remember that these previous API Apps are no longer supported.</span></span> <span data-ttu-id="f0554-164">Ezért át kell helyezni a két előző lehetőségeket, egy felügyelt API-t vagy az egyéni webes API-t üzemeltető.</span><span class="sxs-lookup"><span data-stu-id="f0554-164">So you should move to one of the two other previous options, a managed API or hosting your custom Web API.</span></span>

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a><span data-ttu-id="f0554-165">Átnevezni a "repeat", "foreach"</span><span class="sxs-lookup"><span data-stu-id="f0554-165">Renamed 'repeat' to 'foreach'</span></span>

<span data-ttu-id="f0554-166">A séma korábbi verziójához sok visszajelzései érkezett, amely **ismételje meg a** zavaró, és nem megfelelő rögzíti, amelyek **ismételje meg a** volt, a-each ciklus.</span><span class="sxs-lookup"><span data-stu-id="f0554-166">For the previous schema version, we received much customer feedback that **Repeat** was confusing and didn't properly capture that **Repeat** was really a for-each loop.</span></span> <span data-ttu-id="f0554-167">Ennek eredményeképpen azt átnevezett `repeat` való `foreach`.</span><span class="sxs-lookup"><span data-stu-id="f0554-167">As a result, we have renamed `repeat` to `foreach`.</span></span> <span data-ttu-id="f0554-168">Például korábban meg kellene írni:</span><span class="sxs-lookup"><span data-stu-id="f0554-168">For example, previously you would write:</span></span>

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

<span data-ttu-id="f0554-169">Szeretné most a írni:</span><span class="sxs-lookup"><span data-stu-id="f0554-169">Now you would write:</span></span>

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

<span data-ttu-id="f0554-170">A függvény `@repeatItem()` korábban használt hivatkozhasson rá az az aktuális elem alatt többször is keresztül.</span><span class="sxs-lookup"><span data-stu-id="f0554-170">The function `@repeatItem()` was previously used to reference the current item being iterated over.</span></span> <span data-ttu-id="f0554-171">Ez a funkció most egyszerűsítve `@item()`.</span><span class="sxs-lookup"><span data-stu-id="f0554-171">This function is now simplified to `@item()`.</span></span> 

### <a name="reference-outputs-from-foreach"></a><span data-ttu-id="f0554-172">Hivatkozás kimeneteinek "foreach"</span><span class="sxs-lookup"><span data-stu-id="f0554-172">Reference outputs from 'foreach'</span></span>

<span data-ttu-id="f0554-173">Az egyszerűség kedvéért a kimeneteinek `foreach` műveletek az objektum nincs burkolójuk `repeatItems`.</span><span class="sxs-lookup"><span data-stu-id="f0554-173">For simplification, the outputs from `foreach` actions are not wrapped in an object called `repeatItems`.</span></span> <span data-ttu-id="f0554-174">Miközben az előző kimenetek `repeat` példa volt:</span><span class="sxs-lookup"><span data-stu-id="f0554-174">While the outputs from the previous `repeat` example were:</span></span>

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

<span data-ttu-id="f0554-175">Most ezeket kimenetének létrehozása a következők:</span><span class="sxs-lookup"><span data-stu-id="f0554-175">Now these outputs are:</span></span>

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

<span data-ttu-id="f0554-176">Korábban való hivatkozáskor ezek kimenetek a szervezet művelet eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="f0554-176">Previously, to get to the body of the action when referencing these outputs:</span></span>

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

<span data-ttu-id="f0554-177">Most helyette teheti:</span><span class="sxs-lookup"><span data-stu-id="f0554-177">Now you can do instead:</span></span>

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

<span data-ttu-id="f0554-178">A módosítások a funkciók `@repeatItem()`, `@repeatBody()`, és `@repeatOutputs()` törlődnek.</span><span class="sxs-lookup"><span data-stu-id="f0554-178">With these changes, the functions `@repeatItem()`, `@repeatBody()`, and `@repeatOutputs()` are removed.</span></span>

<a name="http-listener"></a>
## <a name="native-http-listener"></a><span data-ttu-id="f0554-179">Natív HTTP-figyelő</span><span class="sxs-lookup"><span data-stu-id="f0554-179">Native HTTP listener</span></span>

<span data-ttu-id="f0554-180">A HTTP-figyelő képességek most beépített.</span><span class="sxs-lookup"><span data-stu-id="f0554-180">The HTTP Listener capabilities are now built in.</span></span> <span data-ttu-id="f0554-181">Így már nem kell HTTP Listener API-alkalmazások telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0554-181">So you no longer need to deploy an HTTP Listener API App.</span></span> <span data-ttu-id="f0554-182">Lásd: [hogyan végezheti el a Logic app végpont hívható itt teljes részleteit](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="f0554-182">See [the full details for how to make your Logic app endpoint callable here](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<span data-ttu-id="f0554-183">A módosítások eltávolítása a `@accessKeys()` függvénynek, amely azt írni a a `@listCallbackURL()` függvény az első a végpont, amikor erre szükség van.</span><span class="sxs-lookup"><span data-stu-id="f0554-183">With these changes, we removed the `@accessKeys()` function, which we replaced with the `@listCallbackURL()` function for getting the endpoint when necessary.</span></span> <span data-ttu-id="f0554-184">Emellett most definiálnia kell legalább egy eseményindító a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f0554-184">Also, you must now define at least one trigger in your logic app.</span></span> <span data-ttu-id="f0554-185">Ha azt szeretné, hogy `/run` a munkafolyamat, ezek az eseményindítók egyikét kell: `manual`, `apiConnectionWebhook`, vagy `httpWebhook`.</span><span class="sxs-lookup"><span data-stu-id="f0554-185">If you want to `/run` the workflow, you must have one of these triggers: `manual`, `apiConnectionWebhook`, or `httpWebhook`.</span></span>

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a><span data-ttu-id="f0554-186">Gyermek-munkafolyamatok hívása</span><span class="sxs-lookup"><span data-stu-id="f0554-186">Call child workflows</span></span>

<span data-ttu-id="f0554-187">Korábban gyermek munkafolyamatok hívása szükség lesz a munkafolyamat, a hozzáférési jogkivonat beolvasása és a token beilleszteni a logic app-definíciót hívható meg, hogy alárendelt munkafolyamat helyét a.</span><span class="sxs-lookup"><span data-stu-id="f0554-187">Previously, calling child workflows required going to the workflow, getting the access token, and pasting the token in the logic app definition where you want to call that child workflow.</span></span> <span data-ttu-id="f0554-188">Az új sémával a Logic Apps-motor automatikusan létrehozza a gyermek munkafolyamat futásidőben SAS-kód, nem kell a titkos kulcsok illessze be a definíciót.</span><span class="sxs-lookup"><span data-stu-id="f0554-188">With the new schema, the Logic Apps engine automatically generates a SAS at runtime for the child workflow so you don't have to paste any secrets into the definition.</span></span> <span data-ttu-id="f0554-189">Például:</span><span class="sxs-lookup"><span data-stu-id="f0554-189">Here is an example:</span></span>

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

<span data-ttu-id="f0554-190">Egy második fokozása azt hozzáadásakor jogosultságot ad a gyermek-munkafolyamatok teljes hozzáférés a bejövő kérelemre.</span><span class="sxs-lookup"><span data-stu-id="f0554-190">A second improvement is we are giving the child workflows full access to the incoming request.</span></span> <span data-ttu-id="f0554-191">Ez azt jelenti, hogy a paraméterek átadhatók a *lekérdezések* szakasz és a a *fejlécek* objektum és, hogy a teljes szervezet teljesen adhat meg.</span><span class="sxs-lookup"><span data-stu-id="f0554-191">That means that you can pass parameters in the *queries* section and in the *headers* object and that you can fully define the entire body.</span></span>

<span data-ttu-id="f0554-192">Végezetül a gyermek munkafolyamat szükséges módosítások vannak.</span><span class="sxs-lookup"><span data-stu-id="f0554-192">Finally, there are required changes to the child workflow.</span></span> <span data-ttu-id="f0554-193">Amíg korábban hívhatja az alárendelt munkafolyamat közvetlenül, most meg kell adnia egy eseményindító végpont a szülő hívni a munkafolyamatban.</span><span class="sxs-lookup"><span data-stu-id="f0554-193">While you could previously call a child workflow directly, now you must define a trigger endpoint in the workflow for the parent to call.</span></span> <span data-ttu-id="f0554-194">Általában szeretné beállítani, amely rendelkezik egy eseményindító `manual` írja be, és a szülő-definícióban, hogy az eseményindító használja.</span><span class="sxs-lookup"><span data-stu-id="f0554-194">Generally, you would add a trigger that has `manual` type, and then use that trigger in the parent definition.</span></span> <span data-ttu-id="f0554-195">Megjegyzés: a `host` kifejezetten a tulajdonságnak egy `triggerName` mert mindig meg kell adnia amelynek eseményindítója meghívott.</span><span class="sxs-lookup"><span data-stu-id="f0554-195">Note the `host` property specifically has a `triggerName` because you must always specify which trigger you are invoking.</span></span>

## <a name="other-changes"></a><span data-ttu-id="f0554-196">Az egyéb módosítások</span><span class="sxs-lookup"><span data-stu-id="f0554-196">Other changes</span></span>

### <a name="new-queries-property"></a><span data-ttu-id="f0554-197">Új "lekérdezések" tulajdonság</span><span class="sxs-lookup"><span data-stu-id="f0554-197">New 'queries' property</span></span>

<span data-ttu-id="f0554-198">Minden művelettípusok mostantól egy új bemeneti nevű `queries`.</span><span class="sxs-lookup"><span data-stu-id="f0554-198">All action types now support a new input called `queries`.</span></span> <span data-ttu-id="f0554-199">A bemeneti strukturált objektum ahelyett, hogy át kellene kézzel állítsa össze a karakterlánc lehet.</span><span class="sxs-lookup"><span data-stu-id="f0554-199">This input can be a structured object, rather than you having to assemble the string by hand.</span></span>

### <a name="renamed-parse-function-to-json"></a><span data-ttu-id="f0554-200">Átnevezett "parse()" függvény "json()"</span><span class="sxs-lookup"><span data-stu-id="f0554-200">Renamed 'parse()' function to 'json()'</span></span>

<span data-ttu-id="f0554-201">Azt ad hozzá több tartalomtípus hamarosan, így azt átnevezték a `parse()` függvény `json()`.</span><span class="sxs-lookup"><span data-stu-id="f0554-201">We are adding more content types soon, so we renamed the `parse()` function to `json()`.</span></span>

## <a name="coming-soon-enterprise-integration-apis"></a><span data-ttu-id="f0554-202">Hamarosan: vállalati integrációs API-k</span><span class="sxs-lookup"><span data-stu-id="f0554-202">Coming soon: Enterprise Integration APIs</span></span>

<span data-ttu-id="f0554-203">Felügyelt verziói még a vállalati integrációs API-k, például a AS2 jelenleg nem áll.</span><span class="sxs-lookup"><span data-stu-id="f0554-203">We don't have managed versions yet of the Enterprise Integration APIs, like AS2.</span></span> <span data-ttu-id="f0554-204">Ugyanakkor a meglévő üzembe helyezett BizTalk API-k használatával a HTTP-művelet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f0554-204">Meanwhile, you can use your existing deployed BizTalk APIs through the HTTP action.</span></span> <span data-ttu-id="f0554-205">További információkért lásd "A már telepített API-alkalmazások használata" a a [integrációs terv](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span><span class="sxs-lookup"><span data-stu-id="f0554-205">For details, see "Using your already deployed API apps" in the [integration roadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/).</span></span> 
