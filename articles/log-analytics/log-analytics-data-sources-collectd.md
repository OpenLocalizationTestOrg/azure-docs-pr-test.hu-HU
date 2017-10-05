---
title: "Adatgyűjtés a CollectD az OMS szolgáltatáshoz |} Microsoft Docs"
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
ms.openlocfilehash: a63b15ca5126b45451f0694c9ee75d7b67b1ceaf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a><span data-ttu-id="42c41-104">Gyűjti az adatokat a CollectD Naplóelemzési Linux-ügynökök</span><span class="sxs-lookup"><span data-stu-id="42c41-104">Collect data from CollectD on Linux agents in Log Analytics</span></span>
<span data-ttu-id="42c41-105">[CollectD](https://collectd.org/) van egy nyílt forráskódú Linux-démon rendszeresen teljesítménymutatók az összegyűjtő alkalmazások és a rendszer a szintre vonatkozó információ.</span><span class="sxs-lookup"><span data-stu-id="42c41-105">[CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects performance metrics from applications and system level information.</span></span> <span data-ttu-id="42c41-106">Példa alkalmazások közé tartoznak a Java virtuális gép (JVM), a MySQL-kiszolgáló és a Nginx.</span><span class="sxs-lookup"><span data-stu-id="42c41-106">Example applications include the Java Virtual Machine (JVM), MySQL Server, and Nginx.</span></span> <span data-ttu-id="42c41-107">Ez a cikk tájékoztatást nyújt teljesítményadatok összegyűjtése a Naplóelemzési CollectD.</span><span class="sxs-lookup"><span data-stu-id="42c41-107">This article provides information on collecting performance data from CollectD in Log Analytics.</span></span>

<span data-ttu-id="42c41-108">Elérhető beépülő modulok teljes listája megtalálható [beépülő modulok tábla](https://collectd.org/wiki/index.php/Table_of_Plugins).</span><span class="sxs-lookup"><span data-stu-id="42c41-108">A full list of available plugins can be found at [Table of Plugins](https://collectd.org/wiki/index.php/Table_of_Plugins).</span></span>

![CollectD áttekintése](media/log-analytics-data-sources-collectd/overview.png)

<span data-ttu-id="42c41-110">A következő CollectD konfiguráció megtalálható az OMS-ügynököt Linux útvonal CollectD az adatokat az OMS-ügynököt Linux.</span><span class="sxs-lookup"><span data-stu-id="42c41-110">The following CollectD configuration is included in the OMS Agent for Linux to route  CollectD data to the OMS Agent for Linux.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

<span data-ttu-id="42c41-111">Emellett ha egy collectD verzióját használja, mielőtt 5.5 helyette használja az alábbi konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="42c41-111">Additionally, if using an versions of collectD before 5.5 use the following configuration instead.</span></span>

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

<span data-ttu-id="42c41-112">Az alapértelmezett értéket használja, a CollectD konfigurációs`write_http` adatokat küldeni a teljesítmény metrika 26000 porton keresztül OMS-ügynököt a Linux rendszerhez használt beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="42c41-112">The CollectD configuration uses the default`write_http` plugin to send performance metric data over port 26000 to OMS Agent for Linux.</span></span> 

> [!NOTE]
> <span data-ttu-id="42c41-113">Ez a port beállítható úgy, hogy egy egyénileg definiált port szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="42c41-113">This port can be configured to a custom-defined port if needed.</span></span>

<span data-ttu-id="42c41-114">Linux OMS-ügynököt is porton figyel 26000 CollectD metrikáihoz, és majd alakítja át ezeket OMS séma metrikákat.</span><span class="sxs-lookup"><span data-stu-id="42c41-114">The OMS Agent for Linux also listens on port 26000 for CollectD metrics and then converts them to OMS schema metrics.</span></span> <span data-ttu-id="42c41-115">Az alábbiakban található a Linux-konfigurációhoz OMS-ügynököt `collectd.conf`.</span><span class="sxs-lookup"><span data-stu-id="42c41-115">The following is the OMS Agent for Linux configuration  `collectd.conf`.</span></span>

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a><span data-ttu-id="42c41-116">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="42c41-116">Versions supported</span></span>
- <span data-ttu-id="42c41-117">Naplóelemzési jelenleg CollectD 4.8 verzió vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="42c41-117">Log Analytics currently supports CollectD version 4.8 and above.</span></span>
- <span data-ttu-id="42c41-118">A Linux v1.1.0-217 vagy újabb OMS-ügynököt CollectD metrika gyűjtemény szükség.</span><span class="sxs-lookup"><span data-stu-id="42c41-118">OMS Agent for Linux v1.1.0-217 or above is required for CollectD metric collection.</span></span>


## <a name="configuration"></a><span data-ttu-id="42c41-119">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="42c41-119">Configuration</span></span>
<span data-ttu-id="42c41-120">Az alábbi lépések alapvető Naplóelemzési CollectD adatgyűjtés konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="42c41-120">The following are basic steps to configure collection of CollectD data in Log Analytics.</span></span>

1. <span data-ttu-id="42c41-121">Szeretnék adatokat küldeni a az OMS-ügynököt a write_http beépülő modul használata Linux CollectD konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="42c41-121">Configure CollectD to send data to the OMS Agent for Linux using the write_http plugin.</span></span>  
2. <span data-ttu-id="42c41-122">A CollectD adatok a megfelelő porton figyeljen Linux az OMS-ügynök konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="42c41-122">Configure the OMS Agent for Linux to listen for the CollectD data on the appropriate port.</span></span>
3. <span data-ttu-id="42c41-123">Indítsa újra a CollectD és Linux OMS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="42c41-123">Restart CollectD and OMS Agent for Linux.</span></span>

### <a name="configure-collectd-to-forward-data"></a><span data-ttu-id="42c41-124">Adatok CollectD konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42c41-124">Configure CollectD to forward data</span></span> 

1. <span data-ttu-id="42c41-125">Az OMS-ügynököt, Linux, az útvonal CollectD adatokat `oms.conf` hozzá kell adni a CollectD tartozó konfigurációs könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="42c41-125">To route CollectD data to the OMS Agent for Linux, `oms.conf` needs to be added to CollectD's configuration directory.</span></span> <span data-ttu-id="42c41-126">Ez a fájl és a gép a Linux distro függ.</span><span class="sxs-lookup"><span data-stu-id="42c41-126">The destination of this file depends on the Linux  distro of your machine.</span></span>

    <span data-ttu-id="42c41-127">Ha a CollectD konfigurációs könyvtár /etc/collectd.d/ találhatók:</span><span class="sxs-lookup"><span data-stu-id="42c41-127">If your CollectD config directory is located in /etc/collectd.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    <span data-ttu-id="42c41-128">Ha a CollectD konfigurációs könyvtár /etc/collectd/collectd.conf.d/ találhatók:</span><span class="sxs-lookup"><span data-stu-id="42c41-128">If your CollectD config directory is located in /etc/collectd/collectd.conf.d/:</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    ><span data-ttu-id="42c41-129">A címkék módosításához hogy CollectD verzióihoz 5.5 előtt `oms.conf` a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="42c41-129">For CollectD versions before 5.5 you will have to modify the tags in `oms.conf` as shown above.</span></span>
    >

2. <span data-ttu-id="42c41-130">Collectd.conf másolja a kívánt munkaterület omsagent konfigurációs könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="42c41-130">Copy collectd.conf to the desired workspace's omsagent configuration directory.</span></span>

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. <span data-ttu-id="42c41-131">Indítsa újra a CollectD és az OMS-ügynököt Linux az alábbi parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="42c41-131">Restart CollectD and OMS Agent for Linux with the following commands.</span></span>

    <span data-ttu-id="42c41-132">sudo szolgáltatás collectd sudo /opt/microsoft/omsagent/bin/service_control újraindítás</span><span class="sxs-lookup"><span data-stu-id="42c41-132">sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart</span></span>

## <a name="collectd-metrics-to-log-analytics-schema-conversion"></a><span data-ttu-id="42c41-133">A Naplóelemzési séma alakításához CollectD metrikák</span><span class="sxs-lookup"><span data-stu-id="42c41-133">CollectD metrics to Log Analytics schema conversion</span></span>
<span data-ttu-id="42c41-134">CollectD által gyűjtött egy ismerős modell infrastruktúra mérőszámokat már OMS-ügynök a Linux és az új mérőszámok között a következő séma-hozzárendeléséhez karbantartásához használja:</span><span class="sxs-lookup"><span data-stu-id="42c41-134">To maintain a familiar model between infrastructure metrics already collected by OMS Agent for Linux and the new metrics collected by CollectD the following schema mapping is used:</span></span>

| <span data-ttu-id="42c41-135">CollectD metrika mező</span><span class="sxs-lookup"><span data-stu-id="42c41-135">CollectD Metric field</span></span> | <span data-ttu-id="42c41-136">Log Analytics mező</span><span class="sxs-lookup"><span data-stu-id="42c41-136">Log Analytics field</span></span> |
|:--|:--|
| <span data-ttu-id="42c41-137">állomás</span><span class="sxs-lookup"><span data-stu-id="42c41-137">host</span></span> | <span data-ttu-id="42c41-138">Computer</span><span class="sxs-lookup"><span data-stu-id="42c41-138">Computer</span></span> |
| <span data-ttu-id="42c41-139">Beépülő modul</span><span class="sxs-lookup"><span data-stu-id="42c41-139">plugin</span></span> | <span data-ttu-id="42c41-140">None</span><span class="sxs-lookup"><span data-stu-id="42c41-140">None</span></span> |
| <span data-ttu-id="42c41-141">plugin_instance</span><span class="sxs-lookup"><span data-stu-id="42c41-141">plugin_instance</span></span> | <span data-ttu-id="42c41-142">Példány neve</span><span class="sxs-lookup"><span data-stu-id="42c41-142">Instance Name</span></span><br><span data-ttu-id="42c41-143">Ha **plugin_instance** van *null* majd példánynév = "*_Total*"</span><span class="sxs-lookup"><span data-stu-id="42c41-143">If **plugin_instance** is *null* then InstanceName="*_Total*"</span></span> |
| <span data-ttu-id="42c41-144">type</span><span class="sxs-lookup"><span data-stu-id="42c41-144">type</span></span> | <span data-ttu-id="42c41-145">ObjectName</span><span class="sxs-lookup"><span data-stu-id="42c41-145">ObjectName</span></span> |
| <span data-ttu-id="42c41-146">type_instance</span><span class="sxs-lookup"><span data-stu-id="42c41-146">type_instance</span></span> | <span data-ttu-id="42c41-147">CounterName</span><span class="sxs-lookup"><span data-stu-id="42c41-147">CounterName</span></span><br><span data-ttu-id="42c41-148">Ha **type_instance** van *null* majd CounterName =**üres**</span><span class="sxs-lookup"><span data-stu-id="42c41-148">If **type_instance** is *null* then CounterName=**blank**</span></span> |
| <span data-ttu-id="42c41-149">dsnames]</span><span class="sxs-lookup"><span data-stu-id="42c41-149">dsnames[]</span></span> | <span data-ttu-id="42c41-150">CounterName</span><span class="sxs-lookup"><span data-stu-id="42c41-150">CounterName</span></span> |
| <span data-ttu-id="42c41-151">dstypes</span><span class="sxs-lookup"><span data-stu-id="42c41-151">dstypes</span></span> | <span data-ttu-id="42c41-152">None</span><span class="sxs-lookup"><span data-stu-id="42c41-152">None</span></span> |
| <span data-ttu-id="42c41-153">[érték]</span><span class="sxs-lookup"><span data-stu-id="42c41-153">values[]</span></span> | <span data-ttu-id="42c41-154">Ellenértéknek</span><span class="sxs-lookup"><span data-stu-id="42c41-154">CounterValue</span></span> |

## <a name="next-steps"></a><span data-ttu-id="42c41-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42c41-155">Next steps</span></span>
* <span data-ttu-id="42c41-156">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) az adatforrások és a megoldások gyűjtött adatok elemzésére.</span><span class="sxs-lookup"><span data-stu-id="42c41-156">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 
* <span data-ttu-id="42c41-157">Használjon [egyéni mezők](log-analytics-custom-fields.md) syslog rekordokban levő adatok elemzése az egyes mezőkbe.</span><span class="sxs-lookup"><span data-stu-id="42c41-157">Use [Custom Fields](log-analytics-custom-fields.md) to parse data from syslog records into individual fields.</span></span>

