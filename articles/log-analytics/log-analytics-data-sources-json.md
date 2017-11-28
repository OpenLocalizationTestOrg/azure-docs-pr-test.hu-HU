---
title: "aaaCollecting egyéni JSON-adatokat az OMS szolgáltatáshoz |} Microsoft Docs"
description: "Egyéni JSON-adatforrások az OMS-ügynököt hello használata Linux Log Analyticshez gyűjthetők össze.  Az egyéni adatforrások egyszerű parancsfájlok JSON vissza például a curl vagy FluentD tartozó 300 + beépülő modulok egyike lehet. Ez a cikk ismerteti a hello az eszköz konfigurálást igényel az adatgyűjtés."
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
ms.openlocfilehash: 97d401408a8c206d4a9ef2ec9b13ba1ca6b5e92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collecting-custom-json-data-sources-with-hello-oms-agent-for-linux-in-log-analytics"></a><span data-ttu-id="a594e-105">Egyéni JSON adatforrások hello OMS-ügynököt a Log Analyticshez Linux gyűjtése</span><span class="sxs-lookup"><span data-stu-id="a594e-105">Collecting custom JSON data sources with hello OMS Agent for Linux in Log Analytics</span></span>
<span data-ttu-id="a594e-106">Egyéni JSON-adatforrások az OMS-ügynököt hello használata Linux Log Analyticshez gyűjthetők össze.</span><span class="sxs-lookup"><span data-stu-id="a594e-106">Custom JSON data sources can be collected into Log Analytics using hello OMS Agent for Linux.</span></span>  <span data-ttu-id="a594e-107">Az egyéni adatforrások lehet egyszerű parancsfájlok JSON például visszaadó [curl](https://curl.haxx.se/) vagy az egyik [FluentD tartozó 300 + beépülő modulok](http://www.fluentd.org/plugins/all).</span><span class="sxs-lookup"><span data-stu-id="a594e-107">These custom data sources can be simple scripts returning JSON such as [curl](https://curl.haxx.se/) or one of [FluentD's 300+ plugins](http://www.fluentd.org/plugins/all).</span></span> <span data-ttu-id="a594e-108">Ez a cikk ismerteti a hello az eszköz konfigurálást igényel az adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="a594e-108">This article describes hello configuration required for this data collection.</span></span>

> [!NOTE]
> <span data-ttu-id="a594e-109">Linux v1.1.0 OMS-ügynököt-217 + szükség egyéni JSON-adatok</span><span class="sxs-lookup"><span data-stu-id="a594e-109">OMS Agent for Linux v1.1.0-217+ is required for Custom JSON Data</span></span>

## <a name="configuration"></a><span data-ttu-id="a594e-110">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="a594e-110">Configuration</span></span>

### <a name="configure-input-plugin"></a><span data-ttu-id="a594e-111">Bemeneti beépülő modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a594e-111">Configure input plugin</span></span>

<span data-ttu-id="a594e-112">a Naplóelemzési, toocollect JSON-adatok hozzáadása `oms.api.` toohello kezdetét egy bemeneti beépülő modul egy FluentD címkét.</span><span class="sxs-lookup"><span data-stu-id="a594e-112">toocollect JSON data in Log Analytics, add `oms.api.` toohello start of a FluentD tag in an input plugin.</span></span>

<span data-ttu-id="a594e-113">Például az alábbiakban látható egy külön konfigurációs fájl `exec-json.conf` a `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span><span class="sxs-lookup"><span data-stu-id="a594e-113">For example, following is a separate configuration file `exec-json.conf` in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.</span></span>  <span data-ttu-id="a594e-114">Ez az használ hello FluentD beépülő modul `exec` toorun curl-parancsot 30 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="a594e-114">This uses hello FluentD plugin `exec` toorun a curl command every 30 seconds.</span></span>  <span data-ttu-id="a594e-115">a parancs kimenete hello hello JSON kimeneti beépülő modul gyűjti.</span><span class="sxs-lookup"><span data-stu-id="a594e-115">hello output from this command is collected by hello JSON output plugin.</span></span>

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
<span data-ttu-id="a594e-116">hello konfigurációs fájl hozzáadása az `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` a tulajdonjoga módosítható a következő parancs hello toohave van szükség.</span><span class="sxs-lookup"><span data-stu-id="a594e-116">hello configuration file added under `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` will require toohave its ownership changed with hello following command.</span></span>

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a><span data-ttu-id="a594e-117">Kimeneti beépülő modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a594e-117">Configure output plugin</span></span> 
<span data-ttu-id="a594e-118">Adja hozzá a következő kimeneti beépülő modul konfigurációs toohello fő konfiguráció hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` vagy egy külön konfigurációs fájlban elhelyezni`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span><span class="sxs-lookup"><span data-stu-id="a594e-118">Add hello following output plugin configuration toohello main configuration in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` or as a separate configuration file placed in `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`</span></span>

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-oms-agent-for-linux"></a><span data-ttu-id="a594e-119">Indítsa újra az OMS-ügynököt a Linux rendszerhez</span><span class="sxs-lookup"><span data-stu-id="a594e-119">Restart OMS Agent for Linux</span></span>
<span data-ttu-id="a594e-120">Indítsa újra az OMS-ügynököt. hello hello a következő parancsot a Linux-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a594e-120">Restart hello OMS Agent for Linux service with hello following command.</span></span>

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a><span data-ttu-id="a594e-121">Kimenet</span><span class="sxs-lookup"><span data-stu-id="a594e-121">Output</span></span>
<span data-ttu-id="a594e-122">hello adatokat gyűjtenek a Naplóelemzési rekord típussal rendelkező `<FLUENTD_TAG>_CL`.</span><span class="sxs-lookup"><span data-stu-id="a594e-122">hello data will be collected in Log Analytics with a record type of `<FLUENTD_TAG>_CL`.</span></span>

<span data-ttu-id="a594e-123">Például az egyéni címke hello `tag oms.api.tomcat` a típusú rekord Naplóelemzési `tomcat_CL`.</span><span class="sxs-lookup"><span data-stu-id="a594e-123">For example, hello custom tag `tag oms.api.tomcat` in Log Analytics with a record type of `tomcat_CL`.</span></span>  <span data-ttu-id="a594e-124">Az ilyen típusú összes rekord beolvasása a hello napló keresése a következő volt.</span><span class="sxs-lookup"><span data-stu-id="a594e-124">You could retrieve all records of this type with hello following log search.</span></span>

    Type=tomcat_CL

<span data-ttu-id="a594e-125">Beágyazott JSON-adatok források támogatottak, de az indexelt kijelentkezés mező alapján.</span><span class="sxs-lookup"><span data-stu-id="a594e-125">Nested JSON data sources are supported, but are indexed based off of parent field.</span></span> <span data-ttu-id="a594e-126">Például egy Naplóelemzési keresést JSON-adatokat a következő hello küld vissza `tag_s : "[{ "a":"1", "b":"2" }]`.</span><span class="sxs-lookup"><span data-stu-id="a594e-126">For example, hello following JSON data is returned from a Log Analytics search as `tag_s : "[{ "a":"1", "b":"2" }]`.</span></span>

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a><span data-ttu-id="a594e-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a594e-127">Next steps</span></span>
* <span data-ttu-id="a594e-128">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="a594e-128">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span> 
 