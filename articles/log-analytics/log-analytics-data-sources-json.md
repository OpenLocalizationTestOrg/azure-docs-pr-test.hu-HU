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
# <a name="collecting-custom-json-data-sources-with-hello-oms-agent-for-linux-in-log-analytics"></a>Egyéni JSON adatforrások hello OMS-ügynököt a Log Analyticshez Linux gyűjtése
Egyéni JSON-adatforrások az OMS-ügynököt hello használata Linux Log Analyticshez gyűjthetők össze.  Az egyéni adatforrások lehet egyszerű parancsfájlok JSON például visszaadó [curl](https://curl.haxx.se/) vagy az egyik [FluentD tartozó 300 + beépülő modulok](http://www.fluentd.org/plugins/all). Ez a cikk ismerteti a hello az eszköz konfigurálást igényel az adatgyűjtés.

> [!NOTE]
> Linux v1.1.0 OMS-ügynököt-217 + szükség egyéni JSON-adatok

## <a name="configuration"></a>Konfiguráció

### <a name="configure-input-plugin"></a>Bemeneti beépülő modul konfigurálása

a Naplóelemzési, toocollect JSON-adatok hozzáadása `oms.api.` toohello kezdetét egy bemeneti beépülő modul egy FluentD címkét.

Például az alábbiakban látható egy külön konfigurációs fájl `exec-json.conf` a `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Ez az használ hello FluentD beépülő modul `exec` toorun curl-parancsot 30 másodpercenként.  a parancs kimenete hello hello JSON kimeneti beépülő modul gyűjti.

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
hello konfigurációs fájl hozzáadása az `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` a tulajdonjoga módosítható a következő parancs hello toohave van szükség.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Kimeneti beépülő modul konfigurálása 
Adja hozzá a következő kimeneti beépülő modul konfigurációs toohello fő konfiguráció hello `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` vagy egy külön konfigurációs fájlban elhelyezni`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

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

### <a name="restart-oms-agent-for-linux"></a>Indítsa újra az OMS-ügynököt a Linux rendszerhez
Indítsa újra az OMS-ügynököt. hello hello a következő parancsot a Linux-szolgáltatás.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Kimenet
hello adatokat gyűjtenek a Naplóelemzési rekord típussal rendelkező `<FLUENTD_TAG>_CL`.

Például az egyéni címke hello `tag oms.api.tomcat` a típusú rekord Naplóelemzési `tomcat_CL`.  Az ilyen típusú összes rekord beolvasása a hello napló keresése a következő volt.

    Type=tomcat_CL

Beágyazott JSON-adatok források támogatottak, de az indexelt kijelentkezés mező alapján. Például egy Naplóelemzési keresést JSON-adatokat a következő hello küld vissza `tag_s : "[{ "a":"1", "b":"2" }]`.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat. 
 