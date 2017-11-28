---
title: a Queue storage aaaIntroduction tooAzure |} Microsoft Docs
description: "A Queue storage bemutatása tooAzure"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: robinsh
ms.openlocfilehash: 669effedff7beedde8a119c350a2a70edafedcf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooqueues"></a><span data-ttu-id="61c52-103">Bevezetés tooQueues</span><span class="sxs-lookup"><span data-stu-id="61c52-103">Introduction tooQueues</span></span>

<span data-ttu-id="61c52-104">Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához.</span><span class="sxs-lookup"><span data-stu-id="61c52-104">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="61c52-105">Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="61c52-105">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

## <a name="common-uses"></a><span data-ttu-id="61c52-106">Gyakori használati mód</span><span class="sxs-lookup"><span data-stu-id="61c52-106">Common uses</span></span>

<span data-ttu-id="61c52-107">A Queue Storage gyakori használati módjai:</span><span class="sxs-lookup"><span data-stu-id="61c52-107">Common uses of Queue storage include:</span></span>

* <span data-ttu-id="61c52-108">A munkahelyi tooprocess várakozó aszinkron módon létrehozása</span><span class="sxs-lookup"><span data-stu-id="61c52-108">Creating a backlog of work tooprocess asynchronously</span></span>
* <span data-ttu-id="61c52-109">Üzenetek átadása egy Azure webes szerepkör tooan Azure feldolgozói szerepkörnek</span><span class="sxs-lookup"><span data-stu-id="61c52-109">Passing messages from an Azure web role tooan Azure worker role</span></span>

## <a name="queue-service-concepts"></a><span data-ttu-id="61c52-110">A Queue szolgáltatás alapfogalmai</span><span class="sxs-lookup"><span data-stu-id="61c52-110">Queue service concepts</span></span>

<span data-ttu-id="61c52-111">Queue szolgáltatás hello hello a következő összetevőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="61c52-111">hello Queue service contains hello following components:</span></span>

![Várólista fogalmak](./media/storage-queues-introduction/queue1.png)

* <span data-ttu-id="61c52-113">**URL-formátum:** várólisták, amelyek megcímezhető hello URL-cím formátuma a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="61c52-113">**URL format:** Queues are addressable using hello following URL format:</span></span>   
    <span data-ttu-id="61c52-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span><span class="sxs-lookup"><span data-stu-id="61c52-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span></span> 
  
    <span data-ttu-id="61c52-115">a következő URL-cím hello hello ábra egyik üzenetsora címek:</span><span class="sxs-lookup"><span data-stu-id="61c52-115">hello following URL addresses a queue in hello diagram:</span></span>  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* <span data-ttu-id="61c52-116">**A tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="61c52-116">**Storage account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="61c52-117">A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Az Azure Storage méretezhetőségi és teljesítménycéljai).</span><span class="sxs-lookup"><span data-stu-id="61c52-117">See [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) for details about storage account capacity.</span></span>

* <span data-ttu-id="61c52-118">**Üzenetsor:** Az üzenetsorok üzenetek készleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="61c52-118">**Queue:** A queue contains a set of messages.</span></span> <span data-ttu-id="61c52-119">Az összes üzenetnek üzenetsorban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="61c52-119">All messages must be in a queue.</span></span> <span data-ttu-id="61c52-120">Vegye figyelembe, hogy hello várólista neve csak kisbetűket.</span><span class="sxs-lookup"><span data-stu-id="61c52-120">Note that hello queue name must be all lowercase.</span></span> <span data-ttu-id="61c52-121">Az üzenetsorok elnevezésével kapcsolatos információkat lásd: [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Üzenetsorok és metaadatok elnevezése).</span><span class="sxs-lookup"><span data-stu-id="61c52-121">For information on naming queues, see [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

* <span data-ttu-id="61c52-122">**Üzenet:** egy üzenet jelenik meg, tetszőleges méretű a too64 KB fel.</span><span class="sxs-lookup"><span data-stu-id="61c52-122">**Message:** A message, in any format, of up too64 KB.</span></span> <span data-ttu-id="61c52-123">hello maximális időtartam, egy üzenet maradhat hello sorban hét nap.</span><span class="sxs-lookup"><span data-stu-id="61c52-123">hello maximum time that a message can remain in hello queue is seven days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61c52-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61c52-124">Next steps</span></span>

* [<span data-ttu-id="61c52-125">Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="61c52-125">Create a storage account</span></span>](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="61c52-126">Bevezetés az üzenetsorok .NET használatával</span><span class="sxs-lookup"><span data-stu-id="61c52-126">Getting started with Queues using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
