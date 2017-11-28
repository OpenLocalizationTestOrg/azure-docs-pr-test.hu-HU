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
# <a name="syslog-data-sources-in-log-analytics"></a><span data-ttu-id="e463d-104">A Naplóelemzési Syslog adatforrások</span><span class="sxs-lookup"><span data-stu-id="e463d-104">Syslog data sources in Log Analytics</span></span>
<span data-ttu-id="e463d-105">A Syslog egy esemény naplózása protokoll, amely közös tooLinux.</span><span class="sxs-lookup"><span data-stu-id="e463d-105">Syslog is an event logging protocol that is common tooLinux.</span></span>  <span data-ttu-id="e463d-106">Alkalmazások, amelyek a helyi számítógépen hello tárolt vagy tooa Syslog adatgyűjtő-i üzenetet szeretne küldeni.</span><span class="sxs-lookup"><span data-stu-id="e463d-106">Applications will send messages that may be stored on hello local machine or delivered tooa Syslog collector.</span></span>  <span data-ttu-id="e463d-107">Amikor a hello Linux OMS-ügynök telepítve van, konfigurálja a hello helyi Syslog démon tooforward üzenetek toohello ügynök.</span><span class="sxs-lookup"><span data-stu-id="e463d-107">When hello OMS Agent for Linux is installed, it configures hello local Syslog daemon tooforward messages toohello agent.</span></span>  <span data-ttu-id="e463d-108">hello ügynök ezután elküldi a hello üzenet tooLog Analytics ahol hello OMS-tárházban létrejön egy megfelelő bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="e463d-108">hello agent then sends hello message tooLog Analytics where a corresponding record is created in hello OMS repository.</span></span>  

> [!NOTE]
> <span data-ttu-id="e463d-109">A Naplóelemzési támogatja az üzeneteket a rsyslog vagy a syslog-ng, ahol a rsyslog hello alapértelmezett démon az gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="e463d-109">Log Analytics supports collection of messages sent by rsyslog or syslog-ng, where rsyslog is hello default daemon.</span></span> <span data-ttu-id="e463d-110">syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="e463d-110">hello default syslog daemon on version 5 of Red Hat Enterprise Linux, CentOS, and Oracle Linux version (sysklog) is not supported for syslog event collection.</span></span> <span data-ttu-id="e463d-111">ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello [rsyslog démon](http://rsyslog.com) kell telepíteni és konfigurálni tooreplace sysklog.</span><span class="sxs-lookup"><span data-stu-id="e463d-111">toocollect syslog data from this version of these distributions, hello [rsyslog daemon](http://rsyslog.com) should be installed and configured tooreplace sysklog.</span></span>
>
>

![Syslog-gyűjtemény](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a><span data-ttu-id="e463d-113">Syslog konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e463d-113">Configuring Syslog</span></span>
<span data-ttu-id="e463d-114">hello Linux OMS-ügynököt csak hello létesítményekben és konfigurációjában megadott súlyosságokkal eseményeket gyűjt.</span><span class="sxs-lookup"><span data-stu-id="e463d-114">hello OMS Agent for Linux will only collect events with hello facilities and severities that are specified in its configuration.</span></span>  <span data-ttu-id="e463d-115">Syslog hello OMS-portálon keresztül vagy konfigurációs fájlok, a Linux-ügynökök kezelése konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="e463d-115">You can configure Syslog through hello OMS portal or by managing configuration files on your Linux agents.</span></span>

### <a name="configure-syslog-in-hello-oms-portal"></a><span data-ttu-id="e463d-116">Konfigurálhatja a rendszernaplót az hello OMS-portálon</span><span class="sxs-lookup"><span data-stu-id="e463d-116">Configure Syslog in hello OMS portal</span></span>
<span data-ttu-id="e463d-117">Konfigurálhatja a rendszernaplót a hello [Naplóelemzés beállításai adatok menüben](log-analytics-data-sources.md#configuring-data-sources).</span><span class="sxs-lookup"><span data-stu-id="e463d-117">Configure Syslog from hello [Data menu in Log Analytics Settings](log-analytics-data-sources.md#configuring-data-sources).</span></span>  <span data-ttu-id="e463d-118">Ezt a konfigurációt a rendszer toohello konfigurációs fájl minden egyes Linux-ügynök.</span><span class="sxs-lookup"><span data-stu-id="e463d-118">This configuration is delivered toohello configuration file on each Linux agent.</span></span>

<span data-ttu-id="e463d-119">Hozzáadhat egy új létesítményt a személyes, írja be a nevét, majd  **+** .</span><span class="sxs-lookup"><span data-stu-id="e463d-119">You can add a new facility by typing in its name and clicking **+**.</span></span>  <span data-ttu-id="e463d-120">Minden egyes létesítmény csak a kiválasztott hello súlyosságokkal üzenetek gyűjtenek.</span><span class="sxs-lookup"><span data-stu-id="e463d-120">For each facility, only messages with hello selected severities will be collected.</span></span>  <span data-ttu-id="e463d-121">Ellenőrizze a megjeleníteni kívánt toocollect hello adott létesítmény hello fokozatok.</span><span class="sxs-lookup"><span data-stu-id="e463d-121">Check hello severities for hello particular facility that you want toocollect.</span></span>  <span data-ttu-id="e463d-122">Nem adhat meg további feltételeket toofilter üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e463d-122">You cannot provide any additional criteria toofilter messages.</span></span>

![Konfigurálhatja a rendszernaplót](media/log-analytics-data-sources-syslog/configure.png)

<span data-ttu-id="e463d-124">Alapértelmezés szerint az összes konfigurációs módosításhoz automatikusan vannak leküldött tooall ügynökök.</span><span class="sxs-lookup"><span data-stu-id="e463d-124">By default, all configuration changes are automatically pushed tooall agents.</span></span>  <span data-ttu-id="e463d-125">Ha azt szeretné, hogy minden egyes Linux-ügynök manuálisan a Syslog tooconfigure, majd törölje a jelet hello mezőben *alábbi konfigurációs toomy Linux számítógépek alkalmaz*.</span><span class="sxs-lookup"><span data-stu-id="e463d-125">If you want tooconfigure Syslog manually on each Linux agent, then uncheck hello box *Apply below configuration toomy Linux machines*.</span></span>

### <a name="configure-syslog-on-linux-agent"></a><span data-ttu-id="e463d-126">Konfigurálhatja a rendszernaplót a Linux-ügynök</span><span class="sxs-lookup"><span data-stu-id="e463d-126">Configure Syslog on Linux agent</span></span>
<span data-ttu-id="e463d-127">Ha hello [OMS-ügynök telepítve van egy Linux-ügyfél](log-analytics-linux-agents.md), egy alapértelmezett syslog-konfigurációs fájl, amely meghatározza a hello létesítmény telepíti, és hello súlyossága-üzenetek begyűjtött.</span><span class="sxs-lookup"><span data-stu-id="e463d-127">When hello [OMS agent is installed on a Linux client](log-analytics-linux-agents.md), it installs a default syslog configuration file that defines hello facility and severity of hello messages that are collected.</span></span>  <span data-ttu-id="e463d-128">Módosíthatja, hogy a fájl toochange hello konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="e463d-128">You can modify this file toochange hello configuration.</span></span>  <span data-ttu-id="e463d-129">hello konfigurációs fájl eltér attól függően, hogy hello Syslog démon, amely hello ügyfél telepítve van.</span><span class="sxs-lookup"><span data-stu-id="e463d-129">hello configuration file is different depending on hello Syslog daemon that hello client has installed.</span></span>

> [!NOTE]
> <span data-ttu-id="e463d-130">Hello Rendszernaplózás konfigurálásánál szerkesztése esetén újra kell indítani a syslog démon hello hello módosítások tootake hatás.</span><span class="sxs-lookup"><span data-stu-id="e463d-130">If you edit hello syslog configuration, you must restart hello syslog daemon for hello changes tootake effect.</span></span>
>
>

#### <a name="rsyslog"></a><span data-ttu-id="e463d-131">Rsyslog</span><span class="sxs-lookup"><span data-stu-id="e463d-131">rsyslog</span></span>
<span data-ttu-id="e463d-132">hello rsyslog tartozó konfigurációs fájl itt található: **/etc/rsyslog.d/95-omsagent.conf**.</span><span class="sxs-lookup"><span data-stu-id="e463d-132">hello configuration file for rsyslog is located at **/etc/rsyslog.d/95-omsagent.conf**.</span></span>  <span data-ttu-id="e463d-133">Alapértelmezett tartalmának alább látható.</span><span class="sxs-lookup"><span data-stu-id="e463d-133">Its default contents are shown below.</span></span>  <span data-ttu-id="e463d-134">Ez gyűjti az összes létesítményekben figyelmeztetés vagy magasabb szintű hello helyi ügynök által küldött syslog-üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e463d-134">This collects syslog messages sent from hello local agent for all facilities with a level of warning or higher.</span></span>

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

<span data-ttu-id="e463d-135">A létesítmény eltávolíthatja a hello konfigurációs fájl részét eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="e463d-135">You can remove a facility by removing its section of hello configuration file.</span></span>  <span data-ttu-id="e463d-136">Korlátozhatja az egy adott helyen a létesítmény bejegyzés módosításával összegyűjtött hello fokozatok.</span><span class="sxs-lookup"><span data-stu-id="e463d-136">You can limit hello severities that are collected for a particular facility by modifying that facility's entry.</span></span>  <span data-ttu-id="e463d-137">Például toolimit hello felhasználói létesítmény toomessages egy hiba vagy magasabb fontossági ugyanúgy módosíthatja, hogy a következő hello konfigurációs fájl toohello üzletági:</span><span class="sxs-lookup"><span data-stu-id="e463d-137">For example, toolimit hello user facility toomessages with a severity of error or higher you would modify that line of hello configuration file toohello following:</span></span>

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a><span data-ttu-id="e463d-138">Syslog-ng</span><span class="sxs-lookup"><span data-stu-id="e463d-138">syslog-ng</span></span>
<span data-ttu-id="e463d-139">hello konfigurációs syslog-ng fájlja a(z) **/etc/syslog-ng/syslog-ng.conf**.</span><span class="sxs-lookup"><span data-stu-id="e463d-139">hello configuration file for syslog-ng is location at **/etc/syslog-ng/syslog-ng.conf**.</span></span>  <span data-ttu-id="e463d-140">Alapértelmezett tartalmának alább látható.</span><span class="sxs-lookup"><span data-stu-id="e463d-140">Its default contents are shown below.</span></span>  <span data-ttu-id="e463d-141">Ez gyűjti a syslog-üzeneteket hello helyi ügynök által küldött összes és az összes súlyossági szintet használja.</span><span class="sxs-lookup"><span data-stu-id="e463d-141">This collects syslog messages sent from hello local agent for all facilities and all severities.</span></span>   

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

<span data-ttu-id="e463d-142">A létesítmény eltávolíthatja a hello konfigurációs fájl részét eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="e463d-142">You can remove a facility by removing its section of hello configuration file.</span></span>  <span data-ttu-id="e463d-143">Ehhez távolítsa el a listából egy adott helyen az összegyűjtött hello fokozatok korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="e463d-143">You can limit hello severities that are collected for a particular facility by removing them from its list.</span></span>  <span data-ttu-id="e463d-144">Például a toolimit hello felhasználói létesítmény toojust figyelmeztetési és a kritikus üzenetek, ugyanúgy módosíthatja a hello konfigurációs fájl toohello a következő szakaszt:</span><span class="sxs-lookup"><span data-stu-id="e463d-144">For example, toolimit hello user facility toojust alert and critical messages, you would modify that section of hello configuration file toohello following:</span></span>

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a><span data-ttu-id="e463d-145">További Syslog portokat adatainak begyűjtése</span><span class="sxs-lookup"><span data-stu-id="e463d-145">Collecting data from additional Syslog ports</span></span>
<span data-ttu-id="e463d-146">hello OMS-ügynököt a porton 25224 hello helyi ügyfélen Syslog-üzeneteket figyeli.</span><span class="sxs-lookup"><span data-stu-id="e463d-146">hello OMS agent listens for Syslog messages on hello local client on port 25224.</span></span>  <span data-ttu-id="e463d-147">Ha hello ügynök telepítve van, egy alapértelmezett Rendszernaplózás konfigurálásánál alkalmazza, hello a következő helyen található:</span><span class="sxs-lookup"><span data-stu-id="e463d-147">When hello agent is installed, a default syslog configuration is applied and found in hello following location:</span></span>

* <span data-ttu-id="e463d-148">Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`</span><span class="sxs-lookup"><span data-stu-id="e463d-148">Rsyslog: `/etc/rsyslog.d/95-omsagent.conf`</span></span>
* <span data-ttu-id="e463d-149">Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`</span><span class="sxs-lookup"><span data-stu-id="e463d-149">Syslog-ng: `/etc/syslog-ng/syslog-ng.conf`</span></span>

<span data-ttu-id="e463d-150">Hello portszám módosításához két konfigurációs fájlok létrehozása: egy FluentD konfigurációs fájlt és egy rsyslog-vagy-syslog-ng attól függően, hogy hello a Syslog démon telepítését.</span><span class="sxs-lookup"><span data-stu-id="e463d-150">You can change hello port number by creating two configuration files: a FluentD config file and a rsyslog-or-syslog-ng file depending on hello Syslog daemon you have installed.</span></span>  

* <span data-ttu-id="e463d-151">hello FluentD konfigurációs fájlban található új fájlnak kell lennie: `/etc/opt/microsoft/omsagent/conf/omsagent.d` , és cserélje le a hello hello érték **port** az egyéni portszám rendelkező bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="e463d-151">hello FluentD config file should be a new file located in: `/etc/opt/microsoft/omsagent/conf/omsagent.d` and replace hello value in hello **port** entry with your custom port number.</span></span>

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

* <span data-ttu-id="e463d-152">Rsyslog, létre kell hoznia található új konfigurációs fájl: `/etc/rsyslog.d/` és hello érték % SYSLOG_PORT % cserélje le az egyéni portszámot.</span><span class="sxs-lookup"><span data-stu-id="e463d-152">For rsyslog, you should create a new configuration file located in: `/etc/rsyslog.d/` and replace hello value %SYSLOG_PORT% with your custom port number.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="e463d-153">Ha ezt az értéket hello konfigurációs fájl módosítása `95-omsagent.conf`, ha hello ügynök érvényes alapértelmezett konfiguráció felülírja.</span><span class="sxs-lookup"><span data-stu-id="e463d-153">If you modify this value in hello configuration file `95-omsagent.conf`, it will be overwritten when hello agent applies a default configuration.</span></span>
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* <span data-ttu-id="e463d-154">hello syslog-ng config kell módosítani hello példa konfiguráció alább látható másolásával és hello egyéni módosított beállítások toohello hozzáadása hello syslog-ng.conf konfigurációs fájl található `/etc/syslog-ng/`.</span><span class="sxs-lookup"><span data-stu-id="e463d-154">hello syslog-ng config should be modified by copying hello example configuration shown below and adding hello custom modified settings toohello end of hello syslog-ng.conf configuration file located in `/etc/syslog-ng/`.</span></span>  <span data-ttu-id="e463d-155">Tegye **nem** hello alapértelmezett címke használata **% WORKSPACE_ID % _oms** vagy **% WORKSPACE_ID_OMS**, adja meg egy egyéni címke toohelp különböztetheti meg egymástól a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="e463d-155">Do **not** use hello default label **%WORKSPACE_ID%_oms** or **%WORKSPACE_ID_OMS**, define a custom label toohelp distinguish your changes.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="e463d-156">Ha módosítja a hello konfigurációs fájl hello alapértelmezett értékeit, azok felülírja mikor hello ügynök alkalmazandó alapértelmezett konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e463d-156">If you modify hello default values in hello configuration file, they will be overwritten when hello agent applies a default configuration.</span></span>
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

<span data-ttu-id="e463d-157">Hello módosításairól, hello Syslog és hello befejezése után OMS ügynökszolgáltatását újraindítása toobe tooensure hello konfigurációs módosítások érvénybe.</span><span class="sxs-lookup"><span data-stu-id="e463d-157">After completing hello changes, hello Syslog and hello OMS agent service needs toobe restarted tooensure hello configuration changes take effect.</span></span>   

## <a name="syslog-record-properties"></a><span data-ttu-id="e463d-158">Syslog rekord tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e463d-158">Syslog record properties</span></span>
<span data-ttu-id="e463d-159">Syslog-rekordok típusa lehet **Syslog** és a következő táblázat hello hello jellemzőkkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e463d-159">Syslog records have a type of **Syslog** and have hello properties in hello following table.</span></span>

| <span data-ttu-id="e463d-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e463d-160">Property</span></span> | <span data-ttu-id="e463d-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="e463d-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e463d-162">Computer</span><span class="sxs-lookup"><span data-stu-id="e463d-162">Computer</span></span> |<span data-ttu-id="e463d-163">A számítógép, amely esemény hello összegyűjtött.</span><span class="sxs-lookup"><span data-stu-id="e463d-163">Computer that hello event was collected from.</span></span> |
| <span data-ttu-id="e463d-164">Létesítmény</span><span class="sxs-lookup"><span data-stu-id="e463d-164">Facility</span></span> |<span data-ttu-id="e463d-165">Határozza meg a létrehozott üdvözlőüzenetére hello rendszer hello része.</span><span class="sxs-lookup"><span data-stu-id="e463d-165">Defines hello part of hello system that generated hello message.</span></span> |
| <span data-ttu-id="e463d-166">HostIP</span><span class="sxs-lookup"><span data-stu-id="e463d-166">HostIP</span></span> |<span data-ttu-id="e463d-167">IP-cím hello rendszer hello üzenet küldésekor.</span><span class="sxs-lookup"><span data-stu-id="e463d-167">IP address of hello system sending hello message.</span></span> |
| <span data-ttu-id="e463d-168">Állomásnév</span><span class="sxs-lookup"><span data-stu-id="e463d-168">HostName</span></span> |<span data-ttu-id="e463d-169">Hello üzenetet küld hello rendszer nevét.</span><span class="sxs-lookup"><span data-stu-id="e463d-169">Name of hello system sending hello message.</span></span> |
| <span data-ttu-id="e463d-170">Súlyossági szint</span><span class="sxs-lookup"><span data-stu-id="e463d-170">SeverityLevel</span></span> |<span data-ttu-id="e463d-171">Súlyossági szint hello esemény.</span><span class="sxs-lookup"><span data-stu-id="e463d-171">Severity level of hello event.</span></span> |
| <span data-ttu-id="e463d-172">SyslogMessage</span><span class="sxs-lookup"><span data-stu-id="e463d-172">SyslogMessage</span></span> |<span data-ttu-id="e463d-173">Hello üzenet szövegét.</span><span class="sxs-lookup"><span data-stu-id="e463d-173">Text of hello message.</span></span> |
| <span data-ttu-id="e463d-174">Folyamatazonosító</span><span class="sxs-lookup"><span data-stu-id="e463d-174">ProcessID</span></span> |<span data-ttu-id="e463d-175">Generált üdvözlőüzenetére hello folyamat azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e463d-175">ID of hello process that generated hello message.</span></span> |
| <span data-ttu-id="e463d-176">eventTime</span><span class="sxs-lookup"><span data-stu-id="e463d-176">EventTime</span></span> |<span data-ttu-id="e463d-177">Dátum és idő esemény hello jött létre.</span><span class="sxs-lookup"><span data-stu-id="e463d-177">Date and time that hello event was generated.</span></span> |

## <a name="log-queries-with-syslog-records"></a><span data-ttu-id="e463d-178">Syslog-rekordot tartalmazó napló lekérdezések</span><span class="sxs-lookup"><span data-stu-id="e463d-178">Log queries with Syslog records</span></span>
<span data-ttu-id="e463d-179">hello alábbi táblázat példákat különböző Syslog lehívása napló-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="e463d-179">hello following table provides different examples of log queries that retrieve Syslog records.</span></span>

| <span data-ttu-id="e463d-180">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e463d-180">Query</span></span> | <span data-ttu-id="e463d-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="e463d-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e463d-182">Típus = Syslog</span><span class="sxs-lookup"><span data-stu-id="e463d-182">Type=Syslog</span></span> |<span data-ttu-id="e463d-183">Minden rendszerbejegyzések.</span><span class="sxs-lookup"><span data-stu-id="e463d-183">All Syslogs.</span></span> |
| <span data-ttu-id="e463d-184">Típus Syslog súlyossági szint = hiba =</span><span class="sxs-lookup"><span data-stu-id="e463d-184">Type=Syslog SeverityLevel=error</span></span> |<span data-ttu-id="e463d-185">Az összes Syslog rekordot hiba súlyosságát.</span><span class="sxs-lookup"><span data-stu-id="e463d-185">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="e463d-186">Típus = Syslog &#124; Számítógép által a mérték count()</span><span class="sxs-lookup"><span data-stu-id="e463d-186">Type=Syslog &#124; measure count() by Computer</span></span> |<span data-ttu-id="e463d-187">Számítógép által bejegyzések száma a Syslog.</span><span class="sxs-lookup"><span data-stu-id="e463d-187">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="e463d-188">Típus = Syslog &#124; mérték count() létesítmény által</span><span class="sxs-lookup"><span data-stu-id="e463d-188">Type=Syslog &#124; measure count() by Facility</span></span> |<span data-ttu-id="e463d-189">A Syslog száma rekordok létesítmény szerint.</span><span class="sxs-lookup"><span data-stu-id="e463d-189">Count of Syslog records by facility.</span></span> |

>[!NOTE]
> <span data-ttu-id="e463d-190">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.</span><span class="sxs-lookup"><span data-stu-id="e463d-190">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then hello above queries would change toohello following.</span></span>

> | <span data-ttu-id="e463d-191">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="e463d-191">Query</span></span> | <span data-ttu-id="e463d-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="e463d-192">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="e463d-193">Rendszernapló</span><span class="sxs-lookup"><span data-stu-id="e463d-193">Syslog</span></span> |<span data-ttu-id="e463d-194">Minden rendszerbejegyzések.</span><span class="sxs-lookup"><span data-stu-id="e463d-194">All Syslogs.</span></span> |
| <span data-ttu-id="e463d-195">Syslog &#124; adott súlyossági szint == "error"</span><span class="sxs-lookup"><span data-stu-id="e463d-195">Syslog &#124; where SeverityLevel == "error"</span></span> |<span data-ttu-id="e463d-196">Az összes Syslog rekordot hiba súlyosságát.</span><span class="sxs-lookup"><span data-stu-id="e463d-196">All Syslog records with severity of error.</span></span> |
| <span data-ttu-id="e463d-197">Syslog &#124; AggregatedValue összefoglalója = count() számítógépenként</span><span class="sxs-lookup"><span data-stu-id="e463d-197">Syslog &#124; summarize AggregatedValue = count() by Computer</span></span> |<span data-ttu-id="e463d-198">Számítógép által bejegyzések száma a Syslog.</span><span class="sxs-lookup"><span data-stu-id="e463d-198">Count of Syslog records by computer.</span></span> |
| <span data-ttu-id="e463d-199">Syslog &#124; AggregatedValue összefoglalója létesítmény által count() =</span><span class="sxs-lookup"><span data-stu-id="e463d-199">Syslog &#124; summarize AggregatedValue = count() by Facility</span></span> |<span data-ttu-id="e463d-200">A Syslog száma rekordok létesítmény szerint.</span><span class="sxs-lookup"><span data-stu-id="e463d-200">Count of Syslog records by facility.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e463d-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e463d-201">Next steps</span></span>
* <span data-ttu-id="e463d-202">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="e463d-202">Learn about [log searches](log-analytics-log-searches.md) tooanalyze hello data collected from data sources and solutions.</span></span>
* <span data-ttu-id="e463d-203">Használjon [egyéni mezők](log-analytics-custom-fields.md) tooparse adatainak syslog rekordból egyes mezőkbe.</span><span class="sxs-lookup"><span data-stu-id="e463d-203">Use [Custom Fields](log-analytics-custom-fields.md) tooparse data from syslog records into individual fields.</span></span>
* <span data-ttu-id="e463d-204">[Linux-ügynökök konfigurálása](log-analytics-linux-agents.md) toocollect más típusú adatok.</span><span class="sxs-lookup"><span data-stu-id="e463d-204">[Configure Linux agents](log-analytics-linux-agents.md) toocollect other types of data.</span></span>
