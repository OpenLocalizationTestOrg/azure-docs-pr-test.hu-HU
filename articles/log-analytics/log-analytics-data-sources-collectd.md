---
title: "az OMS szolgáltatáshoz CollectD aaaCollect adatait |} Microsoft Docs"
description: "CollectD egy nyílt forráskódú Linux-démon, amely rendszeres időközönként gyűjti az adatokat az alkalmazások és a rendszer a szintre vonatkozó információ.  Ez a cikk tájékoztatást nyújt a Naplóelemzési CollectD adatainak begyűjtése."
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
ms.date: 05/02/2017
ms.author: magoedte
ms.openlocfilehash: 7ad82c9c67a664aabd44f08bef2253d84cd2dfba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a><span data-ttu-id="5c553-104">Gyűjti az adatokat a CollectD Naplóelemzési Linux-ügynökök</span><span class="sxs-lookup"><span data-stu-id="5c553-104">Collect data from CollectD on Linux agents in Log Analytics</span></span>
<span data-ttu-id="5c553-105">[CollectD](https://collectd.org/) van egy nyílt forráskódú Linux-démon rendszeresen teljesítménymutatók az összegyűjtő alkalmazások és a rendszer a szintre vonatkozó információ.</span><span class="sxs-lookup"><span data-stu-id="5c553-105">[CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects performance metrics from applications and system level information.</span></span> <span data-ttu-id="5c553-106">Példa alkalmazások hello Java virtuális gép (JVM), a MySQL-kiszolgáló és a Nginx tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c553-106">Example applications include hello Java Virtual Machine (JVM), MySQL Server, and Nginx.</span></span> <span data-ttu-id="5c553-107">Ez a cikk tájékoztatást nyújt teljesítményadatok összegyűjtése a Naplóelemzési CollectD.</span><span class="sxs-lookup"><span data-stu-id="5c553-107">This article provides information on collecting performance data from CollectD in Log Analytics.</span></span>

<span data-ttu-id="5c553-108">Elérhető beépülő modulok teljes listája megtalálható [beépülő modulok tábla](https://collectd.org/wiki/index.php/Table_of_Plugins).</span><span class="sxs-lookup"><span data-stu-id="5c553-108">A full list of available plugins can be found at [Table of Plugins](https://collectd.org/wiki/index.php/Table_of_Plugins).</span></span>

![CollectD áttekintése](media/log-analytics-data-sources-collectd/overview.png)

<span data-ttu-id="5c553-110">hello következő CollectD konfiguráció része a Linux tooroute CollectD adatok toohello OMS-ügynököt az OMS-ügynököt hello Linux.</span><span class="sxs-lookup"><span data-stu-id="5c553-110">hello following CollectD configuration is included in hello OMS Agent for Linux tooroute  CollectD data toohello OMS Agent for Linux.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

<span data-ttu-id="5c553-111">Emellett ha egy collectD verzióját használja, ehelyett a következő konfigurációs hello 5.5 használatához.</span><span class="sxs-lookup"><span data-stu-id="5c553-111">Additionally, if using an versions of collectD before 5.5 use hello following configuration instead.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

<span data-ttu-id="5c553-112">hello CollectD konfiguráció használja hello alapértelmezett`write_http` beépülő modul toosend metrika teljesítményadatokat keresztül port 26000 tooOMS Linux-ügynök.</span><span class="sxs-lookup"><span data-stu-id="5c553-112">hello CollectD configuration uses hello default`write_http` plugin toosend performance metric data over port 26000 tooOMS Agent for Linux.</span></span> 

> [!NOTE]
> <span data-ttu-id="5c553-113">Ezt a portot lehet egyénileg definiált konfigurált tooa port, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="5c553-113">This port can be configured tooa custom-defined port if needed.</span></span>

<span data-ttu-id="5c553-114">hello Linux OMS-ügynököt is porton figyel 26000 CollectD metrikáihoz, és adja őket az tooOMS séma metrikák alakítja.</span><span class="sxs-lookup"><span data-stu-id="5c553-114">hello OMS Agent for Linux also listens on port 26000 for CollectD metrics and then converts them tooOMS schema metrics.</span></span> <span data-ttu-id="5c553-115">hello az alábbiakban az OMS-ügynököt hello Linux konfiguráció `collectd.conf`.</span><span class="sxs-lookup"><span data-stu-id="5c553-115">hello following is hello OMS Agent for Linux configuration  `collectd.conf`.</span></span>

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a><span data-ttu-id="5c553-116">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="5c553-116">Versions supported</span></span>
- <span data-ttu-id="5c553-117">Naplóelemzési jelenleg CollectD 4.8 verzió vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="5c553-117">Log Analytics currently supports CollectD version 4.8 and above.</span></span>
- <span data-ttu-id="5c553-118">A Linux v1.1.0-217 vagy újabb OMS-ügynököt CollectD metrika gyűjtemény szükség.</span><span class="sxs-lookup"><span data-stu-id="5c553-118">OMS Agent for Linux v1.1.0-217 or above is required for CollectD metric collection.</span></span>


## <a name="configuration"></a><span data-ttu-id="5c553-119">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="5c553-119">Configuration</span></span>
<span data-ttu-id="5c553-120">Az alábbiakban hello CollectD adatok Naplóelemzési a lépéseken tooconfigure gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="5c553-120">hello following are basic steps tooconfigure collection of CollectD data in Log Analytics.</span></span>

1. <span data-ttu-id="5c553-121">CollectD toosend adatok toohello OMS-ügynököt konfigurálása Linux hello write_http beépülő modul használatával.</span><span class="sxs-lookup"><span data-stu-id="5c553-121">Configure CollectD toosend data toohello OMS Agent for Linux using hello write_http plugin.</span></span>  
2. <span data-ttu-id="5c553-122">A Linux toolisten hello CollectD adatok az OMS-ügynököt hello konfigurálása hello megfelelő portot.</span><span class="sxs-lookup"><span data-stu-id="5c553-122">Configure hello OMS Agent for Linux toolisten for hello CollectD data on hello appropriate port.</span></span>
3. <span data-ttu-id="5c553-123">Indítsa újra a CollectD és Linux OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="5c553-123">Restart CollectD and OMS Agent for Linux.</span></span>

### <a name="configure-collectd-tooforward-data"></a><span data-ttu-id="5c553-124">CollectD tooforward adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5c553-124">Configure CollectD tooforward data</span></span> 

1. <span data-ttu-id="5c553-125">tooroute CollectD adatok toohello Linux, OMS-ügynököt `oms.conf` igényeinek toobe hozzáadott tooCollectD tartozó konfigurációs könyvtára.</span><span class="sxs-lookup"><span data-stu-id="5c553-125">tooroute CollectD data toohello OMS Agent for Linux, `oms.conf` needs toobe added tooCollectD's configuration directory.</span></span> <span data-ttu-id="5c553-126">a fájl hello cél hello Linux distro a gép függ.</span><span class="sxs-lookup"><span data-stu-id="5c553-126">hello destination of this file depends on hello Linux  distro of your machine.</span></span>

    <span data-ttu-id="5c553-127">Ha a CollectD konfigurációs könyvtár /etc/collectd.d/ találhatók:</span><span class="sxs-lookup"><span data-stu-id="5c553-127">If your CollectD config directory is located in /etc/collectd.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    <span data-ttu-id="5c553-128">Ha a CollectD konfigurációs könyvtár /etc/collectd/collectd.conf.d/ találhatók:</span><span class="sxs-lookup"><span data-stu-id="5c553-128">If your CollectD config directory is located in /etc/collectd/collectd.conf.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    ><span data-ttu-id="5c553-129">5.5 előtt CollectD verzióihoz fog toomodify hello címkék `oms.conf` a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="5c553-129">For CollectD versions before 5.5 you will have toomodify hello tags in `oms.conf` as shown above.</span></span>
    >

2. <span data-ttu-id="5c553-130">Másolja a collectd.conf szükséges toohello munkaterület omsagent konfigurációs könyvtára.</span><span class="sxs-lookup"><span data-stu-id="5c553-130">Copy collectd.conf toohello desired workspace's omsagent configuration directory.</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. <span data-ttu-id="5c553-131">Indítsa újra a CollectD és az OMS-ügynököt Linux a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="5c553-131">Restart CollectD and OMS Agent for Linux with hello following commands.</span></span>

    <span data-ttu-id="5c553-132">sudo szolgáltatás collectd sudo /opt/microsoft/omsagent/bin/service_control újraindítás</span><span class="sxs-lookup"><span data-stu-id="5c553-132">sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart</span></span>

## <a name="collectd-metrics-toolog-analytics-schema-conversion"></a><span data-ttu-id="5c553-133">CollectD metrikák tooLog Analytics séma átalakítás</span><span class="sxs-lookup"><span data-stu-id="5c553-133">CollectD metrics tooLog Analytics schema conversion</span></span>
<span data-ttu-id="5c553-134">toomaintain séma-hozzárendeléséhez a következő hello használt CollectD által gyűjtött egy ismerős modell infrastruktúra mérőszámokat már OMS-ügynök a Linux és hello új mérőszámok között:</span><span class="sxs-lookup"><span data-stu-id="5c553-134">toomaintain a familiar model between infrastructure metrics already collected by OMS Agent for Linux and hello new metrics collected by CollectD hello following schema mapping is used:</span></span>

| <span data-ttu-id="5c553-135">CollectD metrika mező</span><span class="sxs-lookup"><span data-stu-id="5c553-135">CollectD Metric field</span></span> | <span data-ttu-id="5c553-136">Log Analytics mező</span><span class="sxs-lookup"><span data-stu-id="5c553-136">Log Analytics field</span></span> |
|:--|:--|
| <span data-ttu-id="5c553-137">állomás</span><span class="sxs-lookup"><span data-stu-id="5c553-137">host</span></span> | <span data-ttu-id="5c553-138">Computer</span><span class="sxs-lookup"><span data-stu-id="5c553-138">Computer</span></span> |
| <span data-ttu-id="5c553-139">Beépülő modul</span><span class="sxs-lookup"><span data-stu-id="5c553-139">plugin</span></span> | <span data-ttu-id="5c553-140">None</span><span class="sxs-lookup"><span data-stu-id="5c553-140">None</span></span> |
| <span data-ttu-id="5c553-141">plugin_instance</span><span class="sxs-lookup"><span data-stu-id="5c553-141">plugin_instance</span></span> | <span data-ttu-id="5c553-142">Példány neve</span><span class="sxs-lookup"><span data-stu-id="5c553-142">Instance Name</span></span><br><span data-ttu-id="5c553-143">Ha **plugin_instance** van *null* majd példánynév = "*_Total*"</span><span class="sxs-lookup"><span data-stu-id="5c553-143">If **plugin_instance** is *null* then InstanceName="*_Total*"</span></span> |
| <span data-ttu-id="5c553-144">type</span><span class="sxs-lookup"><span data-stu-id="5c553-144">type</span></span> | <span data-ttu-id="5c553-145">ObjectName</span><span class="sxs-lookup"><span data-stu-id="5c553-145">ObjectName</span></span> |
| <span data-ttu-id="5c553-146">type_instance</span><span class="sxs-lookup"><span data-stu-id="5c553-146">type_instance</span></span> | <span data-ttu-id="5c553-147">CounterName</span><span class="sxs-lookup"><span data-stu-id="5c553-147">CounterName</span></span><br><span data-ttu-id="5c553-148">Ha **type_instance** van *null* majd CounterName =**üres**</span><span class="sxs-lookup"><span data-stu-id="5c553-148">If **type_instance** is *null* then CounterName=**blank**</span></span> |
| <span data-ttu-id="5c553-149">dsnames]</span><span class="sxs-lookup"><span data-stu-id="5c553-149">dsnames[]</span></span> | <span data-ttu-id="5c553-150">CounterName</span><span class="sxs-lookup"><span data-stu-id="5c553-150">CounterName</span></span> |
| <span data-ttu-id="5c553-151">dstypes</span><span class="sxs-lookup"><span data-stu-id="5c553-151">dstypes</span></span> | <span data-ttu-id="5c553-152">None</span><span class="sxs-lookup"><span data-stu-id="5c553-152">None</span></span> |
| <span data-ttu-id="5c553-153">[érték]</span><span class="sxs-lookup"><span data-stu-id="5c553-153">values[]</span></span> | <span data-ttu-id="5c553-154">Ellenértéknek</span><span class="sxs-lookup"><span data-stu-id="5c553-154">CounterValue</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5c553-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c553-155">Next steps</span></span>
* <span data-ttu-id="5c553-156">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="5c553-156">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
* <span data-ttu-id="5c553-157">Használjon [egyéni mezők](log-analytics-custom-fields.md) tooparse adatainak syslog rekordból egyes mezőkbe.</span><span class="sxs-lookup"><span data-stu-id="5c553-157">Use [Custom Fields](log-analytics-custom-fields.md) tooparse data from syslog records into individual fields.</span></span>

