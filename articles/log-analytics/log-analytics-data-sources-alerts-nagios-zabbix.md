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
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-oms-agent-for-linux"></a>Nagios és az OMS-ügynököt a Naplóelemzési Zabbix riasztások gyűjtése Linux 
[Nagios](https://www.nagios.org/) és [Zabbix](http://www.zabbix.com/) nyílt forráskódú figyelési eszközök.  Begyűjtheti riasztások ezeket az eszközöket a Log Analyticshez való elemezheti őket, valamint hogy [más forrásokból származó riasztások](log-analytics-alerts.md).  Ez a cikk ismerteti a riasztások gyűjteni ezek a rendszerek Linux OMS-ügynök konfigurálása.
 
## <a name="configure-alert-collection"></a>Riasztások gyűjtésének konfigurálása

### <a name="configuring-nagios-alert-collection"></a>Nagios riasztások gyűjtésének konfigurálása
A következő lépésekkel a Nagios kiszolgálón riasztások gyűjtése.

1. Adja meg a felhasználónak **omsagent** olvasási hozzáférés a Nagios naplófájl (azaz `/var/log/nagios/nagios.log`). A csoport tulajdonosa feltéve, hogy a nagios.log fájl `nagios`, a felhasználó hozzáadhat **omsagent** való a **nagios** csoport. 

    sudo felhasználó módosítási - a -G nagios omsagent

2.  Módosítsa a konfigurációs fájlban a következő (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Győződjön meg arról, a következő tételek jelen, és nem megjegyzésként ki:  

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

3. Indítsa újra a omsagent démon

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Zabbix riasztások gyűjtésének konfigurálása
A riasztások gyűjtése Zabbix-kiszolgálótól, meg kell adnia egy felhasználói és a jelszó *törölje a szöveget*. Ez nem ideális, de javasoljuk, hogy a felhasználó létrehozása, és engedélyezze, hogy onlu figyelése.

A következő lépésekkel a Nagios kiszolgálón riasztások gyűjtése.

1. Módosítsa a konfigurációs fájlban a következő (`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`). Győződjön meg arról, a következő tételek jelen, és nem megjegyzésként ki.  A felhasználónév és jelszó átállítása értékek környezetnek Zabbix.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Indítsa újra a omsagent démon

    sudo megosztása /opt/microsoft/omsagent/bin/service_control újraindítása


## <a name="alert-records"></a>Riasztási rekordok
Riasztási rekordok beolvasható Nagios és Zabbix használatával [keresések jelentkezzen](log-analytics-log-searches.md) a Naplóelemzési.

### <a name="nagios-alert-records"></a>Nagios riasztás rekordok

Riasztási Nagios által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Nagios**.  A Tulajdonságok rendelkeznek, az alábbi táblázatban.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*Riasztás* |
| SourceSystem |*Nagios* |
| AlertName |A riasztás nevét. |
| AlertDescription | A riasztás leírása. |
| AlertState | A szolgáltatás vagy a gazdagép állapota.<br><br>OKÉ<br>FIGYELMEZTETÉS<br>MENTÉSE<br>LEFELÉ |
| Állomásnév | A gazdagép által létrehozott, a riasztás nevét. |
| PriorityNumber | A riasztás prioritását. |
| StateType | A riasztás állapota típusa.<br><br>SOFT - hiba, amely nem ellenőrizni.<br>MEREVLEMEZ - probléma, amelyet újra ellenőrizni kell a megadott számú alkalommal.  |
| TimeGenerated |A riasztás létrehozásának dátuma és időpontja. |


### <a name="zabbix-alert-records"></a>Zabbix riasztási rekordok
Riasztási Zabbix által gyűjtött rekordok rendelkezik egy **típus** a **riasztási** és egy **SourceSystem** a **Zabbix**.  A Tulajdonságok rendelkeznek, az alábbi táblázatban.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*Riasztás* |
| SourceSystem |*Zabbix* |
| AlertName | A riasztás nevét. |
| AlertPriority | A riasztás súlyossága.<br><br>nem minősített<br>Információ<br>Figyelmeztetés<br>Átlagos<br>Magas<br>vészhelyreállítás  |
| AlertState | A riasztás állapota.<br><br>0 - állapota naprakészek legyenek.<br>1 - állapota ismeretlen.  |
| AlertTypeNumber | Meghatározza, hogy riasztást hozhat létre a probléma több esemény.<br><br>0 - állapota naprakészek legyenek.<br>1 - állapota ismeretlen.    |
| Megjegyzések | A riasztásra vonatkozóan további megjegyzéseket. |
| Állomásnév | A gazdagép által létrehozott, a riasztás nevét. |
| PriorityNumber | A riasztás súlyossága jelző érték.<br><br>0 – nem besorolt<br>1 - információk<br>2 – figyelmeztetés<br>3 – átlag<br>4 - magas<br>5 - vészhelyreállítás |
| TimeGenerated |A riasztás létrehozásának dátuma és időpontja. |
| TimeLastModified |Dátum és idő, a riasztás állapota módosította utoljára. |


## <a name="next-steps"></a>Következő lépések
* További tudnivalók [riasztások](log-analytics-alerts.md) a Naplóelemzési.
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) az adatforrások és a megoldások gyűjtött adatok elemzésére. 
