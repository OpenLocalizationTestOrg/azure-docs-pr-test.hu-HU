---
title: "App Service API Apps metaadatai API feltárásához és kód létrehozásához |} Microsoft Docs"
description: "Ismerje meg, az Azure App Service API Apps használatának Swagger-metaadatok API feltárásához és kód létrehozásához megkönnyítése érdekében."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: c7f8e33a-61cc-486f-89df-4a97dc3c71d4
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: alkarche
ms.openlocfilehash: 800bb9df9b957bec2c80abb3edefbaf354b549ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App Service API Apps metaadatai API feltárásához és kód létrehozásához
Támogatja az [Swagger 2.0](http://swagger.io/) App Service API Apps beépített API-metaadatok. Nem kell használni a Swagger, de ha mégis használja, kihasználhatja az API Apps funkcióinak, amelyek megkönnyítik a felderítés és a fogyasztói.   

## <a name="swagger-endpoint"></a>A swagger-végpont
Megadhat egy végpontot, amely Swagger 2.0 JSON-metaadatok biztosít egy API-alkalmazást, az API-alkalmazás tulajdonsággal. A végpont az API-alkalmazás alap URL-CÍMÉT vagy egy abszolút URL-cím lehet. Abszolút URL-címek kívül az API-alkalmazás mutathat. 

Alsóbb rétegbeli ügyfelek számát (például a Visual Studio kódgenerálás és PowerApps "API hozzáadása" folyamata), az URL-címet kell nyilvánosan elérhető (felhasználó vagy szolgáltatás hitelesítése nem védett). Ez azt jelenti, hogy ha az App Service-hitelesítést használ, és szeretne közzétenni az alkalmazásban, maga az API-definíció, kell használnia, amely lehetővé teszi a névtelen forgalom elérni az API-hitelesítési lehetőséget. További információkért lásd: [hitelesítési és engedélyezési az App Service API Apps](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portál panel
Az a [Azure-portálon](https://portal.azure.com/) a végpont URL-címet is láthatók és módosult a **API-definíció** panelen.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Az Azure Resource Manager tulajdonság
Az API-alkalmazások API definition URL-Címének használatával is konfigurálhatja [erőforrás-kezelő](https://resources.azure.com/) vagy [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md) parancssori eszközökben, például a [Azure PowerShell](/powershell/azureps-cmdlets-docs) és a [Azure CLI](../cli-install-nodejs.md). 

A **erőforrás-kezelő**, keresse fel **előfizetések > {az előfizetés} > resourceGroups > {az erőforráscsoport} > szolgáltatók > Microsoft.Web > Helyek > {a webhely} > konfigurációs > webes**, és látni fogja a `apiDefinition` tulajdonság:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Példa egy Azure Resource Manager-sablon, amely beállítja a `apiDefinition` tulajdonságot, nyissa meg a [azuredeploy.json fájlt a feladatlista mintaalkalmazás](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Keresse meg a sablont, amely hasonlít a JSON-mintát fent látható.

### <a name="default-value"></a>Alapértelmezett érték
API-alkalmazás létrehozása a Visual Studio használatával, az API-definíció végpontot az alap URL-CÍMÉT és API-alkalmazás automatikusan értéke `/swagger/docs/v1`. Ez az alapértelmezett URL-CÍMÉT, amely a [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet csomag által használt kiszolgálására dinamikusan generált Swagger egy ASP.NET Web API-projekt metaadatait. 

## <a name="code-generation"></a>Kódgenerálás
A Swagger integrálása az Azure API apps előnyeit egyik kódok automatikus létrehozása. A generált ügyfélosztályok leegyszerűsítik az API-alkalmazásokat behívó kód megírását.

Visual Studio használatával, vagy a parancssorból generálhat ügyfélkódot az API-alkalmazás. Ügyfél osztályok létrehozása a Visual Studio egy ASP.NET Web API-projekt kapcsolatos információkért lásd: [Bevezetés az API-alkalmazások és ASP.NET](app-service-api-dotnet-get-started.md#codegen). A parancssorból, minden támogatott nyelven menete kapcsolatos információkért lásd: az információs fájl, a [Azure/autorest](https://github.com/azure/autorest) github.com tárházba.

## <a name="next-steps"></a>Következő lépések
További részletes oktatóanyaga, amely végigvezeti Önt a létrehozása, telepítése, és az API-alkalmazások felhasználása: [Ismerkedés az Azure App Service API Apps](app-service-api-dotnet-get-started.md).

Ha az Azure API Management használata API-alkalmazások, Swagger-metaadatok segítségével importálja az API-t az API Management. További információkért lásd: [importálása az Azure API Management műveletek az API-k meghatározása](../api-management/api-management-howto-import-api.md). 

