---
title: "aaaMonitor és kezelése az Ambari webes felhasználói felületen Azure HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Ambari toomonitor és a Linux-alapú HDInsight-fürtök kezelése. Ebből a dokumentumból megismerheti, hogyan toouse hello Ambari webes felhasználói felületén mellékelt a HDInsight-fürtök."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: d422c40e63345d7054839a625e115c50dad040f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-web-ui"></a><span data-ttu-id="9094b-104">A HDInsight-fürtök kezelése hello Ambari webes felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="9094b-104">Manage HDInsight clusters by using hello Ambari Web UI</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="9094b-105">Apache Ambari egyszerűbbé teszi a hello kezelése és figyelése a Hadoop fürtök egy egyszerű toouse webes felhasználói felület és a REST API-k megadásával.</span><span class="sxs-lookup"><span data-stu-id="9094b-105">Apache Ambari simplifies hello management and monitoring of a Hadoop cluster by providing an easy toouse web UI and REST API.</span></span> <span data-ttu-id="9094b-106">Ambari Linux-alapú HDInsight-fürtök része, és a használt toomonitor hello fürt ellenőrizze konfigurációs módosítások.</span><span class="sxs-lookup"><span data-stu-id="9094b-106">Ambari is included on Linux-based HDInsight clusters, and is used toomonitor hello cluster and make configuration changes.</span></span>

<span data-ttu-id="9094b-107">Ebből a dokumentumból megismerheti, hogyan toouse hello HDInsight-fürtök az Ambari webes felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9094b-107">In this document, you learn how toouse hello Ambari Web UI with an HDInsight cluster.</span></span>

## <span data-ttu-id="9094b-108"><a id="whatis"></a>Mi az az Ambari?</span><span class="sxs-lookup"><span data-stu-id="9094b-108"><a id="whatis"></a>What is Ambari?</span></span>

<span data-ttu-id="9094b-109">[Apache Ambari](http://ambari.apache.org) egy könnyen használható webes felhasználói felület biztosításával egyszerűsíti a Hadoop kezelését.</span><span class="sxs-lookup"><span data-stu-id="9094b-109">[Apache Ambari](http://ambari.apache.org) simplifies Hadoop management by providing an easy-to-use web UI.</span></span> <span data-ttu-id="9094b-110">Használhatja az Ambari létrehozása, kezelése és figyelése a Hadoop-fürtök.</span><span class="sxs-lookup"><span data-stu-id="9094b-110">You can use Ambari create, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="9094b-111">A fejlesztők integrálható a ezeket a képességeket a alkalmazások hello segítségével [Ambari REST API-k](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="9094b-111">Developers can integrate these capabilities into their applications by using hello [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="9094b-112">hello Ambari webes felhasználói felület alapértelmezés szerint a HDInsight-fürtök hello Linux operációs rendszert használó valósul meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-112">hello Ambari Web UI is provided by default with HDInsight clusters that use hello Linux operating system.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9094b-113">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="9094b-113">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="9094b-114">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="9094b-114">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 

## <a name="connectivity"></a><span data-ttu-id="9094b-115">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="9094b-115">Connectivity</span></span>

<span data-ttu-id="9094b-116">hello Ambari webes felhasználói felület érhető el a HDInsight-fürthöz: HTTPS://CLUSTERNAME.azurehdidnsight.net, ahol **CLUSTERNAME** hello a fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="9094b-116">hello Ambari Web UI is available on your HDInsight cluster at HTTPS://CLUSTERNAME.azurehdidnsight.net, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9094b-117">A HDInsight tooAmbari csatlakozás igényel a HTTPS PROTOKOLLT.</span><span class="sxs-lookup"><span data-stu-id="9094b-117">Connecting tooAmbari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="9094b-118">Amikor a rendszer, hello rendszergazda fiók nevet és jelszót hello fürt létrehozásakor megadott használja.</span><span class="sxs-lookup"><span data-stu-id="9094b-118">When prompted for authentication, use hello admin account name and password you provided when hello cluster was created.</span></span>

## <a name="ssh-tunnel-proxy"></a><span data-ttu-id="9094b-119">SSH-alagutat (proxy)</span><span class="sxs-lookup"><span data-stu-id="9094b-119">SSH tunnel (proxy)</span></span>

<span data-ttu-id="9094b-120">A fürt Ambari közvetlenül hello interneten keresztül érhető el, amíg az Ambari webes felhasználói felület (pl. toohello JobTracker) nem érhetők el a hello egyes hivatkozások hello internet.</span><span class="sxs-lookup"><span data-stu-id="9094b-120">While Ambari for your cluster is accessible directly over hello Internet, some links from hello Ambari Web UI (such as toohello JobTracker) are not exposed on hello internet.</span></span> <span data-ttu-id="9094b-121">Ezek a szolgáltatások tooaccess, létre kell hoznia egy SSH-alagutat.</span><span class="sxs-lookup"><span data-stu-id="9094b-121">tooaccess these services, you must create an SSH tunnel.</span></span> <span data-ttu-id="9094b-122">További információkért lásd: [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="9094b-122">For more information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

## <a name="ambari-web-ui"></a><span data-ttu-id="9094b-123">Ambari webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="9094b-123">Ambari Web UI</span></span>

<span data-ttu-id="9094b-124">Ha toohello Ambari webes felhasználói felületén, rákérdezéses tooauthenticate toohello lap áll.</span><span class="sxs-lookup"><span data-stu-id="9094b-124">When connecting toohello Ambari Web UI, you are prompted tooauthenticate toohello page.</span></span> <span data-ttu-id="9094b-125">Hello fürt rendszergazdai jogú felhasználó (alapértelmezett Admin) és a fürt létrehozása során használt jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="9094b-125">Use hello cluster admin user (default Admin) and password you used during cluster creation.</span></span>

<span data-ttu-id="9094b-126">Hello lap megnyitása után vegye figyelembe a hello sáv hello tetején.</span><span class="sxs-lookup"><span data-stu-id="9094b-126">When hello page opens, note hello bar at hello top.</span></span> <span data-ttu-id="9094b-127">A sáv tartalmazza hello alábbi információkat és -vezérlők:</span><span class="sxs-lookup"><span data-stu-id="9094b-127">This bar contains hello following information and controls:</span></span>

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* <span data-ttu-id="9094b-129">**Ambari embléma** -megnyílik hello irányítópultot, melyet használt toomonitor hello tárolófürt is lehet.</span><span class="sxs-lookup"><span data-stu-id="9094b-129">**Ambari logo** - Opens hello dashboard, which can be used toomonitor hello cluster.</span></span>

* <span data-ttu-id="9094b-130">**A fürt neve # ops** -hello Ambari folyamatban lévő műveletek számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="9094b-130">**Cluster name # ops** - Displays hello number of ongoing Ambari operations.</span></span> <span data-ttu-id="9094b-131">Kiválasztásával hello fürt nevét vagy **# ops** háttérbeli műveletek listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-131">Selecting hello cluster name or **# ops** displays a list of background operations.</span></span>

* <span data-ttu-id="9094b-132">**# riasztások** -jeleníti meg a figyelmeztetés vagy kritikus riasztás, ha vannak ilyenek, hello fürt.</span><span class="sxs-lookup"><span data-stu-id="9094b-132">**# alerts** - Displays warnings or critical alerts, if any, for hello cluster.</span></span>

* <span data-ttu-id="9094b-133">**Irányítópult** -hello irányítópult jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-133">**Dashboard** - Displays hello dashboard.</span></span>

* <span data-ttu-id="9094b-134">**Szolgáltatások** -hello szolgáltatások hello fürt adatait és konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="9094b-134">**Services** - Information and configuration settings for hello services in hello cluster.</span></span>

* <span data-ttu-id="9094b-135">**Gazdagépek** -információkat és a konfigurációs beállítások hello fürt hello csomópontján.</span><span class="sxs-lookup"><span data-stu-id="9094b-135">**Hosts** - Information and configuration settings for hello nodes in hello cluster.</span></span>

* <span data-ttu-id="9094b-136">**Riasztások** -adatokat, figyelmeztetéseket és a kritikus riasztások naplóját.</span><span class="sxs-lookup"><span data-stu-id="9094b-136">**Alerts** - A log of information, warnings, and critical alerts.</span></span>

* <span data-ttu-id="9094b-137">**Felügyeleti** -verem/szoftverszolgáltatások hello fürt, a szolgáltatás fiókjának adatait, és a Kerberos biztonsági telepített.</span><span class="sxs-lookup"><span data-stu-id="9094b-137">**Admin** - Software stack/services that are installed on hello cluster, service account information, and Kerberos security.</span></span>

* <span data-ttu-id="9094b-138">**Felügyeleti gomb** -Ambari felügyeleti, a felhasználói beállításokat és kijelentkezési.</span><span class="sxs-lookup"><span data-stu-id="9094b-138">**Admin button** - Ambari management, user settings, and logout.</span></span>

## <a name="monitoring"></a><span data-ttu-id="9094b-139">Figyelés</span><span class="sxs-lookup"><span data-stu-id="9094b-139">Monitoring</span></span>

### <a name="alerts"></a><span data-ttu-id="9094b-140">Riasztások</span><span class="sxs-lookup"><span data-stu-id="9094b-140">Alerts</span></span>

<span data-ttu-id="9094b-141">hello alábbi lista hello közös figyelmeztetési állapotok Ambari használják.</span><span class="sxs-lookup"><span data-stu-id="9094b-141">hello following list contains hello common alert statuses used by Ambari:</span></span>

* <span data-ttu-id="9094b-142">**OKÉ**</span><span class="sxs-lookup"><span data-stu-id="9094b-142">**OK**</span></span>
* <span data-ttu-id="9094b-143">**Figyelmeztetés**</span><span class="sxs-lookup"><span data-stu-id="9094b-143">**Warning**</span></span>
* <span data-ttu-id="9094b-144">**KRITIKUS**</span><span class="sxs-lookup"><span data-stu-id="9094b-144">**CRITICAL**</span></span>
* <span data-ttu-id="9094b-145">**ISMERETLEN**</span><span class="sxs-lookup"><span data-stu-id="9094b-145">**UNKNOWN**</span></span>

<span data-ttu-id="9094b-146">Riasztások eltérő **OK** hello következtében **# riasztások** bejegyzés hello tetején hello lap toodisplay hello értesítések száma.</span><span class="sxs-lookup"><span data-stu-id="9094b-146">Alerts other than **OK** cause hello **# alerts** entry at hello top of hello page toodisplay hello number of alerts.</span></span> <span data-ttu-id="9094b-147">Ez a bejegyzés látható értesítések valamelyikének kiválasztásakor hello riasztások és azok állapotát.</span><span class="sxs-lookup"><span data-stu-id="9094b-147">Selecting this entry displays hello alerts and their status.</span></span>

<span data-ttu-id="9094b-148">Riasztások több alapértelmezett csoport, amely hello megtekinthetők vannak szervezve **riasztások** lap.</span><span class="sxs-lookup"><span data-stu-id="9094b-148">Alerts are organized into several default groups, which can be viewed from hello **Alerts** page.</span></span>

![Riasztások lap](./media/hdinsight-hadoop-manage-ambari/alerts.png)

<span data-ttu-id="9094b-150">Hello segítségével kezelheti a hello csoportok **műveletek** menüben, majd válassza **riasztási csoportok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="9094b-150">You can manage hello groups by using hello **Actions** menu and selecting **Manage Alert Groups**.</span></span>

![riasztási csoportok párbeszédpanel kezelése](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

<span data-ttu-id="9094b-152">Is kezelheti a riasztási módszerek, és riasztási értesítések készíteni hello **műveletek** menü kiválasztásával __riasztás értesítések kezelésére__.</span><span class="sxs-lookup"><span data-stu-id="9094b-152">You can also manage alerting methods, and create alert notifications from hello **Actions** menu by selecting __Manage Alert Notifications__.</span></span> <span data-ttu-id="9094b-153">Aktuális értesítések jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-153">Any current notifications are displayed.</span></span> <span data-ttu-id="9094b-154">Itt értesítéseket is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="9094b-154">You can also create notifications from here.</span></span> <span data-ttu-id="9094b-155">Keresztül is elküldhetik az értesítéseket **E-mail** vagy **SNMP** Ha adott riasztás/súlyossági kombinációk történik.</span><span class="sxs-lookup"><span data-stu-id="9094b-155">Notifications can be sent via **EMAIL** or **SNMP** when specific alert/severity combinations occur.</span></span> <span data-ttu-id="9094b-156">Például elküldheti e-mailt minden egyes hello riasztások hello **YARN alapértelmezett** csoportja túl**kritikus**.</span><span class="sxs-lookup"><span data-stu-id="9094b-156">For example, you can send an email message when any of hello alerts in hello **YARN Default** group is set too**Critical**.</span></span>

![Hozzon létre figyelmeztető párbeszédpanel](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

<span data-ttu-id="9094b-158">Végezetül kiválasztásával __riasztást beállításainak kezelése__ a hello __műveletek__ menü lehetővé teszi tooset hello ennyiszer kell a riasztás akkor fordulhat elő, egy értesítés elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="9094b-158">Finally, selecting __Manage Alert Settings__ from hello __Actions__ menu allows you tooset hello number of times an alert must occur before a notification is sent.</span></span> <span data-ttu-id="9094b-159">Ezzel a beállítással lehet használt tooprevent értesítések átmeneti hibák esetén.</span><span class="sxs-lookup"><span data-stu-id="9094b-159">This setting can be used tooprevent notifications for transient errors.</span></span>

### <a name="cluster"></a><span data-ttu-id="9094b-160">Fürt</span><span class="sxs-lookup"><span data-stu-id="9094b-160">Cluster</span></span>

<span data-ttu-id="9094b-161">Hello **metrikák** hello irányítópult lapja tartalmazza, melyek könnyen toomonitor hello állapota a fürt egy pillantással widgeteket.</span><span class="sxs-lookup"><span data-stu-id="9094b-161">hello **Metrics** tab of hello dashboard contains a series of widgets that make it easy toomonitor hello status of your cluster at a glance.</span></span> <span data-ttu-id="9094b-162">Több widgets, például a **CPU-használat**, kattintáskor további információkkal.</span><span class="sxs-lookup"><span data-stu-id="9094b-162">Several widgets, such as **CPU Usage**, provide additional information when clicked.</span></span>

![a metrikák irányítópult](./media/hdinsight-hadoop-manage-ambari/metrics.png)

<span data-ttu-id="9094b-164">Hello **Heatmaps** lapon színes heatmaps zöld toored üzembe helyezésről metrikákat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-164">hello **Heatmaps** tab displays metrics as colored heatmaps, going from green toored.</span></span>

![heatmaps irányítópult](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

<span data-ttu-id="9094b-166">Hello fürt csomópontja hello további információkért válassza **állomások**.</span><span class="sxs-lookup"><span data-stu-id="9094b-166">For more information on hello nodes within hello cluster, select **Hosts**.</span></span> <span data-ttu-id="9094b-167">Ezután válassza ki a hello adott csomópont szüksége.</span><span class="sxs-lookup"><span data-stu-id="9094b-167">Then select hello specific node you are interested in.</span></span>

![állomás részletei](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a><span data-ttu-id="9094b-169">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9094b-169">Services</span></span>

<span data-ttu-id="9094b-170">Hello **szolgáltatások** hello irányítópult oldalsávon hello hello fürt hello szolgáltatás állapotának gyors betekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="9094b-170">hello **Services** sidebar on hello dashboard provides quick insight into hello status of hello services running on hello cluster.</span></span> <span data-ttu-id="9094b-171">Különböző ikonok is használt tooindicate állapot vagy a végrehajtandó műveleteket.</span><span class="sxs-lookup"><span data-stu-id="9094b-171">Various icons are used tooindicate status or actions that should be taken.</span></span> <span data-ttu-id="9094b-172">Például egy sárga újrahasznosítást szimbólum jelenik meg, ha egy szolgáltatásnak kell toobe felhasználását.</span><span class="sxs-lookup"><span data-stu-id="9094b-172">For example, a yellow recycle symbol is displayed if a service needs toobe recycled.</span></span>

![szolgáltatások oldalsó sáv](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> <span data-ttu-id="9094b-174">HDInsight-fürttípusok és verziók eltérő hello szolgáltatások jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-174">hello services displayed differ between HDInsight cluster types and versions.</span></span> <span data-ttu-id="9094b-175">Itt látható hello szolgáltatások eltérhetnek hello szolgáltatások jelenik meg a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="9094b-175">hello services displayed here may be different than hello services displayed for your cluster.</span></span>

<span data-ttu-id="9094b-176">A szolgáltatás látható értesítések valamelyikének kiválasztásakor részletesebb információk hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="9094b-176">Selecting a service displays more detailed information on hello service.</span></span>

![szolgáltatás összefoglaló információk](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a><span data-ttu-id="9094b-178">Gyorshivatkozások</span><span class="sxs-lookup"><span data-stu-id="9094b-178">Quick links</span></span>

<span data-ttu-id="9094b-179">Egyes szolgáltatások megjelenítése egy **Gyorshivatkozások** hivatkozás hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="9094b-179">Some services display a **Quick Links** link at hello top of hello page.</span></span> <span data-ttu-id="9094b-180">Ez lehet használt tooaccess szolgáltatásspecifikus web UI, mint például:</span><span class="sxs-lookup"><span data-stu-id="9094b-180">This can be used tooaccess service-specific web UIs, such as:</span></span>

* <span data-ttu-id="9094b-181">**Feladatelőzmények** -MapReduce-feladatok előzményeinek.</span><span class="sxs-lookup"><span data-stu-id="9094b-181">**Job History** - MapReduce job history.</span></span>
* <span data-ttu-id="9094b-182">**Erőforrás-kezelő** -YARN erőforrás-kezelő felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9094b-182">**Resource Manager** - YARN ResourceManager UI.</span></span>
* <span data-ttu-id="9094b-183">**NameNode** -Hadoop elosztott fájl System (HDFS) NameNode felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="9094b-183">**NameNode** - Hadoop Distributed File System (HDFS) NameNode UI.</span></span>
* <span data-ttu-id="9094b-184">**Oozie webes felhasználói felület** -Oozie felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="9094b-184">**Oozie Web UI** - Oozie UI.</span></span>

<span data-ttu-id="9094b-185">Új lap ezeket a hivatkozásokat kiválasztásával megnyílik a böngészőben, amely hello kijelölt oldalát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-185">Selecting any of these links opens a new tab in your browser, which displays hello selected page.</span></span>

> [!NOTE]
> <span data-ttu-id="9094b-186">Kiválasztásával hello **Gyorshivatkozások** egy szolgáltatás bejegyzése térhetnek vissza egy "a kiszolgáló nem található" hiba.</span><span class="sxs-lookup"><span data-stu-id="9094b-186">Selecting hello **Quick Links** entry for a service may return a "server not found" error.</span></span> <span data-ttu-id="9094b-187">Ha ezt a hibát észlel, használnia kell az SSH-alagút hello használatakor **Gyorshivatkozások** bejegyzés ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9094b-187">If you encounter this error, you must use an SSH tunnel when using hello **Quick Links** entry for this service.</span></span> <span data-ttu-id="9094b-188">További információ: [használata SSH Tunneling a hdinsight eszközzel](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="9094b-188">For information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

## <a name="management"></a><span data-ttu-id="9094b-189">Kezelés</span><span class="sxs-lookup"><span data-stu-id="9094b-189">Management</span></span>

### <a name="ambari-users-groups-and-permissions"></a><span data-ttu-id="9094b-190">Ambari felhasználók, csoportok és engedélyek</span><span class="sxs-lookup"><span data-stu-id="9094b-190">Ambari users, groups, and permissions</span></span>

<span data-ttu-id="9094b-191">Felhasználók, csoportok és engedélyek végzett használata esetén támogatottak. egy [tartományhoz csatlakoztatott](hdinsight-domain-joined-introduction.md) HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="9094b-191">Working with users, groups, and permissions are supported when using a [domain joined](hdinsight-domain-joined-introduction.md) HDInsight cluster.</span></span> <span data-ttu-id="9094b-192">A hello Ambari felügyeleti felhasználói felület használatával olyan tartományhoz csatlakoztatott fürtön információkért lásd: [tartományhoz HDInsight-fürtök kezelése](hdinsight-domain-joined-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9094b-192">For information on using hello Ambari Management UI on a domain-joined cluster, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-introduction.md).</span></span>

> [!WARNING]
> <span data-ttu-id="9094b-193">Ne változtassa meg jelszavát hello hello Ambari figyelő (hdinsightwatchdog) a Linux-alapú HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="9094b-193">Do not change hello password of hello Ambari watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="9094b-194">Hello jelszó oldaltörések módosítása képességét toouse Parancsfájlműveletek hello, vagy a fürthöz méretezési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="9094b-194">Changing hello password breaks hello ability toouse script actions or perform scaling operations with your cluster.</span></span>

### <a name="hosts"></a><span data-ttu-id="9094b-195">Hosts</span><span class="sxs-lookup"><span data-stu-id="9094b-195">Hosts</span></span>

<span data-ttu-id="9094b-196">Hello **állomások** laplistákhoz hello fürt összes gazdagépén.</span><span class="sxs-lookup"><span data-stu-id="9094b-196">hello **Hosts** page lists all hosts in hello cluster.</span></span> <span data-ttu-id="9094b-197">toomanage állomások, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="9094b-197">toomanage hosts, follow these steps.</span></span>

![gazdagépek lap](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> <span data-ttu-id="9094b-199">Hozzáadása, leszerelése és recommissioning egy gazdagép nem használható a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="9094b-199">Adding, decommissioning, and recommissioning a host should not be used with HDInsight clusters.</span></span>

1. <span data-ttu-id="9094b-200">Válassza ki, hogy kívánja-e toomanage hello állomás.</span><span class="sxs-lookup"><span data-stu-id="9094b-200">Select hello host that you wish toomanage.</span></span>

2. <span data-ttu-id="9094b-201">Használjon hello **műveletek** menü tooselect hello művelet, hogy kívánja-e tooperform:</span><span class="sxs-lookup"><span data-stu-id="9094b-201">Use hello **Actions** menu tooselect hello action that you wish tooperform:</span></span>

   * <span data-ttu-id="9094b-202">**Indítsa el az összes összetevő** -elindítja az összes összetevő hello a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9094b-202">**Start all components** - Start all components on hello host.</span></span>

   * <span data-ttu-id="9094b-203">**Állítsa le az összes összetevő** -állítsa le az összes összetevő hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9094b-203">**Stop all components** - Stop all components on hello host.</span></span>

   * <span data-ttu-id="9094b-204">**Indítsa újra az összes összetevő** - Stop, és indítsa el az összes összetevő hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9094b-204">**Restart all components** - Stop and start all components on hello host.</span></span>

   * <span data-ttu-id="9094b-205">**Kapcsolja be a karbantartási mód** -riasztások hello állomás mellőzi.</span><span class="sxs-lookup"><span data-stu-id="9094b-205">**Turn on maintenance mode** - Suppresses alerts for hello host.</span></span> <span data-ttu-id="9094b-206">Hozzon létre riasztást műveleteket hajtja végre ezt a módot kell engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="9094b-206">This mode should be enabled if you are performing actions that generate alerts.</span></span> <span data-ttu-id="9094b-207">Például a szolgáltatás indítása és leállítása.</span><span class="sxs-lookup"><span data-stu-id="9094b-207">For example, stopping and starting a service.</span></span>

   * <span data-ttu-id="9094b-208">**Kapcsolja ki a karbantartási mód** -értéket ad vissza hello állomás toonormal lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9094b-208">**Turn off maintenance mode** - Returns hello host toonormal alerting.</span></span>

   * <span data-ttu-id="9094b-209">**Állítsa le** -leállítja DataNode vagy NodeManagers hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9094b-209">**Stop** - Stops DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="9094b-210">**Start** -DataNode kezdődik vagy NodeManagers hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9094b-210">**Start** - Starts DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="9094b-211">**Indítsa újra a** -leáll, és elindítja DataNode vagy NodeManagers hello gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="9094b-211">**Restart** - Stops and starts DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="9094b-212">**Szerelje le** -állomás eltávolítja hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="9094b-212">**Decommission** - Removes a host from hello cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9094b-213">A HDInsight-fürtökön ne használja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="9094b-213">Do not use this action on HDInsight clusters.</span></span>

   * <span data-ttu-id="9094b-214">**Recommission** – egy korábban már leszerelt toohello gazdagépfürt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9094b-214">**Recommission** - Adds a previously decommissioned host toohello cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9094b-215">A HDInsight-fürtökön ne használja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="9094b-215">Do not use this action on HDInsight clusters.</span></span>

### <span data-ttu-id="9094b-216"><a id="service"></a>Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9094b-216"><a id="service"></a>Services</span></span>

<span data-ttu-id="9094b-217">A hello **irányítópult** vagy **szolgáltatások** lapján hello használata **műveletek** szolgáltatások toostop hello listája hello alján gombra, majd indítsa el az összes szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9094b-217">From hello **Dashboard** or **Services** page, use hello **Actions** button at hello bottom of hello list of services toostop and start all services.</span></span>

![szolgáltatás műveletek](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> <span data-ttu-id="9094b-219">Amíg **hozzáadása szolgáltatás** szerepel ebben a menüben nem használt tooadd szolgáltatások toohello HDInsight-fürt kell tenni.</span><span class="sxs-lookup"><span data-stu-id="9094b-219">While **Add Service** is listed in this menu, it should not be used tooadd services toohello HDInsight cluster.</span></span> <span data-ttu-id="9094b-220">Új szolgáltatások fel kell venni, hogy egy parancsfájl műveletével a fürtök kiépítése során.</span><span class="sxs-lookup"><span data-stu-id="9094b-220">New services should be added using a Script Action during cluster provisioning.</span></span> <span data-ttu-id="9094b-221">Parancsfájlműveletek használatával kapcsolatos további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="9094b-221">For more information on using Script Actions, see [Customize HDInsight clusters using Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="9094b-222">Hello közben **műveletek** gomb újraindíthatja az összes szolgáltatás, gyakran kívánt toostart, állítsa le vagy egy adott szolgáltatás újraindítása.</span><span class="sxs-lookup"><span data-stu-id="9094b-222">While hello **Actions** button can restart all services, often you want toostart, stop, or restart a specific service.</span></span> <span data-ttu-id="9094b-223">Hello lépéseket tooperform műveletek egy szolgáltatás a következő használja:</span><span class="sxs-lookup"><span data-stu-id="9094b-223">Use hello following steps tooperform actions on an individual service:</span></span>

1. <span data-ttu-id="9094b-224">A hello **irányítópult** vagy **szolgáltatások** lapon, válassza ki a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9094b-224">From hello **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="9094b-225">A hello felső részén hello **összegzés** lapján hello használata **szolgáltatás műveletek** gombra, jelölje be hello művelet tootake.</span><span class="sxs-lookup"><span data-stu-id="9094b-225">From hello top of hello **Summary** tab, use hello **Service Actions** button and select hello action tootake.</span></span> <span data-ttu-id="9094b-226">Ezzel a hello szolgáltatást az összes csomópont újraindul.</span><span class="sxs-lookup"><span data-stu-id="9094b-226">This restarts hello service on all nodes.</span></span>

    ![szolgáltatási művelet](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > <span data-ttu-id="9094b-228">Egyes szolgáltatások újraindítása hello fürt futása közben riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="9094b-228">Restarting some services while hello cluster is running may generate alerts.</span></span> <span data-ttu-id="9094b-229">tooavoid riasztást küld, használhatja a hello **szolgáltatás műveletek** gomb tooenable **karbantartási módba** hello szolgáltatás hello végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="9094b-229">tooavoid alerts, you can use hello **Service Actions** button tooenable **Maintenance mode** for hello service before performing hello restart.</span></span>

3. <span data-ttu-id="9094b-230">Egy műveletet a kijelölt hello **# op** bejegyzés hello tetején hello lap lépésekben tooshow a háttérművelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="9094b-230">Once an action has been selected, hello **# op** entry at hello top of hello page increments tooshow that a background operation is occurring.</span></span> <span data-ttu-id="9094b-231">Ha konfigurálva toodisplay, háttér műveletek listájának hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-231">If configured toodisplay, hello list of background operations is displayed.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9094b-232">Ha engedélyezte a **karbantartási módba** hello szolgáltatás, ne feledje toodisable azt hello segítségével **szolgáltatás műveletek** hello művelet befejeződése után gombra.</span><span class="sxs-lookup"><span data-stu-id="9094b-232">If you enabled **Maintenance mode** for hello service, remember toodisable it by using hello **Service Actions** button once hello operation has finished.</span></span>

<span data-ttu-id="9094b-233">tooconfigure egy szolgáltatás, a lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="9094b-233">tooconfigure a service, use hello following steps:</span></span>

1. <span data-ttu-id="9094b-234">A hello **irányítópult** vagy **szolgáltatások** lapon, válassza ki a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9094b-234">From hello **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="9094b-235">Jelölje be hello **Configs** külön-külön hello aktuális konfigurációs jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9094b-235">Select hello **Configs** tab. hello current configuration is displayed.</span></span> <span data-ttu-id="9094b-236">A fenti konfiguráció listáját is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="9094b-236">A list of previous configurations is also displayed.</span></span>

    ![Konfigurációk](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. <span data-ttu-id="9094b-238">Hello megjelenített mezők toomodify hello konfigurációt használja, és válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9094b-238">Use hello fields displayed toomodify hello configuration, and then select **Save**.</span></span> <span data-ttu-id="9094b-239">Vagy válassza ki az előző konfigurációt, és válassza ki **ellenőrizze aktuális** tooroll biztonsági toohello beállításokat.</span><span class="sxs-lookup"><span data-stu-id="9094b-239">Or select a previous configuration and then select **Make current** tooroll back toohello previous settings.</span></span>

## <a name="ambari-views"></a><span data-ttu-id="9094b-240">Az Ambari nézetek</span><span class="sxs-lookup"><span data-stu-id="9094b-240">Ambari views</span></span>

<span data-ttu-id="9094b-241">Az Ambari nézetek oszthatja fejlesztők tooplug felhasználói felületi elemei hello Ambari webes felhasználói felület használatával hello [Ambari nézetek keretrendszer](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span><span class="sxs-lookup"><span data-stu-id="9094b-241">Ambari Views allow developers tooplug UI elements into hello Ambari Web UI using hello [Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span></span> <span data-ttu-id="9094b-242">HDInsight Hadoop-fürt típusú nézetek következő hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9094b-242">HDInsight provides hello following views with Hadoop cluster types:</span></span>

* <span data-ttu-id="9094b-243">Yarn várólista-kezelő: hello várólista-kezelő egy egyszerű felhasználói Felületet biztosít megtekintése és módosítása a YARN várólisták.</span><span class="sxs-lookup"><span data-stu-id="9094b-243">Yarn Queue Manager: hello queue manager provides a simple UI for viewing and modifying YARN queues.</span></span>

* <span data-ttu-id="9094b-244">Hive nézete: hello Hive nézet lehetővé teszi toorun Hive-lekérdezéseket közvetlenül a böngészőből.</span><span class="sxs-lookup"><span data-stu-id="9094b-244">Hive View: hello Hive View allows you toorun Hive queries directly from your web browser.</span></span> <span data-ttu-id="9094b-245">Lekérdezések mentése, eredmények megtekintése, toohello fürttároló-eredményeket menteni, vagy töltse le az eredményeket tooyour helyi rendszer.</span><span class="sxs-lookup"><span data-stu-id="9094b-245">You can save queries, view results, save results toohello cluster storage, or download results tooyour local system.</span></span> <span data-ttu-id="9094b-246">A Hive-nézetek használata további információkért lásd: [Hive nézetek és a HDInsight együttes](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="9094b-246">For more information on using Hive Views, see [Use Hive Views with HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>

* <span data-ttu-id="9094b-247">Tez nézet: hello Tez nézet lehetővé teszi toobetter megértéséhez, valamint feladatok optimalizálása.</span><span class="sxs-lookup"><span data-stu-id="9094b-247">Tez View: hello Tez View allows you toobetter understand and optimize jobs.</span></span> <span data-ttu-id="9094b-248">A Tez feladatok végrehajtásának módját, és milyen erőforrásokat használt információk is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="9094b-248">You can view information on how Tez jobs are executed and what resources are used.</span></span>
