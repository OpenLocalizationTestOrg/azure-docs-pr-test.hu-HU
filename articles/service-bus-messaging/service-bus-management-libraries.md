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
# <a name="service-bus-management-libraries"></a><span data-ttu-id="a8f45-103">A Service Bus kezelési kódtárakat</span><span class="sxs-lookup"><span data-stu-id="a8f45-103">Service Bus management libraries</span></span>

<span data-ttu-id="a8f45-104">hello Azure Service Bus kezelési kódtárakat dinamikusan építhető ki a Service Bus-névterek és az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="a8f45-104">hello Azure Service Bus management libraries can dynamically provision Service Bus namespaces and entities.</span></span> <span data-ttu-id="a8f45-105">Ez lehetővé teszi összetett központi telepítések és üzenetkezelési forgatókönyveket, és lehetővé teszi, hogy tooprogrammatically milyen entitások tooprovision határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a8f45-105">This enables complex deployments and messaging scenarios, and makes it possible tooprogrammatically determine what entities tooprovision.</span></span> <span data-ttu-id="a8f45-106">Ezek a kódtárak jelenleg érhetők el a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="a8f45-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="a8f45-107">Támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="a8f45-107">Supported functionality</span></span>

* <span data-ttu-id="a8f45-108">Namespace létrehozási, frissítési, törlési</span><span class="sxs-lookup"><span data-stu-id="a8f45-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="a8f45-109">Várólista létrehozása, frissítés, törlés</span><span class="sxs-lookup"><span data-stu-id="a8f45-109">Queue creation, update, deletion</span></span>
* <span data-ttu-id="a8f45-110">A témakör létrehozását, frissítés, törlés</span><span class="sxs-lookup"><span data-stu-id="a8f45-110">Topic creation, update, deletion</span></span>
* <span data-ttu-id="a8f45-111">Előfizetés létrehozása, frissítés, törlés</span><span class="sxs-lookup"><span data-stu-id="a8f45-111">Subscription creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8f45-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a8f45-112">Prerequisites</span></span>

<span data-ttu-id="a8f45-113">a tooget használatának hello Service Bus kezelési kódtárakat, hitelesítenie kell a hello Azure Active Directory (AAD) szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a8f45-113">tooget started using hello Service Bus management libraries, you must authenticate with hello Azure Active Directory (AAD) service.</span></span> <span data-ttu-id="a8f45-114">Aad-ben van szükség, hogy hitelesítse magát egy egyszerű szolgáltatást, amely hozzáférési tooyour biztosít az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="a8f45-114">AAD requires that you authenticate as a service principal, which provides access tooyour Azure resources.</span></span> <span data-ttu-id="a8f45-115">Egyszerű szolgáltatás létrehozása kapcsolatos információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="a8f45-115">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="a8f45-116">Hello Azure portál toocreate Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="a8f45-116">Use hello Azure portal toocreate Active Directory application and service principal that can access resources</span></span>](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [<span data-ttu-id="a8f45-117">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate</span><span class="sxs-lookup"><span data-stu-id="a8f45-117">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [<span data-ttu-id="a8f45-118">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate</span><span class="sxs-lookup"><span data-stu-id="a8f45-118">Use Azure CLI toocreate a service principal tooaccess resources</span></span>](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

<span data-ttu-id="a8f45-119">Ezek az oktatóanyagok biztosítanak egy `AppId` (ügyfél-azonosító), `TenantId`, és `ClientSecret` (hitelesítési kulcs), amelyek használnak a hitelesítéshez hello kezelési kódtárakat által.</span><span class="sxs-lookup"><span data-stu-id="a8f45-119">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by hello management libraries.</span></span> <span data-ttu-id="a8f45-120">Rendelkeznie kell **tulajdonos** hello erőforráscsoportot, amelyiken toorun kívánja engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="a8f45-120">You must have **Owner** permissions for hello resource group on which you wish toorun.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="a8f45-121">Programozási minta</span><span class="sxs-lookup"><span data-stu-id="a8f45-121">Programming pattern</span></span>

<span data-ttu-id="a8f45-122">minta toomanipulate hello Service Bus-erőforrásoknál egy közös protokollt követi:</span><span class="sxs-lookup"><span data-stu-id="a8f45-122">hello pattern toomanipulate any Service Bus resource follows a common protocol:</span></span>

1. <span data-ttu-id="a8f45-123">Jogkivonat beszerzése az Azure Active Directory használatával hello **Microsoft.IdentityModel.Clients.ActiveDirectory** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="a8f45-123">Obtain a token from Azure Active Directory using hello **Microsoft.IdentityModel.Clients.ActiveDirectory** library.</span></span>
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. <span data-ttu-id="a8f45-124">Hozzon létre hello `ServiceBusManagementClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="a8f45-124">Create hello `ServiceBusManagementClient` object.</span></span>

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. <span data-ttu-id="a8f45-125">Set hello `CreateOrUpdate` paraméterek tooyour megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="a8f45-125">Set hello `CreateOrUpdate` parameters tooyour specified values.</span></span>

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. <span data-ttu-id="a8f45-126">Hello hívás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a8f45-126">Execute hello call.</span></span>

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a><span data-ttu-id="a8f45-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8f45-127">Next steps</span></span>
* [<span data-ttu-id="a8f45-128">.NET felügyeleti minta</span><span class="sxs-lookup"><span data-stu-id="a8f45-128">.NET management sample</span></span>](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [<span data-ttu-id="a8f45-129">Microsoft.Azure.Management.ServiceBus API-referencia</span><span class="sxs-lookup"><span data-stu-id="a8f45-129">Microsoft.Azure.Management.ServiceBus API reference</span></span>](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
