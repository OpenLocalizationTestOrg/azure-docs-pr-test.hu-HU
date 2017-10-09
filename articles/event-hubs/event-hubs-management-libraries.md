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
# <a name="event-hubs-management-libraries"></a><span data-ttu-id="fbb4f-103">Event Hubs kezelési kódtárakat</span><span class="sxs-lookup"><span data-stu-id="fbb4f-103">Event Hubs management libraries</span></span>

<span data-ttu-id="fbb4f-104">az Event Hubs-kezelési kódtárakat hello dinamikusan építhető ki az Event Hubs-névterek és az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-104">hello Event Hubs management libraries can dynamically provision Event Hubs namespaces and entities.</span></span> <span data-ttu-id="fbb4f-105">Ez lehetővé teszi, hogy üzenetkezelési forgatókönyveket, és a komplex központi telepítései, hogy milyen entitások tooprovision programozott módon meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-105">This enables complex deployments and messaging scenarios, so that you can programmatically determine what entities tooprovision.</span></span> <span data-ttu-id="fbb4f-106">Ezek a kódtárak jelenleg érhetők el a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="fbb4f-107">Támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="fbb4f-107">Supported functionality</span></span>

* <span data-ttu-id="fbb4f-108">Namespace létrehozási, frissítési, törlési</span><span class="sxs-lookup"><span data-stu-id="fbb4f-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="fbb4f-109">Event Hubs létrehozási, frissítési, törlési</span><span class="sxs-lookup"><span data-stu-id="fbb4f-109">Event Hubs creation, update, deletion</span></span>
* <span data-ttu-id="fbb4f-110">Felhasználói csoport létrehozása, frissítés, törlés</span><span class="sxs-lookup"><span data-stu-id="fbb4f-110">Consumer Group creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbb4f-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fbb4f-111">Prerequisites</span></span>

<span data-ttu-id="fbb4f-112">a tooget használatának hello Event Hubs kezelési kódtárakat, hitelesítenie kell az Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="fbb4f-112">tooget started using hello Event Hubs management libraries, you must authenticate with Azure Active Directory (AAD).</span></span> <span data-ttu-id="fbb4f-113">Aad-ben van szükség, hogy hitelesítse magát egy egyszerű szolgáltatást, amely hozzáférési tooyour biztosít az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-113">AAD requires that you authenticate as a service principal, which provides access tooyour Azure resources.</span></span> <span data-ttu-id="fbb4f-114">Egyszerű szolgáltatás létrehozása kapcsolatos információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="fbb4f-114">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="fbb4f-115">Hello Azure portál toocreate Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="fbb4f-115">Use hello Azure portal toocreate Active Directory application and service principal that can access resources</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="fbb4f-116">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate</span><span class="sxs-lookup"><span data-stu-id="fbb4f-116">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="fbb4f-117">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure CLI toocreate</span><span class="sxs-lookup"><span data-stu-id="fbb4f-117">Use Azure CLI toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

<span data-ttu-id="fbb4f-118">Ezek az oktatóanyagok biztosítanak egy `AppId` (ügyfél-azonosító), `TenantId`, és `ClientSecret` (hitelesítési kulcs), amelyek használnak a hitelesítéshez hello kezelési kódtárakat által.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-118">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by hello management libraries.</span></span> <span data-ttu-id="fbb4f-119">Rendelkeznie kell **tulajdonos** kívánja toorun hello erőforráscsoport engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-119">You must have **Owner** permissions for hello resource group on which you want toorun.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="fbb4f-120">Programozási minta</span><span class="sxs-lookup"><span data-stu-id="fbb4f-120">Programming pattern</span></span>

<span data-ttu-id="fbb4f-121">minta toomanipulate hello Event Hubs-erőforrásoknál egy közös protokollt követi:</span><span class="sxs-lookup"><span data-stu-id="fbb4f-121">hello pattern toomanipulate any Event Hubs resource follows a common protocol:</span></span>

1. <span data-ttu-id="fbb4f-122">Jogkivonat beszerzése az aad-ben hello segítségével `Microsoft.IdentityModel.Clients.ActiveDirectory` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-122">Obtain a token from AAD using hello `Microsoft.IdentityModel.Clients.ActiveDirectory` library.</span></span>
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. <span data-ttu-id="fbb4f-123">Hozzon létre hello `EventHubManagementClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-123">Create hello `EventHubManagementClient` object.</span></span>
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. <span data-ttu-id="fbb4f-124">Set hello `CreateOrUpdate` paraméterek tooyour megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-124">Set hello `CreateOrUpdate` parameters tooyour specified values.</span></span>
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. <span data-ttu-id="fbb4f-125">Hello hívás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="fbb4f-125">Execute hello call.</span></span>
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a><span data-ttu-id="fbb4f-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbb4f-126">Next steps</span></span>
* [<span data-ttu-id="fbb4f-127">.NET felügyeleti minta</span><span class="sxs-lookup"><span data-stu-id="fbb4f-127">.NET Management sample</span></span>](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [<span data-ttu-id="fbb4f-128">Microsoft.Azure.Management.EventHub hivatkozás</span><span class="sxs-lookup"><span data-stu-id="fbb4f-128">Microsoft.Azure.Management.EventHub Reference</span></span>](/dotnet/api/Microsoft.Azure.Management.EventHub) 
