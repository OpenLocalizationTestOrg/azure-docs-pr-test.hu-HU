---
title: "aaaAzure továbbítási API áttekintése |} Microsoft Docs"
description: "Elérhető Azure továbbítási API-k – áttekintés"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: 3c4d737d5fee9a8babce094fa6dfddb28910834b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="available-relay-apis"></a><span data-ttu-id="413eb-103">Rendelkezésre álló továbbítási API-k</span><span class="sxs-lookup"><span data-stu-id="413eb-103">Available Relay APIs</span></span>

## <a name="runtime-apis"></a><span data-ttu-id="413eb-104">Futásidejű API-k</span><span class="sxs-lookup"><span data-stu-id="413eb-104">Runtime APIs</span></span>

<span data-ttu-id="413eb-105">hello következő táblázatban minden jelenleg elérhető továbbítási futásidejű ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="413eb-105">hello following table lists all currently available Relay runtime clients.</span></span>

<span data-ttu-id="413eb-106">Hello [további információt](#additional-information) szakasz mindegyik futásidejű kódtár hello állapotával kapcsolatos további információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="413eb-106">hello [additional information](#additional-information) section contains more information about hello status of each runtime library.</span></span>

| <span data-ttu-id="413eb-107">Nyelvi/Platform</span><span class="sxs-lookup"><span data-stu-id="413eb-107">Language/Platform</span></span> | <span data-ttu-id="413eb-108">A szolgáltatás elérhető</span><span class="sxs-lookup"><span data-stu-id="413eb-108">Available feature</span></span> | <span data-ttu-id="413eb-109">Ügyfélcsomag</span><span class="sxs-lookup"><span data-stu-id="413eb-109">Client package</span></span> | <span data-ttu-id="413eb-110">Tárház</span><span class="sxs-lookup"><span data-stu-id="413eb-110">Repository</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="413eb-111">.NET-szabvány</span><span class="sxs-lookup"><span data-stu-id="413eb-111">.NET Standard</span></span> | <span data-ttu-id="413eb-112">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="413eb-112">Hybrid Connections</span></span> | [<span data-ttu-id="413eb-113">Microsoft.Azure.Relay</span><span class="sxs-lookup"><span data-stu-id="413eb-113">Microsoft.Azure.Relay</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [<span data-ttu-id="413eb-114">GitHub</span><span class="sxs-lookup"><span data-stu-id="413eb-114">GitHub</span></span>](https://github.com/azure/azure-relay-dotnet) |
| <span data-ttu-id="413eb-115">.NET-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="413eb-115">.NET Framework</span></span> | <span data-ttu-id="413eb-116">WCF-továbbító</span><span class="sxs-lookup"><span data-stu-id="413eb-116">WCF Relay</span></span> | [<span data-ttu-id="413eb-117">WindowsAzure.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="413eb-117">WindowsAzure.ServiceBus</span></span>](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | <span data-ttu-id="413eb-118">N/A</span><span class="sxs-lookup"><span data-stu-id="413eb-118">N/A</span></span> |
| <span data-ttu-id="413eb-119">Csomópont</span><span class="sxs-lookup"><span data-stu-id="413eb-119">Node</span></span> | <span data-ttu-id="413eb-120">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="413eb-120">Hybrid Connections</span></span> | [`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket) | [<span data-ttu-id="413eb-121">GitHub</span><span class="sxs-lookup"><span data-stu-id="413eb-121">GitHub</span></span>](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a><span data-ttu-id="413eb-122">További információ</span><span class="sxs-lookup"><span data-stu-id="413eb-122">Additional information</span></span>

#### <a name="net"></a><span data-ttu-id="413eb-123">.NET</span><span class="sxs-lookup"><span data-stu-id="413eb-123">.NET</span></span>
<span data-ttu-id="413eb-124">hello .NET ökoszisztéma több futtatókörnyezetek rendelkezik, ezért van több .NET-kódtárakra az Event Hubs számára.</span><span class="sxs-lookup"><span data-stu-id="413eb-124">hello .NET ecosystem has multiple runtimes, hence there are multiple .NET libraries for Event Hubs.</span></span> <span data-ttu-id="413eb-125">hello .NET szabványos függvénytár is futtatható, használja a .NET Core vagy a .NET-keretrendszer hello közben hello .NET Framework könyvtár csak a .NET-keretrendszer környezetben futtatható.</span><span class="sxs-lookup"><span data-stu-id="413eb-125">hello .NET Standard library can be run using either .NET Core or hello .NET Framework, while hello .NET Framework library can only be run in a .NET Framework environment.</span></span> <span data-ttu-id="413eb-126">További információ a .NET-keretrendszer: [keretrendszer-verziók](/dotnet/articles/standard/frameworks#framework-versions).</span><span class="sxs-lookup"><span data-stu-id="413eb-126">For more information on .NET Frameworks, see [framework versions](/dotnet/articles/standard/frameworks#framework-versions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="413eb-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="413eb-127">Next steps</span></span>
<span data-ttu-id="413eb-128">További információ az Azure továbbítási toolearn látogasson el ezeket a hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="413eb-128">toolearn more about Azure Relay, visit these links:</span></span>
* [<span data-ttu-id="413eb-129">Mi az az Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="413eb-129">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="413eb-130">Relay – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="413eb-130">Relay FAQ</span></span>](relay-faq.md)