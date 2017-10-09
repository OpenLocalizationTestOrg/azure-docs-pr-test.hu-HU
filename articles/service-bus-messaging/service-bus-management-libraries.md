---
title: "a Service Bus-kezelési kódtárakat aaaAzure |} Microsoft Docs"
description: "A Service Bus-névterek és az üzenetküldési entitások, a .NET-kezelése."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: 9e4ad91f22815ca0838e6e4647a3606109b2b441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-management-libraries"></a>A Service Bus kezelési kódtárakat

hello Azure Service Bus kezelési kódtárakat dinamikusan építhető ki a Service Bus-névterek és az entitásokat. Ez lehetővé teszi összetett központi telepítések és üzenetkezelési forgatókönyveket, és lehetővé teszi, hogy tooprogrammatically milyen entitások tooprovision határozza meg. Ezek a kódtárak jelenleg érhetők el a .NET-hez.

## <a name="supported-functionality"></a>Támogatott funkciók

* Namespace létrehozási, frissítési, törlési
* Várólista létrehozása, frissítés, törlés
* A témakör létrehozását, frissítés, törlés
* Előfizetés létrehozása, frissítés, törlés

## <a name="prerequisites"></a>Előfeltételek

a tooget használatának hello Service Bus kezelési kódtárakat, hitelesítenie kell a hello Azure Active Directory (AAD) szolgáltatást. Aad-ben van szükség, hogy hitelesítse magát egy egyszerű szolgáltatást, amely hozzáférési tooyour biztosít az Azure-erőforrások. Egyszerű szolgáltatás létrehozása kapcsolatos információkért tekintse meg a következő cikkeket:  

* [Hello Azure portál toocreate Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás használata](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Ezek az oktatóanyagok biztosítanak egy `AppId` (ügyfél-azonosító), `TenantId`, és `ClientSecret` (hitelesítési kulcs), amelyek használnak a hitelesítéshez hello kezelési kódtárakat által. Rendelkeznie kell **tulajdonos** hello erőforráscsoportot, amelyiken toorun kívánja engedélyeit.

## <a name="programming-pattern"></a>Programozási minta

minta toomanipulate hello Service Bus-erőforrásoknál egy közös protokollt követi:

1. Jogkivonat beszerzése az Azure Active Directory használatával hello **Microsoft.IdentityModel.Clients.ActiveDirectory** könyvtárban.
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. Hozzon létre hello `ServiceBusManagementClient` objektum.

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. Set hello `CreateOrUpdate` paraméterek tooyour megadott értéket.

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. Hello hívás végrehajtása.

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Következő lépések
* [.NET felügyeleti minta](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API-referencia](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
