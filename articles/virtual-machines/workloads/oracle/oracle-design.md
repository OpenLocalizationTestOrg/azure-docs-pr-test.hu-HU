---
title: "Azure-adatbázis aaaDesign és alkalmazzon egy Oracle |} Microsoft Docs"
description: "Tervezése és megvalósítása az Oracle-adatbázishoz az Azure környezetben."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 8fa1207458695df1c7330ec626888b1b6b8d8939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a><span data-ttu-id="b4dab-103">Tervezése és megvalósítása az Oracle-adatbázishoz az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b4dab-103">Design and implement an Oracle database in Azure</span></span>

## <a name="assumptions"></a><span data-ttu-id="b4dab-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b4dab-104">Assumptions</span></span>

- <span data-ttu-id="b4dab-105">Arra készül, toomigrate helyszíni tooAzure az Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b4dab-105">You're planning toomigrate an Oracle database from on-premises tooAzure.</span></span>
- <span data-ttu-id="b4dab-106">Van hello ismeretét különböző metrikák Oracle AWR jelentésekben.</span><span class="sxs-lookup"><span data-stu-id="b4dab-106">You have an understanding of hello various metrics in Oracle AWR reports.</span></span>
- <span data-ttu-id="b4dab-107">Rendelkezik egy alapkonfiguráció ismeretekkel az alkalmazások teljesítményének és a platform felhasználását.</span><span class="sxs-lookup"><span data-stu-id="b4dab-107">You have a baseline understanding of application performance and platform utilization.</span></span>

## <a name="goals"></a><span data-ttu-id="b4dab-108">Célok</span><span class="sxs-lookup"><span data-stu-id="b4dab-108">Goals</span></span>

- <span data-ttu-id="b4dab-109">Megértéséhez hogyan toooptimize az Oracle-telepítés az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b4dab-109">Understand how toooptimize your Oracle deployment in Azure.</span></span>
- <span data-ttu-id="b4dab-110">Fedezze fel teljesítményének hangolása beállításait az Azure környezetben Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="b4dab-110">Explore performance tuning options for an Oracle database in an Azure environment.</span></span>

## <a name="hello-differences-between-an-on-premises-and-azure-implementation"></a><span data-ttu-id="b4dab-111">egy helyszíni közötti különbségek hello és Azure végrehajtása</span><span class="sxs-lookup"><span data-stu-id="b4dab-111">hello differences between an on-premises and Azure implementation</span></span> 

<span data-ttu-id="b4dab-112">Az alábbiakban olyan fontos dolgok tookeep szem előtt, amikor, amelybe migrálna a helyszíni alkalmazások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b4dab-112">Following are some important things tookeep in mind when you're migrating on-premises applications tooAzure.</span></span> 

<span data-ttu-id="b4dab-113">Egy fontos különbség az, hogy az Azure-megvalósításban erőforrások, például a virtuális gépek, a lemezek és a virtuális hálózatok megosztott egymás között.</span><span class="sxs-lookup"><span data-stu-id="b4dab-113">One important difference is that in an Azure implementation, resources such as VMs, disks, and virtual networks are shared among other clients.</span></span> <span data-ttu-id="b4dab-114">Ezenkívül erőforrásokat is lehet halmozódni hello követelmények alapján.</span><span class="sxs-lookup"><span data-stu-id="b4dab-114">In addition, resources can be throttled based on hello requirements.</span></span> <span data-ttu-id="b4dab-115">Helyett összpontosító elkerülése sikertelenek lesznek (MTBF), Azure van, ahol inkább a még működő hello hiba (MTTR).</span><span class="sxs-lookup"><span data-stu-id="b4dab-115">Instead of focusing on avoiding failing (MTBF), Azure is more focused on surviving hello failure (MTTR).</span></span>

<span data-ttu-id="b4dab-116">hello alábbi táblázat néhány hello különbségei egy helyszíni megvalósítás és az Oracle-adatbázishoz Azure megvalósítását.</span><span class="sxs-lookup"><span data-stu-id="b4dab-116">hello following table lists some of hello differences between an on-premises implementation and an Azure implementation of an Oracle database.</span></span>

> 
> |  | <span data-ttu-id="b4dab-117">**Helyszíni megvalósítás**</span><span class="sxs-lookup"><span data-stu-id="b4dab-117">**On-premises implementation**</span></span> | <span data-ttu-id="b4dab-118">**Az Azure végrehajtása**</span><span class="sxs-lookup"><span data-stu-id="b4dab-118">**Azure implementation**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="b4dab-119">**Hálózat**</span><span class="sxs-lookup"><span data-stu-id="b4dab-119">**Networking**</span></span> |<span data-ttu-id="b4dab-120">LAN VAGY WAN</span><span class="sxs-lookup"><span data-stu-id="b4dab-120">LAN/WAN</span></span>  |<span data-ttu-id="b4dab-121">SDN-alapú (szoftveresen meghatározott Hálózatkezelés)</span><span class="sxs-lookup"><span data-stu-id="b4dab-121">SDN (software-defined networking)</span></span>|
> | <span data-ttu-id="b4dab-122">**Biztonsági csoport**</span><span class="sxs-lookup"><span data-stu-id="b4dab-122">**Security group**</span></span> |<span data-ttu-id="b4dab-123">IP-/ port korlátozást eszközök</span><span class="sxs-lookup"><span data-stu-id="b4dab-123">IP/port restriction tools</span></span> |[<span data-ttu-id="b4dab-124">Hálózati biztonsági csoport (NSG)</span><span class="sxs-lookup"><span data-stu-id="b4dab-124">Network Security Group (NSG)</span></span>](https://azure.microsoft.com/blog/network-security-groups) |
> | <span data-ttu-id="b4dab-125">**Rugalmasság**</span><span class="sxs-lookup"><span data-stu-id="b4dab-125">**Resilience**</span></span> |<span data-ttu-id="b4dab-126">MTBF (középidő hibák között)</span><span class="sxs-lookup"><span data-stu-id="b4dab-126">MTBF (mean time between failures)</span></span> |<span data-ttu-id="b4dab-127">MTTR (középidő toorecovery)</span><span class="sxs-lookup"><span data-stu-id="b4dab-127">MTTR (mean time toorecovery)</span></span>|
> | <span data-ttu-id="b4dab-128">**Tervezett karbantartás**</span><span class="sxs-lookup"><span data-stu-id="b4dab-128">**Planned maintenance**</span></span> |<span data-ttu-id="b4dab-129">Javítás vagy frissítés</span><span class="sxs-lookup"><span data-stu-id="b4dab-129">Patching/upgrades</span></span>|<span data-ttu-id="b4dab-130">[Rendelkezésre állási készletek](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (javítás/frissítések Azure által kezelt)</span><span class="sxs-lookup"><span data-stu-id="b4dab-130">[Availability sets](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (patching/upgrades managed by Azure)</span></span> |
> | <span data-ttu-id="b4dab-131">**Erőforrás**</span><span class="sxs-lookup"><span data-stu-id="b4dab-131">**Resource**</span></span> |<span data-ttu-id="b4dab-132">Dedikált</span><span class="sxs-lookup"><span data-stu-id="b4dab-132">Dedicated</span></span>  |<span data-ttu-id="b4dab-133">A más ügyfelekkel megosztott</span><span class="sxs-lookup"><span data-stu-id="b4dab-133">Shared with other clients</span></span>|
> | <span data-ttu-id="b4dab-134">**Régiók**</span><span class="sxs-lookup"><span data-stu-id="b4dab-134">**Regions**</span></span> |<span data-ttu-id="b4dab-135">Adatközpontok</span><span class="sxs-lookup"><span data-stu-id="b4dab-135">Datacenters</span></span> |[<span data-ttu-id="b4dab-136">A régióban párok</span><span class="sxs-lookup"><span data-stu-id="b4dab-136">Region pairs</span></span>](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | <span data-ttu-id="b4dab-137">**Tárolás**</span><span class="sxs-lookup"><span data-stu-id="b4dab-137">**Storage**</span></span> |<span data-ttu-id="b4dab-138">SAN vagy fizikai lemezek</span><span class="sxs-lookup"><span data-stu-id="b4dab-138">SAN/physical disks</span></span> |[<span data-ttu-id="b4dab-139">Azure által kezelt tárolási</span><span class="sxs-lookup"><span data-stu-id="b4dab-139">Azure-managed storage</span></span>](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | <span data-ttu-id="b4dab-140">**Méretezés**</span><span class="sxs-lookup"><span data-stu-id="b4dab-140">**Scale**</span></span> |<span data-ttu-id="b4dab-141">Függőleges méretezés</span><span class="sxs-lookup"><span data-stu-id="b4dab-141">Vertical scale</span></span> |<span data-ttu-id="b4dab-142">Horizontális skálázhatóság</span><span class="sxs-lookup"><span data-stu-id="b4dab-142">Horizontal scale</span></span>|


### <a name="requirements"></a><span data-ttu-id="b4dab-143">Követelmények</span><span class="sxs-lookup"><span data-stu-id="b4dab-143">Requirements</span></span>

- <span data-ttu-id="b4dab-144">Hello adatbázis méretére és növekedésére sebesség határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b4dab-144">Determine hello database size and growth rate.</span></span>
- <span data-ttu-id="b4dab-145">Oracle AWR jelentések vagy más hálózati eszközök figyelése alapján megbecsülheti hello IOPS követelmények meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b4dab-145">Determine hello IOPS requirements, which you can estimate based on Oracle AWR reports or other network monitoring tools.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="b4dab-146">Konfigurációs beállítások</span><span class="sxs-lookup"><span data-stu-id="b4dab-146">Configuration options</span></span>

<span data-ttu-id="b4dab-147">Számos négy lehetséges területet, hogy észlelheti a tooimprove teljesítmény az Azure környezetben.</span><span class="sxs-lookup"><span data-stu-id="b4dab-147">There are four potential areas that you can tune tooimprove performance in an Azure environment:</span></span>

- <span data-ttu-id="b4dab-148">Virtuális gép mérete</span><span class="sxs-lookup"><span data-stu-id="b4dab-148">Virtual machine size</span></span>
- <span data-ttu-id="b4dab-149">Hálózati teljesítmény</span><span class="sxs-lookup"><span data-stu-id="b4dab-149">Network throughput</span></span>
- <span data-ttu-id="b4dab-150">Lemez legyen kijelölve, és konfigurációk</span><span class="sxs-lookup"><span data-stu-id="b4dab-150">Disk types and configurations</span></span>
- <span data-ttu-id="b4dab-151">Gyorsítótár-beállítások</span><span class="sxs-lookup"><span data-stu-id="b4dab-151">Disk cache settings</span></span>

### <a name="generate-an-awr-report"></a><span data-ttu-id="b4dab-152">Egy AWR jelentés készítése</span><span class="sxs-lookup"><span data-stu-id="b4dab-152">Generate an AWR report</span></span>

<span data-ttu-id="b4dab-153">Ha egy meglévő Oracle-adatbázishoz és toomigrate tooAzure tervez, több lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="b4dab-153">If you have an existing an Oracle database and are planning toomigrate tooAzure, you have several options.</span></span> <span data-ttu-id="b4dab-154">Hello Oracle AWR jelentés tooget hello metrikák (IOPS, MB/s, GiBs és így tovább) is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b4dab-154">You can run hello Oracle AWR report tooget hello metrics (IOPS, Mbps, GiBs, and so on).</span></span> <span data-ttu-id="b4dab-155">Válassza ki a virtuális gép összegyűjtött hello mérőszámok alapján hello.</span><span class="sxs-lookup"><span data-stu-id="b4dab-155">Then choose hello VM based on hello metrics that you collected.</span></span> <span data-ttu-id="b4dab-156">Vagy felveheti a kapcsolatot az infrastruktúra csoport tooget hasonló információkat.</span><span class="sxs-lookup"><span data-stu-id="b4dab-156">Or you can contact your infrastructure team tooget similar information.</span></span>

<span data-ttu-id="b4dab-157">Érdemes lehet a AWR jelentés futtatása során rendszeres és a csúcsidőre munkaterhelések esetén, összehasonlítás.</span><span class="sxs-lookup"><span data-stu-id="b4dab-157">You might consider running your AWR report during both regular and peak workloads, so you can compare.</span></span> <span data-ttu-id="b4dab-158">Ezek a jelentések alapján, a munkaterhelés átlagos hello vagy hello maximális munkaterhelést alapuló virtuális gépek hello méretét.</span><span class="sxs-lookup"><span data-stu-id="b4dab-158">Based on these reports, you can size hello VMs based on either hello average workload or hello maximum workload.</span></span>

<span data-ttu-id="b4dab-159">Az alábbiakban látható egy példa bemutatja, hogyan toogenerate egy AWR jelentést:</span><span class="sxs-lookup"><span data-stu-id="b4dab-159">Following is an example of how toogenerate an AWR report:</span></span>

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a><span data-ttu-id="b4dab-160">Alapvető metrikákat</span><span class="sxs-lookup"><span data-stu-id="b4dab-160">Key metrics</span></span>

<span data-ttu-id="b4dab-161">Az alábbiakban szerezhet be a hello AWR jelentés hello metrikák:</span><span class="sxs-lookup"><span data-stu-id="b4dab-161">Following are hello metrics that you can obtain from hello AWR report:</span></span>

- <span data-ttu-id="b4dab-162">Magok száma összesen</span><span class="sxs-lookup"><span data-stu-id="b4dab-162">Total number of cores</span></span>
- <span data-ttu-id="b4dab-163">Órajelű Processzor</span><span class="sxs-lookup"><span data-stu-id="b4dab-163">CPU clock speed</span></span>
- <span data-ttu-id="b4dab-164">Teljes memória GB-ban</span><span class="sxs-lookup"><span data-stu-id="b4dab-164">Total memory in GB</span></span>
- <span data-ttu-id="b4dab-165">CPU-felhasználás</span><span class="sxs-lookup"><span data-stu-id="b4dab-165">CPU utilization</span></span>
- <span data-ttu-id="b4dab-166">Maximális adatátviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="b4dab-166">Peak data transfer rate</span></span>
- <span data-ttu-id="b4dab-167">I/o-módosításokat (olvasás/írás) aránya</span><span class="sxs-lookup"><span data-stu-id="b4dab-167">Rate of I/O changes (read/write)</span></span>
- <span data-ttu-id="b4dab-168">Végezze el újra napló gyakorisága (MB/s)</span><span class="sxs-lookup"><span data-stu-id="b4dab-168">Redo log rate (MBPs)</span></span>
- <span data-ttu-id="b4dab-169">Hálózati teljesítmény</span><span class="sxs-lookup"><span data-stu-id="b4dab-169">Network throughput</span></span>
- <span data-ttu-id="b4dab-170">Hálózati késés gyakorisága (alsó vagy felső)</span><span class="sxs-lookup"><span data-stu-id="b4dab-170">Network latency rate (low/high)</span></span>
- <span data-ttu-id="b4dab-171">Adatbázis méretét GB-ban</span><span class="sxs-lookup"><span data-stu-id="b4dab-171">Database size in GB</span></span>
- <span data-ttu-id="b4dab-172">SQL keresztül fogadott bájtok * a nettó / tooclient</span><span class="sxs-lookup"><span data-stu-id="b4dab-172">Bytes received via SQL*Net from/tooclient</span></span>

### <a name="virtual-machine-size"></a><span data-ttu-id="b4dab-173">Virtuális gép mérete</span><span class="sxs-lookup"><span data-stu-id="b4dab-173">Virtual machine size</span></span>

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-hello-awr-report"></a><span data-ttu-id="b4dab-174">1. Processzor, memória és I/O használata a hello AWR jelentés alapján becsült Virtuálisgép-méret</span><span class="sxs-lookup"><span data-stu-id="b4dab-174">1. Estimate VM size based on CPU, memory, and I/O usage from hello AWR report</span></span>

<span data-ttu-id="b4dab-175">Egy dolog, előfordulhat, hogy megnézi hello felső öt időzített előtér jelző események hol vannak a hello rendszer szűk keresztmetszetek.</span><span class="sxs-lookup"><span data-stu-id="b4dab-175">One thing you might look at is hello top five timed foreground events that indicate where hello system bottlenecks are.</span></span>

<span data-ttu-id="b4dab-176">Például a következő diagram hello, hello napló fájlszinkronizálás van hello tetején.</span><span class="sxs-lookup"><span data-stu-id="b4dab-176">For example, in hello following diagram, hello log file sync is at hello top.</span></span> <span data-ttu-id="b4dab-177">Azt jelzi, hogy hello száma, amelyek szükségesek, mielőtt hello LGWR ír hello napló puffer toohello ismét: naplófájl vár.</span><span class="sxs-lookup"><span data-stu-id="b4dab-177">It indicates hello number of waits that are required before hello LGWR writes hello log buffer toohello redo log file.</span></span> <span data-ttu-id="b4dab-178">Ezekkel az eredményekkel jelzi, hogy jobban teljesítő tároló vagy lemezek szükségesek.</span><span class="sxs-lookup"><span data-stu-id="b4dab-178">These results indicate that better performing storage or disks are required.</span></span> <span data-ttu-id="b4dab-179">Emellett hello ábrán is látható hello memóriamennyiség és a Processzor (magok) hello száma.</span><span class="sxs-lookup"><span data-stu-id="b4dab-179">In addition, hello diagram also shows hello number of CPU (cores) and hello amount of memory.</span></span>

![Hello AWR jelentés oldalát bemutató képernyőkép](./media/oracle-design/cpu_memory_info.png)

<span data-ttu-id="b4dab-181">hello alábbi ábrán látható hello teljes i/o-olvasási és írási.</span><span class="sxs-lookup"><span data-stu-id="b4dab-181">hello following diagram shows hello total I/O of read and write.</span></span> <span data-ttu-id="b4dab-182">Nem voltak 59 olvassa el és 247.3 GB-os írt hello jelentés hello idő alatt.</span><span class="sxs-lookup"><span data-stu-id="b4dab-182">There were 59 GB read and 247.3 GB written during hello time of hello report.</span></span>

![Hello AWR jelentés oldalát bemutató képernyőkép](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a><span data-ttu-id="b4dab-184">2. Válassza ki a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b4dab-184">2. Choose a VM</span></span>

<span data-ttu-id="b4dab-185">Begyűjti a hello AWR jelentés hello adatok alapján, hello tovább toochoose igényeknek megfelelő hasonló méretű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b4dab-185">Based on hello information that you collected from hello AWR report, hello next step is toochoose a VM of a similar size that meets your requirements.</span></span> <span data-ttu-id="b4dab-186">Rendelkezésre álló virtuális gépek listájának hello cikkben található [Memóriaoptimalizált](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span><span class="sxs-lookup"><span data-stu-id="b4dab-186">You can find a list of available VMs in hello article [Memory optimized](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).</span></span>

#### <a name="3-fine-tune-hello-vm-sizing-with-a-similar-vm-series-based-on-hello-acu"></a><span data-ttu-id="b4dab-187">3. Hello Virtuálisgép-méretezési finomhangolásához hasonló virtuális gép több hello ACU alapján</span><span class="sxs-lookup"><span data-stu-id="b4dab-187">3. Fine-tune hello VM sizing with a similar VM series based on hello ACU</span></span>

<span data-ttu-id="b4dab-188">Hello VM kiválasztása után kell fizetnie figyelmet toohello ACU hello virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="b4dab-188">After you've chosen hello VM, pay attention toohello ACU for hello VM.</span></span> <span data-ttu-id="b4dab-189">Akkor célszerű használni a különböző virtuális gépek hello igényeinek jobban megfelelő ACU érték alapján.</span><span class="sxs-lookup"><span data-stu-id="b4dab-189">You might choose a different VM based on hello ACU value that better suits your requirements.</span></span> <span data-ttu-id="b4dab-190">További információkért lásd: [Azure számítási egység](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span><span class="sxs-lookup"><span data-stu-id="b4dab-190">For more information, see [Azure compute unit](https://docs.microsoft.com/azure/virtual-machines/windows/acu).</span></span>

![Hello ACU egységek oldalát bemutató képernyőkép](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a><span data-ttu-id="b4dab-192">Hálózati teljesítmény</span><span class="sxs-lookup"><span data-stu-id="b4dab-192">Network throughput</span></span>

<span data-ttu-id="b4dab-193">a következő ábra azt mutatja be hello kapcsolata az átviteli sebesség és IOPS hello:</span><span class="sxs-lookup"><span data-stu-id="b4dab-193">hello following diagram shows hello relation between throughput and IOPS:</span></span>

![Képernyőkép a átviteli sebesség](./media/oracle-design/throughput.png)

<span data-ttu-id="b4dab-195">hello teljes hálózati teljesítmény becsült hello a következő információk alapján:</span><span class="sxs-lookup"><span data-stu-id="b4dab-195">hello total network throughput is estimated based on hello following information:</span></span>
- <span data-ttu-id="b4dab-196">Az SQL * forgalom Net</span><span class="sxs-lookup"><span data-stu-id="b4dab-196">SQL*Net traffic</span></span>
- <span data-ttu-id="b4dab-197">MB/s x (például Oracle Data Guard kimenő adatfolyam) kiszolgálók száma</span><span class="sxs-lookup"><span data-stu-id="b4dab-197">MBps x number of servers (outbound stream such as Oracle Data Guard)</span></span>
- <span data-ttu-id="b4dab-198">Más tényezőktől, például az alkalmazás-replikáció</span><span class="sxs-lookup"><span data-stu-id="b4dab-198">Other factors, such as application replication</span></span>

![Képernyőfelvétel a hello SQL * nettó átviteli sebesség](./media/oracle-design/sqlnet_info.png)

<span data-ttu-id="b4dab-200">A hálózati sávszélesség követelmények alapján, az toochoose a különböző átjáró típusa van.</span><span class="sxs-lookup"><span data-stu-id="b4dab-200">Based on your network bandwidth requirements, there are various gateway types for you toochoose from.</span></span> <span data-ttu-id="b4dab-201">Ezek közé tartozik a basic, VpnGw, és az Azure ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b4dab-201">These include basic, VpnGw, and Azure ExpressRoute.</span></span> <span data-ttu-id="b4dab-202">További információkért lásd: hello [árképzést ismertető oldalra VPN-átjáró](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span><span class="sxs-lookup"><span data-stu-id="b4dab-202">For more information, see hello [VPN gateway pricing page](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).</span></span>

<span data-ttu-id="b4dab-203">**Javaslatok**</span><span class="sxs-lookup"><span data-stu-id="b4dab-203">**Recommendations**</span></span>

- <span data-ttu-id="b4dab-204">Hálózati késés nagyobb összehasonlított tooan helyszíni telepítés.</span><span class="sxs-lookup"><span data-stu-id="b4dab-204">Network latency is higher compared tooan on-premises deployment.</span></span> <span data-ttu-id="b4dab-205">Hálózati round való adatváltások számát is nagy mértékben csökkenti a teljesítmény javításához.</span><span class="sxs-lookup"><span data-stu-id="b4dab-205">Reducing network round trips can greatly improve performance.</span></span>
- <span data-ttu-id="b4dab-206">tooreduce üzenetváltások utak számát, az alkalmazásokat, amelyek magas tranzakciók, illetve "chatty" alkalmazásokat a hello egyesíteni ugyanahhoz a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="b4dab-206">tooreduce round-trips, consolidate applications that have high transactions or “chatty” apps on hello same virtual machine.</span></span>

### <a name="disk-types-and-configurations"></a><span data-ttu-id="b4dab-207">Lemez legyen kijelölve, és konfigurációk</span><span class="sxs-lookup"><span data-stu-id="b4dab-207">Disk types and configurations</span></span>

- <span data-ttu-id="b4dab-208">*Az operációs rendszer lemezek alapértelmezett*: ezek lemeztípusokkal ajánlat állandó adatok és a gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="b4dab-208">*Default OS disks*: These disk types offer persistent data and caching.</span></span> <span data-ttu-id="b4dab-209">Az operációs rendszer hozzáférés indításkor vannak optimalizálva, és nem terveztek vagy tranzakciós vagy az adatraktár (analitikai) munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="b4dab-209">They are optimized for OS access at startup, and aren't designed for either transactional or data warehouse (analytical) workloads.</span></span>

- <span data-ttu-id="b4dab-210">*Nem felügyelt lemezek*: a lemez legyen kijelölve, a tároló hello virtuális merevlemez (VHD) fájlok, amelyek megfelelnek a virtuális gépek lemezei tooyour hello storage-fiókok kezelése.</span><span class="sxs-lookup"><span data-stu-id="b4dab-210">*Unmanaged disks*: With these disk types, you manage hello storage accounts that store hello virtual hard disk (VHD) files that correspond tooyour VM disks.</span></span> <span data-ttu-id="b4dab-211">VHD-fájlokat, az Azure storage-fiókok lapblobokat tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="b4dab-211">VHD files are stored as page blobs in Azure storage accounts.</span></span>

- <span data-ttu-id="b4dab-212">*Által kezelt lemezeken*: Azure kezeli hello storage-fiókok, amelyek a virtuális gép lemezeit használja.</span><span class="sxs-lookup"><span data-stu-id="b4dab-212">*Managed disks*: Azure manages hello storage accounts that you use for your VM disks.</span></span> <span data-ttu-id="b4dab-213">Megadhatja, hogy hello lemeztípus (prémium és standard) és a szükséges hello lemez hello mérete.</span><span class="sxs-lookup"><span data-stu-id="b4dab-213">You specify hello disk type (premium or standard) and hello size of hello disk that you need.</span></span> <span data-ttu-id="b4dab-214">Azure hoz létre, és kezeli a hello lemez meg.</span><span class="sxs-lookup"><span data-stu-id="b4dab-214">Azure creates and manages hello disk for you.</span></span>

- <span data-ttu-id="b4dab-215">*Prémium szintű storage lemezek*: A lemez típusok a következők termelési számítási feladatokhoz ideális.</span><span class="sxs-lookup"><span data-stu-id="b4dab-215">*Premium storage disks*: These disk types are best suited for production workloads.</span></span> <span data-ttu-id="b4dab-216">Prémium szintű storage támogatja virtuális gépek lemezei használható csatolt toospecific mérete sorozatú virtuális gépeket, például a DS, DSv2, GS és F adatsorozat virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b4dab-216">Premium storage supports VM disks that can be attached toospecific size-series VMs, such as DS, DSv2, GS, and F series VMs.</span></span> <span data-ttu-id="b4dab-217">prémium szintű lemez hello a különböző méretű, és 32 GB-os too4, 096 GB közötti lemezek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="b4dab-217">hello premium disk comes with different sizes, and you can choose between disks ranging from 32 GB too4,096 GB.</span></span> <span data-ttu-id="b4dab-218">Minden lemez mérete a saját teljesítmény specifikációi.</span><span class="sxs-lookup"><span data-stu-id="b4dab-218">Each disk size has its own performance specifications.</span></span> <span data-ttu-id="b4dab-219">Az alkalmazás követelményeitől függően egy vagy több lemezt tooyour VM csatolhat.</span><span class="sxs-lookup"><span data-stu-id="b4dab-219">Depending on your application requirements, you can attach one or more disks tooyour VM.</span></span>

<span data-ttu-id="b4dab-220">Egy új felügyelt lemezes hello portálról létrehozásakor választhat hello **fiók típusa** hello típusú lemez toouse keresi.</span><span class="sxs-lookup"><span data-stu-id="b4dab-220">When you create a new managed disk from hello portal, you can choose hello **Account type** for hello type of disk you want toouse.</span></span> <span data-ttu-id="b4dab-221">Ne feledje, hogy nem minden rendelkezésre álló lemezek hello legördülő menüben jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b4dab-221">Keep in mind that not all available disks are shown in hello drop-down menu.</span></span> <span data-ttu-id="b4dab-222">Egy adott VM-méret kiválasztása után hello menü csak hello érhető el prémium szintű storage, hogy a Virtuálisgép-méretet alapuló termékváltozatok jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b4dab-222">After you choose a particular VM size, hello menu shows only hello available premium storage SKUs that are based on that VM size.</span></span>

![Hello felügyelt lemezes oldalát bemutató képernyőkép](./media/oracle-design/premium_disk01.png)

<span data-ttu-id="b4dab-224">További információkért lásd: [prémium szintű Storage nagy teljesítményű és a virtuális gépek felügyelt lemezek](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span><span class="sxs-lookup"><span data-stu-id="b4dab-224">For more information, see [High-performance Premium Storage and managed disks for VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage).</span></span>

<span data-ttu-id="b4dab-225">Miután a virtuális gép tárhelyét konfigurált, érdemes lehet tooload teszt hello lemezek adatbázis létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="b4dab-225">After you configure your storage on a VM, you might want tooload test hello disks before creating a database.</span></span> <span data-ttu-id="b4dab-226">A késés célkitűzések átviteli tudatában hello i/o-sebessége késés és a teljesítmény tekintetében segít meghatározni, ha hello virtuális gépek támogatják hello várt.</span><span class="sxs-lookup"><span data-stu-id="b4dab-226">Knowing hello I/O rate in terms of both latency and throughput can help you determine if hello VMs support hello expected throughput with latency targets.</span></span>

<span data-ttu-id="b4dab-227">Számos alkalmazás terhelés tesztelési, például Oracle Orion Sysbench vagy Fio eszközök.</span><span class="sxs-lookup"><span data-stu-id="b4dab-227">There are a number of tools for application load testing, such as Oracle Orion, Sysbench, and Fio.</span></span>

<span data-ttu-id="b4dab-228">Hello terheléstesztet futtassa újból az Oracle-adatbázis telepítése után.</span><span class="sxs-lookup"><span data-stu-id="b4dab-228">Run hello load test again after you've deployed an Oracle database.</span></span> <span data-ttu-id="b4dab-229">Indítsa el a szabályos és csúcsidőre munkaterhelések, és hello, a környezet eredeti hello eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b4dab-229">Start your regular and peak workloads, and hello results show you hello baseline of your environment.</span></span>

<span data-ttu-id="b4dab-230">Előfordulhat, hogy a fontosabb toosize hello tárolási hello tárméret helyett hello IOPS arány alapján.</span><span class="sxs-lookup"><span data-stu-id="b4dab-230">It might be more important toosize hello storage based on hello IOPS rate rather than hello storage size.</span></span> <span data-ttu-id="b4dab-231">Például hello 5000 IOPS-érték, de csak akkor 200 GB-os csak szükség esetén továbbra is kaphat hello P30 osztály prémium szintű lemez annak ellenére, hogy a több mint 200 GB tárhelyet származik.</span><span class="sxs-lookup"><span data-stu-id="b4dab-231">For example, if hello required IOPS is 5,000, but you only need 200 GB, you might still get hello P30 class premium disk even though it comes with more than 200 GB of storage.</span></span>

<span data-ttu-id="b4dab-232">hello IOPS arány hello AWR jelentés lehet lekérni.</span><span class="sxs-lookup"><span data-stu-id="b4dab-232">hello IOPS rate can be obtained from hello AWR report.</span></span> <span data-ttu-id="b4dab-233">Azt határozza meg hello visszaállítási napló, fizikai olvasási és írási műveletek sebessége.</span><span class="sxs-lookup"><span data-stu-id="b4dab-233">It's determined by hello redo log, physical reads, and writes rate.</span></span>

![Hello AWR jelentés oldalát bemutató képernyőkép](./media/oracle-design/awr_report.png)

<span data-ttu-id="b4dab-235">Például hello ismét: mérete 12,200,000 bájt / s, amely egyenlő too11.63 MB/s.</span><span class="sxs-lookup"><span data-stu-id="b4dab-235">For example, hello redo size is 12,200,000 bytes per second, which is equal too11.63 MBPs.</span></span>
<span data-ttu-id="b4dab-236">hello IOPS értéke 12,200,000 / 2,358 = 5,174.</span><span class="sxs-lookup"><span data-stu-id="b4dab-236">hello IOPS is 12,200,000 / 2,358 = 5,174.</span></span>

<span data-ttu-id="b4dab-237">Miután világossá hello i/o-követelményeinek, dönthet úgy, amelyek a legjobb olyan környezethez a legalkalmasabb toomeet meghajtók kombinációja ezt a követelményt.</span><span class="sxs-lookup"><span data-stu-id="b4dab-237">After you have a clear picture of hello I/O requirements, you can choose a combination of drives that are best suited toomeet those requirements.</span></span>

<span data-ttu-id="b4dab-238">**Javaslatok**</span><span class="sxs-lookup"><span data-stu-id="b4dab-238">**Recommendations**</span></span>

- <span data-ttu-id="b4dab-239">Az adatok táblaterületen elosztva hello i/o-munkaterhelés lemezek számát felügyelt tároló- és Oracle ASM használatával.</span><span class="sxs-lookup"><span data-stu-id="b4dab-239">For data tablespace, spread hello I/O workload across a number of disks by using managed storage or Oracle ASM.</span></span>
- <span data-ttu-id="b4dab-240">Hello i/o-blokkméret olvasásigényű és írási-igényes műveletek növekszik, adja hozzá a további adatlemezt.</span><span class="sxs-lookup"><span data-stu-id="b4dab-240">As hello I/O block size increases for read-intensive and write-intensive operations, add more data disks.</span></span>
- <span data-ttu-id="b4dab-241">Növeli a nagy szekvenciális folyamatok hello blokkméretével.</span><span class="sxs-lookup"><span data-stu-id="b4dab-241">Increase hello block size for large sequential processes.</span></span>
- <span data-ttu-id="b4dab-242">Adatok tömörítése tooreduce i/o használja (az adatok és indexek).</span><span class="sxs-lookup"><span data-stu-id="b4dab-242">Use data compression tooreduce I/O (for both data and indexes).</span></span>
- <span data-ttu-id="b4dab-243">Ismét: naplókat, a rendszer, és a temps külön, és külön adatlemezek a TS visszavonása.</span><span class="sxs-lookup"><span data-stu-id="b4dab-243">Separate redo logs, system, and temps, and undo TS on separate data disks.</span></span>
- <span data-ttu-id="b4dab-244">Ne helyezze el az alapértelmezett operációsrendszer-lemezek (/ dev/sda) alkalmazás fájljai.</span><span class="sxs-lookup"><span data-stu-id="b4dab-244">Don't put any application files on default OS disks (/dev/sda).</span></span> <span data-ttu-id="b4dab-245">Ezek a lemezek nem optimalizált gyors virtuális gép rendszerindítási ideje, és előfordulhat, hogy nem adja meg az alkalmazás a megfelelő teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="b4dab-245">These disks aren't optimized for fast VM boot times, and they might not provide good performance for your application.</span></span>

### <a name="disk-cache-settings"></a><span data-ttu-id="b4dab-246">Gyorsítótár-beállítások</span><span class="sxs-lookup"><span data-stu-id="b4dab-246">Disk cache settings</span></span>

<span data-ttu-id="b4dab-247">Az állomás gyorsítótárazását három lehetőség áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="b4dab-247">There are three options for host caching:</span></span>

- <span data-ttu-id="b4dab-248">*Csak olvasható*: összes kérelem későbbi olvasása gyorsítótárba kerüljenek-e.</span><span class="sxs-lookup"><span data-stu-id="b4dab-248">*Read-only*: All requests are cached for future reads.</span></span> <span data-ttu-id="b4dab-249">Összes írt megmaradnak közvetlenül tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b4dab-249">All writes are persisted directly tooAzure Blob storage.</span></span>

- <span data-ttu-id="b4dab-250">*Olvasási és írási*: Ez a "előreolvasási" algoritmus.</span><span class="sxs-lookup"><span data-stu-id="b4dab-250">*Read-write*: This is a “read-ahead” algorithm.</span></span> <span data-ttu-id="b4dab-251">hello olvas, és írási műveletek jövőbeli olvasása gyorsítótárba kerüljenek-e.</span><span class="sxs-lookup"><span data-stu-id="b4dab-251">hello reads and writes are cached for future reads.</span></span> <span data-ttu-id="b4dab-252">Nem-író bejegyzést ír toohello helyi gyorsítótár először maradnak.</span><span class="sxs-lookup"><span data-stu-id="b4dab-252">Non-write-through writes are persisted toohello local cache first.</span></span> <span data-ttu-id="b4dab-253">Az SQL Server írási műveletek megőrzött tooAzure tárolási, mert író használ.</span><span class="sxs-lookup"><span data-stu-id="b4dab-253">For SQL Server, writes are persisted tooAzure Storage because it uses write-through.</span></span> <span data-ttu-id="b4dab-254">Legkisebb mértékű késleltetést hello a lemez könnyű munkaterhelések is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4dab-254">It also provides hello lowest disk latency for light workloads.</span></span>

- <span data-ttu-id="b4dab-255">*Nincs* (letiltva): Ez a beállítás használatával mellőzhetik hello gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="b4dab-255">*None* (disabled): By using this option, you can bypass hello cache.</span></span> <span data-ttu-id="b4dab-256">Minden hello adat átvitt toodisk és tooAzure tárolási megőrzött.</span><span class="sxs-lookup"><span data-stu-id="b4dab-256">All hello data is transferred toodisk and persisted tooAzure Storage.</span></span> <span data-ttu-id="b4dab-257">A metódus által biztosított, akkor hello i/o-igényes munkaterhelések a legmagasabb i/o-sebességet.</span><span class="sxs-lookup"><span data-stu-id="b4dab-257">This method gives you hello highest I/O rate for I/O intensive workloads.</span></span> <span data-ttu-id="b4dab-258">Figyelembe kell tootake "tranzakciós költségek" is.</span><span class="sxs-lookup"><span data-stu-id="b4dab-258">You also need tootake “transaction cost” into consideration.</span></span>

<span data-ttu-id="b4dab-259">**Javaslatok**</span><span class="sxs-lookup"><span data-stu-id="b4dab-259">**Recommendations**</span></span>

<span data-ttu-id="b4dab-260">toomaximize hello átviteli, azt javasoljuk, hogy a kiindulási pont **nincs** állomás gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="b4dab-260">toomaximize hello throughput, we recommend that you  start with **None** for host caching.</span></span> <span data-ttu-id="b4dab-261">A prémium szintű Storage, vegye figyelembe, hogy le kell tiltania hello "korlátok", ha fájlrendszer hello hello csatlakoztatja **ReadOnly** vagy **nincs** beállítások.</span><span class="sxs-lookup"><span data-stu-id="b4dab-261">For Premium Storage, keep in mind that you must disable hello "barriers" when you mount hello file system with hello **ReadOnly** or **None** options.</span></span> <span data-ttu-id="b4dab-262">Hello /etc/fstab frissítésfájl hello UUID toohello lemezzel.</span><span class="sxs-lookup"><span data-stu-id="b4dab-262">Update hello /etc/fstab file with hello UUID toohello disks.</span></span>

<span data-ttu-id="b4dab-263">További információkért lásd: [prémium szintű Storage Linux virtuális gépek](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span><span class="sxs-lookup"><span data-stu-id="b4dab-263">For more information, see [Premium Storage for Linux VMs](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).</span></span>

![Hello felügyelt lemezes oldalát bemutató képernyőkép](./media/oracle-design/premium_disk02.png)

- <span data-ttu-id="b4dab-265">Az operációs rendszer lemezek, használja az alapértelmezett **olvasási/írási** gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="b4dab-265">For OS disks, use default **Read/Write** caching.</span></span>
- <span data-ttu-id="b4dab-266">A rendszer, a TEMP és a visszavonás használja **nincs** a gyorsítótárazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b4dab-266">For SYSTEM, TEMP, and UNDO use **None** for caching.</span></span>
- <span data-ttu-id="b4dab-267">Az adatokat, használja **nincs** a gyorsítótárazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b4dab-267">For DATA, use **None** for caching.</span></span> <span data-ttu-id="b4dab-268">De ha az adatbázis csak olvasható vagy olvasásigényű, **írásvédett** gyorsítótárazását.</span><span class="sxs-lookup"><span data-stu-id="b4dab-268">But if your database is read-only or read-intensive, use **Read-only** caching.</span></span>

<span data-ttu-id="b4dab-269">Az adatok lemezre beállításainak mentése után a hello állomás gyorsítótár-beállítást csak leválasztása az operációs rendszer szintű hello hello meghajtót, és majd a módosítása hello elvégzése után csatlakoztassa újra módosítható.</span><span class="sxs-lookup"><span data-stu-id="b4dab-269">After your data disk setting is saved, you can't change hello host cache setting unless you unmount hello drive at hello OS level and then remount it after you've made hello change.</span></span>


## <a name="security"></a><span data-ttu-id="b4dab-270">Biztonság</span><span class="sxs-lookup"><span data-stu-id="b4dab-270">Security</span></span>

<span data-ttu-id="b4dab-271">Után, hogy állítson be és konfigurálta az Azure környezetben, hello tovább toosecure-e a hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b4dab-271">After you have set up and configured your Azure environment, hello next step is toosecure your network.</span></span> <span data-ttu-id="b4dab-272">Az alábbiakban néhány javaslatot:</span><span class="sxs-lookup"><span data-stu-id="b4dab-272">Here are some recommendations:</span></span>

- <span data-ttu-id="b4dab-273">*NSG házirend*: NSG definiálni egy alhálózatot vagy egy hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="b4dab-273">*NSG policy*: NSG can be defined by a subnet or NIC.</span></span> <span data-ttu-id="b4dab-274">Hello alhálózat-szintű biztonsági és a kényszerített útválasztást többek között a tűzfalak egyaránt egyszerűbb toocontrol hozzáférést is.</span><span class="sxs-lookup"><span data-stu-id="b4dab-274">It's simpler toocontrol access at hello subnet level both for security and force routing for things like application firewalls.</span></span>

- <span data-ttu-id="b4dab-275">*Jumpbox*: A biztonságosabb hozzáférés érdekében a rendszergazdák nem közvetlenül csatlakoznia toohello alkalmazásszolgáltatás vagy az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b4dab-275">*Jumpbox*: For more secure access, administrators should not directly connect toohello application service or database.</span></span> <span data-ttu-id="b4dab-276">Egy media hello rendszergazda számítógép és az Azure-erőforrások egy jumpbox lesz.</span><span class="sxs-lookup"><span data-stu-id="b4dab-276">A jumpbox is used as a media between hello administrator machine and Azure resources.</span></span>
<span data-ttu-id="b4dab-277">![Hello Jumpbox topológia oldalát bemutató képernyőkép](./media/oracle-design/jumpbox.png)</span><span class="sxs-lookup"><span data-stu-id="b4dab-277">![Screenshot of hello Jumpbox topology page](./media/oracle-design/jumpbox.png)</span></span>

    <span data-ttu-id="b4dab-278">hello rendszergazda gép IP-korlátozott hozzáférés toohello jumpbox kínálja.</span><span class="sxs-lookup"><span data-stu-id="b4dab-278">hello administrator machine should offer IP-restricted access toohello jumpbox only.</span></span> <span data-ttu-id="b4dab-279">hello jumpbox access toohello alkalmazás és az adatbázis kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="b4dab-279">hello jumpbox should have access toohello application and database.</span></span>

- <span data-ttu-id="b4dab-280">*Magánhálózati* (alhálózatok): javasoljuk, hogy külön hello alkalmazásszolgáltatás és az adatbázis külön alhálózatokon, hogy jobb vezérlő NSG házirend állítható be.</span><span class="sxs-lookup"><span data-stu-id="b4dab-280">*Private network* (subnets): We recommend that you have hello application service and database on separate subnets, so better control can be set by NSG policy.</span></span>


## <a name="additional-reading"></a><span data-ttu-id="b4dab-281">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="b4dab-281">Additional reading</span></span>

- [<span data-ttu-id="b4dab-282">Oracle ASM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4dab-282">Configure Oracle ASM</span></span>](configure-oracle-asm.md)
- [<span data-ttu-id="b4dab-283">Oracle Data Guard konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4dab-283">Configure Oracle Data Guard</span></span>](configure-oracle-dataguard.md)
- [<span data-ttu-id="b4dab-284">Oracle Golden kapu beállítása</span><span class="sxs-lookup"><span data-stu-id="b4dab-284">Configure Oracle Golden Gate</span></span>](configure-oracle-golden-gate.md)
- [<span data-ttu-id="b4dab-285">Oracle biztonsági mentés és helyreállítás</span><span class="sxs-lookup"><span data-stu-id="b4dab-285">Oracle backup and recovery</span></span>](oracle-backup-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="b4dab-286">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4dab-286">Next steps</span></span>

- [<span data-ttu-id="b4dab-287">Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b4dab-287">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)
- [<span data-ttu-id="b4dab-288">Virtuális gép telepítése az Azure parancssori felület minták felfedezése</span><span class="sxs-lookup"><span data-stu-id="b4dab-288">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
