---
title: "egy Azure-szolgáltatások ideiglenesek, amely befolyásolja az Azure Key Vault hello eseményben aaaWhat toodo |} Microsoft Docs"
description: "Ismerje meg, milyen toodo egy Azure-szolgáltatások ideiglenesek, amely befolyásolja az Azure Key Vault hello eseményben."
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
ms.openlocfilehash: 88eec82ada401a28323b3eea126168185ba4cdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a><span data-ttu-id="16805-103">Az Azure Key Vault rendelkezésre állás és redundancia</span><span class="sxs-lookup"><span data-stu-id="16805-103">Azure Key Vault availability and redundancy</span></span>
<span data-ttu-id="16805-104">Az Azure Key Vault funkciókat rétege redundancia toomake meg arról, hogy a kulcsok és titkos továbbra is elérhető tooyour alkalmazás akkor is, ha egyes összetevőinek hello szolgáltatás sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="16805-104">Azure Key Vault features multiple layers of redundancy toomake sure that your keys and secrets remain available tooyour application even if individual components of hello service fail.</span></span>

<span data-ttu-id="16805-105">hello a kulcstartót tartalmát replikálódnak hello régió és tooa másodlagos régióba belül legalább 150 miles számítógépnél, de hello belül azonos földrajzi hely.</span><span class="sxs-lookup"><span data-stu-id="16805-105">hello contents of your key vault are replicated within hello region and tooa secondary region at least 150 miles away but within hello same geography.</span></span> <span data-ttu-id="16805-106">Ezzel a megoldással magas tartósság a kulcsok és titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="16805-106">This maintains high durability of your keys and secrets.</span></span> <span data-ttu-id="16805-107">Lásd: hello [Azure párosítva régiók](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentumban talál részletes információt adott régióban párokat.</span><span class="sxs-lookup"><span data-stu-id="16805-107">See hello [Azure paired regions](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) document for details on specific region pairs.</span></span>

<span data-ttu-id="16805-108">Hello kulcstároló szolgáltatáson belül az egyes összetevők sikertelen lesz, ha más összetevők hello régión belül lépést tooserve a kérelem toomake, hogy van-e a funkcióinak csökkenése nélkül működhet.</span><span class="sxs-lookup"><span data-stu-id="16805-108">If individual components within hello key vault service fail, alternate components within hello region step in tooserve your request toomake sure that there is no degradation of functionality.</span></span> <span data-ttu-id="16805-109">Nem kell tootake bármely művelet tootrigger ez.</span><span class="sxs-lookup"><span data-stu-id="16805-109">You do not need tootake any action tootrigger this.</span></span> <span data-ttu-id="16805-110">Automatikusan történik, és transzparens tooyou lesz.</span><span class="sxs-lookup"><span data-stu-id="16805-110">It happens automatically and will be transparent tooyou.</span></span>

<span data-ttu-id="16805-111">Hello ritka esemény, hogy a teljes Azure-régió nem érhető el, az Azure Key Vault az adott régióban végrehajtott hello kérelmek automatikusan útválasztása (*átadja a feladatokat*) tooa másodlagos régióba.</span><span class="sxs-lookup"><span data-stu-id="16805-111">In hello rare event that an entire Azure region is unavailable, hello requests that you make of Azure Key Vault in that region are automatically routed (*failed over*) tooa secondary region.</span></span> <span data-ttu-id="16805-112">Hello elsődleges régió újból elérhető lesz, ha rendszer kérést átirányítja vissza (*vissza nem sikerült*) toohello elsődleges régióban.</span><span class="sxs-lookup"><span data-stu-id="16805-112">When hello primary region is available again, requests are routed back (*failed back*) toohello primary region.</span></span> <span data-ttu-id="16805-113">Ebben az esetben nem kell tootake bármely művelet, mert ez automatikusan megtörténik.</span><span class="sxs-lookup"><span data-stu-id="16805-113">Again, you do not need tootake any action because this happens automatically.</span></span>

<span data-ttu-id="16805-114">Van néhány figyelmeztetések toobe tudomást:</span><span class="sxs-lookup"><span data-stu-id="16805-114">There are a few caveats toobe aware of:</span></span>

* <span data-ttu-id="16805-115">A régió feladatátvétel hello esetben hello szolgáltatás toofail néhány percig is eltarthat,.</span><span class="sxs-lookup"><span data-stu-id="16805-115">In hello event of a region failover, it may take a few minutes for hello service toofail over.</span></span> <span data-ttu-id="16805-116">Ebben az időszakban irányuló kérelmek hello feladatátvételi befejezéséig sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="16805-116">Requests that are made during this time may fail until hello failover completes.</span></span>
* <span data-ttu-id="16805-117">A feladatátvétel befejezése után a kulcstartót csak olvasható módban van.</span><span class="sxs-lookup"><span data-stu-id="16805-117">After a failover is complete, your key vault is in read-only mode.</span></span> <span data-ttu-id="16805-118">Ebben a módban támogatott kéréseket a következők:</span><span class="sxs-lookup"><span data-stu-id="16805-118">Requests that are supported in this mode are:</span></span>
  * <span data-ttu-id="16805-119">Kulcstároló naplófájljainak listája</span><span class="sxs-lookup"><span data-stu-id="16805-119">List key vaults</span></span>
  * <span data-ttu-id="16805-120">Kulcstároló tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="16805-120">Get properties of key vaults</span></span>
  * <span data-ttu-id="16805-121">Titkos kulcsainak listázása</span><span class="sxs-lookup"><span data-stu-id="16805-121">List secrets</span></span>
  * <span data-ttu-id="16805-122">Titkos kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="16805-122">Get secrets</span></span>
  * <span data-ttu-id="16805-123">Listázása</span><span class="sxs-lookup"><span data-stu-id="16805-123">List keys</span></span>
  * <span data-ttu-id="16805-124">(Tulajdonságainak) kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="16805-124">Get (properties of) keys</span></span>
  * <span data-ttu-id="16805-125">Titkosítás</span><span class="sxs-lookup"><span data-stu-id="16805-125">Encrypt</span></span>
  * <span data-ttu-id="16805-126">Visszafejtés</span><span class="sxs-lookup"><span data-stu-id="16805-126">Decrypt</span></span>
  * <span data-ttu-id="16805-127">Naplócsonkulási</span><span class="sxs-lookup"><span data-stu-id="16805-127">Wrap</span></span>
  * <span data-ttu-id="16805-128">Kicsomagolásához</span><span class="sxs-lookup"><span data-stu-id="16805-128">Unwrap</span></span>
  * <span data-ttu-id="16805-129">Ellenőrzés</span><span class="sxs-lookup"><span data-stu-id="16805-129">Verify</span></span>
  * <span data-ttu-id="16805-130">Bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="16805-130">Sign</span></span>
  * <span data-ttu-id="16805-131">Biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="16805-131">Backup</span></span>
* <span data-ttu-id="16805-132">Miután a feladatátvétel nem sikerült vissza, az összes kérelemtípusok (beleértve az olvasás *és* írási kérelmeket szolgál) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="16805-132">After a failover is failed back, all request types (including read *and* write requests) are available.</span></span>

