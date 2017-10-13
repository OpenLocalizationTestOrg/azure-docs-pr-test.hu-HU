---
title: "teljesítmény és méretezés, a VMware-replikáció tooAzure az Azure Site Recovery aaaPlan |} Microsoft Docs"
description: "Ez a cikk tooplan kapacitás és a skála használja, az Azure Site Recovery VMware virtuális gépek tooAzure replikálása esetén"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: rayne
ms.openlocfilehash: 551533ab7090d85c216be242ea92781deb8287ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-vmware-tooazure-replication"></a><span data-ttu-id="5ea5a-103">3. lépés: A kapacitás és a VMware tooAzure replikációs csoportok skálázási módjának tervezése</span><span class="sxs-lookup"><span data-stu-id="5ea5a-103">Step 3: Plan capacity and scaling for VMware tooAzure replication</span></span>

<span data-ttu-id="5ea5a-104">Ez a cikk toofigure jelent a kapacitástervezés és a méretezés, a helyszíni VMware virtuális gépek és fizikai kiszolgálók tooAzure való replikálása esetén használjon [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5ea5a-104">Use this article toofigure out planning for capacity and scaling, when replicating on-premises VMware VMs and physical servers tooAzure with [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="5ea5a-105">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5ea5a-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="how-do-i-start-capacity-planning"></a><span data-ttu-id="5ea5a-106">Hogyan kezdhetem meg, hogy a kapacitástervezés?</span><span class="sxs-lookup"><span data-stu-id="5ea5a-106">How do I start capacity planning?</span></span>

<span data-ttu-id="5ea5a-107">A replikálási környezetre vonatkozó információkat gyűjt, és majd a kapacitástervezés ezeket az adatokat, a cikkben a kijelölt hello szempontok használatával.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-107">You gather information about your replication environment, and then plan capacity using this information, together with hello considerations highlighted in this article.</span></span>


## <a name="gather-information"></a><span data-ttu-id="5ea5a-108">Adatainak összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="5ea5a-108">Gather information</span></span>

1. <span data-ttu-id="5ea5a-109">Töltse le a hello [telepítési Planner eszköz](https://aka.ms/asr-deployment-planner) a VMware-replikáció.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-109">Download hello [Deployment Planner tool](https://aka.ms/asr-deployment-planner) for VMware replication.</span></span>
2. <span data-ttu-id="5ea5a-110">[Ebben a cikkben](site-recovery-deployment-planner.md) toounderstand hogyan toorun hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-110">[Read this article](site-recovery-deployment-planner.md) toounderstand how toorun hello tool.</span></span>
3. <span data-ttu-id="5ea5a-111">Hello eszközzel kompatibilis és nem kompatibilis virtuális gépekhez, virtuális gépenként lemezek információt gyűjteni, és lemezenként kavarog adatokat.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-111">Using hello tool you gather information about compatible and incompatible VMs, disks per VM, and data churn per disk.</span></span> <span data-ttu-id="5ea5a-112">hello eszköz is magában foglalja az hálózat sávszélességigényét és hello Azure-infrastruktúra szükséges sikeres replikáció és a teszt feladatátvételhez.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-112">hello tool also covers network bandwidth requirements, and hello Azure infrastructure needed for successful replication and test failover.</span></span>

## <a name="replication-considerations"></a><span data-ttu-id="5ea5a-113">Adatreplikálási megfontolások</span><span class="sxs-lookup"><span data-stu-id="5ea5a-113">Replication considerations</span></span>

<span data-ttu-id="5ea5a-114">Mielőtt elkezdené a telepítést, jegyezze fel ezeket a szempontokat:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-114">Note these considerations before you start deployment:</span></span>

<span data-ttu-id="5ea5a-115">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-115">**Component**</span></span> | <span data-ttu-id="5ea5a-116">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-116">**Details**</span></span> |
--- | --- | ---
<span data-ttu-id="5ea5a-117">**Replikáció**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-117">**Replication**</span></span> | <span data-ttu-id="5ea5a-118">**Legfeljebb napi adatváltozási sebesség:** A védett gép csak egy folyamat-kiszolgálót is használhat, és a folyamat egyetlen kiszolgáló kezelni tud a napi adatváltozási sebesség too2 TB fel.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-118">**Maximum daily change rate:** A protected machine can only use one process server, and a single process server can handle a daily change rate up too2 TB.</span></span> <span data-ttu-id="5ea5a-119">Így 2 TB nem hello maximális napi módosulása arány, amely a védett gépek esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-119">Thus 2 TB is hello maximum daily data change rate that’s supported for a protected machine.</span></span><br/><br/> <span data-ttu-id="5ea5a-120">**Maximális átviteli sebesség:** A replikált gép tooone Azure storage-fiókot is tartozhatnak.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-120">**Maximum throughput:** A replicated machine can belong tooone storage account in Azure.</span></span> <span data-ttu-id="5ea5a-121">Standard szintű tárfiók kezelni tud a legfeljebb 20 000 kérelmek / másodperc, és azt javasoljuk, hogy a forrás gép too20 000 keresztül tartsa hello bemeneti/kimeneti műveletek (IOPS) másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-121">A standard storage account can handle a maximum of 20,000 requests per second, and we recommend that you keep hello number of input/output operations per second (IOPS) across a source machine too20,000.</span></span> <span data-ttu-id="5ea5a-122">Például ha a forrásgép 5 lemezzel rendelkezik, és egyes lemezek hello forrásgépen 120 IOPS (8 KB-os méret) hoz létre, majd lesz hello Azure lemez IOPS-korlát 500 belül.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-122">For example, if you have a source machine with 5 disks, and each disk generates 120 IOPS (8K size) on hello source machine, then it will be within hello Azure per disk IOPS limit of 500.</span></span> <span data-ttu-id="5ea5a-123">(a storage-fiókok szükséges hello száma egyenlő toohello teljes forrásgép IOPS, 20 000 osztva.)</span><span class="sxs-lookup"><span data-stu-id="5ea5a-123">(hello number of storage accounts required is equal toohello total source machine IOPS, divided by 20,000.)</span></span>

## <a name="configuration-server-capacity"></a><span data-ttu-id="5ea5a-124">Konfigurációs kiszolgáló kapacitásának</span><span class="sxs-lookup"><span data-stu-id="5ea5a-124">Configuration server capacity</span></span>

<span data-ttu-id="5ea5a-125">hello konfigurációs kiszolgáló összes védett gépeken futó munkaterhelések képes toohandle hello napi módosítási gyakorisága kapacitást kell lenniük, és elegendő sávszélesség toocontinuously replikálni kell adatok tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-125">hello configuration server should be able toohandle hello daily change rate capacity across all workloads running on protected machines, and needs sufficient bandwidth toocontinuously replicate data tooAzure Storage.</span></span>

<span data-ttu-id="5ea5a-126">Ajánlott eljárásként, megtalálhatja a konfigurációs kiszolgáló hello hello ugyanazon a hálózaton és a LAN-szegmens, hello tooprotect kívánt gépek.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-126">As a best practice, locate hello configuration server on hello same network and LAN segment as hello machines you want tooprotect.</span></span> <span data-ttu-id="5ea5a-127">Egy másik hálózati, de azt szeretné, hogy a tooprotect rendelkeznie kell a réteg 3 hálózati látható tooit gépek is található.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-127">It can be located on a different network, but machines you want tooprotect should have layer 3 network visibility tooit.</span></span>

## <a name="sizing-recommendations"></a><span data-ttu-id="5ea5a-128">Méretezési javaslatok</span><span class="sxs-lookup"><span data-stu-id="5ea5a-128">Sizing recommendations</span></span>

<span data-ttu-id="5ea5a-129">hello táblázat CPU alapuló méretezési javaslatokat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-129">hello table summarizes sizing recommendations based on CPU.</span></span>

<span data-ttu-id="5ea5a-130">**PROCESSZOR**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-130">**CPU**</span></span> | <span data-ttu-id="5ea5a-131">**Memória**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-131">**Memory**</span></span> | <span data-ttu-id="5ea5a-132">**Gyorsítótár-lemez mérete**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-132">**Cache disk size**</span></span> | <span data-ttu-id="5ea5a-133">**Adatváltozási sebesség**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-133">**Data change rate**</span></span> | <span data-ttu-id="5ea5a-134">**Védett gépek**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-134">**Protected machines**</span></span>
--- | --- | --- | --- | ---
<span data-ttu-id="5ea5a-135">8 Vcpu (2 sockets * @ 2,5 gigahertz [GHz] 4 mag)</span><span class="sxs-lookup"><span data-stu-id="5ea5a-135">8 vCPUs (2 sockets * 4 cores @ 2.5 gigahertz [GHz])</span></span> | <span data-ttu-id="5ea5a-136">16 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-136">16 GB</span></span> | <span data-ttu-id="5ea5a-137">300 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-137">300 GB</span></span> | <span data-ttu-id="5ea5a-138">500 GB vagy kevesebb</span><span class="sxs-lookup"><span data-stu-id="5ea5a-138">500 GB or less</span></span> | <span data-ttu-id="5ea5a-139">100-nál kevesebb gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-139">Replicate less than 100 machines.</span></span>
<span data-ttu-id="5ea5a-140">12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag)</span><span class="sxs-lookup"><span data-stu-id="5ea5a-140">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz)</span></span> | <span data-ttu-id="5ea5a-141">18 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-141">18 GB</span></span> | <span data-ttu-id="5ea5a-142">600 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-142">600 GB</span></span> | <span data-ttu-id="5ea5a-143">500 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-143">500 GB too1 TB</span></span> | <span data-ttu-id="5ea5a-144">100-150 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-144">Replicate between 100-150 machines.</span></span>
<span data-ttu-id="5ea5a-145">16 Vcpu (2 sockets * @ 2,5 GHz-es 8 mag)</span><span class="sxs-lookup"><span data-stu-id="5ea5a-145">16 vCPUs (2 sockets * 8 cores @ 2.5 GHz)</span></span> | <span data-ttu-id="5ea5a-146">32 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-146">32 GB</span></span> | <span data-ttu-id="5ea5a-147">1 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-147">1 TB</span></span> | <span data-ttu-id="5ea5a-148">1 TB-os too2 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-148">1 TB too2 TB</span></span> | <span data-ttu-id="5ea5a-149">150-200 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-149">Replicate between 150-200 machines.</span></span>
<span data-ttu-id="5ea5a-150">Egy másik folyamat-kiszolgáló központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5ea5a-150">Deploy another process server</span></span> | | | <span data-ttu-id="5ea5a-151">> 2 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-151">> 2 TB</span></span> | <span data-ttu-id="5ea5a-152">Ha több mint 200 gépeket replikál, vagy ha hello napi módosulása aránya meghaladja a 2 TB további folyamat kiszolgálók telepítése</span><span class="sxs-lookup"><span data-stu-id="5ea5a-152">Deploy additional process servers if you're replicating more than 200 machines, or if hello daily data change rate exceeds 2 TB.</span></span>

<span data-ttu-id="5ea5a-153">Az elemek magyarázata:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-153">Where:</span></span>

* <span data-ttu-id="5ea5a-154">Minden forrásgép 100 GB, 3 lemezzel van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-154">Each source machine is configured with 3 disks of 100 GB each.</span></span>
* <span data-ttu-id="5ea5a-155">Összehasonlítási tárolása 8 SAS-meghajtókkal 10 k fordulat/PERCES, RAID 10-es, gyorsítótár lemez mérések használtuk.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-155">We used benchmarking storage of 8 SAS drives of 10 K RPM, with RAID 10, for cache disk measurements.</span></span>

## <a name="process-server-capacity"></a><span data-ttu-id="5ea5a-156">Feldolgozása a kiszolgáló kapacitásának</span><span class="sxs-lookup"><span data-stu-id="5ea5a-156">Process server capacity</span></span>


<span data-ttu-id="5ea5a-157">hello folyamatkiszolgáló védett gépek kap replikációs adatokat, és gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-157">hello process server receives replication data from protected machines, and optimizes it with caching, compression, and encryption.</span></span> <span data-ttu-id="5ea5a-158">Majd küldi hello adatok tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-158">Then it sends hello data tooAzure.</span></span>

- <span data-ttu-id="5ea5a-159">hello folyamat server gépnek rendelkeznie kell elegendő erőforrást tooperform ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-159">hello process server machine should have sufficient resources tooperform these tasks.</span></span>
- <span data-ttu-id="5ea5a-160">hello első folyamatkiszolgáló hello konfigurációs kiszolgálón alapértelmezés szerint telepítve van.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-160">hello first process server is installed by default on hello configuration server.</span></span> <span data-ttu-id="5ea5a-161">További folyamat kiszolgálók tooscale telepítheti a környezetében.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-161">You can deploy additional process servers tooscale your environment.</span></span>
- <span data-ttu-id="5ea5a-162">hello folyamat kiszolgáló használ a lemezalapú gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-162">hello process server uses a disk-based cache.</span></span> <span data-ttu-id="5ea5a-163">Használjon külön gyorsítótár lemezt 600 GB vagy több toohandle adatok változásait a hálózati szűk vagy leállás hello esemény tárolja.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-163">Use a separate cache disk of 600 GB or more toohandle data changes stored in hello event of a network bottleneck or outage.</span></span>
- <span data-ttu-id="5ea5a-164">Ha több mint 200 gépek kell tooprotect, vagy hello napi változási sebessége nagyobb, mint 2 TB, folyamat kiszolgálók toohandle hello replikációs terhelés is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-164">If you need tooprotect more than 200 machines, or hello daily change rate is greater than 2 TB, you can add process servers toohandle hello replication load.</span></span> <span data-ttu-id="5ea5a-165">kimenő tooscale, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-165">tooscale out, you can:</span></span>
    - <span data-ttu-id="5ea5a-166">A konfigurációs kiszolgálók hello számának növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-166">Increase hello number of configuration servers.</span></span> <span data-ttu-id="5ea5a-167">Például védelmet biztosíthat a too400 gépek két konfigurációs kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-167">For example, you can protect up too400 machines with two configuration servers.</span></span>
    - <span data-ttu-id="5ea5a-168">Folyamat további kiszolgálók hozzáadásához, és ezek toohandle forgalom helyett (vagy kívül) hello konfigurációs kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-168">Add more process servers, and use these toohandle traffic instead of (or in addition to) hello configuration server.</span></span>


### <a name="example-process-server-scaling"></a><span data-ttu-id="5ea5a-169">Példa folyamat kiszolgálóinak méretezéséről</span><span class="sxs-lookup"><span data-stu-id="5ea5a-169">Example process server scaling</span></span>

<span data-ttu-id="5ea5a-170">a következő táblázat hello egy olyan forgatókönyvet, amelyben ismerteti:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-170">hello following table describes a scenario in which:</span></span>

* <span data-ttu-id="5ea5a-171">Toouse hello konfigurációs kiszolgáló nem készül, folyamat-kiszolgálóként.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-171">You're not planning toouse hello configuration server as a process server.</span></span>
* <span data-ttu-id="5ea5a-172">Egy további folyamatkiszolgáló beállítása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-172">You've set up an additional process server.</span></span>
* <span data-ttu-id="5ea5a-173">Védett virtuális gépek toouse hello további folyamatkiszolgáló konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-173">You've configured protected virtual machines toouse hello additional process server.</span></span>
* <span data-ttu-id="5ea5a-174">Minden védett forrásgép 100 GB három lemez van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-174">Each protected source machine is configured with three disks of 100 GB each.</span></span>

<span data-ttu-id="5ea5a-175">**Konfigurációs kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-175">**Configuration server**</span></span> | <span data-ttu-id="5ea5a-176">**További folyamatkiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-176">**Additional process server**</span></span> | <span data-ttu-id="5ea5a-177">**Gyorsítótár-lemez mérete**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-177">**Cache disk size**</span></span> | <span data-ttu-id="5ea5a-178">**Adatváltozási sebesség**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-178">**Data change rate**</span></span> | <span data-ttu-id="5ea5a-179">**Védett gépek**</span><span class="sxs-lookup"><span data-stu-id="5ea5a-179">**Protected machines**</span></span>
--- | --- | --- | --- | ---
<span data-ttu-id="5ea5a-180">8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB memória</span><span class="sxs-lookup"><span data-stu-id="5ea5a-180">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 16 GB memory</span></span> | <span data-ttu-id="5ea5a-181">4 Vcpu (2 sockets * @ 2,5 GHz-es 2 mag), 8 GB memória</span><span class="sxs-lookup"><span data-stu-id="5ea5a-181">4 vCPUs (2 sockets * 2 cores @ 2.5 GHz), 8 GB memory</span></span> | <span data-ttu-id="5ea5a-182">300 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-182">300 GB</span></span> | <span data-ttu-id="5ea5a-183">250 GB vagy kevesebb</span><span class="sxs-lookup"><span data-stu-id="5ea5a-183">250 GB or less</span></span> | <span data-ttu-id="5ea5a-184">85 vagy kevesebb gépek replikálása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-184">Replicate 85 or fewer machines.</span></span>
<span data-ttu-id="5ea5a-185">8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 16 GB memória</span><span class="sxs-lookup"><span data-stu-id="5ea5a-185">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 16 GB memory</span></span> | <span data-ttu-id="5ea5a-186">8 Vcpu (2 sockets * @ 2,5 GHz, 4 mag), 12 GB memória</span><span class="sxs-lookup"><span data-stu-id="5ea5a-186">8 vCPUs (2 sockets * 4 cores @ 2.5 GHz), 12 GB memory</span></span> | <span data-ttu-id="5ea5a-187">600 GB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-187">600 GB</span></span> | <span data-ttu-id="5ea5a-188">250 GB too1 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-188">250 GB too1 TB</span></span> | <span data-ttu-id="5ea5a-189">Replikálja a 85-150 gépek között.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-189">Replicate between 85-150 machines.</span></span>
<span data-ttu-id="5ea5a-190">12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag), 18 GB memória</span><span class="sxs-lookup"><span data-stu-id="5ea5a-190">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz), 18 GB memory</span></span> | <span data-ttu-id="5ea5a-191">12 Vcpu (2 sockets * @ 2,5 GHz-es 6 mag) 24 GB memória</span><span class="sxs-lookup"><span data-stu-id="5ea5a-191">12 vCPUs (2 sockets * 6 cores @ 2.5 GHz) 24 GB memory</span></span> | <span data-ttu-id="5ea5a-192">1 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-192">1 TB</span></span> | <span data-ttu-id="5ea5a-193">1 TB-os too2 TB</span><span class="sxs-lookup"><span data-stu-id="5ea5a-193">1 TB too2 TB</span></span> | <span data-ttu-id="5ea5a-194">150-225 gépek közti replikálásához.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-194">Replicate between 150-225 machines.</span></span>

<span data-ttu-id="5ea5a-195">amelyben a kiszolgálók méretezése hello módja méretezett és kibővített modell beállításoktól függ.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-195">hello way in which you scale your servers depends on your preference for a scale-up or scale-out model.</span></span>  <span data-ttu-id="5ea5a-196">Vertikális felskálázás néhány csúcskategóriás konfigurációs és folyamat-kiszolgálók üzembe helyezésével, vagy kevesebb erőforrást további kiszolgálók üzembe helyezésével kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-196">You scale up by deploying a few high-end configuration and process servers, or scale out by deploying more servers with fewer resources.</span></span> <span data-ttu-id="5ea5a-197">Például ha tooprotect 220 gépek van szüksége, módszerekkel hello alábbi:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-197">For example, if you need tooprotect 220 machines, you could do either of hello following:</span></span>

* <span data-ttu-id="5ea5a-198">12 vCPU, 18 GB memória, és egy további folyamatkiszolgáló 12 vCPU, 24 GB memóriát a konfigurációs kiszolgáló hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-198">Set up hello configuration server with 12 vCPU, 18 GB of memory, and an additional process server with 12 vCPU, 24 GB of memory.</span></span> <span data-ttu-id="5ea5a-199">Védett gépek toouse hello további folyamatkiszolgáló csak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-199">Configure protected machines toouse hello additional process server only.</span></span>
* <span data-ttu-id="5ea5a-200">(2 darab 8 vCPU, 16 GB RAM) két konfigurációs kiszolgáló és két további folyamat-kiszolgáló (1 x 8 vCPU és 4 vCPU x 1 toohandle 135-ös + 85 [220] gépek) beállítása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-200">Set up two configuration servers (2 x 8 vCPU, 16 GB RAM) and two additional process servers (1 x 8 vCPU and 4 vCPU x 1 toohandle 135 + 85 [220] machines).</span></span> <span data-ttu-id="5ea5a-201">Védett gépek toouse hello további folyamat csak kiszolgálók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-201">Configure protected machines toouse hello additional process servers only.</span></span>

## <a name="deploy-additional-process-servers"></a><span data-ttu-id="5ea5a-202">További folyamat-kiszolgálók központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5ea5a-202">Deploy additional process servers</span></span>

<span data-ttu-id="5ea5a-203">Hajtsa végre az ezen utasításokat tooset fel egy további folyamatkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-203">Follow these instructions tooset up an additional process server.</span></span> <span data-ttu-id="5ea5a-204">Hello kiszolgáló telepítése után telepít át forrás gépek toouse azt.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-204">After setting up hello server, you migrate source machines toouse it.</span></span>

1. <span data-ttu-id="5ea5a-205">A **Site Recovery kiszolgálók**, kattintson a hello konfigurációs kiszolgáló > **+ Folyamatkiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-205">In **Site Recovery servers**, click hello configuration server > **+Process Server**.</span></span>
2. <span data-ttu-id="5ea5a-206">A **kiszolgálótípus**, kattintson a **folyamat server (helyszíni)**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-206">In **Server type**, click **Process server (on-premises)**.</span></span>

    ![Folyamatkiszolgáló](./media/vmware-walkthrough-capacity/migrate-ps2.png)
3. <span data-ttu-id="5ea5a-208">Töltse le a hello Site Recovery az egységes telepítő fájlját.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-208">Download hello Site Recovery Unified Setup file.</span></span>
4. <span data-ttu-id="5ea5a-209">Futtassa a telepítő tooinstall hello folyamatkiszolgáló, és regisztrálja azt hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-209">Run setup tooinstall hello process server, and register it in hello vault.</span></span>
5. <span data-ttu-id="5ea5a-210">A **megkezdése előtt**, jelölje be **további folyamat kiszolgálók tooscale ki a központi telepítés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-210">In **Before you begin**, select **Add additional process servers tooscale out deployment**.</span></span>
6. <span data-ttu-id="5ea5a-211">A **konfigurációs kiszolgáló adatait**, hello hello konfigurációs kiszolgáló IP-címét adja meg, és hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-211">In **Configuration Server Details**, specify hello IP address of hello configuration server, and hello passphrase.</span></span> <span data-ttu-id="5ea5a-212">Ha még nem rendelkezik hello jelszót, töltse le innen futtatásával **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** hello konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-212">If you don't have hello passphrase, get it by running **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe –n** on hello configuration server.</span></span>

    ![Konfigurációs kiszolgáló](./media/vmware-walkthrough-capacity/add-ps2.png)
7. <span data-ttu-id="5ea5a-214">Fejezze be a telepítő hello hello részeinek pontosan ugyanúgy hello konfigurációs kiszolgáló beállításakor.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-214">Complete hello rest of setup in hello same way you did when you set up hello configuration server.</span></span>

### <a name="migrate-machines-toouse-hello-process-server"></a><span data-ttu-id="5ea5a-215">Telepítse át a gépek toouse hello folyamatkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="5ea5a-215">Migrate machines toouse hello process server</span></span>

1. <span data-ttu-id="5ea5a-216">A **beállítások** > **Site Recovery kiszolgálók**, hello konfigurációs kiszolgálón kattintson > **kiszolgálók feldolgozni**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-216">In **Settings** > **Site Recovery servers**, click hello configuration server > **Process servers**.</span></span>
2. <span data-ttu-id="5ea5a-217">Kattintson a jobb gombbal hello folyamatkiszolgáló jelenleg > **kapcsoló**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-217">Right-click hello process server currently in use > **Switch**.</span></span>

    ![Folyamatkiszolgáló kapcsoló](./media/vmware-walkthrough-capacity/migrate-ps3.png)
3. <span data-ttu-id="5ea5a-219">A **válassza folyamat-célkiszolgálót**, jelölje be hello folyamatkiszolgáló toouse szeretné, és válassza ki a hello virtuális gépeket, hogy hello kiszolgáló fogja kezelni.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-219">In **Select target process server**, select hello process server you want toouse, and select hello VMs that hello server will handle.</span></span>
4. <span data-ttu-id="5ea5a-220">Kattintson a hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-220">Click hello information icon.</span></span> <span data-ttu-id="5ea5a-221">döntések betöltése, és szükséges tooreplicate jelenik meg minden kijelölt virtuális gép toohello új folyamatkiszolgáló átlagos lemezterület hello elvégezte toohelp.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-221">toohelp you make load decisions, hello average space that's needed tooreplicate each selected VM toohello new process server is displayed.</span></span>
5. <span data-ttu-id="5ea5a-222">Kattintson a hello pipa toostart replikációs toohello új folyamatkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-222">Click hello check mark toostart replication toohello new process server.</span></span>

## <a name="control-network-bandwidth"></a><span data-ttu-id="5ea5a-223">Hálózati sávszélesség szabályozására</span><span class="sxs-lookup"><span data-stu-id="5ea5a-223">Control network bandwidth</span></span>

<span data-ttu-id="5ea5a-224">Futtatása után [hello telepítési Planner eszköz](site-recovery-deployment-planner.md) toocalculate hello sávszélesség van szüksége a replikáció (hello kezdeti replikáció és majd különbözeti), megadhatja a beállítások több replikáláshoz használt sávszélességet hello mennyisége:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-224">After you run [hello Deployment Planner tool](site-recovery-deployment-planner.md) toocalculate hello bandwidth you need for replication (hello initial replication and then delta), you can control hello amount of bandwidth used for replication using a couple of options:</span></span>

* <span data-ttu-id="5ea5a-225">**A sávszélesség szabályozását**: tooAzure replikáló VMware-forgalom halad át egy adott folyamat kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-225">**Throttle bandwidth**: VMware traffic that replicates tooAzure goes through a specific process server.</span></span> <span data-ttu-id="5ea5a-226">Beállíthatja a hello rendszereken futó folyamat kiszolgálóként sávszélesség szabályozását.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-226">You can throttle bandwidth on hello machines running as process servers.</span></span>
* <span data-ttu-id="5ea5a-227">**Sávszélesség befolyásolja**: hello sávszélesség-replikációhoz használt néhány beállításkulcsok használatával is szabályozhatja:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-227">**Influence bandwidth**: You can influence hello bandwidth used for replication by using a couple of registry keys:</span></span>
  * <span data-ttu-id="5ea5a-228">Hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** beállításazonosítót hello egy lemezen adatátvitelre (kezdeti vagy változásreplikálásra replikációs) használt szálak számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-228">hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** registry value specifies hello number of threads that are used for data transfer (initial or delta replication) of a disk.</span></span> <span data-ttu-id="5ea5a-229">A nagyobb érték növeli a hello replikációhoz használt hálózati sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-229">A higher value increases hello network bandwidth used for replication.</span></span>
  * <span data-ttu-id="5ea5a-230">Hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** hello adatátvitel feladat-visszavétel során használt szálak számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-230">hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** specifies hello number of threads used for data transfer during failback.</span></span>

### <a name="throttle-bandwidth"></a><span data-ttu-id="5ea5a-231">Sávszélesség szabályozása</span><span class="sxs-lookup"><span data-stu-id="5ea5a-231">Throttle bandwidth</span></span>

1. <span data-ttu-id="5ea5a-232">Nyissa meg a hello Azure Backup szolgáltatás MMC beépülő modul a hello gép működő hello folyamatkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-232">Open hello Azure Backup MMC snap-in on hello machine acting as hello process server.</span></span> <span data-ttu-id="5ea5a-233">Alapértelmezés szerint a helyi biztonsági mentés érhető hello asztalon, vagy a mappa a következő hello: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-233">By default, a shortcut for Backup is available on hello desktop, or in hello following folder: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="5ea5a-234">Kattintson a beépülő modul hello **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-234">In hello snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="5ea5a-235">A hello **sávszélesség-szabályozási** lapon jelölje be **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-235">On hello **Throttling** tab, select **Enable internet bandwidth usage throttling for backup operations**.</span></span>
4. <span data-ttu-id="5ea5a-236">Állítsa be a hello munkaórákra és a munkaórákon kívüli időre.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-236">Set hello limits for work and non-work hours.</span></span> <span data-ttu-id="5ea5a-237">Érvényes tartomány: az 512 kbit/s too102 MB / s.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-237">Valid ranges are from 512 Kbps too102 Mbps per second.</span></span>

    ![Szabályozás](./media/vmware-walkthrough-capacity/throttle2.png)

<span data-ttu-id="5ea5a-239">Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-239">You can also use hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset throttling.</span></span> <span data-ttu-id="5ea5a-240">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="5ea5a-240">Here's a sample:</span></span>

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

<span data-ttu-id="5ea5a-241">A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-241">**Set-OBMachineSetting -NoThrottle** indicates that no throttling is required.</span></span>

### <a name="influence-network-bandwidth-for-a-vm"></a><span data-ttu-id="5ea5a-242">A hálózati sávszélesség szabályozása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5ea5a-242">Influence network bandwidth for a VM</span></span>

1. <span data-ttu-id="5ea5a-243">A virtuális gép hello beállításjegyzékében, váltson túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-243">In registry of hello VM, go too**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.</span></span>
   * <span data-ttu-id="5ea5a-244">tooinfluence hello sávszélesség forgalom replikációs lemezen hello értékének módosítása **UploadThreadsPerVM**, vagy hozzon létre hello kulcsot, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-244">tooinfluence hello bandwidth traffic on a replicating disk, modify hello value of **UploadThreadsPerVM**, or create hello key if it doesn't exist.</span></span>
   * <span data-ttu-id="5ea5a-245">tooinfluence hello sávszélesség feladatátvételi forgalomhoz, az Azure-ból hello értékének módosítása **DownloadThreadsPerVM**.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-245">tooinfluence hello bandwidth for failback traffic from Azure, modify hello value of **DownloadThreadsPerVM**.</span></span>
2. <span data-ttu-id="5ea5a-246">hello alapértelmezett értéke 4.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-246">hello default value is 4.</span></span> <span data-ttu-id="5ea5a-247">Egy szükségesnél több erőforrással ellátott hálózatban ezek a kulcsok kell módosítani.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-247">In an overprovisioned network, these registry keys should be modified.</span></span> <span data-ttu-id="5ea5a-248">hello maximális 32.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-248">hello maximum is 32.</span></span> <span data-ttu-id="5ea5a-249">Forgalom toooptimize hello érték figyelése.</span><span class="sxs-lookup"><span data-stu-id="5ea5a-249">Monitor traffic toooptimize hello value.</span></span>




## <a name="next-steps"></a><span data-ttu-id="5ea5a-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ea5a-250">Next steps</span></span>

<span data-ttu-id="5ea5a-251">Nyissa meg túl[4. lépés: hálózati terv](vmware-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="5ea5a-251">Go too[Step 4: Plan networking](vmware-walkthrough-network.md).</span></span>