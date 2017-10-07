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
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-web-ui"></a>A HDInsight-fürtök kezelése hello Ambari webes felhasználói felület használatával

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari egyszerűbbé teszi a hello kezelése és figyelése a Hadoop fürtök egy egyszerű toouse webes felhasználói felület és a REST API-k megadásával. Ambari Linux-alapú HDInsight-fürtök része, és a használt toomonitor hello fürt ellenőrizze konfigurációs módosítások.

Ebből a dokumentumból megismerheti, hogyan toouse hello HDInsight-fürtök az Ambari webes felhasználói felületén.

## <a id="whatis"></a>Mi az az Ambari?

[Apache Ambari](http://ambari.apache.org) egy könnyen használható webes felhasználói felület biztosításával egyszerűsíti a Hadoop kezelését. Használhatja az Ambari létrehozása, kezelése és figyelése a Hadoop-fürtök. A fejlesztők integrálható a ezeket a képességeket a alkalmazások hello segítségével [Ambari REST API-k](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

hello Ambari webes felhasználói felület alapértelmezés szerint a HDInsight-fürtök hello Linux operációs rendszert használó valósul meg.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). 

## <a name="connectivity"></a>Kapcsolatok

hello Ambari webes felhasználói felület érhető el a HDInsight-fürthöz: HTTPS://CLUSTERNAME.azurehdidnsight.net, ahol **CLUSTERNAME** hello a fürt neve van.

> [!IMPORTANT]
> A HDInsight tooAmbari csatlakozás igényel a HTTPS PROTOKOLLT. Amikor a rendszer, hello rendszergazda fiók nevet és jelszót hello fürt létrehozásakor megadott használja.

## <a name="ssh-tunnel-proxy"></a>SSH-alagutat (proxy)

A fürt Ambari közvetlenül hello interneten keresztül érhető el, amíg az Ambari webes felhasználói felület (pl. toohello JobTracker) nem érhetők el a hello egyes hivatkozások hello internet. Ezek a szolgáltatások tooaccess, létre kell hoznia egy SSH-alagutat. További információkért lásd: [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Ambari webes felhasználói felület

Ha toohello Ambari webes felhasználói felületén, rákérdezéses tooauthenticate toohello lap áll. Hello fürt rendszergazdai jogú felhasználó (alapértelmezett Admin) és a fürt létrehozása során használt jelszó használata.

Hello lap megnyitása után vegye figyelembe a hello sáv hello tetején. A sáv tartalmazza hello alábbi információkat és -vezérlők:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Ambari embléma** -megnyílik hello irányítópultot, melyet használt toomonitor hello tárolófürt is lehet.

* **A fürt neve # ops** -hello Ambari folyamatban lévő műveletek számát mutatja. Kiválasztásával hello fürt nevét vagy **# ops** háttérbeli műveletek listáját jeleníti meg.

* **# riasztások** -jeleníti meg a figyelmeztetés vagy kritikus riasztás, ha vannak ilyenek, hello fürt.

* **Irányítópult** -hello irányítópult jeleníti meg.

* **Szolgáltatások** -hello szolgáltatások hello fürt adatait és konfigurációs beállításait.

* **Gazdagépek** -információkat és a konfigurációs beállítások hello fürt hello csomópontján.

* **Riasztások** -adatokat, figyelmeztetéseket és a kritikus riasztások naplóját.

* **Felügyeleti** -verem/szoftverszolgáltatások hello fürt, a szolgáltatás fiókjának adatait, és a Kerberos biztonsági telepített.

* **Felügyeleti gomb** -Ambari felügyeleti, a felhasználói beállításokat és kijelentkezési.

## <a name="monitoring"></a>Figyelés

### <a name="alerts"></a>Riasztások

hello alábbi lista hello közös figyelmeztetési állapotok Ambari használják.

* **OKÉ**
* **Figyelmeztetés**
* **KRITIKUS**
* **ISMERETLEN**

Riasztások eltérő **OK** hello következtében **# riasztások** bejegyzés hello tetején hello lap toodisplay hello értesítések száma. Ez a bejegyzés látható értesítések valamelyikének kiválasztásakor hello riasztások és azok állapotát.

Riasztások több alapértelmezett csoport, amely hello megtekinthetők vannak szervezve **riasztások** lap.

![Riasztások lap](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Hello segítségével kezelheti a hello csoportok **műveletek** menüben, majd válassza **riasztási csoportok kezelése**.

![riasztási csoportok párbeszédpanel kezelése](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Is kezelheti a riasztási módszerek, és riasztási értesítések készíteni hello **műveletek** menü kiválasztásával __riasztás értesítések kezelésére__. Aktuális értesítések jelennek meg. Itt értesítéseket is létrehozhat. Keresztül is elküldhetik az értesítéseket **E-mail** vagy **SNMP** Ha adott riasztás/súlyossági kombinációk történik. Például elküldheti e-mailt minden egyes hello riasztások hello **YARN alapértelmezett** csoportja túl**kritikus**.

![Hozzon létre figyelmeztető párbeszédpanel](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Végezetül kiválasztásával __riasztást beállításainak kezelése__ a hello __műveletek__ menü lehetővé teszi tooset hello ennyiszer kell a riasztás akkor fordulhat elő, egy értesítés elküldése előtt. Ezzel a beállítással lehet használt tooprevent értesítések átmeneti hibák esetén.

### <a name="cluster"></a>Fürt

Hello **metrikák** hello irányítópult lapja tartalmazza, melyek könnyen toomonitor hello állapota a fürt egy pillantással widgeteket. Több widgets, például a **CPU-használat**, kattintáskor további információkkal.

![a metrikák irányítópult](./media/hdinsight-hadoop-manage-ambari/metrics.png)

Hello **Heatmaps** lapon színes heatmaps zöld toored üzembe helyezésről metrikákat jeleníti meg.

![heatmaps irányítópult](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Hello fürt csomópontja hello további információkért válassza **állomások**. Ezután válassza ki a hello adott csomópont szüksége.

![állomás részletei](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Szolgáltatások

Hello **szolgáltatások** hello irányítópult oldalsávon hello hello fürt hello szolgáltatás állapotának gyors betekintést nyújt. Különböző ikonok is használt tooindicate állapot vagy a végrehajtandó műveleteket. Például egy sárga újrahasznosítást szimbólum jelenik meg, ha egy szolgáltatásnak kell toobe felhasználását.

![szolgáltatások oldalsó sáv](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> HDInsight-fürttípusok és verziók eltérő hello szolgáltatások jelenik meg. Itt látható hello szolgáltatások eltérhetnek hello szolgáltatások jelenik meg a fürt számára.

A szolgáltatás látható értesítések valamelyikének kiválasztásakor részletesebb információk hello szolgáltatásban.

![szolgáltatás összefoglaló információk](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Gyorshivatkozások

Egyes szolgáltatások megjelenítése egy **Gyorshivatkozások** hivatkozás hello oldal hello tetején. Ez lehet használt tooaccess szolgáltatásspecifikus web UI, mint például:

* **Feladatelőzmények** -MapReduce-feladatok előzményeinek.
* **Erőforrás-kezelő** -YARN erőforrás-kezelő felhasználói felületén.
* **NameNode** -Hadoop elosztott fájl System (HDFS) NameNode felhasználói felület.
* **Oozie webes felhasználói felület** -Oozie felhasználói felület.

Új lap ezeket a hivatkozásokat kiválasztásával megnyílik a böngészőben, amely hello kijelölt oldalát jeleníti meg.

> [!NOTE]
> Kiválasztásával hello **Gyorshivatkozások** egy szolgáltatás bejegyzése térhetnek vissza egy "a kiszolgáló nem található" hiba. Ha ezt a hibát észlel, használnia kell az SSH-alagút hello használatakor **Gyorshivatkozások** bejegyzés ezt a szolgáltatást. További információ: [használata SSH Tunneling a hdinsight eszközzel](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>Kezelés

### <a name="ambari-users-groups-and-permissions"></a>Ambari felhasználók, csoportok és engedélyek

Felhasználók, csoportok és engedélyek végzett használata esetén támogatottak. egy [tartományhoz csatlakoztatott](hdinsight-domain-joined-introduction.md) HDInsight-fürthöz. A hello Ambari felügyeleti felhasználói felület használatával olyan tartományhoz csatlakoztatott fürtön információkért lásd: [tartományhoz HDInsight-fürtök kezelése](hdinsight-domain-joined-introduction.md).

> [!WARNING]
> Ne változtassa meg jelszavát hello hello Ambari figyelő (hdinsightwatchdog) a Linux-alapú HDInsight-fürt. Hello jelszó oldaltörések módosítása képességét toouse Parancsfájlműveletek hello, vagy a fürthöz méretezési műveleteket.

### <a name="hosts"></a>Hosts

Hello **állomások** laplistákhoz hello fürt összes gazdagépén. toomanage állomások, kövesse az alábbi lépéseket.

![gazdagépek lap](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Hozzáadása, leszerelése és recommissioning egy gazdagép nem használható a HDInsight-fürtök.

1. Válassza ki, hogy kívánja-e toomanage hello állomás.

2. Használjon hello **műveletek** menü tooselect hello művelet, hogy kívánja-e tooperform:

   * **Indítsa el az összes összetevő** -elindítja az összes összetevő hello a gazdagépen.

   * **Állítsa le az összes összetevő** -állítsa le az összes összetevő hello gazdagépen.

   * **Indítsa újra az összes összetevő** - Stop, és indítsa el az összes összetevő hello gazdagépen.

   * **Kapcsolja be a karbantartási mód** -riasztások hello állomás mellőzi. Hozzon létre riasztást műveleteket hajtja végre ezt a módot kell engedélyezni. Például a szolgáltatás indítása és leállítása.

   * **Kapcsolja ki a karbantartási mód** -értéket ad vissza hello állomás toonormal lehetőséget.

   * **Állítsa le** -leállítja DataNode vagy NodeManagers hello gazdagépen.

   * **Start** -DataNode kezdődik vagy NodeManagers hello gazdagépen.

   * **Indítsa újra a** -leáll, és elindítja DataNode vagy NodeManagers hello gazdagépen.

   * **Szerelje le** -állomás eltávolítja hello fürtből.

     > [!NOTE]
     > A HDInsight-fürtökön ne használja ezt a műveletet.

   * **Recommission** – egy korábban már leszerelt toohello gazdagépfürt hozzáadása.

     > [!NOTE]
     > A HDInsight-fürtökön ne használja ezt a műveletet.

### <a id="service"></a>Szolgáltatások

A hello **irányítópult** vagy **szolgáltatások** lapján hello használata **műveletek** szolgáltatások toostop hello listája hello alján gombra, majd indítsa el az összes szolgáltatáshoz.

![szolgáltatás műveletek](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Amíg **hozzáadása szolgáltatás** szerepel ebben a menüben nem használt tooadd szolgáltatások toohello HDInsight-fürt kell tenni. Új szolgáltatások fel kell venni, hogy egy parancsfájl műveletével a fürtök kiépítése során. Parancsfájlműveletek használatával kapcsolatos további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).

Hello közben **műveletek** gomb újraindíthatja az összes szolgáltatás, gyakran kívánt toostart, állítsa le vagy egy adott szolgáltatás újraindítása. Hello lépéseket tooperform műveletek egy szolgáltatás a következő használja:

1. A hello **irányítópult** vagy **szolgáltatások** lapon, válassza ki a szolgáltatást.

2. A hello felső részén hello **összegzés** lapján hello használata **szolgáltatás műveletek** gombra, jelölje be hello művelet tootake. Ezzel a hello szolgáltatást az összes csomópont újraindul.

    ![szolgáltatási művelet](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Egyes szolgáltatások újraindítása hello fürt futása közben riasztásokat hozhat létre. tooavoid riasztást küld, használhatja a hello **szolgáltatás műveletek** gomb tooenable **karbantartási módba** hello szolgáltatás hello végrehajtása előtt.

3. Egy műveletet a kijelölt hello **# op** bejegyzés hello tetején hello lap lépésekben tooshow a háttérművelet folyamatban van. Ha konfigurálva toodisplay, háttér műveletek listájának hello jelenik meg.

   > [!NOTE]
   > Ha engedélyezte a **karbantartási módba** hello szolgáltatás, ne feledje toodisable azt hello segítségével **szolgáltatás műveletek** hello művelet befejeződése után gombra.

tooconfigure egy szolgáltatás, a lépéseket követve hello használata:

1. A hello **irányítópult** vagy **szolgáltatások** lapon, válassza ki a szolgáltatást.

2. Jelölje be hello **Configs** külön-külön hello aktuális konfigurációs jelenik meg. A fenti konfiguráció listáját is megjelenik.

    ![Konfigurációk](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Hello megjelenített mezők toomodify hello konfigurációt használja, és válassza ki **mentése**. Vagy válassza ki az előző konfigurációt, és válassza ki **ellenőrizze aktuális** tooroll biztonsági toohello beállításokat.

## <a name="ambari-views"></a>Az Ambari nézetek

Az Ambari nézetek oszthatja fejlesztők tooplug felhasználói felületi elemei hello Ambari webes felhasználói felület használatával hello [Ambari nézetek keretrendszer](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight Hadoop-fürt típusú nézetek következő hello biztosítja:

* Yarn várólista-kezelő: hello várólista-kezelő egy egyszerű felhasználói Felületet biztosít megtekintése és módosítása a YARN várólisták.

* Hive nézete: hello Hive nézet lehetővé teszi toorun Hive-lekérdezéseket közvetlenül a böngészőből. Lekérdezések mentése, eredmények megtekintése, toohello fürttároló-eredményeket menteni, vagy töltse le az eredményeket tooyour helyi rendszer. A Hive-nézetek használata további információkért lásd: [Hive nézetek és a HDInsight együttes](hdinsight-hadoop-use-hive-ambari-view.md).

* Tez nézet: hello Tez nézet lehetővé teszi toobetter megértéséhez, valamint feladatok optimalizálása. A Tez feladatok végrehajtásának módját, és milyen erőforrásokat használt információk is megtekinthetők.
