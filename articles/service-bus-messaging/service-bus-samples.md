---
title: "Azure Service Bus üzenetkezelés – minták áttekintése |} Microsoft Docs"
description: "Ismerteti a Service Bus üzenetküldési mutató hivatkozásokat tartalmaz az egyes minták"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0b420343-2d2a-4c65-98f1-ee0e39ef55c8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: sethm
ms.openlocfilehash: 0cf21ea9a820de0396b54dd26a625a046de39291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="service-bus-messaging-samples"></a><span data-ttu-id="5c8ec-103">A Service Bus üzenetküldési minták</span><span class="sxs-lookup"><span data-stu-id="5c8ec-103">Service Bus messaging samples</span></span>

<span data-ttu-id="5c8ec-104">A Service Bus üzenetküldési minták a fő funkciókat fogunk bemutatni, [Service Bus üzenetkezelés](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="5c8ec-104">The Service Bus messaging samples demonstrate key features in [Service Bus messaging](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="5c8ec-105">Jelenleg a minták két helyen található:</span><span class="sxs-lookup"><span data-stu-id="5c8ec-105">Currently, you can find the samples in two places:</span></span>

- <span data-ttu-id="5c8ec-106">[Service Bus üzenetkezelés minták a Githubon](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet): minták a Githubon található újabb csoportját.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-106">[Service Bus messaging samples on GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet): a newer set of samples, hosted on GitHub.</span></span> <span data-ttu-id="5c8ec-107">Tekintse meg a [információs fájl](https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.ServiceBus.Messaging/README.md) a következő .NET mintákat leírását a tárházban.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-107">See the [readme file](https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.ServiceBus.Messaging/README.md) in the repo for descriptions of these .NET samples.</span></span> <span data-ttu-id="5c8ec-108">A minták folyamatosan frissülnek, így ellenőrizze gyakran biztonsági frissítések.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-108">The samples are continuously updated, so check back often for updates.</span></span>
- <span data-ttu-id="5c8ec-109">[MSDN minták lap](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5): régebbi minták élnek, az MSDN kódgalériából.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-109">[MSDN samples page](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5): older samples that live in the MSDN code gallery.</span></span> <span data-ttu-id="5c8ec-110">Habár ezek a minták továbbra is működik, elvész, és lehet, hogy elavult aktuális programozási tanácsokat tekintetében.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-110">Although these samples still work, they are not maintained and may be outdated with respect to current recommended programming practices.</span></span>
 
## <a name="service-bus-explorer"></a><span data-ttu-id="5c8ec-111">Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="5c8ec-111">Service Bus Explorer</span></span>

<span data-ttu-id="5c8ec-112">Emellett a [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer) egy minta a Githubon, amely lehetővé teszi egy Service Bus-szolgáltatásnévtér csatlakozhat, és egyszerűen felügyelheti az üzenetküldési entitások található.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-112">In addition, the [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer) is a sample hosted on GitHub that enables you to connect to a Service Bus service namespace and easily manage messaging entities.</span></span> <span data-ttu-id="5c8ec-113">Az eszköz speciális funkciókat, például a importálási/exportálási és tesztelni üzenetküldési entitások, és a továbbítási szolgáltatás lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-113">The tool provides advanced features such as import/export functionality, and the ability to test messaging entities and relay services.</span></span> <span data-ttu-id="5c8ec-114">A teljes Service Bus Explorer forrás- és a dokumentáció a talál [GitHub](https://github.com/paolosalvatori/ServiceBusExplorer).</span><span class="sxs-lookup"><span data-stu-id="5c8ec-114">You can find the full Service Bus Explorer source and documentation on [GitHub](https://github.com/paolosalvatori/ServiceBusExplorer).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c8ec-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c8ec-115">Next steps</span></span>

<span data-ttu-id="5c8ec-116">A minta helyek itt találhatók:</span><span class="sxs-lookup"><span data-stu-id="5c8ec-116">Sample locations are here:</span></span>

- [<span data-ttu-id="5c8ec-117">GitHub-minták</span><span class="sxs-lookup"><span data-stu-id="5c8ec-117">GitHub samples</span></span>](https://github.com/Azure/azure-service-bus/tree/master/samples)
- [<span data-ttu-id="5c8ec-118">Service Bus Explorer</span><span class="sxs-lookup"><span data-stu-id="5c8ec-118">Service Bus Explorer</span></span>](https://github.com/paolosalvatori/ServiceBusExplorer)

<span data-ttu-id="5c8ec-119">A következő témakörök a Service Bus fogalmi áttekintésével.</span><span class="sxs-lookup"><span data-stu-id="5c8ec-119">See the following topics for conceptual overviews of Service Bus.</span></span>

* <span data-ttu-id="5c8ec-120">[Service Bus messaging overview](service-bus-messaging-overview.md) (A Service Bus üzenetkezelésének áttekintése)</span><span class="sxs-lookup"><span data-stu-id="5c8ec-120">[Service Bus messaging overview](service-bus-messaging-overview.md)</span></span>
* [<span data-ttu-id="5c8ec-121">Service Bus-architektúra</span><span class="sxs-lookup"><span data-stu-id="5c8ec-121">Service Bus architecture</span></span>](service-bus-architecture.md)
* [<span data-ttu-id="5c8ec-122">A Service Bus alapjai</span><span class="sxs-lookup"><span data-stu-id="5c8ec-122">Service Bus fundamentals</span></span>](service-bus-fundamentals-hybrid-solutions.md)

