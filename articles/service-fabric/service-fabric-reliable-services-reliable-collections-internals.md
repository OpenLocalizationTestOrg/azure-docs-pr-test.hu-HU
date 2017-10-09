---
title: "Háló megbízható állapot szolgáltatáskezelő és megbízható gyűjtemény internals aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 651bfb52785a2475e4840cd471e87220d1936391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a><span data-ttu-id="5644f-103">Azure Service Fabric megbízható állapotkezelője és megbízható gyűjtemény belső funkciói</span><span class="sxs-lookup"><span data-stu-id="5644f-103">Azure Service Fabric Reliable State Manager and Reliable Collection internals</span></span>
<span data-ttu-id="5644f-104">Ez a dokumentum delves belül megbízható állapotkezelője és megbízható gyűjtemények toosee alapösszetevőket hello háttérben működése.</span><span class="sxs-lookup"><span data-stu-id="5644f-104">This document delves inside Reliable State Manager and Reliable Collections toosee how core components work behind hello scenes.</span></span>

> [!NOTE]
> <span data-ttu-id="5644f-105">Ez a dokumentum munkahelyi folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5644f-105">This document is work in-progress.</span></span> <span data-ttu-id="5644f-106">Adja hozzá a megjegyzések toothis cikk tootell velünk milyen témakör szeretné toolearn további információk.</span><span class="sxs-lookup"><span data-stu-id="5644f-106">Add comments toothis article tootell us what topic you would like toolearn more about.</span></span>
>

##  <a name="local-persistence-model-log-and-checkpoint"></a><span data-ttu-id="5644f-107">Helyi adatmegőrzési modell: naplóhoz és ellenőrzőpont</span><span class="sxs-lookup"><span data-stu-id="5644f-107">Local persistence model: log and checkpoint</span></span>
<span data-ttu-id="5644f-108">hello megbízható állapotkezelője és megbízható gyűjtemények hajtsa végre az adatmegőrzési modell, amely a naplóhoz és ellenőrzőpont nevezik.</span><span class="sxs-lookup"><span data-stu-id="5644f-108">hello Reliable State Manager and Reliable Collections follow a persistence model that is called Log and Checkpoint.</span></span>
<span data-ttu-id="5644f-109">Ebben a modellben minden állapotváltozás először jelentkezik be lemezt, és alkalmazza a memóriában.</span><span class="sxs-lookup"><span data-stu-id="5644f-109">In this model, each state change is logged on disk first and then applied in memory.</span></span>
<span data-ttu-id="5644f-110">hello teljes állapot maga is megőrződjenek csak esetenként (más néven</span><span class="sxs-lookup"><span data-stu-id="5644f-110">hello complete state itself is persisted only occasionally (a.k.a.</span></span> <span data-ttu-id="5644f-111">Ellenőrzőpont).</span><span class="sxs-lookup"><span data-stu-id="5644f-111">Checkpoint).</span></span>
<span data-ttu-id="5644f-112">hello előnye, hogy eltérések szekvenciális csak írási műveleteket a lemezen a jobb teljesítmény be vannak kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="5644f-112">hello benefit is that deltas are turned into sequential append-only writes on disk for improved performance.</span></span>

<span data-ttu-id="5644f-113">toobetter napló hello és ellenőrzőpont-modell, először nézzük hello végtelen lemez forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="5644f-113">toobetter understand hello Log and Checkpoint model, let’s first look at hello infinite disk scenario.</span></span>
<span data-ttu-id="5644f-114">hello megbízható állapotkezelője naplózza a minden műveletet a replikálás előtt.</span><span class="sxs-lookup"><span data-stu-id="5644f-114">hello Reliable State Manager logs every operation before it is replicated.</span></span>
<span data-ttu-id="5644f-115">Naplózás lehetővé teszi, hogy hello megbízható gyűjtemények tooapply hello művelet csak a memóriában.</span><span class="sxs-lookup"><span data-stu-id="5644f-115">Logging allows hello Reliable Collections tooapply hello operation only in memory.</span></span>
<span data-ttu-id="5644f-116">Naplók megmaradnak, mert akkor, amikor hello replika sikertelen lesz, és újra kell indítani, toobe kell hello megbízható állapotkezelője van elegendő információ áll rendelkezésre a napló tooreplay hello replika elvesztette minden hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="5644f-116">Since logs are persisted, even when hello replica fails and needs toobe restarted, hello Reliable State Manager has enough information in its log tooreplay all hello operations hello replica has lost.</span></span>
<span data-ttu-id="5644f-117">Hello lemez végtelen, naplórekordokat soha nem kell eltávolítani toobe és hello megbízható szükség gyűjtemény toomanage csak hello a memóriában.</span><span class="sxs-lookup"><span data-stu-id="5644f-117">As hello disk is infinite, log records never need toobe removed and hello Reliable Collection needs toomanage only hello in-memory state.</span></span>

<span data-ttu-id="5644f-118">Most hello véges lemez forgatókönyv vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="5644f-118">Now let’s look at hello finite disk scenario.</span></span>
<span data-ttu-id="5644f-119">Naplórekordok gyűlik össze, mert nincs elég szabad lemezterület hello megbízható állapotkezelője indul el.</span><span class="sxs-lookup"><span data-stu-id="5644f-119">As log records accumulate, hello Reliable State Manager will run out of disk space.</span></span>
<span data-ttu-id="5644f-120">Mielőtt ebben az esetben a hello megbízható állapotkezelője hello újabb rögzíti a napló toomake helyet kell tootruncate.</span><span class="sxs-lookup"><span data-stu-id="5644f-120">Before that happens, hello Reliable State Manager needs tootruncate its log toomake room for hello newer records.</span></span>
<span data-ttu-id="5644f-121">Megbízható állapot Manager kérések hello megbízható gyűjtemények toocheckpoint azok a memóriában toodisk.</span><span class="sxs-lookup"><span data-stu-id="5644f-121">Reliable State Manager requests hello Reliable Collections toocheckpoint their in-memory state toodisk.</span></span>
<span data-ttu-id="5644f-122">Ezen a ponton hello megbízható gyűjtemények szeretné megőrizni a memórián belüli állapotában.</span><span class="sxs-lookup"><span data-stu-id="5644f-122">At this point, hello Reliable Collections' would persist its in-memory state.</span></span>
<span data-ttu-id="5644f-123">Miután hello megbízható gyűjtemények végezze el az ellenőrzőpontokat, megbízható állapotkezelője hello hello napló toofree fel megfelelő mennyiségű lemezterületet is csonkolja.</span><span class="sxs-lookup"><span data-stu-id="5644f-123">Once hello Reliable Collections complete their checkpoints, hello Reliable State Manager can truncate hello log toofree up disk space.</span></span>
<span data-ttu-id="5644f-124">Hello replikának szüksége lesz a toobe újraindítása, ha megbízható gyűjtemények helyreállításához alkulcsaihoz állapotukra és hello megbízható állapotkezelője állítja helyre, és lejátssza hello állapot változtatások hello utolsó ellenőrzőpont óta történt.</span><span class="sxs-lookup"><span data-stu-id="5644f-124">When hello replica needs toobe restarted, Reliable Collections recover their checkpointed state, and hello Reliable State Manager recovers and plays back all hello state changes that occurred since hello last checkpoint.</span></span>

<span data-ttu-id="5644f-125">Ellenőrzőpontok hozzáadása egy másik értéket, hogy ez növeli a gyakori forgatókönyvek helyreállítási alkalommal.</span><span class="sxs-lookup"><span data-stu-id="5644f-125">Another value add of checkpointing is that it improves recovery times in common scenarios.</span></span> <span data-ttu-id="5644f-126">A napló tartalmazza minden művelet hello utolsó ellenőrzőpont óta bekövetkezett.</span><span class="sxs-lookup"><span data-stu-id="5644f-126">Log contains all operations that have happened since hello last checkpoint.</span></span>
<span data-ttu-id="5644f-127">Így például a megadott sor több értéket elem több verziója tartalmazhat megbízható szótárban.</span><span class="sxs-lookup"><span data-stu-id="5644f-127">So it may include multiple versions of an item like multiple values for a given row in Reliable Dictionary.</span></span>
<span data-ttu-id="5644f-128">Ezzel szemben egy megbízható gyűjtemény ellenőrzőpontokat csak hello kulcsok minden egyes érték legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="5644f-128">In contrast, a Reliable Collection checkpoints only hello latest version of each value for a key.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5644f-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5644f-129">Next steps</span></span>
* [<span data-ttu-id="5644f-130">Tranzakciók és a zárolásokat</span><span class="sxs-lookup"><span data-stu-id="5644f-130">Transactions and Locks</span></span>](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

