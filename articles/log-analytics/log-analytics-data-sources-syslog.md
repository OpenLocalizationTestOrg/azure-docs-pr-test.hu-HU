---
title: "aaaCollect és elemezheti a Syslog-üzeneteket az OMS szolgáltatáshoz |} Microsoft Docs"
description: "A Syslog egy esemény naplózása protokoll, amely közös tooLinux. Ez a cikk ismerteti, hogyan hello rekordok részleteit és a Syslog-üzeneteket a Naplóelemzési tooconfigure gyűjtésére hoznak létre a hello OMS-tárházban."
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
ms.date: 07/12/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 8bfa0bca3f2f18287d1352c98bbaa2a70e41e276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="syslog-data-sources-in-log-analytics"></a>A Naplóelemzési Syslog adatforrások
A Syslog egy esemény naplózása protokoll, amely közös tooLinux.  Alkalmazások, amelyek a helyi számítógépen hello tárolt vagy tooa Syslog adatgyűjtő-i üzenetet szeretne küldeni.  Amikor a hello Linux OMS-ügynök telepítve van, konfigurálja a hello helyi Syslog démon tooforward üzenetek toohello ügynök.  hello ügynök ezután elküldi a hello üzenet tooLog Analytics ahol hello OMS-tárházban létrejön egy megfelelő bejegyzés.  

> [!NOTE]
> A Naplóelemzési támogatja az üzeneteket a rsyslog vagy a syslog-ng, ahol a rsyslog hello alapértelmezett démon az gyűjteménye. syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját. ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello [rsyslog démon](http://rsyslog.com) kell telepíteni és konfigurálni tooreplace sysklog.
>
>

![Syslog-gyűjtemény](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Syslog konfigurálása
hello Linux OMS-ügynököt csak hello létesítményekben és konfigurációjában megadott súlyosságokkal eseményeket gyűjt.  Syslog hello OMS-portálon keresztül vagy konfigurációs fájlok, a Linux-ügynökök kezelése konfigurálható.

### <a name="configure-syslog-in-hello-oms-portal"></a>Konfigurálhatja a rendszernaplót az hello OMS-portálon
Konfigurálhatja a rendszernaplót a hello [Naplóelemzés beállításai adatok menüben](log-analytics-data-sources.md#configuring-data-sources).  Ezt a konfigurációt a rendszer toohello konfigurációs fájl minden egyes Linux-ügynök.

Hozzáadhat egy új létesítményt a személyes, írja be a nevét, majd  **+** .  Minden egyes létesítmény csak a kiválasztott hello súlyosságokkal üzenetek gyűjtenek.  Ellenőrizze a megjeleníteni kívánt toocollect hello adott létesítmény hello fokozatok.  Nem adhat meg további feltételeket toofilter üzeneteket.

![Konfigurálhatja a rendszernaplót](media/log-analytics-data-sources-syslog/configure.png)

Alapértelmezés szerint az összes konfigurációs módosításhoz automatikusan vannak leküldött tooall ügynökök.  Ha azt szeretné, hogy minden egyes Linux-ügynök manuálisan a Syslog tooconfigure, majd törölje a jelet hello mezőben *alábbi konfigurációs toomy Linux számítógépek alkalmaz*.

### <a name="configure-syslog-on-linux-agent"></a>Konfigurálhatja a rendszernaplót a Linux-ügynök
Ha hello [OMS-ügynök telepítve van egy Linux-ügyfél](log-analytics-linux-agents.md), egy alapértelmezett syslog-konfigurációs fájl, amely meghatározza a hello létesítmény telepíti, és hello súlyossága-üzenetek begyűjtött.  Módosíthatja, hogy a fájl toochange hello konfigurációt.  hello konfigurációs fájl eltér attól függően, hogy hello Syslog démon, amely hello ügyfél telepítve van.

> [!NOTE]
> Hello Rendszernaplózás konfigurálásánál szerkesztése esetén újra kell indítani a syslog démon hello hello módosítások tootake hatás.
>
>

#### <a name="rsyslog"></a>Rsyslog
hello rsyslog tartozó konfigurációs fájl itt található: **/etc/rsyslog.d/95-omsagent.conf**.  Alapértelmezett tartalmának alább látható.  Ez gyűjti az összes létesítményekben figyelmeztetés vagy magasabb szintű hello helyi ügynök által küldött syslog-üzeneteket.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

A létesítmény eltávolíthatja a hello konfigurációs fájl részét eltávolításával.  Korlátozhatja az egy adott helyen a létesítmény bejegyzés módosításával összegyűjtött hello fokozatok.  Például toolimit hello felhasználói létesítmény toomessages egy hiba vagy magasabb fontossági ugyanúgy módosíthatja, hogy a következő hello konfigurációs fájl toohello üzletági:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng
hello konfigurációs syslog-ng fájlja a(z) **/etc/syslog-ng/syslog-ng.conf**.  Alapértelmezett tartalmának alább látható.  Ez gyűjti a syslog-üzeneteket hello helyi ügynök által küldött összes és az összes súlyossági szintet használja.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

A létesítmény eltávolíthatja a hello konfigurációs fájl részét eltávolításával.  Ehhez távolítsa el a listából egy adott helyen az összegyűjtött hello fokozatok korlátozhatja.  Például a toolimit hello felhasználói létesítmény toojust figyelmeztetési és a kritikus üzenetek, ugyanúgy módosíthatja a hello konfigurációs fájl toohello a következő szakaszt:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>További Syslog portokat adatainak begyűjtése
hello OMS-ügynököt a porton 25224 hello helyi ügyfélen Syslog-üzeneteket figyeli.  Ha hello ügynök telepítve van, egy alapértelmezett Rendszernaplózás konfigurálásánál alkalmazza, hello a következő helyen található:

* Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`

Hello portszám módosításához két konfigurációs fájlok létrehozása: egy FluentD konfigurációs fájlt és egy rsyslog-vagy-syslog-ng attól függően, hogy hello a Syslog démon telepítését.  

* hello FluentD konfigurációs fájlban található új fájlnak kell lennie: `/etc/opt/microsoft/omsagent/conf/omsagent.d` , és cserélje le a hello hello érték **port** az egyéni portszám rendelkező bejegyzés.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* Rsyslog, létre kell hoznia található új konfigurációs fájl: `/etc/rsyslog.d/` és hello érték % SYSLOG_PORT % cserélje le az egyéni portszámot.  

    > [!NOTE]
    > Ha ezt az értéket hello konfigurációs fájl módosítása `95-omsagent.conf`, ha hello ügynök érvényes alapértelmezett konfiguráció felülírja.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* hello syslog-ng config kell módosítani hello példa konfiguráció alább látható másolásával és hello egyéni módosított beállítások toohello hozzáadása hello syslog-ng.conf konfigurációs fájl található `/etc/syslog-ng/`.  Tegye **nem** hello alapértelmezett címke használata **% WORKSPACE_ID % _oms** vagy **% WORKSPACE_ID_OMS**, adja meg egy egyéni címke toohelp különböztetheti meg egymástól a módosításokat.  

    > [!NOTE]
    > Ha módosítja a hello konfigurációs fájl hello alapértelmezett értékeit, azok felülírja mikor hello ügynök alkalmazandó alapértelmezett konfiguráció.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Hello módosításairól, hello Syslog és hello befejezése után OMS ügynökszolgáltatását újraindítása toobe tooensure hello konfigurációs módosítások érvénybe.   

## <a name="syslog-record-properties"></a>Syslog rekord tulajdonságai
Syslog-rekordok típusa lehet **Syslog** és a következő táblázat hello hello jellemzőkkel rendelkezik.

| Tulajdonság | Leírás |
|:--- |:--- |
| Computer |A számítógép, amely esemény hello összegyűjtött. |
| Létesítmény |Határozza meg a létrehozott üdvözlőüzenetére hello rendszer hello része. |
| HostIP |IP-cím hello rendszer hello üzenet küldésekor. |
| Állomásnév |Hello üzenetet küld hello rendszer nevét. |
| Súlyossági szint |Súlyossági szint hello esemény. |
| SyslogMessage |Hello üzenet szövegét. |
| Folyamatazonosító |Generált üdvözlőüzenetére hello folyamat azonosítója. |
| eventTime |Dátum és idő esemény hello jött létre. |

## <a name="log-queries-with-syslog-records"></a>Syslog-rekordot tartalmazó napló lekérdezések
hello alábbi táblázat példákat különböző Syslog lehívása napló-lekérdezéseket.

| Lekérdezés | Leírás |
|:--- |:--- |
| Típus = Syslog |Minden rendszerbejegyzések. |
| Típus Syslog súlyossági szint = hiba = |Az összes Syslog rekordot hiba súlyosságát. |
| Típus = Syslog &#124; Számítógép által a mérték count() |Számítógép által bejegyzések száma a Syslog. |
| Típus = Syslog &#124; mérték count() létesítmény által |A Syslog száma rekordok létesítmény szerint. |

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.

> | Lekérdezés | Leírás |
|:--- |:--- |
| Rendszernapló |Minden rendszerbejegyzések. |
| Syslog &#124; adott súlyossági szint == "error" |Az összes Syslog rekordot hiba súlyosságát. |
| Syslog &#124; AggregatedValue összefoglalója = count() számítógépenként |Számítógép által bejegyzések száma a Syslog. |
| Syslog &#124; AggregatedValue összefoglalója létesítmény által count() = |A Syslog száma rekordok létesítmény szerint. |

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.
* Használjon [egyéni mezők](log-analytics-custom-fields.md) tooparse adatainak syslog rekordból egyes mezőkbe.
* [Linux-ügynökök konfigurálása](log-analytics-linux-agents.md) toocollect más típusú adatok.
