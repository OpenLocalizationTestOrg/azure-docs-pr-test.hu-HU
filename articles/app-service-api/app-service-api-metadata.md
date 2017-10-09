---
title: "aaaApp Service API Apps metaadatai API feltárásához és kód létrehozásához |} Microsoft Docs"
description: "Ismerje meg, hogyan használják az Azure App Service API Apps a Swagger-metaadatok toofacilitate API feltárásához és kód létrehozását."
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
ms.openlocfilehash: b27e70b7dd6bd97f5b0b490b496320befe7442c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>App Service API Apps metaadatai API feltárásához és kód létrehozásához
Támogatja az [Swagger 2.0](http://swagger.io/) App Service API Apps beépített API-metaadatok. Nincs toouse Swagger, de ha mégis használja, kihasználhatja az API Apps funkcióinak, amelyek megkönnyítik a felderítés és a fogyasztói.   

## <a name="swagger-endpoint"></a>A swagger-végpont
Megadhat egy végpontot, amely Swagger 2.0 JSON-metaadatok API-alkalmazás egy olyan tulajdonságban hello API-alkalmazás biztosítja. hello végpont relatív toohello alap URL-cím hello API-alkalmazás vagy egy abszolút URL-cím lehet. Abszolút URL-címek kívül hello API-alkalmazás mutathat. 

Alsóbb rétegbeli ügyfelek számát (például a Visual Studio kódgenerálás és PowerApps "API hozzáadása" folyamata), hello URL-címet kell nyilvánosan elérhető (felhasználó vagy szolgáltatás hitelesítése nem védett). Ez azt jelenti, hogy ha App Service-hitelesítést használ, és szeretné, hogy maga az alkalmazáson belül tooexpose hello API-definíció, van szüksége, amely lehetővé teszi a névtelen forgalom tooreach az API-toouse hitelesítési lehetőséget. További információkért lásd: [hitelesítési és engedélyezési az App Service API Apps](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portál panel
A hello [Azure-portálon](https://portal.azure.com/) végpont URL-címet is láthatók és módosítja a hello hello **API-definíció** panelen.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Az Azure Resource Manager tulajdonság
Az API-alkalmazás hello API definition URL-cím használatával is konfigurálhatja [erőforrás-kezelő](https://resources.azure.com/) vagy [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md) parancssori eszközökben, például a [Azure PowerShell](/powershell/azureps-cmdlets-docs)és hello [Azure CLI](../cli-install-nodejs.md). 

A **erőforrás-kezelő**, lépjen túl**előfizetések > {az előfizetés} > resourceGroups > {az erőforráscsoport} > szolgáltatók > Microsoft.Web > Helyek > {a webhely} > konfigurációs > webes**, és látni fogja, hello `apiDefinition` tulajdonság:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Egy Azure Resource Manager-sablon, amely beállítja a hello példát `apiDefinition` tulajdonságot, nyissa meg hello [hello feladatlista mintaalkalmazás azuredeploy.json fájlt](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Hello sablonból, amely a fent látható hello JSON-mintát néz hello szakaszban találja.

### <a name="default-value"></a>Alapértelmezett érték
Visual Studio toocreate API-alkalmazás használatakor hello API definition endpoint értéke automatikusan toohello alap URL-CÍMÉT hello API app plusz `/swagger/docs/v1`. Ez az hello alapértelmezett URL-CÍMÉT, hogy hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-csomag tooserve dinamikusan generált Swagger-metaadatokat használ egy ASP.NET Web API-projektet. 

## <a name="code-generation"></a>Kódgenerálás
A Swagger integrálása az Azure API apps előnyeit hello egyik kódok automatikus létrehozása. A generált révén könnyebben toowrite kódot, amely hívja az API-alkalmazást.

Visual Studio használatával, vagy a parancssorból hello ügyfélkódot az API-alkalmazást hozhat létre. Hogyan toogenerate ügyfél osztályok, a Visual Studio egy ASP.NET Web API-projekt kapcsolatos információkért lásd: [Bevezetés az API-alkalmazások és ASP.NET](app-service-api-dotnet-get-started.md#codegen). Hogyan toodo hello parancs le sor minden támogatott nyelven kapcsolatos információkért lásd: hello információs fájl a hello [Azure/autorest](https://github.com/azure/autorest) github.com tárházba.

## <a name="next-steps"></a>Következő lépések
További részletes oktatóanyaga, amely végigvezeti Önt a létrehozása, telepítése, és az API-alkalmazások felhasználása: [Ismerkedés az Azure App Service API Apps](app-service-api-dotnet-get-started.md).

Ha az Azure API Management használata API-alkalmazások, is használhatja a Swagger-metaadatok tooimport az API-API Management. További információkért lásd: [hogyan tooimport hello műveleteket az Azure API Management az API-k meghatározása](../api-management/api-management-howto-import-api.md). 

