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
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Nagios és az OMS-ügynököt a Naplóelemzési Zabbix riasztások gyűjtése Linux 
[Nagios](https://www.nagios.org/) és [Zabbix](http://www.zabbix.com/) nyílt forráskódú figyelési eszközök.  Képes összegyűjteni riasztást ezeket az eszközöket a Log Analyticshez való rendelés tooanalyze a azokat, valamint [más forrásokból származó riasztások](log-analytics-alerts.md).  Ez a cikk ismerteti, hogyan tooconfigure hello OMS-ügynököt a Linux toocollect riasztást küld, ezen rendszerekből.
 
## <a name="configure-alert-collection"></a>Riasztások gyűjtésének konfigurálása

### <a name="configuring-nagios-alert-collection"></a>Nagios riasztások gyűjtésének konfigurálása
Hajtsa végre a lépéseket követve hello Nagios server toocollect riasztások hello.

1. Támogatás hello felhasználói **omsagent** olvasási hozzáférés toohello Nagios naplófájl (azaz `/var/log/nagios/nagios.log`). Feltéve, hogy hello nagios.log fájl hello csoport tulajdonosa `nagios`, hozzáadhat hello felhasználót **omsagent** toohello **nagios** csoport. 

    sudo felhasználó módosítási - a -G nagios omsagent

2.  Hello konfigurációs fájlban a következő módosítása (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Győződjön meg arról, bejegyzéseket a következő hello jelen, és nem megjegyzésként ki:  

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

3. Indítsa újra a hello omsagent démon

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Zabbix riasztások gyűjtésének konfigurálása
toocollect riasztások Zabbix-kiszolgálótól, kell toospecify egy felhasználó és a jelszó *törölje a szöveget*. Ez nem ideális, de javasoljuk, hogy hello felhasználó létrehozása, és engedélyek toomonitor onlu biztosítani.

Hajtsa végre a lépéseket követve hello Nagios server toocollect riasztások hello.

1. Hello konfigurációs fájlban a következő módosítása (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Győződjön meg arról, bejegyzéseket a következő hello jelen, és nem megjegyzésként ki.  Hello felhasználói nevet és jelszót a Zabbix környezetnek toovalues módosítása.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Indítsa újra a hello omsagent démon

    sudo megosztása /opt/microsoft/omsagent/bin/service_control újraindítása


## <a name="alert-records"></a>Riasztási rekordok
Riasztási rekordok beolvasható Nagios és Zabbix használatával [keresések jelentkezzen](log-analytics-log-searches.md) a Naplóelemzési.

### <a name="nagios-alert-records"></a>Nagios riasztás rekordok

Riasztási Nagios által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Nagios**.  A következő táblázat hello hello tulajdonságok rendelkeznek.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*Riasztás* |
| SourceSystem |*Nagios* |
| AlertName |Hello riasztás nevét. |
| AlertDescription | Hello riasztás leírása. |
| AlertState | Hello szolgáltatást vagy a gazdagép állapota.<br><br>OKÉ<br>FIGYELMEZTETÉS<br>MENTÉSE<br>LEFELÉ |
| Állomásnév | Hello riasztást létrehozó hello állomás neve. |
| PriorityNumber | Hello riasztás prioritását. |
| StateType | hello típusa hello riasztás állapota.<br><br>SOFT - hiba, amely nem ellenőrizni.<br>MEREVLEMEZ - probléma, amelyet újra ellenőrizni kell a megadott számú alkalommal.  |
| TimeGenerated |Dátum és idő hello riasztás létrejött. |


### <a name="zabbix-alert-records"></a>Zabbix riasztási rekordok
Riasztási Zabbix által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Zabbix**.  A következő táblázat hello hello tulajdonságok rendelkeznek.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*Riasztás* |
| SourceSystem |*Zabbix* |
| AlertName | Hello riasztás nevét. |
| AlertPriority | Hello riasztás súlyossága.<br><br>nem minősített<br>Információ<br>Figyelmeztetés<br>Átlagos<br>Magas<br>vészhelyreállítás  |
| AlertState | Hello riasztás állapota.<br><br>0 - állapot toodate működik.<br>1 - állapota ismeretlen.  |
| AlertTypeNumber | Meghatározza, hogy riasztást hozhat létre a probléma több esemény.<br><br>0 - állapot toodate működik.<br>1 - állapota ismeretlen.    |
| Megjegyzések | A riasztásra vonatkozóan további megjegyzéseket. |
| Állomásnév | Hello riasztást létrehozó hello állomás neve. |
| PriorityNumber | Hello riasztás súlyossága jelző érték.<br><br>0 – nem besorolt<br>1 - információk<br>2 – figyelmeztetés<br>3 – átlag<br>4 - magas<br>5 - vészhelyreállítás |
| TimeGenerated |Dátum és idő hello riasztás létrejött. |
| TimeLastModified |Utolsó módosításának dátuma és időpontja hello riasztás hello állapota. |


## <a name="next-steps"></a>Következő lépések
* További tudnivalók [riasztások](log-analytics-alerts.md) a Naplóelemzési.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat. 
