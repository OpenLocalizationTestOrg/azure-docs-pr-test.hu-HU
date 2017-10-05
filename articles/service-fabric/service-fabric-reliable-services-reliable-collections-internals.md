---
title: "Azure Service Fabric megbízható állapotkezelője és megbízható gyűjtemény internals |} Microsoft Docs"
description: "Részletes bemutatója a megbízható gyűjtemény fogalmakat és az Azure Service Fabric Tervező."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: d607449a16e886337ab1bd96213fbb4231124353
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a><span data-ttu-id="d63ef-103">Azure Service Fabric megbízható állapotkezelője és megbízható gyűjtemény belső funkciói</span><span class="sxs-lookup"><span data-stu-id="d63ef-103">Azure Service Fabric Reliable State Manager and Reliable Collection internals</span></span>
<span data-ttu-id="d63ef-104">Ez a dokumentum delves belül megbízható állapotkezelője és megbízható gyűjtemények megjelenítéséhez kiadásának alapvető összetevői működése a háttérben.</span><span class="sxs-lookup"><span data-stu-id="d63ef-104">This document delves inside Reliable State Manager and Reliable Collections to see how core components work behind the scenes.</span></span>

> [!NOTE]
> <span data-ttu-id="d63ef-105">Ez a dokumentum munkahelyi folyamatban.</span><span class="sxs-lookup"><span data-stu-id="d63ef-105">This document is work in-progress.</span></span> <span data-ttu-id="d63ef-106">Ez a cikk adja meg, milyen további információ szeretné témakör megjegyzések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d63ef-106">Add comments to this article to tell us what topic you would like to learn more about.</span></span>
>

##  <a name="local-persistence-model-log-and-checkpoint"></a><span data-ttu-id="d63ef-107">Helyi adatmegőrzési modell: naplóhoz és ellenőrzőpont</span><span class="sxs-lookup"><span data-stu-id="d63ef-107">Local persistence model: log and checkpoint</span></span>
<span data-ttu-id="d63ef-108">A megbízható állapotkezelője és megbízható hajtsa végre az adatmegőrzési modell, amely a naplóhoz és ellenőrzőpont nevezik.</span><span class="sxs-lookup"><span data-stu-id="d63ef-108">The Reliable State Manager and Reliable Collections follow a persistence model that is called Log and Checkpoint.</span></span>
<span data-ttu-id="d63ef-109">Ebben a modellben minden állapotváltozás először jelentkezik be lemezt, és alkalmazza a memóriában.</span><span class="sxs-lookup"><span data-stu-id="d63ef-109">In this model, each state change is logged on disk first and then applied in memory.</span></span>
<span data-ttu-id="d63ef-110">A kész állapot maga is megőrződjenek csak esetenként (más néven</span><span class="sxs-lookup"><span data-stu-id="d63ef-110">The complete state itself is persisted only occasionally (a.k.a.</span></span> <span data-ttu-id="d63ef-111">Ellenőrzőpont).</span><span class="sxs-lookup"><span data-stu-id="d63ef-111">Checkpoint).</span></span>
<span data-ttu-id="d63ef-112">Az az előnye, hogy eltérések szekvenciális csak írási műveleteket a lemezen a jobb teljesítmény be vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="d63ef-112">The benefit is that deltas are turned into sequential append-only writes on disk for improved performance.</span></span>

<span data-ttu-id="d63ef-113">Jobb megértése érdekében a naplóhoz és ellenőrzőpont modell, először nézzük a végtelen lemez forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="d63ef-113">To better understand the Log and Checkpoint model, let’s first look at the infinite disk scenario.</span></span>
<span data-ttu-id="d63ef-114">A megbízható állapotkezelője naplózza a minden műveletet a replikálás előtt.</span><span class="sxs-lookup"><span data-stu-id="d63ef-114">The Reliable State Manager logs every operation before it is replicated.</span></span>
<span data-ttu-id="d63ef-115">Naplózás lehetővé teszi, hogy a megbízható gyűjtemény a művelet csak a memóriában.</span><span class="sxs-lookup"><span data-stu-id="d63ef-115">Logging allows the Reliable Collections to apply the operation only in memory.</span></span>
<span data-ttu-id="d63ef-116">Naplók megmaradnak, még akkor is, ha a replika nem sikerül, és újra kell indítani, mert a megbízható állapotkezelője elegendő információ áll rendelkezésre a naplót a műveleteket a replika elvesztette visszajátszani van.</span><span class="sxs-lookup"><span data-stu-id="d63ef-116">Since logs are persisted, even when the replica fails and needs to be restarted, the Reliable State Manager has enough information in its log to replay all the operations the replica has lost.</span></span>
<span data-ttu-id="d63ef-117">Amennyiben a lemez végtelen, naplórekordokat soha nem kell őket távolítani, és csak a memórián belüli állapot kezelése szükség a megbízható gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="d63ef-117">As the disk is infinite, log records never need to be removed and the Reliable Collection needs to manage only the in-memory state.</span></span>

<span data-ttu-id="d63ef-118">Most már a véges lemez forgatókönyv vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="d63ef-118">Now let’s look at the finite disk scenario.</span></span>
<span data-ttu-id="d63ef-119">Naplórekordok gyűlik össze, mivel a megbízható állapotkezelője nincs elég szabad lemezterület fog futni.</span><span class="sxs-lookup"><span data-stu-id="d63ef-119">As log records accumulate, the Reliable State Manager will run out of disk space.</span></span>
<span data-ttu-id="d63ef-120">Mielőtt ebben az esetben a megbízható állapotkezelője kell csonkolja a naplót, hogy helyet biztosítsanak az újabb bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="d63ef-120">Before that happens, the Reliable State Manager needs to truncate its log to make room for the newer records.</span></span>
<span data-ttu-id="d63ef-121">Megbízható állapotkezelője kérelmek ellenőrzőpont megbízható gyűjtemények azok a memóriában lemezre.</span><span class="sxs-lookup"><span data-stu-id="d63ef-121">Reliable State Manager requests the Reliable Collections to checkpoint their in-memory state to disk.</span></span>
<span data-ttu-id="d63ef-122">Ezen a ponton a megbízható gyűjtemények szeretné megőrizni a memórián belüli állapotában.</span><span class="sxs-lookup"><span data-stu-id="d63ef-122">At this point, the Reliable Collections' would persist its in-memory state.</span></span>
<span data-ttu-id="d63ef-123">Ha a megbízható gyűjtemények végezze el az ellenőrzőpontokat, a megbízható állapotkezelő is csonkolja a naplót, szabadítson fel megfelelő mennyiségű lemezterületet.</span><span class="sxs-lookup"><span data-stu-id="d63ef-123">Once the Reliable Collections complete their checkpoints, the Reliable State Manager can truncate the log to free up disk space.</span></span>
<span data-ttu-id="d63ef-124">Ha a replika újra kell indítani, megbízható gyűjtemények helyreállításához alkulcsaihoz állapotukra és megbízható állapotkezelő állítja helyre, és az utolsó ellenőrzőpont óta történt összes állapotváltozások lejátssza.</span><span class="sxs-lookup"><span data-stu-id="d63ef-124">When the replica needs to be restarted, Reliable Collections recover their checkpointed state, and the Reliable State Manager recovers and plays back all the state changes that occurred since the last checkpoint.</span></span>

<span data-ttu-id="d63ef-125">Ellenőrzőpontok hozzáadása egy másik értéket, hogy ez növeli a gyakori forgatókönyvek helyreállítási alkalommal.</span><span class="sxs-lookup"><span data-stu-id="d63ef-125">Another value add of checkpointing is that it improves recovery times in common scenarios.</span></span> <span data-ttu-id="d63ef-126">A napló tartalmazza az összes művelet történt, mert az utolsó ellenőrzőpont.</span><span class="sxs-lookup"><span data-stu-id="d63ef-126">Log contains all operations that have happened since the last checkpoint.</span></span>
<span data-ttu-id="d63ef-127">Így például a megadott sor több értéket elem több verziója tartalmazhat megbízható szótárban.</span><span class="sxs-lookup"><span data-stu-id="d63ef-127">So it may include multiple versions of an item like multiple values for a given row in Reliable Dictionary.</span></span>
<span data-ttu-id="d63ef-128">Ezzel szemben, a megbízható gyűjtemény ellenőrzőpontokat kulcsok minden egyes érték csak a legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="d63ef-128">In contrast, a Reliable Collection checkpoints only the latest version of each value for a key.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d63ef-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d63ef-129">Next steps</span></span>
* [<span data-ttu-id="d63ef-130">Tranzakciók és a zárolásokat</span><span class="sxs-lookup"><span data-stu-id="d63ef-130">Transactions and Locks</span></span>](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

