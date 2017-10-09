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
# <a name="collect-data-from-collectd-on-linux-agents-in-log-analytics"></a>Gyűjti az adatokat a CollectD Naplóelemzési Linux-ügynökök
[CollectD](https://collectd.org/) van egy nyílt forráskódú Linux-démon rendszeresen teljesítménymutatók az összegyűjtő alkalmazások és a rendszer a szintre vonatkozó információ. Példa alkalmazások hello Java virtuális gép (JVM), a MySQL-kiszolgáló és a Nginx tartalmazza. Ez a cikk tájékoztatást nyújt teljesítményadatok összegyűjtése a Naplóelemzési CollectD.

Elérhető beépülő modulok teljes listája megtalálható [beépülő modulok tábla](https://collectd.org/wiki/index.php/Table_of_Plugins).

![CollectD áttekintése](media/log-analytics-data-sources-collectd/overview.png)

hello következő CollectD konfiguráció része a Linux tooroute CollectD adatok toohello OMS-ügynököt az OMS-ügynököt hello Linux.

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Emellett ha egy collectD verzióját használja, ehelyett a következő konfigurációs hello 5.5 használatához.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

hello CollectD konfiguráció használja hello alapértelmezett`write_http` beépülő modul toosend metrika teljesítményadatokat keresztül port 26000 tooOMS Linux-ügynök. 

> [!NOTE]
> Ezt a portot lehet egyénileg definiált konfigurált tooa port, ha szükséges.

hello Linux OMS-ügynököt is porton figyel 26000 CollectD metrikáihoz, és adja őket az tooOMS séma metrikák alakítja. hello az alábbiakban az OMS-ügynököt hello Linux konfiguráció `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Támogatott verziók
- Naplóelemzési jelenleg CollectD 4.8 verzió vagy újabb verzió.
- A Linux v1.1.0-217 vagy újabb OMS-ügynököt CollectD metrika gyűjtemény szükség.


## <a name="configuration"></a>Konfiguráció
Az alábbiakban hello CollectD adatok Naplóelemzési a lépéseken tooconfigure gyűjteménye.

1. CollectD toosend adatok toohello OMS-ügynököt konfigurálása Linux hello write_http beépülő modul használatával.  
2. A Linux toolisten hello CollectD adatok az OMS-ügynököt hello konfigurálása hello megfelelő portot.
3. Indítsa újra a CollectD és Linux OMS-ügynököt.

### <a name="configure-collectd-tooforward-data"></a>CollectD tooforward adatok konfigurálása 

1. tooroute CollectD adatok toohello Linux, OMS-ügynököt `oms.conf` igényeinek toobe hozzáadott tooCollectD tartozó konfigurációs könyvtára. a fájl hello cél hello Linux distro a gép függ.

    Ha a CollectD konfigurációs könyvtár /etc/collectd.d/ találhatók:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Ha a CollectD konfigurációs könyvtár /etc/collectd/collectd.conf.d/ találhatók:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >5.5 előtt CollectD verzióihoz fog toomodify hello címkék `oms.conf` a fentiek szerint.
    >

2. Másolja a collectd.conf szükséges toohello munkaterület omsagent konfigurációs könyvtára.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Indítsa újra a CollectD és az OMS-ügynököt Linux a következő parancsok hello.

    sudo szolgáltatás collectd sudo /opt/microsoft/omsagent/bin/service_control újraindítás

## <a name="collectd-metrics-toolog-analytics-schema-conversion"></a>CollectD metrikák tooLog Analytics séma átalakítás
toomaintain séma-hozzárendeléséhez a következő hello használt CollectD által gyűjtött egy ismerős modell infrastruktúra mérőszámokat már OMS-ügynök a Linux és hello új mérőszámok között:

| CollectD metrika mező | Log Analytics mező |
|:--|:--|
| állomás | Computer |
| Beépülő modul | None |
| plugin_instance | Példány neve<br>Ha **plugin_instance** van *null* majd példánynév = "*_Total*" |
| type | ObjectName |
| type_instance | CounterName<br>Ha **type_instance** van *null* majd CounterName =**üres** |
| dsnames] | CounterName |
| dstypes | None |
| [érték] | Ellenértéknek |

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat. 
* Használjon [egyéni mezők](log-analytics-custom-fields.md) tooparse adatainak syslog rekordból egyes mezőkbe.

