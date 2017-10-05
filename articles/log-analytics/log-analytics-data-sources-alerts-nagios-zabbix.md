---
title: "Az OMS szolgáltatáshoz Nagios és Zabbix riasztások gyűjtése |} Microsoft Docs"
description: "Nagios és Zabbix figyelési eszközök nyílt forráskódú. Begyűjtheti riasztások ezeket az eszközöket a Log Analyticshez való ahhoz, hogy a más forrásokból származó riasztások együtt elemezheti őket.  Ez a cikk ismerteti a riasztások gyűjteni ezek a rendszerek Linux OMS-ügynök konfigurálása."
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
ms.openlocfilehash: 0b64c32e1031e704d50aab0b38eaea41e27d134b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a><span data-ttu-id="8e08e-105">Nagios és az OMS-ügynököt a Naplóelemzési Zabbix riasztások gyűjtése Linux</span><span class="sxs-lookup"><span data-stu-id="8e08e-105">Collect alerts from Nagios and Zabbix in Log Analytics from OMS Agent for Linux</span></span> 
<span data-ttu-id="8e08e-106">[Nagios](https://www.nagios.org/) és [Zabbix](http://www.zabbix.com/) nyílt forráskódú figyelési eszközök.</span><span class="sxs-lookup"><span data-stu-id="8e08e-106">[Nagios](https://www.nagios.org/) and [Zabbix](http://www.zabbix.com/) are open source monitoring tools.</span></span>  <span data-ttu-id="8e08e-107">Begyűjtheti riasztások ezeket az eszközöket a Log Analyticshez való elemezheti őket, valamint hogy [más forrásokból származó riasztások](log-analytics-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="8e08e-107">You can collect alerts from these tools into Log Analytics in order to analyze them along with [alerts from other sources](log-analytics-alerts.md).</span></span>  <span data-ttu-id="8e08e-108">Ez a cikk ismerteti a riasztások gyűjteni ezek a rendszerek Linux OMS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8e08e-108">This article describes how to configure the OMS Agent for Linux to collect alerts from these systems.</span></span>
 
## <a name="configure-alert-collection"></a><span data-ttu-id="8e08e-109">Riasztások gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e08e-109">Configure alert collection</span></span>

### <a name="configuring-nagios-alert-collection"></a><span data-ttu-id="8e08e-110">Nagios riasztások gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e08e-110">Configuring Nagios alert collection</span></span>
<span data-ttu-id="8e08e-111">A következő lépésekkel a Nagios kiszolgálón riasztások gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="8e08e-111">Perform the following steps on the Nagios server to collect alerts.</span></span>

1. <span data-ttu-id="8e08e-112">Adja meg a felhasználónak **omsagent** olvasási hozzáférés a Nagios naplófájl (azaz `/var/log/nagios/nagios.log`).</span><span class="sxs-lookup"><span data-stu-id="8e08e-112">Grant the user **omsagent** read access to the Nagios log file (i.e. `/var/log/nagios/nagios.log`).</span></span> <span data-ttu-id="8e08e-113">A csoport tulajdonosa feltéve, hogy a nagios.log fájl `nagios`, a felhasználó hozzáadhat **omsagent** való a **nagios** csoport.</span><span class="sxs-lookup"><span data-stu-id="8e08e-113">Assuming the nagios.log file is owned by the group `nagios`, you can add the user **omsagent** to the **nagios** group.</span></span> 

    <span data-ttu-id="8e08e-114">sudo felhasználó módosítási - a -G nagios omsagent</span><span class="sxs-lookup"><span data-stu-id="8e08e-114">sudo usermod -a -G nagios omsagent</span></span>

2.  <span data-ttu-id="8e08e-115">Módosítsa a konfigurációs fájlban a következő (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="8e08e-115">Modify the configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="8e08e-116">Győződjön meg arról, a következő tételek jelen, és nem megjegyzésként ki:</span><span class="sxs-lookup"><span data-stu-id="8e08e-116">Ensure the following entries are present and not commented out:</span></span>  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. <span data-ttu-id="8e08e-117">Indítsa újra a omsagent démon</span><span class="sxs-lookup"><span data-stu-id="8e08e-117">Restart the omsagent daemon</span></span>

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a><span data-ttu-id="8e08e-118">Zabbix riasztások gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8e08e-118">Configuring Zabbix alert collection</span></span>
<span data-ttu-id="8e08e-119">A riasztások gyűjtése Zabbix-kiszolgálótól, meg kell adnia egy felhasználói és a jelszó *törölje a szöveget*.</span><span class="sxs-lookup"><span data-stu-id="8e08e-119">To collect alerts from a Zabbix server, you need to specify a user and password in *clear text*.</span></span> <span data-ttu-id="8e08e-120">Ez nem ideális, de javasoljuk, hogy a felhasználó létrehozása, és engedélyezze, hogy onlu figyelése.</span><span class="sxs-lookup"><span data-stu-id="8e08e-120">This is not ideal, but we recommend that you create the user and grant permissions to monitor onlu.</span></span>

<span data-ttu-id="8e08e-121">A következő lépésekkel a Nagios kiszolgálón riasztások gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="8e08e-121">Perform the following steps on the Nagios server to collect alerts.</span></span>

1. <span data-ttu-id="8e08e-122">Módosítsa a konfigurációs fájlban a következő (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span><span class="sxs-lookup"><span data-stu-id="8e08e-122">Modify the configuration file at (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`).</span></span> <span data-ttu-id="8e08e-123">Győződjön meg arról, a következő tételek jelen, és nem megjegyzésként ki.</span><span class="sxs-lookup"><span data-stu-id="8e08e-123">Ensure the following entries are present and not commented out.</span></span>  <span data-ttu-id="8e08e-124">A felhasználónév és jelszó átállítása értékek környezetnek Zabbix.</span><span class="sxs-lookup"><span data-stu-id="8e08e-124">Change the user name and password to values for your Zabbix environment.</span></span>

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. <span data-ttu-id="8e08e-125">Indítsa újra a omsagent démon</span><span class="sxs-lookup"><span data-stu-id="8e08e-125">Restart the omsagent daemon</span></span>

    <span data-ttu-id="8e08e-126">sudo megosztása /opt/microsoft/omsagent/bin/service_control újraindítása</span><span class="sxs-lookup"><span data-stu-id="8e08e-126">sudo sh /opt/microsoft/omsagent/bin/service_control restart</span></span>


## <a name="alert-records"></a><span data-ttu-id="8e08e-127">Riasztási rekordok</span><span class="sxs-lookup"><span data-stu-id="8e08e-127">Alert records</span></span>
<span data-ttu-id="8e08e-128">Riasztási rekordok beolvasható Nagios és Zabbix használatával [keresések jelentkezzen](log-analytics-log-searches.md) a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="8e08e-128">You can retrieve alert records from Nagios and Zabbix using [log searches](log-analytics-log-searches.md) in Log Analytics.</span></span>

### <a name="nagios-alert-records"></a><span data-ttu-id="8e08e-129">Nagios riasztás rekordok</span><span class="sxs-lookup"><span data-stu-id="8e08e-129">Nagios Alert records</span></span>

<span data-ttu-id="8e08e-130">Riasztási Nagios által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Nagios**.</span><span class="sxs-lookup"><span data-stu-id="8e08e-130">Alert records collected by Nagios have a **Type** of **Alert** and a **SourceSystem** of **Nagios**.</span></span>  <span data-ttu-id="8e08e-131">A Tulajdonságok rendelkeznek, az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="8e08e-131">They have the properties in the following table.</span></span>

| <span data-ttu-id="8e08e-132">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8e08e-132">Property</span></span> | <span data-ttu-id="8e08e-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="8e08e-133">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e08e-134">Típus</span><span class="sxs-lookup"><span data-stu-id="8e08e-134">Type</span></span> |<span data-ttu-id="8e08e-135">*Riasztás*</span><span class="sxs-lookup"><span data-stu-id="8e08e-135">*Alert*</span></span> |
| <span data-ttu-id="8e08e-136">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="8e08e-136">SourceSystem</span></span> |<span data-ttu-id="8e08e-137">*Nagios*</span><span class="sxs-lookup"><span data-stu-id="8e08e-137">*Nagios*</span></span> |
| <span data-ttu-id="8e08e-138">AlertName</span><span class="sxs-lookup"><span data-stu-id="8e08e-138">AlertName</span></span> |<span data-ttu-id="8e08e-139">A riasztás nevét.</span><span class="sxs-lookup"><span data-stu-id="8e08e-139">Name of the alert.</span></span> |
| <span data-ttu-id="8e08e-140">AlertDescription</span><span class="sxs-lookup"><span data-stu-id="8e08e-140">AlertDescription</span></span> | <span data-ttu-id="8e08e-141">A riasztás leírása.</span><span class="sxs-lookup"><span data-stu-id="8e08e-141">Description of the alert.</span></span> |
| <span data-ttu-id="8e08e-142">AlertState</span><span class="sxs-lookup"><span data-stu-id="8e08e-142">AlertState</span></span> | <span data-ttu-id="8e08e-143">A szolgáltatás vagy a gazdagép állapota.</span><span class="sxs-lookup"><span data-stu-id="8e08e-143">Status of the service or host.</span></span><br><br><span data-ttu-id="8e08e-144">OKÉ</span><span class="sxs-lookup"><span data-stu-id="8e08e-144">OK</span></span><br><span data-ttu-id="8e08e-145">FIGYELMEZTETÉS</span><span class="sxs-lookup"><span data-stu-id="8e08e-145">WARNING</span></span><br><span data-ttu-id="8e08e-146">MENTÉSE</span><span class="sxs-lookup"><span data-stu-id="8e08e-146">UP</span></span><br><span data-ttu-id="8e08e-147">LEFELÉ</span><span class="sxs-lookup"><span data-stu-id="8e08e-147">DOWN</span></span> |
| <span data-ttu-id="8e08e-148">Állomásnév</span><span class="sxs-lookup"><span data-stu-id="8e08e-148">HostName</span></span> | <span data-ttu-id="8e08e-149">A gazdagép által létrehozott, a riasztás nevét.</span><span class="sxs-lookup"><span data-stu-id="8e08e-149">Name of the host that created the alert.</span></span> |
| <span data-ttu-id="8e08e-150">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="8e08e-150">PriorityNumber</span></span> | <span data-ttu-id="8e08e-151">A riasztás prioritását.</span><span class="sxs-lookup"><span data-stu-id="8e08e-151">Priority level of the alert.</span></span> |
| <span data-ttu-id="8e08e-152">StateType</span><span class="sxs-lookup"><span data-stu-id="8e08e-152">StateType</span></span> | <span data-ttu-id="8e08e-153">A riasztás állapota típusa.</span><span class="sxs-lookup"><span data-stu-id="8e08e-153">The type of state of the alert.</span></span><br><br><span data-ttu-id="8e08e-154">SOFT - hiba, amely nem ellenőrizni.</span><span class="sxs-lookup"><span data-stu-id="8e08e-154">SOFT - Issue that has not been rechecked.</span></span><br><span data-ttu-id="8e08e-155">MEREVLEMEZ - probléma, amelyet újra ellenőrizni kell a megadott számú alkalommal.</span><span class="sxs-lookup"><span data-stu-id="8e08e-155">HARD - Issue that has been rechecked a specified number of times.</span></span>  |
| <span data-ttu-id="8e08e-156">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="8e08e-156">TimeGenerated</span></span> |<span data-ttu-id="8e08e-157">A riasztás létrehozásának dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="8e08e-157">Date and time the alert was created.</span></span> |


### <a name="zabbix-alert-records"></a><span data-ttu-id="8e08e-158">Zabbix riasztási rekordok</span><span class="sxs-lookup"><span data-stu-id="8e08e-158">Zabbix alert records</span></span>
<span data-ttu-id="8e08e-159">Riasztási Zabbix által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Zabbix**.</span><span class="sxs-lookup"><span data-stu-id="8e08e-159">Alert records collected by Zabbix have a **Type** of **Alert** and a **SourceSystem** of **Zabbix**.</span></span>  <span data-ttu-id="8e08e-160">A Tulajdonságok rendelkeznek, az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="8e08e-160">They have the properties in the following table.</span></span>

| <span data-ttu-id="8e08e-161">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8e08e-161">Property</span></span> | <span data-ttu-id="8e08e-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="8e08e-162">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="8e08e-163">Típus</span><span class="sxs-lookup"><span data-stu-id="8e08e-163">Type</span></span> |<span data-ttu-id="8e08e-164">*Riasztás*</span><span class="sxs-lookup"><span data-stu-id="8e08e-164">*Alert*</span></span> |
| <span data-ttu-id="8e08e-165">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="8e08e-165">SourceSystem</span></span> |<span data-ttu-id="8e08e-166">*Zabbix*</span><span class="sxs-lookup"><span data-stu-id="8e08e-166">*Zabbix*</span></span> |
| <span data-ttu-id="8e08e-167">AlertName</span><span class="sxs-lookup"><span data-stu-id="8e08e-167">AlertName</span></span> | <span data-ttu-id="8e08e-168">A riasztás nevét.</span><span class="sxs-lookup"><span data-stu-id="8e08e-168">Name of the alert.</span></span> |
| <span data-ttu-id="8e08e-169">AlertPriority</span><span class="sxs-lookup"><span data-stu-id="8e08e-169">AlertPriority</span></span> | <span data-ttu-id="8e08e-170">A riasztás súlyossága.</span><span class="sxs-lookup"><span data-stu-id="8e08e-170">Severity of the alert.</span></span><br><br><span data-ttu-id="8e08e-171">nem minősített</span><span class="sxs-lookup"><span data-stu-id="8e08e-171">not classified</span></span><br><span data-ttu-id="8e08e-172">Információ</span><span class="sxs-lookup"><span data-stu-id="8e08e-172">information</span></span><br><span data-ttu-id="8e08e-173">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="8e08e-173">warning</span></span><br><span data-ttu-id="8e08e-174">Átlagos</span><span class="sxs-lookup"><span data-stu-id="8e08e-174">average</span></span><br><span data-ttu-id="8e08e-175">Magas</span><span class="sxs-lookup"><span data-stu-id="8e08e-175">high</span></span><br><span data-ttu-id="8e08e-176">vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="8e08e-176">disaster</span></span>  |
| <span data-ttu-id="8e08e-177">AlertState</span><span class="sxs-lookup"><span data-stu-id="8e08e-177">AlertState</span></span> | <span data-ttu-id="8e08e-178">A riasztás állapota.</span><span class="sxs-lookup"><span data-stu-id="8e08e-178">State of the alert.</span></span><br><br><span data-ttu-id="8e08e-179">0 - állapota naprakészek legyenek.</span><span class="sxs-lookup"><span data-stu-id="8e08e-179">0 - State is up to date.</span></span><br><span data-ttu-id="8e08e-180">1 - állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="8e08e-180">1 - State is unknown.</span></span>  |
| <span data-ttu-id="8e08e-181">AlertTypeNumber</span><span class="sxs-lookup"><span data-stu-id="8e08e-181">AlertTypeNumber</span></span> | <span data-ttu-id="8e08e-182">Meghatározza, hogy riasztást hozhat létre a probléma több esemény.</span><span class="sxs-lookup"><span data-stu-id="8e08e-182">Specifies whether alert can generate multiple problem events.</span></span><br><br><span data-ttu-id="8e08e-183">0 - állapota naprakészek legyenek.</span><span class="sxs-lookup"><span data-stu-id="8e08e-183">0 - State is up to date.</span></span><br><span data-ttu-id="8e08e-184">1 - állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="8e08e-184">1 - State is unknown.</span></span>    |
| <span data-ttu-id="8e08e-185">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8e08e-185">Comments</span></span> | <span data-ttu-id="8e08e-186">A riasztásra vonatkozóan további megjegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="8e08e-186">Additional comments for alert.</span></span> |
| <span data-ttu-id="8e08e-187">Állomásnév</span><span class="sxs-lookup"><span data-stu-id="8e08e-187">HostName</span></span> | <span data-ttu-id="8e08e-188">A gazdagép által létrehozott, a riasztás nevét.</span><span class="sxs-lookup"><span data-stu-id="8e08e-188">Name of the host that created the alert.</span></span> |
| <span data-ttu-id="8e08e-189">PriorityNumber</span><span class="sxs-lookup"><span data-stu-id="8e08e-189">PriorityNumber</span></span> | <span data-ttu-id="8e08e-190">A riasztás súlyossága jelző érték.</span><span class="sxs-lookup"><span data-stu-id="8e08e-190">Value indicating severity of the alert.</span></span><br><br><span data-ttu-id="8e08e-191">0 – nem besorolt</span><span class="sxs-lookup"><span data-stu-id="8e08e-191">0 - not classified</span></span><br><span data-ttu-id="8e08e-192">1 - információk</span><span class="sxs-lookup"><span data-stu-id="8e08e-192">1 - information</span></span><br><span data-ttu-id="8e08e-193">2 – figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="8e08e-193">2 - warning</span></span><br><span data-ttu-id="8e08e-194">3 – átlag</span><span class="sxs-lookup"><span data-stu-id="8e08e-194">3 - average</span></span><br><span data-ttu-id="8e08e-195">4 - magas</span><span class="sxs-lookup"><span data-stu-id="8e08e-195">4 - high</span></span><br><span data-ttu-id="8e08e-196">5 - vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="8e08e-196">5 - disaster</span></span> |
| <span data-ttu-id="8e08e-197">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="8e08e-197">TimeGenerated</span></span> |<span data-ttu-id="8e08e-198">A riasztás létrehozásának dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="8e08e-198">Date and time the alert was created.</span></span> |
| <span data-ttu-id="8e08e-199">TimeLastModified</span><span class="sxs-lookup"><span data-stu-id="8e08e-199">TimeLastModified</span></span> |<span data-ttu-id="8e08e-200">Dátum és idő, a riasztás állapota módosította utoljára.</span><span class="sxs-lookup"><span data-stu-id="8e08e-200">Date and time the state of the alert was last changed.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="8e08e-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e08e-201">Next steps</span></span>
* <span data-ttu-id="8e08e-202">További tudnivalók [riasztások](log-analytics-alerts.md) a Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="8e08e-202">Learn about [alerts](log-analytics-alerts.md) in Log Analytics.</span></span>
* <span data-ttu-id="8e08e-203">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) az adatforrások és a megoldások gyűjtött adatok elemzésére.</span><span class="sxs-lookup"><span data-stu-id="8e08e-203">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 
