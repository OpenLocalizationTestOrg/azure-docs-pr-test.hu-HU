---
title: "Az Azure Event Hubs kezelési kódtárakat |} Microsoft Docs"
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
ms.openlocfilehash: 0d659cb860a6c98342b548212820efe046decfcc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="event-hubs-management-libraries"></a><span data-ttu-id="81027-103">Event Hubs kezelési kódtárakat</span><span class="sxs-lookup"><span data-stu-id="81027-103">Event Hubs management libraries</span></span>

<span data-ttu-id="81027-104">Az Event Hubs kezelési kódtárakat dinamikusan építhető ki az Event Hubs-névterek és az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="81027-104">The Event Hubs management libraries can dynamically provision Event Hubs namespaces and entities.</span></span> <span data-ttu-id="81027-105">Ez lehetővé teszi összetett telepítések és üzenetkezelési forgatókönyveket, így programozott módon meghatározhatja, milyen entitások kiépítését.</span><span class="sxs-lookup"><span data-stu-id="81027-105">This enables complex deployments and messaging scenarios, so that you can programmatically determine what entities to provision.</span></span> <span data-ttu-id="81027-106">Ezek a kódtárak jelenleg érhetők el a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="81027-106">These libraries are currently available for .NET.</span></span>

## <a name="supported-functionality"></a><span data-ttu-id="81027-107">Támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="81027-107">Supported functionality</span></span>

* <span data-ttu-id="81027-108">Namespace létrehozási, frissítési, törlési</span><span class="sxs-lookup"><span data-stu-id="81027-108">Namespace creation, update, deletion</span></span>
* <span data-ttu-id="81027-109">Event Hubs létrehozási, frissítési, törlési</span><span class="sxs-lookup"><span data-stu-id="81027-109">Event Hubs creation, update, deletion</span></span>
* <span data-ttu-id="81027-110">Felhasználói csoport létrehozása, frissítés, törlés</span><span class="sxs-lookup"><span data-stu-id="81027-110">Consumer Group creation, update, deletion</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81027-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81027-111">Prerequisites</span></span>

<span data-ttu-id="81027-112">Első lépések használatával az Event Hubs kezelési kódtárakat, hitelesítenie kell az Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="81027-112">To get started using the Event Hubs management libraries, you must authenticate with Azure Active Directory (AAD).</span></span> <span data-ttu-id="81027-113">Az AAD megköveteli, hogy hitelesítse magát egy egyszerű szolgáltatást, amely az Azure-erőforrások hozzáférést tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="81027-113">AAD requires that you authenticate as a service principal, which provides access to your Azure resources.</span></span> <span data-ttu-id="81027-114">Egyszerű szolgáltatás létrehozása kapcsolatos információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="81027-114">For information about creating a service principal, see one of these articles:</span></span>  

* [<span data-ttu-id="81027-115">Active Directory-alkalmazás és az erőforrások eléréséhez egyszerű szolgáltatás létrehozása az Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="81027-115">Use the Azure portal to create Active Directory application and service principal that can access resources</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="81027-116">Szolgáltatásnév létrehozása erőforrások eléréséhez az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="81027-116">Use Azure PowerShell to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="81027-117">Szolgáltatásnév létrehozása erőforrások eléréséhez az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="81027-117">Use Azure CLI to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

<span data-ttu-id="81027-118">Ezek az oktatóanyagok biztosítanak egy `AppId` (ügyfél-azonosító), `TenantId`, és `ClientSecret` (hitelesítési kulcs), amelyek használnak a hitelesítéshez a felügyeleti függvénytárai.</span><span class="sxs-lookup"><span data-stu-id="81027-118">These tutorials provide you with an `AppId` (Client ID), `TenantId`, and `ClientSecret` (authentication key), all of which are used for authentication by the management libraries.</span></span> <span data-ttu-id="81027-119">Rendelkeznie kell **tulajdonos** ahhoz az erőforráscsoporthoz, amelyen futtatni kívánt engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="81027-119">You must have **Owner** permissions for the resource group on which you want to run.</span></span>

## <a name="programming-pattern"></a><span data-ttu-id="81027-120">Programozási minta</span><span class="sxs-lookup"><span data-stu-id="81027-120">Programming pattern</span></span>

<span data-ttu-id="81027-121">Minden Event Hubs erőforrás kezelheti a minta egy közös protokollt követi:</span><span class="sxs-lookup"><span data-stu-id="81027-121">The pattern to manipulate any Event Hubs resource follows a common protocol:</span></span>

1. <span data-ttu-id="81027-122">Aad-ben a jogkivonat beszerzése az `Microsoft.IdentityModel.Clients.ActiveDirectory` könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="81027-122">Obtain a token from AAD using the `Microsoft.IdentityModel.Clients.ActiveDirectory` library.</span></span>
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. <span data-ttu-id="81027-123">Hozzon létre a `EventHubManagementClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="81027-123">Create the `EventHubManagementClient` object.</span></span>
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. <span data-ttu-id="81027-124">Állítsa be a `CreateOrUpdate` paramétereit, és a megadott értékeket.</span><span class="sxs-lookup"><span data-stu-id="81027-124">Set the `CreateOrUpdate` parameters to your specified values.</span></span>
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. <span data-ttu-id="81027-125">A hívás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="81027-125">Execute the call.</span></span>
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a><span data-ttu-id="81027-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81027-126">Next steps</span></span>
* [<span data-ttu-id="81027-127">.NET felügyeleti minta</span><span class="sxs-lookup"><span data-stu-id="81027-127">.NET Management sample</span></span>](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [<span data-ttu-id="81027-128">Microsoft.Azure.Management.EventHub hivatkozás</span><span class="sxs-lookup"><span data-stu-id="81027-128">Microsoft.Azure.Management.EventHub Reference</span></span>](/dotnet/api/Microsoft.Azure.Management.EventHub) 
