---
title: "Service Bus-névtér létrehozása az Azure Portalon | Microsoft Docs"
description: "Service Bus-névtér létrehozása az Azure Portal használatával."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: c8654ed547a9001e2e968d2a45d990a73ef27d3b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a><span data-ttu-id="910d2-103">Service Bus-névtér létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="910d2-103">Create a Service Bus namespace using the Azure portal</span></span>

<span data-ttu-id="910d2-104">A névtér egy hatókörkezelési tároló az üzenetkezelés összes összetevője számára.</span><span class="sxs-lookup"><span data-stu-id="910d2-104">A namespace is a scoping container for all messaging components.</span></span> <span data-ttu-id="910d2-105">Egyetlen névtér több üzenetsort és témakört tartalmazhat, és a névterek gyakran alkalmazástárolókként is szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="910d2-105">Multiple queues and topics can reside within a single namespace, and namespaces often serve as application containers.</span></span> <span data-ttu-id="910d2-106">A Service Bus-névterek létrehozásának két különböző módja van:</span><span class="sxs-lookup"><span data-stu-id="910d2-106">There are two different ways to create a Service Bus namespace:</span></span>

1. <span data-ttu-id="910d2-107">Azure Portal (ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="910d2-107">Azure portal (this article)</span></span>
2. <span data-ttu-id="910d2-108">[Resource Manager-sablonok][create-namespace-using-arm]</span><span class="sxs-lookup"><span data-stu-id="910d2-108">[Resource Manager templates][create-namespace-using-arm]</span></span>

## <a name="create-a-namespace-in-the-azure-portal"></a><span data-ttu-id="910d2-109">Névtér létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="910d2-109">Create a namespace in the Azure portal</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

<span data-ttu-id="910d2-110">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="910d2-110">Congratulations!</span></span> <span data-ttu-id="910d2-111">Létrehozott egy Service Bus üzenetkezelési névteret.</span><span class="sxs-lookup"><span data-stu-id="910d2-111">You have now created a Service Bus Messaging namespace.</span></span>

## <a name="next-steps"></a><span data-ttu-id="910d2-112">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="910d2-112">Next steps</span></span>

<span data-ttu-id="910d2-113">Tekintse meg a [GitHub-mintáinkat][github-samples], ahol további példákat talál, amelyek az Azure Service Bus üzenetkezelési szolgáltatásának speciális funkcióit mutatják be.</span><span class="sxs-lookup"><span data-stu-id="910d2-113">Check out our [GitHub samples][github-samples], which show some of the more advanced features of Azure Service Bus Messaging.</span></span>

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
