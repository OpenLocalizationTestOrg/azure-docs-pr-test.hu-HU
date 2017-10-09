---
title: "aaaTroubleshooting és a figyelés az SAP HANA (nagy példányok) Azure-on |} Microsoft Docs"
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
ms.openlocfilehash: 1f1cd35820e227fd99af495431cd4b826aa53600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a><span data-ttu-id="4d35a-103">Hogyan tootroubleshoot és a figyelő SAP HANA (nagy példány) az Azure-on</span><span class="sxs-lookup"><span data-stu-id="4d35a-103">How tootroubleshoot and monitor SAP HANA (large instances) on Azure</span></span>


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a><span data-ttu-id="4d35a-104">Az SAP HANA Azure (nagy példányok) figyelése</span><span class="sxs-lookup"><span data-stu-id="4d35a-104">Monitoring in SAP HANA on Azure (Large Instances)</span></span>

<span data-ttu-id="4d35a-105">Az Azure (nagy példányok) SAP HANA ugyanolyan helyzetet teremt, az egyéb IaaS-telepítésből – kell toomonitor milyen operációs rendszer hello és hello alkalmazás ezzel, és hogyan ezeket felhasználhatják a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="4d35a-105">SAP HANA on Azure (Large Instances) is no different from any other IaaS deployment — you need toomonitor what hello OS and hello application is doing and how these consume hello following resources:</span></span>

- <span data-ttu-id="4d35a-106">CPU</span><span class="sxs-lookup"><span data-stu-id="4d35a-106">CPU</span></span>
- <span data-ttu-id="4d35a-107">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="4d35a-107">Memory</span></span>
- <span data-ttu-id="4d35a-108">Hálózati sávszélesség</span><span class="sxs-lookup"><span data-stu-id="4d35a-108">Network bandwidth</span></span>
- <span data-ttu-id="4d35a-109">Lemezterület</span><span class="sxs-lookup"><span data-stu-id="4d35a-109">Disk space</span></span>

<span data-ttu-id="4d35a-110">Azure virtuális gépekkel van lehetősége, hogy hello erőforrás osztályok fent megnevezett szoftverre-e elegendő, és hogy ezek beolvasása elfogytak toofigure.</span><span class="sxs-lookup"><span data-stu-id="4d35a-110">As with Azure Virtual Machines you need toofigure out whether hello resource classes named above are sufficient, or whether these get depleted.</span></span> <span data-ttu-id="4d35a-111">Íme további információkhoz juthat az egyes hello különböző osztályú:</span><span class="sxs-lookup"><span data-stu-id="4d35a-111">Here is more detail on each of hello different classes:</span></span>

<span data-ttu-id="4d35a-112">**Processzor-erőforrás-felhasználás:** hello arány SAP HANA elleni bizonyos munkaterhelések meghatározott érvényben levő toomake meg arról, hogy van elég CPU erőforrások elérhető toowork hello a memóriában tárolt adatok keresztül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4d35a-112">**CPU resource consumption:** hello ratio that SAP defined for certain workload against HANA is enforced toomake sure that there should be enough CPU resources available toowork through hello data that is stored in memory.</span></span> <span data-ttu-id="4d35a-113">Mindazonáltal néhány esetben előfordulhat HANA igényel, ahol nagy mennyiségű Processzor toomissing indexek vagy hasonló problémák miatt a lekérdezések végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="4d35a-113">Nevertheless, there might be cases where HANA consumes a lot of CPU executing queries due toomissing indexes or similar issues.</span></span> <span data-ttu-id="4d35a-114">Ez azt jelenti, célszerű figyelemmel kísérni hello HANA nagy példány egység, valamint a Processzor-erőforrások hello adott HANA szolgáltatások által használt CPU hálózatierőforrás-fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="4d35a-114">This means you should monitor CPU resource consumption of hello HANA large instance unit as well as CPU resources consumed by hello specific HANA services.</span></span>

<span data-ttu-id="4d35a-115">**Memória-felhasználás:** fontos toomonitor a HANA belül, valamint HANA kívül hello egységen van.</span><span class="sxs-lookup"><span data-stu-id="4d35a-115">**Memory consumption:** Is important toomonitor from within HANA, as well as outside of HANA on hello unit.</span></span> <span data-ttu-id="4d35a-116">Belül HANA hogyan hello adatok nem használ-e rendelés toostay belül szükséges SAP útmutatást méretezése hello memóriája kiosztott HANA figyelése.</span><span class="sxs-lookup"><span data-stu-id="4d35a-116">Within HANA, monitor how hello data is consuming HANA allocated memory in order toostay within hello required sizing guidelines of SAP.</span></span> <span data-ttu-id="4d35a-117">Szeretné a hello nagy példány szintű toomake, hogy további telepített nem-HANA szoftver nem túl sok memóriát használ, és ezért versenyeznek HANA-memória memória-felhasználás toomonitor.</span><span class="sxs-lookup"><span data-stu-id="4d35a-117">You also want toomonitor memory consumption on hello Large Instance level toomake sure that additional installed non-HANA software does not consume too much memory, and therefore compete with HANA for memory.</span></span>

<span data-ttu-id="4d35a-118">**Hálózati sávszélességet:** hello Azure VNet átjáró korlátozott sávszélesség az adatok áthelyezése hello Azure virtuális hálózat, ezért hasznos lehet az összes fogadott toomonitor hello adatok hello Azure virtuális gépek a virtuális hálózat toofigure ki, hogy milyen közel a hello Azure toohello határain belül a kiválasztott SKU átjáró.</span><span class="sxs-lookup"><span data-stu-id="4d35a-118">**Network bandwidth:** hello Azure VNet gateway is limited in bandwidth of data moving into hello Azure VNet, so it is helpful toomonitor hello data received by all hello Azure VMs within a VNet toofigure out how close you are toohello limits of hello Azure gateway SKU you selected.</span></span> <span data-ttu-id="4d35a-119">Hello HANA nagy példány egység akkor győződjön meg logika toomonitor bejövő és kimenő hálózati forgalmat is, és adott idő alatt végrehajtott hello kötetek tookeep nyomon.</span><span class="sxs-lookup"><span data-stu-id="4d35a-119">On hello HANA Large Instance unit, it does make sense toomonitor incoming and outgoing network traffic as well, and tookeep track of hello volumes that are handled over time.</span></span>

<span data-ttu-id="4d35a-120">**Szabad lemezterület:** lemezterület-felhasználást általában növekszik adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="4d35a-120">**Disk space:** Disk space consumption usually increases over time.</span></span> <span data-ttu-id="4d35a-121">Ennek számos oka lehet, de a legtöbb összes: adatmennyiség növekszik, a tranzakciónapló biztonsági mentései, nyomkövetési fájlokat tárolja, és a storage-pillanatfelvételekkel végrehajtása végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="4d35a-121">There are many reasons for this, but most of all are: data volume increases, execution of transaction log backups, storing trace files, and performing storage snapshots.</span></span> <span data-ttu-id="4d35a-122">Ezért fontos toomonitor lemezterület-használat és hello HANA nagy példány egységhez társított hello lemezterület felügyeletét.</span><span class="sxs-lookup"><span data-stu-id="4d35a-122">Therefore, it is important toomonitor disk space usage and manage hello disk space associated with hello HANA Large Instance unit.</span></span>

## <a name="monitoring-and-troubleshooting-from-hana-side"></a><span data-ttu-id="4d35a-123">Figyelés és hibaelhárítás céljából HANA oldaláról</span><span class="sxs-lookup"><span data-stu-id="4d35a-123">Monitoring and troubleshooting from HANA side</span></span>

<span data-ttu-id="4d35a-124">A sorrend tooeffectively elemzése a problémák kapcsolódó tooSAP HANA Azure (nagy példányokat), a hasznos toonarrow le hello okozza a problémát.</span><span class="sxs-lookup"><span data-stu-id="4d35a-124">In order tooeffectively analyze problems related tooSAP HANA on Azure (Large Instances), it is useful toonarrow down hello root cause of a problem.</span></span> <span data-ttu-id="4d35a-125">SAP közzétette dokumentáció toohelp nagy mennyiségű meg.</span><span class="sxs-lookup"><span data-stu-id="4d35a-125">SAP has published a large amount of documentation toohelp you.</span></span>

<span data-ttu-id="4d35a-126">Vonatkozó gyakran ismételt kérdéseket kapcsolódó tooSAP HANA teljesítmény található SAP megjegyzések következő hello:</span><span class="sxs-lookup"><span data-stu-id="4d35a-126">Applicable FAQs related tooSAP HANA performance can be found in hello following SAP Notes:</span></span>

- [<span data-ttu-id="4d35a-127">SAP Megjegyzés #2222200 – gyakran ismételt kérdések: SAP HANA-hálózat</span><span class="sxs-lookup"><span data-stu-id="4d35a-127">SAP Note #2222200 – FAQ: SAP HANA Network</span></span>](https://launchpad.support.sap.com/#/notes/2222200)
- [<span data-ttu-id="4d35a-128">SAP Megjegyzés #2100040 – gyakran ismételt kérdések: SAP HANA Processzor</span><span class="sxs-lookup"><span data-stu-id="4d35a-128">SAP Note #2100040 – FAQ: SAP HANA CPU</span></span>](https://launchpad.support.sap.com/#/notes/0002100040)
- [<span data-ttu-id="4d35a-129">SAP Megjegyzés #199997 – gyakran ismételt kérdések: SAP HANA memória</span><span class="sxs-lookup"><span data-stu-id="4d35a-129">SAP Note #199997 – FAQ: SAP HANA Memory</span></span>](https://launchpad.support.sap.com/#/notes/2177064)
- [<span data-ttu-id="4d35a-130">SAP Megjegyzés #200000 – gyakran ismételt kérdések: SAP HANA teljesítmény optimalizálása</span><span class="sxs-lookup"><span data-stu-id="4d35a-130">SAP Note #200000 – FAQ: SAP HANA Performance Optimization</span></span>](https://launchpad.support.sap.com/#/notes/2000000)
- [<span data-ttu-id="4d35a-131">SAP Megjegyzés #199930 – gyakran ismételt kérdések: SAP HANA i/o-elemzés</span><span class="sxs-lookup"><span data-stu-id="4d35a-131">SAP Note #199930 – FAQ: SAP HANA I/O Analysis</span></span>](https://launchpad.support.sap.com/#/notes/1999930)
- [<span data-ttu-id="4d35a-132">SAP Megjegyzés #2177064 – gyakran ismételt kérdések: SAP HANA-szolgáltatás újraindítása és összeomlik</span><span class="sxs-lookup"><span data-stu-id="4d35a-132">SAP Note #2177064 – FAQ: SAP HANA Service Restart and Crashes</span></span>](https://launchpad.support.sap.com/#/notes/2177064)

<span data-ttu-id="4d35a-133">**SAP HANA riasztások**</span><span class="sxs-lookup"><span data-stu-id="4d35a-133">**SAP HANA Alerts**</span></span>

<span data-ttu-id="4d35a-134">Első lépésként ellenőrizze a hello aktuális SAP HANA riasztások naplókat.</span><span class="sxs-lookup"><span data-stu-id="4d35a-134">As a first step, check hello current SAP HANA alert logs.</span></span> <span data-ttu-id="4d35a-135">Az SAP HANA Studio eszközben lépjen túl**felügyeleti konzol: riasztások: megjelenítése: az összes riasztás**.</span><span class="sxs-lookup"><span data-stu-id="4d35a-135">In SAP HANA Studio, go too**Administration Console: Alerts: Show: all alerts**.</span></span> <span data-ttu-id="4d35a-136">Ezen a lapon adott értékekre (szabad fizikai memória, a processzorkihasználtság stb.) hello beállítása a minimális és maximális küszöbértékek kívül eső összes SAP HANA riasztások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4d35a-136">This tab will show all SAP HANA alerts for specific values (free physical memory, CPU utilization, etc.) that fall outside of hello set minimum and maximum thresholds.</span></span> <span data-ttu-id="4d35a-137">Alapértelmezés szerint legyenek ellenőrzések automatikus frissítése 15 percenként.</span><span class="sxs-lookup"><span data-stu-id="4d35a-137">By default, checks are auto-refreshed every 15 minutes.</span></span>

![Az SAP HANA Studióban, lépjen a konzol tooAdministration: riasztások: megjelenítése: az összes riasztás](./media/troubleshooting-monitoring/image1-show-alerts.png)

<span data-ttu-id="4d35a-139">**PROCESSZOR**</span><span class="sxs-lookup"><span data-stu-id="4d35a-139">**CPU**</span></span>

<span data-ttu-id="4d35a-140">Tooimproper küszöbérték beállítása miatt kiváltott riasztást a felbontása esetén tooreset toohello alapértelmezett érték vagy egyéb ésszerű küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="4d35a-140">For an alert triggered due tooimproper threshold setting, a resolution is tooreset toohello default value or a more reasonable threshold value.</span></span>

![Új toohello alapértelmezett értékét, vagy egyéb ésszerű küszöbértéket](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

<span data-ttu-id="4d35a-142">a következő figyelmeztetések hello jelezheti CPU erőforrás problémák:</span><span class="sxs-lookup"><span data-stu-id="4d35a-142">hello following alerts may indicate CPU resource problems:</span></span>

- <span data-ttu-id="4d35a-143">Gazdagép CPU-használat (riasztás 5)</span><span class="sxs-lookup"><span data-stu-id="4d35a-143">Host CPU Usage (Alert 5)</span></span>
- <span data-ttu-id="4d35a-144">Legutóbbi mentésipont-művelet (riasztás 28)</span><span class="sxs-lookup"><span data-stu-id="4d35a-144">Most recent savepoint operation (Alert 28)</span></span>
- <span data-ttu-id="4d35a-145">Mentési pont időtartama (riasztás 54)</span><span class="sxs-lookup"><span data-stu-id="4d35a-145">Savepoint duration (Alert 54)</span></span>

<span data-ttu-id="4d35a-146">Azt tapasztalhatja, hogy magas CPU-felhasználás a SAP HANA-adatbázisból származó hello következő meg:</span><span class="sxs-lookup"><span data-stu-id="4d35a-146">You may notice high CPU consumption on your SAP HANA database from one of hello following:</span></span>

- <span data-ttu-id="4d35a-147">Riasztási 5 (gazdagép CPU-használat) jelenik meg, az aktuális és korábbi CPU-használat</span><span class="sxs-lookup"><span data-stu-id="4d35a-147">Alert 5 (Host CPU usage) is raised for current or past CPU usage</span></span>
- <span data-ttu-id="4d35a-148">hello CPU-használat a hello áttekintés képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4d35a-148">hello displayed CPU usage on hello overview screen</span></span>

![CPU-használat megjelenő hello áttekintés képernyő](./media/troubleshooting-monitoring/image3-cpu-usage.png)

<span data-ttu-id="4d35a-150">hello terhelés graph magas CPU-felhasználás, vagy a fogyasztás magas lehet, hogy megjelenítése a múltbeli hello:</span><span class="sxs-lookup"><span data-stu-id="4d35a-150">hello Load graph might show high CPU consumption, or high consumption in hello past:</span></span>

![hello terhelés graph előfordulhat, hogy megjelenítése magas CPU-felhasználás, vagy magas fogyasztás hello elmúlt](./media/troubleshooting-monitoring/image4-load-graph.png)

<span data-ttu-id="4d35a-152">Riasztás toohigh Processzor kihasználtsága miatt indított több oka is, de nem kizárólagosan okozhatja: bizonyos tranzakciókat, az adatok betöltése, a feladatok sokáig futnak az SQL-utasításokat vagy hibás lekérdezések teljesítményét (például a BW HANA a függő végrehajtása a kockák).</span><span class="sxs-lookup"><span data-stu-id="4d35a-152">An alert triggered due toohigh CPU utilization could be caused by several reasons, including, but not limited to: execution of certain transactions, data loading, hanging of jobs, long running SQL statements, and bad query performance (for example, with BW on HANA cubes).</span></span>

<span data-ttu-id="4d35a-153">Tekintse meg a toohello [SAP HANA-hibaelhárítás: CPU kapcsolódó okoz, és a megoldások](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4d35a-153">Refer toohello [SAP HANA Troubleshooting: CPU Related Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="4d35a-154">**Operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="4d35a-154">**Operating System**</span></span>

<span data-ttu-id="4d35a-155">Az egyik legfontosabb hello ellenőrzi, hogy az SAP HANA Linux toomake meg arról, hogy a transzparens túl nagy lapok le vannak tiltva, lásd: [SAP Megjegyzés #2131662 – átlátszó túl nagy lapok (THP) SAP HANA-kiszolgálókon](https://launchpad.support.sap.com/#/notes/2131662).</span><span class="sxs-lookup"><span data-stu-id="4d35a-155">One of hello most important checks for SAP HANA on Linux is toomake sure that Transparent Huge Pages are disabled, see [SAP Note #2131662 – Transparent Huge Pages (THP) on SAP HANA Servers](https://launchpad.support.sap.com/#/notes/2131662).</span></span>

- <span data-ttu-id="4d35a-156">Ellenőrizheti, hogy engedélyezve vannak-e átlátszó túl nagy lapok hello Linux parancs a következő használatával: **/sys/kernel/mm/transparent macskaeledel\_hugepage/engedélyezve**</span><span class="sxs-lookup"><span data-stu-id="4d35a-156">You can check if Transparent Huge Pages are enabled through hello following Linux command: **cat /sys/kernel/mm/transparent\_hugepage/enabled**</span></span>
- <span data-ttu-id="4d35a-157">Ha _mindig_ kapcsos zárójelek között, az alábbi, az azt jelenti, hogy engedélyezve vannak-e a hello átlátszó túl nagy lapok: [mindig] madvise soha nem; Ha _soha nem_ szimpla zárójelek között, az alábbi, az azt jelenti, hogy hello átlátszó Túl nagy lapok le vannak tiltva: mindig madvise [soha nem]</span><span class="sxs-lookup"><span data-stu-id="4d35a-157">If _always_ is enclosed in brackets as below, it means that hello Transparent Huge Pages are enabled: [always] madvise never; if _never_ is enclosed in brackets as below, it means that hello Transparent Huge Pages are disabled: always madvise [never]</span></span>

<span data-ttu-id="4d35a-158">Linux parancs a következő hello semmi sem kell visszaadnia: **rpm - qa |} grep ulimit.**</span><span class="sxs-lookup"><span data-stu-id="4d35a-158">hello following Linux command should return nothing: **rpm -qa | grep ulimit.**</span></span> <span data-ttu-id="4d35a-159">Ha úgy tűnik, _ulimit_ van telepítve, távolítsa el azonnal.</span><span class="sxs-lookup"><span data-stu-id="4d35a-159">If it appears _ulimit_ is installed, uninstall it immediately.</span></span>

<span data-ttu-id="4d35a-160">**Memória**</span><span class="sxs-lookup"><span data-stu-id="4d35a-160">**Memory**</span></span>

<span data-ttu-id="4d35a-161">Előfordulhat, hogy figyelje meg, hogy hello összeg hello SAP HANA által lefoglalt memória adatbázisa a vártnál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="4d35a-161">You may observe that hello amount of memory allocated by hello SAP HANA database is higher than expected.</span></span> <span data-ttu-id="4d35a-162">a következő figyelmeztetések hello magas memóriahasználatban kapcsolatos hibákat jelzik:</span><span class="sxs-lookup"><span data-stu-id="4d35a-162">hello following alerts indicate issues with high memory usage:</span></span>

- <span data-ttu-id="4d35a-163">Állomás fizikai memória használata (riasztás 1.)</span><span class="sxs-lookup"><span data-stu-id="4d35a-163">Host physical memory usage (Alert 1)</span></span>
- <span data-ttu-id="4d35a-164">A névkiszolgáló (riasztás 12) memóriahasználata</span><span class="sxs-lookup"><span data-stu-id="4d35a-164">Memory usage of name server (Alert 12)</span></span>
- <span data-ttu-id="4d35a-165">Teljes memóriahasználatát oszlop tárolási táblák (riasztás 40)</span><span class="sxs-lookup"><span data-stu-id="4d35a-165">Total memory usage of Column Store tables (Alert 40)</span></span>
- <span data-ttu-id="4d35a-166">Memóriahasználat (riasztás 43) szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="4d35a-166">Memory usage of services (Alert 43)</span></span>
- <span data-ttu-id="4d35a-167">Memóriahasználat fő tárolási oszlop tárolási táblák (riasztás 45)</span><span class="sxs-lookup"><span data-stu-id="4d35a-167">Memory usage of main storage of Column Store tables (Alert 45)</span></span>
- <span data-ttu-id="4d35a-168">Futásidejű memóriaképek (riasztás 46)</span><span class="sxs-lookup"><span data-stu-id="4d35a-168">Runtime dump files (Alert 46)</span></span>

<span data-ttu-id="4d35a-169">Tekintse meg a toohello [SAP HANA-hibaelhárítás: memóriával kapcsolatos problémák](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4d35a-169">Refer toohello [SAP HANA Troubleshooting: Memory Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="4d35a-170">**Hálózat**</span><span class="sxs-lookup"><span data-stu-id="4d35a-170">**Network**</span></span>

<span data-ttu-id="4d35a-171">Tekintse meg a túl[SAP Megjegyzés #2081065 – hibaelhárítás SAP HANA hálózati](https://launchpad.support.sap.com/#/notes/2081065) , és végezze el a hello hálózati hibaelhárítási lépések az SAP Megjegyzés vehető fel.</span><span class="sxs-lookup"><span data-stu-id="4d35a-171">Refer too[SAP Note #2081065 – Troubleshooting SAP HANA Network](https://launchpad.support.sap.com/#/notes/2081065) and perform hello network troubleshooting steps in this SAP Note.</span></span>

1. <span data-ttu-id="4d35a-172">Kiszolgáló és az ügyfél közötti üzenetváltások idő elemzése.</span><span class="sxs-lookup"><span data-stu-id="4d35a-172">Analyzing round-trip time between server and client.</span></span>
  <span data-ttu-id="4d35a-173">A.</span><span class="sxs-lookup"><span data-stu-id="4d35a-173">A.</span></span> <span data-ttu-id="4d35a-174">Hello SQL-parancsfájl futtatása [ _HANA\_hálózati\_ügyfelek_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="4d35a-174">Run hello SQL script [_HANA\_Network\_Clients_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>
  
2. <span data-ttu-id="4d35a-175">Elemezze a fürtök csomóponton belüli kommunikációjához kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="4d35a-175">Analyze internode communication.</span></span>
  <span data-ttu-id="4d35a-176">A.</span><span class="sxs-lookup"><span data-stu-id="4d35a-176">A.</span></span> <span data-ttu-id="4d35a-177">SQL-parancsfájl futtatása [ _HANA\_hálózati\_szolgáltatások_](https://launchpad.support.sap.com/#/notes/1969700)_._</span><span class="sxs-lookup"><span data-stu-id="4d35a-177">Run SQL script [_HANA\_Network\_Services_](https://launchpad.support.sap.com/#/notes/1969700)_._</span></span>

3. <span data-ttu-id="4d35a-178">Linux parancs **ifconfig** (hello kimeneti látható, ha a csomag veszteségeiért is megjelenhetnek).</span><span class="sxs-lookup"><span data-stu-id="4d35a-178">Run Linux command **ifconfig** (hello output shows if any packet losses are occurring).</span></span>
4. <span data-ttu-id="4d35a-179">Linux parancs **tcpdump parancsot**.</span><span class="sxs-lookup"><span data-stu-id="4d35a-179">Run Linux command **tcpdump**.</span></span>

<span data-ttu-id="4d35a-180">Használja továbbá hello nyílt forráskódú [IPERF](https://iperf.fr/) eszköz (vagy hasonlót) toomeasure alkalmazás valós hálózati teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="4d35a-180">Also, use hello open source [IPERF](https://iperf.fr/) tool (or similar) toomeasure real application network performance.</span></span>

<span data-ttu-id="4d35a-181">Tekintse meg a toohello [SAP HANA-hibaelhárítás: hálózati teljesítmény és a kapcsolódási problémái vannak](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4d35a-181">Refer toohello [SAP HANA Troubleshooting: Networking Performance and Connectivity Problems](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="4d35a-182">**Tárolás**</span><span class="sxs-lookup"><span data-stu-id="4d35a-182">**Storage**</span></span>

<span data-ttu-id="4d35a-183">Végfelhasználói szempontból egy alkalmazás (vagy hello rendszer egészének) nehézkesen futtatja, nem válaszol vagy is még úgy tűnik, toohang i/o-teljesítménnyel kapcsolatos problémák esetén.</span><span class="sxs-lookup"><span data-stu-id="4d35a-183">From an end-user perspective, an application (or hello system as a whole) runs sluggishly, is unresponsive, or can even seem toohang if there are issues with I/O performance.</span></span> <span data-ttu-id="4d35a-184">A hello **kötetek** lapon a SAP HANA Studio, hogy hello csatolt kötetek, és milyen kötetek minden szolgáltatás által használt.</span><span class="sxs-lookup"><span data-stu-id="4d35a-184">In hello **Volumes** tab in SAP HANA Studio, you can see hello attached volumes, and what volumes are used by each service.</span></span>

![A SAP HANA Studio hello kötetek lapján látható hello csatolt kötetek, és milyen kötetek minden szolgáltatás által használt](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

<span data-ttu-id="4d35a-186">Csatlakoztatott kötetek részleteit láthatja hello képernyő alsó részén hello hello kötetek, fájlok és az i/o-statisztikák például.</span><span class="sxs-lookup"><span data-stu-id="4d35a-186">Attached volumes in hello lower part of hello screen you can see details of hello volumes, such as files and I/O statistics.</span></span>

![Csatlakoztatott kötetek részleteit láthatja hello képernyő alsó részén hello hello kötetek, például fájlok és az i/o-statisztikák](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

<span data-ttu-id="4d35a-188">Tekintse meg a toohello [SAP HANA-hibaelhárítás: i/o kapcsolódó okait és megoldásait](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) és [SAP HANA-hibaelhárítás: lemez kapcsolódó okait és megoldásait](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) helyen részletes hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4d35a-188">Refer toohello [SAP HANA Troubleshooting: I/O Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) and [SAP HANA Troubleshooting: Disk Related Root Causes and Solutions](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) site for detailed troubleshooting steps.</span></span>

<span data-ttu-id="4d35a-189">**Diagnosztikai eszközök**</span><span class="sxs-lookup"><span data-stu-id="4d35a-189">**Diagnostic Tools**</span></span>

<span data-ttu-id="4d35a-190">Hajtsa végre egy SAP HANA állapot-ellenőrzéssel keresztül HANA\_konfigurációs\_Minichecks.</span><span class="sxs-lookup"><span data-stu-id="4d35a-190">Perform an SAP HANA Health Check through HANA\_Configuration\_Minichecks.</span></span> <span data-ttu-id="4d35a-191">Ez az eszköz, amely kell már merült fel az SAP HANA Studio riasztásként elvégzésével kritikus technikai problémák adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4d35a-191">This tool returns potentially critical technical issues that should have already been raised as alerts in SAP HANA Studio.</span></span>

<span data-ttu-id="4d35a-192">Tekintse meg a túl[SAP Megjegyzés #1969700 – SQL utasítás gyűjtemény SAP Hana](https://launchpad.support.sap.com/#/notes/1969700) , és töltse le a hello SQL Statements.zip fájl csatolt toothat megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="4d35a-192">Refer too[SAP Note #1969700 – SQL statement collection for SAP HANA](https://launchpad.support.sap.com/#/notes/1969700) and download hello SQL Statements.zip file attached toothat note.</span></span> <span data-ttu-id="4d35a-193">A .zip fájlt hello helyi merevlemezen tárolja.</span><span class="sxs-lookup"><span data-stu-id="4d35a-193">Store this .zip file on hello local hard drive.</span></span>

<span data-ttu-id="4d35a-194">Az SAP HANA Studio hello **Rendszerinformáció** lapra, kattintson a jobb gombbal a hello **neve** oszlop, és válassza ki **importálási SQL-utasítások**.</span><span class="sxs-lookup"><span data-stu-id="4d35a-194">In SAP HANA Studio, on hello **System Information** tab, right-click in hello **Name** column and select **Import SQL Statements**.</span></span>

![Az SAP HANA Studio, a hello Rendszerinformáció lapon kattintson a jobb gombbal a hello neve oszlopban, és válassza ki a importálási SQL-utasítások](./media/troubleshooting-monitoring/image7-import-statements-a.png)

<span data-ttu-id="4d35a-196">SELECT hello SQL Statements.zip fájlt helyileg tárolja, és importálja a megfelelő SQL-utasítások hello egy mappa.</span><span class="sxs-lookup"><span data-stu-id="4d35a-196">Select hello SQL Statements.zip file stored locally, and a folder with hello corresponding SQL statements will be imported.</span></span> <span data-ttu-id="4d35a-197">Ezen a ponton hello számos különböző diagnosztikai ellenőrzések az alábbi SQL-utasítások is futtatható.</span><span class="sxs-lookup"><span data-stu-id="4d35a-197">At this point, hello many different diagnostic checks can be run with these SQL statements.</span></span>

<span data-ttu-id="4d35a-198">Például tootest SAP HANA rendszer replikáció sávszélesség-követelményekkel, kattintson a jobb gombbal hello **sávszélesség** utasítás alapján **replikációs: sávszélesség** válassza ki **nyitott** SQL-konzolon.</span><span class="sxs-lookup"><span data-stu-id="4d35a-198">For example, tootest SAP HANA System Replication bandwidth requirements, right-click hello **Bandwidth** statement under **Replication: Bandwidth** and select **Open** in SQL Console.</span></span>

<span data-ttu-id="4d35a-199">hello teljes SQL utasítás megnyitja lehetővé tevő bemeneti paraméterek (módosítása szakaszát) toobe megváltozott, és végrehajthatók.</span><span class="sxs-lookup"><span data-stu-id="4d35a-199">hello complete SQL statement opens allowing input parameters (modification section) toobe changed and then executed.</span></span>

![hello teljes SQL-utasítás lehetővé tevő bemeneti paraméterek (módosítása szakaszát) megváltozott, és végrehajthatók toobe megnyitása](./media/troubleshooting-monitoring/image8-import-statements-b.png)

<span data-ttu-id="4d35a-201">Egy másik példa: hello utasítások alapján a jobb gombbal kattint **replikációs: áttekintés**.</span><span class="sxs-lookup"><span data-stu-id="4d35a-201">Another example is right-clicking on hello statements under **Replication: Overview**.</span></span> <span data-ttu-id="4d35a-202">Válassza ki **Execute** hello helyi menüben:</span><span class="sxs-lookup"><span data-stu-id="4d35a-202">Select **Execute** from hello context menu:</span></span>

![Jobb gombbal kattint, a replikációs csoportban hello utasítások egy másik példa: áttekintése.](./media/troubleshooting-monitoring/image9-import-statements-c.png)

<span data-ttu-id="4d35a-205">Ennek eredményeként a hibaelhárítási információkat:</span><span class="sxs-lookup"><span data-stu-id="4d35a-205">This results in information that helps with troubleshooting:</span></span>

![Ennek eredményeképpen az adatokat, amelyek segítenek a hibaelhárításban](./media/troubleshooting-monitoring/image10-import-statements-d.png)

<span data-ttu-id="4d35a-207">Hello azonos Hana\_konfigurációs\_Minichecks és az összes ellenőrzés _X_ jeleket a hello _C_ (kritikus) oszlopban.</span><span class="sxs-lookup"><span data-stu-id="4d35a-207">Do hello same for HANA\_Configuration\_Minichecks and check for any _X_ marks in hello _C_ (Critical) column.</span></span>

<span data-ttu-id="4d35a-208">A minta kimenete:</span><span class="sxs-lookup"><span data-stu-id="4d35a-208">Sample outputs:</span></span>

<span data-ttu-id="4d35a-209">**HANA\_konfigurációs\_MiniChecks\_Rev102.01 + 1** az általános SAP HANA ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="4d35a-209">**HANA\_Configuration\_MiniChecks\_Rev102.01+1** for general SAP HANA checks.</span></span>

![HANA\_konfigurációs\_MiniChecks\_Rev102.01 + 1. a általános SAP HANA-ellenőrzése](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

<span data-ttu-id="4d35a-211">**HANA\_szolgáltatások\_áttekintése** mi SAP HANA futó szolgáltatások áttekintését.</span><span class="sxs-lookup"><span data-stu-id="4d35a-211">**HANA\_Services\_Overview** for an overview of what SAP HANA services are currently running.</span></span>

![HANA\_szolgáltatások\_megtudhatja, mi SAP HANA futó szolgáltatások – áttekintés](./media/troubleshooting-monitoring/image12-services-overview.png)

<span data-ttu-id="4d35a-213">**HANA\_szolgáltatások\_statisztika** SAP Hana szolgáltatás információkat (Processzor, memória, stb.).</span><span class="sxs-lookup"><span data-stu-id="4d35a-213">**HANA\_Services\_Statistics** for SAP HANA service information (CPU, memory, etc.).</span></span>

![<span data-ttu-id="4d35a-214">HANA\_szolgáltatások\_SAP Hana statisztika szolgáltatás információkat</span><span class="sxs-lookup"><span data-stu-id="4d35a-214">HANA\_Services\_Statistics for SAP HANA service information</span></span> ](./media/troubleshooting-monitoring/image13-services-statistics.png)

<span data-ttu-id="4d35a-215">**HANA\_konfigurációs\_áttekintése\_Rev110 +** hello SAP HANA-példány általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="4d35a-215">**HANA\_Configuration\_Overview\_Rev110+** for general information on hello SAP HANA instance.</span></span>

![HANA\_konfigurációs\_áttekintése\_Rev110 + hello SAP HANA-példányon általános információk](./media/troubleshooting-monitoring/image14-configuration-overview.png)

<span data-ttu-id="4d35a-217">**HANA\_konfigurációs\_paraméterek\_Rev70 +** toocheck SAP HANA-paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="4d35a-217">**HANA\_Configuration\_Parameters\_Rev70+** toocheck SAP HANA parameters.</span></span>

![HANA\_konfigurációs\_paraméterek\_Rev70 + toocheck SAP HANA-paraméterek](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

