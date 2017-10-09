---
title: "aaaCollect Nagios és Zabbix riasztások az OMS szolgáltatáshoz |} Microsoft Docs"
description: "Nagios és Zabbix figyelési eszközök nyílt forráskódú. Is összegyűjteni riasztást ezeket az eszközöket a rendelés tooanalyze a Log Analyticshez való azokat más forrásokból származó riasztások együtt.  Ez a cikk ismerteti, hogyan tooconfigure hello OMS-ügynököt a Linux toocollect riasztást küld, ezen rendszerekből."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 23e2252e4fed8bc87baec063694a8472ca84220d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a><span data-ttu-id="e8f60-105">Nagios és az OMS-ügynököt a Naplóelemzési Zabbix riasztások gyűjtése Linux</span><span class="sxs-lookup"><span data-stu-id="e8f60-105">Collect alerts from Nagios and Zabbix in Log Analytics from OMS Agent for Linux</span></span> 
<span data-ttu-id="e8f60-106">[Nagios](https://www.nagios.org/) és [Zabbix](http://www.zabbix.com/) nyílt forráskódú figyelési eszközök.</span><span class="sxs-lookup"><span data-stu-id="e8f60-106">[Nagios](https://www.nagios.org/) and [Zabbix](http://www.zabbix.com/) are open source monitoring tools.</span></span>  <span data-ttu-id="e8f60-107">Képes összegyűjteni riasztást ezeket az eszközöket a Log Analyticshez való rendelés tooanalyze a azokat, valamint [más forrásokból származó riasztások](log-analytics-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e8f60-107">You can collect alerts from these tools into Log Analytics in order tooanalyze them along with [alerts from other sources](log-analytics-alerts.md).</span></span>  <span data-ttu-id="e8f60-108">Ez a cikk ismerteti, hogyan tooconfigure hello OMS-ügynököt a Linux toocollect riasztást küld, ezen rendszerekből.</span><span class="sxs-lookup"><span data-stu-id="e8f60-108">This article describes how tooconfigure hello OMS Agent for Linux toocollect alerts from these systems.</span></span>
 
## <a name="configure-alert-collection"></a><span data-ttu-id="e8f60-109">Riasztások gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8f60-109">Configure alert collection</span></span>

### <a name="configuring-nagios-alert-collection"></a><span data-ttu-id="e8f60-110">Nagios riasztások gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8f60-110">Configuring Nagios alert collection</span></span>
<span data-ttu-id="e8f60-111">Hajtsa végre a lépéseket követve hello Nagios server toocollect riasztások hello.</span><span class="sxs-lookup"><span data-stu-id="e8f60-111">Perform hello following steps on hello Nagios server toocollect alerts.</span></span>

1. <span data-ttu-id="e8f60-112">Támogatás hello felhasználói **omsagent** olvasási hozzáférés toohello Nagios naplófájl (azaz `/var/log/nagios/nagios.log`).</span><span class="sxs-lookup"><span data-stu-id="e8f60-112">Grant hello user **omsagent** read access toohello Nagios log file (i.e. `/var/log/nagios/nagios.log`).</span></span> <span data-ttu-id="e8f60-113">Feltéve, hogy hello nagios.log fájl hello csoport tulajdonosa `nagios`, hozzáadhat hello felhasználót **omsagent** toohello **nagios** csoport.</span><span class="sxs-lookup"><span data-stu-id="e8f60-113">Assuming hello nagios.log file is owned by hello group `nagios`, you can add hello user **omsagent** toohello **nagios** group.</span></span> 

    <span data-ttu-id="e8f60-114">sudo felhasználó módosítási - a -G nagios omsagent</span><span class="sxs-lookup"><span data-stu-id="e8f60-114">sudo usermod -a -G nagios omsagent</span></span>

2.  <span data-ttu-id="e8f60-115">Hello konfigurációs fájlban a következő módosítása (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="e8f60-115">Modify hello configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="e8f60-116">Győződjön meg arról, bejegyzéseket a következő hello jelen, és nem megjegyzésként ki:</span><span class="sxs-lookup"><span data-stu-id="e8f60-116">Ensure hello following entries are present and not commented out:</span></span>  

        <source>  
          type tail  
          #Update path toopoint tooyour nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. <span data-ttu-id="e8f60-117">Indítsa újra a hello omsagent démon</span><span class="sxs-lookup"><span data-stu-id="e8f60-117">Restart hello omsagent daemon</span></span>

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a><span data-ttu-id="e8f60-118">Zabbix riasztások gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e8f60-118">Configuring Zabbix alert collection</span></span>
<span data-ttu-id="e8f60-119">toocollect riasztások Zabbix-kiszolgálótól, kell toospecify egy felhasználó és a jelszó *törölje a szöveget*.</span><span class="sxs-lookup"><span data-stu-id="e8f60-119">toocollect alerts from a Zabbix server, you need toospecify a user and password in *clear text*.</span></span> <span data-ttu-id="e8f60-120">Ez nem ideális, de javasoljuk, hogy hello felhasználó létrehozása, és engedélyek toomonitor onlu biztosítani.</span><span class="sxs-lookup"><span data-stu-id="e8f60-120">This is not ideal, but we recommend that you create hello user and grant permissions toomonitor onlu.</span></span>

<span data-ttu-id="e8f60-121">Hajtsa végre a lépéseket követve hello Nagios server toocollect riasztások hello.</span><span class="sxs-lookup"><span data-stu-id="e8f60-121">Perform hello following steps on hello Nagios server toocollect alerts.</span></span>

1. <span data-ttu-id="e8f60-122">Hello konfigurációs fájlban a következő módosítása (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="e8f60-122">Modify hello configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="e8f60-123">Győződjön meg arról, bejegyzéseket a következő hello jelen, és nem megjegyzésként ki.  Hello felhasználói nevet és jelszót a Zabbix környezetnek toovalues módosítása.</span><span class="sxs-lookup"><span data-stu-id="e8f60-123">Ensure hello following entries are present and not commented out.  Change hello user name and password toovalues for your Zabbix environment.</span></span>

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. <span data-ttu-id="e8f60-124">Indítsa újra a hello omsagent démon</span><span class="sxs-lookup"><span data-stu-id="e8f60-124">Restart hello omsagent daemon</span></span>

    <span data-ttu-id="e8f60-125">sudo megosztása /opt/microsoft/omsagent/bin/service_control újraindítása</span><span class="sxs-lookup"><span data-stu-id="e8f60-125">sudo sh /opt/microsoft/omsagent/bin/service_control restart</span></span>


## <a name="alert-records"></a><span data-ttu-id="e8f60-126">Riasztási rekordok</span><span class="sxs-lookup"><span data-stu-id="e8f60-126">Alert records</span></span>
<span data-ttu-id="e8f60-127">Riasztási rekordok beolvasható Nagios és Zabbix használatával [keresések jelentkezzen](log-analytics-log-searches.md) a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="e8f60-127">You can retrieve alert records from Nagios and Zabbix using [log searches](log-analytics-log-searches.md) in Log Analytics.</span></span>

### <a name="nagios-alert-records"></a><span data-ttu-id="e8f60-128">Nagios riasztás rekordok</span><span class="sxs-lookup"><span data-stu-id="e8f60-128">Nagios Alert records</span></span>

<span data-ttu-id="e8f60-129">Riasztási Nagios által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Nagios**.</span><span class="sxs-lookup"><span data-stu-id="e8f60-129">Alert records collected by Nagios have a **Type** of **Alert** and a **SourceSystem** of **Nagios**.</span></span>  <span data-ttu-id="e8f60-130">A következő táblázat hello hello tulajdonságok rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="e8f60-130">They have hello properties in hello following table.</span></span>

| <span data-ttu-id="e8f60-131">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8f60-131">Property</span></span> | <span data-ttu-id="e8f60-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8f60-132">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e8f60-133">Típus</span><span class="sxs-lookup"><span data-stu-id="e8f60-133">Type</span></span> |<span data-ttu-id="e8f60-134">*Riasztás*</span><span class="sxs-lookup"><span data-stu-id="e8f60-134">*Alert*</span></span> |
| <span data-ttu-id="e8f60-135">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="e8f60-135">SourceSystem</span></span> |<span data-ttu-id="e8f60-136">*Nagios*</span><span class="sxs-lookup"><span data-stu-id="e8f60-136">*Nagios*</span></span> |
| <span data-ttu-id="e8f60-137">AlertName</span><span class="sxs-lookup"><span data-stu-id="e8f60-137">AlertName</span></span> |<span data-ttu-id="e8f60-138">Hello riasztás nevét.</span><span class="sxs-lookup"><span data-stu-id="e8f60-138">Name of hello alert.</span></span> |
| <span data-ttu-id="e8f60-139">AlertDescription</span><span class="sxs-lookup"><span data-stu-id="e8f60-139">AlertDescription</span></span> | <span data-ttu-id="e8f60-140">Hello riasztás leírása.</span><span class="sxs-lookup"><span data-stu-id="e8f60-140">Description of hello alert.</span></span> |
| <span data-ttu-id="e8f60-141">AlertState</span><span class="sxs-lookup"><span data-stu-id="e8f60-141">AlertState</span></span> | <span data-ttu-id="e8f60-142">Hello szolgáltatást vagy a gazdagép állapota.</span><span class="sxs-lookup"><span data-stu-id="e8f60-142">Status of hello service or host.</span></span><br><br><span data-ttu-id="e8f60-143">OKÉ</span><span class="sxs-lookup"><span data-stu-id="e8f60-143">OK</span></span><br><span data-ttu-id="e8f60-144">FIGYELMEZTETÉS</span><span class="sxs-lookup"><span data-stu-id="e8f60-144">WARNING</span></span><br><span data-ttu-id="e8f60-145">MENTÉSE</span><span class="sxs-lookup"><span data-stu-id="e8f60-145">UP</span></span><br><span data-ttu-id="e8f60-146">LEFELÉ</span><span class="sxs-lookup"><span data-stu-id="e8f60-146">DOWN</span></span> |
| <span data-ttu-id="e8f60-147">Állomásnév</span><span class="sxs-lookup"><span data-stu-id="e8f60-147">HostName</span></span> | <span data-ttu-id="e8f60-148">Hello riasztást létrehozó hello állomás neve.</span><span class="sxs-lookup"><span data-stu-id="e8f60-148">Name of hello host that created hello alert.</span></span> |
| <span data-ttu-id="e8f60-149">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="e8f60-149">PriorityNumber</span></span> | <span data-ttu-id="e8f60-150">Hello riasztás prioritását.</span><span class="sxs-lookup"><span data-stu-id="e8f60-150">Priority level of hello alert.</span></span> |
| <span data-ttu-id="e8f60-151">StateType</span><span class="sxs-lookup"><span data-stu-id="e8f60-151">StateType</span></span> | <span data-ttu-id="e8f60-152">hello típusa hello riasztás állapota.</span><span class="sxs-lookup"><span data-stu-id="e8f60-152">hello type of state of hello alert.</span></span><br><br><span data-ttu-id="e8f60-153">SOFT - hiba, amely nem ellenőrizni.</span><span class="sxs-lookup"><span data-stu-id="e8f60-153">SOFT - Issue that has not been rechecked.</span></span><br><span data-ttu-id="e8f60-154">MEREVLEMEZ - probléma, amelyet újra ellenőrizni kell a megadott számú alkalommal.</span><span class="sxs-lookup"><span data-stu-id="e8f60-154">HARD - Issue that has been rechecked a specified number of times.</span></span>  |
| <span data-ttu-id="e8f60-155">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="e8f60-155">TimeGenerated</span></span> |<span data-ttu-id="e8f60-156">Dátum és idő hello riasztás létrejött.</span><span class="sxs-lookup"><span data-stu-id="e8f60-156">Date and time hello alert was created.</span></span> |


### <a name="zabbix-alert-records"></a><span data-ttu-id="e8f60-157">Zabbix riasztási rekordok</span><span class="sxs-lookup"><span data-stu-id="e8f60-157">Zabbix alert records</span></span>
<span data-ttu-id="e8f60-158">Riasztási Zabbix által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Zabbix**.</span><span class="sxs-lookup"><span data-stu-id="e8f60-158">Alert records collected by Zabbix have a **Type** of **Alert** and a **SourceSystem** of **Zabbix**.</span></span>  <span data-ttu-id="e8f60-159">A következő táblázat hello hello tulajdonságok rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="e8f60-159">They have hello properties in hello following table.</span></span>

| <span data-ttu-id="e8f60-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8f60-160">Property</span></span> | <span data-ttu-id="e8f60-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8f60-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e8f60-162">Típus</span><span class="sxs-lookup"><span data-stu-id="e8f60-162">Type</span></span> |<span data-ttu-id="e8f60-163">*Riasztás*</span><span class="sxs-lookup"><span data-stu-id="e8f60-163">*Alert*</span></span> |
| <span data-ttu-id="e8f60-164">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="e8f60-164">SourceSystem</span></span> |<span data-ttu-id="e8f60-165">*Zabbix*</span><span class="sxs-lookup"><span data-stu-id="e8f60-165">*Zabbix*</span></span> |
| <span data-ttu-id="e8f60-166">AlertName</span><span class="sxs-lookup"><span data-stu-id="e8f60-166">AlertName</span></span> | <span data-ttu-id="e8f60-167">Hello riasztás nevét.</span><span class="sxs-lookup"><span data-stu-id="e8f60-167">Name of hello alert.</span></span> |
| <span data-ttu-id="e8f60-168">AlertPriority</span><span class="sxs-lookup"><span data-stu-id="e8f60-168">AlertPriority</span></span> | <span data-ttu-id="e8f60-169">Hello riasztás súlyossága.</span><span class="sxs-lookup"><span data-stu-id="e8f60-169">Severity of hello alert.</span></span><br><br><span data-ttu-id="e8f60-170">nem minősített</span><span class="sxs-lookup"><span data-stu-id="e8f60-170">not classified</span></span><br><span data-ttu-id="e8f60-171">Információ</span><span class="sxs-lookup"><span data-stu-id="e8f60-171">information</span></span><br><span data-ttu-id="e8f60-172">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="e8f60-172">warning</span></span><br><span data-ttu-id="e8f60-173">Átlagos</span><span class="sxs-lookup"><span data-stu-id="e8f60-173">average</span></span><br><span data-ttu-id="e8f60-174">Magas</span><span class="sxs-lookup"><span data-stu-id="e8f60-174">high</span></span><br><span data-ttu-id="e8f60-175">vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="e8f60-175">disaster</span></span>  |
| <span data-ttu-id="e8f60-176">AlertState</span><span class="sxs-lookup"><span data-stu-id="e8f60-176">AlertState</span></span> | <span data-ttu-id="e8f60-177">Hello riasztás állapota.</span><span class="sxs-lookup"><span data-stu-id="e8f60-177">State of hello alert.</span></span><br><br><span data-ttu-id="e8f60-178">0 - állapot toodate működik.</span><span class="sxs-lookup"><span data-stu-id="e8f60-178">0 - State is up toodate.</span></span><br><span data-ttu-id="e8f60-179">1 - állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="e8f60-179">1 - State is unknown.</span></span>  |
| <span data-ttu-id="e8f60-180">AlertTypeNumber</span><span class="sxs-lookup"><span data-stu-id="e8f60-180">AlertTypeNumber</span></span> | <span data-ttu-id="e8f60-181">Meghatározza, hogy riasztást hozhat létre a probléma több esemény.</span><span class="sxs-lookup"><span data-stu-id="e8f60-181">Specifies whether alert can generate multiple problem events.</span></span><br><br><span data-ttu-id="e8f60-182">0 - állapot toodate működik.</span><span class="sxs-lookup"><span data-stu-id="e8f60-182">0 - State is up toodate.</span></span><br><span data-ttu-id="e8f60-183">1 - állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="e8f60-183">1 - State is unknown.</span></span>    |
| <span data-ttu-id="e8f60-184">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e8f60-184">Comments</span></span> | <span data-ttu-id="e8f60-185">A riasztásra vonatkozóan további megjegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="e8f60-185">Additional comments for alert.</span></span> |
| <span data-ttu-id="e8f60-186">Állomásnév</span><span class="sxs-lookup"><span data-stu-id="e8f60-186">HostName</span></span> | <span data-ttu-id="e8f60-187">Hello riasztást létrehozó hello állomás neve.</span><span class="sxs-lookup"><span data-stu-id="e8f60-187">Name of hello host that created hello alert.</span></span> |
| <span data-ttu-id="e8f60-188">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="e8f60-188">PriorityNumber</span></span> | <span data-ttu-id="e8f60-189">Hello riasztás súlyossága jelző érték.</span><span class="sxs-lookup"><span data-stu-id="e8f60-189">Value indicating severity of hello alert.</span></span><br><br><span data-ttu-id="e8f60-190">0 – nem besorolt</span><span class="sxs-lookup"><span data-stu-id="e8f60-190">0 - not classified</span></span><br><span data-ttu-id="e8f60-191">1 - információk</span><span class="sxs-lookup"><span data-stu-id="e8f60-191">1 - information</span></span><br><span data-ttu-id="e8f60-192">2 – figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="e8f60-192">2 - warning</span></span><br><span data-ttu-id="e8f60-193">3 – átlag</span><span class="sxs-lookup"><span data-stu-id="e8f60-193">3 - average</span></span><br><span data-ttu-id="e8f60-194">4 - magas</span><span class="sxs-lookup"><span data-stu-id="e8f60-194">4 - high</span></span><br><span data-ttu-id="e8f60-195">5 - vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="e8f60-195">5 - disaster</span></span> |
| <span data-ttu-id="e8f60-196">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="e8f60-196">TimeGenerated</span></span> |<span data-ttu-id="e8f60-197">Dátum és idő hello riasztás létrejött.</span><span class="sxs-lookup"><span data-stu-id="e8f60-197">Date and time hello alert was created.</span></span> |
| <span data-ttu-id="e8f60-198">TimeLastModified</span><span class="sxs-lookup"><span data-stu-id="e8f60-198">TimeLastModified</span></span> |<span data-ttu-id="e8f60-199">Utolsó módosításának dátuma és időpontja hello riasztás hello állapota.</span><span class="sxs-lookup"><span data-stu-id="e8f60-199">Date and time hello state of hello alert was last changed.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="e8f60-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8f60-200">Next steps</span></span>
* <span data-ttu-id="e8f60-201">További tudnivalók [riasztások](log-analytics-alerts.md) a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="e8f60-201">Learn about [alerts](log-analytics-alerts.md) in Log Analytics.</span></span>
* <span data-ttu-id="e8f60-202">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="e8f60-202">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
