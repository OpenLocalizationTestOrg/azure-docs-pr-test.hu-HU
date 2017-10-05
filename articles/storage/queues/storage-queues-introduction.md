---
title: "Azure Queue storage bemutatása |} Microsoft Docs"
description: "Azure Queue storage bemutatása"
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
ms.openlocfilehash: 4db7552a1b76c89151405c55c8682abbb5326bb6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-queues"></a><span data-ttu-id="e4895-103">Az üzenetsorok bemutatása</span><span class="sxs-lookup"><span data-stu-id="e4895-103">Introduction to Queues</span></span>

<span data-ttu-id="e4895-104">Az Azure Queue Storage szolgáltatás üzenetek nagy számban történő tárolására szolgál, amelyek HTTP- vagy HTTPS-kapcsolattal, hitelesített hívásokon keresztül a világon bárhonnan elérhetők.</span><span class="sxs-lookup"><span data-stu-id="e4895-104">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="e4895-105">Egyetlen üzenetsor akár 64 KB méretű is lehet, és a tárfiók maximális kapacitásán belül több millió üzenetet tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e4895-105">A single queue message can be up to 64 KB in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

## <a name="common-uses"></a><span data-ttu-id="e4895-106">Gyakori használati mód</span><span class="sxs-lookup"><span data-stu-id="e4895-106">Common uses</span></span>

<span data-ttu-id="e4895-107">A Queue Storage gyakori használati módjai:</span><span class="sxs-lookup"><span data-stu-id="e4895-107">Common uses of Queue storage include:</span></span>

* <span data-ttu-id="e4895-108">Hátralékos munkák létrehozása aszinkron feldolgozáshoz</span><span class="sxs-lookup"><span data-stu-id="e4895-108">Creating a backlog of work to process asynchronously</span></span>
* <span data-ttu-id="e4895-109">Üzenetek átadása egy Azure webes szerepkörről egy Azure feldolgozói szerepkörnek</span><span class="sxs-lookup"><span data-stu-id="e4895-109">Passing messages from an Azure web role to an Azure worker role</span></span>

## <a name="queue-service-concepts"></a><span data-ttu-id="e4895-110">A Queue szolgáltatás alapfogalmai</span><span class="sxs-lookup"><span data-stu-id="e4895-110">Queue service concepts</span></span>

<span data-ttu-id="e4895-111">A Queue szolgáltatás az alábbi összetevőkből áll:</span><span class="sxs-lookup"><span data-stu-id="e4895-111">The Queue service contains the following components:</span></span>

![Várólista fogalmak](./media/storage-queues-introduction/queue1.png)

* <span data-ttu-id="e4895-113">**URL-formátum:** Az üzenetsorok a következő URL-formátummal érhetők el:</span><span class="sxs-lookup"><span data-stu-id="e4895-113">**URL format:** Queues are addressable using the following URL format:</span></span>   
    <span data-ttu-id="e4895-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span><span class="sxs-lookup"><span data-stu-id="e4895-114">http://`<storage account>`.queue.core.windows.net/`<queue>`</span></span> 
  
    <span data-ttu-id="e4895-115">Az ábra egyik üzenetsora a következő URL-címmel érhető el:</span><span class="sxs-lookup"><span data-stu-id="e4895-115">The following URL addresses a queue in the diagram:</span></span>  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* <span data-ttu-id="e4895-116">**A tárfiók:** Azure Storage minden hozzáférés a storage-fiók segítségével történik.</span><span class="sxs-lookup"><span data-stu-id="e4895-116">**Storage account:** All access to Azure Storage is done through a storage account.</span></span> <span data-ttu-id="e4895-117">A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Az Azure Storage méretezhetőségi és teljesítménycéljai).</span><span class="sxs-lookup"><span data-stu-id="e4895-117">See [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) for details about storage account capacity.</span></span>

* <span data-ttu-id="e4895-118">**Üzenetsor:** Az üzenetsorok üzenetek készleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="e4895-118">**Queue:** A queue contains a set of messages.</span></span> <span data-ttu-id="e4895-119">Az összes üzenetnek üzenetsorban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e4895-119">All messages must be in a queue.</span></span> <span data-ttu-id="e4895-120">Vegye figyelembe, hogy az üzenetsor neve csak kisbetűket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e4895-120">Note that the queue name must be all lowercase.</span></span> <span data-ttu-id="e4895-121">Az üzenetsorok elnevezésével kapcsolatos információkat lásd: [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Üzenetsorok és metaadatok elnevezése).</span><span class="sxs-lookup"><span data-stu-id="e4895-121">For information on naming queues, see [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

* <span data-ttu-id="e4895-122">**Üzenet:** Egy legfeljebb 64 KB méretű, tetszőleges méretű üzenet.</span><span class="sxs-lookup"><span data-stu-id="e4895-122">**Message:** A message, in any format, of up to 64 KB.</span></span> <span data-ttu-id="e4895-123">A maximális időt, amely egy üzenetet a várólistában lévő maradjanak hét nap.</span><span class="sxs-lookup"><span data-stu-id="e4895-123">The maximum time that a message can remain in the queue is seven days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4895-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4895-124">Next steps</span></span>

* [<span data-ttu-id="e4895-125">Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4895-125">Create a storage account</span></span>](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [<span data-ttu-id="e4895-126">Bevezetés az üzenetsorok .NET használatával</span><span class="sxs-lookup"><span data-stu-id="e4895-126">Getting started with Queues using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
