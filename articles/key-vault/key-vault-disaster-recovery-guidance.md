---
title: "Mi a teendő, ha az Azure szolgáltatás, amely befolyásolja az Azure Key Vault megszűnésének |} Microsoft Docs"
description: "Ismerje meg, mi a teendő az Azure-szolgáltatások becsukódjon, amely befolyásolja az Azure Key Vault."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 6419d54c54e7d19103419262b79e7a5268b2268c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a><span data-ttu-id="62d04-103">Az Azure Key Vault rendelkezésre állás és redundancia</span><span class="sxs-lookup"><span data-stu-id="62d04-103">Azure Key Vault availability and redundancy</span></span>
<span data-ttu-id="62d04-104">Az Azure Key Vault a redundancia csökkentése érdekében győződjön meg arról, hogy a kulcsok és titkos használhatóak maradnak arra az alkalmazás akkor is, ha a szolgáltatás egyes összetevők sikertelen rétege funkciókat.</span><span class="sxs-lookup"><span data-stu-id="62d04-104">Azure Key Vault features multiple layers of redundancy to make sure that your keys and secrets remain available to your application even if individual components of the service fail.</span></span>

<span data-ttu-id="62d04-105">A kulcstartót tartalmát replikálódnak a régión belül, és egy másodlagos régióban legalább 150 miles számítógépnél, de ugyanazon a földrajzi belül.</span><span class="sxs-lookup"><span data-stu-id="62d04-105">The contents of your key vault are replicated within the region and to a secondary region at least 150 miles away but within the same geography.</span></span> <span data-ttu-id="62d04-106">Ezzel a megoldással magas tartósság a kulcsok és titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="62d04-106">This maintains high durability of your keys and secrets.</span></span> <span data-ttu-id="62d04-107">Tekintse meg a [Azure párosítva régiók](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentumban talál részletes információt adott régióban párokat.</span><span class="sxs-lookup"><span data-stu-id="62d04-107">See the [Azure paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) document for details on specific region pairs.</span></span>

<span data-ttu-id="62d04-108">A kulcstároló szolgáltatáson belül az egyes összetevők sikertelen lesz, ha más összetevők abban a régióban lépéseket teljesíteni a kérést, győződjön meg arról, hogy van-e a funkcióinak csökkenése nélkül működhet.</span><span class="sxs-lookup"><span data-stu-id="62d04-108">If individual components within the key vault service fail, alternate components within the region step in to serve your request to make sure that there is no degradation of functionality.</span></span> <span data-ttu-id="62d04-109">Nem kell elindítani a teendője.</span><span class="sxs-lookup"><span data-stu-id="62d04-109">You do not need to take any action to trigger this.</span></span> <span data-ttu-id="62d04-110">Ez automatikusan történik, és lesz érzékelhető.</span><span class="sxs-lookup"><span data-stu-id="62d04-110">It happens automatically and will be transparent to you.</span></span>

<span data-ttu-id="62d04-111">Az esemény ritkán fordul elő, hogy a teljes Azure-régió nem érhető el, a rendszer a kérést, az Azure Key Vault az adott régióban végrehajtott automatikusan átirányítja (*átadja a feladatokat*) másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="62d04-111">In the rare event that an entire Azure region is unavailable, the requests that you make of Azure Key Vault in that region are automatically routed (*failed over*) to a secondary region.</span></span> <span data-ttu-id="62d04-112">Az elsődleges régióban újból elérhető lesz, ha rendszer kérést átirányítja vissza (*vissza nem sikerült*) az elsődleges régióban.</span><span class="sxs-lookup"><span data-stu-id="62d04-112">When the primary region is available again, requests are routed back (*failed back*) to the primary region.</span></span> <span data-ttu-id="62d04-113">Ebben az esetben nem kell tennie semmit, mert ez automatikusan megtörténik.</span><span class="sxs-lookup"><span data-stu-id="62d04-113">Again, you do not need to take any action because this happens automatically.</span></span>

<span data-ttu-id="62d04-114">Van néhány figyelmeztetések-érdemes figyelembe:</span><span class="sxs-lookup"><span data-stu-id="62d04-114">There are a few caveats to be aware of:</span></span>

* <span data-ttu-id="62d04-115">Régió feladatátvétel esetén a szolgáltatás számára, hogy a feladatátvétel néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="62d04-115">In the event of a region failover, it may take a few minutes for the service to fail over.</span></span> <span data-ttu-id="62d04-116">Ebben az időszakban irányuló kérelmek sikertelenek lehetnek mindaddig, amíg befejeződik a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="62d04-116">Requests that are made during this time may fail until the failover completes.</span></span>
* <span data-ttu-id="62d04-117">A feladatátvétel befejezése után a kulcstartót csak olvasható módban van.</span><span class="sxs-lookup"><span data-stu-id="62d04-117">After a failover is complete, your key vault is in read-only mode.</span></span> <span data-ttu-id="62d04-118">Ebben a módban támogatott kéréseket a következők:</span><span class="sxs-lookup"><span data-stu-id="62d04-118">Requests that are supported in this mode are:</span></span>
  * <span data-ttu-id="62d04-119">Kulcstároló naplófájljainak listája</span><span class="sxs-lookup"><span data-stu-id="62d04-119">List key vaults</span></span>
  * <span data-ttu-id="62d04-120">Kulcstároló tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="62d04-120">Get properties of key vaults</span></span>
  * <span data-ttu-id="62d04-121">Titkos kulcsainak listázása</span><span class="sxs-lookup"><span data-stu-id="62d04-121">List secrets</span></span>
  * <span data-ttu-id="62d04-122">Titkos kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="62d04-122">Get secrets</span></span>
  * <span data-ttu-id="62d04-123">Listázása</span><span class="sxs-lookup"><span data-stu-id="62d04-123">List keys</span></span>
  * <span data-ttu-id="62d04-124">(Tulajdonságainak) kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="62d04-124">Get (properties of) keys</span></span>
  * <span data-ttu-id="62d04-125">Titkosítás</span><span class="sxs-lookup"><span data-stu-id="62d04-125">Encrypt</span></span>
  * <span data-ttu-id="62d04-126">Visszafejtés</span><span class="sxs-lookup"><span data-stu-id="62d04-126">Decrypt</span></span>
  * <span data-ttu-id="62d04-127">Naplócsonkulási</span><span class="sxs-lookup"><span data-stu-id="62d04-127">Wrap</span></span>
  * <span data-ttu-id="62d04-128">Kicsomagolásához</span><span class="sxs-lookup"><span data-stu-id="62d04-128">Unwrap</span></span>
  * <span data-ttu-id="62d04-129">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="62d04-129">Verify</span></span>
  * <span data-ttu-id="62d04-130">Bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="62d04-130">Sign</span></span>
  * <span data-ttu-id="62d04-131">Biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="62d04-131">Backup</span></span>
* <span data-ttu-id="62d04-132">Miután a feladatátvétel nem sikerült vissza, az összes kérelemtípusok (beleértve az olvasás *és* írási kérelmeket szolgál) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="62d04-132">After a failover is failed back, all request types (including read *and* write requests) are available.</span></span>

