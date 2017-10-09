---
title: "az Event Hubs-kezelési kódtárakat aaaAzure |} Microsoft Docs"
description: "Az Event Hubs-névterek és az entitásokat a .NET-kezelése"
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b7db0077f6f31397ae46e926c3c28630a157824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-management-libraries"></a>Event Hubs kezelési kódtárakat

az Event Hubs-kezelési kódtárakat hello dinamikusan építhető ki az Event Hubs-névterek és az entitásokat. Ez lehetővé teszi, hogy üzenetkezelési forgatókönyveket, és a komplex központi telepítései, hogy milyen entitások tooprovision programozott módon meghatározhatja. Ezek a kódtárak jelenleg érhetők el a .NET-hez.

## <a name="supported-functionality"></a>Támogatott funkciók

* Namespace létrehozási, frissítési, törlési
* Event Hubs létrehozási, frissítési, törlési
* Felhasználói csoport létrehozása, frissítés, törlés

## <a name="prerequisites"></a>Előfeltételek

a tooget használatának hello Event Hubs kezelési kódtárakat, hitelesítenie kell az Azure Active Directory (AAD). Aad-ben van szükség, hogy hitelesítse magát egy egyszerű szolgáltatást, amely hozzáférési tooyour biztosít az Azure-erőforrások. Egyszerű szolgáltatás létrehozása kapcsolatos információkért tekintse meg a következő cikkeket:  

* [Hello Azure portál toocreate Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás használata](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Ezek az oktatóanyagok biztosítanak egy `AppId` (ügyfél-azonosító), `TenantId`, és `ClientSecret` (hitelesítési kulcs), amelyek használnak a hitelesítéshez hello kezelési kódtárakat által. Rendelkeznie kell **tulajdonos** kívánja toorun hello erőforráscsoport engedélyeit.

## <a name="programming-pattern"></a>Programozási minta

minta toomanipulate hello Event Hubs-erőforrásoknál egy közös protokollt követi:

1. Jogkivonat beszerzése az aad-ben hello segítségével `Microsoft.IdentityModel.Clients.ActiveDirectory` könyvtár.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Hozzon létre hello `EventHubManagementClient` objektum.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Set hello `CreateOrUpdate` paraméterek tooyour megadott értéket.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Hello hívás végrehajtása.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Következő lépések
* [.NET felügyeleti minta](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub hivatkozás](/dotnet/api/Microsoft.Azure.Management.EventHub) 
