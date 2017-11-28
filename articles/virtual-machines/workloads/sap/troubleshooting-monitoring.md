---
title: "Hibaelhárítás és megfigyelése az Azure (nagy példányok) SAP HANA |} Microsoft Docs"
description: "Hibaelhárítás, és figyelje az SAP HANA egy Azure (nagy példány)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee5be707b443cbe42bf4a492d79390e534d4b91f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-troubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a><span data-ttu-id="d50d2-103">És figyelheti az SAP HANA (nagy példány) az Azure-on</span><span class="sxs-lookup"><span data-stu-id="d50d2-103">How to troubleshoot and monitor SAP HANA (large instances) on Azure</span></span>


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="d50d2-104">Az SAP HANA Azure (nagy példányok) figyelése</span><span class="sxs-lookup"><span data-stu-id="d50d2-104">Monitoring in SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="d50d2-105">Az Azure (nagy példányok) SAP HANA ugyanolyan helyzetet teremt, az egyéb IaaS-telepítésből – kell figyelnie, az operációs rendszer és az alkalmazás tevékenységétől, és hogyan ezek erőforrást a következő:</span><span class="sxs-lookup"><span data-stu-id="d50d2-105">SAP HANA on Azure (Large Instances) is no different from any other IaaS deployment — you need to monitor what the OS and the application is doing and how these consume the following resources:</span></span>

- <span data-ttu-id="d50d2-106">CPU</span><span class="sxs-lookup"><span data-stu-id="d50d2-106">CPU</span></span>
- <span data-ttu-id="d50d2-107">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="d50d2-107">Memory</span></span>
- <span data-ttu-id="d50d2-108">Hálózati sávszélesség</span><span class="sxs-lookup"><span data-stu-id="d50d2-108">Network bandwidth</span></span>
- <span data-ttu-id="d50d2-109">Lemezterület</span><span class="sxs-lookup"><span data-stu-id="d50d2-109">Disk space</span></span>

<span data-ttu-id="d50d2-110">Mi a teendő, hogy a fent megnevezett szoftverre erőforrás osztályok-e elegendő, és hogy ezek beolvasása elfogytak van Azure virtuális gépekkel.</span><span class="sxs-lookup"><span data-stu-id="d50d2-110">As with Azure Virtual Machines you need to figure out whether the resource classes named above are sufficient, or whether these get depleted.</span></span> <span data-ttu-id="d50d2-111">Íme további információkhoz juthat az egyes a különböző osztályú:</span><span class="sxs-lookup"><span data-stu-id="d50d2-111">Here is more detail on each of the different classes:</span></span>

<span data-ttu-id="d50d2-112">**Processzor-erőforrás-felhasználás:** SAP HANA elleni bizonyos munkaterhelések meghatározott aránya érvénybe van léptetve, győződjön meg arról, hogy az elegendő Processzor-erőforrások a memóriában tárolt adatok keresztül elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="d50d2-112">**CPU resource consumption:** The ratio that SAP defined for certain workload against HANA is enforced to make sure that there should be enough CPU resources available to work through the data that is stored in memory.</span></span> <span data-ttu-id="d50d2-113">Mindazonáltal néhány esetben előfordulhat HANA igényel, ahol nagy mennyiségű Processzor végrehajtott lekérdezések hiányzó indexek vagy hasonló hibák miatt.</span><span class="sxs-lookup"><span data-stu-id="d50d2-113">Nevertheless, there might be cases where HANA consumes a lot of CPU executing queries due to missing indexes or similar issues.</span></span> <span data-ttu-id="d50d2-114">Ez azt jelenti, célszerű figyelemmel kísérni a HANA nagy példány egység, valamint a Processzor-erőforrások, az adott HANA szolgáltatások által használt CPU hálózatierőforrás-fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="d50d2-114">This means you should monitor CPU resource consumption of the HANA large instance unit as well as CPU resources consumed by the specific HANA services.</span></span>

<span data-ttu-id="d50d2-115">**Memória-felhasználás:** fontos, hogy a egységen HANA belül, valamint HANA kívül a figyelheti.</span><span class="sxs-lookup"><span data-stu-id="d50d2-115">**Memory consumption:** Is important to monitor from within HANA, as well as outside of HANA on the unit.</span></span> <span data-ttu-id="d50d2-116">Belül HANA hogy az adatok nem használ-e HANA lefoglalt memória ahhoz, hogy az SAP szükséges méretezési útmutatást belül maradnak figyelése.</span><span class="sxs-lookup"><span data-stu-id="d50d2-116">Within HANA, monitor how the data is consuming HANA allocated memory in order to stay within the required sizing guidelines of SAP.</span></span> <span data-ttu-id="d50d2-117">Szeretné a nagy példány szintjén, győződjön meg arról, hogy további telepített nem-HANA szoftver nem túl sok memóriát használ, és ezért versenyeznek HANA-memória memória-felhasználás figyelése.</span><span class="sxs-lookup"><span data-stu-id="d50d2-117">You also want to monitor memory consumption on the Large Instance level to make sure that additional installed non-HANA software does not consume too much memory, and therefore compete with HANA for memory.</span></span>

<span data-ttu-id="d50d2-118">**Hálózati sávszélességet:** az Azure virtuális hálózatot átjáró korlátozott sávszélesség az adatok áthelyezése az Azure VNet, akkor célszerű egy Vnetet, hogy milyen közel vannak a kiválasztott Termékváltozat Azure átjáró keretein belül az összes Azure VMs által fogadott adatok figyelésére.</span><span class="sxs-lookup"><span data-stu-id="d50d2-118">**Network bandwidth:** The Azure VNet gateway is limited in bandwidth of data moving into the Azure VNet, so it is helpful to monitor the data received by all the Azure VMs within a VNet to figure out how close you are to the limits of the Azure gateway SKU you selected.</span></span> <span data-ttu-id="d50d2-119">A nagy példány HANA egységen akkor értelme bejövő és kimenő hálózati forgalom figyelését is, és a kötetek adott idő alatt végrehajtott nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="d50d2-119">On the HANA Large Instance unit, it does make sense to monitor incoming and outgoing network traffic as well, and to keep track of the volumes that are handled over time.</span></span>

<span data-ttu-id="d50d2-120">**Szabad lemezterület:** lemezterület-felhasználást általában növekszik adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="d50d2-120">**Disk space:** Disk space consumption usually increases over time.</span></span> <span data-ttu-id="d50d2-121">Ennek számos oka lehet, de a legtöbb összes: adatmennyiség növekszik, a tranzakciónapló biztonsági mentései, nyomkövetési fájlokat tárolja, és a storage-pillanatfelvételekkel végrehajtása végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="d50d2-121">There are many reasons for this, but most of all are: data volume increases, execution of transaction log backups, storing trace files, and performing storage snapshots.</span></span> <span data-ttu-id="d50d2-122">Ezért fontos, a felhasznált lemezterület felügyeletét és kezelését a HANA nagy példány egységhez társított lemezterület.</span><span class="sxs-lookup"><span data-stu-id="d50d2-122">Therefore, it is important to monitor disk space usage and manage the disk space associated with the HANA Large Instance unit.</span></span>

## <a name="monitoring-and-troubleshooting-from-hana-side"></a><span data-ttu-id="d50d2-123">Figyelés és hibaelhárítás céljából HANA oldaláról</span><span class="sxs-lookup"><span data-stu-id="d50d2-123">Monitoring and troubleshooting from HANA side</span></span>

<span data-ttu-id="d50d2-124">Ahhoz, hogy hatékonyan elemezheti Azure (nagy példányok) SAP HANA kapcsolatos problémák, célszerű a probléma okának szűkítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d50d2-124">In order to effectively analyze problems related to SAP HANA on Azure (Large Instances), it is useful to narrow down the root cause of a problem.</span></span> <span data-ttu-id="d50d2-125">SAP dokumentációja segít nagy mennyiségű tett közzé.</span><span class="sxs-lookup"><span data-stu-id="d50d2-125">SAP has published a large amount of documentation to help you.</span></span>

<span data-ttu-id="d50d2-126">SAP HANA-teljesítménnyel kapcsolatos vonatkozó gyakori kérdések a következő SAP megjegyzések találhatók:</span><span class="sxs-lookup"><span data-stu-id="d50d2-126">Applicable FAQs related to SAP HANA performance can be found in the following SAP Notes:</span></span>

- [<span data-ttu-id="d50d2-127">SAP Megjegyzés #2222200 – gyakran ismételt kérdések: SAP HANA-hálózat</span><span class="sxs-lookup"><span data-stu-id="d50d2-127">SAP Note #2222200 – FAQ: SAP HANA Network</span></span>](https://launchpad.support.sap.com/#/notes/2222200)
- [<span data-ttu-id="d50d2-128">SAP Megjegyzés #2100040 – gyakran ismételt kérdések: SAP HANA Processzor</span><span class="sxs-lookup"><span data-stu-id="d50d2-128">SAP Note #2100040 – FAQ: SAP HANA CPU</span></span>](https://launchpad.support.sap.com/#/notes/0002100040)
- [<span data-ttu-id="d50d2-129">SAP Megjegyzés #199997 – gyakran ismételt kérdések: SAP HANA memória</span><span class="sxs-lookup"><span data-stu-id="d50d2-129">SAP Note #199997 – FAQ: SAP HANA Memory</span></span>](https://launchpad.support.sap.com/#/notes/2177064)
- [<span data-ttu-id="d50d2-130">SAP Megjegyzés #200000 – gyakran ismételt kérdések: SAP HANA teljesítmény optimalizálása</span><span class="sxs-lookup"><span data-stu-id="d50d2-130">SAP Note #200000 – FAQ: SAP HANA Performance Optimization</span></span>](https://launchpad.support.sap.com/#/notes/2000000)
- [<span data-ttu-id="d50d2-131">SAP Megjegyzés #199930 – gyakran ismételt kérdések: SAP HANA i/o-elemzés</span><span class="sxs-lookup"><span data-stu-id="d50d2-131">SAP Note #199930 – FAQ: SAP HANA I/O Analysis</span></span>](https://launchpad.support.sap.com/#/notes/1999930)
- [<span data-ttu-id="d50d2-132">SAP Megjegyzés #2177064 – gyakran ismételt kérdések: SAP HANA-szolgáltatás újraindítása és összeomlik</span><span class="sxs-lookup"><span data-stu-id="d50d2-132">SAP Note #2177064 – FAQ: SAP HANA Service Restart and Crashes</span></span>](https://launchpad.support.sap.com/#/notes/2177064)

<span data-ttu-id="d50d2-133">**SAP HANA riasztások**</span><span class="sxs-lookup"><span data-stu-id="d50d2-133">**SAP HANA Alerts**</span></span>

<span data-ttu-id="d50d2-134">Első lépésként a naplófájlokban aktuális SAP HANA riasztások.</span><span class="sxs-lookup"><span data-stu-id="d50d2-134">As a first step, check the current SAP HANA alert logs.</span></span> <span data-ttu-id="d50d2-135">Az SAP HANA Studio eszközben navigáljon **felügyeleti konzol: riasztások: megjelenítése: az összes riasztás**.</span><span class="sxs-lookup"><span data-stu-id="d50d2-135">In SAP HANA Studio, go to **Administration Console: Alerts: Show: all alerts**.</span></span> <span data-ttu-id="d50d2-136">Ezen a lapon adott értékekre (szabad fizikai memória, a processzorkihasználtság stb.) a set minimális és maximális küszöbértékek kívül eső összes SAP HANA riasztások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d50d2-136">This tab will show all SAP HANA alerts for specific values (free physical memory, CPU utilization, etc.) that fall outside of the set minimum and maximum thresholds.</span></span> <span data-ttu-id="d50d2-137">Alapértelmezés szerint legyenek ellenőrzések automatikus frissítése 15 percenként.</span><span class="sxs-lookup"><span data-stu-id="d50d2-137">By default, checks are auto-refreshed every 15 minutes.</span></span>

![Az SAP HANA Studio, nyissa meg a felügyeleti konzol: riasztások: megjelenítése: az összes riasztás](./media/troubleshooting-monitoring/image1-show-alerts.png)

<span data-ttu-id="d50d2-139">**PROCESSZOR**</span><span class="sxs-lookup"><span data-stu-id="d50d2-139">**CPU**</span></span>

<span data-ttu-id="d50d2-140">Küszöbérték helytelen beállítás miatt indított riasztást megoldást, hogy visszaállítása az alapértelmezett érték vagy egyéb ésszerű küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="d50d2-140">For an alert triggered due to improper threshold setting, a resolution is to reset to the default value or a more reasonable threshold value.</span></span>

![Az alapértelmezett érték vagy egyéb ésszerű küszöbértéket](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

<span data-ttu-id="d50d2-142">Az alábbi riasztások utalhat a Processzor-erőforrás problémák:</span><span class="sxs-lookup"><span data-stu-id="d50d2-142">The following alerts may indicate CPU resource problems:</span></span>

- <span data-ttu-id="d50d2-143">Gazdagép CPU-használat (riasztás 5)</span><span class="sxs-lookup"><span data-stu-id="d50d2-143">Host CPU Usage (Alert 5)</span></span>
- <span data-ttu-id="d50d2-144">Legutóbbi mentésipont-művelet (riasztás 28)</span><span class="sxs-lookup"><span data-stu-id="d50d2-144">Most recent savepoint operation (Alert 28)</span></span>
- <span data-ttu-id="d50d2-145">Mentési pont időtartama (riasztás 54)</span><span class="sxs-lookup"><span data-stu-id="d50d2-145">Savepoint duration (Alert 54)</span></span>

<span data-ttu-id="d50d2-146">Azt tapasztalhatja, hogy magas CPU-felhasználás az SAP HANA-adatbázishoz a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="d50d2-146">You may notice high CPU consumption on your SAP HANA database from one of the following:</span></span>

- <span data-ttu-id="d50d2-147">Riasztási 5 (gazdagép CPU-használat) jelenik meg, az aktuális és korábbi CPU-használat</span><span class="sxs-lookup"><span data-stu-id="d50d2-147">Alert 5 (Host CPU usage) is raised for current or past CPU usage</span></span>
- <span data-ttu-id="d50d2-148">A megjelenített CPU-használat a az Áttekintés képernyő</span><span class="sxs-lookup"><span data-stu-id="d50d2-148">The displayed CPU usage on the overview screen</span></span>

![CPU-használat jelenik meg az Áttekintés képernyő](./media/troubleshooting-monitoring/image3-cpu-usage.png)

<span data-ttu-id="d50d2-150">A betöltési graph magas CPU-felhasználás, vagy a fogyasztás magas lehet, hogy megjelenítése a múltban:</span><span class="sxs-lookup"><span data-stu-id="d50d2-150">The Load graph might show high CPU consumption, or high consumption in the past:</span></span>

![A betöltési graph előfordulhat, hogy megjelenítése magas CPU-felhasználás, vagy magas fogyasztás az elmúlt](./media/troubleshooting-monitoring/image4-load-graph.png)

<span data-ttu-id="d50d2-152">Lehetséges okok miatt magas fokú Processzorhasználatot tapasztalható kiváltott riasztást több oka is, de nem kizárólagosan: bizonyos tranzakciókat, az adatok betöltése, a feladatok sokáig futnak az SQL-utasításokat vagy hibás lekérdezések teljesítményét (például a HANA kockák a BW) függő végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="d50d2-152">An alert triggered due to high CPU utilization could be caused by several reasons, including, but not limited to: execution of certain transactions, data loading, hanging of jobs, long running SQL statements, and bad query performance (for example, with BW on HANA cubes).</span></span>

<span data-ttu-id="d50d2-153">Tekintse meg a [SAP HANA-hibaelhárítás: CPU kapcsolódó okoz, és a megoldások](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d50d2-153">Refer to the [SAP HANA Troubleshooting: CPU Related Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="d50d2-154">**Operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="d50d2-154">**Operating System**</span></span>

<span data-ttu-id="d50d2-155">A legfontosabb ellenőrzések SAP Hana Linux egyik győződjön meg arról, hogy van-e tiltva átlátszó túl nagy lapok [SAP Megjegyzés #2131662 – átlátszó túl nagy lapok (THP) SAP HANA-kiszolgálókon](https://launchpad.support.sap.com/#/notes/2131662).</span><span class="sxs-lookup"><span data-stu-id="d50d2-155">One of the most important checks for SAP HANA on Linux is to make sure that Transparent Huge Pages are disabled, see [SAP Note #2131662 – Transparent Huge Pages (THP) on SAP HANA Servers](https://launchpad.support.sap.com/#/notes/2131662).</span></span>

- <span data-ttu-id="d50d2-156">Ha átlátszó túl nagy lapok engedélyezve vannak-e a következő Linux paranccsal ellenőrizheti: **/sys/kernel/mm/transparent macskaeledel\_hugepage/engedélyezve**</span><span class="sxs-lookup"><span data-stu-id="d50d2-156">You can check if Transparent Huge Pages are enabled through the following Linux command: **cat /sys/kernel/mm/transparent\_hugepage/enabled**</span></span>
- <span data-ttu-id="d50d2-157">Ha _mindig_ szimpla zárójelek között, az alábbi, az azt jelenti, hogy engedélyezve vannak-e a transzparens túl nagy lapok: [mindig] madvise soha nem; Ha _soha nem_ kapcsos zárójelek között, az alábbi, az azt jelenti, hogy van-e tiltva a transzparens túl nagy lapok: mindig madvise [soha nem]</span><span class="sxs-lookup"><span data-stu-id="d50d2-157">If _always_ is enclosed in brackets as below, it means that the Transparent Huge Pages are enabled: [always] madvise never; if _never_ is enclosed in brackets as below, it means that the Transparent Huge Pages are disabled: always madvise [never]</span></span>

<span data-ttu-id="d50d2-158">A következő Linux parancs semmi sem kell visszaadnia: **rpm - qa |} grep ulimit.**</span><span class="sxs-lookup"><span data-stu-id="d50d2-158">The following Linux command should return nothing: **rpm -qa | grep ulimit.**</span></span> <span data-ttu-id="d50d2-159">Ha úgy tűnik, _ulimit_ van telepítve, távolítsa el azonnal.</span><span class="sxs-lookup"><span data-stu-id="d50d2-159">If it appears _ulimit_ is installed, uninstall it immediately.</span></span>

<span data-ttu-id="d50d2-160">**Memória**</span><span class="sxs-lookup"><span data-stu-id="d50d2-160">**Memory**</span></span>

<span data-ttu-id="d50d2-161">Azt is láthatja, hogy a SAP HANA-adatbázisból által lefoglalt memória mérete nagyobb, mint a várt.</span><span class="sxs-lookup"><span data-stu-id="d50d2-161">You may observe that the amount of memory allocated by the SAP HANA database is higher than expected.</span></span> <span data-ttu-id="d50d2-162">Az alábbi riasztások a magas memóriahasználatban kapcsolatos hibákat jelzik:</span><span class="sxs-lookup"><span data-stu-id="d50d2-162">The following alerts indicate issues with high memory usage:</span></span>

- <span data-ttu-id="d50d2-163">Állomás fizikai memória használata (riasztás 1.)</span><span class="sxs-lookup"><span data-stu-id="d50d2-163">Host physical memory usage (Alert 1)</span></span>
- <span data-ttu-id="d50d2-164">A névkiszolgáló (riasztás 12) memóriahasználata</span><span class="sxs-lookup"><span data-stu-id="d50d2-164">Memory usage of name server (Alert 12)</span></span>
- <span data-ttu-id="d50d2-165">Teljes memóriahasználatát oszlop tárolási táblák (riasztás 40)</span><span class="sxs-lookup"><span data-stu-id="d50d2-165">Total memory usage of Column Store tables (Alert 40)</span></span>
- <span data-ttu-id="d50d2-166">Memóriahasználat (riasztás 43) szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d50d2-166">Memory usage of services (Alert 43)</span></span>
- <span data-ttu-id="d50d2-167">Memóriahasználat fő tárolási oszlop tárolási táblák (riasztás 45)</span><span class="sxs-lookup"><span data-stu-id="d50d2-167">Memory usage of main storage of Column Store tables (Alert 45)</span></span>
- <span data-ttu-id="d50d2-168">Futásidejű memóriaképek (riasztás 46)</span><span class="sxs-lookup"><span data-stu-id="d50d2-168">Runtime dump files (Alert 46)</span></span>

<span data-ttu-id="d50d2-169">Tekintse meg a [SAP HANA-hibaelhárítás: memóriával kapcsolatos problémák](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d50d2-169">Refer to the [SAP HANA Troubleshooting: Memory Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="d50d2-170">**Hálózat**</span><span class="sxs-lookup"><span data-stu-id="d50d2-170">**Network**</span></span>

<span data-ttu-id="d50d2-171">Tekintse meg [SAP Megjegyzés #2081065 – hibaelhárítás SAP HANA hálózati](https://launchpad.support.sap.com/#/notes/2081065) , és végezze el a hibaelhárítási SAP Megjegyzés vehető fel a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="d50d2-171">Refer to [SAP Note #2081065 – Troubleshooting SAP HANA Network](https://launchpad.support.sap.com/#/notes/2081065) and perform the network troubleshooting steps in this SAP Note.</span></span>

1. <span data-ttu-id="d50d2-172">Kiszolgáló és az ügyfél közötti üzenetváltások idő elemzése.</span><span class="sxs-lookup"><span data-stu-id="d50d2-172">Analyzing round-trip time between server and client.</span></span>
  <span data-ttu-id="d50d2-173">A.</span><span class="sxs-lookup"><span data-stu-id="d50d2-173">A.</span></span> <span data-ttu-id="d50d2-174">Az SQL-parancsfájl futtatása [ _HANA\_hálózati\_ügyfelek_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="d50d2-174">Run the SQL script [_HANA\_Network\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>
  
2. <span data-ttu-id="d50d2-175">Elemezze a fürtök csomóponton belüli kommunikációjához kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="d50d2-175">Analyze internode communication.</span></span>
  <span data-ttu-id="d50d2-176">A.</span><span class="sxs-lookup"><span data-stu-id="d50d2-176">A.</span></span> <span data-ttu-id="d50d2-177">SQL-parancsfájl futtatása [ _HANA\_hálózati\_szolgáltatások_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="d50d2-177">Run SQL script [_HANA\_Network\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>

3. <span data-ttu-id="d50d2-178">Linux parancs **ifconfig** (a kimeneti látható, ha a csomag veszteségeiért is megjelenhetnek).</span><span class="sxs-lookup"><span data-stu-id="d50d2-178">Run Linux command **ifconfig** (the output shows if any packet losses are occurring).</span></span>
4. <span data-ttu-id="d50d2-179">Linux parancs **tcpdump parancsot**.</span><span class="sxs-lookup"><span data-stu-id="d50d2-179">Run Linux command **tcpdump**.</span></span>

<span data-ttu-id="d50d2-180">Használja továbbá a nyílt forráskódú [IPERF](https://iperf.fr/) eszköz (vagy hasonlót) alkalmazás valós hálózati teljesítmény mérését.</span><span class="sxs-lookup"><span data-stu-id="d50d2-180">Also, use the open source [IPERF](https://iperf.fr/) tool (or similar) to measure real application network performance.</span></span>

<span data-ttu-id="d50d2-181">Tekintse meg a [SAP HANA-hibaelhárítás: hálózati teljesítmény és a kapcsolódási problémái vannak](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d50d2-181">Refer to the [SAP HANA Troubleshooting: Networking Performance and Connectivity Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="d50d2-182">**Storage**</span><span class="sxs-lookup"><span data-stu-id="d50d2-182">**Storage**</span></span>

<span data-ttu-id="d50d2-183">Végfelhasználói szempontból egy alkalmazás (vagy a rendszer egészének) nehézkesen futtatja, nem válaszol, vagy még is úgy tűnik, hogy lefagy, ha problémák i/o-teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d50d2-183">From an end-user perspective, an application (or the system as a whole) runs sluggishly, is unresponsive, or can even seem to hang if there are issues with I/O performance.</span></span> <span data-ttu-id="d50d2-184">Az a **kötetek** lapon SAP HANA Studio, a megtekintheti, milyen kötetek minden szolgáltatás által használt, és a kapcsolódó kötetek.</span><span class="sxs-lookup"><span data-stu-id="d50d2-184">In the **Volumes** tab in SAP HANA Studio, you can see the attached volumes, and what volumes are used by each service.</span></span>

![A SAP HANA Studio kötetek lapján megtekintheti, milyen kötetek minden szolgáltatás által használt, és a kapcsolódó kötetek](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

<span data-ttu-id="d50d2-186">Csatlakoztatott kötetek a képernyő alsó részén látható a kötetek, fájlok és az i/o-statisztikák például részleteit.</span><span class="sxs-lookup"><span data-stu-id="d50d2-186">Attached volumes in the lower part of the screen you can see details of the volumes, such as files and I/O statistics.</span></span>

![Csatlakoztatott kötetek a képernyő alsó részén látható a kötetek, fájlok és az i/o-statisztikák például részleteit](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

<span data-ttu-id="d50d2-188">Tekintse meg a [SAP HANA-hibaelhárítás: i/o kapcsolódó okait és megoldásait](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) és [SAP HANA-hibaelhárítás: lemez kapcsolódó okait és megoldásait](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d50d2-188">Refer to the [SAP HANA Troubleshooting: I/O Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) and [SAP HANA Troubleshooting: Disk Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="d50d2-189">**Diagnosztikai eszközök**</span><span class="sxs-lookup"><span data-stu-id="d50d2-189">**Diagnostic Tools**</span></span>

<span data-ttu-id="d50d2-190">Hajtsa végre egy SAP HANA állapot-ellenőrzéssel keresztül HANA\_konfigurációs\_Minichecks.</span><span class="sxs-lookup"><span data-stu-id="d50d2-190">Perform an SAP HANA Health Check through HANA\_Configuration\_Minichecks.</span></span> <span data-ttu-id="d50d2-191">Ez az eszköz, amely kell már merült fel az SAP HANA Studio riasztásként elvégzésével kritikus technikai problémák adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d50d2-191">This tool returns potentially critical technical issues that should have already been raised as alerts in SAP HANA Studio.</span></span>

<span data-ttu-id="d50d2-192">Tekintse meg [SAP Megjegyzés #1969700 – SQL utasítás gyűjtemény SAP Hana](https://launchpad.support.sap.com/#/notes/1969700) és töltse le az SQL Statements.zip fájlt, hogy a megjegyzés csatolva.</span><span class="sxs-lookup"><span data-stu-id="d50d2-192">Refer to [SAP Note #1969700 – SQL statement collection for SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) and download the SQL Statements.zip file attached to that note.</span></span> <span data-ttu-id="d50d2-193">A .zip fájlt a helyi merevlemez-meghajtón tárolja.</span><span class="sxs-lookup"><span data-stu-id="d50d2-193">Store this .zip file on the local hard drive.</span></span>

<span data-ttu-id="d50d2-194">Az SAP HANA Studio a a **Rendszerinformáció** lapra, kattintson a jobb gombbal a a **neve** oszlop, és válassza ki **importálási SQL-utasítások**.</span><span class="sxs-lookup"><span data-stu-id="d50d2-194">In SAP HANA Studio, on the **System Information** tab, right-click in the **Name** column and select **Import SQL Statements**.</span></span>

![Az SAP HANA Studio, a rendszer adatok lapon kattintson a jobb gombbal a neve oszlopban, és válassza ki a importálási SQL-utasítások](./media/troubleshooting-monitoring/image7-import-statements-a.png)

<span data-ttu-id="d50d2-196">Válassza ki a helyileg tárolt SQL Statements.zip fájlt, és importálja a megfelelő SQL-utasítások egy mappa.</span><span class="sxs-lookup"><span data-stu-id="d50d2-196">Select the SQL Statements.zip file stored locally, and a folder with the corresponding SQL statements will be imported.</span></span> <span data-ttu-id="d50d2-197">Ezen a ponton a sok különböző diagnosztikai ellenőrzések futtatható ezen az SQL-utasítások.</span><span class="sxs-lookup"><span data-stu-id="d50d2-197">At this point, the many different diagnostic checks can be run with these SQL statements.</span></span>

<span data-ttu-id="d50d2-198">Például SAP HANA rendszer replikáció sávszélesség-követelményekkel teszteléséhez kattintson a jobb gombbal a **sávszélesség** utasítás alapján **replikációs: sávszélesség** válassza ki **nyitott** SQL-konzolon.</span><span class="sxs-lookup"><span data-stu-id="d50d2-198">For example, to test SAP HANA System Replication bandwidth requirements, right-click the **Bandwidth** statement under **Replication: Bandwidth** and select **Open** in SQL Console.</span></span>

<span data-ttu-id="d50d2-199">A teljes SQL-utasítás megnyílik, lehetővé téve a bemeneti paramétereket (módosítása szakaszát) megváltozott, és akkor hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="d50d2-199">The complete SQL statement opens allowing input parameters (modification section) to be changed and then executed.</span></span>

![A teljes SQL-utasítás megnyitása (módosítása szakaszát) megváltozott, és akkor hajtja végre a bemeneti paraméterek engedélyezése](./media/troubleshooting-monitoring/image8-import-statements-b.png)

<span data-ttu-id="d50d2-201">Az utasítások alapján a jobb gombbal kattint egy másik példa: **replikációs: áttekintés**.</span><span class="sxs-lookup"><span data-stu-id="d50d2-201">Another example is right-clicking on the statements under **Replication: Overview**.</span></span> <span data-ttu-id="d50d2-202">Válassza ki **Execute** a helyi menüben:</span><span class="sxs-lookup"><span data-stu-id="d50d2-202">Select **Execute** from the context menu:</span></span>

![Jobb gombbal kattint, a replikációs csoportban utasítások egy másik példa: áttekintése.](./media/troubleshooting-monitoring/image9-import-statements-c.png)

<span data-ttu-id="d50d2-205">Ennek eredményeként a hibaelhárítási információkat:</span><span class="sxs-lookup"><span data-stu-id="d50d2-205">This results in information that helps with troubleshooting:</span></span>

![Ennek eredményeképpen az adatokat, amelyek segítenek a hibaelhárításban](./media/troubleshooting-monitoring/image10-import-statements-d.png)

<span data-ttu-id="d50d2-207">Tegye meg ugyanezt a HANA\_konfigurációs\_Minichecks és az összes ellenőrzés _X_ megjelöli a a _C_ (kritikus) oszlopban.</span><span class="sxs-lookup"><span data-stu-id="d50d2-207">Do the same for HANA\_Configuration\_Minichecks and check for any _X_ marks in the _C_ (Critical) column.</span></span>

<span data-ttu-id="d50d2-208">A minta kimenete:</span><span class="sxs-lookup"><span data-stu-id="d50d2-208">Sample outputs:</span></span>

<span data-ttu-id="d50d2-209">**HANA\_konfigurációs\_MiniChecks\_Rev102.01 + 1** az általános SAP HANA ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="d50d2-209">**HANA\_Configuration\_MiniChecks\_Rev102.01+1** for general SAP HANA checks.</span></span>

![HANA\_konfigurációs\_MiniChecks\_Rev102.01 + 1. a általános SAP HANA-ellenőrzése](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

<span data-ttu-id="d50d2-211">**HANA\_szolgáltatások\_áttekintése** mi SAP HANA futó szolgáltatások áttekintését.</span><span class="sxs-lookup"><span data-stu-id="d50d2-211">**HANA\_Services\_Overview** for an overview of what SAP HANA services are currently running.</span></span>

![HANA\_szolgáltatások\_megtudhatja, mi SAP HANA futó szolgáltatások – áttekintés](./media/troubleshooting-monitoring/image12-services-overview.png)

<span data-ttu-id="d50d2-213">**HANA\_szolgáltatások\_statisztika** SAP Hana szolgáltatás információkat (Processzor, memória, stb.).</span><span class="sxs-lookup"><span data-stu-id="d50d2-213">**HANA\_Services\_Statistics** for SAP HANA service information (CPU, memory, etc.).</span></span>

![<span data-ttu-id="d50d2-214">HANA\_szolgáltatások\_SAP Hana statisztika szolgáltatás információkat</span><span class="sxs-lookup"><span data-stu-id="d50d2-214">HANA\_Services\_Statistics for SAP HANA service information</span></span> ](./media/troubleshooting-monitoring/image13-services-statistics.png)

<span data-ttu-id="d50d2-215">**HANA\_konfigurációs\_áttekintése\_Rev110 +** általános tudnivalók az SAP HANA-példányon.</span><span class="sxs-lookup"><span data-stu-id="d50d2-215">**HANA\_Configuration\_Overview\_Rev110+** for general information on the SAP HANA instance.</span></span>

![HANA\_konfigurációs\_áttekintése\_Rev110 + általános tudnivalók az SAP HANA-példányon](./media/troubleshooting-monitoring/image14-configuration-overview.png)

<span data-ttu-id="d50d2-217">**HANA\_konfigurációs\_paraméterek\_Rev70 +** SAP HANA-paraméterekhez kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="d50d2-217">**HANA\_Configuration\_Parameters\_Rev70+** to check SAP HANA parameters.</span></span>

![HANA\_konfigurációs\_paraméterek\_Rev70 + SAP HANA-paraméterekhez ellenőrzése](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

